# Organisation Multi-instance

- [Organisation Multi-instance](#organisation-multi-instance)
  - [Problématique](#problématique)
  - [Partage du code](#partage-du-code)
    - [Repository par app avec packages](#repository-par-app-avec-packages)
    - [Monorepo](#monorepo)
    - [NX](#nx)
      - [Structure monorepo](#structure-monorepo)
      - [Dépendances](#dépendances)
  - [Routage et pages](#routage-et-pages)
    - [Pages communes](#pages-communes)
    - [Pages spécifiques](#pages-spécifiques)
  - [Components spécifiques](#components-spécifiques)
    - [Approches](#approches)
      - [Approche historique - configuration JSON](#approche-historique---configuration-json)
      - [Fichier de configuration](#fichier-de-configuration)
      - [Provider de components](#provider-de-components)
  - [Création d'instance](#création-dinstance)
  - [Problèmes et améliorations](#problèmes-et-améliorations)
    - [Librairie "ui-components"](#librairie-ui-components)
    - [Librairies "ui-pages"](#librairies-ui-pages)

## Problématique

IntrepidKnowledge possède une quinzaines de plateformes (instances) conçues sur la même base. Elles partagent la plupart du code ce qui permet de faire des mises-à-jour communes.

Le partage du code est physique, c'est à dire que c'est les fichiers de code sont communs. Le partage se fait par des liens symboliques.

Le désavantage de cette approche est qu'il est difficile de mettre à jour une instance sans impacter les autres. L'absence de procédure de test veut aussi dire qu'il arrive parfois d'apporter des régressions involontaires aux autres instances.

Plusieurs mécanismes ont été mis en place pour réimplémenter le multi-instance et également limiter les régressions.

## Partage du code

Chaque application/instance doit pouvoir avoir accès à des librairies de code partagé.

Pour permettre la création de librairies partagées, deux structures de repository se sont présentées:

- Repository par app avec packages
- Monorepo

### Repository par app avec packages

Une première option est de créer des repositories séparés pour chaque librairie et d'ensuite les importer dans chaque projet de plateforme à l'aide d'un gestionnaire de dépendances tel que _yarn_ ou _npm_.

**Avantages**

L'avantage principal de cette approche est la séparation physique entre les plateformes et leur versions grâce aux repositories séparés. Les modifications aux packages communs peuvent être réalisées et intégrées au fur et à mesure dans les plateformes en modifiant leur fichier de dépendance.
Le risque de régression est donc faible.

**Inconvénients**

Le désavantage est la complexification du développement car il devient difficile de voir rapidement le résultat après une modification d'une librairie.
En effet il faudrait d'abord déployer la librairie puis mettre à jour les dépendances d'une application avant de voir le résultat de la modification dans celle ci.

### Monorepo

La deuxième option est la création d'un repository commun à toutes les plateformes et librairies.

**Avantages**

Avec cette solution, les modifications effectuées dans les librairies se répercutent instantanément dans les instances car elles se situent dans le même repository.

Celà permet de rapidement déployer une modification sur toutes les instances sans avoir à mettre à jour les dépendances chez chacune d'elles.

**Inconvénients**

Cette solution présente quelques inconvénients:

1. Le risque de régression des autres instances est toujours présent
2. Les messages de commits des instances et des librairies sont tous à plat
3. Le repository a une grande taille.
4. L'intégration continue redéploie l'intégralité des applications/instances

**Solutions**

1. Le risque de régression peut être très diminué grâce à une couverture de tests suffisante, ce qui est aussi un objectif de ce projet.
2. Des outils existent afin d'avoir un affichage des commits par dossier (par exemple `git log --follow [folder]`)
3. Le problème de taille n'est n'est pas vraiment un car il n'y a pas besoin de télécharger des dépendances pour toutes les librairies. En effet elles sont déjà présentes dans le repo. De plus le `clone` initial n'a besoin d'être fait qu'une fois.

   [Des propositions](https://gitlab.com/groups/gitlab-org/-/epics/93) existent cependant pour tenter de résoudre ce problème en téléchargeant les fichiers à la demande.

4. Ce problème est résolu par NX, expliqué [plus bas](#nx).

**Choix**

Les avantages listés au dessus ont permit de choisir la structure MonoRepo avec NX.

Le point décisif a été la vitesse de développement. En effet chez WorkStreams, le développement est principalement réalisé par une seule personne. La séparation en différents packages npm aurait rendu le développement trop fastidieux.

### NX

NX est un outil de gestion de monorepo.
L'outil est développé par Nrwl.

Les fonctionnalités clés de NX sont:

- Structure monorepo
- Intégration par dépendance
- Générateurs

#### Structure monorepo

NX propose une structure basée sur des applications et des librairies.
La structure de dossiers est la suivante:

    📦 nx-monorepo
    ┣━📂 apps
    ┃ ┣━📂 e-cffe.ch
    ┃ ┣━📂 hfrformation.ch
    ┃ ┗━📂 intrepidknowledge.ch
    ┣━📂 dist
    ┃ ┗━📂 apps
    ┃   ┗━📂 intrepidknowledge
    ┣━📂 libs
    ┃ ┣━📂 helpers
    ┃ ┗━📂 ui
    ┃   ┣━📂 assets
    ┃   ┣━📂 components
    ┃   ┣━📂 contexts
    ┃   ┣━📂 hooks
    ┃   ┣━📂 layouts
    ┃   ┣━📂 pages
    ┃   ┃ ┣━📂 next
    ┃   ┃ ┣━📂 react
    ┃   ┃ ┗━📂 styles
    ┃   ┗━📂 popups
    ┗━📂 tools
      ┗━📂 generator
        ┗━📂 instance

Informations notables:

- `apps` contient les applications
  - `e-cffe.ch`, `hfrformation.ch` et `intrepidknowledge.ch` sont des instances de la plateforme IntrepidKnowledge.
- `libs` contient les différentes librairies partagées
  - `ui/assets` contient des svg transformés en components React par _svgr_
  - `ui/components` contient la plupart des components utilisés dans l'application. Celà pose un problème (Voir la section [Problèmes et améliorations](#librairie-ui-components)).
  - `ui/pages` contient les pages communes à toutes les instances
    - `next` et `react` contiennent pratiquement les mêmes pages (Voir la section [Problèmes et améliorations](#séparation-des-librairies)).
    - `styles` contient une librairie de _styled-components_.
- `tools/generator` contient les générateurs custom (Voir la section [Création d'instance](#création-dinstance))
- `dist` contient les fichiers de build des applications et librairies.

#### Dépendances

La puissance de NX réside dans sa construction d'un arbre de dépendances. Un projet ne sera _rebuilt_ que si ses dépendances changent.

Les dépendances sont identifié lors de leur import dans une autre librairie ou app. Les chemins d'imports sont virtuels et se composent de la sorte:

    @[workspace]/[library]

L'exemple suivant montre l'import d'un svg depuis la librairie `ui-assets`

```tsx
import { Star } from "@ws/ui/assets";
```

_Graphique de dépendances du projet (généré par NX). Les flèches représentent une dépendance_

![dep graph](/assets/full-dep-graph.png)

Les dépendances entre applications et librairies sont unidirectionnelles ce qui veut dire qu'une librairie ne peut pas dépendre d'une application.

Cet exemple montre bien l'unidirectionnalité des dépendances

![simple dep graph](/assets/intrepid-dep-graph.png)

## Routage et pages

Avec NextJS, le routage est lié à la structure de dossier des pages.

Un exemple de structure de routage est disponible dans la section [Technologies de rendu -> NextJS -> Avantages -> Structure](./Technologies%20de%20rendu.md#structure)

Ce design permet de très facilement mettre en place des pages personnalisées par instance.

### Pages communes

Les pages communes aux instances sont exportées depuis la librairie `ui-pages`

Exemple de définition d'une route pour une page commune entre instances. Ici la page "groups".  
Le fichier se situe à `intrepidknowledge/src/pages/groups/index.tsx`

```tsx
export { GroupsPage as default } from "@ws/ui/pages/next";
export { getSPTranslations as getStaticProps } from "@ws/next-ssr";
```

Dans cet exemple la page "groups" est utilisée telle-quelle depuis la librairie `ui-pages`.

La seconde ligne est expliquée dans la section [Traductions]()  
TODO: Link to section

### Pages spécifiques

Pour créer une page spécifique, il suffit de la créer directement dans le fichier de route.

Il n'y a pas de page spécifique pour le moment donc le code n'est qu'un exemple.

```tsx
  const PortfolioPage:PageWithLayout = () => {
    return <>
      <h1>Mon portfolio</h1>
    </>
  }
  ...
  export PortfolioPage as default;
```

La notion de `PageWithLayout` est expliquée dans la section [Technologies de rendu -> NextJS -> Problèmes rencontrés -> Layouts](Technologies%20de%20rendu.md#layouts)

## Components spécifiques

Même si la plupart du code est partagé entre les instances, il faut pouvoir injecter des personnalisations dans les différentes instances.

### Approches

#### Approche historique - configuration JSON

Dans la plateforme historique la construction se fait par des fichiers JSON définissant la structure et le paramétrage de toute l'application. Ele agit plus comme un fichier de configuration. Elle définit les pages et leur contenu (barre d'action, appels api, construction de liste). Ces configurations sont passées en profondeur à des méthodes de construction.

Des fichiers d'_override_ sont définit dans chaque instances et permettent de remplacer certaines configurations en profondeur.

Cette approche fonctionne bien lorsque qu'un changement majeur doit être effectué entre les instances, par exemple le contenu d'une page ou la présence d'une barre d'action.

Cependant quand des changements dans les "components" eux-mêmes doivent être faits, deux approches sont faites:

- Ajout de paramètres supplémentaires au component permettant un affichage différent.
- Override du "component" pour l'instance. Le component doit être entièrement réécrit.

Ces deux approches ont le problème de complexifier le code avec des conditions ou d'apporter de la duplication de code.

Les styles peuvent également avoir des _overrides_.

Globalement ce système fonctionne bien mais peut être amélioré.

#### Fichier de configuration

Une solution est de reprendre un système similaire à la plateforme historique sauf que le fichier de configuration n'est pas utilisé directement pour construire l'interface mais est passé par _props_ aux components en profondeur lorsqu'on veut pouvoir paramétrer un affichage particulier.

Le problème est le même que le précédent car cette approche mène à complexification du code. Cependant grâce à la composition de React ce problème serait probablement moins présent.

#### Provider de components

La solution, retenue, du provider de components est d'utiliser un _provider_ React pour mettre à disposition les components spécifiques. Ainsi nous n'avons plus besoin de passer de configuration en profondeur car l'override se fait au niveau du component.

La solution n'a pas été mise en oeuvre pour le multi-instance mais a été utilisée pour obtenir des components compatible entre React-Router et Next-Router. En effet l'application éléctron utilise React-Router.

L'exemple ci dessous montre l'utilisation du component `NavLink` qui est différent en fonction de la plateforme (react/next). Dans ce cas le hook de traduction `useTranslation` y est également récupérée par le hook `usePlatform`.

```tsx
export const GroupsLeftMenu: React.FC = () => {
  //get platform specific items
  const { NavLink, useTranslation } = usePlatform();
  const { t } = useTranslation();

  return (
    <NavLink href="/groups">{t("intrepid.pages.groups.allMyGroups")}</NavLink>
  );
};
```

Ces éléments sont présents dans le `PlatformContext` dont voici la définition.

```tsx
export const PlatformContext = React.createContext<PlatformContextValue>({
  Link: undefined,
  NavLink: undefined,
  Image: undefined,
  Head: undefined,
  useTranslation: undefined,
  useRouter: undefined,
});
```

Le typage est particulier car il doit pouvoir contenir les différent types en fonction du nombre de plateformes. L'union de type `|` est utilisée. (Lorsqu'un seul type est spécifié c'est que la version React n'a pas encore été implémentée).

```tsx
export interface PlatformContextValue {
  Link: typeof ReactLink | typeof NextLink;
  NavLink: typeof ReactRouterNavLink | typeof NextNavLink;
  Image?: typeof NextImage | HTMLFactory<HTMLImageElement>;
  Head: typeof Helmet | typeof NextHead;
  useTranslation: typeof nextUseTranslation;
  useRouter: typeof useRouter;
}
```

Pour permettre ce comportement l'application entière est encapsulée dans un component provider du `PlatformContext`. Les components fournis sont évidemment différents entre les plateformes,

App Next (\_app.tsx):

```tsx
function App({ Component }: AppProps): JSX.Element {
  const platformContextValue: PlatformContextValue = {
    Head: NextHead,
    Link: NextLink,
    NavLink: NextNavLink,
    Image: NextImage,
    useTranslation,
    useRouter,
  };

  return (
    <PlatformContext.Provider value={platformContextValue}>
      <Component />
    </PlatformContext.Provider>
  );
}
```

App React (app.tsx):

```tsx
export function App(): JSX.Element {
  const platformContextValue: PlatformContextValue = {
    Link: ReactLink,
    NavLink: ReactNavLink,
    Head: Helmet,
    //fake implementations to prevent crash:
    useTranslation: () => ({ t: (s: string) => s }),
    useRouter: () => ({}),
  };

  return (
    <PlatformContext.Provider value={platformContextValue}>
      <Router>...</Router>
    </PlatformContext.Provider>
  );
}
```

Il est alors facile d'imaginer l'implémentation d'un mécanisme similaire pour les instances avec un `InstanceContext` à l'instar du `PlatformContext`.

Le système d'`InstanceContext` n'a malheureusement pas été implémenté.

**Problème de cette approche**

Le principal problème de cette approche est que les components doivent avoir la même structure (interface) de _props_ afin de pouvoir être interchangeables.

Cependant ce problème ne devrait se manifester lorsque des changements plus profonds dans la fonctionnalité du component sont modifié entre les plateformes.

Pour résoudre ce problème, l'utilisation de composition devrait suffire, c'est à dire encapsuler les changements d'interface à l'aide d'un component intermédiaire.

## Création d'instance

Un objectif secondaire du projet était la mise en place d'une automatisation de la création d'instances. En effet cette création implique un travail répétitif de configuration et de personnalisation.

L'idée d'un script de création acceptant en paramètre les personnalisations les plus communes s'est alors imposée.

L'outil NX propose la création de "générateurs". Ce sont des scripts

## Problèmes et améliorations

### Librairie "ui-components"

Actuellement trop d'éléments se trouvent dans la librairie `ui/component`. Celà pourrait poser un problème car à la moindre modification d'un component contenu dans cette librairie, la totalité des applications ayant une référence à la librairie auraient besoin d'être _built_ - _tested_ - _deployed_.

En extrayant les components vers des sous-librairies plus précises de components, par exemple `ui/components/groups` qui contiendrait tous les components liés aux groupes, ce problème pourrait disparaître.

Ce n'est pas un gros problème pour le moment étant donné le nombre réduit d'applications.

### Librairies "ui-pages"

Le fait que la librairie "ui-page-next" et "ui-pages-react" soient des librairies séparées est dû à l'application Electron développée en parallèle sur la même base. Des problèmes de compatibilité se sont présentés car React et Next n'utilisent pas le même routage.

Une solution à ce problème a désormais été trouvée, comme expliqué dans la section [Components spécifiques](#components-spécifiques), ce qui permettra à la séparation entre les deux librairies de disparaître.

> générateur d'instance

<!-- Styles for markdown numbered headings -->
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
