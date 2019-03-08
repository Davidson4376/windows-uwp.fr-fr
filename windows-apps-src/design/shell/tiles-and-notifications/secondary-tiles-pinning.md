---
Description: Découvrez comment épingler une vignette secondaire pour démarrer à partir de votre application UWP.
title: Épingler des vignettes secondaires à démarrer
label: Pin secondary tiles to Start
template: detail.hbs
ms.date: 05/25/2017
ms.topic: article
keywords: Windows 10, uwp, vignettes secondaires, épingler, épinglage, démarrage rapide, exemple de code, exemple, secondarytile
ms.localizationpriority: medium
ms.openlocfilehash: 4bebee86c824242cf031503617d4a880ebbb74df
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653154"
---
# <a name="pin-secondary-tiles-to-start"></a>Épingler des vignettes secondaires à démarrer


Cette rubrique vous guide à travers les étapes de création d’une vignette secondaire pour votre application UWP et son épinglage au menu Démarrer.

![Capture d’écran de vignettes secondaires](images/secondarytiles.png)

Pour en savoir plus sur les vignettes secondaires, consultez la [Vue d’ensemble des vignettes secondaires ](secondary-tiles.md).


## <a name="add-namespace"></a>Ajouter l’espace de noms

Classe Windows.UI.StartScreen namespace includes the SecondaryTile.

```csharp
using Windows.UI.StartScreen;
```


## <a name="initialize-the-secondary-tile"></a>Initialiser la vignette secondaire

Les vignettes secondaires sont composées de plusieurs composants clés...

* **Paramètre TileId**: Identificateur unique qui vous permet d’identifier la vignette parmi vos autres vignettes secondaires.
* **DisplayName**: Le nom que vous voulez voir apparaître sur la vignette.
* **Arguments**: Les arguments que vous souhaitez passé à votre application lorsque l’utilisateur clique sur votre vignette.
* **Square150x150Logo**: Le logo requis, affiché sur la taille moyenne vignette (et si aucun petit logo fourni redimensionné à la vignette de petite taille).

Vous **DEVEZ** fournir des valeurs initialisées pour toutes les propriétés mentionnées ci-dessus, faute de quoi vous obtiendrez une exception.

Vous pouvez utiliser plusieurs constructeurs, toutefois, l’utilisation du constructeur prenant en compte tileId, displayName, arguments, square150x150Logo et desiredSize permet de s’assurer que toutes les propriétés requises sont définies.

```csharp
// Construct a unique tile ID, which you will need to use later for updating the tile
string tileId = "City" + zipCode;

// Use a display name you like
string displayName = cityName;

// Provide all the required info in arguments so that when user
// clicks your tile, you can navigate them to the correct content
string arguments = "action=viewCity&zipCode=" + zipCode;

// Initialize the tile with required arguments
SecondaryTile tile = new SecondaryTile(
    tileId,
    displayName,
    arguments,
    new Uri("ms-appx:///Assets/CityTiles/Square150x150Logo.png"),
    TileSize.Default);
```


## <a name="optional-add-support-for-larger-tile-sizes"></a>Facultatif : Ajouter la prise en charge des tailles supérieures de vignette

Si vous souhaitez afficher les notifications par vignette enrichies sur votre vignette secondaire, vous souhaiterez probablement permettre à l’utilisateur de redimensionner ses vignettes pour les élargir ou les agrandir, afin qu’ils puissent voir davantage de contenu.

Pour permettre l’élargissement ou l’agrandissement des vignettes de grande taille, vous devez indiquer les éléments *Wide310x150Logo* et *Square310x310Logo* . En outre, vous devez indiquer si possible l’élément *Square71x71Logo* pour la petite taille de vignette (dans le cas contraire nous réduirons la taille de l’élément Square150x150Logo requis pour la petite vignette).

Vous pouvez également indiquer un élément *Square44x44Logo* unique, qui s’affiche si vous le souhaitez dans le coin inférieur droit en présence d’une notification. Si vous n’en n’indiquez pas, l’élément *Square44x44Logo* de votre vignette principale sera utilisé.

```csharp
// Enable wide and large tile sizes
tile.VisualElements.Wide310x150Logo = new Uri("ms-appx:///Assets/CityTiles/Wide310x150Logo.png");
tile.VisualElements.Square310x310Logo = new Uri("ms-appx:///Assets/CityTiles/Square310x310Logo.png");

// Add a small size logo for better looking small tile
tile.VisualElements.Square71x71Logo = new Uri("ms-appx:///Assets/CityTiles/Square71x71Logo.png");

// Add a unique corner logo for the secondary tile
tile.VisualElements.Square44x44Logo = new Uri("ms-appx:///Assets/CityTiles/Square44x44Logo.png");
```


