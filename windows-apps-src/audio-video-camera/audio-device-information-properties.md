---
ms.assetid: 3b75d881-bdcf-402b-a330-23cd29d68e53
description: Cet article répertorie les propriétés DeviceInformation liées aux appareils audio
title: Propriétés d’informations des appareils audio
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 97b6bd0c3567c00902a9528d54e6417f41ac66ed
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359094"
---
# <a name="audio-device-information-properties"></a>Propriétés d’informations des appareils audio

Cet article répertorie les propriétés d’informations liées aux appareils audio Dans Windows, chaque périphérique matériel dispose de propriétés [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) fournissant des informations détaillées sur l’appareil. Vous pouvez les consulter quand vous avez besoin d’informations spécifiques sur l’appareil ou quand vous créez un sélecteur d’appareil. Pour obtenir des informations générales sur l’énumération des appareils sur Windows, voir [Énumérer les appareils](../devices-sensors/enumerate-devices.md) et [Propriétés d’informations d’appareil](../devices-sensors/device-information-properties.md).


|Nom|type|Description|
|------------------------------------------------------------|------------|------------------------------------------------------|
|**System.Devices.AudioDevice.Microphone.SensitivityInDbfs**|Double|Spécifie la sensibilité du microphone en décibels par rapport aux décibels pleine échelle (dBFS).|
|**System.Devices.AudioDevice.Microphone.SignalToNoiseRatioInDb**|Double|Spécifie le ratio entre le signal du microphone  et le bruit mesuré en décibels (dB).|
|**System.Devices.AudioDevice.SpeechProcessingSupported**|Booléen|Indique si l’appareil audio prend en charge le traitement de la parole.|
|**System.Devices.AudioDevice.RawProcessingSupported**|Booléen|Indique si l’appareil audio prend en charge le traitement des fichiers bruts.|
|**System.Devices.MicrophoneArray.Geometry**|unsigned char[]|Données géométriques pour un réseau de microphones.|

## <a name="related-topics"></a>Rubriques connexes

* [Énumérer les appareils](../devices-sensors/enumerate-devices.md)
* [Propriétés des informations de périphérique](../devices-sensors/device-information-properties.md)
* [Créer un sélecteur d’appareils](../devices-sensors/build-a-device-selector.md)
* [Lecture de contenu multimédia](media-playback.md)




