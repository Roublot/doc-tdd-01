# Technologies de rendu

## ReactJS

### Qu'est-ce que ReactJS

ReactJS est une librairie de rendu d'interface s'interfaçant entre le JavaScript et le DOM.
La librairie React est Open Source et est maintenue par Facebook.

### Avantages

#### Gestion d'état

ReactJs a une vision très fonctionelle du rendu Web. L'affichage est le résultat d'une séries de fonctions.

    UI = F(props)

- Permettre une gestion d'état simplifiée, par exemple les selections

## NextJs

### Qu'est-ce que NextJS?

NextJS est un framework construit autour de ReactJS permettant le rendu de pages par le serveur en plus du client.

Le framework NextJS est Open Source et est maintenu par Vercel.

### Avantages

#### Vitesse d'affichage

NextJS utilise principalement deux méthodes pour accélérer le premier chargement de l'application.

- Rendu côté serveur

  NextJS permet d'effectuer un rendu complêt ou partiel de chaque page sur le serveur. L'utilisateur aura donc une page déjà construite même avant le chargement du code JavaScript.
  Celà permet d'augmenter l'impression de vitesse du site web.

- Imports dynamiques

  Contrairement à une Single Page Application classique, l'intégralité du code n'est pas chargé au premier chargement.  
  Seules les dépendances de la page chargée le sont.

  Ce processus permet de limiter la taille du bundle JavaScript envoyé au client.

  Pour améliorer la vitesse de changement de page, NextJS peut précharger les pages référencées sur la page actuelle (par les balises "Link"). Ainsi il n'y aura pas de délai lors du changement de page.

#### Structure

NextJS étant un framework, il impose une certaine structure. Celà permet de ne pas avoir à faire certains choix.

La structure des pages, par exemple, est liée au système de routage.

Example de routage des groupes:

![Example de routage des groupes](/assets/route_groups.png)

Le routage se fait d'une façon similaire au traditionnel php, chaque fichier correspondant à une route.

### Intérêt dans le cas d'intrepid

Intrepidknowledge contient un grand nombre de pages dont le contenu peut être très différent.  
Voici trois example de pages:

- Page "Groupes"

  Cette page contient la liste des groupes dont l'utilisateur connecté fait partie.

  C'est l'exemple de page où NextJS est probablement le moins utile, se contentant de précharger les éléments statiques tels que les menus.

- Page "Actualités"

  Cette page contient les actualités de la plateforme et est commune à tous les utilisateurs.

  NextJS permet de complètement prégénérer la page sur le serveur car les données seront les mêmes pour tous les utilisateurs. Le chargement sera donc pratiquement instantané.

- Page "Calendrier"

  Cette page contient un calendrier interactif venant d'une autre librairie. Son contenu est également spécifique à chaque utilisateur.

  NextJs permet de charger la librairie de calendrier uniquement sur cette page. Le chargement des autres pages de l'application sera ainsi accéléré.

On peut donc dire qu'avec NextJS, IntrepidKnowledge gagnerait en:

- Vitesse de chargement

  Le "bundle" JavaScript actuel mesure 8.73MB (1.65MB après cache) et utilise 39 fichiers. Il pourrait être réduit grâce à la minification (webpack) et les imports dynamiques de NextJS.

- Stabilité

  L'utilisation de React facilite la gestion d'état de l'application, par exemple la sélections d'élements.

### //CHOSES A DIRE//

- Routage
  - Respect de la convention REST
  - Approche pour permettre le multi-instance

## Choix du type de rendu

- page groupes
- page actualités
