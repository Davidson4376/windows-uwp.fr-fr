---
ms.assetid: D20C8E01-4E78-4115-A2E8-07BB3E67DDDC
description: Cet article montre comment accéder à la lampe d’un appareil et comment l’utiliser, le cas échéant. La fonctionnalité de lampe est gérée indépendamment des fonctionnalités de flash et d’appareil photo.
title: Lampe torche indépendante de la caméra
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5fd69900995a51af806c99b25aae43149d6e30a7
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7971074"
---
# <a name="camera-independent-flashlight"></a>Lampe torche indépendante de la caméra



Cet article montre comment accéder à la lampe d’un appareil et comment l’utiliser, le cas échéant. La fonctionnalité de lampe est gérée indépendamment des fonctionnalités de flash et d’appareil photo de l’appareil. Outre des informations de références sur la lampe et le réglage de ses paramètres, cet article vous montre comment libérer la ressource de la lampe quand elle n’est pas utilisée et comment détecter les changements de disponibilité de la lampe quand elle est utilisée par une autre application.

## <a name="get-the-devices-default-lamp"></a>Accéder à la lampe par défaut de l’appareil

Pour accéder à la lampe par défaut d’un appareil, appelez [**Lamp.GetDefaultAsync**](https://msdn.microsoft.com/library/windows/apps/dn894327). Les API de lampe se trouvent dans l’espace de noms [**Windows.Devices.Lights**](https://msdn.microsoft.com/library/windows/apps/dn894331). Veillez à ajouter une directive d’utilisation pour cet espace de noms avant d’essayer d’accéder à ces API.

[!code-cs[LightsNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetLightsNamespace)]


[!code-cs[DeclareLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDeclareLamp)]


[!code-cs[GetDefaultLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetDefaultLamp)]

Si l’objet renvoyé est **null**, l’API **Lamp** n’est pas prise en charge sur l’appareil. Certains appareils ne peuvent pas prendre en charge l’API **Lamp**, même si la lampe est physiquement présente sur l’appareil.

## <a name="get-a-specific-lamp-using-the-lamp-selector-string"></a>Accéder à une lampe spécifique à l’aide de la chaîne de sélecteur de lampe

Certains appareils possèdent plusieurs lampes. Pour obtenir la liste des lampes disponibles sur l’appareil, accédez à la chaîne de sélecteur d’appareil en appelant [**GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn894328). Cette chaîne de sélecteur peut alors être transmise à [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432). Cette méthode est utilisée pour énumérer les différents types d’appareil et la chaîne de sélecteur indique à la méthode de renvoyer uniquement les lampes. L’objet [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) renvoyé par **FindAllAsync** est une collection d’objets [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) représentant les lampes disponibles sur l’appareil. Sélectionnez l’un des objets de la liste, puis transmettez la propriété [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437) à [**Lamp.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn894326) pour obtenir une référence à la lampe demandée. Cet exemple utilise la méthode d’extension **GetFirstOrDefault** à partir de l’espace de noms **System.Linq** pour sélectionner l’objet **DeviceInformation** dans lequel la propriété [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) a une valeur de **Back**, qui permet de sélectionner une lampe à l’arrière du boîtier de l’appareil, le cas échéant.

Les API [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) se trouvent dans l’espace de noms [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459).

[!code-cs[EnumerationNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

[!code-cs[GetLampWithSelectionString](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetLampWithSelectionString)]

## <a name="adjust-lamp-settings"></a>Régler les paramètres de la lampe

Une fois que vous avez une instance de la classe [**Lamp**](https://msdn.microsoft.com/library/windows/apps/dn894310), activez la lampe en définissant la propriété [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn894330) sur **true**.

[!code-cs[LampSettingsOn](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOn)]

Éteignez la lampe en définissant la propriété [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn894330) sur **false**.

[!code-cs[LampSettingsOff](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOff)]

Certains appareils sont équipés de lampes qui prennent en charge les valeurs de couleur. Déterminez si une lampe prend en charge la couleur en vérifiant la propriété [**IsColorSettable**](https://msdn.microsoft.com/library/windows/apps/dn894329). Si cette valeur est **true**, vous pouvez définir la couleur de la lampe avec la propriété [**Color**](https://msdn.microsoft.com/library/windows/apps/dn894322).

[!code-cs[LampSettingsColor](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsColor)]

## <a name="register-to-be-notified-if-the-lamp-availability-changes"></a>S’inscrire pour être averti en cas de modification de la disponibilité de la lampe

L’accès à la lampe est accordé à l’application l’ayant demandé en dernier. Par conséquent, si une seconde application est lancée et qu’elle demande une ressource de lampe déjà utilisée par une première application, celle-ci ne sera plus en mesure de contrôler la lampe jusqu’à ce que la seconde application libère la ressource. Pour recevoir une notification lorsque la disponibilité de la lampe change, inscrivez un gestionnaire pour l’événement [**Lamp.AvailabilityChanged**](https://msdn.microsoft.com/library/windows/apps/dn894317).

[!code-cs[AvailabilityChanged](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChanged)]

Dans le gestionnaire de l’événement, vérifiez la propriété [**LampAvailabilityChanged.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/dn894315) pour déterminer si la lampe est disponible. Dans cet exemple, un bouton bascule permettant d’allumer ou d’éteindre la lampe est activé ou désactivé en fonction de la disponibilité de la lampe.

[!code-cs[AvailabilityChangedHandler](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChangedHandler)]

## <a name="properly-dispose-of-the-lamp-resource-when-not-in-use"></a>Libérer correctement la ressource de lampe quand elle n’est pas en cours d’utilisation

Lorsque vous n’utilisez plus la lampe, vous devez la désactiver et appeler [**Lamp.Close**](https://msdn.microsoft.com/library/windows/apps/dn894320) pour libérer les ressources et permettre aux autres applications d’accéder à la lampe. Cette propriété est mappée à la méthode **Dispose** si vous utilisezC#. Si vous êtes inscrit à [**AvailabilityChanged**](https://msdn.microsoft.com/library/windows/apps/dn894317), vous devez vous désinscrire du gestionnaire lorsque vous libérez la ressource de la lampe. L’emplacement idéal dans le code pour procéder à une telle opération dépend de l’application. Pour limiter l’accès à la lampe sur une seule page, libérez la ressource dans l’événement [**OnNavigatingFrom**](https://msdn.microsoft.com/library/windows/apps/br227509).

[!code-cs[DisposeLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDisposeLamp)]

## <a name="related-topics"></a>Rubriquesconnexes
- [Lecture de contenu multimédia](media-playback.md)

 




