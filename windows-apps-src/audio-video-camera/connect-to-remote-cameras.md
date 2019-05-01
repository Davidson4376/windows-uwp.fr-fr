---
ms.assetid: ''
description: Cet article vous montre comment se connecter à des caméras de surveillance à distance et obtenir un MediaFrameSourceGroup pour récupérer des images à partir de chaque appareil photo.
title: Se connecter à des caméras de surveillance à distance
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bc719b8dad2adef0542edf284d257846052eac21
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63789585"
---
# <a name="connect-to-remote-cameras"></a>Se connecter à des caméras de surveillance à distance

Cet article vous montre comment se connecter à un ou plusieurs caméras de surveillance à distance et obtenir un [ **MediaFrameSourceGroup** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) objet qui permet de lire des images à partir de chaque appareil photo. Pour plus d’informations sur la lecture des frames à partir d’une source de média, consultez [traiter des images de média avec MediaFrameReader](process-media-frames-with-mediaframereader.md). Pour plus d’informations sur l’association avec des appareils, consultez [Couplez appareils](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices).

> [!NOTE] 
> Les fonctionnalités décrites dans cet article sont uniquement disponibles à partir de Windows 10, version 1903.

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>Créer une classe DeviceWatcher pour surveiller les caméras de surveillance à distance disponibles

Le [ **DeviceWatcher** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) classe surveille les appareils disponibles pour votre application et informe votre application lorsque les appareils sont ajoutés ou supprimés. Obtenir une instance de **DeviceWatcher** en appelant [ **DeviceInformation.CreateWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_), en passant une chaîne de syntaxe de requête avancée (AQS) qui identifie le type de appareils que vous souhaitez surveiller. La chaîne AQS spécifiant les caméras réseau est la suivante :

```
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> La méthode d’assistance [ **MediaFrameSourceGroup.GetDeviceSelector** ](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) retourne une chaîne AQS qui surveille les caméras réseau connectés localement et à distance. Pour surveiller uniquement les caméras réseau, vous devez utiliser la chaîne AQS indiquée ci-dessus.


Lorsque vous démarrez retourné **DeviceWatcher** en appelant le [ **Démarrer** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.start) (méthode), il déclenchera le [ **Added** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added) événement pour chaque caméra réseau qui est actuellement disponible. Jusqu'à ce que vous arrêtiez l’Observateur en appelant [ **arrêter**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop), le **Added** événement est déclenché lorsque les nouveaux appareils de caméra réseau deviennent disponibles et le [ **Supprimé** ](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.devicewatcher.removed) événement est déclenché quand un appareil photo n’est plus disponible.

Les arguments d’événement passé dans le **Added** et **supprimé** gestionnaires d’événements sont un [ **élément DeviceInformation** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) ou un [  **DeviceInformationUpdate** ](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.deviceinformationupdate) de l’objet, respectivement. Chacun de ces objets a un **Id** propriété qui est l’identificateur de l’appareil photo de réseau pour lequel l’événement a été déclenché. Transmettre cette ID dans le [ **MediaFrameSourceGroup.FromIdAsync** ](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) méthode pour obtenir un [ **MediaFrameSourceGroup** ](https://docs.microsoft.com/en-us/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) objet que vous pouvez utiliser pour récupérer des images à partir de l’appareil photo.

## <a name="remote-camera-pairing-helper-class"></a>Classe d’assistance appariement de caméra à distance

L’exemple suivant montre une classe d’assistance qui utilise un **DeviceWatcher** pour créer et mettre à jour un **ObservableCollection** de **MediaFrameSourceGroup** objets pour prendre en charge liaison de données à la liste des appareils photo. Les applications classiques encapsulent la **MediaFrameSourceGroup** dans une classe de modèle personnalisé. Notez que la classe d’assistance conserve une référence à l’application [ **CoreDispatcher** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) et met à jour la collection des caméras dans des appels aux [ **RunAsync** ](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) pour vous assurer que l’interface utilisateur liée à la collection est mise à jour sur le thread d’interface utilisateur.

En outre, cet exemple gère le [ **DeviceWatcher.Updated** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) événement outre le **Added** et **supprimé** événements. Dans le **mise à jour** gestionnaire, l’appareil photo à distance associé est supprimé de, puis à la collection.

[!code-cs[SnippetRemoteCameraPairingHelper](./code/Frames_Win10/Frames_Win10/RemoteCameraPairingHelper.cs#SnippetRemoteCameraPairingHelper)]


## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Photo de base, vidéo, audio et de capture à MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Exemple de trames de la caméra](https://go.microsoft.com/fwlink/?LinkId=823230)
* [Blocs de processus multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md)
 

 




