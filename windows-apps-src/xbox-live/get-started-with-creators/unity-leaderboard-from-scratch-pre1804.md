---
title: Créer un classement dans Unity
description: Guide sur la création de votre propre classement dans Unity
ms.date: 04/24/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, unity, classements
ms.openlocfilehash: a62cb2b3bd4afebcc7aa9060db60ac167f052977
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657824"
---
# <a name="leaderboards-data-in-unity"></a>Données de tableaux de résultats dans Unity

Pour ceux d'entre vous qui souhaitent une expérience de classement personnalisé, cet article vous donne les outils nécessaires pour implémenter votre propre classement en accédant via les API disponibles pour les développeurs Unity. Une fois que vous comprenez comment extraire des données de classement, vous serez en mesure de s’appliquent à l’interface utilisateur de votre choix.

[!IMPORTANT]
> Cet article s’applique à une version du plug-in avant une mise à jour effectuée en mai 2018 (version 1804). Si vous installé le plug-in Xbox Live passé ce délai, ou que vous ne le n'avez pas encore téléchargé peut avoir une version plus récente qui utilise différents appels pour collecter des données de classement. En outre, vous constaterez que les captures d’écran de ce plug-in ne correspondent pas à ceux de la version la plus récente. Reportez-vous à la place à la [plus récente leaderboards unity à partir de l’article scratch](unity-leaderboard-from-scratch.md).


## <a name="call-for-leaderboard-data"></a>Appel pour le classement des données

Pour demander le classement des données à partir du service Xbox Live, que vous devez appeler  `void GetLeaderboard(XboxLiveUser user,  LeaderboardQuery query)`

Pour réussir cet appel, vous devez acquérir une `XboxLiveUser` à partir de [connexion](unity-prefabs-and-sign-in.md), ont un [configuré stat](add-stats-and-leaderboards-in-unity.md) avec une valeur pour au moins un lecteur et un formulaire un `LeaderboardQuery`. Vous pouvez consulter les articles liés Si vous ne connaissez pas déjà de la connexion à un utilisateur ou devez initialiser une statistique pour votre classement. Une fois que vous avez un initialisé une statistique le plus simple pour l’associer à votre script leaderboard consiste à inclure l’un des statistique prefabs : `IntegerStat`, `DoubleStat`, ou `StringStat` comme une variable publique. Votre stat doivent avoir la propriété ID son configurée sur le minimum comme c’est ce que nous allons utiliser pour le **statName** paramètre lorsque nous appelons de nos données de classement. Enfin, vous devez pour former un `LeaderboardQuery` objet.
Un `LeaderboardQuery` a quelques attributs qui peuvent être définies et qui aura un effet sur les données retournées :

- **StatName(Required) :** il s’agit de l’ID de la statistique votre classement récupérera les données, si aucune valeur n’est ne pas défini sur une valeur d’ID de statistiques, aucune donnée n’est retournée.
- **SocialGroup :** vos données de classement retournées sont filtrées selon la valeur de cette chaîne à vos amis, vos amis préférés ou si elle est non définie, récupère un tableau de scores global non filtrée. La valeur « » sera retourne une liste non filtrée, la valeur « all » renvoie une liste avec uniquement vos amis qu’il contient, comme « favori » renvoie une liste avec uniquement vos amis favoris présents.
- **SkipResultToRank :** si la valeur, cette variable uint détermine ce que les données de classement de classement démarrera lors du retour. Classement commence au rang 1.
- **SkipResultToMe :** si défini sur true cette valeur booléenne entraîne les données de classement retournées à partir du `XboxLiveUser` utilisé dans le `GetLeaderboard()` appeler.
- **commande :** Énumérations du type `Microsoft.Xbox.Services.Leaderboard.SortOrder` ont deux valeurs possibles, croissant ou décroissant. Définition de cette variable pour votre requête détermine l’ordre de tri de votre classement. Par défaut classements retournent des données dans l’ordre décroissant.
- **MaxItems :** Cette uint détermine le nombre maximal de lignes à retourner par l’appel à `GetLeaderboard()`.

Formation de votre LeaderboardQuery peut ressembler à ce qui suit :

```csharp
using Microsoft.Xbox.Services.Leaderboard;

LeaderboardQuery query = new LeaderboardQuery
        {
            StatName = stat.ID,
            SkipResultToRank = 100,
            MaxItems = 5
        };
```

À l’aide de cette requête renvoie cinq lignes de la partie du classement, en commençant à la 100th le classement individuels pour la donnée statistique.

> [!WARNING]
> Paramètre SkipResultToRank supérieure au nombre de lecteurs contenus dans le classement entraîne le classement des données à retourner avec des lignes nulles.

Maintenant que nous avons tous les éléments ensemble, nous pouvons appeler le `GetLeaderboard(XboxLiveUser user,  LeaderboardQuery query)` fonction, où nous allons utiliser notre signé dans `XboxLiveUser` (probablement une `XboxLiveUserInfo.User`) provenant connectez-vous, en tant que notre utilisateur et le `LeaderboardQuery` nous venons de créer en tant que notre requête.

