- [Technologies de rendu](#technologies-de-rendu)
  - [ReactJS](#reactjs)
    - [Qu'est-ce que ReactJS](#quest-ce-que-reactjs)
    - [Avantages](#avantages)
      - [Gestion d'état](#gestion-détat)
      - [Construction d'interface](#construction-dinterface)
      - [Communauté](#communauté)
    - [Intérêt dans le cas d'intrepid](#intérêt-dans-le-cas-dintrepid)
  - [NextJS](#nextjs)
    - [Qu'est-ce que NextJS?](#quest-ce-que-nextjs)
    - [Avantages](#avantages-1)
      - [Vitesse d'affichage](#vitesse-daffichage)
      - [Structure](#structure)
    - [Intérêt dans le cas d'intrepid](#intérêt-dans-le-cas-dintrepid-1)


# Technologies de rendu

## ReactJS

### Qu'est-ce que ReactJS

ReactJS est une librairie de rendu d'interface s'interfaçant entre le JavaScript et le DOM HTML.  
Elle simplifie la création et la gestion d'interface.

La librairie React est Open Source et est maintenue par Facebook.

### Avantages

#### Gestion d'état

React a une vision très fonctionelle du rendu Web.  
Pour simplifier on peut dire que l'affichage est le résultat d'une fonction ou "component" eux mêmes faisant appel à d'autres "components".

    UI = F(props)

Le principe de base est le "**event up, data down**" qui signifie que les données sont gérées par les parents et non pas les enfants -> "data down".  
Les enfants notifient les parents pour mettre à jour ces données -> "event up".

Les "Providers" sont un exemple typique de ce principe _eudd_. Ce sont des components qui mettent à disposition un contexte (valeur) et qui viennent se positionner comme parent des components ayant besoin de leur contexte.  
La valeur du contexte ne peut ensuite être modifiée que par un évenement remontant depuis les components enfants.

Cette gestion d'état permet d'avoir des résultats stables et de simplifier sa gestion. Le DOM ne sera jamais dans un état intermédiaire car il est uniquement contrôlé par la librairie.

#### Construction d'interface

React, couplé au JSX, permet de construire les interfaces avec une sythaxe proche de l'HTML.

Example de component (fonctionnel) utilisant la synthaxe JSX:

```jsx
function UserCard(user) {
  return (
    <div>
      <img src={user.image} />
      <p>Profil de {user.name}</p>
      <button onClick={() => displayProfile(user)}>Voir le profil</button>
    </div>
  );
}
```

> Le JSX transforme simplement les tags vers la fonction `React.createElement`

#### Communauté

L'utilisation de React permet d'avoir accès à une grande quantité de components déjà réalisés.

Il est également facile de trouver de l'aide par exemple sur StackOverflow.com

### Intérêt dans le cas d'intrepid

React permettra d'améliorer:

- La vitesse de création des interfaces

  Le JSX permettra considérablement d'améliorer la vitesse de création des interfaces ainsi que la clareté du code d'interface.

- La gestion d'état

  La plateforme IntrepidKnowledge contient beaucoup d'états. Par exemple les selections d'éléments, le presse papier et surtout un éditeur de slides très complexe.

  L'utilisation de react peut simplifier tous ces éléments.

- Compatibilité

  L'utilisation du JSX nous obligeant à utiliser Babel, l'application peut être transpilée en ES5 permettant de développer en JavaScript moderne et même TypeScript.

## NextJS

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

- Structure

  Actuellement les pages externes "commerciales / hors-plateforme" sont réalisées dans un tout autre framework "maison".  
  Passer le tout sous NextJS permettrait d'avoir une structure commune à "l'interne" et "l'externe" et ainsi réutiliser des éléments.

  Le routage en serait également amélioré car il pourrait respecter les standards REST. Actuellement seule la première section du path identifie la page.  
  `/group/#16253` deviendrait `/groups/16253`


<!-- Styles for headings -->
<style>
body {
    counter-reset: h1 0;
}

h1 {
    counter-reset: h2;
}

h2 {
    counter-reset: h3;
}

h3 {
    counter-reset: h4;
}

h1:before {
    counter-increment: h1;
    content: counter(h1) ". ";
}

h2:before {
    counter-increment: h2;
    content: counter(h1) "." counter(h2) ". ";
}

h3:before {
    counter-increment: h3;
    content: counter(h1) "." counter(h2) "." counter(h3) ". ";
}

h4:before {
    counter-increment: h4;
    content: counter(h1) "." counter(h2) "." counter(h3) "." counter(h4) ". ";
}</style>