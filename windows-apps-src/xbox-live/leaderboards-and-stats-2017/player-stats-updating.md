---
title: La mise à jour des statistiques 2017
description: Découvrez comment mettre à jour des statistiques de joueur Xbox Live à l’aide de statistiques 2017.
ms.assetid: 019723e9-4c36-4059-9377-4a191c8b8775
ms.date: 08/24/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, statistiques, statistiques 2017
ms.localizationpriority: medium
ms.openlocfilehash: d5b37c008e6aa719b641321c5e5a1c3360b20786
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631824"
---
# <a name="updating-stats-2017"></a>La mise à jour des statistiques 2017

Vous mettez à jour les statistiques en envoyant la valeur la plus récente pour la Xbox Live service en utilisant le `StatsManager` API qui seront abordés ci-dessous.

C’est à votre titre pour effectuer le suivi des statistiques, et que vous appelez `StatsManager` aux mettre à jour comme il convient.  `StatsManager` seront toutes les modifications de la mémoire tampon et ceux-ci au service vider régulièrement.  Votre titre pouvez vider manuellement.

> [!NOTE]
> Ne videz pas trop souvent les statistiques.  Sinon, votre titre sera taux limité.  Une meilleure pratique consiste à vider toutes les 5 minutes au maximum une fois.

### <a name="multiple-devices"></a>Plusieurs appareils

Il est possible pour un lecteur lire votre titre sur plusieurs appareils.  Dans ce cas, vous devez rendre certains efforts pour préserver la synchronisation des éléments.

Par exemple, si un lecteur a été 15 headshots sur leur Xbox à domicile.  Une version ultérieure, ils pouvaient headshots plus 10 sur leur Xbox à un ami.  Vous devez envoyer la valeur stat 25 sur le deuxième.  Mais vous auriez aucun moyen de le savoir sans synchronisation d’une certaine manière ces informations.

Il existe plusieurs façons, vous pouvez le faire :

1. Les Store à l’aide de [stockage connecté](../storage-platform/connected-storage/connected-storage-technical-overview.md).  Vous utiliseriez généralement connecté de stockage pour chaque utilisateur enregistrer des données.  Ces données sont synchronisées sur des appareils différents pour un utilisateur donné.
2. Utilisez votre propre service web pour préserver la synchronisation des statistiques si vous avez déjà une pour l’exécution des tâches d’auxiliaire pour votre titre.

### <a name="offline"></a>Hors connexion

Comme nous l’avons mentionné ci-dessus, votre titre est chargé d’effectuer le suivi des statistiques et donc responsable pour prendre en charge les scénarios hors connexion. 

### <a name="examples"></a>Exemples

Nous allons examiner un exemple pour relier ces concepts.

Un stat courantes dans un jeu de course est temps de présentation.  Généralement inférieure est préférable que ces statistiques.  Vous pourriez créer un stat et le classement associé, où réduire est préférable.  En d’autres termes, ce classement sont trié dans l’ordre croissant.

Votre titre est suivi de temps de présentation d’un utilisateur pendant leur session de lecture.  Vous serez de mettre à jour le Gestionnaire de statistiques uniquement si elles avaient un temps au tour inférieur à la meilleure précédente.

Vous pouvez suivre leur meilleur précédente dans une des manières suivantes :
1. À partir de l’enregistrement de fichiers à l’aide de stockage connectés.
2. Votre propre service web.

Le service remplacera la valeur stat quelle.  Par conséquent, même si vous deviez mettre à jour avec une heure de présentation qui est supérieure à la meilleure précédente, puis leur meilleur précédente serait remplacées.

Par conséquent, vérifiez que votre titre, que vous envoyez uniquement les valeurs statistiques appropriées selon votre scénario de jeu.  Dans certains cas, des valeurs plus faibles peut être préférable, dans d’autres cas plus élevées peut être une meilleure ou autre chose entièrement.

## <a name="programming-guide"></a>Guide de programmation

En règle générale, votre flux pour l’utilisation de statistiques est :

1. Initialiser le `StatsManager` API en passant un utilisateur local.
1. Comme un utilisateur lit un via votre titre, mettre à jour les valeurs statistiques à l’aide de la `set_stat` fonctions.
1. Ces mises à jour statistiques seront périodiquement vidés et écrites sur Xbox Live.  Vous pouvez également procéder manuellement.