## <a name="optional-enable-showing-the-display-name"></a>Facultatif : Activer l’affichage du nom d’affichage

Par défaut le nom d’affichage NE s’affiche PAS. Pour afficher le nom d’affichage en moyen, large ou grand, ajoutez le code suivant.

```csharp
// Show the display name on all sizes
tile.VisualElements.ShowNameOnSquare150x150Logo = true;
tile.VisualElements.ShowNameOnWide310x150Logo = true;
tile.VisualElements.ShowNameOnSquare310x310Logo = true;
```


## <a name="optional-3d-secondary-tiles"></a>Facultatif : Vignettes secondaires 3D
Vous pouvez améliorer votre vignette secondaire pour Windows Mixed Reality en ajoutant des ressources 3D. Les utilisateurs peuvent placer des vignettes 3D directement dans leur page d’accueil Windows Mixed Reality au lieu du menu Démarrer lorsque votre application est utilisée dans un environnement Mixed Reality. Par exemple, vous pouvez créer des photosphères à 360° qui pointent directement dans une visionneuse de photos à 360°, ou laisser les utilisateurs placer un modèle 3D d’une chaise à partir d’un catalogue de meubles qui ouvre une page de détails sur les options de tarification et de couleur pour cet objet lorsqu’il est sélectionné. Pour commencer, reportez-vous à la [documentation pour développeurs Mixed Reality](https://developer.microsoft.com/windows/mixed-reality/implementing_3d_deep_links_for_your_app_in_the_windows_mixed_reality_home).



## <a name="pin-the-secondary-tile"></a>Épingler la vignette secondaire

Enfin, demandez l’épinglage de la vignette. Notez que cela doit faire l’objet d’un appel à partir d’un thread d’interface utilisateur. Sur le bureau, une boîte de dialogue s’affiche demandant à l’utilisateur à confirmer s’il souhaite épingler la vignette.

> [!IMPORTANT]
> Si vous êtes une application de bureau Windows utilisant le Pont du bureau, vous devez d’abord effectuer une étape supplémentaire comme décrit dans [Épingler à partir de l’application de bureau](secondary-tiles-desktop-pinning.md)

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="check-if-a-secondary-tile-exists"></a>Vérifier l’existence d’une vignette secondaire

Si l’utilisateur visite une page dans votre application qu’il a déjà épinglée au menu Démarrer, vous pouvez afficher à la place un bouton « Désépingler ».

Par conséquent, lorsque vous choisissez le bouton à afficher, vous devez d’abord vérifier si la vignette secondaire est déjà épinglée.

```csharp
// Check if the secondary tile is pinned
bool isPinned = SecondaryTile.Exists(tileId);

// TODO: Update UI to reflect whether user can either unpin or pin
```


## <a name="unpinning-a-secondary-tile"></a>Désépingler une vignette secondaire

Si la vignette est actuellement épinglée, et que l’utilisateur clique sur le bouton désépingler, vous devrez désépingler (supprimer) la vignette.

```csharp
// Initialize a secondary tile with the same tile ID you want removed
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then unpin the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="updating-a-secondary-tile"></a>Mise à jour d’une vignette secondaire

Si vous avez besoin de mettre à jour les logos, le nom d’affichage ou tout autre élément de la vignette secondaire, vous pouvez utiliser *RequestUpdateAsync* .

```csharp
// Initialize a secondary tile with the same tile ID you want to update
SecondaryTile tile = new SecondaryTile(tileId);

// Assign ALL properties, including ones you aren't changing

// And then update it
await tile.UpdateAsync();
```


## <a name="enumerating-all-pinned-secondary-tiles"></a>Énumération de toutes les vignettes secondaires épinglées

Si vous avez besoin de détecter toutes les vignettes épinglées par votre utilisateur, au lieu d’utiliser *SecondaryTile.Exists*, vous pouvez également utiliser *SecondaryTile.FindAllAsync()*.

```csharp
// Get all secondary tiles
var tiles = await SecondaryTile.FindAllAsync();
```


## <a name="send-a-tile-notification"></a>Envoyer une notification par vignette

Pour savoir comment afficher le contenu enrichi sur votre vignette via les notifications par vignette, consultez [Envoyer une notification par vignette locale ](sending-a-local-tile-notification.md).


## <a name="related"></a>Liens apparentés

* [Vue d’ensemble des vignettes secondaires](secondary-tiles.md)
* [Conseils de vignettes secondaires](secondary-tiles-guidance.md)
* [Ressources de la vignette](app-assets.md)
* [Documentation de contenu de vignette](create-adaptive-tiles.md)
* [Envoyer une notification de vignette local](sending-a-local-tile-notification.md)
