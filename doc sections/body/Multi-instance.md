# Organisation Multi-instance

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

- Le risque de régression des autres instances est toujours présent
- Les messages de commits des instances et des librairies sont tous à plat
-

# BLABLABLA

## Monorepo et librairies

NX...
Système de librairies
Dépendances unidirectionnelles

## Providers de components

React Providers, Multi targets (Electron)

## Comparaison customisation par routage vs provider

## Importance des tests pour le multi-instance