### <a name="initialization"></a>Initialisation

Vous appelez le `StatsManager` avec un utilisateur local pour initialiser l’API avec les informations nécessaires.

Voir ci-dessous pour obtenir un exemple

```cpp
std::shared_ptr<stats_manager> statsManager = stats_manager::get_singleton_instance();
statsManager->add_local_user(user);
statsManager->do_work();  // returns stat_event_type::local_user_added
```

```csharp
Microsoft.Xbox.Services.Statistics.Manager.StatisticManager statManager = StatisticManager.SingletonInstance;
statManager.AddLocalUser(user);
statEvent = statManager.DoWork();
```

### <a name="writing-stats"></a>Statistiques d’écriture

Vous écrivez des statistiques à l’aide de la `stats_manager::set_stat` famille de fonctions.  Il existe trois variantes de cette fonction pour chaque type de données :

* `set_stat_number` pour les valeurs float.
* `set_stat_integer` pour les entiers.
* `set_stat_string` pour les chaînes.

Lorsque vous appelez ces, les statistiques mises à jour sont mis en cache localement sur l’appareil.  Ces seront périodiquement vidés à Xbox Live.

Vous avez la possibilité de vider manuellement les statistiques via la `stats_manager::request_flush_to_service` API.  Veuillez noter que si vous appelez cette fonction trop souvent, vous serez soumis à restriction.  Cela ne signifie pas que les jours fériés sera jamais mis à jour.  Cela signifie simplement que la mise à jour se produit lorsque le délai d’attente expire.

```cpp
statsManager->set_stat_integer(user, L"numHeadshots", 20);
statsManager->request_flush_to_service(user); // requests flush to service, performs a do_work
statsManager->do_work();  // applies the stat changes, returns stat_update_complete after flush to service
```

```csharp
statManager.SetStatisticIntegerData(user, statName, (long)statValue);
statManager.RequestFlushToService(user);
statManager.DoWork();
```

#### <a name="example"></a>Exemple

Supposons que vous avez un tir.  Au cours d’une correspondance, vous pourrez accumuler les statistiques suivantes :

| Nom Stat | Format |
|-----------|--------|
| Meilleures tue par tourniquet | Entier |
| Durée de vie TUE | Entier |
| Décès de durée de vie | Entier |
| Durée de vie Kill/mort Ratio | Numéro |

Comme pour le joueur passe par la correspondance, il serait incrémenter le *arrête par Round*, *arrête de durée de vie* et *décès de durée de vie* localement.

À la fin de la correspondance, vous procéderiez comme suit :
1. Comparez le tue qu'ils obtenus à l’arrondi, avec leurs meilleures précédente.  Si elle est supérieure, puis mettre à jour les `StatsManager`.
2. Mettre à jour leurs arrête de durée de vie et les décès avec les nouvelles valeurs et `StatsManager`.
3. Calculer tue/décès et mettre à jour `StatsManager`

Notez que par 1 et 2, vous devez connaître leurs valeurs statistiques précédentes.  Consultez les sections ci-dessus pour obtenir des recommandations sur la récupération de celles-ci.

Une de ces statistiques peuvent correspondre à un classement qui est abordé dans le prochain article.

### <a name="flushing-stats"></a>Statistiques de vidage

Vous pouvez vider manuellement les statistiques à l’aide de `stats_manager::request_flush_to_service`.  Vous souhaiterez peut-être faire si souhaité pour afficher un classement.

Par exemple, si vous aviez un classement pour `Lifetime Kills` dans l’exemple ci-dessus, vous souhaitez vous assurer que les statistiques mises à jour correspondant à cette statistique avaient été vidées sur le serveur avant d’afficher le classement.  De cette façon, les tableaux de résultats reflète la progression plus récente du joueur.

### <a name="cleanup"></a>Nettoyage
Lorsque le titre se ferme, supprimez l’utilisateur à partir du Gestionnaire de statistiques. Vous videz ainsi les dernières valeurs statistiques au service.

```cpp
statsManager->remove_local_user(user);
statsManager->do_work();  // applies the stat changes, returns local_user_removed after flush to service
```

```csharp
statManager.RemoveLocalUser(user);
statManager.DoWork();
```
