---
author: drewbatgit
ms.assetid: 42A06423-670F-4CCC-88B7-3DCEEDDEBA57
description: "Cet article explique comment utiliser les profils d’appareil photo pour découvrir et gérer les fonctionnalités des différents appareils de capture vidéo."
title: "Profils d’appareil photo"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 755b2747b2250c4ad19970095aed220551389471

---

# Profils d’appareil photo

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Cet article explique comment utiliser les profils d’appareil photo pour découvrir et gérer les fonctionnalités des différents appareils de capture vidéo.

**Remarque**  
Cet article repose sur les concepts et sur le code décrits dans [Capturer des photos et des vidéos à l’aide de MediaCapture](capture-photos-and-video-with-mediacapture.md), qui détaille les étapes d’implémentation de capture photo et vidéo de base. Il est recommandé de vous familiariser avec le modèle de capture multimédia de base dans cet article avant de passer à des scénarios de capture plus avancés. Le code de cet article part du principe que votre application possède déjà une instance de MediaCapture initialisée correctement.

 

## À propos des profils d’appareil photo

Les appareils photo offrent différentes fonctionnalités, notamment l’ensemble des résolutions de capture, la fréquence d’images pour la capture vidéo, et si les vidéos HDR ou les captures de fréquence d’images variable sont prises en charge. L’infrastructure de capture multimédia de plateforme Windows universelle (UWP) stocke cet ensemble de fonctionnalités dans une [**MediaCaptureVideoProfileMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926695). Un profil d’appareil photo, représenté par un objet [**MediaCaptureVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926694), comporte trois ensembles de descriptions du média : un premier pour la capture photo, un deuxième pour la capture vidéo et un dernier pour l’aperçu vidéo.

Avant d’initialiser votre objet [MediaCapture](capture-photos-and-video-with-mediacapture.md), vous pouvez interroger les appareils de capture de l’appareil actuel pour découvrir les profils pris en charge. Lorsque vous sélectionnez un profil pris en charge, vous savez que l’appareil de capture prend en charge toutes les fonctionnalités dans les descriptions du média du profil. Cela permet de ne pas tâtonner pour déterminer quelles combinaisons de fonctionnalités sont prises en charge sur un appareil particulier.

Dans l’article sur la capture multimédia de base, [Capturer des photos et des vidéos à l’aide de MediaCapture](capture-photos-and-video-with-mediacapture.md), le [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573) utilisé pour initialiser la capture multimédia est créé uniquement avec la chaîne d’ID de l’appareil de capture (quantité minimale de données requises pour l’initialisation).

[!code-cs[BasicInitExample](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetBasicInitExample)]

Les exemples de code dans cet article remplacent cette initialisation minimale par la découverte de profils d’appareil photo prenant en charge différentes fonctionnalités, qui sont ensuite utilisées pour initialiser l’appareil de capture multimédia.

## Trouver un appareil vidéo prenant en charge des profils d’appareil photo

