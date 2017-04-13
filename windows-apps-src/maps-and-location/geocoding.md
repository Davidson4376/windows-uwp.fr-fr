---
author: PatrickFarley
title: "Effectuer un géocodage et un géocodage inverse"
description: "Convertissez des adresses en emplacements géographiques et vice versa (géocod. ou géocod. inverse) à l’aide de MapLocationFinder dans Windows.Services.Maps."
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows10, uwp, géocodage, carte, emplacement"
ms.openlocfilehash: a68898e86ad2e901499077e8f856c5318a7feae7
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>Effectuer un géocodage et un géocodage inverse


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Convertissez des adresses en emplacements géographiques (géocodage) et des emplacements géographiques en adresses (géocodage inverse) en appelant les méthodes de la classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) dans l’espace de noms [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979).

**Conseil** Pour en savoir plus sur l’utilisation de cartes dans votre application, téléchargez l’exemple suivant à partir du [référentiel Windows-universal-samples](http://go.microsoft.com/fwlink/p/?LinkId=619979) sur GitHub.

-   [Exemple de carte pour la plateforme Windows universelle (UWP, Universal Windows Platform)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

Les classes pour le géocodage et le géocodage inverse sont associées comme suit :

-   La classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) dispose de méthodes qui effectuent le géocodage ([**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)) et le géocodage inverse ([**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)).
-   Ces méthodes renvoient un [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551).
-   La classe [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) contient une collection d’objets [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549). Accédez à cette collection via la propriété [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552) du **MapLocationFinderResult**.
-   Chaque objet [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) contient un objet [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533). Accédez à cet objet via la propriété [**Address**](https://msdn.microsoft.com/library/windows/apps/dn636929) de chaque classe **MapLocation**.

**Important**  Vous devez spécifier une clé d’authentification pour Cartes avant de pouvoir utiliser les services de carte. Pour plus d’informations, voir [Demander une clé d’authentification pour Cartes](authentication-key.md).

 

## <a name="get-a-location-geocode"></a>Obtenir un emplacement (géocode)


Convertissez une adresse ou un nom de lieu en emplacement géographique (géocodage) en suivant les étapes ci-dessous.

1.  Appelez l’une des surcharges de la méthode [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) de la classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550).
2.  La méthode [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) renvoie un objet [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) contenant une collection d’objets [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) correspondants.
3.  Accédez à cette collection via la propriété [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552) de la classe [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551).

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


Convertissez un emplacement géographique en adresse (géocodage inverse) en procédant aux étapes suivantes.

1.  Appelez la méthode [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) de la classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550).
2.  La méthode [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) renvoie un objet [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) contenant une collection d’objets [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) correspondants.
3.  Accédez à cette collection via la propriété [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552) du [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551).
4.  Accédez à l’objet [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533) via la propriété [**Address**](https://msdn.microsoft.com/library/windows/apps/dn636929) de chaque [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549).

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

* [Centre de développement Bing Cartes](https://www.bingmapsportal.com/)
* [Exemple de carte UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Recommandations en matière de conception pour les cartes](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vidéos de la build 2015: utilisation des cartes et de la localisation sur un téléphone, une tablette et un PC dans vos applications Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemple d’application de trafic UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)
* [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)
* [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)
