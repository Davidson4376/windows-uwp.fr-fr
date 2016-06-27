---
author: Jwmsft
Description: "Découvrez comment intégrer des images dans votre application et comment utiliser les API des deux principales classes XAML, Image et ImageBrush."
title: Images et pinceaux image
ms.assetid: CEA8780C-71A3-4168-A6E8-6361CDFB2FAF
label: Images and image brushes
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 972480aabb6f0db3b5091bd55323f9d1946086e6

---
# Images et pinceaux image

Pour afficher une image, vous pouvez utiliser l’objet **Image** ou l’objet **ImageBrush**. Un objet Image affiche une image, tandis qu’un objet ImageBrush peint un autre objet avec une image. 

<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [**Classe Image**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx)
-   [**Propriété Source**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.source.aspx)
-   [**Classe ImageBrush**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.aspx)
-   [**Propriété ImageSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.imagesource.aspx)

## S’agit-il des éléments appropriés ?
Utilisez un élément **Image** pour afficher une image autonome dans votre application.

Utilisez un élément **ImageBrush** pour appliquer une image à un autre objet. L’utilisation d’un objet ImageBrush comprend des effets d’ornement pour le texte ou des arrière-plans sous forme de vignettes pour les contrôles ou les conteneurs de disposition. Vous pouvez contrôler comment l’image est étirée, alignée et disposée sous forme de vignette, ce qui vous permet de créer des modèles et d’autres effets. 

## Exemples



## Créer une image

### Image
Cet exemple montre comment créer une image à l’aide de l’objet [**Image**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx).


```XAML
<Image Width="200" Source="licorice.jpg" />
```

Voici l’objet Image affiché.

![Exemple d’un élément d’image](images/Image_Licorice.jpg)

