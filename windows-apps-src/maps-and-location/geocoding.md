---
title: Effectuer un géocodage et un géocodage inverse
description: Ce guide vous montre comment convertissez des adresses en emplacements géographiques (géocodage) et emplacements géographiques en adresses rues (géocodage inverse) en appelant les méthodes de la classe MapLocationFinder dans Windows.Services.Maps.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: windows10, uwp, géocodage, carte, emplacement
ms.localizationpriority: medium
ms.openlocfilehash: e8b0efe39578974090844a4224055821c29f8ced
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8352403"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>Effectuer un géocodage et un géocodage inverse

Ce guide vous montre comment convertir les adresses en emplacements géographiques (géocodage) et convertir des emplacements géographiques en adresses rues (géocodage inverse) en appelant les méthodes de la classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) dans le [**Windows.Services.Maps **](https://msdn.microsoft.com/library/windows/apps/dn636979)espace de noms.

> [!TIP]
> Pour en savoir plus sur l’utilisation de cartes dans votre application, téléchargez l’exemple de [MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) à partir du [référentiel d’exemples universelle Windows](hhttps://github.com/Microsoft/Windows-universal-samples) sur GitHub.

Les classes qui entrent en jeu dans le géocodage et géocodage inverse sont organisés comme suit.

-   La classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) contient des méthodes qui gèrent les géocodage ([**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)) et un géocodage ([**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)) inverse.
-   Les deux de ces méthodes retourne une instance de [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) .
-   La propriété [**d’emplacements**](https://msdn.microsoft.com/library/windows/apps/dn627552) de la [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) expose une collection d’objets [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) . 
-   Objets [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) ont une propriété [**d’adresse**](https://msdn.microsoft.com/library/windows/apps/dn636929) , qui expose un objet [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533) représentant une adresse et une propriété de [**Point**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point) , qui expose un objet [**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) représentant un emplacement géographique.

> [!IMPORTANT]
> Vous devez spécifier une clé d’authentification pour Cartes avant de pouvoir utiliser les services de carte. Pour plus d’informations, voir [Demander une clé d’authentification de cartes](authentication-key.md).

## <a name="get-a-location-geocode"></a>Obtenir un emplacement (géocode)

Cette section montre comment convertir une adresse ou un nom de lieu en emplacement géographique (géocodage).

1.  Appelez l’une des surcharges de la méthode [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) de la classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) avec un nom de lieu ou de la rue.
2.  La méthode [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) renvoie un objet [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) .
3.  Utilisez la propriété [**d’emplacements**](https://msdn.microsoft.com/library/windows/apps/dn627552) de l' [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) à exposer une collection des objets [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) . Il peut exister plusieurs objets [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) dans la mesure où le système peut rechercher plusieurs emplacements qui correspondent à l’entrée donnée.

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

Cette section montre comment convertir un emplacement géographique en adresse (géocodage inverse).

1.  Appelez la méthode [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) de la classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550).
2.  La méthode [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) renvoie un objet [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) contenant une collection d’objets [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) correspondants.
3.  Utilisez la propriété [**d’emplacements**](https://msdn.microsoft.com/library/windows/apps/dn627552) de l' [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) à exposer une collection des objets [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) . Il peut exister plusieurs objets [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) dans la mesure où le système peut rechercher plusieurs emplacements qui correspondent à l’entrée donnée.
4.  Accéder aux objets [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533) par le biais de la propriété [**d’adresse**](https://msdn.microsoft.com/library/windows/apps/dn636929) de chaque [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549).

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

* [Exemple de carte UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Exemple d’application de trafic UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [Recommandations en matière de conception pour les cartes](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vidéo: Tirant parti des cartes et emplacement sur un téléphone, tablettes et PC dans vos applications Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Centre de développement Bing Cartes](https://www.bingmapsportal.com/)
* [Classe **MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)
* [Méthode **FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)
* [Méthode **FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)
