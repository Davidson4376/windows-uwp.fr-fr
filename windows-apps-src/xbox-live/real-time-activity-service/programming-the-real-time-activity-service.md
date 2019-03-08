---
title: Programmation de Service de l’activité en temps réel
description: En savoir plus sur la programmation du Service Xbox Live en temps réel activité avec les APIs C++.
ms.assetid: 98cdcb1f-41d8-43db-98fc-6647755d3b17
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, activité en temps réel
ms.localizationpriority: medium
ms.openlocfilehash: f8846d57343f4f7262bbeea2cec03465fa23b2ab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629994"
---
# <a name="programming-the-real-time-activity-service-using-c-apis"></a>Programmation de Service de l’activité en temps réel à l’aide de C++ APIs

Cet article contient les sections suivantes

* Connexion au Service de l’activité en temps réel à partir de Xbox Live
* Déconnexion de l’activité en temps réel
* Création d’une statistique
* S’abonner à une statistique de l’activité en temps réel
* Annulation à partir d’une statistique à partir du service de l’activité d’en temps réel
* Exemple

## <a name="connecting-to-the-real-time-activity-service-from-xbox-live"></a>Connexion au Service de l’activité en temps réel à partir de Xbox Live

Applications doivent se connecter au service de l’activité en temps réel (RTA) pour obtenir des informations sur les événements à partir de la Xbox Live. Cette rubrique montre comment établir une connexion de ce type.

> [!NOTE]
> Les exemples utilisés dans cette rubrique indiquent des appels de méthode pour un utilisateur. Toutefois, votre titre doit effectuer ces appels pour tous les utilisateurs à se connecter à et de vous déconnecter du service de l’activité en temps réel (RTA).

### <a name="connecting-to-the-real-time-activity-service"></a>Connexion au service en temps réel activité

```cpp
void Example_RealTimeActivity_ConnectAsync()
{
    // Get the list of users from the system, and create an Xbox Live context from the first.
    // This user's authentication token will be used for the service requests.

    // Note, only retrieve an XboxLiveContext for a given user *once*.  Otherwise you may encounter unpredictable behavior.
    std::shared_ptr<xbox_live_context> xboxLiveContext = std::make_shared<xbox_live_context>(User::Users->GetAt(0));

    xboxLiveContext->real_time_activity_service()->activate();
}
```

### <a name="creating-a-statistic"></a>Création d’une statistique

Créer des statistiques sur XDP si vous êtes un développeur XDK ou un travail sur un titre de cross-play.  Vous créez des statistiques dans Partner Center si vous apportez une pure UWP s’exécutant sur Windows 10.

#### <a name="xdk-developers"></a>Développeurs XDK

