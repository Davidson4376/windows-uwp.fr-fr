---
author: normesta
title: Superposer des images sous forme de vignettes sur une carte
description: Superposez des images sous forme de vignettes tierces ou personnalisées sur une carte à l’aide de sources de vignettes. Utilisez des sources de vignette pour superposer des informations spécifiques (informations météorologiques, démographiques, sismiques...), ou pour remplacer entièrement la carte par défaut.
ms.assetid: 066BD6E2-C22B-4F5B-AA94-5D6C86A09BDF
ms.author: normesta
ms.date: 07/19/2018
ms.topic: article
keywords: windows10, uwp, carte, emplacement, images, superposition
ms.localizationpriority: medium
ms.openlocfilehash: 7e01b115d3e2b2a305468357440acff50a06f3fd
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2018
ms.locfileid: "6991926"
---
# <a name="overlay-tiled-images-on-a-map"></a>Superposer des images sous forme de vignettes sur une carte

Superposez des images sous forme de vignettes tierces ou personnalisées sur une carte à l’aide de sources de vignettes. Utilisez des sources de vignette pour superposer des informations spécifiques (informations météorologiques, démographiques, sismiques...), ou pour remplacer entièrement la carte par défaut.

**Astuce** Pour plus d’informations sur l’utilisation de cartes dans votre app, téléchargez l’exemple de carte pour [plateforme Windows universelle (UWP) ](http://go.microsoft.com/fwlink/p/?LinkId=619977) sur Github.

<a id="tileintro" />

## <a name="tiled-image-overview"></a>Vue d’ensemble des images sous forme de vignettes

Les différents services de carte (cartes Nokia et cartes Bing, par exemple) découpent les cartes en vignettes de forme carrée, afin de permettre une récupération et un affichage rapides. Ces vignettes sont au format 256 x 256 pixels et font l’objet d’un rendu préalable, selon différents niveaux de détail. De nombreux services tiers fournissent également des données cartographiques divisées en vignettes. Utilisez des sources de vignette pour récupérer des vignettes tierces ou pour créer vos propres vignettes personnalisées, avant de les superposer sur la carte affichée dans la classe [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

**Important**  lorsque vous utilisez des sources de vignette, vous n’avez pas à écrire du code pour demander ou pour positionner les vignettes individuelles. La classe [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) demande les vignettes dont elle a besoin. Chaque demande spécifie les coordonnées X et Y et le niveau de zoom de la vignette individuelle. Il vous suffit de spécifier le format de l’URI ou le nom de fichier à utiliser pour récupérer les vignettes dans la propriété **UriFormatString**. Ainsi, vous insérez des paramètres remplaçables dans l’URI ou le nom de fichier de base afin d’indiquer où transmettre les coordonnées X et Y et le niveau de zoom de chaque vignette.

Voici un exemple de la propriété [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) pour une classe [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986) qui affiche les paramètres remplaçables des coordonnéesX etY et le niveau de zoom.

```syntax
http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
```

(Les coordonnées X et Y représentent l’emplacement de la vignette individuelle au sein de la carte du monde, au niveau de détail spécifié. Le système de numérotation des vignettes démarre à partir de la valeur {0, 0}, dans le coin supérieur gauche de la carte. Par exemple, la vignette située à {1, 2} se trouve dans la deuxième colonne de la troisième ligne de la grille de vignettes.)

Pour plus d’informations sur le système de vignettes utilisé par les services cartographiques, voir [Système de vignette CartesBing](http://go.microsoft.com/fwlink/p/?LinkId=626692).

### <a name="overlay-tiles-from-a-tile-source"></a>Superposer des vignettes à partir d’une source de vignette

Superposez des images sous forme de vignettes à partir d’une source de vignette sur une carte, à l’aide de la [**MapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn637141).

1.  Instanciez l’une des troisclasses de source de données de vignette qui héritent des données de la classe [**MapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn637141).

    -   [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986)
    -   [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994)
    -   [**CustomMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636983)

    Configurez le paramètre **UriFormatString** à utiliser pour demander des vignettes en insérant des paramètres remplaçables dans l’URI ou le nom de fichier de base.

    L’exemple suivant instancie une classe [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986). Cet exemple spécifie la valeur de la propriété [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) dans le constructeur de la classe **HttpMapTileDataSource**.

    ```csharp
        HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
          "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");
    ```

2.  Instanciez et configurez une classe [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144). Spécifiez la classe [**MapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn637141) que vous avez configurée à l’étape précédente en tant que propriété [**DataSource**](https://msdn.microsoft.com/library/windows/apps/dn637149) de la classe **MapTileSource**.

    L’exemple suivant spécifie la propriété [**DataSource**](https://msdn.microsoft.com/library/windows/apps/dn637149) dans le constructeur de la classe [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144).

    ```csharp
        MapTileSource tileSource = new MapTileSource(dataSource);
    ```

    Vous pouvez restreindre les conditions dans lesquelles les vignettes sont affichées à l’aide des propriétés de la classe [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144).

    -   Affichez les vignettes uniquement dans une zone géographique spécifique en affectant une valeur à la propriété [**Bounds**](https://msdn.microsoft.com/library/windows/apps/dn637147).
    -   Affichez les vignettes selon certains niveaux de détail spécifiques en affectant une valeur à la propriété [**ZoomLevelRange**](https://msdn.microsoft.com/library/windows/apps/dn637171).

    Vous pouvez également configurer d’autres propriétés de la [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144) qui affectent le chargement ou l’affichage de vignettes, comme [**Layer**](https://msdn.microsoft.com/library/windows/apps/dn637157), [**AllowOverstretch**](https://msdn.microsoft.com/library/windows/apps/dn637145), [**IsRetryEnabled**](https://msdn.microsoft.com/library/windows/apps/dn637153) et [**IsTransparencyEnabled**](https://msdn.microsoft.com/library/windows/apps/dn637155).

3.  Ajoutez la [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144) à la collection [**TileSources**](https://msdn.microsoft.com/library/windows/apps/dn637053) du [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

    ```csharp
         MapControl1.TileSources.Add(tileSource);
    ```

## <a name="overlay-tiles-from-a-web-service"></a>Superposer des vignettes provenant d’un service web


Superposez des images sous forme de vignettes récupérées à partir d’un service web via la [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986).

1.  Instanciez une [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986).
2.  Spécifiez le format de l’URI que le service web attend, en tant que valeur de la propriété [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992). Pour créer cette valeur, insérez les paramètres remplaçables dans l’URI de base. Par exemple, dans l’exemple de code suivant, la valeur de l’élément **UriFormatString** est la suivante:

    ``` syntax
    http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
    ```

    Le service web doit prendre en charge un URI qui contient les paramètres remplaçables {zoomlevel}, {x} et {y}. La plupart des services web (par exemple ceux de Nokia, Bing et Google) prennent en charge les URI dans ce format. Si le service web requiert des arguments supplémentaires qui ne sont pas disponibles avec la propriété [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992), vous devez créer un URI personnalisé. Créez et renvoyez un URI personnalisé en gérant l’événement [**UriRequested**](https://msdn.microsoft.com/library/windows/apps/dn636993). Pour plus d’informations, voir la section [Fournir un URI personnalisé](#customuri) plus loin dans cette rubrique.

3.  Suivez ensuite les étapes restantes décrites précédemment dans [Vue d’ensemble des images sous forme de vignettes](#tileintro).

L’exemple suivant illustre la superposition de vignettes provenant d’un service web fictif sur une carte représentant l’Amérique du Nord. La valeur de la propriété [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) est spécifiée dans le constructeur de la classe [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986). Dans cet exemple, les vignettes sont uniquement affichées dans les limites géographiques spécifiées par la propriété facultative [**Bounds**](https://msdn.microsoft.com/library/windows/apps/dn637147).

```csharp
private void AddHttpMapTileSource()
{
    // Create the bounding box in which the tiles are displayed.
    // This example represents North America.
    BasicGeoposition northWestCorner =
        new BasicGeoposition() { Latitude = 48.38544, Longitude = -124.667360 };
    BasicGeoposition southEastCorner =
        new BasicGeoposition() { Latitude = 25.26954, Longitude = -80.30182 };
    GeoboundingBox boundingBox = new GeoboundingBox(northWestCorner, southEastCorner);

    // Create an HTTP data source.
    // This example retrieves tiles from a fictitious web service.
    HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
        "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");

    // Optionally, add custom HTTP headers if the web service requires them.
    dataSource.AdditionalRequestHeaders.Add("header name", "header value");

    // Create a tile source and add it to the Map control.
    MapTileSource tileSource = new MapTileSource(dataSource);
    tileSource.Bounds = boundingBox;
    MapControl1.TileSources.Add(tileSource);
}
```

```cppwinrt
...
#include <winrt/Windows.Devices.Geolocation.h>
#include <winrt/Windows.UI.Xaml.Controls.Maps.h>
...
void MainPage::AddHttpMapTileSource()
{
    Windows::Devices::Geolocation::BasicGeoposition northWest{ 48.38544, -124.667360 };
    Windows::Devices::Geolocation::BasicGeoposition southEast{ 25.26954, -80.30182 };
    Windows::Devices::Geolocation::GeoboundingBox boundingBox{ northWest, southEast };

    Windows::UI::Xaml::Controls::Maps::HttpMapTileDataSource dataSource{
        L"http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}" };

    dataSource.AdditionalRequestHeaders().Insert(L"header name", L"header value");

    Windows::UI::Xaml::Controls::Maps::MapTileSource tileSource{ dataSource };
    tileSource.Bounds(boundingBox);

    MapControl1().TileSources().Append(tileSource);
}
...
```

```cpp
void MainPage::AddHttpMapTileSource()
{
    BasicGeoposition northWest = { 48.38544, -124.667360 };
    BasicGeoposition southEast = { 25.26954, -80.30182 };
    GeoboundingBox^ boundingBox = ref new GeoboundingBox(northWest, southEast);

    auto dataSource = ref new Windows::UI::Xaml::Controls::Maps::HttpMapTileDataSource(
        "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");

    dataSource->AdditionalRequestHeaders->Insert("header name", "header value");

    auto tileSource = ref new Windows::UI::Xaml::Controls::Maps::MapTileSource(dataSource);
    tileSource->Bounds = boundingBox;

    this->MapControl1->TileSources->Append(tileSource);
}
```

## <a name="overlay-tiles-from-local-storage"></a>Superposer des vignettes provenant d’un stockage local


Superposez des images sous forme de vignettes stockées en tant que fichiers dans le stockage local, à l’aide de la [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994). En règle générale, vous placez ces fichiers dans un package, puis vous les distribuez avec votre application.

1.  Instanciez une [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994).
2.  Spécifiez le format des noms de fichier en tant que valeur de la propriété [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998). Pour créer cette valeur, insérez les paramètres remplaçables dans le nom de fichier de base. Par exemple, dans l’exemple de code suivant, la valeur de l’élément [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) est la suivante:

    ``` syntax
        Tile_{zoomlevel}_{x}_{y}.png
    ```

    Si le format de nom de fichiers nécessite des arguments supplémentaires qui ne sont pas fournis avec la propriété [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998), vous devez créer un URI personnalisé. Créez et renvoyez un URI personnalisé en gérant l’événement [**UriRequested**](https://msdn.microsoft.com/library/windows/apps/dn637001). Pour plus d’informations, voir la section [Fournir un URI personnalisé](#customuri) plus loin dans cette rubrique.

3.  Suivez ensuite les étapes restantes décrites précédemment dans [Vue d’ensemble des images sous forme de vignettes](#tileintro).

Vous pouvez utiliser les protocoles et les emplacements suivants pour charger les vignettes à partir du stockage local :

| URI | Informations supplémentaires |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| ms-appx:/// | Pointe vers la racine du dossier d’installation de l’application. |
|  | Il s’agit de l’emplacement référencé par la propriété [Package.InstalledLocation](https://msdn.microsoft.com/library/windows/apps/br224681). |
| ms-appdata:///local | Pointe vers la racine du stockage local de l’application. |
|  | Il s’agit de l’emplacement référencé par la propriété [ApplicationData.LocalFolder](https://msdn.microsoft.com/library/windows/apps/br241621). |
| ms-appdata:///temp | Pointe vers le dossier temporaire de l’application. |
|  | Il s’agit de l’emplacement référencé par la propriété [ApplicationData.TemporaryFolder](https://msdn.microsoft.com/library/windows/apps/br241629). |

 

L’exemple suivant illustre le chargement de vignettes stockées en tant que fichiers dans le dossier d’installation de l’application, via le protocole `ms-appx:///`. La valeur de la propriété [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998) est spécifiée dans le constructeur de la classe [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994). Dans cet exemple, les vignettes sont uniquement affichées lorsque le niveau de zoom de la carte est inclus dans la plage indiquée par la propriété facultative [**ZoomLevelRange**](https://msdn.microsoft.com/library/windows/apps/dn637171).

```csharp
        void AddLocalMapTileSource()
        {
            // Specify the range of zoom levels
            // at which the overlaid tiles are displayed.
            MapZoomLevelRange range;
            range.Min = 11;
            range.Max = 20;

            // Create a local data source.
            LocalMapTileDataSource dataSource = new LocalMapTileDataSource(
                "ms-appx:///TileSourceAssets/Tile_{zoomlevel}_{x}_{y}.png");

            // Create a tile source and add it to the Map control.
            MapTileSource tileSource = new MapTileSource(dataSource);
            tileSource.ZoomLevelRange = range;
            MapControl1.TileSources.Add(tileSource);
        }
```

<a id="customuri" />

## <a name="provide-a-custom-uri"></a>Fournir un URI personnalisé

Si les paramètres remplaçables fournis avec la propriété [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) de la classe [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986) ou la propriété [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998) de la classe [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994) ne sont pas suffisants pour récupérer vos vignettes, vous devez créer un URI personnalisé. Créez et renvoyez un URI personnalisé en proposant un gestionnaire personnalisé pour l’événement **UriRequested**. L’événement **UriRequested** est déclenché pour chaque vignette individuelle.

1.  Dans le gestionnaire personnalisé de l’événement **UriRequested**, combinez les arguments personnalisés requis avec les propriétés [**X**](https://msdn.microsoft.com/library/windows/apps/dn610743), [**Y**](https://msdn.microsoft.com/library/windows/apps/dn610744)et [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn610745) de la classe [**MapTileUriRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637177) pour créer l’URI personnalisé.
2.  Renvoyez l’URI personnalisé dans la propriété [**Uri**](https://msdn.microsoft.com/library/windows/apps/dn610748) de la classe [**MapTileUriRequest**](https://msdn.microsoft.com/library/windows/apps/dn637173), qui est contenue dans la propriété [**Request**](https://msdn.microsoft.com/library/windows/apps/dn637179) de la classe [**MapTileUriRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637177).

L’exemple suivant explique comment fournir un URI personnalisé en créant un gestionnaire personnalisé pour l’événement **UriRequested**. Il indique également comment implémenter le modèle de report si vous devez exécuter une action asynchrone pour créer l’URI personnalisé.

```csharp
using Windows.UI.Xaml.Controls.Maps;
using System.Threading.Tasks;
...
            var httpTileDataSource = new HttpMapTileDataSource();
            // Attach a handler for the UriRequested event.
            httpTileDataSource.UriRequested += HandleUriRequestAsync;
            MapTileSource httpTileSource = new MapTileSource(httpTileDataSource);
            MapControl1.TileSources.Add(httpTileSource);
...
        // Handle the UriRequested event.
        private async void HandleUriRequestAsync(HttpMapTileDataSource sender,
            MapTileUriRequestedEventArgs args)
        {
            // Get a deferral to do something asynchronously.
            // Omit this line if you don't have to do something asynchronously.
            var deferral = args.Request.GetDeferral();

            // Get the custom Uri.
            var uri = await GetCustomUriAsync(args.X, args.Y, args.ZoomLevel);

            // Specify the Uri in the Uri property of the MapTileUriRequest.
            args.Request.Uri = uri;

            // Notify the app that the custom Uri is ready.
            // Omit this line also if you don't have to do something asynchronously.
            deferral.Complete();
        }

        // Create the custom Uri.
        private async Task<Uri> GetCustomUriAsync(int x, int y, int zoomLevel)
        {
            // Do something asynchronously to create and return the custom Uri.        }
        }
```

## <a name="overlay-tiles-from-a-custom-source"></a>Superposer des vignettes provenant d’une source personnalisée

Superposez des vignettes personnalisées à l’aide de la [**CustomMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636983). Créez des vignettes par programme en mémoire, à la volée, ou écrivez votre propre code pour charger les vignettes existantes à partir d’une autre source.

Pour créer ou charger des vignettes personnalisées, fournissez un gestionnaire personnalisé pour l’événement [**BitmapRequested**](https://msdn.microsoft.com/library/windows/apps/dn636984). L’événement **BitmapRequested** est déclenché pour chaque vignette individuelle.

1.  Dans le gestionnaire personnalisé de l’événement [**BitmapRequested**](https://msdn.microsoft.com/library/windows/apps/dn636984), combinez les arguments personnalisés requis avec les propriétés [**X**](https://msdn.microsoft.com/library/windows/apps/dn637135), [**Y**](https://msdn.microsoft.com/library/windows/apps/dn637136) et [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637137) de la classe [**MapTileBitmapRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637132) pour créer ou récupérer une vignette personnalisée.
2.  Renvoyez la vignette personnalisée dans la propriété [**PixelData**](https://msdn.microsoft.com/library/windows/apps/dn637140) de la classe [**MapTileBitmapRequest**](https://msdn.microsoft.com/library/windows/apps/dn637128), qui est contenue dans la propriété [**Request**](https://msdn.microsoft.com/library/windows/apps/dn637134) de la classe [**MapTileBitmapRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637132). La propriété **PixelData** présente le type [**IRandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701664).

L’exemple suivant indique comment fournir des vignettes personnalisées en créant un gestionnaire personnalisé pour l’événement **BitmapRequested**. Cet exemple permet de créer des vignettes rouges identiques, partiellement opaques. L’exemple ne tient pas compte des propriétés [**X**](https://msdn.microsoft.com/library/windows/apps/dn637135), [**Y**](https://msdn.microsoft.com/library/windows/apps/dn637136) et [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637137) de la classe [**MapTileBitmapRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637132). Cet exemple n’est pas concret, mais il indique comment créer des vignettes personnalisées en mémoire, à la volée. Il indique également comment implémenter le modèle de report si vous devez exécuter une action asynchrone pour créer les vignettes personnalisées.

```csharp
using Windows.UI.Xaml.Controls.Maps;
using Windows.Storage.Streams;
using System.Threading.Tasks;
...
        CustomMapTileDataSource customDataSource = new CustomMapTileDataSource();
        // Attach a handler for the BitmapRequested event.
        customDataSource.BitmapRequested += customDataSource_BitmapRequestedAsync;
        customTileSource = new MapTileSource(customDataSource);
        MapControl1.TileSources.Add(customTileSource);
...
        // Handle the BitmapRequested event.
        private async void customDataSource_BitmapRequestedAsync(
            CustomMapTileDataSource sender,
            MapTileBitmapRequestedEventArgs args)
        {
            var deferral = args.Request.GetDeferral();
            args.Request.PixelData = await CreateBitmapAsStreamAsync();
            deferral.Complete();
        }

        // Create the custom tiles.
        // This example creates red tiles that are partially opaque.
        private async Task<RandomAccessStreamReference> CreateBitmapAsStreamAsync()
        {
            int pixelHeight = 256;
            int pixelWidth = 256;
            int bpp = 4;

            byte[] bytes = new byte[pixelHeight * pixelWidth * bpp];

            for (int y = 0; y < pixelHeight; y++)
            {
                for (int x = 0; x < pixelWidth; x++)
                {
                    int pixelIndex = y * pixelWidth + x;
                    int byteIndex = pixelIndex * bpp;

                    // Set the current pixel bytes.
                    bytes[byteIndex] = 0xff;        // Red
                    bytes[byteIndex + 1] = 0x00;    // Green
                    bytes[byteIndex + 2] = 0x00;    // Blue
                    bytes[byteIndex + 3] = 0x80;    // Alpha (0xff = fully opaque)
                }
            }

            // Create RandomAccessStream from byte array.
            InMemoryRandomAccessStream randomAccessStream =
                new InMemoryRandomAccessStream();
            IOutputStream outputStream = randomAccessStream.GetOutputStreamAt(0);
            DataWriter writer = new DataWriter(outputStream);
            writer.WriteBytes(bytes);
            await writer.StoreAsync();
            await writer.FlushAsync();
            return RandomAccessStreamReference.CreateFromStream(randomAccessStream);
        }
```

```cppwinrt
...
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Foundation::IAsyncOperation<Windows::Storage::Streams::InMemoryRandomAccessStream> MainPage::CustomRandomAccessStream()
{
    constexpr int pixelHeight{ 256 };
    constexpr int pixelWidth{ 256 };
    constexpr int bpp{ 4 };

    std::array<uint8_t, pixelHeight * pixelWidth * bpp> bytes;

    for (int y = 0; y < pixelHeight; y++)
    {
        for (int x = 0; x < pixelWidth; x++)
        {
            int pixelIndex{ y * pixelWidth + x };
            int byteIndex{ pixelIndex * bpp };

            // Set the current pixel bytes.
            bytes[byteIndex] = (byte)(std::rand() % 256);        // Red
            bytes[byteIndex + 1] = (byte)(std::rand() % 256);    // Green
            bytes[byteIndex + 2] = (byte)(std::rand() % 256);    // Blue
            bytes[byteIndex + 3] = (byte)((std::rand() % 56) + 200);    // Alpha (0xff = fully opaque)
        }
    }

    // Create RandomAccessStream from byte array.
    Windows::Storage::Streams::InMemoryRandomAccessStream randomAccessStream;
    Windows::Storage::Streams::IOutputStream outputStream{ randomAccessStream.GetOutputStreamAt(0) };
    Windows::Storage::Streams::DataWriter writer{ outputStream };
    writer.WriteBytes(bytes);

    co_await writer.StoreAsync();
    co_await writer.FlushAsync();

    co_return randomAccessStream;
}
...
```

```cpp
InMemoryRandomAccessStream^ TileSources::CustomRandomAccessStream::get()
{
    int pixelHeight = 256;
    int pixelWidth = 256;
    int bpp = 4;

    Array<byte>^ bytes = ref new Array<byte>(pixelHeight * pixelWidth * bpp);

    for (int y = 0; y < pixelHeight; y++)
    {
        for (int x = 0; x < pixelWidth; x++)
        {
            int pixelIndex = y * pixelWidth + x;
            int byteIndex = pixelIndex * bpp;

            // Set the current pixel bytes.
            bytes[byteIndex] = (byte)(std::rand() % 256);        // Red
            bytes[byteIndex + 1] = (byte)(std::rand() % 256);    // Green
            bytes[byteIndex + 2] = (byte)(std::rand() % 256);    // Blue
            bytes[byteIndex + 3] = (byte)((std::rand() % 56) + 200);    // Alpha (0xff = fully opaque)
        }
    }

    // Create RandomAccessStream from byte array.
    InMemoryRandomAccessStream^ randomAccessStream = ref new InMemoryRandomAccessStream();
    IOutputStream^ outputStream = randomAccessStream->GetOutputStreamAt(0);
    DataWriter^ writer = ref new DataWriter(outputStream);
    writer->WriteBytes(bytes);

    create_task(writer->StoreAsync()).then([writer](unsigned int)
    {
        create_task(writer->FlushAsync());
    });

    return randomAccessStream;
}
```

## <a name="replace-the-default-map"></a>Remplacer la carte par défaut

Pour remplacer entièrement la carte par défaut par des vignettes personnalisées ou tierces, procédez comme suit:

-   Spécifiez [**MapTileLayer**](https://msdn.microsoft.com/library/windows/apps/dn637143).**BackgroundReplacement** en tant que valeur de la propriété [**Layer**](https://msdn.microsoft.com/library/windows/apps/dn637157) de la classe [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144).
-   Spécifiez [**MapStyle**](https://msdn.microsoft.com/library/windows/apps/dn637127).**None** en tant que valeur de la propriété [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) de la classe [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004).

## <a name="related-topics"></a>Rubriques connexes

* [Centre de développement Bing Cartes](https://www.bingmapsportal.com/)
* [Exemple de carte UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Recommandations en matière de conception pour les cartes](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vidéos de la build 2015: utilisation des cartes et de la localisation sur un téléphone, une tablette et un PC dans vos applications Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemple d’application de trafic UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)
