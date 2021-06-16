# Data fetching
La récupération de données depuis l'api a une place centrale dans le projet. En effet la page "groupes" doit pouvoir les afficher.

La librairie sélectionnée pour gérer les requêtes est ReactQuery
## React Query

## Server side

## Résultat
Le résultat de l'utilisation de ReactQuery mets à disposition des hooks rendant facile l'intégration de données serveur sur n'importe quelle page. La solution intégrée de cache fonctionne très bien et permet de ne pas à avoir à gérer cette partie complexe.
L'intégration de RectQuery à NextJS permet de préremplir le cache de la page "news" et ainsi économiser des appels à l'API.

## Améliorations
Cette solution ne prend pas avantage de la structure unique des données d'IntrepidKnowledge, dans le sens que chaque donnée a exactement la même base.
En développant une solution personalisée, il aurait peut-être été possible de prévoir un système compatible avec le hors-ligne.

La solution actuelle s'apparente plus à un *POC* car elle n'est utilisée que pour appeler la liste de groupes.

Il aurait été intéressant d'intégrer la solution d'appels API réalisée en parralèle par Diogo Veira, celà aurait permis l'utilisation d'un librairie commune d'appel au sein de l'entreprise.