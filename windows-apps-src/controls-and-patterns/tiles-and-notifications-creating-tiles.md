---
Une vignette est la représentation d’une application dans le menu Démarrer. Chaque application dispose d’une vignette. Lorsque vous créez un projet d’application de plateforme Windows universelle (UWP) dans Microsoft Visual Studio, il inclut une vignette par défaut qui affiche le nom et le logo de l’application.
Vignettes
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
Vignettes
template: detail.hbs
---

# Vignettes pour les applications UWP


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Une *vignette* est la représentation d’une application dans le menu Démarrer. Chaque application dispose d’une vignette. Lorsque vous créez un projet d’application de plateforme Windows universelle (UWP) dans Microsoft Visual Studio, il inclut une vignette par défaut qui affiche le nom et le logo de l’application. Windows affiche cette vignette lors de la première installation de l’application. Une fois votre application installée, vous pouvez modifier le contenu de votre vignette par le biais de notifications. Par exemple, vous pouvez modifier la vignette pour communiquer de nouvelles informations à l’utilisateur, telles que des titres d’actualités, ou l’objet du dernier message non lu.

## <span id="Configure_the_default_tile"> </span> <span id="configure_the_default_tile"> </span> <span id="CONFIGURE_THE_DEFAULT_TILE"> </span>Configurer la vignette par défaut


Lorsque vous créez un projet dans Visual Studio, cela a pour effet de créer une vignette par défaut simple qui affiche le nom et le logo de votre application.

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

    Vous devez remplacer ces images par vos propres images. Vous avez la possibilité de fournir des images pour différentes échelles visuelles, mais vous n’êtes pas obligé de le faire pour toutes. Pour vous assurer que votre application s’affiche correctement sur divers appareils, nous vous recommandons de fournir des versions de chaque image aux échelles 100 %, 200 % et 400 %.

    Les images mises à l’échelle suivent la convention d’affectation de noms suivante :
    
    *&lt;nom_d’image&gt;*.scale-*&lt;facteur_d’échelle&gt;*.*&lt;extension_de_fichier_image&gt;* 


     

    For example: SmallLogo.scale-100.png

    When you refer to the image, you refer to it as *&lt;image name&gt;*.*&lt;image file extension&gt;* ("SmallLogo.png" in this example). The system will automatically select the appropriate scaled image for the device from the images you've provided.

-   Nous vous conseillons vivement de fournir des logos pour les vignettes de grande taille afin que l’utilisateur puisse redimensionner la vignette de votre application si nécessaire. Pour fournir ces images supplémentaires, vous créez un élément `DefaultTile` et utilisez les attributs `Wide310x150Logo` et `Square310x310Logo` pour spécifier les images supplémentaires :
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

## <span id="Use_notifications_to_customize_your_tile"> </span> <span id="use_notifications_to_customize_your_tile"> </span> <span id="USE_NOTIFICATIONS_TO_CUSTOMIZE_YOUR_TILE"> </span>Utiliser les notifications pour personnaliser votre vignette


Une fois votre application installée, vous pouvez utiliser les notifications pour personnaliser votre vignette. Vous pouvez le faire lors du premier lancement de l’application ou en réponse à certains événements tels qu’une notification Push.

1.  Créez une charge utile XML (sous la forme d’un [**Windows.Data.Xml.Dom.XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br206173)) décrivant la vignette.

    -   Windows 10 inaugure un nouveau schéma de vignette adaptative que vous pouvez utiliser. Pour obtenir des instructions, voir [Vignettes adaptatives](tiles-and-notifications-create-adaptive-tiles.md). Pour découvrir le schéma, voir l’article [Schéma des vignettes adaptatives](tiles-and-notifications-adaptive-tiles-schema.md). 

    -   Pour définir votre vignette, vous pouvez utiliser les modèles de vignette Windows 8.1. Pour plus d’informations, voir [Création de vignettes et badges (Windows 8.1)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868260).

2.  Créez un objet notification par vignette et passez-lui le [**XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br206173) que vous avez créé. Il existe plusieurs types d’objets notification :
    -   Un objet [**Windows.UI.NotificationsTileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616) pour la mise à jour immédiate de la vignette.
    -   Un objet [**Windows.UI.Notifications.ScheduledTileNotification**](https://msdn.microsoft.com/library/windows/apps/hh701637) pour la mise à jour la vignette à un moment donné dans le futur.

3.  Utilisez la méthode [**Windows.UI.Notifications.TileUpdateManager.CreateTileUpdaterForApplication**](https://msdn.microsoft.com/library/windows/apps/br208623) pour créer un objet [**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628).
4.  Appelez la méthode [**TileUpdater.Update**](https://msdn.microsoft.com/library/windows/apps/br208632) et passez-lui l’objet notification par vignette créé à l’étape 2.

 

 






<!--HONumber=Mar16_HO1-->


