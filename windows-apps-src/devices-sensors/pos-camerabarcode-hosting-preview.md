---
title: Héberger l'aperçu du scanneur de codes-barres à caméra
description: Découvrez comment héberger un aperçu de scanneur de codes-barres à caméra dans votre application
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: b3391a7640e49fb43ac0f7ea33a0fa992c4a7495
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243280"
---
# <a name="hosting-a-camera-barcode-scanner-preview-in-your-application"></a>Héberger un aperçu de scanneur de codes-barres à caméra dans votre application
## <a name="step-1-setup-your-camera-preview"></a>Étape 1 : Configurer la préversion de votre appareil photo
Vous pouvez accomplir la première étape de l’ajout d’un aperçu à votre application de scanneur de codes-barres à caméra en suivant les instructions fournies dans la rubrique [Afficher l’aperçu de la caméra](../audio-video-camera/simple-camera-preview-access.md).  Une fois que vous avez terminé cette étape, revenez à cette rubrique pour voir les modifications spécifiques du scanneur de code-barres à caméra.

## <a name="step-2-update-capability-declarations"></a>Étape 2 : Déclaration des fonctionnalités de mise à jour
Pour empêcher vos utilisateurs de recevoir l'invite de consentement du microphone, vous pouvez l'exclure des fonctionnalités répertoriées dans le manifeste de votre application.

1. Dans Microsoft Visual Studio, dans l’**Explorateur de solutions**, ouvrez le concepteur pour le manifeste de l’application en double-cliquant sur l’élément **package.appxmanifest**.
2. Sélectionnez l’onglet **Fonctionnalités**.
3. Désélectionnez la case à cocher **Microphone**.

 ## <a name="step-3-add-additional-using-directive-for-media-capture"></a>Étape 3 : Ajouter une directive using supplémentaire pour la capture de média

```Csharp
using Windows.Media.Capture;
```

## <a name="step-4-set-up-your-mediacapture-initialization-settings"></a>Étape 4 : Configurer vos paramètres d’initialisation de MediaCapture
L’exemple suivant initialise les paramètres [**MediaCaptureInitializationSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings). 

```Csharp
 private void InitCaptureSettings()
{
    _captureInitSettings = new MediaCaptureInitializationSettings();
    _captureInitSettings.VideoDeviceId = BarcodeScanner.VideoDeviceId;
    _captureInitSettings.StreamingCaptureMode = StreamingCaptureMode.Video;
    _captureInitSettings.PhotoCaptureSource = PhotoCaptureSource.VideoPreview;
}
```
## <a name="step-5-associate-your-mediacapture-object-with-the-camera-barcode-scanner"></a>Étape 5 : Associer votre objet MediaCapture à l’aide du scanneur de codes-barres de l’appareil photo
Remplacez le mediaCapture.InitializeAsync() existant dans *StartPreviewAsync()* par ce qui suit :

```Csharp
try
    {

        mediaCapture = new MediaCapture();
        await mediaCapture.InitializeAsync(InitCaptureSettings());

        displayRequest.RequestActive();
        DisplayInformation.AutoRotationPreferences = DisplayOrientations.Landscape;
    }
```

> [!TIP]
> Consultez [Afficher l’aperçu de la caméra](https://docs.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access#add-capability-declarations-to-the-app-manifest) pour voir des rubriques plus approfondies sur l’hébergement d’un aperçu de la caméra dans votre application.

## <a name="see-also"></a>Voir aussi

### <a name="samples"></a>Exemples

- [Exemple de scanneur de codes-barres](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
