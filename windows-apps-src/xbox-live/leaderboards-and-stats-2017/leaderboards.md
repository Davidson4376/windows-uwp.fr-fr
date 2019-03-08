---
title: Classements
description: Découvrez comment utiliser les classements de Xbox Live pour comparer des joueurs.
ms.assetid: 132604f9-6107-4479-9246-f8f497978db7
ms.date: 09/28/2018
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8fd7e30b99418fda614a888d9269548cdc57a88a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662024"
---
# <a name="leaderboards"></a>Classements

## <a name="introduction"></a>Introduction

Comme décrit dans [vue d’ensemble de la plate-forme de données](../data-platform/data-platform.md), classements sont un excellent moyen pour encourager une concurrence entre vos joueurs et motiver tenter de battre leur meilleur score précédente, ainsi que de leurs amis les joueurs.

Tableaux de résultats pour [statistiques proposées](stats2017.md#configured-stats-and-featured-leaderboards) sont toujours affichés dans le Hub de jeu d’un titre et parfois comme une partie de l’interface utilisateur pour un titre lorsqu’elle est épinglée à la page d’accueil. Vous pouvez également utiliser vos statistiques proposées configuré pour créer des tableaux de résultats à l’intérieur de votre titre.

## <a name="choosing-good-leaderboards"></a>Choix du bon Leaderboards

Comme indiqué dans [statistiques](player-stats.md), un classement correspond à une statistique qui vous avez défini.  Vous devez choisir les tableaux de résultats qui correspondent à une réalisation un joueur peut fonctionner vers l’amélioration.

Par exemple, meilleur temps de présentation dans un jeu de course est un bon classement, étant donné que les joueurs souhaiteront travailler vers l’amélioration de leur meilleur temps de présentation.  Rapport de Kill/mort pour subjectifs, ou taille maximale de liste déroulante dans un jeu de lutte contre sont d’autres exemples.

## <a name="when-to-display-leaderboards"></a>Quand afficher des tableaux de résultats

Vous avez la possibilité d’afficher des tableaux de résultats à tout moment dans votre titre.  Vous devez choisir un temps lorsqu’un classement n’interfère pas avec le jeu ou le flux de votre titre.  Entre arrondit et une fois que les correspondances sont les deux bons moments.

## <a name="how-to-display-leaderboards"></a>Comment afficher des tableaux de résultats

Il existe de nombreuses options pour l’affichage des classements fournis dans le Kit de développement Xbox Live.  Si vous utilisez Unity avec le programme Xbox Live Creators, vous pouvez commencer en utilisant un classement Prefab pour afficher vos données de classement.  Consultez le [configurer Xbox Live dans Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) article pour plus de détails.

Si vous codez directement sur le Kit de développement Xbox Live, puis poursuivez votre lecture pour en savoir plus sur les API que vous pouvez utiliser.

## <a name="programming-guide"></a>Guide de programmation

Il existe plusieurs classement APIs, vous pouvez utiliser pour obtenir l’état actuel d’un classement.  Toutes les API sont asynchrones et ne bloquent pas.  Vous effectueriez une demande pour obtenir des données de classement et continuer le traitement de votre jeu habituel.  Lorsque le tableau de résultats est retournés à partir du service, vous pouvez afficher les résultats au moment opportun.

Vous devez demander les données de classement à partir du service, légèrement avance lorsque vous souhaitez afficher, afin que les lecteurs ne sont pas bloqués en attente pour le classement à afficher.

## <a name="leaderboards-2013-apis"></a>API de 2013 Leaderboards

Vous pouvez voir le `leaderboard_service` espace de noms pour les API de classement de toutes les statistiques 2013.

<table>

<tr>
<td>API C++</td><td>Description</td>
</tr>

<tr>
<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard(
        const string_t& scid,
        const string_t& name
        );
```

</td>

<td>Version de base de l’API.  Cela retourne les valeurs de classement pour le classement donné, à partir de l’acteur en haut du classement.</td>

</tr>

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardAsync (
        _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName
        ) 
```

</td>

<td>WinRT C# Code - obtenir un classement pour un classement unique, étant donné un ID de configuration de service et un nom de classement.</td>

</tr>

<tr>
<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard(
    _In_ const string_t& scid,
    _In_ const string_t& name,
    _In_ uint32_t skipToRank,
    _In_ uint32_t maxItems = 0
    );

```

</td>

<td>Cette API fournit une certaine flexibilité de plus, vous pouvez spécifier le rang (position) que vous souhaitez afficher, ainsi que la valeur maximale d’éléments à retourner.  Par exemple, vous utiliseriez cette API si vous souhaitez afficher le classement en démarrant à la position 1000.</td>

</tr>

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardAsync (
         _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName,
         _In_ uint32 skipToRank,
         _In_ uint32 maxItems
        ) 
```

</td>

<td>WinRT C# code - obtenir une page de tableau de résultats pour un classement unique donner un ID de configuration de service et un nom de classement, le classement des résultats démarre au rang de « skipToRank ».</td>

</tr>

<tr>

<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard_skip_to_xuid(
    _In_ const string_t& scid,
    _In_ const string_t& name,
    _In_ const string_t& skipToXuid = string_t(),
    _In_ uint32_t maxItems = 0
    );

```

</td>

<td>

Utilisez-le si vous souhaitez ignorer le classement d’un utilisateur spécifique.  Un `XUID` est un identificateur unique pour chaque utilisateur de la Xbox.  Vous pouvez obtenir de l’utilisateur connecté, ou l’un de ses amis et passez dans cette fonction.

</td>

</tr>

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardWithSkipToUserAsync (
         _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName,
         _In_opt_ Platform::String^ skipToXboxUserId,
         _In_ uint32 maxItems
        )
```

</td>

<td>WinRT C# code - obtenir un classement en commençant à un lecteur spécifié, quel que soit le rang ou le score, classés par rang de centile du lecteur du lecteur</td>

</tr>

</table>

## <a name="2013-c-example"></a>Exemple C++ de 2013

Lors de l’utilisation de la couche API C++, vous pouvez ensuite définir un rappel à appeler une fois que le tableau de résultats est retournés à partir du service.  Nous allons montrer un exemple ci-dessous.

Si vous n’êtes pas familiarisé avec la `pplx::task` soit retournée à partir de ces API, ceci est un objet de tâche asynchrone à partir de la bibliothèque de programmation parallèle Microsoft (PPL).  Vous trouverez plus d’informations en [ https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks ](https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks).

La section ci-dessous illustre comment vous pouvez récupérer le classement des résultats et les utiliser.

### <a name="1-create-an-async-task-to-retrieve-leaderboard-results"></a>1. Créer une tâche asynchrone pour récupérer le classement des résultats

La première étape consiste à appeler le service de classements pour récupérer les résultats pour un classement particulier.

```cpp
pplx::task<xbox_live_result<leaderboard_result>> asyncTask;
auto& leaderboardService = xboxLiveContext->leaderboard_service();

asyncTask = leaderboardService.get_leaderboard(m_liveResources->GetServiceConfigId(), LeaderboardIdEnemyDefeats);
```

### <a name="2-setup-a-callback"></a>2. Programme d’installation un rappel

Vous pouvez configurer un [tâche de continuation](https://msdn.microsoft.com/en-us/library/dd492427(v=vs.110).aspx#continuations) à être appelée une fois que le tableau de résultats est retournés.  Procéder comme suit ci-dessous.

```cpp
asyncTask.then([this](xbox::services::xbox_live_result<xbox::services::leaderboard::leaderboard_result> result)
{
    // Handle result here
});
```

Cette tâche de continuation est appelée dans le contexte de l’objet à l’origine a appelée et reçoit le ```leaderboard_result``` qui peuvent être affiché dans une manière qui correspond le mieux à votre titre.


### <a name="3-display-leaderboard"></a>3. Afficher le classement

Les données de classement sont contenues dans ```leaderboard_result``` et les champs sont explicites.  Voir ci-dessous pour obtenir un exemple.

```cpp
auto leaderboard = result.payload();

for (const xbox::services::leaderboard::leaderboard_row& row : leaderboard.rows())
{
    string_t colValues;
    for (auto columnValue : row.column_values())
    {
        colValues = colValues + L" ";
        colValues = colValues + columnValue;
    }
    m_console->Format(L"%18s %8d %14f %10s\n", row.gamertag().c_str(), row.rank(), row.percentile(), colValues.c_str());
}

```

## <a name="2013-winrt-c-example"></a>2013 WinRT C# exemple

Lorsque vous utilisez le WinRT C# couche vous serez inutile d’effectuer un rappel séparée de tâches et sera simplement besoin d’utiliser le `await` mot clé lors de l’appel du service de classement.

### <a name="1-access-the-leaderboardservice"></a>1. Accéder à la LeaderboardService

Le `LeaderboardService` peuvent être récupérées à partir de la `XboxLiveContext` créé lors de la connexion d’un utilisateur pour le jeu, vous en aurez besoin à appeler pour le classement des données.

```csharp
XboxLiveContext xboxLiveContext = idManager.xboxLiveContext;
LeaderboardService boardService = xboxLiveContext.LeaderboardService;
```

### <a name="2-call-the-leaderboardservice"></a>2. Appelez le LeaderboardService

```csharp
LeaderboardResult boardResult = await boardService.GetLeaderboardAsync(
     xboxLiveConfig.ServiceConfigurationId,
     leaderboardName
     );
```

### <a name="3-retrieve-leaderboard-data"></a>3. Récupérer des données de classement

`GetLeaderboardAsync()` Retourne un `LeaderboardResult` qui contient les statistiques de remplir le classement nommé.

`LeaderboardResult` possède plusieurs fonctions et propriétés afin de faciliter la lecture des données de classement.

|Propriété  |Description  |
|---------|---------|
|IAsyncOperation publique<LeaderboardResult> GetNextAsync (uint maxItems) ;     |Récupère l’ensemble suivant de rangs jusqu’au nombre du paramètre maxItems. Il s’agit essentiellement d’un autre appel à `GetLeaderboard()`         |
|public LeaderboardQuery GetNextQuery();     |Récupère le LeaderboardQuery qui peut être utilisé pour effectuer l’appel de classement pour récupérer l’ensemble suivant de données.         |
|public bool HasNext {get ;}    |Indique s’il y a plus le classement des lignes à récupérer         |
|public IReadOnlyList<LeaderboardRow> Rows { get; }     | Lignes contenant des données de classement par rang        |
|IReadOnlyList publique<LeaderboardColumn> colonnes {get ;}     | La liste des colonnes qui composent le classement        |
|public uint TotalRowCount { get; }     | Montant total de lignes dans le classement        |
|chaîne publique DisplayName {get ;}     | Nom à afficher pour le classement       |

Le classement des données fournira une page à la fois. Vous pouvez parcourir le `LeaderboardResult` lignes et colonnes pour récupérer les données.  
Utilisez le `HasNext` booléenne et `GetNextAsync()` fonction pour récupérer les autres pages de données de classement.

```csharp
if (boardResult != null)
{
    foreach (LeaderboardRow row in boardResult.Rows)
    {
        Debug.Write(string.Format("Rank: {0} | Gamertag: {1} | {2}\n", row.Rank, row.Gamertag, row.Values.ToString()));
    }
}
```

## <a name="leaderboard-2017"></a>Classement 2017

Pour effectuer des appels au service Stats 2017 classement que vous utiliserez le `StatisticManager` API classement au lieu du `LeaderboardService` le classement des API.  

`xbox::services::stats:manager::stats_manager::get_leaderboard`  

```cpp
xbox_live_result< void >  get_leaderboard (
     _In_ const xbox_live_user_t &user,
     _In_ const string_t &statName,
     _In_ leaderboard::leaderboard_query query
     ) 
```  

`xbox::services::stats:manager::stats_manager::get_leaderboard`  

```cpp
xbox_live_result< void >  get_social_leaderboard (_In_ const xbox_live_user_t &user,
     _In_ const string_t &statName,
     _In_ const string_t &socialGroup,
     _In_ leaderboard::leaderboard_query query
)
```  

`Microsoft.Xbox.Services.Statistics.Manager.StatisticManager.GetLeaderboard`  

```csharp
public void GetLeaderboard(
    XboxLiveUser user,
    string statName,
    LeaderboardQuery query
    )
```  

`Microsoft.Xbox.Services.Statistics.Manager.StatisticManager.GetSocialLeaderboard`  

```csharp
public void GetSocialLeaderboard(
    XboxLiveUser user,
    string statName,
    string socialGroup,
    LeaderboardQuery query
    )
```  

## <a name="2017-c-example"></a>Exemple C++ de 2017

### <a name="1-get-a-singleton-instance-of-the-statsmanager"></a>1. Obtenir une Instance Singleton de la stats_manager

Avant de pouvoir appeler le `stats_manager` fonctions, vous devez définir une variable à son Instance de Singleton.

```csharp
m_statsManager = stats_manager::get_singleton_instance();
```

### <a name="2-create-a-leaderboardquery"></a>2. Créer un LeaderboardQuery

Le `leaderboard_query` déterminent la quantité, l’ordre, et le point de départ des données retournées par l’appel de classement.

Un `leaderboard_query` a quelques attributs qui peuvent être définies et qui aura un effet sur les données retournées :

|Propriété |Description  |
|---------|---------|
|m_skipResultToRank     |Cette variable uint déterminent ce que les données de classement de classement démarrera lors du retour. Classement commence au rang 1.         |
|m_skipResultToMe     |Si défini sur true cette valeur booléenne entraîne les données de classement retournées à partir du `XboxLiveUser` utilisé dans le `get_leaderboard()` appeler.  |
|m_order     |Énumérations du type `xbox::services::leaderboard::sort_order` ont deux valeurs possibles, croissant ou décroissant. Définition de cette variable pour votre requête détermine l’ordre de tri de votre classement.        |
|m_maxItems     |Cette uint détermine le nombre maximal de lignes à retourner par l’appel à `get_leaderboard` ou `get_social_leaderboard()`.         |

`leaderboard_query` a plusieurs fonction set utilisables pour affecter la valeur à ces propriétés. Le code suivant vous montre comment configurer votre `leaderboard_query`

```cpp
leaderboard::leaderboard_query leaderboardQuery;
leaderboardQuery.set_skip_result_to_rank(10);
leaderboardQuery.set_max_items(10);
leaderboardQuery.set_order(sort_order::descending);
```

Cette requête retournerait dix lignes du classement en commençant à la 100th le classement individuels.

> [!WARNING]
> Paramètre SkipResultToRank supérieure au nombre de lecteurs contenus dans le classement entraîne le classement des données à retourner avec des lignes nulles.

### <a name="3-call-getleaderboard"></a>3. Appel get_leaderboard

```cpp
leaderboard::leaderboard_query leaderboardQuery;
m_statsManager->get_leaderboard(user, statName, leaderboardQuery);
```

> [!IMPORTANT]
> Le `statName` utilisé dans le `GetLeaderboard()` appel doit être le même que le nom d’un état configuré pour votre titre dans [partenaires](https://partner.microsoft.com/dashboard), qui respecte la casse.

### <a name="4-read-the-leaderboard-data"></a>4. Lire les données de classement

Pour pouvoir lire les données de classement que vous devez appeler la `stats_manager::do_work()` fonction qui retourne une liste de `stat_event` valeurs. Le classement des données seront trouvera dans un `stat_event` du type `stat_event_type::get_leaderboard_complete`. Lorsque vous rencontrez un événement de ce type dans la liste des `stat_event`s, vous pouvez consulter le `leaderboard_result` contenus dans le `stat_event` pour accéder aux données.

Exemple `do_work()` Gestionnaire

```cpp
void Sample::UpdateStatsManager()
{
    // Process events from the stats manager
    // This should be called each frame update

    auto statsEvents = m_statsManager->do_work();
    std::wstring text;

    for (const auto& evt : statsEvents)
    {
        switch (evt.event_type())
        {
            case stat_event_type::local_user_added: 
                text = L"local_user_added"; 
                break;

            case stat_event_type::local_user_removed: 
                text = L"local_user_removed"; 
                break;

            case stat_event_type::stat_update_complete: 
                text = L"stat_update_complete"; 
                break;

            case stat_event_type::get_leaderboard_complete: //leaderboard data is read here
                text = L"get_leaderboard_complete";
                auto getLeaderboardCompleteArgs = std::dynamic_pointer_cast<leaderboard_result_event_args>(evt.event_args());
                ProcessLeaderboards(evt.local_user(), getLeaderboardCompleteArgs->result());
                break;
        }

        stringstream_t source;
        source << _T("StatsManager event: ");
        source << text;
        source << _T(".");
        m_console->WriteLine(source.str().c_str());
    }
}
```

Lire les données de classement à partir du résultat de classement  

```cpp
void Sample::PrintLeaderboard(const xbox::services::leaderboard::leaderboard_result& leaderboard)
{
    if (leaderboard.rows().size() > 0)
    {
        m_console->Format(L"%16s %6s %12s %12s\n", L"Gamertag", L"Rank", L"Percentile", L"Values");
    }

    for (const xbox::services::leaderboard::leaderboard_row& row : leaderboard.rows())
    {
        string_t colValues;
        for (auto columnValue : row.column_values())
        {
            colValues = colValues + L" ";
            colValues = colValues + columnValue;
        }
        m_console->Format(L"%16s %6d %12f %12s\n", row.gamertag().c_str(), row.rank(), row.percentile(), colValues.c_str());
    }
}
```  

Extraction de nouvelles pages de données à partir de la partie du classement.  

```cpp
void Sample::ProcessLeaderboards(
    _In_ std::shared_ptr<xbox::services::system::xbox_live_user> user,
    _In_ xbox::services::xbox_live_result<xbox::services::leaderboard::leaderboard_result> result
    )
{
    if (!result.err())
    {
        auto leaderboardResult = result.payload();
        PrintLeaderboard(leaderboardResult);

        // Keep processing if there is more data in the leaderboard
        if (leaderboardResult.has_next())
        {
            if (!leaderboardResult.get_next_query().err())
            {               
                auto lbQuery = leaderboardResult.get_next_query().payload();
                if (lbQuery.social_group().empty())
                {
                    m_statsManager->get_leaderboard(user, lbQuery.stat_name(), lbQuery);
                }
                else
                {
                    m_statsManager->get_social_leaderboard(user, lbQuery.stat_name(), lbQuery.social_group(), lbQuery);
                }
            }
        }
    }
}
```  

## <a name="2017-winrt-c-example"></a>2017 WinRT C# exemple

### <a name="1-get-a-singleton-instance-of-the-statisticmanager"></a>1. Obtenir une instance singleton de la StatisticManager

Avant de pouvoir appeler le `StatisticManager` fonctions, vous devez définir une variable à son Instance de Singleton.

```csharp
statManager = StatisticManager.SingletonInstance;
```

### <a name="2-create-a-leaderboardquery"></a>2. Créer un LeaderboardQuery

Le `LeaderboardQuery` déterminent la quantité, l’ordre, et le point de départ des données retournées par l’appel de classement.  

```csharp
public sealed class LeaderboardQuery : __ILeaderboardQueryPublicNonVirtuals
    {
        [Overload("CreateInstance1")]
        public LeaderboardQuery();

        public bool HasNext { get; }
        public string SocialGroup { get; }
        public string StatName { get; }
        public SortOrder Order { get; set; }
        public uint MaxItems { get; set; }
        public uint SkipResultToRank { get; set; }
        public bool SkipResultToMe { get; set; }
    }
```

Un `LeaderboardQuery` a quelques attributs qui peuvent être définies et qui aura un effet sur les données retournées :

|Propriété |Description  |
|---------|---------|
|SkipResultToRank     |Cette variable uint déterminent ce que les données de classement de classement démarrera lors du retour. Classement commence au rang 1.         |
|SkipResultToMe     |Si défini sur true cette valeur booléenne entraîne les données de classement retournées à partir du `XboxLiveUser` utilisé dans le `GetLeaderboard()` appeler.  |
|Ordre     |Énumérations du type `Microsoft.Xbox.Services.Leaderboard.SortOrder` ont deux valeurs possibles, croissant ou décroissant. Définition de cette variable pour votre requête détermine l’ordre de tri de votre classement.        |
|maxItems     |Cette uint détermine le nombre maximal de lignes à retourner par l’appel à `GetLeaderboard()` ou `GetSocialLeaderboard()`.         |

Formation de votre `LeaderboardQuery` peut ressembler à ce qui suit :

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

### <a name="3-call-getleaderboard"></a>3. Call GetLeaderboard()

Vous pouvez maintenant appeler `GetLeaderboard()` avec votre `XboxLiveUser`, le nom de votre statistique et un `LeaderboardQuery`.

```csharp
statManager.GetLeaderboard(xboxLiveUser, statName, leaderboardQuery);
```

> [!IMPORTANT]
> Le `statName` utilisé dans le `GetLeaderboard()` appel doit être le même que le nom d’un état configuré pour votre titre dans [partenaires](https://partner.microsoft.com/dashboard), qui respecte la casse.

### <a name="4-read-leaderboard-data"></a>4. Classement de lecture des données

Pour pouvoir lire les données de classement que vous devez appeler la `StatisticManager.DoWork()` fonction qui retourne une liste de `StatisticEvent` valeurs. Le classement des données seront trouvera dans un `StatisticEvent` du type `GetLeaderboardComplete`. Lorsque vous rencontrez un événement de ce type dans la liste des `StatisticEvent`s, vous pouvez consulter le `LeaderboardResult` contenus dans le `StatisticEvent` pour accéder aux données.

```csharp
IReadOnlyList<StatisticEvent> statEvents = statManager.DoWork(); //In practice this should be called every update frame

foreach(StatisticEvent statEvent in statEvents)
{
    if(statEvent.EventType == StatisticEventType.GetLeaderboardComplete
        && statEvent.ErrorCode == 0)
    {
        LeaderboardResultEventArgs leaderArgs = (LeaderboardResultEventArgs)statEvent.EventArgs;
        LeaderboardResult leaderboardResult = leaderArgs.Result;
        foreach(LeaderboardRow leaderRow in leaderboardResult.Rows)
        {
            Debug.WriteLine(string.Format("Rank: {0} | Gamertag: {1} | {2}:{3}\n\n", leaderRow.Rank, leaderRow.Gamertag, "test", leaderRow.Values[0]));
        }
    }
}
```

Dans votre code de titre `StatisticManager.DoWork()` doit être utilisé pour gérer tous les événements du Gestionnaire de statistiques entrants et pas seulement pour les tableaux de résultats. 

> [!NOTE]
> Afin de récupérer le `LeaderboardResultEventArgs` vous devrez effectuer un cast du `StatisticEvent.EventArgs` comme un `LeaderboardResultEventArgs` variable.

### <a name="5-retrieve-more-leaderboard-data"></a>5. Plus de données de classement

Afin de récupérer les autres pages de données de classement vous devrez utiliser le `LeaderboardResult.HasNext` propriété et la `LeaderboardResult.GetNextQuery()` fonction pour récupérer le `LeaderboardQuery` qui s’affiche la page suivante de données.

```csharp
while (leaderboardResult.HasNext)
{
    leaderboardQuery = leaderboardResult.GetNextQuery();
    statManager.GetLeaderboard(xboxLiveUser, leaderboardQuery.StatName, leaderboardQuery)
    // StatisticManager.DoWork() is called
    // Leaderboard results are read
}
```