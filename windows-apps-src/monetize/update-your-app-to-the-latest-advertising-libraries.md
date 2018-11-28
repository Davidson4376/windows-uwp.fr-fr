---
description: Découvrez comment mettre à jour votre application afin d’utiliser les bibliothèques de publicités Microsoft les plus récentes prises en charge et assurez-vous que votre application continue à recevoir des bannières publicitaires.
title: Mettre à jour votre application avec les bibliothèques de publicités les plus récentes
ms.date: 08/23/2017
ms.topic: article
keywords: windows 10, uwp, annonces, publicité, AdMediatorControl, AdControl, migrer
ms.assetid: f8d5b2ad-fcdb-4891-bd68-39eeabdf799c
ms.localizationpriority: medium
ms.openlocfilehash: adac5cfdb1b4a10674fb7173e5b84a86b509f130
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7846575"
---
# <a name="update-your-app-to-the-latest-advertising-libraries-for-banner-ads"></a>Mettre à jour votre application avec les bibliothèques de publicités les plus récentes pour bannières publicitaires

Depuis le 1er avril 2017, nous ne servons plus de bannières publicitaires aux applications qui utilisent une version non prise en charge du SDK publicitaire. Si vous utilisez **AdControl** pour afficher des bannières publicitaires dans votre application de plateforme Windows universelle (UWP), utilisez les informations de cet article pour déterminer si vous utilisez un SDK Microsoft Advertising non pris en charge et faites migrer votre application vers un SDK pris en charge.

## <a name="overview"></a>Vue d'ensemble

