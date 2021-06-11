# Introduction

## Cadre

Ce travail a été réalisé dans l'entreprise WorkStreams.  
Le principal produit de WorkStreams est IntrepidKnowledge, une plateforme web de formation axée sur la collaboration et la mise à disposition de contenus de formation.

Le travail de diplôme est principalement basé sur le redéveloppement de cette plateforme avec une autre technologie.

## Contexte du projet

### Technologie

La plateforme IntrepidKnowledge actuelle est écrite dans un framework JavaScript "maison".
La version JavaScript est l'EcmaScript 5 pour garder une compatibilité avec Internet Explorer 11 utilisé par un des client les plus importants de l'entreprise.

La construction de l'interface se fait déjà par le JavaScript et se base sur une structure JSON.
Celà permet à IntrepidKnowledge de se décliner en de nombreuses instances personnalisables indépendamment tout en gardant le même coeur.

### Environnement de développement

Le développement se fait sur un serveur prévu à cet effet. Il n'est pas possible ou compliqué de développer en local à cause du lien fort avec Linux et Apache (Utilisation de règles de routage et de liens symboliques).

## Objectifs
