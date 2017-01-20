---
author: msatranjr
title: "Afficher les points d’intérêt sur une carte"
description: "Ajoutez des points d’intérêt à une carte à l’aide des punaises, des images, des formes et des éléments d’interface utilisateur XAML."
ms.assetid: CA00D8EB-6C1B-4536-8921-5EAEB9B04FCA
translationtype: Human Translation
ms.sourcegitcommit: d00ba80ac7d0f033a69ad070dc8ee681cbd0ed18
ms.openlocfilehash: 8afdb41d6790bb9647a6b89086c4b86872940c51

---

# <a name="display-points-of-interest-poi-on-a-map"></a>Afficher des centres d’intérêt sur une carte


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Ajoutez des points d’intérêt à une carte à l’aide des punaises, des images, des formes et des éléments d’interface utilisateur XAML. Un point d’intérêt est un point spécifique sur la carte, qui représente un élément intéressant. Il peut s’agir, par exemple, de l’emplacement d’une entreprise, d’une localité ou d’un ami.

**Conseil** Pour en savoir plus sur l’affichage des points d’intérêt dans votre application, téléchargez l’exemple suivant à partir du [référentiel Windows-universal-samples](http://go.microsoft.com/fwlink/p/?LinkId=619979) sur GitHub.

-   [Exemple de carte pour la plateforme Windows universelle (UWP, Universal Windows Platform)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

Affichez des punaises, des images et des formes sur la carte en ajoutant des objets [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077), [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103) et [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114) à la collection [**MapElements**](https://msdn.microsoft.com/library/windows/apps/dn637033) du contrôle de carte. Utilisez la liaison de données ou ajoutez des éléments par programmation. Vous ne pouvez pas lier à la collection **MapElements** de façon déclarative dans votre balisage XAML.

Affichez des éléments de l’interface utilisateur XAML ([**Button**](https://msdn.microsoft.com/library/windows/apps/br209265), [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) ou [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)) sur la carte en les ajoutant en tant qu’éléments [**Children**](https://msdn.microsoft.com/library/windows/apps/dn637008) de la classe [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004). Vous pouvez également les ajouter à la classe [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094), ou lier la classe **MapItemsControl** à une collection d’éléments.

En résumé :

-   [Ajoutez un élément MapIcon à la carte](#mapicon) pour afficher une image telle qu’une punaise avec un texte facultatif.
-   [Ajoutez un élément MapPolygon à la carte](#mappolygon) pour afficher une forme multipoint.
-   [Ajoutez un élément MapPolyline à la carte](#mappolyline) pour afficher des lignes sur la carte.
-   [Ajoutez du code XAML à la carte](#mapxaml) pour afficher des éléments d’interface utilisateur personnalisés.

Si vous avez un grand nombre d’éléments à placer sur la carte, songez à [superposer des images sous forme de vignettes à la carte](overlay-tiled-images.md). Pour afficher des routes sur la carte, voir [Afficher des itinéraires et indications](routes-and-directions.md).

## <a name="add-a-mapicon"></a>Ajouter un élément MapIcon


Affichez une image comme une punaise, avec un texte facultatif, sur la carte à l’aide de la classe [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077). Vous pouvez accepter l’image par défaut ou fournir une image personnalisée à l’aide de la propriété [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078). L’image suivante affiche l’image par défaut pour un élément **MapIcon**, sans qu’aucune valeur ne soit spécifiée pour la propriété [**Title**](https://msdn.microsoft.com/library/windows/apps/dn637088), avec un titre court, un autre long et un autre très long.

![exemple de classe MapIcon présentant des titres de différentes longueurs.](images/mapctrl-mapicons.png)

L’exemple suivant représente une carte de la ville de Seattle et ajoute une classe [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) avec l’image par défaut et un titre facultatif pour indiquer l’emplacement de l’Aiguille de l’espace de Seattle. Il centre également la carte sur l’icône et effectue un zoom avant. Pour plus d’informations générales sur l’utilisation du contrôle de carte, voir [Afficher des cartes avec des vues 2D, 3D et Streetside](display-maps.md).

```csharp
      private void displayPOIButton_Click(object sender, RoutedEventArgs e)
      {
         // Specify a known location.
         BasicGeoposition snPosition = new BasicGeoposition() { Latitude = 47.620, Longitude = -122.349 };
         Geopoint snPoint = new Geopoint(snPosition);

         // Create a MapIcon.
         MapIcon mapIcon1 = new MapIcon();
         mapIcon1.Location = snPoint;
         mapIcon1.NormalizedAnchorPoint = new Point(0.5, 1.0);
         mapIcon1.Title = "Space Needle";
         mapIcon1.ZIndex = 0;

         // Add the MapIcon to the map.
         MapControl1.MapElements.Add(mapIcon1);

         // Center the map over the POI.
         MapControl1.Center = snPoint;
         MapControl1.ZoomLevel = 14;
      }
```

Cet exemple affiche le point d’intérêt suivant sur la carte (l’image par défaut au centre).

![carte avec l’élément MapIcon](images/displaypoidefault.png)

La ligne de code suivante affiche la classe [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) avec une image personnalisée enregistrée dans le dossier Assets du projet. La propriété [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) de la classe **MapIcon** attend une valeur de type [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813). Ce type nécessite une instruction **using** pour l’espace de noms [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791).

**Conseil** Si vous utilisez la même image pour plusieurs icônes de carte, déclarez l’élément [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813) au niveau de la page ou de l’application pour optimiser les performances.

```csharp
    MapIcon1.Image =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/customicon.png"));
```

Gardez à l’esprit les considérations suivantes lorsque vous travaillez avec la classe [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) :

-   Le propriété [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) prend en charge une taille d’image maximale de 2 048 x 2 048 pixels.
-   Par défaut, il n’est pas garanti que l’image de l’icône de carte s’affiche. Cette classe peut être masquée quand elle cache d’autres éléments ou étiquettes sur la carte. Pour la garder visible, définissez la propriété [**CollisionBehaviorDesired**](https://msdn.microsoft.com/library/windows/apps/dn974327) de l’icône de carte sur [**MapElementCollisionBehavior.RemainVisible**](https://msdn.microsoft.com/library/windows/apps/dn974314).
-   Il n’est pas garanti non plus que le [**Title**](https://msdn.microsoft.com/library/windows/apps/dn637088) facultatif de [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) s’affiche. Si le texte n’est pas visible, effectuez un zoom arrière en réduisant la valeur de la propriété [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) de la classe [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).
-   Quand vous affichez une image [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) qui pointe vers un emplacement spécifique sur la carte, par exemple une punaise ou une flèche, envisagez d’affecter à la valeur de la propriété [**NormalizedAnchorPoint**](https://msdn.microsoft.com/library/windows/apps/dn637082) l’emplacement approximatif du pointeur sur l’image. Si vous conservez la valeur par défaut (0, 0) de **NormalizedAnchorPoint**, qui représente le coin supérieur gauche de l’image, des modifications de la propriété [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) de la carte peuvent laisser l’image pointer vers un autre emplacement.

## <a name="add-a-mappolygon"></a>Ajouter un élément MapPolygon


Utilisez la classe [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103) pour afficher une forme multipoint sur la carte. L’exemple suivant, tiré de l’[exemple de carte UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977), affiche une zone rouge bordée de bleu sur la carte.

```csharp
private void mapPolygonAddButton_Click(object sender, Windows.UI.Xaml.RoutedEventArgs e)
{
   double centerLatitude = myMap.Center.Position.Latitude;
   double centerLongitude = myMap.Center.Position.Longitude;
   MapPolygon mapPolygon = new MapPolygon();
   mapPolygon.Path = new Geopath(new List<BasicGeoposition>() {
         new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude-0.001 },                
         new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },                
         new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude+0.001 },
         new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },

   });
           
   mapPolygon.ZIndex = 1;
   mapPolygon.FillColor = Colors.Red;
   mapPolygon.StrokeColor = Colors.Blue;
   mapPolygon.StrokeThickness = 3;
   mapPolygon.StrokeDashed = false;
   myMap.MapElements.Add(mapPolygon);
}
```

## <a name="add-a-mappolyline"></a>Ajouter un élément MapPolyline


Utilisez la classe [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114) pour afficher une ligne sur la carte. L’exemple suivant, tiré de l’[exemple de carte UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977), affiche une ligne en pointillé sur la carte.

```csharp
private void mapPolylineAddButton_Click(object sender, Windows.UI.Xaml.RoutedEventArgs e)
{
   double centerLatitude = myMap.Center.Position.Latitude;
   double centerLongitude = myMap.Center.Position.Longitude;
   MapPolyline mapPolyline = new MapPolyline();
   mapPolyline.Path = new Geopath(new List<BasicGeoposition>() {                
         new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },                
         new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
   });
              
   mapPolyline.StrokeColor = Colors.Black;
   mapPolyline.StrokeThickness = 3;
   mapPolyline.StrokeDashed = true;
   myMap.MapElements.Add(mapPolyline);
}
```

## <a name="add-xaml"></a>Ajouter du code XAML


Utilisez du code XAML pour afficher des éléments d’interface utilisateur personnalisés sur la carte. Positionnez le code XAML sur la carte en spécifiant son emplacement et son point d’ancrage normalisé.

-   Appelez [**SetLocation**](https://msdn.microsoft.com/library/windows/desktop/ms704369) pour définir l’emplacement du code XAML sur la carte.
-   Appelez [**SetNormalizedAnchorPoint**](https://msdn.microsoft.com/library/windows/apps/dn637050) pour définir l’emplacement relatif du code XAML correspondant à l’emplacement spécifié.

L’exemple suivant affiche une carte de la ville de Seattle et ajoute un contrôle [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) en code XAML pour indiquer l’emplacement de la Space Needle. Il centre également la carte sur la zone et effectue un zoom avant. Pour plus d’informations générales sur l’utilisation du contrôle de carte, voir [Afficher des cartes avec des vues 2D, 3D et Streetside](display-maps.md).

```csharp
private void displayXAMLButton_Click(object sender, RoutedEventArgs e)
{
   // Specify a known location.
   BasicGeoposition snPosition = new BasicGeoposition() { Latitude = 47.620, Longitude = -122.349 };
   Geopoint snPoint = new Geopoint(snPosition);

   // Create a XAML border.
   Border border = new Border
   {
      Height = 100,
      Width = 100,
      BorderBrush = new SolidColorBrush(Windows.UI.Colors.Blue),
      BorderThickness = new Thickness(5),
   };

   // Center the map over the POI.
   MapControl1.Center = snPoint;
   MapControl1.ZoomLevel = 14;

   // Add XAML to the map.
   MapControl1.Children.Add(border);
   MapControl.SetLocation(border, snPoint);
   MapControl.SetNormalizedAnchorPoint(border, new Point(0.5, 0.5));
}
```

Cet exemple affiche une bordure bleue sur la carte.

![Capture d’écran d’un balisage XAML à l’emplacement du point d’intérêt sur la carte](images/displaypoixaml.png)

Les exemples suivants montrent comment ajouter des éléments d’interface utilisateur en code XAML directement dans le balisage XAML de la page à l’aide de la liaison de données. Comme pour d’autres éléments XAML qui affichent du contenu, la propriété [**Children**](https://msdn.microsoft.com/library/windows/apps/dn637008) est la propriété de contenu par défaut de la classe [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) et n’a pas besoin d’être spécifiée explicitement dans le balisage XAML.

Cet exemple montre comment afficher deux contrôles XAML en tant qu’enfants implicites du [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

```xml
<maps:MapControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{Binding SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{Binding BellevueLocation}"/>
</maps:MapControl>
```

Cet exemple montre comment afficher deux contrôles XAML contenus dans un [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094).

```xml
<maps:MapControl>
  <maps:MapItemsControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{Binding SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{Binding BellevueLocation}"/>
  </maps:MapItemsControl>
</maps:MapControl>
```

Cet exemple affiche une collection d’éléments XAML liés à un [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094).

```xml
<maps:MapControl x:Name="MapControl" MapTapped="MapTapped" MapDoubleTapped="MapTapped" MapHolding="MapTapped">
  <maps:MapItemsControl ItemsSource="{Binding StateOverlays}">
    <maps:MapItemsControl.ItemTemplate>
      <DataTemplate>
        <StackPanel Background="Black" Tapped="Overlay_Tapped">
          <TextBlock maps:MapControl.Location="{Binding Location}" Text="{Binding Name}" maps:MapControl.NormalizedAnchorPoint="0.5,0.5" FontSize="20" Margin="5"/>
        </StackPanel>
      </DataTemplate>
    </maps:MapItemsControl.ItemTemplate>
  </maps:MapItemsControl>
</maps:MapControl>
```

## <a name="related-topics"></a>Rubriques connexes

* [Centre de développement Bing Cartes](https://www.bingmapsportal.com/)
* [Exemple de carte UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Recommandations en matière de conception pour les cartes](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vidéos de la build 2015 : utilisation des cartes et de la localisation sur un téléphone, une tablette et un PC dans vos applications Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemple d’application de trafic UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)
* [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103)
* [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114)





<!--HONumber=Dec16_HO1-->


