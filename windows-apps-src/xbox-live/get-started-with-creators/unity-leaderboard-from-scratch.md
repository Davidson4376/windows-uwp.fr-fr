---
title: Script d’un classement dans Unity
description: Guide sur la création de votre propre classement dans Unity
ms.date: 04/24/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, unity, classements
ms.openlocfilehash: 6e73ffd9b55f3638eb3cf4245c6f7943fe92dc48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608754"
---
# <a name="script-a-leaderbaord-gameobject"></a>Un GameObject leaderbaord de script

Pour ceux d'entre vous qui souhaitent une expérience de classement personnalisé, cet article vous donne les outils nécessaires pour implémenter votre propre classement en accédant via les API disponibles pour les développeurs Unity. Une fois que vous comprenez comment extraire des données de classement, vous serez en mesure de s’appliquent à l’interface utilisateur de votre choix.

## <a name="call-for-leaderboard-data"></a>Appel pour le classement des données

Il existe deux appels d’API pour récupérer des données de classement.

- `void GetLeaderboard(XboxLiveUser user, string statName, LeaderboardQuery query)`
- `void GetSocialLeaderboard(XboxLiveUSer user, string statName, string socialGroup, LeaderboardQuery query)`

Pour modifier correctement une des ces appels retournent des données, vous devez acquérir une `XboxLiveUser` par [connexion](unity-prefabs-and-sign-in.md), ont un [configuré stat](add-stats-and-leaderboards-in-unity.md) avec une valeur pour au moins un lecteur et un formulaire un `LeaderboardQuery`. Vous pouvez consulter les articles liés Si vous ne connaissez pas déjà de la connexion à un utilisateur ou devez initialiser une statistique pour votre classement. Une fois que vous avez une statistique initialisée le moyen le plus simple à associer à votre script leaderboard consiste à inclure l’un des statistique prefabs : `IntegerStat`, `DoubleStat`, ou `StringStat` comme une variable publique. Votre stat doivent avoir la propriété ID son configurée sur le minimum comme c’est ce que nous allons utiliser pour le **statName** paramètre lorsque nous appelons de nos données de classement. Enfin, vous devez pour former un `LeaderboardQuery` objet.
Un `LeaderboardQuery` a quelques attributs qui peuvent être définies et qui aura un effet sur les données retournées :

- **SkipResultToRank**: si la valeur, cette variable uint détermine ce que les données de classement de classement démarrera lors du retour. Classement commence au rang 1.
- **SkipResultToMe**: si défini sur true cette valeur booléenne entraîne les données de classement retournées à partir du `XboxLiveUser` utilisé dans le `GetLeaderboard()` appeler.
- **Commande**: Énumérations du type `Microsoft.Xbox.Services.Leaderboard.SortOrder` ont deux valeurs possibles, croissant ou décroissant. Définition de cette variable pour votre requête détermine l’ordre de tri de votre classement.
- **MaxItems**: Cette uint détermine le nombre maximal de lignes à retourner par l’appel à `GetLeaderboard()` ou `GetSocialLeaderboard()`.

Formation de votre leaderboardQuery peut ressembler à ce qui suit :

```csharp
using Microsoft.Xbox.Services.Leaderboard;

LeaderboardQuery query = new LeaderboardQuery
        {
            SkipResultToRank = 100,
            MaxItems = 5
        };
```

Cette requête retourne cinq lignes de la partie du classement, en commençant à la 100th le classement individuels.

> [!WARNING]
> Paramètre SkipResultToRank supérieure au nombre de lecteurs contenus dans le classement entraîne le classement des données à retourner avec des lignes nulles.

Maintenant que nous avons tous les éléments ensemble, nous pouvons appeler le `GetLeaderboard(XboxLiveUser user, string statName, LeaderboardQuery query)` (fonction).

Le `GetSocialLeaderboard(XboxLiveUSer user, string statName, string socialGroup, LeaderboardQuery query)` fonction a un paramètre supplémentaire appelé socialGroup. Cette chaîne agit comme un filtre de relation sur les données retournées. Les valeurs acceptables pour socialGroup sont les suivantes :

- « all » : Ceci renverra un classement filtré à vos amis de le XboxLiveUser
- « favorite » : Ceci renverra un classement filtré à amis Favoris de le XboxLiveUser

