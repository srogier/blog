---
layout: post
title: "Retour sur le Blend Web Mix"
---

La semaine dernière, j'ai pu aller au [Blend Web Mix](http://www.blendwebmix.com/), événement Web Lyonnais où se côtoient des profils business, design et technique. C'est donc l'occasion de faire un retour sur les conférences auxquelles j'ai assisté.


## Écouter sa communauté, les bons outils pour développer le meilleur produit
Christophe Gagin, [@chrisgagin](https://twitter.com/chrisgagin)
 
Première conférence du premier jour, ayant pour objectif de répondre à cette problématique : comment fidéliser/conserver ses utilisateurs ?
Un des moyens possible est de personnaliser au maximum l'expérience utilisateur afin qu'il s'approprie le produit.

L'important est donc de comprendre ses utilisateurs sur les deux axes suivants :

* User analytics => WHAT ? => quantitatif
* User experience => WHY ? => qualitatif

Voici une liste de différents outils qui peuvent être mis en place pour suivre sa communauté d'utilisateurs :

* Uservoice,
* segments.io,
* Google Analytics,
* Kiss Metrics,
* Intercom.io.
 
La conférence était très intéressante, malheureusement la plénière d'ouverture a pris du retard qui s'est répercuté sur cette conférence et n'a donc pu être terminée.
 
## Dans l'enfer du Web Mobile : embûches et solutions pour les dév
Mathieu Parisot, [@matparisot](https://twitter.com/matparisot)
 
Quelles sont les problèmes soulevés lors de la mise en place d'un site mobile face à la diversité des terminaux et systèmes d'exploitations.
Avant tout, il faut identifier et se limiter au support de son panel d'utilisateur.
 
Un rappel est fait sur pour n'importe quel navigateur utilisé sous iOS, son moteur sera celui de Safari.
 
Un des gros problème du Web mobile est la fragmentation Android qui intervient à différents niveaux :

* système,
* écran,
* surcouche constructeur,
* surcouche opérateur.
 
Il existe des outils de debug sous iOS et Android pour faciliter le développement , mais iOS nécessite un mac tandis que pour Android 4.2+ a besoin de drivers adaptés en fonction du modèle pour utiliser le débugger intégré dans Chrome

Qui dit mobile dit penser au réseau et aux latences (500ms en edge) : il faut trouver la balance nombre de fichiers et taille des fichiers.
Pour finir quelques quirks :

* La taille des champs de saisie qui ne dézoomment pas au blur sous iOS (il faut passer le font-size à 16px),
* capture du clic => différents événements/implémentations (touch, click, double click). La librairie fastclick.js est là pour aider à résoudre ce problème.

Dans tous les cas, [caniuse.com](http://caniuse.com/) est ton ami.
Bonne conférence, avec plusieurs exemples sur la difficulté de réaliser une site mobile, et qui a fait remonter quelques souvenirs de problématiques rencontrées lors du passage en responsive de decitre.fr.

Les slides sont disposibles [ici](http://fr.slideshare.net/soatexpert/blendwebmix-webmobile).
 
## Prétotyping : l’exécution est la clé, comment s'y prendre
Laurent Hausermann, [@lhausermann](https://twitter.com/lhausermann)
 
On a souvent ce qu'on pense être une bonne idée. Mais, malgré une bonne implémentation, cela peut se terminer par un échec (pas de besoin, besoin surestimé ou mal estimé).
 
Le prétotyping permet de tester l'attrait et l'usage réel d'un potentiel nouveau produit en simulant son "expérience vitale" avec un strict minimum investissement en temps et en argent. (Inspiré par Alberto Savoia)

> "Doing the right s Doing it right" Alberto Savoia (père de adwords et du prétotyping)

L'objectif est de valider l'usage et l'attractivité afin de valider son idée.
 
* **Prototype** : Peut-on faire le produit ?
* **Prétotype** : Est-ce le bon produit ?
 
Méthode à mettre en place :

* Identifier le plus petit test,
* Mesurer,
* Itérer.
 
Quelques exemples sont cités :

* **Palmpilot** : première version en bois pour éprouver l'idée ⇒ pinocchio
* **Dropbox** : vidéo fake pour simuler l'outil ⇒ validation de l'intêret des utilisateurs (cf MVP de lean)
* **Zappos** (site de ventes de chaussures) : prétendre que les stocks sont possédés, mais les acheter en magasin. Permet de prendre le temps de comprendre les clés du succès : service clients, échange et frais de port gratuit
Une conférence concrète et intéressante sur la manière dont le prétotyping a été mis en place par certaines entreprises dans leur produit.
 
## Icônes, il faut qu'on parle
Sébastien Desbenoit, [@desbenoit](https://twitter.com/desbenoit)

Les pictogrammes remontent à la préhistoire et ont fait leur Retour à la fin du XXe siècle avec l'apparition de l'automobile.
Au JO de Tokyo 1964 ils ont été popularisés pour résoudre le problème de la barrière de la langue.

Avant tout, quelques définitions pour présenter le contexte :

* **Pictogramme** : représentation visuelle d'un élément réel.
* **Idéogramme** : représentation visuelle d'un concept.
* **Symbole** : signe portant des significations.
* **Pictogramme** : la plus simple de représentation.
* **Icône** : une de représentations.
 
Pourquoi utilise-t-on des icônes face à l'écriture :

* gain de place,
* barrière de langue,
* méthode d'expression,
* plus rapide à comprendre (cf prise médicament),
* principe supérieur de l'image.
 
Lors du choix d'une icône, il faut prendre en compte :

* l'utilisation,
* le contexte géographique,
* le contexte socio-économique,
* le contexte psychoschopique (tabous),
* les conditions d'utilisations (web, voiture, etc.),
* l'interface globale du produit (icônes Android différente d'iOS).
 
Attention un même pictogramme peut avoir plusieurs significations (par exemple la loupe).
 
Il faut penser à l'usage qui en sera fait :
L'intégration technique passe avant tout (une icône pour un téléphone sera différente de celle pour de la signalisation routière).

Conférence très intéressante et très pédagogique sur les usages des icônes et les questions à se poser lors de leur choix.

Les slides sont disposibles [ici](https://speakerdeck.com/desbenoit/icones-a-blend-web-mix).
 
## Google ment
Jean-François Longy

Il y a beaucoup de règles compliquées et obscures pour leur moteur de référence ment.

La conférence était en mode normande : peut-être bien que Google fait comme ça, mais peut-être bien que non.

Le seul élément concret que j'ai pu noter est que l'impact de la vitesse d'un site sur le référencement est faible : tant que le robot peut accéder à la page, cela suffit. Cela n'empêche pas que pour le visiteur, c'est un critère important.

Bref, une conférence avec pas mal de blabla, peu de concret, et une invitation à implicite à s'entourer d'un professionnel du référencement qui connaît cet univers impitoyable...

## Dette technique, responsabilité collective, solution individuelle ?
Brice Favre, [@briceatwork](https://twitter.com/briceatwork)
 
Je démarre la seconde journée par une conférence sur les méfaits de la dette technique.

Avant tout, quelles sont les composantes de la dette elle-même ?

* la dette elle-même,
* les intérêts (bugs, adaptation du code), 
* le remboursement de la dette.
 
Comment s'en affranchir en différentes étapes ?

* décrassage (lecture du code, documentation),
* corrections petites et rapides pour valider la compréhension du code,
* zonage (se concentrer sur ce qu'on connait, déléguer le reste).
 
Afin de partager la connaissance au sein de l'équipe, il faut utiliser le pair rogramming et la revue de code.
 
Il faut chercher à partager des connaissance, compétences sur les chantiers en cours, identifier les points problématiques,  chercher la solution et saisir les opportunités lors de développements pour les appliquer.
 
 
## Un jeu web accessible avec HTML5, JAvascript et ARIA
Sylvie Duchateau et Victor Brito

Cette conférence avait pour but de présenter une application Web accessible aux non voyants au travers d'un tirage de bingo.

Les problématiques à prendre en compte lors de la. Réalisation d'une interface accessible sont les suivants :

* interactions avec la souris,
* toute information codée par la forme, couleur, position,
* rafraîchissement des infos.
 
Il y a ensuite eu une démonstration du bingo avec un outil de synthèse vocale et clavier en braille) : la synthèse vocale lit les titres, label de champs au fil de la navigation.

On constate également que les popins ne sont pas très accessibles par manque d'information sur le fait que le contenu soit dans cette popin.

Côté code, il faut poser les attributs ARIA dans le code html pour les lecteurs écrans.

Sur un sujet sur lequel je ne m'étais jamais penché, cette conférence était un bon outil de sensibilisation et une bonne introduction aux outils à mettre en œuvre pour la réalisation de sites Web accessibles.
 
## API fait de la résistance
Maxime Prades, [@prades_maxime](http://www.twitter.com/prades_maxime)

La conférence était un retour d'expérience Basé  de Zendesk
La définition présentée de l'API est celle d'un pont entre deux systèmes.
L'idéal est de créer son application en API first pour permettre :

* la réduction de la complexité,
* le découpage/nettoyage de code,
* KISS (toujours penser à la vue produit).
Il faut se poser en client de sa propre API afin de l’éprouver, la valider et en améliorer la qualité.
 
Les derniers conseils donnés sont les suivants :

* ne pas casser les API (éviter les breaking change),
* fournir un sdk,
* penser au retry after pour limiter les accès,
* en cas de création d'une Api, oublier SOAP et faire du REST.
 
 
## Growth hacking
Oussama Ammar, [@daedalium](http://www.twitter.com/daedalium)

Le growth hacking est une technique à appliquer pour faire croître son entreprise :

 * Travailler la retention pour aider la croissance.
 * Importance du service client
 * Utilisation de Intercom.io  pour traiter et identifier chaque client.
 * Mettre l'accent sur la personnalisation de l'expérience.
 
 
## Faire le pont entre dev et designers
Kaelig Deloumeau-Prigent, [@kaelig](http://www.twitter.com/kaelig) 

Présentation de techniques utilisées au Guardian pour fournir un point de référence commun entre désigner et développeur à l'aide de sass.
 
Parmi les conseils données, il faut, par exemple, fournir une page avec les différentes couleurs,  pour savoir lorsqu'un gris clair est demandé lequel est le bon.
Pour gérer la cohérence des typos au sein du site, les développeurs ont mis en place des mixins qui gèrent les caractéristiques des polices utilisées (font, size, height, bold).

Les slides sont disposibles [ici](http://fr.slideshare.net/kaelig/faire-le-pont-entre-designers-et-developpeurs-au-guardian)
 
##Git Protips : faire du git plus vite et mieux graàace à cette sélection d'astuce
Christophe Porteneuve, [@porteneuve](http://www.twitter.com/porteneuve)

Je n'ai assisté qu'à la seconde moitié de cette conférence. Il y avait pas mal d'astuces d'utilisation, de commandes bien pratiques (le log pour se concentrer sur une fonction <3 )

Je recommande de visualiser la [vidéo](http://www.youtube.com/watch?v=ypR-rEBCoag&feature=youtu.be) ou regarder les [slides](http://delicious-insights.com/talks/blend2014-git-protips/). Que vous soyez utilisateur débutant ou avancé de git, il y aura pas mal de commandes et concepts à découvrir.
 
## Architecture REST : On ne va pas se mentir 
William Durand, [@couac](http://www.twitter.com/couac)

Cette conférence présentait la définition et un ensemble de bonnes pratiques autour la création d'une API REST full au sein de l'écosystème Symfony.

L'occasion pour moi de découvrir le modèle de maturité de Richardson ainsi qu'en deuxième de partie de conférence différents outils pour aider à documenter et tester son API REST.

Un très bon rappel de ce que doit être dans l'idéal une API, peu importe qu'elle soit faite en PHP ou non. 

Je vous conseille de regarder les [slides](https://speakerdeck.com/willdurand/architecture-rest-on-ne-va-pas-se-mentir-blendwebmix) de cette présentation, si vous voulez rentrer dans les détails de ces bonnes pratiques.

## Et l'organisation ?

Lors de la plénière d'ouverture, le ton avait été le donné : "l'objectif est que vous soyez frustrés de ne pas avoir vu toutes les conférences que vous vouliez voir."

Et on peut dire que l'objectif a été en partie atteint, mais pas pour les bonnes raisons. La principale raison de cette frustration est qu'il n'y avait pas de temps de battement entre les conférences. La plénière d'ouverture ayant pris du retard, cela a impacté les sessions de la matinée qui ont mécaniquement accumulé du retard, et au final je me suis retrouvé à sacrifier certains sujets car la conférence avait déjà débuté depuis plusieurs minutes.

Dommage également que sur un espace utilisant 4 étages, il n'y ait pas de fléchage ou de plan des salles utilisées. Néanmoins, les nombreux bénévole étaient là pour nous aiguiller dès qu'on prenait l'air perdu : merci à eux :)

Autre déception, l'espace question pour les conférences. A l'issue des 30 minutes (format majoritaire des conférences) les questions se déroulaient dans un espace dédié à l'extérieur de la salle et surtout impliquaient le choix entre voir la conférence suivante et suivre les éventuelles questions : dommage.

Dans les points positifs, étant d'un profil techniques, j'ai beaucoup apprécié certains conférences business (écoute de sa communautre, prétotyping) qui rappelent qu'on ne fait pas de la technique pour faire de la technique. La bataille de Nerf pendant la pause de l'après-midi était également très sympa :).

Bref, malgré mes quelques critiques, les bénévoles de l'organisation étaient très disponibles. Félicitations et encore merci à eux, d'autant plus que gérer un événement accueillant plus de 1000 personnes sur deux jours est loin d'être facile.

