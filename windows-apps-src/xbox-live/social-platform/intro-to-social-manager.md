---
title: Présentation de sociale Manager
description: En savoir plus sur l’API du Gestionnaire de sociale Xbox Live à effectuer le suivi de vos amis en ligne.
ms.assetid: d4c6d5aa-e18c-4d59-91f8-63077116eda3
ms.date: 03/26/2018
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5dff3dcfd79fe43ff8af1513a4358bd0ff98b8d1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57609004"
---
# <a name="introduction-to-social-manager"></a>Présentation de sociale Manager

## <a name="description"></a>Description

Xbox Live fournit un graphique social riches qui permettent de titres pour différents scénarios.

À l’aide de l’API sociales dans l’API Xbox Live (XSAPI) pour obtenir et mettre à jour les informations sur un graphique social est complexe, et conserver ces informations à jour peut être complexe.  Ne pas le faire correctement peut entraîner des problèmes de performances, des données obsolètes ou limitées en raison de l’appel des services sociaux Xbox Live plus fréquemment que nécessaire.

Le Gestionnaire de sociale résout ce problème en :

* Création d’une API simple pour appeler
* Création des informations à jour à l’aide de service de l’activité en temps réel dans les coulisses
* Les développeurs peuvent appeler l’API du Gestionnaire de sociale synchrone sans toute souche supplémentaire sur le service

Le Gestionnaire de sociale masque la complexité de la gestion avec plusieurs abonnements de l’agrégation RTA et l’actualisation des données pour les utilisateurs et permet aux développeurs de facilement obtenir le graphique à jour qu'ils veulent créer des scénarios intéressants.

Pour examiner la mémoire sociale Manager et les performances caractéristiques jeter un œil [sociale le Gestionnaire de mémoire et la vue d’ensemble des performances](social-manager-memory-and-performance-overview.md)

## <a name="features"></a>Fonctionnalités

Le Gestionnaire de sociale fournit les fonctionnalités suivantes :

* API de réseau Social simplifiée
* Graphique social à jour
* Contrôler le niveau de détail des informations affichées
* Réduire le nombre d’appels aux services Xbox Live
  * Cela correspond directement à réduire la latence globale dans l’acquisition de données
* Thread-safe
* Maintenir efficacement à jour les données

## <a name="core-concepts"></a>Concepts fondamentaux

**Graphique social**: Un *graphique social* est créé pour un utilisateur local sur l’appareil. Cette opération crée une structure qui met à jour les informations sur tous les amis d’un utilisateurs.

> [!NOTE]
> Sur Windows ne peut contenir qu’un utilisateur local

**Utilisateur de la Xbox sociale**: Un *utilisateur social Xbox* est un ensemble complet de données sociales associés à un utilisateur à partir d’un groupe

**Groupe d’utilisateurs Xbox sociale**: Un groupe est une collection d’utilisateurs qui est utilisée pour les éléments tels que le remplissage de l’interface utilisateur. Il existe deux types de groupes

* **Filtrer les groupes**: Un groupe de filtres prend un utilisateur (appel) local *graphique social* et retourne un jeu fraîches de manière cohérente des utilisateurs en fonction des paramètres de filtre spécifié
* **Groupes d’utilisateurs**: Un groupe d’utilisateurs prend une liste d’utilisateurs et retourne une vue cohérente fraîches de ces utilisateurs. Ces utilisateurs peuvent être en dehors de la liste d’amis d’un utilisateur.

Afin de conserver un *groupe d’utilisateurs de réseaux sociaux* jusqu'à la date de la fonction `social_manager::do_work()` doit être appelé pour chaque trame.

## <a name="api-overview"></a>Présentation de l’API

Vous utiliserez fréquemment les classes principales suivantes :

### <a name="social-manager"></a>Gestionnaire sociale

