---
author: jnHs
Description: After your packages have been successfully uploaded, you'll see a table that indicates which packages will be offered to specific Windows 10 device families (and earlier OS versions, if applicable), in ranked order.
title: Disponibilité de la famille d’appareils
ms.author: wdg-dev-content
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, packages, télécharger, disponibilité famille d’appareils
ms.localizationpriority: medium
ms.openlocfilehash: e86b56c09f907e45655a0ef9b94fad30a4959b59
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4571399"
---
# <a name="device-family-availability"></a>Disponibilité de la famille d’appareils

Une fois vos packages correctement chargés dans la page **Packages**, la section **Disponibilité de la famille d’appareils** affiche un tableau identifiant les packages offerts aux familles d’appareilsWindows10 spécifiques (et aux versions antérieures du système d’exploitation, le cas échéant), classés par ordre. Dans cette section, vous déterminez également si vous souhaitez offrir ou non la soumission aux clients sur des familles d’appareils Windows10 spécifiques.

> [!NOTE]
> Si vous n’avez pas encore chargé les packages, la section **Disponibilité de la famille d’appareils** affiche les familles d’appareils Windows10 avec des cases à cocher vous permettant de configurer la soumission pour les clients sur ces familles d’appareils. Le tableau apparaît une fois que vous avez chargé un ou plusieurs packages.

Cette section inclut également une case à cocher permettant d’indiquer si vous souhaitez permettre à Microsoft de rendre l’application disponible pour les futures familles d’appareils Windows10. Nous recommandons de conserver cette case à cocher activée pour que votre application puisse être disponible pour davantage de clients potentiels à mesure que de nouvelles familles d’appareils seront introduites.


## <a name="choosing-which-device-families-to-support"></a>Sélection des familles d’appareils prises en charge

Si vous téléchargez des packages ciblant une famille d’appareils spécifique, nous allons cocher la case pour rendre ces packages disponibles pour les nouveaux clients sur ce type d’appareil. Par exemple, si un package cible Windows.Desktop, la case **Windows10 Desktop** sera cochée pour ce package (et vous ne pourrez pas cocher les cases pour d’autres familles d’appareils).

Les packages ciblant la famille d’appareils Windows.Universal peuvent s’exécuter sur n’importe quel appareil Windows10 (y compris XboxOne). Par défaut, nous allons rendre ces packages disponibles pour les nouveaux clients sur tous les types d’appareils *sauf* pour Xbox.

Vous pouvez décocher la case en regard d’une famille d’appareilsWindows10 si vous ne souhaitez pas proposer la soumission aux clients sur ce type d’appareils. Si la case associée à une famille d’appareils est décochée, les nouveaux clients sur ce type d’appareils ne pourront pas acquérir l’application (bien que les clients qui possèdent déjà l’application puissent toujours l’utiliser, et bénéficient des mises à jour soumises).

