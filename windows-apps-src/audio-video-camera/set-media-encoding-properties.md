---
author: drewbatgit
ms.assetid: 09BA9250-A476-4803-910E-52F0A51704B1
description: "Cet article vous montre comment utiliser l’interface IMediaEncodingProperties pour définir la résolution et la fréquence d’images du flux d’aperçu de l’appareil photo, et des photos et vidéos capturées."
title: "Définir le format, la résolution et la fréquence d’images pour MediaCapture"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 8c8defd41ea1b65ac78d159b52eea926c7252e9e
ms.lasthandoff: 02/07/2017

---

# <a name="set-format-resolution-and-frame-rate-for-mediacapture"></a>Définir le format, la résolution et la fréquence d’images pour MediaCapture

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Cet article vous montre comment utiliser l’interface [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) pour définir la résolution et la fréquence d’images du flux d’aperçu de caméra et des photos et vidéos capturées. Il montre également comment s’assurer que les proportions du flux d’aperçu correspondent à celle du média capturé.

Les profils de caméra offrent un moyen plus avancé de détecter et de définir les propriétés de flux de l’appareil photo, mais ne sont pas pris en charge pour tous les appareils. Pour plus d’informations voir [Profils d’appareil photo](camera-profiles.md).

Le code figurant dans cet article a été adapté à partir de l’[exemple CameraResolution](http://go.microsoft.com/fwlink/p/?LinkId=624252&clcid=0x409). Vous pouvez télécharger l’exemple pour voir le code utilisé en contexte ou pour vous en servir comme point de départ pour votre propre application.

> [!NOTE] 
> Cet article repose sur les concepts et le code décrits dans [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), qui décrit comment implémenter la capture photo et vidéo de base. Nous vous recommandons de vous familiariser avec le modèle de capture multimédia de base dans cet article avant de passer à des scénarios de capture plus avancés. Le code de cet article repose sur l’hypothèse que votre application possède déjà une instance de MediaCapture initialisée correctement.

## <a name="a-media-encoding-properties-helper-class"></a>Classe d’assistance des propriétés d’encodage du média

La création d’une classe d’assistance simple englobant la fonctionnalité de l’interface [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) simplifie la sélection d’un ensemble de propriétés d’encodage qui répondent à des critères particuliers. Cette classe d’assistance est particulièrement utile en raison du comportement de la fonctionnalité des propriétés d’encodage suivant :

**Avertissement**  
La méthode [**VideoDeviceController.GetAvailableMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211994) utilise un membre de l’énumération [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/br226640), tel que **VideoRecord** ou **Photo**, et renvoie une liste d’objets [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) ou [**VideoEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701217) transmettant les paramètres d’encodage du flux, tels que la résolution de la photo ou de la vidéo capturée. Les résultats de l’appel de **GetAvailableMediaStreamProperties** peuvent inclure **ImageEncodingProperties** ou **VideoEncodingProperties** quelle que soit la valeur de **MediaStreamType** spécifiée. Pour cette raison, vous devez toujours vérifier le type de chaque valeur renvoyée et le convertir dans le type approprié avant d’essayer d’accéder à une des valeurs de propriété.

La classe d’assistance définie ci-dessous gère la vérification du type et son transtypage pour [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) ou [**VideoEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701217) afin que votre code d’application n’ait pas besoin de faire la distinction entre les deux types. En outre, pour ce faire, la classe d’assistance expose les propriétés pour les proportions des propriétés, la fréquence d’images (pour les propriétés d’encodage vidéo uniquement) et un nom convivial qui facilite l’affichage des propriétés d’encodage dans l’interface utilisateur de l’application.

Vous devez inclure l’espace de noms [**Windows.Media.MediaProperties**](https://msdn.microsoft.com/library/windows/apps/hh701296) dans le fichier source pour la classe d’assistance.

[!code-cs[MediaEncodingPropertiesUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMediaEncodingPropertiesUsing)]

[!code-cs[StreamPropertiesHelper](./code/BasicMediaCaptureWin10/cs/StreamPropertiesHelper.cs#SnippetStreamPropertiesHelper)]

## <a name="determine-if-the-preview-and-capture-streams-are-independent"></a>Déterminer si les flux de capture et d’aperçu sont indépendants

Sur certains appareils, le même code confidentiel de matériel est utilisé pour afficher et capturer des flux. Sur ces appareils, la définition des propriétés d’encodage d’un flux a pour effet de définir l’autre. Sur les appareils utilisant différents codes de matériel pour la capture et l’aperçu, il est possible de définir les propriétés de chaque flux de manière indépendante. Utilisez le code suivant pour déterminer si les flux de capture et d’aperçu sont indépendants. Vous devez ajuster votre interface utilisateur pour activer ou désactiver le paramètre des flux de manière indépendante en fonction du résultat de ce test.

[!code-cs[CheckIfStreamsAreIdentical](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCheckIfStreamsAreIdentical)]

## <a name="get-a-list-of-available-stream-properties"></a>Obtenir une liste des propriétés de flux disponibles

Obtenez une liste des propriétés de flux disponibles pour un appareil de capture en obtenant l’élément [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) pour l’objet [MediaCapture](capture-photos-and-video-with-mediacapture.md) de votre application, puis en appelant [**GetAvailableMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211994) et en transmettant l’une des valeurs de [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/br226640) : **VideoPreview**, **VideoRecord** ou **Photo**. Dans cet exemple, la syntaxe Linq est utilisée pour créer une liste d’objets **StreamPropertiesHelper**, définis précédemment dans cet article, pour chacune des valeurs [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) renvoyées par **GetAvailableMediaStreamProperties**. Cet exemple utilise d’abord les méthodes d’extension Linq pour classer les propriétés renvoyées tout d’abord en fonction de la résolution, puis de la fréquence d’images.

Si votre application exige une résolution ou une fréquence d’images spécifique, vous pouvez sélectionner un ensemble de propriétés d’encodage du média par programme. Une application de caméra classique expose plutôt la liste des propriétés disponibles dans l’interface utilisateur et permet à l’utilisateur de sélectionner les paramètres qu’il souhaite. Un **ComboBoxItem** est créé pour chaque élément dans la liste des objets **StreamPropertiesHelper** de la liste. Le contenu est défini sur le nom convivial renvoyé par la classe d’assistance, et la balise est définie sur la classe d’assistance elle-même pour une utilisation ultérieure à des fins de récupération des propriétés d’encodage associées. Chaque **ComboBoxItem** est ensuite ajouté à l’élément **ComboBox** transmis à la méthode.

[!code-cs[PopulateStreamPropertiesUI](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPopulateStreamPropertiesUI)]

## <a name="set-the-desired-stream-properties"></a>Définir les propriétés de flux souhaitées

Indiquez au contrôleur d’appareil vidéo d’utiliser les propriétés d’encodage voulues en appelant [**SetMediaStreamPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh700895) et en transmettant la valeur **MediaStreamType** indiquant si les propriétés de photo, de vidéo ou d’aperçu doivent être définies. Cet exemple définit les propriétés d’encodage demandées lorsque l’utilisateur sélectionne un élément dans l’un des objets **ComboBox** remplis à l’aide de la méthode d’assistance **PopulateStreamPropertiesUI**.

[!code-cs[PreviewSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewSettingsChanged)]

[!code-cs[PhotoSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhotoSettingsChanged)]

[!code-cs[VideoSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoSettingsChanged)]

## <a name="match-the-aspect-ratio-of-the-preview-and-capture-streams"></a>Faire correspondre les proportions des flux d’aperçu et de la capture

Une application de caméra classique fournira l’interface utilisateur permettant à l’utilisateur de sélectionner la résolution de capture vidéo ou photo, mais définira par programme la résolution de l’aperçu. Plusieurs stratégies différentes permettent de sélectionner la meilleure résolution de flux d’aperçu pour votre application :

-   Sélectionnez la résolution de l’aperçu la plus élevée possible, en laissant le soin à l’infrastructure d’interface utilisateur d’effectuer la mise à l’échelle nécessaire de l’aperçu.

-   Sélectionnez la résolution de l’aperçu la plus proche possible de la résolution de capture pour que l’aperçu affiche la représentation la plus fidèle du contenu multimédia capturé final.

-   Sélectionnez la résolution de l’aperçu la plus proche possible de la taille de [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) afin qu’aucun pixel en plus de ceux nécessaires ne soit transmis dans le pipeline du flux d’aperçu.

**Important**  
Il est possible, sur certains appareils, de définir des proportions différentes pour le flux d’aperçu de la caméra et le flux de la capture. Le rognage d’image causé par cette incompatibilité peut entraîner la présence de contenu dans le média capturé qui n’était pas visible dans l’aperçu, ce qui peut aboutir à une expérience négative pour l’utilisateur. Il est fortement recommandé d’utiliser les mêmes proportions, dans une faible plage de tolérance, pour les flux d’aperçu et de capture. Il est bon que des résolutions entièrement différentes soient activées pour la capture et l’aperçu tant que les proportions correspondent étroitement.


Pour vous assurer que les flux de capture de photo ou de vidéo correspondent aux proportions du flux d’aperçu, cet exemple appelle [**VideoDeviceController.GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995) et transmet la valeur enum **VideoPreview** pour demander les propriétés de flux actuelles du flux d’aperçu. Ensuite, une faible plage de tolérance des proportions est définie afin de pouvoir inclure des proportions qui ne sont pas exactement identiques au flux d’aperçu, tant qu’elles sont proches. Ensuite, une méthode d’extension Linq est utilisée pour sélectionner uniquement les objets **StreamPropertiesHelper** où les proportions sont comprises dans la plage de tolérance définie du flux d’aperçu.

[!code-cs[MatchPreviewAspectRatio](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMatchPreviewAspectRatio)]

 

 





