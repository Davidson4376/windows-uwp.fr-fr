---
author: drewbatgit
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: Cet article vous montre comment utiliser les contrôles des appareils vidéo pour activer les scénarios de capture vidéo, y compris la vidéo HDR et la priorité de l’exposition.
title: Contrôles de l’appareil de capture pour la vidéo
---

# Contrôles de l’appareil de capture pour la vidéo

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Cet article vous montre comment utiliser les contrôles des appareils vidéo pour activer les scénarios de capture vidéo, y compris la vidéo HDR et la priorité de l’exposition.

Les contrôles des appareils vidéo mentionnés dans cet article sont tous ajoutés à votre application en utilisant le même modèle. Tout d’abord, vérifiez si le contrôle est pris en charge sur l’appareil sur lequel votre application est en cours d’exécution. Si le contrôle est pris en charge, définissez le mode de votre choix pour le contrôle. En règle générale, si un contrôle particulier n’est pas pris en charge sur l’appareil actuel, vous devez désactiver ou masquer l’élément d’interface utilisateur qui permet à l’utilisateur d’activer la fonctionnalité.

Toutes les API de contrôle des appareils mentionnées dans cet article sont membres de l’espace de noms [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902).

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

**Remarque**  
Cet article repose sur les concepts et sur le code décrits dans [Capturer des photos et des vidéos à l’aide de MediaCapture](capture-photos-and-video-with-mediacapture.md), qui détaille les étapes d’implémentation de capture photo et vidéo de base. Il est recommandé de vous familiariser avec le modèle de capture multimédia de base dans cet article avant de passer à des scénarios de capture plus avancés. Le code de cet article part du principe que votre application possède déjà une instance de MediaCapture initialisée correctement.

## Vidéo HDR

La fonctionnalité vidéo HDR (High Dynamic Range) applique le traitement HDR au flux vidéo de l’appareil de capture. Déterminez si la vidéo HDR est prise en charge en vérifiant la propriété [**HdrVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926682).

Le contrôle vidéo HDR prend en charge trois modes : activé, désactivé et automatique, ce qui signifie que l’appareil détermine dynamiquement si le traitement vidéo HDR est susceptible d’améliorer la capture multimédia et l’active, le cas échéant. Pour déterminer si un mode particulier est pris en charge sur l’appareil actuel, vérifiez que la collection [**HdrVideoControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926683) contient le mode de votre choix.

Activez ou désactivez le traitement vidéo HDR en définissant le [**HdrVideoControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926681) sur le mode de votre choix.

[!code-cs[SetHdrVideoMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetHdrVideoMode)]

## Priorité d’exposition

Lorsqu’il est activé, le [**ExposurePriorityVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926644) évalue les images vidéo issues de l’appareil de capture pour déterminer si la vidéo capture une scène de faible éclairage. Dans ce cas, le contrôle réduit la fréquence d’images de la vidéo capturée pour augmenter le temps d’exposition pour chaque image et améliorer la qualité visuelle de la vidéo capturée.

Déterminez si le contrôle de priorité d’exposition est pris en charge sur l’appareil actuel en vérifiant la propriété [**ExposurePriorityVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926647).

Activez ou désactivez le contrôle de priorité d’exposition en définissant le [**ExposurePriorityVideoControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926646) sur le mode de votre choix.

[!code-cs[EnableExposurePriority](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEnableExposurePriority)]

## Rubriques connexes

* [Capturer des photos et des vidéos à l’aide de MediaCapture](capture-photos-and-video-with-mediacapture.md)
 

 






<!--HONumber=May16_HO2-->