Les applications UWP qui affichent des bannières publicitaires doivent utiliser le contrôle **AdControl** provenant des bibliothèques de publicités distribuées dans le [SDK MicrosoftAdvertising](http://aka.ms/ads-sdk-uwp). Ce SDK prend en charge un ensemble minimal de fonctionnalités, notamment la possibilité de servir des médias enrichis HTML5 via les [spécifications MRAID (définitions de l’interface publicitaire Rich-Media mobile) 1.0](http://www.iab.com/wp-content/uploads/2015/08/IAB_MRAID_VersionOne.pdf) définies par l’Interactive Advertising Bureau (IAB). La plupart de nos annonceurs veulent profiter de ces fonctionnalités. C’est pourquoi nous demandons aux développeurs d’utiliser une de ces versions de SDK pour rendre notre écosystème d’applications plus attrayant pour eux, ce qui par ailleurs augmentera vos revenus.

Avant la publication de ce Kit de développement, nous avions créé la classe **AdControl** dans plusieurs versions plus anciennes du SDK publicitaire. Ces anciennes versions des SDK publicitaires ne sont plus prises en charge, car elles ne gèrent pas les fonctionnalités publicitaires minimales décrites ci-dessus. Depuis le 1er avril 2017, nous ne servons plus de bannières publicitaires aux applications qui utilisent une version non prise en charge du SDK publicitaire. Si vous disposez d’une application qui utilise toujours une version non prise en charge du SDK publicitaire, vous constaterez le comportement suivant:

* Les bannières publicitaires ne seront plus fournies aux contrôles **AdControl** dans votre application, et vous ne gagnerez plus de revenus publicitaires à partir de ces contrôles.

* Lorsque le contrôle **AdControl** de votre application demande une nouvelle publicité, l’événement **ErrorOccurred** du contrôle est déclenché et la propriété **ErrorCode** des arguments d’événement prend la valeur **NoAdAvailable**.

* Toutes les unités publicitaires associées à votre application seront désactivées. Vous ne pouvez pas supprimer ces unités publicitaires désactivée à partir de votre compte du centre de DePartnerv. Si vous mettez à jour votre application pour qu’elle utilise un [SDK Microsoft Advertising](http://aka.ms/ads-sdk-uwp) pris en charge, ignorez ces unités publicitaires et créez-en de nouvelles.

* En outre, les bannières publicitaires ne seront plus servies aux unités publicitaires utilisées dans plusieurs applications. Assurez-vous que vos unités publicitaires ne sont utilisées que dans une seule application.

Si vous disposez d’une application existante (qui se trouve déjà dans le Store ou est en cours de développement) qui affiche des bannières publicitaires à l’aide du contrôle **AdControl** et si vous avez des doutes sur le SDK publicitaire utilisé dans votre application, suivez les instructions de cet article pour déterminer si vous devez mettre à jour votre application pour qu’elle utilise un SDK pris en charge. Si vous rencontrez des problèmes ou si vous avez besoin d’une assistance, [contactez le support](http://go.microsoft.com/fwlink/?LinkId=393643).

> [!NOTE]
> Si votre application utilise déjà le [SDK MicrosoftAdvertising](http://aka.ms/ads-sdk-uwp) (pour les applications UWP), vous n’avez pas besoin de lui apporter des modifications supplémentaires.

## <a name="prerequisites"></a>Éléments prérequis

* Le code source complet et les fichiers de projet Visual Studio de votre application qui utilise **AdControl**.
* Le package .appx de votre application.

> [!NOTE]
> Si vous n’avez plus le package .appx de votre application, mais si vous disposez toujours d’un ordinateur de développement avec la version de Visual Studio et le SDK Microsoft Advertising utilisé pour générer votre application, vous pouvez régénérer le package .appx dans Visual Studio.

<span id="part-1" />

## <a name="part-1-determine-whether-you-need-to-update-your-uwp-app"></a>Partie 1: Déterminez si vous avez besoin de mettre à jour votre application UWP

Suivez les instructions dans les sections suivantes pour déterminer si vous devez mettre à jour votre application.

1. Créez une copie du package .appx pour votre application afin de ne pas modifier l’original, renommez la copie pour qu’elle ait une extension .zip et extrayez le contenu du fichier.

2. Vérifier le contenu extrait du package de votre application:
  * Si vous voyez un fichier Microsoft.Advertising.dll, votre application utilise un ancien SDK et vous devez mettre à jour votre projet en suivant les instructions dans les sections ci-dessous. Passez à la [Partie 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).
  * Si vous ne voyez pas de fichier Microsoft.Advertising.dll, votre application UWP utilise déjà le SDK Microsoft Advertising le plus récent disponible et vous n’avez pas besoin d’apporter des modifications à votre projet.


<span id="part-2" />

## <a name="part-2-install-the-latest-sdk"></a>Partie 2 : Installez le SDK le plus récent

Si votre application utilise une ancienne version du SDK, suivez ces instructions pour vous assurer que vous disposez du SDK le plus récent sur votre ordinateur de développement.

1. Vérifiez que Visual Studio2015 ou une version ultérieure est installé votre ordinateur de développement.
    > [!NOTE]
    > Si Visual Studio est ouvert sur votre ordinateur de développement, fermez-le avant de passer aux étapes suivantes.

1.  Désinstallez toutes les versions précédentes du SDK Microsoft Advertising et du SDK médiateur de publicités sur votre ordinateur de développement.

2.  Ouvrez une fenêtre **Invite de commandes** et exécutez ces commandes pour nettoyer les versions de SDK qui peuvent avoir été installées avec Visual Studio, mais qui n’apparaissent peut-être pas dans la liste des programmes installés sur votre ordinateur:
    ```syntax
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Installez le [SDK Microsoft Advertising](http://aka.ms/ads-sdk-uwp).

## <a name="part-3-update-your-project"></a>Partie 3: Mettez à jour votre projet

Supprimez du projet toutes les références existantes aux bibliothèques de publicités Microsoft et suivez [ces instructions](install-the-microsoft-advertising-libraries.md#reference) pour ajouter les références requises. Ainsi vous êtes certain que votre projet utilise les bibliothèques correctes. Vous pouvez conserver le balisage et le code existants.

## <a name="part-4-test-and-republish-your-app"></a>Partie 4: Testez et republiez votre application

Testez votre application pour vous assurer qu’elle affiche les bannières publicitaires comme prévu.

Si la version précédente de votre application est déjà disponible dans le Windows Store, [créer une nouvelle soumission](../publish/app-submissions.md) pour votre application mise à jour dans l’espace partenaires de republier l’application.