Vous pouvez utiliser la `LeaderboardTypes` enum dans le `Microsoft.Xbox.Services.Client` espace de noms pour votre socialGroup de tableaux de résultats de l’étiquette, puis utilisez le `LeaderboardHelper` fonction de la classe `GetSocialGroupFromLeaderboardType(LeaderboardTypes leaderboardType)` pour extraire la chaîne appropriée.

> [!NOTE]
> En passant une chaîne vide pour le paramètre socialGroup retourne les mêmes résultats que si vous appelez le `GetLeaderboard()` (fonction). Vous recevrez un non filtrées *global* classement qui affiche tout le monde avec un classement dans le classement qui a joué le jeu.

```csharp
using Microsoft.Xbox.Services.Leaderboard;
using Microsoft.Xbox.Services.Statistics.Manager;
using Microsoft.Xbox.Services;

public void LoadLeaderboard()
{

    if (this.stat == null)
    {
        // TO DO: Display "Stat not specified" error message!
        return;
    }

    if (this.xboxLiveUser == null)
    {
        if (SignInManager.Instance.GetCurrentNumberOfPlayers() > 0)
        {
            this.xboxLiveUser = SignInManager.Instance.GetPlayer(1);
            this.isLocalUserAdded = true;
        }
        else
        {
            // TO DO: Display "No user signed-in" error message!
            return;
        }
    }

    LeaderboardQuery query = new LeaderboardQuery
    {
        MaxItems = 5,
    };

    XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, this.stat.ID, otherQuery);
}

```

Maintenant vous avez peut-être remarqué que nos deux fonctions lors de la récupération du classement retournent void et par conséquent, ne retournent pas les données de classement que nous recherchons. Nous avons réellement récupère les données de classement dans une fonction d’événement décrite dans la section suivante.

## <a name="receive-the-leaderboard-data"></a>Recevoir les données de classement

Afin de récupérer les données de classement que vous devrez ajouter une fonction à l’écoute pour le `StatsManagerComponent` instance pour votre titre. Vous devez ajouter la ligne suivante de code pour le `Awake()` fonction de votre code : `StatsManagerComponent.Instance.GetLeaderboardCompleted += this.MyGetLeaderboardCompletedFunction`. Le `StatsManagerComponent` dans le `Microsoft.Xbox.Services.Client` espace de noms écoute les événements de saisie semi-automatique de classement. En exécutant cette ligne de code, vous allez ajouter une fonction à la liste des fonctions d’être appelées lorsqu’un événement d’achèvement de classement se produit. Dans cet exemple que fonction est appelée MyGetLeaderBoardCompletedFunction, vous pouvez nommer la fonction que vous le souhaitez dans votre propre script. La fonction « MyGetLeaderboardCompletedFunction » vous devrez prendre deux paramètres, un objet qui représente l’expéditeur, et un `Microsoft.Xbox.Services.Client.StatEventArgs` paramètre. L’interpréteur de commandes de votre fonction peut ressembler à ceci :

```csharp
using Microsoft.Xbox.Services.Client;
using Microsoft.Xbox.Services.Statistics.Manager;

private void GetLeaderboardCompleted(object sender, StatEventArgs statArgs)
    {
        //Do Something;
    }
```

La première chose que cette fonction doit faire est de rechercher les erreurs qui se trouve dans le `StatEventArgs` statArgs de paramètre. StatArgs contient un `StatisticEvent` EventData qui contient les données d’erreur. Si une erreur s’est produite lors de la récupération des données de classement vous le trouverez dans `statArgs.EventData.ErrorCode` ou `statArgs.EventData.ErrorMessage`. Si aucune erreur s’est produite le code d’erreur est 0 et le message d’erreur sera une chaîne vide « ». Vous pouvez ajouter un cas simple instruction au code précédent pour rechercher les erreurs.

```csharp
using Microsoft.Xbox.Services.Client;
using Microsoft.Xbox.Services.Statistics.Manager;

private void GetLeaderboardCompleted(object sender, StatEventArgs statArgs)
    {
        if (statArgs.EventData.ErrorCode != 0) //if there is an error
        {
            // TO DO: Display error message
            return;
        }
    }
```

