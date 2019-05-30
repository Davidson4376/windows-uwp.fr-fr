---
title: Effectuer un géocodage et un géocodage inverse
description: Ce guide vous montre comment convertir des adresses postales à des emplacements géographiques (géocodage) et de convertir des emplacements géographiques à des adresses postales (géocodage inverse) en appelant les méthodes de la classe MapLocationFinder dans l’espace de noms Windows.Services.Maps.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp, géocodage, carte, emplacement
ms.localizationpriority: medium
ms.openlocfilehash: 88687f6e7074ff7c914927a81a08720336b3c60c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371872"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>Effectuer un géocodage et un géocodage inverse

Ce guide vous montre comment convertir des adresses postales à des emplacements géographiques (géocodage) et convertir des emplacements géographiques des adresses postales (géocodage inverse) en appelant les méthodes de la [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) classe dans le [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) espace de noms.

> [!TIP]
> Pour en savoir plus sur l’utilisation de cartes dans votre application, téléchargez le [MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) exemple à partir de la [référentiel d’exemples Windows universels](h https://github.com/Microsoft/Windows-universal-samples) sur GitHub.

Les classes impliquées dans géocodage inversé et le géocodage sont organisées comme suit.

-   Le [ **MapLocationFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) classe contient des méthodes qui gèrent le géocodage ([**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)) et le géocodage (inversé[ **FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)).
-   Ces méthodes retournent un [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) instance.
-   Le [ **emplacements** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) propriété de la [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) expose une collection de [  **ISBNCarte** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) objets. 
-   [**ISBNCarte** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) objets ont tous deux un [ **adresse** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address) propriété, qui expose un [ **MapAddress** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress) objet représentant une adresse postale et un [ **Point** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point) propriété, qui expose un [ **Geopoint** ](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) objet qui représente un emplacement géographique.

> [!IMPORTANT]
> Vous devez spécifier une clé d’authentification maps avant de pouvoir utiliser les services de mappage. Pour plus d’informations, voir [Demander une clé d’authentification pour Cartes](authentication-key.md).

## <a name="get-a-location-geocode"></a>Obtenir un emplacement (géocode)

Cette section montre comment convertir une adresse postale ou un nom de lieu dans un emplacement géographique (géocodage).

1.  Appelez une des surcharges de la [ **FindLocationsAsync** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) méthode de la [ **MapLocationFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) classe avec un nom de lieu ou de la rue adresse.
2.  Le [ **FindLocationsAsync** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) méthode retourne un [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) objet.
3.  Utilisez le [ **emplacements** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) propriété de la [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) pour exposer une collection [  **ISBNCarte** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) objets. Il peut y avoir plusieurs [ **ISBNCarte** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) objets, car le système peut trouver plusieurs emplacements correspondant à l’entrée donnée.

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void geocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The address or business to geocode.
   string addressToGeocode = "Microsoft";

   // The nearby location to use as a query hint.
   BasicGeoposition queryHint = new BasicGeoposition();
   queryHint.Latitude = 47.643;
   queryHint.Longitude = -122.131;
   Geopoint hintPoint = new Geopoint(queryHint);

   // Geocode the specified address, using the specified reference point
   // as a query hint. Return no more than 3 results.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAsync(
                           addressToGeocode,
                           hintPoint,
                           3);

   // If the query returns results, display the coordinates
   // of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "result = (" +
            result.Locations[0].Point.Position.Latitude.ToString() + "," +
            result.Locations[0].Point.Position.Longitude.ToString() + ")";
   }
}
```

Ce code affiche les résultats suivants dans la zone de texte `tbOutputText`.

``` syntax
result = (47.6406099647284,-122.129339994863)
```

## <a name="get-an-address-reverse-geocode"></a>Obtenir une adresse (géocode inverse)

Cette section montre comment convertir un emplacement géographique à une adresse (géocodage inversé).

1.  Appelez la méthode [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) de la classe [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder).
2.  La méthode [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) renvoie un objet [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) contenant une collection d’objets [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) correspondants.
3.  Utilisez le [ **emplacements** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) propriété de la [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) pour exposer une collection [  **ISBNCarte** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) objets. Il peut y avoir plusieurs [ **ISBNCarte** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) objets, car le système peut trouver plusieurs emplacements correspondant à l’entrée donnée.
4.  Accès [ **MapAddress** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress) objets via le [ **adresse** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address) propriété de chaque [ **ISBNCarte** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation).

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void reverseGeocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The location to reverse geocode.
   BasicGeoposition location = new BasicGeoposition();
   location.Latitude = 47.643;
   location.Longitude = -122.131;
   Geopoint pointToReverseGeocode = new Geopoint(location);

   // Reverse geocode the specified geographic location.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAtAsync(pointToReverseGeocode);

   // If the query returns results, display the name of the town
   // contained in the address of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "town = " +
            result.Locations[0].Address.Town;
   }
}
```

Ce code affiche les résultats suivants dans la zone de texte `tbOutputText`.

``` syntax
town = Redmond
```

## <a name="related-topics"></a>Rubriques connexes

* [Exemple de carte UWP](https://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Exemple d’application de trafic UWP](https://go.microsoft.com/fwlink/p/?LinkId=619982)
* [Recommandations de conception pour les cartes](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [Vidéo : Utilisation de cartes et de la localisation sur un téléphone, une tablette et un PC dans vos applications Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Espace partenaires Bing Cartes](https://www.bingmapsportal.com/)
* [**MapLocationFinder** classe](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [**FindLocationsAsync** (méthode)](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [**FindLocationsAtAsync** (méthode)](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)
