---
ms.assetid: D5D98044-7221-4C2A-9724-56E59F341AB0
description: Vous saurez comment lire et écrire des propriétés de métadonnées d’image et indiquer la position géographique de fichiers à l’aide de la classe GeotagHelper.
title: Métadonnées d’image
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 589dd40282fc3186f225e3873295863cc41b6a0c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361704"
---
# <a name="image-metadata"></a>Métadonnées d’image



Cet article montre comment lire et écrire des propriétés de métadonnées d’image et indiquer la position géographique de fichiers à l’aide de la classe utilitaire [**GeotagHelper**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.GeotagHelper).

## <a name="image-properties"></a>Propriétés d’image

La propriété [**StorageFile.Properties**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.properties) renvoie un objet [**StorageItemContentProperties**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.StorageItemContentProperties) qui donne accès aux informations relatives au contenu du fichier. Obtenez les propriétés spécifiques de l’image en appelant [**GetImagePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.storageitemcontentproperties.getimagepropertiesasync). L’objet [**ImageProperties**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.ImageProperties) renvoyé indique les membres qui contiennent des champs de métadonnées d’image de base, tels que le titre de l’image et la date de capture.

[!code-cs[GetImageProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetImageProperties)]

Pour accéder à un plus vaste ensemble de métadonnées de fichier, utilisez le système de propriétés Windows, qui est un ensemble de propriétés de métadonnées de fichier pouvant être récupéré à l’aide d’un identificateur de chaîne unique. Créez une liste de chaînes et ajoutez l’identificateur pour chaque propriété que vous souhaitez récupérer. La méthode [**ImageProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.imageproperties.retrievepropertiesasync) prend cette liste de chaînes et renvoie un dictionnaire de paires clé/valeur où la clé correspond à l’identificateur de propriété et la valeur à la valeur de propriété.

[!code-cs[GetWindowsProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetWindowsProperties)]

