# Introduction

## Cadre

Ce travail a été réalisé dans l'entreprise WorkStreams.  
Le principal produit de WorkStreams est IntrepidKnowledge, une plateforme web de formation axée sur la collaboration et la mise à disposition de contenus de formation.

Le travail de diplôme est principalement basé sur le redéveloppement de cette plateforme avec une autre technologie.

## Contexte du projet

### Technologie front-end

La plateforme IntrepidKnowledge actuelle est écrite dans un framework JavaScript "maison".
La version JavaScript est l'EcmaScript 5 pour garder une compatibilité avec Internet Explorer 11 utilisé par un des client les plus importants de l'entreprise.

La construction de l'interface se fait déjà par le JavaScript et se base sur une structure JSON.
Celà permet à IntrepidKnowledge de se décliner en de nombreuses instances personnalisables indépendamment tout en gardant le même coeur.

Il n'y a actuellement pas de système de build (p.ex webpack). Le code est donc servi tel-quel au client.

### Technologie back-end

Le back-end consiste en:

- Une API basée sur PHP
- Un serveur websocket basé sur NodeJS

### Environnement de développement

Le développement se fait sur un serveur prévu à cet effet qui imite l'environnement de production.  
Il est compliqué de développer en local à cause du lien fort avec Linux et Apache (Utilisation de règles de routage et de liens symboliques).

Il n'y a pas de processus de tests ni de déploiement implémentés.
Le déploiement se fait en tirant les modifications vers le serveur de production avec Git.

## Objectifs

Le but du projet est la réécriture complète de la partie "front-end" de l'application avec une autre technologie. Pour le travail de diplôme seule une page (la page "groupes") est à réaliser.

La réécriture devrait permettre d'améliorer:

- La lisibilité du code par l'utilisation de JavaScript moderne (ES2020) et même TypeScript.
- La rapidité de création d'interface et la gestion d'état par l'utilisation de React.

En parallèle, une refonte de l'environnement de développement sera réalisée ce qui devrait permettre d'intégrer:

- Un développement simplifié en local
- Des tests (unitaires ou d'intégration)
- Une intégration continue partielle (lancement des tests sur GitLab, build de l'application pour le déploiement...)
- Une solution de monitoring d'erreurs post-déploiement

Les objectifs complets sont présentés dans le [Cahier des charges](../CDC%20Nicolas%20Maitre%20WS%20v2.docx)