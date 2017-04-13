---
author: drewbatgit
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: "Cet article vous montre comment utiliser les contrôles des appareils manuels pour activer les scénarios de capture vidéo, y compris la vidéo HDR et la priorité de l’exposition."
title: "Contrôles d’appareil photo manuel pour la capture vidéo"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: cd2adcffa233b76563e47f93f298cf954154adef
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="manual-camera-controls-for-video-capture"></a>Contrôles d’appareil photo manuel pour la capture vidéo

\[ Article mis à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Cet article vous montre comment utiliser les contrôles des appareils manuels pour activer les scénarios de capture vidéo, y compris la vidéo HDR et la priorité de l’exposition.

Les contrôles des appareils vidéo mentionnés dans cet article sont tous ajoutés à votre application en utilisant le même modèle. Tout d’abord, vérifiez si le contrôle est pris en charge sur l’appareil sur lequel votre application est en cours d’exécution. Si le contrôle est pris en charge, définissez le mode de votre choix pour le contrôle. En règle générale, si un contrôle particulier n’est pas pris en charge sur l’appareil actuel, vous devez désactiver ou masquer l’élément d’interface utilisateur qui permet à l’utilisateur d’activer la fonctionnalité.

Toutes les API de contrôle des appareils mentionnées dans cet article sont membres de l’espace de noms [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902).

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

> [!NOTE] 
> Cet article repose sur les concepts et le code décrits dans [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), qui décrit comment implémenter la capture photo et vidéo de base. Nous vous recommandons de vous familiariser avec le modèle de capture simple de contenu multimédia de cet article avant d’adopter des scénarios de capture plus avancés. Le code de cet article suppose que votre application possède déjà une instance de MediaCapture correctement lancée.

## <a name="hdr-video"></a>Vidéo HDR

La fonctionnalité vidéo HDR (High Dynamic Range) applique le traitement HDR au flux vidéo de l’appareil de capture. Déterminez si la vidéo HDR est prise en charge en vérifiant la propriété [**HdrVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926682).

Le contrôle vidéo HDR prend en charge trois modes: activé, désactivé et automatique, ce qui signifie que l’appareil détermine dynamiquement si le traitement vidéo HDR est susceptible d’améliorer la capture multimédia et l’active, le cas échéant. Pour déterminer si un mode particulier est pris en charge sur l’appareil actuel, vérifiez que la collection [**HdrVideoControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926683) contient le mode de votre choix.

Activez ou désactivez le traitement vidéo HDR en définissant le [**HdrVideoControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926681) sur le mode de votre choix.

[!code-cs[SetHdrVideoMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetHdrVideoMode)]

## <a name="exposure-priority"></a>Priorité d’exposition

Lorsqu’il est activé, le [**ExposurePriorityVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926644) évalue les images vidéo issues de l’appareil de capture pour déterminer si la vidéo capture une scène de faible éclairage. Dans ce cas, le contrôle réduit la fréquence d’images de la vidéo capturée pour augmenter le temps d’exposition pour chaque image et améliorer la qualité visuelle de la vidéo capturée.

Déterminez si le contrôle de priorité d’exposition est pris en charge sur l’appareil actuel en vérifiant la propriété [**ExposurePriorityVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926647).

Activez ou désactivez le contrôle de priorité d’exposition en définissant le [**ExposurePriorityVideoControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926646) sur le mode de votre choix.

[!code-cs[EnableExposurePriority](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEnableExposurePriority)]

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




