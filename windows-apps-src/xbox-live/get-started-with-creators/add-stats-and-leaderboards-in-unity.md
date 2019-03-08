---
title: Ajouter des statistiques et des tableaux de résultats à votre projet Unity
description: Apprenez à utiliser le plug-in de Xbox Live Unity pour ajouter des statistiques et classements à votre projet Unity.
ms.assetid: 756b3c31-a459-4ad2-97af-119adcd522b5
ms.date: 10/19/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox un, Unity, les créateurs
ms.localizationpriority: medium
ms.openlocfilehash: 17af9afc8a9048e7222115d2afdc6108d25df1d7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658134"
---
# <a name="add-player-stats-and-leaderboards-to-your-unity-project"></a>Ajouter des statistiques et des tableaux de résultats à votre projet Unity

> [!IMPORTANT]
> Le plug-in de Xbox Live Unity ne prend pas en charge les primes ou mode multijoueur en ligne et qu’il est recommandé uniquement pour [programme Xbox Live Creators](../developer-program-overview.md) membres.

Une fois que vous avez ajouté [Xbox Live connectez-vous](unity-prefabs-and-sign-in.md) à votre projet Unity, l’étape suivante consiste à ajouter des statistiques et classements basés sur ces statistiques.

Avec le [plug-in de Xbox Live Unity](https://github.com/Microsoft/xbox-live-unity-plugin), vous pouvez facilement ajouter des statistiques et des classements dans votre projet Unity. Comme pour les étapes de la connexion, que vous pouvez choisir d’utiliser les prefabs inclus ou d’attacher les scripts inclus à vos propres objets de jeu personnalisés.

## <a name="prerequisites"></a>Conditions préalables
1. [Configurer Xbox Live dans Unity](configure-xbox-live-in-unity.md)
2. [Connectez-vous à Xbox Live dans Unity](unity-prefabs-and-sign-in.md)

## <a name="player-stats"></a>Statistiques de lecteur

Un stat player est toutes les statistiques intéressantes que vous souhaitez effectuer le suivi pour vos joueurs. Les statistiques que vous suivez à Xbox Live doivent être des statistiques qui sont pertinents pour le lecteur et sont affichés dans un mode quelconque. Ces statistiques sont couramment utilisés pour générer des résultats commerciaux, ce qui peuvent afficher les joueurs pour déterminer la façon dont leurs performances évalue par rapport à d’autres joueurs. Toutes les statistiques sont considérées comme « statistiques proposés », ce qui signifie que les statistiques seront affichés dans la page GameHub pour le jeu.

Statistiques du lecteur doivent représenter une valeur unique. Le plug-in de Xbox Live Unity contient prefabs pour integer, double et les statistiques de la chaîne. En outre, un script est fourni pour un objet de statistiques de base qui peut être étendu à d’autres types de données.

Pour plus d’informations sur les statistiques, consultez [statistiques](../leaderboards-and-stats-2017/player-stats.md).

> [!NOTE]
> Pour utiliser des statistiques ou des tableaux de résultats avec le service Xbox Live, vous devez disposer un utilisateur connecté avec succès avant que vous pouvez accéder à toutes les données.

## <a name="using-the-player-stat-prefabs"></a>À l’aide des prefabs stat player

Il existe plusieurs prefabs fournis dans le plug-in de Xbox Live Unity que vous pouvez utiliser relatives aux statistiques :

* IntegerStat : Une statistique qui peut être exprimée comme une valeur entière, comme le nombre total de tue dans un cycle.
* DoubleStat: Une statistique qui peut être exprimée comme flottante point valeur, telle qu’un ratio kill/mort.
* StringStat: Une statistique qui peut être exprimée comme valeur de chaîne, généralement une énumération, par exemple un rang attribué pour un cycle, tels que « Gold », « Silver » ou « Bronze ».
* StatPanel : Exemple d’interface utilisateur que vous pouvez utiliser pour afficher la valeur actuelle d’une statistique.

Pour ajouter un stat player, faites simplement glisser le préfabriqué qui correspond au type de données de la statistique sur la scène. Dans l’inspecteur de Unity pour la statistique, vous pouvez spécifier trois valeurs :

* ID de la statistique. Cela doit correspondre à l’ID configuré dans le centre partenaires et respecte la casse.
* Nom complet de la statistique (ce nom s’affiche dans l’interface utilisateur de prefab StatPanel).
* La valeur initiale de la statistique au démarrage de la scène.

Vous pouvez utiliser la **StatPanel** préfabriqué pour afficher la valeur d’une statistique. Pour ce faire, faites glisser un **StatPanel** prefab sur la scène. Vous pouvez spécifier les jours fériés à afficher en faisant glisser le gameobject stat à la **Stat** champ la **StatPanel** objet dans l’inspecteur de Unity.

### <a name="manipulating-the-player-stat-values"></a>Manipuler les valeurs de stat player

Les objets de statistiques de lecteur ont un nombre de fonctions que vous pouvez appeler pour régler la valeur de la statistique. Ces fonctions peuvent être appelées à partir d’autres routines ou liées aux éléments d’interface utilisateur. Vous pouvez examiner le **DoubleStat**, **IntegerStat**, et **StringStat** scripts pour voir des exemples de fonctions pour modifier la valeur de la statistique. Vous pouvez modifier ou créer des scripts pour représenter des statistiques avec les fonctions et la logique plus complexe. Nouvelles classes statistiques est souhaitable d’étendre le `StatBase` (classe), définie dans le `StatBase` script.

Par exemple, un test simple, vous pouvez ajouter un bouton de l’interface utilisateur à votre scène et dans le `OnClick` événements du bouton, dans l’inspecteur de Unity, ajoutez un **IntegerStat** et appelez le `Increment()` pour augmenter la valeur de la statistique (fonction) d’une unité chaque fois que vous cliquez sur le bouton.

Si vous avez la statistique également liée à un **StatPanel** objet, vous pouvez voir la valeur stat mettre à jour chaque fois que vous cliquez sur le bouton.

Chaque fois que vous mettez à jour vos statistiques (incrément, décrémentation, etc.), les valeurs mis à jour localement. Pour que ces mises à jour de statistiques reflétées dans Xbox Live doivent se passe deux choses. Tout d’abord, vous devez définir la valeur de statistiques avec l’une des fonctions StatisticManager.SetStatistic. Il existe trois `StatisticManager` fonctions pour définir des statistiques, `StatisticManager.SetStatisticIntegerData(XboxLiveUser user, String statName, Int64 value)`, `StatisticManager.SetStatisticNumberData(XboxLiveUser user, String statName, Double value)`, et `StatisticManager.SetStatisticStringData(XboxLiveUser user, String statName, String value)`. Chacune de ces fonctions est utilisé pour définir la valeur appropriée pour le type de données de votre stat. La deuxième chose que vous devez effectuer pour mettre à jour vos statistiques sur le serveur, consiste à *vider* les données locales. Les données obtient vidées automatiquement toutes les 5 minutes par le `StatManagerComponent` script.  Si votre jeu se termine avant les 5 minutes, vous devez veiller à vider les données manuellement tout d’abord s’assurer que vous ne perdez pas ce cours. Pour ce faire, vous devez appeler la `statManagerComponent.RequestFlushToService()` méthode, en veillant à appeler pour le **XboxLiveUser** est écrit pour la statistique.

> [!TIP]
> Il est considéré comme bonne pratique consiste à toujours vider les données avant la fin de votre jeu pour vous assurer que vous ne perdez pas la progression.

### <a name="checking-and-verifying-stats"></a>La vérification et la vérification des statistiques

Le `StatisticManager` classe a deux fonctions qui sont utiles pour la vérification sur les statistiques configurés pour un `XboxLiveUser`, `StatisticManager.GetStatisticNames(XboxLiveUser user)` et `StatisticManager.GetStatistic(XboxLiveUser user, String statName)`. `GetStatisticNames()` fournit un `list<string>` rempli par les noms des statistiques pour le XboxLiveUser fourni. Ces noms peuvent être utilisés pour appeler de la valeur actuelle d’une statistique en appelant le `GetStatistic()` (fonction). Il est important de noter que pendant que vous pouvez lire les statistiques du service stats Xbox Live conseillons de ne pas utiliser pour la logique de jeu, mais plutôt à la vérification de l’état de la statistique après que qu’il a été envoyée. Le service est destiné uniquement pour aider à exécuter d’autres services tels que des tableaux de résultats et n’est pas censé être une source de vérité pour les statistiques dans votre jeu. Il est important que le titre de la gérer toute la logique de statistiques comme aucun vérifications ne sont effectuées sur vos statistiques par le service Xbox et il accepte simplement toute valeur donnée en tant que l’état actuel.

## <a name="leaderboards"></a>Classements

Un classement représente une liste triée et numérotée des lecteurs ayant atteint la valeur « optimale » d’une statistique. Par exemple, un classement peut répertorier les personnes qui ont réalisé le temps le plus rapide sur une présentation de la concurrence, afin que les joueurs peuvent comparer leur meilleur moment concurrence sur les meilleurs temps de course obtenue par les autres lecteurs.

Classements sont basées sur les statistiques de lecteur sont envoyées au service Xbox Live par le jeu. Par conséquent, le classement des données sont en lecture seule, comme vous ne pouvez pas les modifier directement.

Le plug-in de Xbox Live Unity fournit un préfabriqué de classement d’exemple que vous pouvez utiliser pour comprendre comment implémenter des classements dans votre jeu.

Pour plus d’informations sur les classements, consultez [Leaderboards](../leaderboards-and-stats-2017/leaderboards.md).

## <a name="using-the-leaderboard-prefabs"></a>À l’aide des prefabs de classement

Le plug-in de Xbox Live Unity contient deux prefabs pour les tableaux de résultats :

* Classement : Un objet qui représente un classement et contient l’interface utilisateur simple pour afficher les valeurs à partir de la partie du classement.
* LeaderboardEntry: Objet qui représente une ligne unique d’un classement.

Vous pouvez faire glisser un **Leaderboard** prefab sur la scène. Dans l’inspecteur de Unity, vous pouvez définir les attributs suivants :

* Stat : Le gameobject stat ce classement est associé.
* Type de classement : L’étendue des résultats qui doivent être retournées pour les entrées de classement.
* Nombre d’entrées : Le nombre de lignes de données à afficher par page.

> [!NOTE]
> La partie Stat du préfabriqué de classement est initialement vide. Essayez de faire glisser l’un de le prefabs statistiques mentionnées ci-dessus dans l’emplacement de gameobject pour le test.

Dans l’éditeur Unity, le **classement** préfabriqué affiche toujours les mêmes données fictives, quels que soient les paramètres de l’inspecteur. Vous devez générer et exporter de votre projet dans Visual Studio et connectez-vous avec un utilisateur autorisé à voir les valeurs des données réelles. Pour plus d’informations, consultez [configurer Xbox Live dans Unity](configure-xbox-live-in-unity.md).

## <a name="see-also"></a>Voir également

* [L’authentification sur Xbox Live dans Unity](unity-prefabs-and-sign-in.md)
* [Configurer Xbox Live dans Unity](configure-xbox-live-in-unity.md)
* [La scène de l’exemple du classement](setup-leaderboard-example-scene.md)
* [Obtenir des données de classement](unity-leaderboard-from-scratch.md)
