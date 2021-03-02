---
title: Démarrer dans le développement de moteurs de jeux
date: 2021-03-02 16:00:00 +0100
category: Game Engine
---

Dans le cas où vous auriez lu [l'article de présentation]({% post_url 2021-02-20-Introducing %}), vous devriez savoir que ce blog a été en partie conçu
pour parler de développement de moteurs de jeux. Ce premier article, d'une longue série, a pour but d'introduire le sujet : Qu'est-ce qu'un moteur, comment réaliser un tel projet, qu'est-ce que cela demande... Bref, par quel bout s'y prendre.

## Avant-propos

Avant de commencer, je souhaiterais rappeler que **je ne suis pas un professionnel**. Je ne suis pas officiellement issu du milieu du jeu vidéo et ne me consacre
au développement que durant mon temps libre. L'objectif de ce blog est de retransmettre l'expérience et le résultat des recherches que j'ai pu mener au fil des
années sur le développement et la programmation, c'est pourquoi je m'appliquerai à lier à mes articles le plus de ressources possible sur le sujet.
Ainsi je vous encourage vivement à étudier les dites ressources et à mener vos propres recherches et développer votre propre expérience.

Je ne cherche qu'à partager un peu de mon expérience, mais ce que je pourrais présenter au travers de mes articles peux ne pas correspondre à vos besoins ou ne pas être optimal.
Par la même occasion je vous invite à me faire part de vos retours et vos impressions dans la sections commentaires, tout en bas 👍.

Ceci étant dit, entrons maintenant dans le vif du sujet...

## Qu'est-ce qu'un moteur de jeu ?

Si vous êtes ici, c'est probablement que vous vous intéressez au développement de moteurs de jeux. Mais savez-vous seulement de quoi il s'agit ?
Je vous propose de passer en revu certaines notions afin de clarifier cela.

### Framework ou Moteur ?

En cherchant un peu sur Internet, vous devriez avoir trouvé de nombreuses définitions différentes de ce qu'est un moteur, et il est probable que vous soyez passé
par la notion de framework. En effet, les notions de framework, de moteur et même de bibliothèque sont comparables, après-tout, il ne s'agit dans les trois cas
que d'une grosse collection de fonctions destinées à un usage précis, autrement dit un outil destiné à simplifier la vie d'un programmeur :
une bibliothèque rassemble un tas de fonctions destinées, par exemple, à la gestion de la mémoire, au logging ou à la communication avec la
carte graphique _(e.g OpenGL, DirectX)_; un framework rassemble un tas de bibliothèques _(e.g Qt, SFML, Boost...)_; et un moteur rassemble un tas de framework.

C'est une définition assez simpliste mais suffisamment proche de la réalité. D'un point de vu technique, la différence se situe aux **niveaux d'abstractions**.
Pour faire court, le niveau d'abstraction d'un programme correspond à son degré de spécialisation, mais si vous désirez en savoir plus sur la notion de niveaux d'abstractions
_(que je considère personnellement comme fondamentale en programmation)_ je vous conseil [cet excellent article](https://www.fluentcpp.com/2016/12/15/respect-levels-of-abstraction/) _(en anglais)_.
En effet, une bibliothèque de logging, par exemple, est d'un niveau d'abstraction assez bas, on peut en faire à peu près ce qu'on veut, tandis qu'un framework comme la SDL
est conçue pour rassembler un certain nombre d'outils permettant la création de jeux vidéos, qui est donc d'un niveau d'abstraction plus haut.

Un moteur est théoriquement d'un niveau d'abstraction encore plus élevé _(voir plus)_. Un moteur de qualité industriel doit dans l'idéal _(mais l'idéal n'existe pas)_ permettre la création d'un jeu vidéo
sans la moindre ligne de code ! Il est en fait quasiment du niveau de l'application, c'est à dire qu'il ne s'agit plus d'un simple amas de fonctions mais presque d'une application pouvant être exécutée
sur votre ordinateur ! La plus grande partie du travail consiste finalement en la création des "assets" : sons, musiques, textures, cartes, modèles 3D, sprites, animations...
Jetez un œil à des moteurs commerciaux de cette qualité tel que Unreal Engine (prononcez ~~"Enn-jaïne"~~ "Enn-gine") ou Unity, et vous verrez !

## Complexité

Vous êtes peut-être en train de vous dire "C'est bien trop compliqué, je n'arriverai jamais à faire un moteur comme Unreal Engine tout seul !", et personne ne vous demande de refaire Unreal Engine !

Le niveau de complexité de votre moteur dépend réellement des objectifs que vous vous fixez : typiquement, de nombreux développeurs de Game Jam qui n'utilisent pas de moteurs déjà existant _(Comme Unity ou Godot)_ développent leurs propres moteurs de Game Jam "simplifiés" en une semaine ou deux, parfois basé sur des framework multimédia _(comme SFML ou SDL)_, et ce n'est vraiment pas sorcier ! À l'inverse, d'autres développeurs visent un peu plus haut et créent _(seul ou en petit groupe)_ des moteurs de "qualité studio"... en un peu plus de temps cependant.

Tout dépend aussi du choix des dépendances : se servir de bibliothèques ou de frameworks _(pour les mathématiques, la gestion des fenêtres et des entrées/sorties, la lecture de sons, l'ouverture de fichiers d'images, la gestion de la mémoire... Et la liste peut être longue)_ permet de faciliter le développement du moteur en se délestant du travail sur des routines de plus bas niveaux et en se servant de code fiable et efficace.
Cependant, le sujet des dépendances est à étudier avec soin, car si certaines dépendances peuvent améliorer la qualité du produit final, trop de dépendances peuvent rendre votre programme et votre code difficile à
maintenir et surtout difficile à porter. Si certaines bibliothèques sont "cross-plateform" _(i.e fonctionnent sur plusieurs plateformes : Windows, Mac, Linux, Android, Playstation...)_ d'autres ne le sont pas.
Qui plus est, la mise en place de l'environnement de développement, que ce soit pour vous ou un développeur qui débarquerais dans votre projet, devient d'une complexité croissante avec le nombre de dépendances.
Vous pouvez même dans certains cas éprouver la nécessité de retravailler certaines dépendances, pour améliorer les performances par exemple
_(e.g [EASTL](https://github.com/electronicarts/EASTL), la STL optimisée de EA (Merci à Bm d'Arcadia Corp pour cet exemple))_.
Il y a encore beaucoup à dire sur le sujet, et nous aurons l'occasion d'y revenir.

Pour cette série, nous développerons un moteur de "qualité studio", c'est à dire qu'on ne s'attend pas à développer un outil aussi complet que Unity par exemple, mais un moteur capable de supporter
le développement d'un jeu indépendant commercialisable, qui pourrait être utilisé par un studio indépendant.
_(D'ailleurs, vous ai-je dis que j'étais développeur indépendant ? C'est pour ça que je fais des moteurs indépendants pour des studios indépendants 😄)_

Pour cela, passons en revu les éléments que nous nous attendons à devoir développer.

## Anatomie

Effectivement, un moteur est complexe _(j'entends par là, non pas compliqué, mais l'inverse de primitif. Nuance)_, il est composé de nombreux sous-systèmes et modules divers, typiquement un moteur graphique, physique,
audio voire réseau mais aussi de nombreux outils de debugging, de chargement de ressources, de gestion de mémoire etc. destinés à simplifier la vie du développeur et améliorer la fiabilité et la
maintenabilité du programme.

### Le moteur graphique

C'est là que se trouve l'interface de communication avec la carte graphique et l'abstraction permettant de dessiner à peu près n'importe quoi à l'écran dans la plus grande simplicité.
Il est généralement basé sur OpenGL, DirectX et plus récemment Vulkan qui sont les bibliothèques standards pour cette tâche et dont l'implémentation est fournie avec les pilotes
de votre carte graphique : voilà pourquoi il est impératif de les mettre régulièrement à jour !

C'est généralement un des sous-système les plus massif et sur lequel les développeurs mettent le plus l'accent, bien que ce ne soit pas le seul (loin de là). D'ailleurs, beaucoup de tutoriels sur internet se concentre essentiellement sur celui-ci plus que sur le reste. Mon objectif est aussi de rétablir l'équilibre sur ce point au travers de cette série : le moteur graphique ne sera abordé que bien plus tard.

Il peut être simple comme compliqué, 2D, 3D ou entre les deux, demander beaucoup ou pas du tout de connaissances en mathématiques,
notamment en algèbre linéaire et communiquera généralement avec le moteur physique. Ce qui me donne une bonne transition.

### Le moteur physique

Encore un gros morceau. Sans surprise, ce système s'occupera de la gestion de la physique : collisions, forces, solides etc.
Cela dit, beaucoup d'autres techniques et technologies peuvent intervenir et peuvent nécessité certaines connaissances en physique. Il existe de nombreuses ressources disponible sur Internet
et vous pouvez aussi directement vous lancez dans des études de physique _(comme moi 😄)_ mais si cela vous effraie, il est tout à fait possible de vous baser sur des moteurs existants comme
Box2D _(Pour... La 2D)_ ou PhysX de NVidia, qui est maintenant libre.

### L'audio

De la même façon, le moteur audio peut être très simple comme très compliqué. L'ouverture et la lecture de fichiers de sons de différents types _(mp3, ogg, wav...)_ et la nécessité d'utiliser le multi-threading
_(ou les interruptions système, pour les plus nostalgiques)_ sans parler des routines de manipulation du son _(réverbération, distorsion...)_ poussera à l'utilisation de bibliothèques tierces
puissantes et fiables : OpenAL, FMOD, ou Wwise.

### Le réseau

La partie réseau peut être optionnel, vos jeux peuvent très bien être hors-ligne. Mais si ce n'est pas le cas, la mise en place d'un moteur réseau relève encore d'un autre domaine d'expertise, qui peut
encore une fois être simplifié par l'utilisation de bibliothèques tierces, mais il peut aussi être intéressant d'attendre c++23 _(c'est à dire 2023)_ pour _(potentiellement)_
voir apparaître la gestion du réseau dans la bibliothèque standard.

### L'interface graphique

La plupart des moteurs modernes viennent avec une interface graphique les faisant plus ressembler à de simples logiciels de conception multimédia qu'à un réel moteur.
C'est une des choses que l'on pourrait souhaiter avec l'évolution de notre moteur : une interface graphique simple d'utilisation, permettant de créer un jeu sans la moindre ligne de code.

### Le cœur du système

C'est la partie du moteur au niveau d'abstraction le plus faible. Elle contient tous les outils et systèmes de base, c'est l'interface principale entre le système d'exploitation et les
autres sous-moteurs. On y trouvera d'ailleurs généralement la "Platform Independance Layer" sur laquelle on reviendra. Typiquement, on s'attend à y trouver _(entre autres)_ :

* Le logger
* La gestion de la mémoire
* La gestion des ressources/assets
* La gestion du temps
* Les fonctions mathématiques
* La gestion des bibliothèques dynamiques
* La gestion des événements
* Les outils de debugging de manière générale
* L'internationalisation _(Ou I18n)_
* Le RTTI et la sérialisation
* La lecture et la gestion de fichiers _(CSV, XML, YAML...)_
* Le générateur de nombres aléatoires
* Un gestionnaire d'exceptions
* Le scripting
* ...

Et potentiellement beaucoup, beaucoup plus de choses, dépendant du niveau de complexité que vous vous êtes fixé, comme les collections, les treads, la lecture de film, les signaux, etc.
Vous n'êtes absolument pas dans l'obligation d'implémenter chacune de ces choses, tout dépend de vous et de vos préférences.

En résumé, c'est là partie de moteur qui sera la moins visible de l'extérieur dans vos futurs jeux, mais c'est là, selon moi, que toute la magie opère. Un système central et des outils bien designés et bien architecturés
est selon moi ce qui détermine la fiabilité et l'évolutivité de votre moteur : si au court du développement d'un jeu utilisant votre moteur vous détectez une faiblesse imprévue dans un de ces systèmes,
vous obligeant à revoir tout votre design, vous perdrez un temps précieux et une quantité non négligeable de motivation. C'est bien la dernière chose que vous souhaiteriez, voilà pourquoi il ne faudra pas
négliger le design et l'architecture.

## Les bons tuyaux

Au final, créer un moteur est un gros projet. Un très gros projet. Voilà pourquoi on ne peux pas se contenter de plonger dans le code et commencer le développement sans préparation préalable.
Il est important de consacrer en premier lieu du temps à l'étude du problème, le design et l'architecture.
Inspirez-vous de ce qui a déjà été fait : il existe beaucoup de jeux et de moteurs dont le code est disponible sur internet/github _(regardez du côté du github d'id Software par exemple)_.
De cette façon, vous pourrez vous faire une meilleure idée de comment vous y prendre, des choses que vous voudriez ou ne voudriez pas dans votre moteur et des meilleurs choix de design.

Enfin, lorsque vous commencerez à avoir une bonne vision de votre projet et vous sentirez prêt à coder, je ne peux que vous conseiller de bien connaître votre langage :
dans mon cas je travaillerais en C++. Une bonne compréhension du langage, de la Programmation Orientée Objet (POO), de la mémoire, de la bibliothèque standard,
de ses algorithmes et, optionnellement, de la template meta-programmation permettrons de concevoir un programme plus robuste et plus fiable.
J'accorderai de la même façon une grande importance à "l'expressivité" du code : il n'y a rien de pire que de ne plus comprendre le sens d'un bout de code que l'on a fait quelques temps auparavant.
Ceci aura pour objectif de rendre le code plus compréhensible _(par nous ou par un autre développeur)_ et de la même façon plus maintenable. Pour cela, je m'inspirerai beaucoup des articles disponibles sur
[Fluent C++](https://www.fluentcpp.com).

Pour finir, _(Merci à Kern, créateur de [Kalaxia](https://kalaxia.org/), pour cette remarque)_ il est essentiel de bien connaître ses Design Patterns et les différents design qui peuvent être associés à votre langage
_(Comme les "Concepts" en C++)_.

## Ressources utiles

Pour conclure cet article d'introduction, je vous propose une liste des principales ressources sur le sujet que j'ai pu recueillir au fil des années _(Youtube, Blog, Livres...)_.
Il s'agit d'une liste non exhaustive sur de nombreux sujets en lien avec ce que j'ai présenté, il en existe beaucoup d'autres et je vous invite à m'en proposer de nouvelles dans les
commentaires que je me ferai plaisir de rajouter à cette lite.

### Chaînes Youtube

Je vous invites à aller jeter un œil aux playlists de ces chaînes.

* [The Cherno](https://www.youtube.com/c/TheChernoProject), beaucoup de ressources et tutoriels sur le C++ et le Game Engine Development.
* [3Blue1Brown](https://www.youtube.com/channel/UCYO_jab_esuFRV4b17AJtAw), Mathématiques, algèbre linéaire, analyse et beaucoup d'autres choses, si vous êtes curieux.
* [thebennybox](https://www.youtube.com/user/thebennybox), tutoriels et vidéos sur le développement de jeux vidéos, la programmation graphique et bien d'autres.
* [Hopson](https://www.youtube.com/c/Hopsonn), quelques tutoriels et beaucoup de projets de jeux, si vous êtes curieux de découvrir de nouvelles choses et de lire du code.
* [ChiliTomatoNoodle](https://www.youtube.com/c/ChiliTomatoNoodle), encore des tutoriels sur le C++, la programmation et la programmation graphique (notamment DirectX).

### Sites

* [Fluent C++](https://www.fluentcpp.com), l'excellent blog de Jonathan Boccara sur le C++, les derniers concepts et nouveautés du langage et surtout l'expressivité du code.
* [Openclassrooms](https://openclassrooms.com), si vous souhaitez apprendre un nouveau langage. Les forums peuvent aussi présenter de l'intérêt.
* [Developpez](https://www.developpez.com/), de nombreuses ressources diverses sur beaucoup d'aspect de la programmation et sur le Game Dev.
* [Learn OpenGL](https://learnopengl.com/), si vous souhaitez en apprendre plus sur la programmation graphique avec OpenGL.
* [Open.gl](https://open.gl/), encore OpenGL.
* [Lighthouse 3D](https://www.lighthouse3d.com/), tutoriels sur la 3D, notamment OpenGL et GLSL.
* [Codeproject](https://codeproject.com/), des extraits de codes pour plein d'applications différentes.
* [Stack Overflow](https://stackoverflow.com/), le forum du développeur _(indispensable)_.
* [Game Dev](https://gamedev.net/), des forums et tutoriels sur le Game Dev.

### Livres

* Game Engine Architecture (Third Edition), Jason Gregory
* 3D Game Engine Programming, Stefan Zerbst
* Head First Design Patterns, Eric Freeman & Elisabeth Robson
* 97 Things Every Programmer Should Know, Kevlin Henney
* The Art of Readable Code, Dustin Boswell & Trevor Foucher
* Becoming a Better Programmer, Pete Goodliffe

## Pour conclure

J'espère que cette introduction vous aura éclairé concernant les principes de bases de la création d'un moteur de jeu et vous aura fourni de quoi lire/regarder/réfléchir sur le sujet.
Comme dit précédemment, je vous conseille aussi de lire du code sur le sujet dans le cas où vous souhaiteriez voir à quoi peut ressembler du code de production et vous en inspirer.
Une des meilleures façon de progresser est aussi de s'essayer : développez de petits jeux et voyez où se trouve les faiblesses et les atouts de vos productions : les Game Jam sont là pour ça !.

Enfin, je suis bien entendu ouvert aux critiques et la section commentaires est là pour ça !

Si vous êtes intéressez par le monde du jeu vidéo indépendant français et souhaitez rencontrer des gens du domaine/trouver du travail, je vous invites vivement à rejoindre
[Arcadia Corporation](https://www.arcadia-corporation.com/) !

Je vous dis à bientôt pour le prochain article qui portera sur la mise en place de l'architecture de notre moteur.
