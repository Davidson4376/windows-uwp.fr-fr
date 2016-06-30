---
author: drewbatgit
ms.assetid: 7DBEE5E2-C3EC-4305-823D-9095C761A1CD
description: "Cet article indique comment capturer une séquence de photos variables pour capturer plusieurs trames d’images en une succession rapide et configurer des paramètres de mise au point, de flash, de sensibilité ISO, d’exposition et de compensation de l’exposition différents pour chaque trame."
title: "Séquence de photos variables"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c6be6c0ea255c38bba65550ae44c7f88c140ca0f

---

# Séquence de photos variables

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Cet article indique comment capturer une séquence de photos variables pour capturer plusieurs trames d’images en une succession rapide et configurer des paramètres de mise au point, de flash, de sensibilité ISO, d’exposition et de compensation de l’exposition différents pour chaque trame. Cette fonctionnalité permet par exemple de créer des images HDR (High Dynamic Range).

Si vous souhaitez capturer des images HDR, mais que vous ne voulez pas implémenter votre propre algorithme de traitement, vous pouvez utiliser l’API [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) pour utiliser les fonctionnalités HDR intégrées dans Windows. Pour plus d’informations, voir [Capture photo avec plage dynamique élevée (HDR)](high-dynamic-range-hdr-photo-capture.md).

**Remarque**  
Cet article repose sur les concepts et sur le code décrits dans [Capturer des photos et des vidéos à l’aide de MediaCapture](capture-photos-and-video-with-mediacapture.md), qui détaille les étapes d’implémentation de capture photo et vidéo de base. Il est recommandé de vous familiariser avec le modèle de capture multimédia de base dans cet article avant de passer à des scénarios de capture plus avancés. Le code de cet article repose sur l’hypothèse que votre application possède déjà une instance de MediaCapture initialisée correctement.

## Configurer votre application pour utiliser une capture de séquence de photos variables

Outre les espaces de noms requis pour la capture multimédia de base, l’implémentation d’une capture de séquence de photos variables requiert les espaces de noms suivants.

[!code-cs[VPSUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSUsing)]