Dans cet exemple, la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.source.aspx) indique l’emplacement de l’image à afficher. Vous pouvez définir la propriété Source en spécifiant une URL absolue (par exemple, http://contoso.com/myPicture.jpg) ou en spécifiant une URL relative à la structure de votre package d’application. Dans le cadre de notre exemple, nous plaçons le fichier image « licorice.jpg » dans le fichier racine de notre projet, puis nous déclarons les paramètres du projet qui incluent le fichier image en tant que contenu.

### ImageBrush

Avec l’objet [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.aspx), vous pouvez utiliser une image pour peindre une zone qui accepte un objet [**Brush**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.aspx). Par exemple, vous pouvez utiliser un objet ImageBrush pour la valeur de la propriété [**Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx) d’un objet [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.ellipse.aspx) ou la propriété [**Background**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.background.aspx) d’un objet [**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx).

L’exemple suivant montre comment utiliser un objet ImageBrush pour peindre un objet Ellipse.

```XAML
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="licorice.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

Voici l’objet Ellipse peint par l’objet ImageBrush.

![Objet Ellipse peint par un objet ImageBrush.](images/Image_ImageBrush_Ellipse.jpg)

### Étirer une image

Si vous ne définissez pas les valeurs [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.width.aspx) ou [**Height**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx) d’un objet **Image**, ce dernier est affiché avec les dimensions de l’image spécifiée par la propriété **Source**. Le fait de définir les valeurs **Width** et **Height** crée une zone rectangulaire dans laquelle l’image est affichée. Vous pouvez spécifier la façon dont l’image remplit cette zone à l’aide de la propriété [**Stretch**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.stretch.aspx). La propriété Stretch accepte ces valeurs qui sont définies par l’énumération [**Stretch**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.stretch.aspx) :

-   **None** : l’image n’est pas étirée pour remplir les dimensions de sortie. Faites preuve de prudence avec le paramètre Stretch : si l’image source est plus grande que la zone qui doit la contenir, votre image sera découpée. Cela n’est pas généralement souhaitable car, contrairement à une propriété [**Clip**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.clip.aspx) délibérée, vous n’avez aucun contrôle sur la fenêtre d’affichage.
-   **Uniform** : l’image est mise à l’échelle afin de correspondre aux dimensions de sortie. Mais les proportions du contenu sont conservées. Il s’agit de la valeur par défaut.
-   **UniformToFill** : l’image est mise à l’échelle de sorte qu’elle remplisse complètement la zone de sortie tout en conservant ses proportions d’origine.
-   **Fill** : l’image est mise à l’échelle afin de correspondre aux dimensions de sortie. Étant donné que la hauteur et la largeur du contenu sont mises à l’échelle indépendamment, les dimensions d’origine de l’image risquent de ne pas être conservées. C’est-à-dire que l’image risque d’être déformée afin de remplir complètement la zone de sortie.

![Exemple de paramètres d’étirement.](images/Image_Stretch.jpg)

### Rogner une image

Vous pouvez utiliser la propriété [**Clip**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.clip.aspx) pour découper une zone de la sortie image. Vous affectez à la propriété Clip la valeur [**Geometry**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.geometry.aspx). Le découpage non rectangulaire n’est actuellement pas pris en charge.

L’exemple suivant montre comment utiliser un objet [**RectangleGeometry**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.rectanglegeometry.aspx) en tant que région de découpage pour une image. Dans cet exemple, nous définissons un objet **Image** et affectons à la propriété Height la valeur 200. Un objet **RectangleGeometry** définit un rectangle pour la zone de l’image à afficher. La propriété [**Rect**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.rectanglegeometry.rect.aspx) a la valeur 25,25,100,150, ce qui définit un rectangle avec pour position de départ 25,25, une largeur de 100 et une hauteur de 150. Seule la partie de l’image qui se trouve dans la zone du rectangle est affichée.

```xaml
<Image Source="licorice.jpg" Height="200">
    <Image.Clip>
        <RectangleGeometry Rect="25,25,100,150" />
    </Image.Clip>
</Image>
```

Voici l’image découpée sur un arrière-plan noir.

![Objet Image rogné par un objet RectangleGeometry.](images/Image_Cropped.jpg)

### Appliquer une opacité

Vous pouvez appliquer une propriété [**Opacity**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.opacity.aspx) à une image de façon à ce qu’elle soit affichée de manière semi-translucide. Les valeurs d’opacité vont de 0,0 à 1,0, où 1,0 correspond à une opacité totale et 0,0 correspond à une transparence totale. Cet exemple montre comment appliquer une opacité de 0,5 à un objet Image.

```xaml
<Image Height="200" Source="licorice.jpg" Opacity="0.5" />
```

Voici l’objet Image affiché avec une opacité de 0,5 et un arrière-plan noir laissant passer l’opacité partielle.

![Objet Image avec une opacité de 0,5.](images/Image_Opacity.jpg)

### Formats de fichier d’image

Les objets **Image** et **ImageBrush** permettent d’afficher les formats de fichier d’image suivants :

-   Joint Photographic Experts Group (JPEG)
-   format PNG (Portable Network Graphics)
-   image bitmap (BMP)
-   format GIF (Graphics Interchange Format)
-   format TIFF (Tagged Image File Format)
-   JPEG XR
-   icônes (ICO)

L’API pour [**Image**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx), [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapimage.aspx) et [**BitmapSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapsource.aspx) n’inclut aucune méthode dédiée d’encodage ou de décodage des formats de média. Toutes les opérations d’encodage et de décodage sont intégrées et, au plus, feront émerger les aspects de l’encodage ou du décodage dans le cadre des données d’événement pour les événements de chargement. Si vous voulez effectuer des tâches spéciales avec encodage ou décodage d’image, que vous pouvez utiliser si votre application effectue des conversions ou des manipulations d’image, vous devez utiliser les API disponibles dans l’espace de noms [**Windows.Graphics.Imaging**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.graphics.imaging.aspx). Ces API sont également prises en charge par le composant Imagerie Windows (WIC) de Windows.

Pour plus d’informations sur les ressources d’application et la manière de créer un package des sources d’image dans une application, voir [Définition des ressources d’application](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321).

### WriteableBitmap

Un élément [**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.writeablebitmap.aspx) fournit un élément [**BitmapSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapsource.aspx) qui peut être modifié et qui n’utilise pas le décodage standard basé sur les fichiers à partir du composant WIC. Vous pouvez modifier des images dynamiquement et réafficher l’image mise à jour. Pour définir le contenu du tampon d’un élément **WriteableBitmap**, utilisez la propriété [**PixelBuffer**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.writeablebitmap.pixelbuffer.aspx) pour accéder au tampon et utilisez un flux ou un type de tampon propre à un langage pour le remplir. Pour obtenir un exemple de code, voir [**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.writeablebitmap.aspx).

### RenderTargetBitmap

La classe [**RenderTargetBitmap**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.rendertargetbitmap.aspx) peut capturer l’arborescence de l’interface utilisateur XAML d’une application en cours d’exécution, puis représenter une source d’image bitmap. Après la capture, cette source d’image peut être appliquée à d’autres parties de l’application, enregistrée en tant que ressource ou données d’application par l’utilisateur, ou utilisée à d’autres fins. Il peut notamment être très utile de créer une miniature au moment de l’exécution d’une page XAML pour un schéma de navigation, par exemple dans le but de fournir un lien vers une image à partir d’un contrôle [**Hub**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.hub.aspx). **RenderTargetBitmap** limite toutefois le contenu pouvant apparaître dans l’image capturée. Pour plus d’informations, voir la rubrique de référence de l’API pour [**RenderTargetBitmap**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.rendertargetbitmap.aspx).

### Sources d’images et mise à l’échelle

Vous devez créer vos sources d’images selon plusieurs tailles recommandées afin de vous assurer que votre application garde son aspect esthétique lorsque Windows la redimensionne. Lorsque vous spécifiez une propriété **Source** pour un objet **Image**, vous pouvez utiliser une convention d’affectation de noms qui référencera automatiquement la ressource appropriée pour la mise à l’échelle actuelle. Pour connaître les spécificités de la convention d’affectation de noms et obtenir plus d’informations, voir [Démarrage rapide : utilisation de ressources de fichiers ou d’images](https://msdn.microsoft.com/library/windows/apps/xaml/hh965325).

Pour plus d’informations sur la conception prenant en charge la mise à l’échelle, voir [Recommandations en matière d’expérience utilisateur pour la disposition et la mise à l’échelle](https://msdn.microsoft.com/library/windows/apps/dn611863).

### Image et ImageBrush dans le code

Il est courant de spécifier les éléments Image et ImageBrush en XAML plutôt que sous forme de code. Cela est dû au fait que ces éléments sont souvent la sortie d’outils de conception dans le cadre d’une définition d’interface utilisateur XAML.

Si vous définissez un élément Image ou ImageBrush à l’aide de code, utilisez les constructeurs par défaut, puis définissez la propriété Source appropriée ([**Image.Source**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.source.aspx) ou [**ImageBrush.ImageSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.imagesource.aspx)). Celle-ci nécessite un élément [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapimage.aspx) (et non un URI) quand vous la définissez à l’aide de code. Si votre source est un flux, utilisez la méthode [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync.aspx) pour initialiser la valeur. Si votre source est un URI incluant du contenu de votre application qui utilise les modèles **ms-appx** ou **ms-resource**, utilisez le constructeur [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/xaml/br243238.aspx) qui prend un URI. Vous pouvez également envisager de gérer l’événement [**ImageOpened**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.imageopened.aspx) s’il existe des problèmes de délai liés à la récupération ou au décodage de la source de l’image, auquel cas un contenu alternatif peut s’avérer nécessaire à afficher tant que la source de l’image n’est pas disponible. Pour obtenir un exemple de code, voir [Exemples d’images XAML](http://go.microsoft.com/fwlink/p/?linkid=238575).

> **Note** &nbsp;&nbsp;Si vous établissez des images à l’aide de code, vous pouvez utiliser la gestion automatique pour l’accès aux ressources non qualifiées avec les qualificateurs d’échelle et de culture actuels, ou vous pouvez utiliser les classes [**ResourceManager**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.resources.core.resourcemanager.aspx) et [**ResourceMap**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.resources.core.resourcemap.aspx) avec des qualificateurs pour la culture et l’échelle afin d’obtenir les ressources directement. Pour plus d’informations, voir [Système de gestion des ressources](https://msdn.microsoft.com/library/windows/apps/xaml/jj552947.aspx).




<!--HONumber=Jun16_HO3-->