Avant de rechercher des profils d’appareil photo pris en charge, vous devez trouver un appareil de capture qui prend en charge l’utilisation de profils d’appareil photo. La méthode d’assistance **GetVideoProfileSupportedDeviceIdAsync** définie dans l’exemple ci-dessous utilise la méthode [**DeviceInformaion.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) pour récupérer la liste de tous les appareils de capture vidéo disponibles. Elle parcourt tous les appareils de la liste en appelant la méthode statique [**IsVideoProfileSupported**](https://msdn.microsoft.com/library/windows/apps/dn926714) pour chaque appareil afin de déterminer s’il prend en charge les profils vidéo. En outre, la propriété [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) vous permet de spécifier pour chaque appareil si vous préférez que l’appareil photo se situe à l’avant ou à l’arrière.

Si un appareil prenant en charge des profils d’appareil photo est détecté dans le panneau de configuration spécifié, la valeur [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437) contenant la chaîne d’ID de l’appareil, est renvoyée.

[!code-cs[GetVideoProfileSupportedDeviceIdAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetVideoProfileSupportedDeviceIdAsync)]

Si l’ID d’appareil renvoyé par la méthode d’assistance **GetVideoProfileSupportedDeviceIdAsync** est null ou une chaîne vide, il n’existe aucun appareil sur le panneau de configuration spécifié prenant en charge des profils d’appareil photo. Dans ce cas, vous devez initialiser votre appareil de capture multimédia sans utiliser de profils.

[!code-cs[GetDeviceWithProfileSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetDeviceWithProfileSupport)]

## Sélectionner un profil en fonction de la résolution et de la fréquence d’images prises en charge

Pour sélectionner un profil avec des fonctionnalités particulières, comme la possibilité d’atteindre une résolution et une fréquence d’images données, vous devez d’abord appeler la méthode d’assistance définie ci-dessus pour obtenir l’ID d’un appareil de capture qui prend en charge l’utilisation des profils de l’appareil photo.

Créez un objet [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573), en passant l’ID d’appareil sélectionné. Ensuite, appelez la méthode statique [**MediaCapture.FindAllVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926708) pour obtenir la liste de tous les profils d’appareil photo pris en charge par l’appareil.

Cet exemple utilise une méthode de requête Linq, incluse dans l’espace de noms using **System.Linq**, pour sélectionner un profil qui contient un objet [**SupportedRecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926705) dans lequel les propriétés [**Width**](https://msdn.microsoft.com/library/windows/apps/dn926700), [**Height**](https://msdn.microsoft.com/library/windows/apps/dn926697) et [**FrameRate**](https://msdn.microsoft.com/library/windows/apps/dn926696) correspondent aux valeurs demandées. Si une correspondance est trouvée, [**VideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926679) et [**RecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926678) du **MediaCaptureInitializationSettings** sont définies sur les valeurs de type anonyme renvoyées par la requête Linq. Si aucune correspondance n’est trouvée, le profil par défaut est utilisé.

[!code-cs[FindWVGA30FPSProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindWVGA30FPSProfile)]

Une fois que vous avez rempli le **MediaCaptureInitializationSettings** avec le profil d’appareil photo souhaité, il vous suffit d’appeler [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) sur l’objet de capture multimédia pour le configurer sur le profil de votre choix.

[!code-cs[InitCaptureWithProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitCaptureWithProfile)]

## Sélectionner un profil prenant en charge la simultanéité

Vous pouvez utiliser des profils d’appareil photo pour déterminer si un appareil prend en charge la capture vidéo à partir de plusieurs appareils photo simultanément. Pour ce scénario, vous devez créer deux jeux d’objets de capture, un pour l’appareil photo avant et l’autre pour l’appareil photo arrière. Pour chaque appareil photo, créez une **MediaCapture**, un **MediaCaptureInitializationSettings** et une chaîne contenant l’ID d’appareil de capture. Ajoutez aussi une variable booléenne qui déterminera si la simultanéité est prise en charge.

[!code-cs[ConcurrencySetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConcurrencySetup)]

La méthode statique [**MediaCapture.FindConcurrentProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926709) renvoie la liste des profils d’appareil photo pris en charge par l’appareil de capture spécifié prenant en charge la simultanéité. Utilisez une requête Linq pour rechercher un profil qui prend en charge la simultanéité et qui est pris en charge par l’appareil photo avant et arrière. Si un profil répondant à ces exigences est trouvé, définissez le profil sur chacun des objets **MediaCaptureInitializationSettings** et définissez la variable booléenne de suivi de simultanéité sur la valeur True.

[!code-cs[FindConcurrencyDevices](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindConcurrencyDevices)]

Appelez **MediaCapture.InitializeAsync** pour l’appareil photo principal de votre scénario d’application. Si la simultanéité est prise en charge, initialisez également le deuxième appareil photo.

[!code-cs[InitConcurrentMediaCaptures](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitConcurrentMediaCaptures)]

## Utiliser des profils connus pour rechercher un profil qui prend en charge la vidéo HDR

La sélection d’un profil prenant en charge la vidéo HDR commence comme tous les autres scénarios. Créez un **MediaCaptureInitializationSettings** et une chaîne contenant l’ID d’appareil de capture. Ajoutez une variable booléenne qui déterminera si la vidéo HDR est prise en charge.

[!code-cs[GetHdrProfileSetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetHdrProfileSetup)]

Utilisez la méthode d’assistance **GetVideoProfileSupportedDeviceIdAsync** définie ci-dessus pour obtenir l’ID d’un appareil de capture prenant en charge les profils d’appareil photo.

[!code-cs[FindDeviceHDR](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindDeviceHDR)]

La méthode statique [**MediaCapture.FindKnownVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926710) renvoie les profils d’appareil photo pris en charge par l’appareil spécifié classé par la valeur [**KnownVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn948843) spécifiée. Pour ce scénario, la valeur **VideoRecording** est spécifiée de manière à limiter les profils d’appareil photo renvoyés à ceux prenant en charge l’enregistrement vidéo.

Parcourez la liste renvoyée des profils d’appareil photo. Pour chaque profil d’appareil photo, parcourez chaque [**VideoProfileMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926695) dans le profil pour vérifier si la propriété [**IsHdrVideoSupported**](https://msdn.microsoft.com/library/windows/apps/dn926698) est définie sur la valeur True. Une fois qu’une description du média appropriée est détectée, quittez la boucle et affectez le profil et les objets de description à l’objet **MediaCaptureInitializationSettings**.

[!code-cs[FindHDRProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindHDRProfile)]

## Déterminer si un appareil prend en charge la capture simultanée de photo et de vidéo

De nombreux appareils prennent en charge la capture simultanée de photos et de vidéos. Pour déterminer si un appareil de capture prend cette fonctionnalité en charge, appelez [**MediaCapture.FindAllVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926708) pour obtenir tous les profils d’appareil photo pris en charge. Utilisez une requête Linq pour rechercher un profil comportant au moins une entrée pour [**SupportedPhotoMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926703) et pour [**SupportedRecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926705), ce qui indiquera que le profil prend en charge la capture simultanée.

[!code-cs[GetPhotoAndVideoSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPhotoAndVideoSupport)]

Vous pouvez affiner la requête pour rechercher des profils qui prennent en charge des résolutions spécifiques ou autres fonctionnalités en plus de l’enregistrement vidéo simultané. Vous pouvez également utiliser le [**MediaCapture.FindKnownVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926710) et spécifier la valeur [**BalancedVideoAndPhoto**](https://msdn.microsoft.com/library/windows/apps/dn948843) pour récupérer les profils prenant en charge la capture simultanée, mais vous obtiendrez des résultats plus complets en interrogeant tous les profils.

## Rubriques connexes

* [Capturer des photos et des vidéos à l’aide de MediaCapture](capture-photos-and-video-with-mediacapture.md)
 

 







<!--HONumber=Jun16_HO4-->