Déclarez une variable membre pour stocker l’objet [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564), utilisé pour initier la capture de la séquence de photos. Déclarez un tableau d’objets [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) pour stocker chaque image capturée dans la séquence. Déclarez également un tableau pour stocker l’objet [**CapturedFrameControlValues**](https://msdn.microsoft.com/library/windows/apps/dn608020) pour chaque trame. Ce dernier peut être utilisé par votre algorithme de traitement d’image afin de déterminer les paramètres utilisés pour la capture de chaque trame. Enfin, déclarez un index qui sera utilisé pour suivre l’image en cours de capture dans la séquence.

[!code-cs[VPSMemberVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSMemberVariables)]

## Préparer la capture de la séquence de photos variables

Une fois que vous avez initialisé votre [MediaCapture](capture-photos-and-video-with-mediacapture.md), assurez-vous que les séquences de photos variables sont prises en charge sur l’appareil actuel en obtenant une instance de l’élément [**VariablePhotoSequenceController**](https://msdn.microsoft.com/library/windows/apps/dn640573)à partir de l’objet [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) de la capture multimédia et en activant la propriété [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn640580).

[!code-cs[IsVPSSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsVPSSupported)]

Obtenez un objet [**FrameControlCapabilities**](https://msdn.microsoft.com/library/windows/apps/dn652548) à partir du contrôleur de séquence de photos variables. Cet objet possède une propriété pour chaque paramètre pouvant être configuré par trame d’une séquence de photos. Ces propriétés sont les suivantes :

-   [**Exposure**](https://msdn.microsoft.com/library/windows/apps/dn652552)
-   [**ExposureCompensation**](https://msdn.microsoft.com/library/windows/apps/dn652560)
-   [**Flash**](https://msdn.microsoft.com/library/windows/apps/dn652566)
-   [**Focus**](https://msdn.microsoft.com/library/windows/apps/dn652570)
-   [**IsoSpeed**](https://msdn.microsoft.com/library/windows/apps/dn652574)
-   [**PhotoConfirmation**](https://msdn.microsoft.com/library/windows/apps/dn652578)

Cet exemple définit une valeur de compensation d’exposition différente pour chaque trame. Pour vérifier que la compensation d’exposition est prise en charge pour les séquences de photos sur l’appareil actuel, vérifiez la propriété [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn278905) de l’objet [**FrameExposureCompensationCapabilities**](https://msdn.microsoft.com/library/windows/apps/dn652628) accessible par le biais de la propriété **ExposureCompensation**.

[!code-cs[IsExposureCompensationSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsExposureCompensationSupported)]

Créez un objet [**FrameController**](https://msdn.microsoft.com/library/windows/apps/dn652582) pour chaque trame que vous souhaitez capturer. Cet exemple capture trois trames. Définissez les valeurs des contrôles que vous souhaitez faire varier pour chaque trame. Ensuite, supprimez la collection [**DesiredFrameControllers**](https://msdn.microsoft.com/library/windows/apps/dn640574) de l’objet **VariablePhotoSequenceController** et ajoutez chaque contrôleur de trame à la collection.

[!code-cs[InitFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitFrameControllers)]

Créez un objet [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) pour définir le codage que vous souhaitez utiliser pour les images capturées. Appelez la méthode statique [**MediaCapture.PrepareVariablePhotoSequenceCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/dn608097) en indiquant les propriétés de codage. Cette méthode renvoie un objet [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564). Enfin, enregistrez des gestionnaires d’événements pour les événements [**PhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/dn652573) et [**Stopped**](https://msdn.microsoft.com/library/windows/apps/dn652585).

[!code-cs[PrepareVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPrepareVPS)]

## Démarrer la capture de la séquence de photos variables

Pour démarrer la capture de la séquence de photos variables, appelez [**VariablePhotoSequenceCapture.StartAsync**](https://msdn.microsoft.com/library/windows/apps/dn652577). Veillez à initialiser les tableaux pour stocker les images capturées et les valeurs de contrôle de trame et à définir l’index actuel sur la valeur 0. Définissez la variable d’état d’enregistrement de votre application et mettez à jour votre interface utilisateur pour désactiver le démarrage d’une autre capture pendant que cette capture est en cours.

[!code-cs[StartVPSCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartVPSCapture)]

## Recevoir les trames capturées

L’événement [**PhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/dn652573) est déclenché pour chaque trame capturée. Enregistrez les valeurs de contrôle de trame et l’image capturée pour la trame, puis augmentez l’index de trame actuel. Cet exemple indique comment obtenir une représentation [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) de chaque trame. Pour plus d’informations sur l’utilisation de **SoftwareBitmap**, voir [Acquisition d’images](imaging.md).

[!code-cs[OnPhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnPhotoCaptured)]

## Achever la capture de la séquence de photos variables

L’événement [**Stopped**](https://msdn.microsoft.com/library/windows/apps/dn652585) est déclenché lorsque toutes les trames de la séquence ont été capturées. Mettez à jour l’état d’enregistrement de votre application et votre interface utilisateur pour permettre à l’utilisateur de créer de nouvelles captures. À ce stade, vous pouvez transférer les images capturées et les valeurs de contrôle de trame dans votre code de traitement d’image.

[!code-cs[OnStopped](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnStopped)]

## Mettre à jour des contrôleurs de trame

Si vous souhaitez effectuer une autre capture de séquence de photos variables avec différents paramètres de trame, vous n’avez pas besoin de réinitialiser complètement l’élément **VariablePhotoSequenceCapture**. Vous pouvez supprimer la collection [**DesiredFrameControllers**](https://msdn.microsoft.com/library/windows/apps/dn640574) et ajouter de nouveaux contrôleurs de trame ou bien modifier les valeurs du contrôleur de trame existant. L’exemple suivant active l’objet [**FrameFlashCapabilities**](https://msdn.microsoft.com/library/windows/apps/dn652657) pour vérifier que l’appareil actuel prend en charge le flash et la puissance du flash pour les trames de séquences de photos variables. Si c’est le cas, chaque trame est mise à jour pour activer le flash à 100 %. Les valeurs de compensation d’exposition précédemment définies pour chaque trame sont toujours actives.

[!code-cs[UpdateFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateFrameControllers)]

## Nettoyer la capture de la séquence de photos variables

Lorsque vous avez terminé la capture de séquences de photos variables ou que votre application est mise en suspens, nettoyez l’objet de séquence de photos variables en appelant [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/dn652569). Annulez l’inscription des gestionnaires d’événements de l’objet et définissez-les sur la valeur Null.

[!code-cs[CleanUpVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpVPS)]

## Rubriques connexes

* [Capturer des photos et des vidéos à l’aide de MediaCapture](capture-photos-and-video-with-mediacapture.md)
 

 







<!--HONumber=Jun16_HO4-->


