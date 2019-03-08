---
title: Succès 2017
description: Succès 2017
ms.assetid: d424db04-328d-470c-81d3-5d4b82cb792f
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: bfc67f6aca27abf095a89c451111e6429bca82e1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590984"
---
# <a name="achievements-2017"></a>Succès 2017

Le système de primes 2017 permet aux développeurs de jeux à utiliser un modèle d’appel direct pour déverrouiller des primes pour les nouveaux jeux Xbox Live sur Xbox One, Windows 10, 10 de Windows Phone, Android et iOS.

## <a name="introduction"></a>Introduction

Avec Xbox One, nous avons introduit un nouveau système Cloud-Powered primes qui permet aux développeurs de jeux pour piloter les données de leurs capacités de Xbox Live, telles que les statistiques de l’utilisateur, primes, présence riche et multijoueurs, en envoyant simplement les événements dans le jeu de données de télémétrie. Cela a permis d’obtenir une multitude de nouveaux avantages : un événement unique peut mettre à jour les données de plusieurs fonctionnalités Xbox Live. Configuration de Xbox Live se trouve sur le serveur et non dans le client ; et bien plus encore.

Dans les années suivant le lancement de Xbox One, nous avons écouté étroitement aux commentaires des développeurs de jeux, et les développeurs ont constamment les éléments suivants :

1.  **Vous souhaitez déverrouiller primes via un modèle d’appel direct.** De nombreux développeurs créez des jeux pour différentes plateformes, y compris les versions précédentes de Xbox, et les systèmes de type prime sur ces plateformes utilisent une méthode d’appel directe. Prise en charge directe de déverrouiller les appels sur Xbox One et d’autres plateformes de Xbox actuel-gen seraient facilité leurs besoins de développement de jeux inter-plateformes et les coûts de temps de développement.

2.  **Réduire la complexité de la configuration.** Avec le système Cloud-Powered primes, une prime déverrouiller logique doit être configuré sur le Xbox Live, afin que les services de savoir comment interpréter les données de statistiques du titre et déverrouiller la réalisation d’un utilisateur. Cela a été effectuée via une nouvelle section de règles de réalisation de la configuration d’une prime qui n’existait pas précédemment. Tandis qu’avoir déverrouiller logique dans le cloud permettre s’avérer très performante, cette exigence de configuration supplémentaire augmente la complexité dans la conception et la création des résultats obtenus, d’un titre.

3.  **Difficile à dépanner.** Bien que le système Cloud-Powered primes présente un éventail de fonctionnalités utiles, il peut également être plus difficile pour le jeu aux développeurs de valider et résoudre les problèmes avec leurs résultats dans la mesure où prime déverrouille sont déclenchées indirectement par les règles qui en direct sur le service plutôt que directement contrôlée par le jeu en lui-même.

Il est important de noter que les développeurs de jeux et ont été également à plusieurs reprises partagés commentaires qu’ils apprécient valeur certaines fonctionnalités qui ont été introduites, ainsi que le système Cloud-Powered primes :

1.  Nouvelles fonctionnalités d’expérience utilisateur telles que la progression de la réalisation, les mises à jour en temps réel, concept art récompenses et validation déverrouille dans le flux d’activités.

2.  Améliorations de configuration, par exemple une configuration de service au lieu d’une configuration locale qui doit être incluse dans le package de jeu (par exemple, gameconfig, XLAST, SPA, etc.) et la possibilité de modifier facilement la réalisation des chaînes & images après le jeu a été expédiée.

Avec primes 2017, nous créons un remplacement du Cloud-Powered primes système existant pour les titres de futures à utiliser qui facilite encore davantage pour les développeurs de jeux Xbox configurer des primes, intégrer prime déverrouille & mises à jour dans le code de jeux et confirmer que les primes fonctionne comme prévu.

## <a name="whats-different-with-achievements-2017"></a>Quelle est la différence avec primes 2017

|                          | Système de primes 2017        | Système de primes sur le cloud      |
|--------------------------|---------------------------------------|----------------------------------------|
| Déverrouiller le déclencheur           | Directement via un appel d’API                 | Indirectement via les événements de télémétrie        |
| Déverrouiller le propriétaire             | Titre                                 | Xbox Live                              |
| Configuration            | Chaînes, des images, des récompenses              | Chaînes, des images, des récompenses, déverrouiller règles \[+ statistiques + événements\]                    |
| Progression              | Prise en charge <br>*directement via un appel d’API*                | Prise en charge <br> *indirectement via les événements de télémétrie*       |
| Activité en temps réel (RTA) | Prise en charge                             | Prise en charge                              |
| Défis               | Non prise en charge   | Prise en charge                      |

## <a name="title-requirements"></a>Demandes de titre

Les exigences de n’importe quel titre qui utilisera le système de primes 2017 sont les suivantes :

