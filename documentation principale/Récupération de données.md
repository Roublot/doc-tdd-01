# Récupération de données de l'API

La récupération de données depuis l'api a une place centrale dans le projet. En effet la page "groupes" doit pouvoir les afficher.

La librairie sélectionnée pour gérer les requêtes est ReactQuery

## React Query

React query est une librairie permettant de gérer la concurrence des requêtes ainsi que leurs états (chargement, erreur, succès).
React Query gère aussi le cache des requêtes en stockant leur résultat.

Ce que React Query ne gère pas est la méthode d'appel HTTP.  
React Query attend uniquement une fonction retournant une _Promise_ contenant le résultat de la requête. La librairie ne contraint donc pas à un protocole particulier.

La librairie identifie ses différentes queries à l'aide d'une clé (_string_). Si deux queries ont la même clé, seule une sera exécutée et les résultats seront réutilisés.

L'API de la librairie est exclusivement en _hooks_ et est donc utilisée dans les components fonctionnels.

## Implémentation

Pour permettre une utilisation simple des données dans le code l'approche a été de créer des _hooks custom_.  
Voici un exemple de leur utilisation dans le menu de la page "groupes":

```tsx
const GroupsLeftMenu: React.FC = () => {
  const { data, isLoading } = useInfiniteGroups();
  const groups = data?.pages?.flat();
  ...
  return (
    ...
    <MenuGroupsList groups={groups} isLoading={isLoading} />
    ...
  );
};
```

Le custom hook est simplement une abstraction de l'utilisation complète du `usInfiniteQuery` de React Query.

Voici son implémentation simplifiée:

```tsx
//unique key for this query in the app
export const GroupsQueryKey = "groups";

export function useInfiniteGroups(pageSize = 20) {
  const queryClient = useQueryClient();
  const query = useInfiniteQuery<QueryGroupPage>(
    GroupsQueryKey,
    async (context: QueryFunctionContext<...>) => {
      //query the actual groups
      const groups = await getGroups({
        start: context.pageParam.cursor,
      });
      return { groups };
    },
    {
      getNextPageParam(lastPage) {
        //get parameters for next page query
      },
      onSuccess(result) {
        //pre-fill group query with data
        result.pages.groups?.forEach((group) =>
          queryClient.setQueryData([GroupsQueryKey, group.id], group)
        );
      },
    }
  );
  return query;
}
```

Le code est assez compliqué car il s'agit d'une requête paginée.  
Voilà les éléments à noter:

- La première fonction asynchrone réalise l'appel api à l'aide de `getGroups`
- `getNextPageParams` permet d'obtenir les paramètres pour la prochaine "page" de données
- Lors du `onSuccess` de la requête, nous préremplissons les résultat des queries `/groups/[id]` afin d'économiser un appel lors de l'accès à un groupe déjà récupéré dans la liste.

## Rendu serveur

Pour l'intégration à NextJS

TODO finir phrase

## Résultat

Le résultat de l'utilisation de ReactQuery mets à disposition des hooks custom rendant facile l'intégration de données serveur sur n'importe quelle page. La solution intégrée de cache est performante et permet de ne pas à avoir à gérer cette partie complexe.
L'intégration de RectQuery à NextJS permet de préremplir le cache de la page "news" et ainsi économiser des appels à l'API.

## Améliorations

Cette solution ne prend pas avantage de la structure unique des données d'IntrepidKnowledge, dans le sens que chaque donnée a exactement la même base.
En développant une solution personnalisée, il aurait peut-être été possible de prévoir un système compatible avec le hors-ligne.

La solution actuelle s'apparente plus à un _POC_ car elle n'est utilisée que pour appeler la liste de groupes et les news.

Il aurait été intéressant d'intégrer la solution d'appels API réalisée en parallèle par Diogo Vieira, celà aurait permis l'utilisation d'un librairie commune d'appel au sein de l'entreprise.
