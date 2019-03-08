---
ms.assetid: D5D98044-7221-4C2A-9724-56E59F341AB0
description: Vous saurez comment lire et écrire des propriétés de métadonnées d’image et indiquer la position géographique de fichiers à l’aide de la classe GeotagHelper.
title: Métadonnées d’image
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2ab1279a8744d6dc9cddc88abaa064058f1259c2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631804"
---
# <a name="image-metadata"></a>Métadonnées d’image



Cet article montre comment lire et écrire des propriétés de métadonnées d’image et indiquer la position géographique de fichiers à l’aide de la classe utilitaire [**GeotagHelper**](https://msdn.microsoft.com/library/windows/apps/dn903683).

## <a name="image-properties"></a>Propriétés d’image

La propriété [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225) renvoie un objet [**StorageItemContentProperties**](https://msdn.microsoft.com/library/windows/apps/hh770642) qui donne accès aux informations relatives au contenu du fichier. Obtenez les propriétés spécifiques de l’image en appelant [**GetImagePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh770646). L’objet [**ImageProperties**](https://msdn.microsoft.com/library/windows/apps/br207718) renvoyé indique les membres qui contiennent des champs de métadonnées d’image de base, tels que le titre de l’image et la date de capture.

[!code-cs[GetImageProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetImageProperties)]

Pour accéder à un plus vaste ensemble de métadonnées de fichier, utilisez le système de propriétés Windows, qui est un ensemble de propriétés de métadonnées de fichier pouvant être récupéré à l’aide d’un identificateur de chaîne unique. Créez une liste de chaînes et ajoutez l’identificateur pour chaque propriété que vous souhaitez récupérer. La méthode [**ImageProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br207732) prend cette liste de chaînes et renvoie un dictionnaire de paires clé/valeur où la clé correspond à l’identificateur de propriété et la valeur à la valeur de propriété.

[!code-cs[GetWindowsProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetWindowsProperties)]

-   Pour obtenir la liste complète des propriétés Windows, y compris les identificateurs et le type de chaque propriété, voir [Propriétés Windows](https://msdn.microsoft.com/library/windows/desktop/dd561977).

-   Certaines propriétés sont uniquement prises en charge pour certains conteneurs de fichiers et codecs d’image. Pour obtenir la liste des métadonnées d’image prises en charge pour chaque type d’image, voir [Stratégies de métadonnées de photos](https://msdn.microsoft.com/library/windows/desktop/ee872003).

-   Étant donné que les propriétés qui ne sont pas prises en charge peuvent renvoyer une valeur null une fois récupérées, vérifiez toujours la présence d’une telle valeur avant d’utiliser une valeur de métadonnées renvoyée.

## <a name="geotag-helper"></a>Assistance d’indication de position géographique

GeotagHelper est une classe utilitaire qui facilite le balisage d’images à l’aide de données géographiques en utilisant directement les API [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603), sans nécessiter l’analyse ou la construction manuelle du format des métadonnées.

Si vous possédez déjà un objet [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675) représentant l’emplacement que vous souhaitez baliser dans l’image, à partir d’une précédente utilisation d’API de géolocalisation ou d’une autre source, vous pouvez définir les données d’indication de la position géographique en appelant [**GeotagHelper.SetGeotagAsync**](https://msdn.microsoft.com/library/windows/apps/dn903685) et en transmettant [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) et **Geopoint**.

[!code-cs[SetGeoDataFromPoint](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromPoint)]

Pour définir les données d’indication de la position géographique à l’aide de la localisation actuelle de l’appareil, créez un objet [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) et appelez [**GeotagHelper.SetGeotagFromGeolocatorAsync**](https://msdn.microsoft.com/library/windows/apps/dn903686) en transmettant **Geolocator** et le fichier à baliser.

[!code-cs[SetGeoDataFromGeolocator](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromGeolocator)]

-   Vous devez inclure la fonctionnalité de **localisation** de l’appareil dans le manifeste de votre application afin d’utiliser l’API [**SetGeotagFromGeolocatorAsync**](https://msdn.microsoft.com/library/windows/apps/dn903686).

-   Vous devez appeler [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) avant [**SetGeotagFromGeolocatorAsync**](https://msdn.microsoft.com/library/windows/apps/dn903686) pour vous assurer que l’utilisateur a accordé à votre application l’autorisation d’utiliser sa localisation.

-   Pour plus d’informations sur les API de géolocalisation, voir [Cartes et emplacement](https://msdn.microsoft.com/library/windows/apps/mt219699).

Pour obtenir un GeoPoint indiquant la position géographique d’un fichier image, appelez [**GetGeotagAsync**](https://msdn.microsoft.com/library/windows/apps/dn903684).

[!code-cs[GetGeoData](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetGeoData)]

## <a name="decode-and-encode-image-metadata"></a>Décoder et encoder des métadonnées d’image

La manière la plus avancée de travailler avec des données d’image consiste à lire et à écrire les propriétés au niveau de flux à l’aide d’une classe [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) ou d’un [BitmapEncoder](bitmapencoder-options-reference.md). Pour ces opérations, vous pouvez utiliser les propriétés de Windows pour spécifier les données en cours de lecture ou d’écriture, et également le langage de requête de métadonnées fourni par le composant Imagerie Windows (WIC) pour spécifier le chemin d’accès à une propriété demandée.

La lecture des métadonnées d’image à l’aide de cette technique nécessite de disposer d’un [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) qui a été créé avec le flux du fichier image source. Pour plus d’informations sur la façon de procéder, voir [Acquisition d’images](imaging.md).

Une fois que vous disposez du décodeur, créez une liste de chaînes et ajoutez une entrée pour chaque propriété de métadonnées que vous souhaitez récupérer, à l’aide de la chaîne d’identificateur de propriété Windows ou d’une requête de métadonnées WIC. Appelez la méthode [**BitmapPropertiesView.GetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226250) sur le membre du décodeur [**BitmapProperties**](https://msdn.microsoft.com/library/windows/apps/br226248) pour demander les propriétés spécifiées. Les propriétés sont renvoyées dans un dictionnaire de paires clé/valeur contenant le nom de la propriété ou son chemin d’accès et sa valeur.

[!code-cs[ReadImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetReadImageMetadata)]

-   Pour plus d’informations sur le langage de requête de métadonnées WIC et les propriétés prises en charge, voir [Requêtes de métadonnées natives au format d’image WIC](https://msdn.microsoft.com/library/windows/desktop/ee719904).

-   De nombreuses propriétés de métadonnées sont uniquement prises en charge par un sous-ensemble de types d’images. [**GetPropertiesAsync** ](https://msdn.microsoft.com/library/windows/apps/br226250) échoue avec le code d’erreur 0x88982F41 si l’une des propriétés demandées n’est pas pris en charge par l’image associée le décodeur et 0x88982F81 si l’image ne prend pas en charge les métadonnées à tous les. Les constantes associées à ces codes d’erreur sont WINCODEC\_ERR\_PROPERTYNOTSUPPORTED et WINCODEC\_ERR\_UNSUPPORTEDOPERATION et sont définies dans le fichier d’en-tête winerror.h.
-   Dans la mesure où une image peut contenir ou non une valeur pour une propriété particulière, utilisez **IDictionary.ContainsKey** pour vérifier la présence d’une propriété dans les résultats avant d’essayer d’y accéder.

L’écriture de métadonnées d’image dans le flux nécessite un élément **BitmapEncoder** associé au fichier de sortie image.

Créez un objet [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/hh974338) destiné à contenir les valeurs de propriété que vous voulez définir. Créez un objet [**BitmapTypedValue**](https://msdn.microsoft.com/library/windows/apps/hh700687) destiné à représenter la valeur de propriété. Cet objet utilise un **object** comme valeur et membre de l’énumération [**PropertyType**](https://msdn.microsoft.com/library/windows/apps/br225871) qui définit le type de valeur. Ajoutez l’élément **BitmapTypedValue** à **BitmapPropertySet**, puis appelez [**BitmapProperties.SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252) pour que l’encodeur écrive les propriétés dans le flux.

[!code-cs[WriteImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteImageMetadata)]

-   Pour obtenir plus d’informations sur les propriétés prises en charge par type de fichier image, voir [Propriétés Windows](https://msdn.microsoft.com/library/windows/desktop/dd561977), [Stratégies de métadonnées de photos](https://msdn.microsoft.com/library/windows/desktop/ee872003) et [Requêtes de métadonnées natives au format d’image WIC](https://msdn.microsoft.com/library/windows/desktop/ee719904).

-   [**SetPropertiesAsync** ](https://msdn.microsoft.com/library/windows/apps/br226252) échoue avec le code d’erreur 0x88982F41 si l’une des propriétés demandées n’est pas pris en charge par l’image associée à l’encodeur.

## <a name="related-topics"></a>Rubriques connexes

* [Création d’images](imaging.md)
 

 




