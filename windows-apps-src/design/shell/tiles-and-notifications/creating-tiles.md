---
author: mijacobs
Description: A tile is an app's representation on the Start menu. Every app has a tile. When you create a new Universal Windows Platform (UWP) app project in Microsoft Visual Studio, it includes a default tile that displays your app's name and logo.
title: Vignettes
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
label: Tiles
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f4388b67335bce497987ab22e3b281cf86e029af
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7167255"
---
# <a name="tiles-for-uwp-apps"></a>Vignettes pour les applications UWP

 

Une *vignette* est la représentation d’une application dans le menu Démarrer. Chaque application dispose d’une vignette. Lorsque vous créez un projet d’application de plateforme Windows universelle (UWP) dans Microsoft Visual Studio, il inclut une vignette par défaut qui affiche le nom et le logo de l’application.Windows affiche cette vignette lors de la première installation de l’application. Une fois votre application installée, vous pouvez modifier le contenu de votre vignette via des notifications. Par exemple, vous pouvez modifier la vignette pour communiquer de nouvelles informations à l’utilisateur, telles que des titres d’actualités, ou l’objet du dernier message non lu.

## <a name="configure-the-default-tile"></a>Configurer la vignette par défaut


Lorsque vous créez un projet dans Visual Studio, cela a pour effet de créer une vignette par défaut simple qui affiche le nom et le logo de votre application.

Pour modifier votre vignette, double-cliquez sur le fichier **Package.appxmanifest** dans votre projet UWP principal pour ouvrir le concepteur (ou cliquez avec le bouton droit sur le fichier et sélectionnez Afficher le Code).

```XML
  <Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="ExampleApp.App">
      <uap:VisualElements
        DisplayName="ExampleApp"
        Square150x150Logo="Assets\Square150x150Logo.png"
        Square44x44Logo="Assets\Square44x44Logo.png"
        Description="ExampleApp"
        BackgroundColor="#464646">
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
    </Application>
  </Applications>
```

Vous devez mettre à jour quelques éléments:

-   DisplayName : remplacez cette valeur par le nom à afficher sur votre vignette.
-   ShortName : l’espace disponible pour le nom d’affichage sur les vignettes étant limité, nous vous recommandons de spécifier ce nom court pour éviter que le nom de votre application soit tronqué.
-   Images de logo :

    Vous devez remplacer ces images par vos propres images. Vous avez la possibilité de fournir des images pour différentes échelles visuelles, mais vous n’êtes pas obligé de le faire pour toutes. Pour vérifier que votre application s’affiche correctement sur divers appareils, nous vous recommandons de fournir des versions de chaque image aux échelles 100%, 200% et 400%. Voir [Ressources de vignette et d’icône](app-assets.md) pour en savoir plus sur la génération de ces ressources.

    Les images mises à l’échelle suivent cette convention de nommage:
    
    *&lt;nom de l’image&gt;*.scale-*&lt;facteur d’échelle&gt;*.*&lt;extension de fichier image&gt;* 

    Par exemple: SplashScreen.scale-100.png

    Lorsque vous faites référence à l’image, vous spécifiez *&lt;nom de l’image&gt;*.*&lt;extension de fichier image&gt;* («SplashScreen.png» dans cet exemple). Le système sélectionne automatiquement l’image à l’échelle appropriée pour l’appareil parmi les images que vous avez fournies.

-   Nous vous conseillons vivement de fournir des logos pour les vignettes de grande taille afin que l’utilisateur puisse redimensionner la vignette de votre application si nécessaire. Pour fournir ces images supplémentaires, vous créez un élément **DefaultTile** et utilisez les attributs **Wide310x150Logo** et **Square310x310Logo** pour spécifier les images supplémentaires:
```    XML
  <Applications>
        <Application Id="App"
          Executable="$targetnametoken$.exe"
          EntryPoint="ExampleApp.App">
          <uap:VisualElements
            DisplayName="ExampleApp"
            Square150x150Logo="Assets\Square150x150Logo.png"
            Square44x44Logo="Assets\Square44x44Logo.png"
            Description="ExampleApp"
            BackgroundColor="#464646">
            <uap:DefaultTile
              Wide310x150Logo="Assets\Wide310x150Logo.png"
              Square310x310Logo="Assets\Square310x310Logo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
      </Applications>
```

## <a name="use-notifications-to-customize-your-tile"></a>Utiliser les notifications pour personnaliser votre vignette


Une fois votre application installée, vous pouvez utiliser les notifications pour personnaliser votre vignette. Vous pouvez le faire lors du premier lancement de l’application ou en réponse à un événement tel qu’une notification Push.

Pour savoir comment envoyer des notifications par vignette, consultez [Envoyer une notification par vignette locale](sending-a-local-tile-notification.md).