1.  **Doit être un nouveau titre (non publiée).** Les titres qui ont déjà été libérées et utilisent le système Cloud-Powered primes ne sont pas éligibles. Pour plus d’informations, consultez [pourquoi ne peut pas titres existants « migrer » sur le nouveau système de primes 2017 ?](#_Why_can’t_existing)

2.  **Doit utiliser août 2016 XDK ou une version ultérieure.** L’API Update_Achievement a été publiée dans le Kit de développement août 2016 XDK.

3.  **Doit être un titre XDK ou UWP.** Le système de primes 2017 n’est pas disponible pour les plates-formes héritées, notamment la Xbox 360, Windows 8.x ou une version antérieure, ni de Windows Phone 8 ou une version antérieure.

## <a name="updateachievement-api"></a>Update_Achievement API

Une fois que vos primes sont configurés via XDP ou [UDC](../configure-xbl/dev-center/achievements-in-udc.md) et publié sur votre sandbox de développement, votre titre permettre les déverrouiller en appelant l’API Update_Achievement.

L’API est disponible dans le XDK et le Kit de développement Xbox Live.

### <a name="api-signature"></a>Signature de l’API

La signature de l’API est la suivante :

```c++
/// <summary>
    /// Allow achievement progress to be updated and achievements to be unlocked.  
    /// This API will work even when offline. On PC and Xbox One, updates will be posted by the system when connection is re-established even if the title isn't running
    /// </summary>
    /// <param name="xboxUserId">The Xbox User ID of the player.</param>
    /// <param name="titleId">The title ID.</param>
    /// <param name="serviceConfigurationId">The service configuration ID (SCID) for the title.</param>
    /// <param name="achievementId">The achievement ID as defined by XDP or Partner Center.</param>
    /// <param name="percentComplete">The completion percentage of the achievement to indicate progress.
    /// Valid values are from 1 to 100. Set to 100 to unlock the achievement.  
    /// Progress will be set by the server to the highest value sent</param>
    /// <remarks>
    /// Returns a task<T> object that represents the state of the asynchronous operation.
    ///
    /// This method calls V2 GET /users/xuid({xuid})/achievements/{scid}/update
    /// </remarks>
    _XSAPIIMP pplx::task<xbox::services::xbox_live_result<void>> update_achievement(
        _In_ const string_t& xboxUserId,
        _In_ uint32_t titleId,
        _In_ const string_t& serviceConfigurationId,
        _In_ const string_t& achievementId,
        _In_ uint32_t percentComplete
        );
```

`xbox::services::xbox_live_result<T>` est l’appel de retour pour tous les appels d’API de Services Live C++ Xbox.

Pour plus d’informations, consultez la discussion Xfest 2015, « XSAPI : C++, aucune exception ! »<br>
[video](https://go.microsoft.com/?linkid=9888207) |  [slides](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Documents/Xfest_2015/Xbox_Live_Track/XSAPI_Cpp_No_Exceptions.pptx)

### <a name="unlocking-via-updateachievement-api"></a>Déverrouillage via Update_Achievement API

Pour déverrouiller une prime, affectez la *percentComplete* à 100.

Si l’utilisateur est en ligne, la demande sera envoyée immédiatement au service Xbox Live primes et déclenche les expériences utilisateur suivantes :

-   L’utilisateur recevra une notification de réalisation déverrouillé ;

-   La prime spécifiée s’affiche en tant que déverrouillé dans la liste de réalisation de l’utilisateur pour le titre ;

-   La prime déverrouillée est ajoutée au flux d’activités de l’utilisateur.

> *Remarque : Il n’y aura aucune différence visible dans les expériences utilisateur pour les primes qui utilisent le système de primes 2017 et les primes Cloud-Powered.*

Si l’utilisateur est hors connexion, la demande de déverrouillage est mises en attente localement sur l’appareil de l’utilisateur. Lors de l’appareil de l’utilisateur a rétabli la connectivité réseau, la demande sera automatiquement être envoyées au service primes : Remarque : aucune action n’est requise à partir du jeu pour déclencher cette solution et les expériences utilisateur ci-dessus seront produit comme décrit.

### <a name="updating-completion-progress-via-updateachievement-api"></a>La mise à jour de progression de saisie semi-automatique via l’API Update_Achievement

Pour mettre à jour la progression d’un utilisateur de déverrouiller une prime, définissez le *percentComplete* au nombre entier approprié entre 1 et 100.

État d’avancement d’une prime peut uniquement augmenter. Si *percentComplete* est défini sur un nombre inférieur à la réalisation dernière *percentComplete* valeur, la mise à jour sera ignorée. Par exemple, si la prime *percentComplete* a déjà été définie sur 75, l’envoi d’une mise à jour avec la valeur 25 sera ignorée et la réalisation toujours apparaîtront en tant qu’achevée à 75 %.

Si *percentComplete* est défini à 100, la prime sera déverrouillé.

Si *percentComplete* est définie par un nombre supérieur à 100, l’API se comporte comme si vous le définissez sur 100 exactement.

## <a name="frequently-asked-questions"></a>Forum Aux Questions

### <a name="span-idwhyarechallenges-classanchorspancan-i-ship-my-title-using-the-achievements-2017-system-yet"></a><span id="_Why_are_Challenges" class="anchor"></span>Puis-je expédier me titre à l’aide du système de primes 2017 encore ?

Absolument ! Tous les nouveaux titres sont bienvenues et encouragées d’utiliser le 2017 primes système à la place le système Cloud-Powered primes.

### <a name="why-are-challenges-not-supported-in-the-achievements-2017-system"></a>Pourquoi sont défis pas pris en charge dans le système de primes 2017 ?

Données d’utilisation dans tous les jeux de Xbox a montré que l’implémentation actuelle et la présentation des défis ne répond pas à un besoin pour la plupart des développeurs de jeux. Nous continuer la collecte des commentaires dans cet espace des développeurs et vous efforcer de distribuer des fonctionnalités futures qui sont plus sur le point avec les besoins des développeurs. L’arène Xbox nouvellement publié est un exemple d’une fonctionnalité qui introduit de nouvelles fonctionnalités de concurrence pour Xbox une direction de nouveau, mais il est similaire, des jeux.

### <a name="can-i-still-add-new-achievements-every-calendar-quarter-if-my-title-is-using-the-achievements-2017-system"></a>Toujours ajouter nouveau primes chaque trimestre de calendrier si mon titre utilise le système de primes 2017 ?

Oui. La stratégie de primes est inchangée.

### <a name="span-idwhycantexisting-classanchorspanwhy-cant-existing-titles-migrate-onto-the-new-achievements-2017-system"></a><span id="_Why_can’t_existing" class="anchor"></span>Pourquoi ne peut pas titres existants « migrer » sur le nouveau système de primes 2017 ?

Pour la grande majorité des titres existants, une « migration » dans le système de primes 2017 ne serait pas limitée à simplement la mise à jour leurs configurations de service et permuter des écritures des événements pour la réalisation de déverrouiller appels : bien que ces modifications seules serait très coûteux et aurait un risque significatif de l’erreur et un comportement inattendu qui peut entraîner les primes à irrémédiablement l’arrêt. Au lieu de cela, la plupart des titres existants ont également des utilisateurs avec des données existantes. Une tentative de conversion d’un jeu dynamique qui utilise déjà les primes Cloud-Powered système serait non seulement un effort très coûteux, pour le développement et la Xbox, mais compromettrait considérablement les profils des utilisateurs existants et/ou les expériences de jeu.

### <a name="if-my-title-was-released-using-the-cloud-powered-achievements-system-can-any-future-dlc-for-the-title-switch-to-achievements-2017"></a>Si mon titre a été publiée à l’aide du système Cloud-Powered primes, n’importe quel DLC futures pour le titre peut basculer vers primes 2017 ?

Toutes les primes pour un titre doivent utiliser le même système de primes. Quelle que soit la system primes est utilisé par le succès de base est le système qui doit être utilisé pour toutes les primes futures pour le titre.

### <a name="while-testing-achievements-in-my-dev-sandbox-can-i-mix-and-match-between-using-the-achievements-2017-system-and-the-cloud-powered-achievements-system"></a>Lors du test des primes dans mon sandbox de développement, faire correspondance entre l’utilisation du système de primes 2017 et le système Cloud-Powered primes ?

Non. Toutes les primes pour un titre doivent utiliser le même système de primes.

### <a name="does-achievements-2017-also-include-offline-unlocks"></a>Primes 2017 également inclut-il hors connexion déverrouille ?

Si le titre déverrouille une prime, tandis que l’appareil est hors connexion, l’API Update_Achievement les requêtes de déverrouillage hors connexion de la file d’attente et automatiquement sera la synchronisation automatique à la Xbox Live lorsque l’appareil a rétabli la connectivité réseau, similaire à actuel Expérience hors connexion sur le cloud primes du système. Primes déverrouille va pas se produire lorsque l’utilisateur est hors connexion.

### <a name="i-see-a-new-achievementupdate-event-in-xdp-if-my-title-uses-that-event-does-that-mean-it-has-achievements-2017"></a>Je vois un nouvel événement de « AchievementUpdate » dans XDP. Si mon titre utilise cet événement, cela veut-il dire qu’il a les primes 2017 ?

Le *AchievementUpdate* événement de base est requis par Xbox Live à des fins de serveur principal. Vous pouvez l’ignorer en toute sécurité. Si votre titre configure un événement à l’aide de ce type d’événement de base, ces écritures événement seront ignorées par Xbox Live. Les titres qui reposent sur le système Cloud-Powered primes doivent continuer à configurer leurs événements en utilisant les autres types d’événements de base. Les titres qui reposent sur le système de primes 2017 pas nécessaire de configurer *n’importe quel* événements à des fins de réalisation.