Votre appel de fonction de classement peut ressembler à ce qui suit :

```csharp
using Microsoft.Xbox.Services.Leaderboard;

public void LoadLeaderboard()
    {
        // check to make sure you have a valid stat
        if (this.stat == null)
        {
            // TO DO: Display "Stat not specified" error message!
            return;
        }

        // check to make sure you have an XboxLiveUser
        if (this.xboxLiveUser == null)
        {
            if (XboxLiveUserManager.Instance.UserForSingleUserMode != null
                && XboxLiveUserManager.Instance.UserForSingleUserMode.User != null)
            {
                // set the XboxLiveUser if one is available
                this.xboxLiveUser = XboxLiveUserManager.Instance.UserForSingleUserMode.User;
            }
            else // if you don't have a user signed in display an error message and exit. 
            {
                // TO DO: Display "User Not logged In" error message
                return;
            }
        }

        LeaderboardQuery query;

        query = new LeaderboardQuery
        {
            StatName = this.stat.ID,
            MaxItems = this.maxItems,

        };

        XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query);
    }
```

À présent, vous avez peut-être remarqué que notre classement récupération fonction `GetLeaderboard()` retourne void et ne renvoie pas les données de classement que nous recherchons. Nous avons réellement récupère les données de classement dans une fonction d’événement décrite dans la section suivante.

## <a name="receive-the-leaderboard-data"></a>Recevoir les données de classement

Afin de récupérer les données de classement que vous devrez ajouter une fonction à l’écoute pour le `StatsManagerComponent` instance pour votre titre. Vous devez ajouter la ligne suivante de code pour le `Awake()` fonction de votre code : `StatsManagerComponent.Instance.GetLeaderboardCompleted += this.MyGetLeaderboardCompletedFunction`.

```csharp
private void Awake()
    {
        this.EnsureEventSystem();
        XboxLiveServicesSettings.EnsureXboxLiveServicesSettings();

        StatsManagerComponent.Instance.GetLeaderboardCompleted += this.GetLeaderboardCompleted;
    }
```

Le `StatsManagerComponent` écoute les événements de saisie semi-automatique de classement. En exécutant cette ligne de code, vous allez ajouter une fonction à la liste des fonctions d’être appelées lorsqu’un événement d’achèvement de classement se produit. Dans cet exemple que la fonction est appelée `MyGetLeaderBoardCompletedFunction()`, vous pouvez nommer la fonction que vous le souhaitez dans votre propre script. La fonction « MyGetLeaderboardCompletedFunction » vous devrez prendre deux paramètres, un objet qui représente l’expéditeur, et un `Microsoft.Xbox.Services.Client.StatEventArgs` paramètre. L’interpréteur de commandes de votre fonction peut ressembler à ceci :

```csharp
using Microsoft.Xbox.Services.Statistics.Manager;

private void MyGetLeaderBoardCompletedFunction(object sender, StatEventArgs statArgs)
    {
        //Do Something;
    }
```

La première chose que cette fonction doit faire est de rechercher les erreurs qui se trouve dans le `StatEventArgs` statArgs de paramètre. StatArgs contient un `StatisticEvent` appelé EventData, qui contient le `System.Exception` appelé ErrorInfo. Si une erreur s’est produite lors de la récupération des données de classement vous la trouverez dans statArgs.EventData.ErrorInfo. Vous pouvez ajouter un cas simple instruction au code précédent pour rechercher les erreurs.

```csharp
using Microsoft.Xbox.Services.Statistics.Manager;

private void MyGetLeaderBoardCompletedFunction(object sender, StatEventArgs statArgs)
    {
        if (e.EventData.ErrorInfo != null && e.EventData.ErrorInfo.Message == "")
        {
            // TO DO: Display error message
            return;
        }
    }
```

Après avoir confirmé qu’il n’y a aucune erreur, stocker les résultats de la demande de classement qui se trouvent dans `statArgs.EventData.EventArgs.Result`. `Result` est un `LeaderBoardResult` objet qui contient les données que vous avez besoin pour remplir votre classement. Dans notre exemple de code nous extraire ces données et envoyer à une autre fonction appelée LoadResult.

```csharp
using Microsoft.Xbox.Services.Statistics.Manager;

private void MyGetLeaderBoardCompletedFunction(object sender, StatEventArgs statArgs)
    {
        if (e.EventData.ErrorInfo != null && e.EventData.ErrorInfo.Message == "")
        {
            // TO DO: Display error message
            return;
        }

        LeaderboardResultEventArgs leaderboardArgs = (LeaderboardResultEventArgs)statArgs.EventData.EventArgs;
        this.LoadResult(leaderboardArgs.Result);
    }
```

Le `LeaderboardResult` résultat que nous envoyons à la fonction LoadResult comporte toutes les données que nous avons besoin pour lire les données de classement a été renvoyées comme ainsi que pour effectuer des appels supplémentaires pour récupérer les rangs pas encore retournés par l’appel d’origine. Vous souhaitez stocker les résultats dans une variable de classe pour votre script leaderboard comme suit :