Après avoir confirmé qu’il n’y a aucune erreur, stocker les résultats de la demande de classement qui se trouvent dans `statArgs.EventData.EventArgs.Result`. `Result` est un `LeaderBoardResult` objet qui contient les données que vous avez besoin pour remplir votre classement. Dans notre exemple de code nous sera extraire ces données et les envoyer vers une autre fonction appelée `LoadResult()`.

```csharp
using Microsoft.Xbox.Services.Client;
using Microsoft.Xbox.Services.Statistics.Manager;

private void GetLeaderboardCompleted(object sender, StatEventArgs statArgs)
    {
        if (statArgs.EventData.ErrorCode != 0)
        {
            // TO DO: Display error message
            return;
        }

        LeaderboardResultEventArgs leaderboardArgs = (LeaderboardResultEventArgs)statArgs.EventData.EventArgs;
        this.LoadResult(leaderboardArgs.Result);
    }
```

Le `LeaderboardResult` résultat que nous envoyons à le `LoadResult()` fonction n’a pas toutes les données que nous avons besoin pour lire les données de classement a été renvoyées comme ainsi que pour effectuer des appels supplémentaires pour récupérer les rangs pas encore retournés par l’appel d’origine. Vous souhaitez stocker les résultats dans une variable de classe pour votre script leaderboard comme suit :

```csharp
using Microsoft.Xbox.Services.Leaderboard;

void LoadResult(LeaderboardResult result)
    {
        this.leaderboardData = result;
    }
```

Ceci est important, car le `LeaderboardResult` contient un `HasNext` propriété qui détermine s’il y a une section ultérieure du classement qui peut être récupérée, le résultat contient également un nombre total de lignes qui composent le classement. Ces propriétés seront importantes à la navigation dans votre classement. Pour extraire les données à partir de votre `LeaderBoardResult` simplement implémenter un pour la boucle à l’aide la `LeaderboardResults` liste des `LeaderboardRow` appelée `Rows`. Dans notre exemple de code nous allons simplement pour concaténer les valeurs dans chaque `LeaderboardRow` en une chaîne à afficher.


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

Dans notre exemple, nous avons utilisé les propriétés de classement, Gamertag et valeurs de LeaderBoardResult pour remplir notre chaînes, ainsi que la propriété DisplayName de la statistique associée avec le classement.

Je suis sûr que vous serez en mesure de faire quelque chose de plus créatif avec toutes ces données de classement.

## <a name="navigating-the-leaderboard-data"></a>Navigation dans les données de classement

Dans les cas courants, vous ne chargera pas chaque rang unique dans votre classement à la fois et devrez être en mesure de parcourir les rangs pour afficher les différentes sections du classement de l’utilisateur. Supposons que vous disposez d’un classement avec 100 joueurs classés. Dans votre appel initial à `GetLeaderboard()` ou `GetSocialLeaderboard` vous récupérer 10 `LeaderboardRows` et de les afficher pour le lecteur. Le joueur souhaiterez peut-être plus que les dix meilleurs joueurs. Pour obtenir le jeu suivant de dix utilisateurs le plus simple consiste à déterminer ou non le `LeaderboardResult` vous avez stocké à partir de votre dernière requête a plusieurs lignes à récupérer et à appeler ensuite `GetLeaderboard()` avec la requête suivante de ce LeaderboardResult. Pour utiliser un LeaderBoardResult *nextQuery* vous devez utiliser la fonction `LeaderBoardResult.GetNextQuery()`. Le code pour récupérer l’ensemble suivant de rangs se présenterait comme suit.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

void GetNextLeaders()
    {
        if(this.leaderboardData.HasNext)
        {
            LeaderboardQuery query = this.leaderboardData.GetNextQuery();
            XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, this.stat.ID, query);
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
        SkipResultToRank = targetRank,
        MaxItems = this.maxItems
    };

    XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, this.stat.ID, query); // call the GetLeaderboard() function with the new query
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
            SkipResultToMe = true,
            MaxItems = this.maxItems
        };

        XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, this.stat.ID, query); // call the GetLeaderboard() function with the new query
    }
```

Si vous souhaitez vous plonger dans un exemple plus détaillé de classement vous pouvez toujours lire le script Leaderboard.cs dans le dossier XboxLive Plugin sous ressources >> XboxLive >> Scripts >> Leaderboard.cs.