Si votre application les prend en charge, nous vous recommandons de conserver toutes les cases cochées, sauf si vous avez une raison spécifique de limiter les types d’appareils Windows10 pouvant acquérir votre application. Par exemple, si vous savez que votre application n’offre pas une bonne expérience sur [SurfaceHub](https://developer.microsoft.com/windows/surfacehub) et/ou [MicrosoftHoloLens](https://developer.microsoft.com/windows/mixed-reality), vous pouvez décocher la case **Windows10 Collaboration** et/ou **Windows10 Holographique**. Cela empêche tout nouveau client d’acquérir l’application sur ces appareils. Si vous estimez ultérieurement être prêt à proposer l’application à ces clients, vous pouvez créer une soumission en activant ces cases à cocher.

<span id="xbox" />

La seule famille d’appareils Windows10 qui n’est pas activée par défaut pour les packages Windows.Universal est **Xbox Windows10**. Si votre application n’est pas un jeu (ou s’il s’agit d’un jeu et que vous avez activé le [Programme Créateurs Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) ou que vous êtes passé par le processus d’[approbation de concept](../gaming/concept-approval.md)), et que votre soumission comprend des packagesUWP neutres et/ou x64 compilés à l’aide de la version14393 du SDKWindows10 ou une version ultérieure, vous pouvez cocher la case **Xbox Windows10** afin de proposer l'application aux clients sur XboxOne.

> [!IMPORTANT]
> Pour que votre application se lance sur les appareilsXbox, vous devez inclure un package neutre ou x64 compilé avec la version14393du SDKWindows ou une version ultérieure. Toutefois, si vous cochez la case **Xbox Windows10**, votre package présentant la version la plus élevée applicable à Xbox (un package neutre ou x64 ciblant la famille d’appareilsuniverselle ou Xbox) sera toujours proposé aux clients sur Xbox, même s’il est compilé avec une versionantérieure du SDK. Pour cette raison, il est essentiel de vous assurer que le package présentant la version la plus élevée applicable à Xbox soit compilé avec la version14393 du SDKWindows ou une version supérieure. Si ce n’est pas le cas, vous verrez un message d’erreur indiquant que les clientsXbox ne seront pas en mesure de lancer votre application. 
> 
> Pour résoudre ce problème, procédez comme suit:
> - Remplacez les packages applicables par les instances compilées à l’aide de la version14393 du SDKWindows ou une version ultérieure.
> - Si vous disposez déjà d’un package qui prend en charge Xbox et qui est compilé à l’aide de la version14393 du SDKWindows ou une version ultérieure, augmentez son numéro de version, de manière à en faire le package présentant la version la plus élevée de la soumission.
> - Désélectionnez la case à cocher **Xbox Windows 10**.
>   
> SI vous ne parvenez toujours pas à résoudre le problème, contactez le support technique.

Si vous soumettez une applicationUWP pour Windows10IoT Standard, vous ne devez pas modifier les sélections par défaut après avoir chargé vos packages; il n’existe aucune case à cocher distincte pour Windows10IoT. Pour plus d’informations sur la publication d’applicationsUWP sous loT Standard, voir [Prise en charge dans le MicrosoftStore pour les applicationsUWP sous IoT Standard](https://docs.microsoft.com/windows/iot-core/commercialize-your-device/installingandservicing).

Si votre soumission comprend des packages pouvant s’exécuter sur **Windows8/8.1** et **Windows Phone8.x et les versions antérieures**, ces packages seront rendus disponibles pour les clients, comme illustré dans le tableau. Il n’existe aucune case à cocher pour ces versions de système d’exploitation. Pour arrêter de proposer votre application à ces clients, supprimez les packages correspondants de votre soumission.

> [!IMPORTANT]
> Pour empêcher complètement une famille d’appareils Windows 10 spécifique d’obtenir votre soumission, mettez à jour de l’élément [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) dans votre manifeste pour cibler uniquement la famille d’appareils que vous souhaitez prendre en charge (c'est-à-dire, Windows.Mobile ou Windows.Desktop), au lieu de cela que de laisser la valeur Windows.Universal (pour la famille d’appareils universelle) que Microsoft Visual Studio inclut dans le manifeste par défaut.

Il est important de savoir que les sélections que vous effectuez dans la section **Disponibilité de la famille d’appareils** s’appliquent uniquement aux nouvelles acquisitions. Quiconque disposant déjà de votre application peut continuer à l’utiliser et obtenir les mises à jour que vous soumettez, même si vous supprimez sa famille d’appareils ici. Cela s’applique même aux clients ayant acquis votre application avant la mise à niveau vers Windows10. Par exemple, si vous avez une application publiée avec des packages Windows Phone 8.1, puis ajoutez un package Windows 10 (UWP) à la même application, qui cible la famille d’appareils universelle, les clients mobiles Windows 10 qui disposaient de votre package Windows Phone 8.1 recevront une mise à jour vers ce package Windows 10 (UWP), même si vous avez désactivé la case à cocher **Windows 10 Mobile** (car il ne s’agit pas d’une nouvelle acquisition, mais d’une mise à jour). En revanche, si vous ne fournissez pas de package Windows 10 (UWP) ciblant la famille d’appareils universelle ou d’appareils mobiles, vos clients mobiles Windows 10 resteront avec le package Windows Phone 8.1.

Pour plus d’informations sur les familles d’appareils, voir [**Vue d’ensemble des familles d’appareils**](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview).

## <a name="understanding-ranking"></a>Compréhension du classement

La section **Disponibilité de la famille d’appareils**, qui vous permet d’identifier les familles d’appareils Windows10 pouvant télécharger votre soumission, vous indique également les packages spécifiques qui seront rendus disponibles pour les différentes familles d’appareils. Si vous possédez plusieurs packages pouvant s’exécuter sur une famille d’appareils spécifique, le tableau indique l’ordre de proposition des packages, en fonction de leur numéro de version. Pour plus d’informations sur la façon dont le Store classe les packages en fonction de leur numéro de version, consultez la section [Numérotation des versions de packages](package-version-numbering.md). 

Par exemple, imaginons que vous disposez de deuxpackages: Package_A.appxupload et Package_B.appxupload. Pour une famille d’appareils donnée, si Package_A.appxupload est classé au premier rang et que Package_B.appxupload est classé au second rang, lorsqu’un client sur ce type d’appareil acquiert votre application, le WindowsStore tentera dans un premier temps d’offrir Package_A.appxupload. Si l’appareil du client n’est pas en mesure d’exécuter Package_A.appxupload, le WindowsStore propose Package_B.appxupload. Si l’appareil du client ne peut exécuter aucun des packages associés à cette famille d’appareils, par exemple, si l’instance **MinVersion** prise en charge par votre application est supérieure à la version de l’appareil du client, le client ne pourra pas télécharger l’appareil sur cet appareil.

> [!NOTE]
> Les numéros de version des packages.xap ne sont pas pris en compte lors de la détermination du package à fournir à un client donné. Pour cette raison, si vous disposez de plusieurs packages de rang égal, vous verrez un astérisque en lieu et place d’un numéro; les clients peuvent recevoir les deuxpackages. Pour mettre à jour les clients d’un package .xap vers une version plus récente, veillez à supprimer l’ancien .xap dans la nouvelle soumission.

