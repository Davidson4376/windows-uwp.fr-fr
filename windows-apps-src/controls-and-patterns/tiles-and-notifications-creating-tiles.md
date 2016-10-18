---
author: mijacobs
Description: "Une vignette est la représentation d’une application dans le menu Démarrer. Chaque application dispose d’une vignette. Lorsque vous créez un projet d’application de plateforme Windows universelle (UWP) dans Microsoft Visual Studio, il inclut une vignette par défaut qui affiche le nom et le logo de l’application."
title: Vignettes
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
label: Tiles
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 37de1a413ac9b5e74c905c140899ec7577a6fae5

---
# Vignettes pour les applications UWP

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Une *vignette* est la représentation d’une application dans le menu Démarrer. Chaque application dispose d’une vignette. Lorsque vous créez un projet d’application de plateforme Windows universelle (UWP) dans Microsoft Visual Studio, il inclut une vignette par défaut qui affiche le nom et le logo de l’application. Windows affiche cette vignette lors de la première installation de l’application. Une fois votre application installée, vous pouvez modifier le contenu de votre vignette via des notifications. Par exemple, vous pouvez modifier la vignette pour communiquer de nouvelles informations à l’utilisateur, telles que des titres d’actualités, ou l’objet du dernier message non lu.

## Configurer la vignette par défaut


Lorsque vous créez un projet dans Visual Studio, cela a pour effet de créer une vignette par défaut simple qui affiche le nom et le logo de votre application.

```XML
  <Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="ExampleApp.App">
      <uap:VisualElements
        DisplayName="ExampleApp"
        Square150x150Logo="Assets\Logo.png"
        Square44x44Logo="Assets\SmallLogo.png"
        Description="ExampleApp"
        BackgroundColor="#464646">
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
    </Application>
  </Applications>
```

Vous devez mettre à jour quelques éléments :

-   DisplayName : remplacez cette valeur par le nom à afficher sur votre vignette.
-   ShortName : l’espace disponible pour le nom d’affichage sur les vignettes étant limité, nous vous recommandons de spécifier ce nom court pour éviter que le nom de votre application soit tronqué.
-   Images de logo :

    Vous devez remplacer ces images par vos propres images. Vous avez la possibilité de fournir des images pour différentes échelles visuelles, mais vous n’êtes pas obligé de le faire pour toutes. Pour vérifier que votre application s’affiche correctement sur divers appareils, nous vous recommandons de fournir des versions de chaque image aux échelles 100%, 200% et 400%.

    Les images mises à l’échelle suivent cette convention de nommage: tests
    
    *&lt;nom de l’image&gt;*.scale-*&lt;facteur d’échelle&gt;*.*&lt;extension de fichier image&gt;* 

    Par exemple: SmallLogo.scale-100.png

    Lorsque vous faites référence à l’image, vous spécifiez *&lt;nom de l’image&gt;*.*&lt;extension de fichier image&gt;* (« SmallLogo.png » dans cet exemple). Le système sélectionne automatiquement l’image à l’échelle appropriée pour l’appareil parmi les images que vous avez fournies.

-   Nous vous conseillons vivement de fournir des logos pour les vignettes de grande taille afin que l’utilisateur puisse redimensionner la vignette de votre application si nécessaire. Pour fournir ces images supplémentaires, vous créez un élément `DefaultTile` et utilisez les attributs `Wide310x150Logo` et `Square310x310Logo` pour spécifier les images supplémentaires :
```    XML
  <Applications>
        <Application Id="App"
          Executable="$targetnametoken$.exe"
          EntryPoint="ExampleApp.App">
          <uap:VisualElements
            DisplayName="ExampleApp"
            Square150x150Logo="Assets\Logo.png"
            Square44x44Logo="Assets\SmallLogo.png"
            Description="ExampleApp"
            BackgroundColor="#464646">
            <uap:DefaultTile
              Wide310x150Logo="Assets\WideLogo.png"
              Square310x310Logo="Assets\LargeLogo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
      </Applications>
```

## Utiliser les notifications pour personnaliser votre vignette


Une fois votre application installée, vous pouvez utiliser les notifications pour personnaliser votre vignette. Vous pouvez le faire lors du premier lancement de l’application ou en réponse à certains événements tels qu’une notification Push.

1.  Créez une charge utile XML (sous la forme d’un [**Windows.Data.Xml.Dom.XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br206173)) décrivant la vignette.

    -   Windows10 inaugure un nouveau schéma de vignette adaptative que vous pouvez utiliser. Pour obtenir des instructions, voir [Vignettes adaptatives](tiles-and-notifications-create-adaptive-tiles.md). Pour découvrir le schéma, voir l’article [Schéma des vignettes adaptatives](tiles-and-notifications-adaptive-tiles-schema.md). 

    -   Pour définir votre vignette, vous pouvez utiliser les modèles de vignette Windows8.1. Pour plus d’informations, voir [Création de vignettes et badges (Windows8.1)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868260).

2.  Créez un objet notification par vignette et passez-lui le [**XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br206173) que vous avez créé. Il existe plusieurs types d’objets notification :
    -   Un objet [**Windows.UI.NotificationsTileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616) pour la mise à jour immédiate de la vignette.
    -   Un objet [**Windows.UI.Notifications.ScheduledTileNotification**](https://msdn.microsoft.com/library/windows/apps/hh701637) pour la mise à jour la vignette à un moment donné dans le futur.

3.  Utilisez la méthode [**Windows.UI.Notifications.TileUpdateManager.CreateTileUpdaterForApplication**](https://msdn.microsoft.com/library/windows/apps/br208623) pour créer un objet [**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628).
4.  Appelez la méthode [**TileUpdater.Update**](https://msdn.microsoft.com/library/windows/apps/br208632) et passez-lui l’objet notification par vignette créé à l’étape2.

 

 







<!--HONumber=Aug16_HO3-->