* Nom de la classe API C++ : social_manager
* WinRT (C#) nom de la classe API : [SocialManager](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager.socialmanager?view=xboxlive-dotnet-2017.11.20171204.01)

Il s’agit d’une classe singleton qui peut être utilisée pour obtenir **groupes d’utilisateur social Xbox** les vues décrits ci-dessus.

Le Gestionnaire de sociale conserve xbox groupes d’utilisateurs de réseaux sociaux à jour et peut filtrer les groupes d’utilisateurs en présence ou en relation avec l’utilisateur.  Par exemple, un groupe d’utilisateur social xbox contenant tous les amis de l’utilisateur qui sont en ligne et de lire le titre en cours peut être créé.  Cela serait rester à jour comme amis démarrer ou arrêtent la lecture du titre.

### <a name="xbox-social-user-group"></a>Groupe d’utilisateur social Xbox

* Nom de la classe API C++ : xbox_social_user_group
* WinRT (C#) nom de la classe API : [XboxSocialUserGroup](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager.xboxsocialusergroup?view=xboxlive-dotnet-2017.11.20171204.01)

Un groupe d’utilisateurs qui répondent à certains critères, comme décrit ci-dessus. Groupes d’utilisateur social Xbox exposent le type d’un groupe qu’ils sont, les utilisateurs qui sont suivies ou ce que le filtre est défini sur et que l’utilisateur local auquel appartient le groupe.

Vous trouverez une description complète de l’API du Gestionnaire de sociale dans le [référence d’API de Xbox Live](https://aka.ms/xboxliveuwpdocs).
Vous pouvez également trouver les APIs WinRT dans le [Microsoft.Xbox.Services.Social.Manager.Namespace documentation](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager?view=xboxlive-dotnet-2017.11.20171204.01)

## <a name="usage"></a>Utilisation

### <a name="creating-a-social-user-group-from-filters"></a>Création d’un groupe d’utilisateurs social à partir de filtres

Dans ce scénario, vous voulez une liste d’utilisateurs à partir d’un filtre, tel que de toutes les personnes dont cet utilisateur est le suivant ou marqué comme favori.

#### <a name="source-example-using-the-c-api"></a>Exemple de code source à l’aide de l’API C++

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();

socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::preferred_color_level | social_manager_extra_detail_level::title_history_level
    );

auto socialUserGroup = socialManager->create_social_user_group_from_filters(
    xboxLiveContext->user(),
    presence_filter::all,
    relationship_filter::friends
    );

while(true)
{
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
}
```

#### <a name="source-example-using-the-c-api"></a>Exemple de la source à l’aide du C# API

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromFilters(
     xboxLiveContext.User,
     PresenceFilter.All,
     RelationshipFilter.Friends
     );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}

```

#### <a name="events-returned"></a>Événements renvoyés

`local_user_added`(C++) | `LocalUserAdded`(C#)-Déclenche quand le chargement du graphique social utilisateurs est terminé. Indique si des erreurs se sont produites pendant l’initialisation

`social_user_group_loaded`(C++) | `SocialUserGroupLoaded`(C#)-Déclenche la création d’un groupe d’utilisateurs de réseaux sociaux

`users_added_to_social_graph`(C++) | `UsersAddedToSocialGraph`(C#)-Déclenche quand les utilisateurs sont chargés dans

#### <a name="additional-details"></a>Informations supplémentaires

L’exemple ci-dessus montre comment initialiser le gestionnaire sociale pour un utilisateur, créer un groupe d’utilisateurs de réseaux sociaux pour cet utilisateur et maintenir à jour.

Les options de filtrage peuvent être consultées dans le `presence_filter` et `relationship_filter` enums

Dans la boucle du jeu, le `do_work` fonction met à jour toutes les vues avec la dernière capture instantanée des utilisateurs dans ce groupe.

Les utilisateurs dans la vue peuvent être obtenus en appelant le `xbox_social_user_group::get_users()` fonction qui retourne une liste de `xbox_social_user` objets.  Le `xbox_social_user` contient les informations de réseaux sociaux tels que gamertag, gamerpic uri, etc.

### <a name="create-and-update-a-social-user-group-from-list"></a>Créer et mettre à jour un groupe d’utilisateurs social à partir de la liste

Dans ce scénario, vous souhaitez que les informations de réseau sociales d’une liste d’utilisateurs tels que des utilisateurs dans une session multijoueur.

#### <a name="source-example-using-the-c-api"></a>Exemple de code source à l’aide de l’API C++

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();

socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::preferred_color_level | social_manager_extra_detail_level::title_history_level
    );

auto socialUserGroup = socialManager->create_social_user_group_from_list(
    xboxLiveContext->user(),
    userList  // this is a std::vector<string_t> (list of xuids)
    );

while(true)
{
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
}
```

#### <a name="source-example-using-the-c-api"></a>Exemple de la source à l’aide du C# API

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromList(
     xboxLiveContext.User,
     userList //this is a IReadOnlyList<string> (list of xbox user ids a.k.a. xuids)
    );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}
```

#### <a name="events-returned"></a>Événements renvoyés

`local_user_added`(C++) | `LocalUserAdded`(C#)-Déclenche quand le chargement du graphique social utilisateurs est terminé. Indique si des erreurs se sont produites pendant l’initialisation

`social_user_group_loaded`(C++) | `SocialUserGroupLoaded`(C#)-Déclenche la création d’un groupe d’utilisateurs de réseaux sociaux

`users_added_to_social_graph`(C++) | `UsersAddedToSocialGraph`(C#)-Déclenche quand les utilisateurs sont chargés dans

### <a name="updating-social-user-group-from-list"></a>Groupe d’utilisateurs sociale à partir de la liste de la mise à jour

Vous pouvez également modifier la liste des utilisateurs suivis dans le groupe d’utilisateurs de réseaux sociaux en appelant update_social_user_group()

#### <a name="source-example-using-the-c-api"></a>Exemple de code source à l’aide de l’API C++

```cpp
//#include "Social.h"

socialManager->update_social_user_group(
    xboxSocialUserGroup,
    newUserList    // std::vector<string_t> (list of xuids)
    );

    while(true)
    {
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
    }
```

#### <a name="source-example-using-the-c-api"></a>Exemple de la source à l’aide du C# API

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.UpdateSocialUserGroup(
     xboxSocialUserGroup,
     newUserList //IReadOnlyList<string> (list of xbox user ids a.k.a. xuids)
     );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}
```

#### <a name="events-returned"></a>Événements renvoyés

`social_user_group_updated`(C++) | `SocialUserGroupUpdated`(C#)-Déclenche lors de la mise à jour du groupe utilisateur social est terminée.

`users_added_to_social_graph` | `UsersAddedToSocialGraph`(C#)-Déclenche quand les utilisateurs sont chargés dans. Si les utilisateurs ajoutés par le biais de liste sont déjà dans le graphique, cet événement ne se déclenchera pas

### <a name="using-social-manager-events"></a>À l’aide des événements du Gestionnaire de sociale

Les réseaux sociaux Manager vous indiquera également que s’est-il passé sous la forme d’événements.  Vous pouvez utiliser ces événements pour mettre à jour de votre interface utilisateur ou d’effectuer toute autre logique.

#### <a name="source-example-using-the-c-api"></a>Exemple de code source à l’aide de l’API C++

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();
socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::no_extra_detail
    );

auto socialUserGroup = socialManager->create_social_user_group_from_filters(
    xboxLiveContext->user(),
    presence_filter::all,
    relationship_filter::friends
    );

socialManager->do_work();
// TODO: initialize the game UI containing the friends list using game UI, socialUserGroup->users()

while(true)
{
    // some update loop in the game
    auto events = socialManager->do_work();
    for(auto evt : events)
    {
        auto affectedUsersFromGraph = socialUserGroup->get_users_from_xbox_user_ids(evt.users_affected());
        // TODO: render the changes to the friends list using game UI, passing in affectedUsersFromGraph
    }
}
```

##### <a name="source-example-using-the-c-api"></a>Exemple de la source à l’aide du C# API

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromFilters(
     xboxLiveContext.User,
     PresenceFilter.All,
     RelationshipFilter.Friends
     );

socialManager.DoWork();
// TODO: initialize the game UI containing the friends list using game UI, socialUserGroup->users()

while(true)
{
    // some update loop in the game
    IReadOnlyList<SocialEvent> Events = socialManager.DoWork();
    IReadOnlyList<XboxSocialUser> affectedUsersFromGraph;
    foreach(SocialEvent managerEvent in Events)
    {
        affectedUsersFromGraph = socialUserGroup.GetUsersFromXboxUserIds(managerEvent.UsersAffected);
    }
}

```

#### <a name="events-returned"></a>Événements renvoyés

`local_user_added`(C++) | `LocalUserAdded`(C#)-Déclenche quand le chargement du graphique social utilisateurs est terminé. Indique si des erreurs se sont produites pendant l’initialisation

`social_user_group_loaded`(C++) | `SocialUserGroupLoaded`(C#)-Déclenche la création d’un groupe d’utilisateurs de réseaux sociaux

`users_added_to_social_graph`(C++) | `UsersAddedToSocialGraph`(C#)-Déclenche quand les utilisateurs sont chargés dans

#### <a name="additional-details"></a>Informations supplémentaires

Cet exemple montre une partie du contrôle supplémentaire proposé.  Plutôt que d’utiliser les filtres de groupe utilisateur social pour fournir une liste d’utilisateur actualisée lors de la boucle du jeu, le graphique social est initialisé en dehors de la boucle du jeu.  Puis le titre s’appuie sur le `events` retourné par le `socialManager->do_work()` (fonction).  `events` est une liste de `social_event`et chaque `social_event` contient une modification au graphique social qui se sont produites pendant la dernière image.  Par exemple `profiles_changed`, `users_added`, etc.  Vous trouverez plus d’informations dans le `social_event` documentation de l’API.

### <a name="cleanup"></a>Nettoyage

#### <a name="cleaning-up-social-user-groups"></a>Nettoyage des groupes d’utilisateurs de réseaux sociaux

```cpp
//#include "Social.h"

socialManger->destroy_social_user_group(
    socialUserGroup
    );
```

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.DestroySocialUserGroup(
     socialUserGroup
     );
```

Nettoie le groupe d’utilisateurs de réseaux sociaux qui a été créé. L’appelant doit également supprimer toutes les références qu'ils ont à aucun groupe d’utilisateur social créé car il contient maintenant des données périmées.

#### <a name="cleaning-up-local-users"></a>Nettoyage des utilisateurs locaux

```cpp
//#include "Social.h"

socialManger->remove_local_user(
    xboxLiveContext->user()
    );
```

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.RemoveLocalUser(
     xboxLiveContext.User
     );
```

Supprimer l’utilisateur local supprime le graphique social utilisateurs chargés, ainsi que les groupes d’utilisateurs sociaux qui ont été créés à l’aide de cet utilisateur.

#### <a name="events-returned"></a>Événements renvoyés

`local_user_removed`(C++) | `LocalUserRemoved`(C#)-Déclenche quand un utilisateur local a été correctement supprimé.