-   Pour obtenir la liste complète des propriétés Windows, y compris les identificateurs et le type de chaque propriété, voir [Propriétés Windows](https://docs.microsoft.com/windows/desktop/properties/props).

-   Certaines propriétés sont uniquement prises en charge pour certains conteneurs de fichiers et codecs d’image. Pour obtenir la liste des métadonnées d’image prises en charge pour chaque type d’image, voir [Stratégies de métadonnées de photos](https://docs.microsoft.com/windows/desktop/wic/photo-metadata-policies).

-   Étant donné que les propriétés qui ne sont pas prises en charge peuvent renvoyer une valeur null une fois récupérées, vérifiez toujours la présence d’une telle valeur avant d’utiliser une valeur de métadonnées renvoyée.

## <a name="geotag-helper"></a>Assistance d’indication de position géographique

GeotagHelper est une classe utilitaire qui facilite le balisage d’images à l’aide de données géographiques en utilisant directement les API [**Windows.Devices.Geolocation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation), sans nécessiter l’analyse ou la construction manuelle du format des métadonnées.

Si vous possédez déjà un objet [**Geopoint**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint) représentant l’emplacement que vous souhaitez baliser dans l’image, à partir d’une précédente utilisation d’API de géolocalisation ou d’une autre source, vous pouvez définir les données d’indication de la position géographique en appelant [**GeotagHelper.SetGeotagAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagasync) et en transmettant [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) et **Geopoint**.

[!code-cs[SetGeoDataFromPoint](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromPoint)]

Pour définir les données d’indication de la position géographique à l’aide de la localisation actuelle de l’appareil, créez un objet [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) et appelez [**GeotagHelper.SetGeotagFromGeolocatorAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync) en transmettant **Geolocator** et le fichier à baliser.

[!code-cs[SetGeoDataFromGeolocator](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromGeolocator)]

-   Vous devez inclure la fonctionnalité de **localisation** de l’appareil dans le manifeste de votre application afin d’utiliser l’API [**SetGeotagFromGeolocatorAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync).

-   Vous devez appeler [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) avant [**SetGeotagFromGeolocatorAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync) pour vous assurer que l’utilisateur a accordé à votre application l’autorisation d’utiliser sa localisation.

-   Pour plus d’informations sur les API de géolocalisation, voir [Cartes et emplacement](https://docs.microsoft.com/windows/uwp/maps-and-location/index).

Pour obtenir un GeoPoint indiquant la position géographique d’un fichier image, appelez [**GetGeotagAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.geotaghelper.getgeotagasync).

[!code-cs[GetGeoData](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetGeoData)]

## <a name="decode-and-encode-image-metadata"></a>Décoder et encoder des métadonnées d’image

La manière la plus avancée de travailler avec des données d’image consiste à lire et à écrire les propriétés au niveau de flux à l’aide d’une classe [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) ou d’un [BitmapEncoder](bitmapencoder-options-reference.md). Pour ces opérations, vous pouvez utiliser les propriétés de Windows pour spécifier les données en cours de lecture ou d’écriture, et également le langage de requête de métadonnées fourni par le composant Imagerie Windows (WIC) pour spécifier le chemin d’accès à une propriété demandée.

La lecture des métadonnées d’image à l’aide de cette technique nécessite de disposer d’un [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) qui a été créé avec le flux du fichier image source. Pour plus d’informations sur la façon de procéder, voir [Acquisition d’images](imaging.md).

Une fois que vous disposez du décodeur, créez une liste de chaînes et ajoutez une entrée pour chaque propriété de métadonnées que vous souhaitez récupérer, à l’aide de la chaîne d’identificateur de propriété Windows ou d’une requête de métadonnées WIC. Appelez la méthode [**BitmapPropertiesView.GetPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmappropertiesview.getpropertiesasync) sur le membre du décodeur [**BitmapProperties**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapProperties) pour demander les propriétés spécifiées. Les propriétés sont renvoyées dans un dictionnaire de paires clé/valeur contenant le nom de la propriété ou son chemin d’accès et sa valeur.

[!code-cs[ReadImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetReadImageMetadata)]

-   Pour plus d’informations sur le langage de requête de métadonnées WIC et les propriétés prises en charge, voir [Requêtes de métadonnées natives au format d’image WIC](https://docs.microsoft.com/windows/desktop/wic/-wic-native-image-format-metadata-queries).

-   De nombreuses propriétés de métadonnées sont uniquement prises en charge par un sous-ensemble de types d’images. [**GetPropertiesAsync** ](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmappropertiesview.getpropertiesasync) échoue avec le code d’erreur 0x88982F41 si l’une des propriétés demandées n’est pas pris en charge par l’image associée le décodeur et 0x88982F81 si l’image ne prend pas en charge les métadonnées à tous les. Les constantes associées à ces codes d’erreur sont WINCODEC\_ERR\_PROPERTYNOTSUPPORTED et WINCODEC\_ERR\_UNSUPPORTEDOPERATION et sont définies dans le fichier d’en-tête winerror.h.
-   Dans la mesure où une image peut contenir ou non une valeur pour une propriété particulière, utilisez **IDictionary.ContainsKey** pour vérifier la présence d’une propriété dans les résultats avant d’essayer d’y accéder.

L’écriture de métadonnées d’image dans le flux nécessite un élément **BitmapEncoder** associé au fichier de sortie image.

Créez un objet [**BitmapPropertySet**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet) destiné à contenir les valeurs de propriété que vous voulez définir. Créez un objet [**BitmapTypedValue**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapTypedValue) destiné à représenter la valeur de propriété. Cet objet utilise un **object** comme valeur et membre de l’énumération [**PropertyType**](https://docs.microsoft.com/uwp/api/Windows.Foundation.PropertyType) qui définit le type de valeur. Ajoutez l’élément **BitmapTypedValue** à **BitmapPropertySet**, puis appelez [**BitmapProperties.SetPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync) pour que l’encodeur écrive les propriétés dans le flux.

[!code-cs[WriteImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteImageMetadata)]

-   Pour obtenir plus d’informations sur les propriétés prises en charge par type de fichier image, voir [Propriétés Windows](https://docs.microsoft.com/windows/desktop/properties/props), [Stratégies de métadonnées de photos](https://docs.microsoft.com/windows/desktop/wic/photo-metadata-policies) et [Requêtes de métadonnées natives au format d’image WIC](https://docs.microsoft.com/windows/desktop/wic/-wic-native-image-format-metadata-queries).

-   [**SetPropertiesAsync** ](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync) échoue avec le code d’erreur 0x88982F41 si l’une des propriétés demandées n’est pas pris en charge par l’image associée à l’encodeur.

## <a name="related-topics"></a>Rubriques connexes

* [Création d’images](imaging.md)
 

 