```csharp
using Microsoft.Xbox.Services.Leaderboard;

void LoadResult(LeaderboardResult result)
    {
        this.leaderboardData = result;
    }
```

Ceci est important, car le `LeaderboardResult` contient une propriété HasNext qui détermine s’il y a une section ultérieure du classement qui peut être récupérée. La `LeaderboardResult` stocke également une variable pour le total de lignes qui composent le classement. Ces propriétés seront importantes à la navigation dans votre classement. Pour extraire les données à partir de votre `LeaderBoardResult` simplement implémenter un pour la boucle à l’aide la `LeaderboardResults` liste des `LeaderboardRow` appelée `Rows`. Dans notre exemple de code nous allons simplement pour concaténer les valeurs dans chaque `LeaderboardRow` en une chaîne à afficher.

```csharp
using Microsoft.Xbox.Services.Leaderboard

void LoadResult(LeaderboardResult result)
{

    this.leaderboardData = result;

    this.leaderBoardText.text = "" // leaderBoardText is a UnityEngine.UI text box.

    foreach (LeaderboardRow row in this.leaderboardData.Rows)
    {
        leaderBoardText.text += string.Format("Rank: {0} | Gamertag: {1} | {2}:{3}\n\n", row.Rank, row.Gamertag, this.stat.DisplayName, row.Values[0]);
    }
}
```

Dans notre exemple, nous avons utilisé les propriétés de classement, Gamertag et valeurs de `LeaderBoardResult` pour remplir notre chaînes, ainsi que la propriété DisplayName de la statistique associée avec le classement.

Je suis sûr que vous serez en mesure de faire quelque chose de plus créatif avec toutes ces données de classement.

## <a name="navigating-the-leaderboard-data"></a>Navigation dans les données de classement

Dans les cas courants, vous ne chargera pas chaque rang unique dans votre classement à la fois et devrez être en mesure de parcourir les rangs pour afficher les différentes sections du classement de l’utilisateur. Supposons que vous disposez d’un classement avec cent classés joueurs. Dans votre appel initial à `GetLeaderboard()` vous récupérez dix `LeaderboardRows` et de les afficher pour le lecteur. Le joueur souhaiterez peut-être plus que les dix meilleurs joueurs. Pour obtenir le jeu suivant de dix utilisateurs le plus simple consiste à déterminer ou non le `LeaderboardResult` vous avez stocké à partir de votre dernière requête a plusieurs lignes à récupérer et à appeler ensuite `GetLeaderboard()` avec de cette LeaderboardResult `NextQuery` propriété. Le code suivant récupère l’ensemble suivant de classement des données.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

void GetNextLeaders()
    {
        if(this.leaderboardData.HasNext)
        {
            LeaderboardQuery query = this.leaderboardData.NextQuery;
            XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query);
        }
        else
        {
            //TO DO: Display an error message or go back to the beginning of the leaderboard as the situation demands.
        }
    }
```

Déplacement vers l’arrière dans votre classement est un peu plus difficile car il n’existe pas de fonction pour extraire le X nombre de rangs à partir de votre classement précédent. Afin de récupérer les classements précédentes, vous devrez écrire votre propre logique. Une méthode consisterait à stocker votre `MaxItems` par `LeaderboardQuery` et calculer le quel classement que vous avez besoin pour passer à l’aide du `SkipToRank` attribut de votre `LeaderboardQuery`. Ce code peut ressembler à ceci :

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

void GetPreviousLeader()
{
    if(leaderboardData == null || leaderboardData.Rows.Count < 1)
    {
        return;
    }

    uint topRank = leaderboardData.Rows[0].Rank; //get the first rank of the leaderboard.
    uint targetRank = topRank - this.maxItems > 0 ? topRank - this.maxItems : 0; //set your targetRank equal to the current top rank of your leaderboard minus the configured number of rows to display at a time.

    LeaderboardQuery query = new LeaderboardQuery // make a new query with the calculated targetRank
    {
        StatName = stat.ID,
        SkipResultToRank = targetRank,
        MaxItems = this.maxItems
    };

    XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query); // call the GetLeaderboard() function with the new query
}
```

Le scénario le plus courant final est qu’un lecteur peut simplement voir leur place sur le classement. Facilement le faire en appelant le `GetLeaderboard()` fonction avec une requête où le `SkipResultToMe` attribut est défini sur true.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

    void GetRankForTag()
    {

        LeaderboardQuery query = new LeaderboardQuery // make a new query with the calculated targetRank
        {
            StatName = stat.ID,
            SkipResultToMe = true,
            MaxItems = this.maxItems
        };

        XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query); // call the GetLeaderboard() function with the new query
    }
```

Si vous souhaitez vous plonger dans un exemple plus détaillé de classement vous pouvez toujours lire le script Leaderboard.cs dans le dossier XboxLive Plugin sous ressources >> XboxLive >> Scripts >> Leaderboard.cs.