---
author: msatranjr
title: Afficher des cartes avec des vues 2D, 3D et Streetside
description: "Affichez des cartes personnalisables dans votre application en utilisant la classe MapControl. Cette rubrique présente également des vues 3D aériennes et Streetside."
ms.assetid: 3839E00B-2C1E-4627-A45F-6DDA98D7077F
ms.sourcegitcommit: 92285ce32548bd6035c105e35c2b152432f8575a
ms.openlocfilehash: 249503f6a43ef8c38e76ed29aed4a1bfdb26e9fb

---

# Afficher des cartes avec des vues 2D, 3D et Streetside


\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Affichez des cartes personnalisables dans votre application en utilisant la classe [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004). Cette rubrique présente également des vues 3D aériennes et Streetside.

**Conseil** Pour en savoir plus sur l’utilisation de cartes dans votre application, téléchargez l’exemple suivant à partir du [référentiel Windows-universal-samples](http://go.microsoft.com/fwlink/p/?LinkId=619979) sur GitHub.

-   [Exemple de carte pour la plateforme Windows universelle (UWP, Universal Windows Platform)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## Ajouter le contrôle de carte à votre application


Affichez une carte sur une page XAML en ajoutant un élément [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004). Pour utiliser le **MapControl**, vous devez déclarer les espace de noms [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751) dans la page XAML ou dans votre code. Si vous faites glisser le contrôle à partir de la boîte à outils, cette déclaration d’espace de noms est ajoutée automatiquement. Si vous ajoutez manuellement le **MapControl** à la page XAML, vous devez également ajouter manuellement la déclaration d’espace de noms en haut de la page.

L’exemple suivant affiche un contrôle de carte de base et configure la carte pour afficher les contrôles de zoom et d’inclinaison en plus de l’acceptation des entrées tactiles. Pour plus d’informations sur la personnalisation de l’apparence de la carte, voir [Configurer la carte](#mapconfig).

```xml
<Page
    x:Class="MapsAndLocation1.DisplayMaps"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MapsAndLocation1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:Maps="using:Windows.UI.Xaml.Controls.Maps"
    mc:Ignorable="d">

 <Grid x:Name="pageGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Maps:MapControl
       x:Name="MapControl1"            
       ZoomInteractionMode="GestureAndControl"
       TiltInteractionMode="GestureAndControl"   
       MapServiceToken="EnterYourAuthenticationKeyHere"/>
  
 </Grid>
</Page>
```

Si vous ajoutez le contrôle de carte dans votre code, vous devez déclarer manuellement l’espace de noms en haut du fichier de code.

```csharp
using Windows.UI.Xaml.Controls.Maps;
...

// Add the MapControl and the specify maps authentication key.
MapControl MapControl2 = new MapControl();
MapControl2.ZoomInteractionMode = MapInteractionMode.GestureAndControl;
MapControl2.TiltInteractionMode = MapInteractionMode.GestureAndControl;
MapControl2.MapServiceToken = "EnterYourAuthenticationKeyHere";
pageGrid.Children.Add(MapControl2);
```

## Demander et définir une clé d’authentification de cartes


Avant d’utiliser les services de carte et [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004), vous devez spécifier la clé d’authentification de cartes en tant que valeur de la propriété [**MapServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn637036). Dans les exemples précédents, remplacez `EnterYourAuthenticationKeyHere` par la clé que vous recevez du [Centre de développement Bing Cartes](https://www.bingmapsportal.com/). Le texte **Avertissement : MapServiceToken non spécifié** continue d’apparaître sous le contrôle jusqu’à ce que vous spécifiiez la clé d’authentification de cartes. Pour plus d’informations sur l’obtention et la définition d’une clé d’authentification de cartes, voir [Demander une clé d’authentification de cartes](authentication-key.md).

## Définir un emplacement de départ pour la carte


Définissez l’emplacement à afficher sur la carte en spécifiant la propriété [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005) du [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) dans votre code, ou en liant la propriété dans votre balisage XAML. L’exemple suivant représente une carte centrée sur la ville de Seattle, aux États-Unis.

**Conseil** Dans la mesure où une chaîne ne peut pas être convertie en valeur [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675), vous ne pouvez pas spécifier une valeur pour la propriété [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005) dans le balisage XAML, sauf si vous utilisez la fonction de liaison de données. (Cette limitation s’applique également à la propriété [**MapControl.Location**](https://msdn.microsoft.com/library/windows/apps/dn653264) jointe.)

 

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
   // Specify a known location.
   BasicGeoposition cityPosition = new BasicGeoposition() { Latitude = 47.604, Longitude = -122.329 };
   Geopoint cityCenter = new Geopoint(cityPosition);

   // Set the map location.
   MapControl1.Center = cityCenter;
   MapControl1.ZoomLevel = 12;
   MapControl1.LandmarksVisible = true;
}
```

![exemple du contrôle de carte.](images/displaymapsexample1.png)

## Définir l’emplacement actuel de la carte


Avant d’accéder à l’emplacement de l’utilisateur, votre application doit appeler la méthode [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152). À ce stade, votre application doit être au premier plan et l’élément **RequestAccessAsync** doit être appelé à partir du thread d’interface utilisateur. Jusqu’à ce que l’utilisateur l’y autorise, votre application ne peut pas accéder aux données d’emplacement.

Utilisez la méthode [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) de la classe [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) pour obtenir l’emplacement actuel de l’appareil (si la localisation est disponible). Pour obtenir le [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675) correspondant, utilisez la propriété [**Point**](https://msdn.microsoft.com/library/windows/apps/dn263665) des coordonnées longitudinale et latitudinale de la position géographique. Pour plus d’informations, voir [Obtenir l’emplacement actuel](get-location.md).

```csharp
// Set your current location.
var accessStatus = await Geolocator.RequestAccessAsync();
switch (accessStatus)
{
   case GeolocationAccessStatus.Allowed:

      // Get the current location.
      Geolocator geolocator = new Geolocator();
      Geoposition pos = await geolocator.GetGeopositionAsync();
      Geopoint myLocation = pos.Coordinate.Point;

      // Set the map location.
      MapControl1.Center = myLocation;
      MapControl1.ZoomLevel = 12;
      MapControl1.LandmarksVisible = true;
      break;

   case GeolocationAccessStatus.Denied:
      // Handle the case  if access to location is denied.
      break;

   case GeolocationAccessStatus.Unspecified:
      // Handle the case if  an unspecified error occurs.
      break;
}
```

Lorsque vous affichez l’emplacement de votre appareil sur une carte, songez à afficher les graphiques et à définir le niveau de zoom en fonction de la précision des données d’emplacement. Pour plus d’informations, voir [Recommandations sur les applications de géolocalisation](https://msdn.microsoft.com/library/windows/apps/hh465148).

## Modifier l’emplacement de la carte


Pour modifier l’emplacement qui s’affiche sur une carte 2D, appelez l’une des surcharges de la méthode [**TrySetViewAsync**](https://msdn.microsoft.com/library/windows/apps/dn637060). Cette méthode permet de spécifier de nouvelles valeurs pour [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005), [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068), [**Heading**](https://msdn.microsoft.com/library/windows/apps/dn637019) et [**Pitch**](https://msdn.microsoft.com/library/windows/apps/dn637044). Vous pouvez également spécifier une animation facultative à utiliser quand l’affichage est modifié en fournissant une constante de l’énumération [**MapAnimationKind**](https://msdn.microsoft.com/library/windows/apps/dn637002).

Pour modifier l’emplacement sur une carte 3D, utilisez plutôt la méthode [**TrySetSceneAsync**](https://msdn.microsoft.com/library/windows/apps/dn974296). Pour plus d’informations, voir [Afficher des vues 3D](#display3d).

Appelez la méthode [**TrySetViewBoundsAsync**](https://msdn.microsoft.com/library/windows/apps/dn637065) pour afficher le contenu d’une classe [**GeoboundingBox**](https://msdn.microsoft.com/library/windows/apps/dn607949) sur la carte. Utilisez cette méthode (par exemple) pour afficher un itinéraire ou une partie d’un itinéraire sur la carte. Pour plus d’informations, voir [Afficher des itinéraires et indications sur une carte](routes-and-directions.md).

## Configurer la carte


Configurez la carte et son apparence en définissant les valeurs des propriétés suivantes de la classe [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

**Paramètres de la carte**

-   Affectez au **centre** de la carte un point géographique en définissant la propriété [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005).
-   Définissez le **niveau de zooml** de la carte en affectant à la propriété [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) une valeur comprise entre 1 et 20.
-   Définissez la **rotation** de la carte en affectant une valeur à la propriété [**Heading**](https://msdn.microsoft.com/library/windows/apps/dn637019) (dans laquelle une valeur de 0 ou 360 degrés indique le nord ; une valeur de 90 signale l’est ; une valeur de 180 représente le sud et une valeur de 270, l’ouest).
-   Définissez une **inclinaison** pour la carte en affectant à la propriété [**DesiredPitch**](https://msdn.microsoft.com/library/windows/apps/dn637012) une valeur comprise entre 0 et 65 degrés.

**Apparence de la carte**

-   Spécifiez le **type** de carte, par exemple une carte routière ou une vue aérienne, en définissant la propriété [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) avec l’une des constantes [**MapStyle**](https://msdn.microsoft.com/library/windows/apps/dn637127).
-   Définissez le **modèle de couleurs** de la carte comme étant clair ou foncé en définissant la propriété [**ColorScheme**](https://msdn.microsoft.com/library/windows/apps/dn637010) avec l’une des constantes [**MapColorScheme**](https://msdn.microsoft.com/library/windows/apps/dn637003).

Affichez des informations sur la carte en définissant les valeurs des propriétés suivantes de la classe [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

-   Affichez des **bâtiments et repères** sur la carte en activant ou en désactivant la propriété [**LandmarksVisible**](https://msdn.microsoft.com/library/windows/apps/dn637023).
-   Affichez des **structures piétonnes**, par exemple des escaliers publics, sur la carte en activant ou en désactivant la propriété [**PedestrianFeaturesVisible**](https://msdn.microsoft.com/library/windows/apps/dn637042).
-   Affichez le **trafic** sur la carte en activant ou en désactivant la propriété [**TrafficFlowVisible**](https://msdn.microsoft.com/library/windows/apps/dn637055).
-   Indiquez si le **filigrane** est affiché sur la carte en définissant la propriété [**WatermarkMode**](https://msdn.microsoft.com/library/windows/apps/dn637066) sur l’une des constantes [**MapWatermarkMode**](https://msdn.microsoft.com/library/windows/apps/dn610749).
-   Pour afficher une **route ou une voie piétonne** sur la carte, ajoutez un élément [**MapRouteView**](https://msdn.microsoft.com/library/windows/apps/dn637122) à la collection [**Routes**](https://msdn.microsoft.com/library/windows/apps/dn637047) du contrôle de carte. Pour plus d’informations et un exemple, voir [Afficher des itinéraires et indications sur une carte](routes-and-directions.md).

Pour plus d’informations sur l’affichage de punaises, de formes et de contrôles XAML dans le [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004), voir [Afficher des centres d’intérêt sur une carte](display-poi.md).

## Afficher des vues Streetside


Une vue Streetside est une perspective au niveau rue d’un emplacement qui s’affiche au-dessus du contrôle de carte.

![exemple de vue Streetside du contrôle de carte.](images/onlystreetside-730width.png)

Considérez l’expérience « à l’intérieur » de la vue Streetside comme distincte de la carte affichée à l’origine dans le contrôle de carte. Par exemple, la modification de l’emplacement dans la vue Streetside ne change pas l’emplacement ou l’apparence de la carte « sous » la vue Streetside. Après avoir fermé la vue Streetside (en cliquant sur le **X** dans l’angle supérieur droit du contrôle), la carte d’origine reste inchangée.

Pour afficher une vue Streetside

1.  Déterminez si les vues Streetside sont prises en charge sur l’appareil en vérifiant [**IsStreetsideSupported**](https://msdn.microsoft.com/library/windows/apps/dn974271).
2.  Si la vue Streetside est prise en charge, créez un [**StreetsidePanorama**](https://msdn.microsoft.com/library/windows/apps/dn974360) près de l’emplacement spécifié en appelant [**FindNearbyAsync**](https://msdn.microsoft.com/library/windows/apps/dn974361).
3.  Déterminez si un panorama à proximité a été trouvé en vérifiant que la valeur [**StreetsidePanorama**](https://msdn.microsoft.com/library/windows/apps/dn974360) n’est pas null.
4.  Si un panorama à proximité a été trouvé, créez une [**StreetsideExperience**](https://msdn.microsoft.com/library/windows/apps/dn974356) pour la propriété [**CustomExperience**](https://msdn.microsoft.com/library/windows/apps/dn974263) du contrôle de carte.

Cet exemple montre comment afficher une vue Streetside similaire à l’image précédente.

**Remarque** La vue d’ensemble n’apparaît pas si le contrôle de carte est trop petit.

 

```csharp
private async void showStreetsideView()
{
   // Check if Streetside is supported.
   if (MapControl1.IsStreetsideSupported)
   {
      // Find a panorama near Avenue Gustave Eiffel.
      BasicGeoposition cityPosition = new BasicGeoposition() { Latitude = 48.858, Longitude = 2.295};
      Geopoint cityCenter = new Geopoint(cityPosition);
      StreetsidePanorama panoramaNearCity = await StreetsidePanorama.FindNearbyAsync(cityCenter);

      // Set the Streetside view if a panorama exists.
      if (panoramaNearCity != null)
      {
         // Create the Streetside view.
         StreetsideExperience ssView = new StreetsideExperience(panoramaNearCity);
         ssView.OverviewMapVisible = true;
         MapControl1.CustomExperience = ssView;
      }
   }
   else
   {
      // If Streetside is not supported
      ContentDialog viewNotSupportedDialog = new ContentDialog()
      {
         Title = "Streetside is not supported",
         Content ="\nStreetside views are not supported on this device.",
         PrimaryButtonText = "OK"
      };
      await viewNotSupportedDialog.ShowAsync();            
   }
}
```

## Afficher des vues 3D aériennes


La classe [**MapScene**](https://msdn.microsoft.com/library/windows/apps/dn974329) vous permet de spécifier une perspective 3D de la carte. La scène représente la vue 3D qui s’affiche dans la carte. La classe [**MapCamera**](https://msdn.microsoft.com/library/windows/apps/dn974244) représente la position d’un appareil photo qui afficherait une telle vue.

![](images/mapcontrol-techdiagram.png)

Pour faire en sorte que des bâtiments et d’autres éléments à la surface de la carte s’affichent en 3D, définissez la propriété [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) du contrôle de carte sur [**MapStyle.Aerial3DWithRoads**](https://msdn.microsoft.com/library/windows/apps/dn637127). Voici un exemple de vue 3D avec le style **Aerial3DWithRoads**.

![exemple de vue de carte 3D.](images/only3d-730width.png)

Pour afficher une vue 3D

1.  Déterminez si les vues 3D sont prises en charge sur l’appareil en vérifiant [**Is3DSupported**](https://msdn.microsoft.com/library/windows/apps/dn974265).
2.  Si les vues 3D sont prises en charge, définissez la propriété [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) du contrôle de carte sur [**MapStyle.Aerial3DWithRoads**](https://msdn.microsoft.com/library/windows/apps/dn637127).
3.  Créez un objet [**MapScene**](https://msdn.microsoft.com/library/windows/apps/dn974329) en utilisant l’une des nombreuses méthodes **CreateFrom**, telles que [**CreateFromLocationAndRadius**](https://msdn.microsoft.com/library/windows/apps/dn974336) et [**CreateFromCamera**](https://msdn.microsoft.com/library/windows/apps/dn974334).
4.  Appelez [**TrySetSceneAsync**](https://msdn.microsoft.com/library/windows/apps/dn974296) pour afficher la vue 3D. Vous pouvez également spécifier une animation facultative à utiliser quand l’affichage est modifié en fournissant une constante de l’énumération [**MapAnimationKind**](https://msdn.microsoft.com/library/windows/apps/dn637002).

Cet exemple montre comment afficher une vue 3D.

```csharp
private async void display3DLocation()
{
   if (MapControl1.Is3DSupported)
   {
      // Set the aerial 3D view.
      MapControl1.Style = MapStyle.Aerial3DWithRoads;

      // Specify the location.
      BasicGeoposition hwGeoposition = new BasicGeoposition() { Latitude = 34.134, Longitude = -118.3216};
      Geopoint hwPoint = new Geopoint(hwGeoposition);

      // Create the map scene.
      MapScene hwScene = MapScene.CreateFromLocationAndRadius(hwPoint,
                                                                           80, /* show this many meters around */
                                                                           0, /* looking at it to the North*/
                                                                           60 /* degrees pitch */);
      // Set the 3D view with animation.
      await MapControl1.TrySetSceneAsync(hwScene,MapAnimationKind.Bow);
   }
   else
   {
      // If 3D views are not supported, display dialog.
      ContentDialog viewNotSupportedDialog = new ContentDialog()
      {
         Title = "3D is not supported",
         Content = "\n3D views are not supported on this device.",
         PrimaryButtonText = "OK"
      };
      await viewNotSupportedDialog.ShowAsync();
   }
}
```

## Obtenir des informations sur les emplacements et les éléments


Obtenez des informations sur les emplacements sur la carte en appelant les méthodes suivantes du [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

-   Méthode [**GetLocationFromOffset**](https://msdn.microsoft.com/library/windows/apps/dn637016) : permet d’obtenir l’emplacement géographique qui correspond au point spécifié dans la fenêtre d’affichage du contrôle de carte.
-   Méthode [**GetOffsetFromLocation**](https://msdn.microsoft.com/library/windows/apps/dn637018) : permet d’obtenir le point dans la fenêtre d’affichage du contrôle de carte qui correspond à l’emplacement géographique indiqué.
-   Méthode [**IsLocationInView**](https://msdn.microsoft.com/library/windows/apps/dn637022) : permet de déterminer si l’emplacement géographique indiqué est actuellement visible dans la fenêtre d’affichage du contrôle de carte.
-   Méthode [**FindMapElementsAtOffset**](https://msdn.microsoft.com/library/windows/apps/dn637014) : permet d’obtenir les éléments de la carte situés au niveau du point spécifié dans la fenêtre d’affichage du contrôle de carte.

## Gérer les interactions et modifications de l’utilisateur


Gérez les mouvements d’entrée utilisateur sur la carte en traitant les événements suivants de l’élément [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004). Vérifiez les valeurs des propriétés [**Location**](https://msdn.microsoft.com/library/windows/apps/dn637091) et [**Position**](https://msdn.microsoft.com/library/windows/apps/dn637093) de l’élément [**MapInputEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637090) pour obtenir des informations sur l’emplacement géographique sur la carte et l’emplacement physique dans la fenêtre d’affichage où le mouvement s’est produit.

-   [**MapTapped**](https://msdn.microsoft.com/library/windows/apps/dn637038)
-   [**MapDoubleTapped**](https://msdn.microsoft.com/library/windows/apps/dn637032)
-   [**MapHolding**](https://msdn.microsoft.com/library/windows/apps/dn637035)

Déterminez si la carte est en cours de chargement ou entièrement chargée en gérant l’événement [**LoadingStatusChanged**](https://msdn.microsoft.com/library/windows/apps/dn637028) du contrôle.

Gérez les modifications qui se produisent lorsque l’utilisateur ou l’application modifie les paramètres de la carte en gérant les événements suivants de l’élément [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004). [Recommandations sur les cartes](https://msdn.microsoft.com/library/windows/apps/dn596102)

-   [**CenterChanged**](https://msdn.microsoft.com/library/windows/apps/dn637006)
-   [**HeadingChanged**](https://msdn.microsoft.com/library/windows/apps/dn637020)
-   [**PitchChanged**](https://msdn.microsoft.com/library/windows/apps/dn637045)
-   [**ZoomLevelChanged**](https://msdn.microsoft.com/library/windows/apps/dn637069)

## Rubriques connexes

* [Centre de développement Bing Cartes](https://www.bingmapsportal.com/)
* [Exemple de carte UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Obtenir l’emplacement actuel](get-location.md)
* [Recommandations en matière de conception pour les applications prenant en charge la géolocalisation](https://msdn.microsoft.com/library/windows/apps/hh465148)
* [Recommandations en matière de conception pour les cartes](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vidéos de la build 2015 : utilisation des cartes et de la localisation sur un téléphone, une tablette et un PC dans vos applications Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemple d’application de trafic UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)





<!--HONumber=Jun16_HO4-->


