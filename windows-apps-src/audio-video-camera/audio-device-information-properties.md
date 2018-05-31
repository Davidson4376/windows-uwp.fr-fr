---
author: drewbatgit
ms.assetid: 3b75d881-bdcf-402b-a330-23cd29d68e53
description: Cet article répertorie les propriétés DeviceInformation liées aux appareils audio
title: Propriétés d’informations des appareils audio
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c221e3d77419ca02b46e8be227f3b943fe8dc241
ms.sourcegitcommit: ef5a1e1807313a2caa9c9b35ea20b129ff7155d0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2018
ms.locfileid: "1639009"
---
# <a name="audio-device-information-properties"></a>Propriétés d’informations des appareils audio

Cet article répertorie les propriétés d’informations liées aux appareils audio Dans Windows, chaque périphérique matériel dispose de propriétés [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) fournissant des informations détaillées sur l’appareil. Vous pouvez les consulter quand vous avez besoin d’informations spécifiques sur l’appareil ou quand vous créez un sélecteur d’appareil. Pour obtenir des informations générales sur l’énumération des appareils sur Windows, voir [Énumérer les appareils](../devices-sensors/enumerate-devices.md) et [Propriétés d’informations d’appareil](../devices-sensors/device-information-properties.md).


|Nom|Type|Description|
|------------------------------------------------------------|------------|------------------------------------------------------|
|**System.Devices.AudioDevice.Microphone.SensitivityInDbfs**|Double|Spécifie la sensibilité du microphone en décibels par rapport aux décibels pleine échelle (dBFS).|
|**System.Devices.AudioDevice.Microphone.SignalToNoiseRatioInDb**|Double|Spécifie le ratio entre le signal du microphone et le bruit mesuré en décibels (dB).|
|**System.Devices.AudioDevice.SpeechProcessingSupported**|Booléen|Indique si l’appareil audio prend en charge le traitement de la parole.|
|**System.Devices.AudioDevice.RawProcessingSupported**|Booléen|Indique si l’appareil audio prend en charge le traitement des fichiers bruts.|
|**System.Devices.MicrophoneArray.Geometry**|unsigned char[]|Données géométriques pour un réseau de microphones.|

## <a name="related-topics"></a>Rubriques connexes

* [Énumérer les appareils](../devices-sensors/enumerate-devices.md)
* [Propriétés d’informations de périphérique](../devices-sensors/device-information-properties.md)
* [Créer un sélecteur d’appareil](../devices-sensors/build-a-device-selector.md)
* [Lecture de contenu multimédia](media-playback.md)




