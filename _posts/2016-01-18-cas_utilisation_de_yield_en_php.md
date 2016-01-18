---
layout: post
title: "Cas d'utilisation de yield en PHP"
---

Je profite de la semaine des générateurs organisée par [Pascal Martin](https://twitter.com/pascal_martin) pour vous faire un retour sur l'usage de cette fonctionnalité super pratique sur [decitre.fr](http://www.decitre.fr).

Depuis juin 2013 et l'arrivée PHP 5.5, nous avons la possibilité d'utiliser les générateurs dans nos codes.

Récemment, nous avons eu le besoin de construire une commande CLI afin de générer les visuels de notre catalogue produit.

Pour cette commande, nous avons identifié différents points d'entrée possibles pour récupérer nos produits :

* l'identifiant en autoincrement du produit,
* l'identifiant naturel du produit (son SKU),
* un fichier contenant une liste de SKU,
* une date de référence pour récupérer les produits modifiés depuis cette date.

Pour préciser un peu plus le contexte de cette commande, nos produits sont stockés dans un Solr, tandis que les informations concernant les dates d'intégration des images restent dans notre base MySQL de référentiel produit, ce qui implique de réaliser la récupération du produit en 2 étapes (MySQL puis Solr).

De plus requêter Solr sur un grand nombre de produits coûte cher en mémoire et en ressource CPU (décodage du json), c'est pourquoi nous préférons faire une requête par produit.

## La méthode "old school"

En reprenant les structures de base php (quand on est fan des [array](http://php.net/manual/en/language.types.array.php) ou lorsqu'on ne connaît pas encore les Generator), le premier réflexe serait de remplir un tableau de produits, de le remplir en fonction des critères dont on a besoin, et de boucler dessus pour effectuer les opérations souhaitées.

>  **SPOILER ALERT** 
>
>  N'hurlez pas tout de suite pour le non-découpage des méthodes :)

Voici l'exemple de code tel qu'on aurait pu le faire il y a quelques années :

{% highlight php startinline %}

/**
 * @param array  $productIds
 * @param array  $skus
 * @param string $filename
 * @param string $referenceDate
 *
 * @return Product[]
 */
private function getProducts(
    array $productIds, array $skus, $filename, $referenceDate
)
{
    $products = [];

    foreach ($productIds as $id) {
        // on récupère le produit dans Solr via sa clé primaire 
        $product = $this->getProductFactory()->loadById($id);
        if (null === $product) {
            continue;
        }

        $products[] = $product;
    }

    foreach ($skus as $sku) {
        // on récupère le produit dans Solr via sa clé naturelle
        $product = $this->getProductFactory()->loadBySku($sku);
        if (null === $product) {
            continue;
        }

        $products[] = $product;
    }

    if (null !== $filename) {
        // le fichier contient une liste de sku 
        // sur lesquels on va boucler pour récupérer les produits 
        $file = new \SplFileObject($filename);
        foreach ($file as $sku) {
            // on récupère le produit dans Solr via sa clé primaire 
            $product = $this->getProductFactory()->loadBySku($sku);
            if (null === $product) {
                continue;
            }

            $products[] = $product;
        }
    }

    if (null !== $referenceDate) {
        // on effectue une requête pour retourner la liste des skus 
        // pour les produits modifiés depuis la date passée en paramètre 
        $stmt = $this->getErpConnexion()->prepare(
            "SELECT DISTINCT
                product.sku
            FROM
                erp.product
            WHERE
                product.updated_at LIKE :date
        ");
        $stmt->execute(['date' => sprintf('%s%%', $referenceDate)]);
        $stmt->setFetchMode(\PDO::FETCH_ASSOC);

        foreach ($stmt as $data) {
            // on récupère le produit dans solr via sa clé primaire 
            $product = $this->getProductFactory()->loadBySku($data['sku']);
            if (null === $product) {
                continue;
            }

            $products[] = $product;
        }
    }

    return $products;
}

{% endhighlight %}

L'inconvénient de cette méthode est que tous les objets doivent être chargés en mémoire. Si on doit travailler sur un nombre important de produits, le tableau va tous les contenir et on risque d'avoir une memory limit.

## Bonne nouvelle : qui dit boucle dit, depuis PHP 5.0, Iterator.

On peut donc construire un objet implémentant [Iterator](http://php.net/manual/en/class.iterator.php) qui pourra être parcouru et chargé à la demande, afin de réduire la charge mémoire.
Sauf que :

* il faut créer un nouvel objet pour gérer cela,
* à chaque méthode de récupération des produits, il faut être capable de déterminer où reprendre dans la liste.

Bref, l'Iterator est une solution possible, mais qui, dans ce cas, nécessitera d'écrire plusieurs classes et beaucoup de code, compte tenu des différentes sources de chargement (Solr, requête SQL, fichier de sku) et de la volonté de ne pas charger tous les objets en mémoire, mais au fur et à mesure.

## Bonne nouvelle (bis) : qui dit boucle dit, depuis PHP 5.5, Generator.

Mais c'est quoi un générateur ? La [documentation de php](http://php.net/manual/en/language.generators.overview.php) va fournir l'explication :


> Les générateurs fournissent une façon simple de mettre en place des itérateurs sans le coût ni la complexité du développement d'une classe qui implémente l'interface Iterator.
> Un générateur vous permet d'écrire du code qui utilise foreach pour parcourir un jeu de données, sans avoir à construire un tableau en mémoire pouvant conduire à dépasser la limite de la mémoire ou nécessiter un temps important pour sa génération. Au lieu de cela, vous pouvez écrire une fonction générateur, qui est identique à une fonction normale, mis à part le fait qu'au lieu de retourner une seule fois, un générateur peut utiliser yield autant de fois que nécessaire, afin de fournir les valeurs à parcourir.


En reprenant le premier exemple, il suffit de remplacer l'assignation dans le tableau $products par le mots clé yield pour retourner au fur et à mesure les objets

{% highlight php startinline %}

/**
 * @param array  $productIds
 * @param array  $skus
 * @param string $filename
 * @param string $referenceDate
 *
 * @return \Generator
 */
private function getProducts(
    array $productIds, array $skus, $filename, $referenceDate
)
{
    foreach ($productIds as $id) {
        // on récupère le produit dans Solr via sa clé primaire
        $product = $this->getProductFactory()->loadById($id);
        if (null === $product) {
            continue;
        }

        yield $product;
    }

    foreach ($skus as $sku) {
        // on récupère le produit dans Solr via sa clé naturelle
        $product = $this->getProductFactory()->loadBySku($sku);
        if (null === $product) {
            continue;
        }

        yield $product;
    }

    if (null !== $filename) {
        // le fichier contient une liste de sku 
        // sur lesquels on va boucler pour récupérer les produits 
        $file = new \SplFileObject($filename);
        foreach ($file as $sku) {
            // on récupère le produit dans Solr via sa clé primaire 
            $product = $this->getProductFactory()->loadBySku($sku);
            if (null === $product) {
                continue;
            }

            yield $product;
        }
    }

    if (null !== $referenceDate) {
        // on effectue une requête pour retourner la liste des skus
        // pour les produits modifiés depuis la date passée en paramètre 
        $stmt = $this->getErpConnexion()->prepare(
            "SELECT DISTINCT
                product.sku
            FROM
                erp.product
            WHERE
                product.updated_at LIKE :date
        ");
        $stmt->execute(['date' => sprintf('%s%%', $referenceDate)]);
        $stmt->setFetchMode(\PDO::FETCH_ASSOC);

        foreach ($stmt as $data) {
            // on récupère le produit dans solr via sa clé primaire 
            $product = $this->getProductFactory()->loadBySku($data['sku']);
            if (null === $product) {
                continue;
            }

            yield $product;
        }
    }
}

{% endhighlight %}


Et voilà : notre méthode est désormais capable de boucler sur des objets produits, chargés au fur et à mesure, sans risque de memory limit et tout ça avec un refactoring très simple et n'impactant pas les appels effectués : la consommmation de la méthode continue de se faire grâce à un foreach.

{% highlight php startinline %}

foreach ($this->getProductsGenerator(
            $productIds, $skus, $filename, $referenceDate
        ) as $product) {
    $product->generateVisuals();
}

{% endhighlight %}


## Et PHP 7 dans tout ça ?

Pour ceux qui ne le savent pas encore, PHP 7 est sorti depuis le début du mois de décembre et apporte de nombreuses évolutions. Les générateurs n'ont pas été oublié parmi la liste des nouveautés et des évolutions, notamment suite à cette [RFC](https://wiki.php.net/rfc/generator-delegation).

Il est désormais possible de déléguer le générateur à un autre sous ensemble en faisant les yield en cascade, de manière à mieux découper et séparer le code, comme le montre ce portage du précédent exemple.

{% highlight php startinline %}

/**
 * @param array  $productIds
 * @param array  $skus
 * @param string $filename
 * @param string $referenceDate
 *
 * @return \Generator
 */
private function getProducts(
    array $productIds, array $skus, $filename, $referenceDate
)
{
    // la méthode principale délègue à chaque sous méthode 
    // le soin de gérer son propre générateur
    yield from $this->getProductsGeneratorFromIds($productIds);
    yield from $this->getProductsGeneratorFromSku($skus);

    if (null !== $filename) {
        yield from $this->getProductsGeneratorFromFilename($filename);
    }

    if (null !== $referenceDate) {
        yield from $this->getProductsGeneratorFromReferenceDate($referenceDate);
    }
}

/**
 * @param array $productIds
 *
 * @return \Generator
 */
private function getProductsGeneratorFromIds($productIds)
{
    foreach ($productIds as $id) {
        // on récupère le produit dans Solr via sa clé primaire
        $product = $this->getProductFactory()->loadById($id);
        if (null === $product) {
            continue;
        }

        yield $product;
    }
}

/**
 * @param array $skus
 *
 * @return \Generator
 */
private function getProductsGeneratorFromSku(array $skus)
{
    foreach ($skus as $sku) {
        // on récupère le produit dans Solr via sa clé naturelle
        $product = $this->getProductFactory()->loadBySku($sku);
        if (null === $product) {
            continue;
        }

        yield $product;
    }
}

/**
 * @param string $filename
 *
 * @return \Generator
 */
private function getProductsGeneratorFromFilename($filename)
{
    // le fichier contient une liste de sku
    // sur lesquels on va boucler pour récupérer les produits 
    $file = new \SplFileObject($filename);
    foreach ($file as $sku) {
        // on récupère le produit dans Solr via sa clé primaire 
        $product = $this->getProductFactory()->loadBySku($sku);
        if (null === $product) {
            continue;
        }

        yield $product;
    }
}

/**
 * @param string $referenceDate
 *
 * @return \Generator
 */
private function getProductsGeneratorFromReferenceDate($referenceDate)
{
    // on effectue une requête pour retourner la liste des skus 
    // pour les produits modifiés depuis la date passée en paramètre 
    $stmt = $this->getErpConnexion()->prepare(
        "SELECT DISTINCT
            product.sku
        FROM
            erp.product
        WHERE
            product.updated_at LIKE :date
    ");
    $stmt->execute(['date' => sprintf('%s%%', $referenceDate)]);
    $stmt->setFetchMode(\PDO::FETCH_ASSOC);

    foreach ($stmt as $data) {
        // on récupère le produit dans solr via sa clé primaire 
        $product = $this->getProductFactory()->loadBySku($data['sku']);
        if (null === $product) {
            continue;
        }

        yield $product;
    }
}

{% endhighlight %}


## Pour finir

Avec 5000 produits pour lesquels les visuels doivent être générés, notre script avec le tableau fourre-tout a un pic de consommation mémoire de près 300M. 
Lorsqu'on utilise les générateurs, le pic passe à 20M et reste stable quelque soit le volume de produit impacté.

Si vous êtes confrontés à des collections dont la charge mémoire est trop forte, ou plus simplement si vous êtes à la recherche d'une syntaxe facile à écrire et efficace, pensez aux Generator.

> Merci à Pascal d'avoir organisé cette semaine d'articles :)