Pour plus d’informations sur la création d’une statistique sur XDP, veuillez consulter la [XDP Documentation](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_10_27_15_a.aspx#events).  Après avoir créé votre stat et défini vos événements, vous devez exécuter le [XCETool](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/atoc_xce_jun15.aspx) pour générer un en-tête utilisé par votre application.  Cet en-tête contient des fonctions que vous pouvez appeler pour envoyer des événements qui modifient les statistiques.

#### <a name="uwp-developers"></a>Développeurs UWP

Si vous développez une UWP sur Windows 10 qui n’est pas un titre de cross-play, vous définissez vos statistiques dans [partenaires](https://partner.microsoft.com/dashboard). Lire le [l’article configuration de statistiques de partenaires](../leaderboards-and-stats-2017/player-stats-configure-2017.md) pour savoir comment configurer des statistiques sur les partenaires.

> [!NOTE]
> Statistiques 2013 développeur doit contacter leur mère pour plus d’informations concernant [statistiques 2013 configuration](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/windows-configure-stats-2013) dans [partenaires](https://partner.microsoft.com/dashboard).

### <a name="disconnecting-from-the-real-time-activity-service"></a>Déconnexion du service de l’activité d’en temps réel

```cpp
void Example_RealTimeActivity_Disconnect()
{
    // Get the list of users from the system, and create an Xbox Live context from the first.
    // This user's authentication token will be used for the service requests.
    std::shared_ptr<xbox_live_context> xboxLiveContext = std::make_shared<xbox_live_context>(User::Users->GetAt(0));

    xboxLiveContext->real_time_activity_service()->deactivate();
}
```

## <a name="subscribing-to-a-statistic-from-the-real-time-activity"></a>S’abonner à une statistique de l’activité en temps réel

Les applications s’abonner à une activité en temps réel (RTA) pour obtenir des mises à jour lorsque les statistiques configurés dans le portail de développement Xbox (XDP) ou partenaires changent.

### <a name="subscribing-to-a-statistic-from-the-real-time-activity-service"></a>S’abonner à une statistique à partir du service de l’activité d’en temps réel

```cpp
void Example_RealTimeActivity_SubscribeToStatisticChangeAsync()
{
    // Obtain xboxLiveContext as shown in the connect function.  Then add a handler to be called on statistic changes.
    function_context statisticsChangeContext = xboxLiveContext->user_statistics_service().add_statistic_changed_handler(
        [](statistic_change_event_args args )
        {
            // Called on statistic change.  Inspect args to see which one.
            DebugPrint("%S %S", args.latest_statistic().statistic_name().c_str(), args.latest_statistic().value().c_str());
        }
    );

    // Call to subscribe to an individual statistic.  Once the subscription is complete, the handler will be called with the initial value of the statistic.
    auto statisticResults = xboxLiveContext->user_statistics_service().subscribe_to_statistic_change(
        User::Users->GetAt(0)->XboxUserId->Data(),
        L"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx",    // Get SCID from "Product Details" page in XDP or the Xbox Live Setup page in Partner Center
         L"YourStat"
        );

    std::shared_ptr<statistic_change_subscription> statisticsChangeSubscription;

    if(!statisticResults.err())
    {
        statisticsChangeSubscription = statisticResults.payload();
    }
    else
    {
        DebugPrint("Error calling SubscribeToStatistic: %S", statisticResults.err_message().c_str());
    }
}
```

## <a name="unsubscribing-from-a-statistic-from-the-real-time-activity-service"></a>Annulation à partir d’une statistique à partir du service de l’activité d’en temps réel

Les applications s’abonner à une statistique à partir du service de l’activité en temps réel (RTA) pour obtenir des mises à jour lorsque la statistique change. Lorsque ces mises à jour ne sont plus nécessaires, l’abonnement peut être arrêtée, et le code de cette rubrique montre comment procéder.

### <a name="unsubscribing-from-a-real-time-services-statistic"></a>L’annulation de l’abonnement à partir d’une statistique de services en temps réel

```cpp
void Example_RealTimeActivity_UnsubscribeFromStatisticChangeAsync()
{
    // statisticsChangeSubscription from the Example_RealTimeActivity_SubscribeToStatisticChangeAsync function.
    xboxLiveContext->user_statistics_service().unsubscribe_from_statistic_change(
        statisticsChangeSubscription
        );
}
```

> [!IMPORTANT]
> L’activité en temps réel de service se déconnecte après deux heures d’utilisation, votre code doit être en mesure de détecter et de rétablir une connexion au service en temps réel activité s’il est toujours nécessaire. Pour cela, principalement pour garantir l’actualisation des jetons d’authentification à l’expiration.
> 
> Si un client utilise la valeur RTA pour les sessions multijoueurs et est déconnecté pendant 30 secondes, le mode multijoueur Directory(MPSD) de Session détecte que la session de l’agrégation RTA est fermée et intervient l’utilisateur de la session. Il incombe au client d’agrégation RTA pour détecter lorsque la connexion est fermée et lancer une reconnexion et se réabonner avant MPSD termine la session.