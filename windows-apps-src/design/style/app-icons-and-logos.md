---
Description: Comment créer des application icônes/logos représentatives de votre application dans le menu Démarrer, vignettes des applications, la barre des tâches, le Microsoft Store et bien plus encore.
title: Icônes et logos d’application
template: detail.hbs
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b755e8d165d58ce4303d9fefe6d051abce6c9765
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622454"
---
# <a name="app-icons-and-logos"></a>Icônes et logos d’application 

Chaque application possède un icône/logo qui le représente, et elle apparaît dans plusieurs emplacements dans l’interpréteur de commandes Windows : 

:::row:::
    :::column:::
        * La barre de titre de votre fenêtre d’application
        * La liste des applications dans le menu Démarrer
        * Le Gestionnaire de tâches et de la barre des tâches
        * Vignettes de votre application
        * Écran de démarrage de votre application
        * Dans le Microsoft Store
    :::column-end:::
    :::column:::
        ![windows 10 start and tiles](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

Cet article aborde les principes fondamentaux de création d’icônes d’application, comment utiliser Visual Studio pour les gérer et comment les gérer manuellement, si vous avez besoin.
 
(Cet article est spécialement pour les icônes qui représentent l’application proprement dit ; pour obtenir des conseils de l’icône général, consultez le [icônes](icons.md) article.)

## <a name="icon-types-locations-and-scale-factors"></a>Types d’icônes, des emplacements et des facteurs d’échelle

Par défaut, Visual Studio stocke vos ressources de l’icône dans un sous-répertoire de ressources. Voici une liste des différents types d’icônes, où ils apparaissent, et ils sont appelés. 

| Nom de l’icône | S’affiche dans | Nom du fichier multimédia |
| ---      | ---        | --- |
| Petite vignette | Menu Démarrer |  SmallTile.png  |
| Vignette moyenne |Menu Démarrer, annonce de Microsoft Store\*  |  Square150x150Logo.png |
| Vignette large  | Menu Démarrer   | Wide310x150Logo.png |
| Grande vignette   | Menu Démarrer, annonce de Microsoft Store\* |  LargeTile.png  |
| Icône de l’application | Liste des applications dans le menu Démarrer, barre des tâches, le Gestionnaire des tâches | Square44x44Logo.png |
| Écran de démarrage | Écran de démarrage de l’application | SplashScreen.png  |
| Logo du badge | Vignettes de votre application | BadgeLogo.png  |
| Logo du logo/Store package | Programme d’installation App, partenaires, l’option « Rapport une application » dans le Store, l’option « Rédiger une critique » dans le Store | StoreLogo.png  |

\* Utilisé, sauf si vous choisissez de [affichage seulement téléchargé des images dans le Store](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store). 

Pour garantir que ces icônes nettes sur chaque écran, vous pouvez créer plusieurs versions de la même icône pour afficher différents facteurs d’échelle. 

Le facteur d’échelle détermine la taille des éléments d’interface, telles que du texte. Les facteurs de mise à l’échelle comprise entre 100 % et 400 %. Plus grandes valeurs créent des éléments d’interface utilisateur plus volumineux, ce qui les rend plus facile à voir sur les affichages haute résolution. 

:::row:::
    :::column:::
        Windows automatically sets the scale factor for each display based on its DPI (dots-per-inch) and the viewing distance of the device. 

        (Users can override the default value by going to the **Settings &gt; Display &gt; Scale and layout** page.)
    :::column-end:::
    :::column:::
        ![](images/icons/display-settings-screen.png)
    :::column-end:::
:::row-end:::  


Étant donné que les ressources de l’icône application sont des bitmaps et les bitmaps n’évoluent pas bien, nous vous recommandons d’en fournissant une version à chaque ressource d’icône pour chaque facteur d’échelle : 100 %, 125 %, 150 %, 200 % et 400 %. C’est un grand nombre d’icônes ! Heureusement, Visual Studio fournit un outil qui permet de facilement générer et mettre à jour de ces icônes. 

## <a name="microsoft-store-listing-image"></a>Image de la liste de Microsoft Store

« Comment spécifier les images pour la liste de mon application dans le Microsoft Store ? »

Par défaut, nous utilisons certaines des images à partir de vos packages dans le Store, comme décrit dans la table en haut de cette page (ainsi que d’autres [images que vous fournissez pendant le processus de soumission](https://docs.microsoft.com/en-us/windows/uwp/publish/app-screenshots-and-images)). Toutefois, vous avez la possibilité peuvent empêcher le Store d’utiliser les images de logo dans les packages de votre application lors de l’affichage de votre annonce aux clients sur Windows 10 (y compris Xbox) et à la place le Store utiliser uniquement les images que vous téléchargez. Cela vous permet de mieux contrôler l’apparence de votre application sur différents affichages partout dans le Windows Store. (Notez que si votre produit prend en charge les versions antérieures du système d’exploitation, les clients peuvent toujours voir les images à partir de vos packages, même si vous utilisez cette option.) Vous pouvez le faire dans le **Store logos** section de la **annonce de Store** étape du processus de soumission.

![Spécification des logos de Store pendant le processus de soumission d’application](images/app-icons/storelogodisplay.png)

Lorsque vous activez cette case, une nouvelle section appelée **afficher des images Store** s’affiche. Ici, vous pouvez charger des 3 tailles d’image que le Store utilisera à la place des images de logo à partir de packages de votre application : 300 x 300, 150 x 150 et 71 x 71 pixels. Uniquement la taille de 300 x 300 est requise, bien que nous vous recommandons de fournir toutes les tailles de 3.

Pour plus d’informations, consultez [affichage seulement téléchargé des images de logo dans le Store](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store).

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>La gestion des icônes d’application avec le Concepteur de manifeste Visual Studio

Visual Studio fournit un outil très utile pour la gestion des icônes d’application appelé le **Concepteur de manifeste**. 

> Si vous ne disposez pas de Visual Studio 2017, plusieurs versions sont disponibles, y compris une version gratuite, (Visual Studio 2017 Community Edition), et les autres versions offrent les essais gratuits. Vous pouvez les télécharger ici : [https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


Pour lancer le Concepteur de manifeste :
<!-- 1. Use Visual Studio to open a UWP project.
2. In the **Solution Explorer**, double-click the package.appmanifest file. 

    ![The Visual Studio 2017 Solution Explorer](images/icons/vs-solution-explorer.png)

    Visual Studio displays the manifest designer.

    ![The Visual Studio 2017 manifest designer](images/icons/vs-manfiest-designer.png)
3. Click the **Visual Assets** tab.

    ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png) -->


:::row:::
    :::column:::
        1. Utilisez Visual Studio pour ouvrir un projet UWP.
    :::column-end:::
    :::column:::
        
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        2. Dans le **l’Explorateur de solutions**, double-cliquez sur le fichier Package.appmxanifest.
    :::column-end:::
    :::column:::
        ![The Visual Studio 2017 Manifest Designer](images/icons/vs-solution-explorer.png)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
            Visual Studio displays the Manifest Designer.
    :::column-end:::
    :::column:::
            ![The Visual Assets tab](images/icons/vs-manfiest-designer.png)
    :::column-end:::
:::row-end:::    
:::row:::
    :::column:::
        3. Cliquez sur le **ressources visuelles** onglet.
    :::column-end:::
    :::column:::
        ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>Génération de toutes les ressources à la fois

Le premier menu d’élément dans le **ressources visuelles** onglet, **toutes les ressources visuelles**, exactement ce que son nom l’indique : génère toutes ressources visuelles de votre application a besoin en appuyant sur un bouton.

![Générer toutes les ressources visuelles dans Visual Studio](images/app-icons/all-visual-assets.png)

Il vous suffit de faire est de fournir une image unique, et Visual Studio génère la petite vignette, vignette moyenne, grande vignette, vignette large, grande vignette, icône de l’application, l’écran de démarrage et empaqueter des ressources de logo pour chaque facteur d’échelle.

Pour générer tous les éléments multimédias à la fois :
1. Cliquez sur le **...**  à côté du **Source** champ, puis sélectionnez l’image que vous souhaitez utiliser. Si vous utilisez une image bitmap, assurez-vous qu’il est au moins de 400 x 400 pixels afin que vous obtenez des résultats sharp. Images vectorielles fonctionnent le mieux ; Visual Studio vous permet d’utiliser l’intelligence artificielle (Adobe Illustrator) et des fichiers PDF. 
2. (Facultatif). Dans le **paramètres d’affichage** section, configurez ces options :

    a.  **Nom court**:  Spécifiez un nom court pour votre application.

    b.  **Afficher le nom**: Indiquer si vous souhaitez afficher le nom court sur medium, large, ou grandes vignettes. 

    c. **Vignette d’arrière-plan**: Spécifiez la valeur hexadécimale ou un nom de couleur pour la couleur d’arrière-plan de mosaïque. Exemple : `#464646`. La valeur par défaut est `transparent`.

    d. **Arrière-plan de l’écran NVIDIA**: Spécifiez le nom de la valeur ou la couleur hexadécimal pour l’arrière-plan d’écran de NVIDIA. 

3. Cliquez sur **générer**. 

Visual Studio génère les fichiers de votre image et les ajoute au projet. Si vous souhaitez modifier vos ressources, il suffit de répéter le processus. 

Ressources de l’icône de mise à l’échelle suivent cette convention d’affectation de noms de fichier :

*nom de fichier*- mise à l’échelle -*facteur d’échelle*.png

Par exemple,

Square150x150Logo-mise à l’échelle-100.png, Square150x150Logo-mise à l’échelle-200.png, Square150x150Logo-mise à l’échelle-400.png

Notez que Visual Studio ne génère pas un logo de badge par défaut. C’est parce que votre logo du badge est unique et sans doute ne doit pas correspondre à vos autres icônes d’application. Pour plus d’informations, consultez le [Badge des notifications pour l’article d’applications UWP](/windows/uwp/design/shell/tiles-and-notifications/badges). 


## <a name="more-about-app-icon-assets"></a>Plus d’informations sur les ressources de l’icône application
Visual Studio va générer toutes les ressources de l’icône de l’application requis par votre projet, mais si vous souhaitez que les personnaliser, il est utile de comprendre comment ils sont différents à partir d’autres ressources de l’application. 

La ressource d’icône application apparaît dans de nombreux endroits : la barre des tâches Windows, la vue des tâches, ALT + TAB et le coin inférieur droit des vignettes de début. Étant donné que la ressource d’icône application apparaît dans de nombreuses situations, il a certains dimensionnement supplémentaire et options n’ont pas les autres ressources de placage : actifs de la « taille de la cible » et « sans plaque » actifs. 

### <a name="target-size-app-icon-assets"></a>Ressources de l’icône application de la taille de la cible
Outre les tailles de facteur d’échelle standard (« Square44x44Logo.scale-400.png »), nous vous recommandons également de création d’éléments multimédias de « taille de la cible ». Nous appelons ces ressources-taille de la cible, car elles ciblent des tailles spécifiques, telles que les pixels de 16, plutôt que les facteurs d’échelle spécifiques, tels que 400. Composants de la taille de la cible sont pour les surfaces qui n’utilisent pas le système de plateau mise à l’échelle :

* Liste des raccourcis du menu Démarrer (bureau)
* Coin inférieur de la vignette du menu Démarrer (bureau)
* Raccourcis (bureau)
* Panneau de configuration (bureau)

Voici la liste des composants de la taille de la cible :


| Taille de la ressource | Exemple de nom de fichier                  |
|------------|------------------------------------|
| 16x16\*    | Square44x44Logo.targetsize-16.png  |
| 24x24\*    | Square44x44Logo.targetsize-24.png  |
| 32x32\*    | Square44x44Logo.targetsize-32.png  |
| 48x48\*    | Square44x44Logo.targetsize-48.png  |
| 256x256\*  | Square44x44Logo.targetsize-256.png |
| 20 x 20      | Square44x44Logo.targetsize-20.png  |
| 30 x 30      | Square44x44Logo.targetsize-30.png  |
| 36 x 36      | Square44x44Logo.targetsize-36.png  |
| 40 x 40      | Square44x44Logo.targetsize-40.png  |
| 60 x 60      | Square44x44Logo.targetsize-60.png  |
| 64 x 64      | Square44x44Logo.targetsize-64.png  |
| 72 x 72      | Square44x44Logo.targetsize-72.png  |
| 80 x 80      | Square44x44Logo.targetsize-80.png  |
| 96 x 96      | Square44x44Logo.targetsize-96.png  |

\* Au minimum, nous vous recommandons de fournir ces tailles. 

Vous n’avez pas besoin d’ajouter du remplissage à ces ressources ; Windows s’en charge le cas échéant. Ces ressources doivent présenter un encombrement minimal de 16 pixels. 

Voici un exemple de ces ressources telles qu’elles apparaissent dans les icônes de la barre des tâches Windows :

![Ressources dans la barre des tâches Windows](images/assetguidance21.png)

### <a name="unplated-assets"></a>Ressources sans plaque
Par défaut, Windows utilise une ressource basée sur la cible sur un plateau de couleur par défaut. Si vous le souhaitez, vous pouvez fournir une ressource sans plaque sur la cible. « Sans plaque » signifie que l’élément multimédia s’affiche sur un arrière-plan transparent. N’oubliez pas que ces ressources apparaîtront sur une variété de couleurs d’arrière-plan. 

![Ressources avec et sans plaque](images/assetguidance22.png)

Voici les surfaces qui utilisent des ressources de l’application sans plaque icône :
* Barre des tâches et miniature de la barre des tâches (bureau)
* Liste des raccourcis de la barre des tâches
* Applications actives
* ALT+TAB


### <a name="target-and-unplated-sizing"></a>Cible et dimensionnement sans plaque

Voici les recommandations de taille pour les ressources sur la cible, à l’échelle de 100 % :

![Dimensionnement des ressources basées sur la cible à l’échelle 100 %](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>Plus d’informations sur les ressources d’écran de démarrage
Pour plus d’informations sur les écrans de démarrage, consultez le [UWP démarrage écrans article](/windows/uwp/launch-resume/splash-screens).

## <a name="more-about-badge-logo-assets"></a>Plus d’informations sur les ressources de logo de badge

Lorsque vous utilisez le Générateur de ressources pour générer toutes les ressources que vous avez besoin, il existe une raison pourquoi il ne génère pas les logos de badge par défaut : ils sont très différents à partir d’autres ressources de l’application. Le logo de badge est une image d’état qui s’affiche dans les notifications et sur les vignettes de l’application. 

Pour plus d’informations, consultez le [Badge des notifications pour l’article d’applications UWP](/windows/uwp/design/shell/tiles-and-notifications/badges).


## <a name="customizing-asset-padding"></a>Personnalisation de remplissage de l’élément multimédia

Par défaut, le Générateur de ressources Visual Studio applique remplissage recommandé à toute image. Si vos images contiennent déjà la marge intérieure, ou vous souhaitez que les images pleine page qui s’étendent à la fin de la vignette, vous pouvez désactiver cette fonctionnalité en désactivant le **appliquer recommandé remplissage** case à cocher. 

### <a name="tile-padding-recommendations"></a>Recommandations de remplissage de vignette
Si vous souhaitez fournir votre propre remplissage, Voici nos recommandations pour les vignettes. 

Il existe 4 tailles de vignettes : petit (71 x 71), moyenne (150 x 150), à l’échelle (310 x 150) et grand (310 x 310). 

Chaque ressource de vignette a la même taille que la vignette sur laquelle elle est positionnée.

![Vignette affichant toute la surface](images/app-icons/tile-assets1.png)

Si vous ne souhaitez pas l’icône d’étendre à la périphérie de la vignette, vous pouvez utiliser les pixels transparents dans votre élément multimédia pour créer le remplissage. 

![Vignette et plaque de fond](images/assetguidance05.png)

Pour les petites vignettes, limitez la largeur et la hauteur de l’icône à 66 % de la taille de la vignette :

![Ratios de dimensionnement d’une petite vignette](images/assetguidance09.png)

Pour les vignettes moyennes, limitez la largeur de l’icône à 66 % et la hauteur de l’icône à 50 % de la taille de la vignette. Cela empêche le chevauchement des éléments dans la barre de marque :

![Ratios de dimensionnement d’une vignette moyenne](images/assetguidance10.png)

Pour les vignettes larges, limitez la largeur de l’icône à 66 % et la hauteur de l’icône à 50 % de la taille de la vignette. Cela empêche le chevauchement des éléments dans la barre de marque :

![Ratios de dimensionnement d’une vignette large](images/assetguidance11.png)

Pour les grandes vignettes, limitez la largeur de l’icône à 66 % et la hauteur de l’icône à 50 % de la taille de la vignette :

![Ratios de dimensionnement d’une grande vignette](images/assetguidance12.png)

Certaines icônes sont conçues pour être positionnées à l’horizontale ou à la verticale, tandis que certaines ont des formes plus complexes qui les empêchent de respecter les dimensions cibles. Les icônes qui semblent centrées peuvent déborder d’un côté. Dans ce cas, certaines parties de l’icône peuvent se retrouver hors de l’emplacement recommandé, à condition que l’icône offre le même attrait visuel qu’une icône aux bonnes dimensions :

![Trois icônes centrées](images/assetguidance13.png)

Pour les ressources pleine page, prenez en compte les éléments qui interagissent avec les marges et les bords des vignettes. Conservez des marges équivalentes à au moins 16 % de la hauteur ou de la largeur de la vignette. Ce pourcentage représente le double de la largeur des marges pour la plus petite taille de vignette :

![Vignette pleine page avec marges](images/assetguidance14.png)

Dans cet exemple, les marges sont trop étroites :

![Vignette pleine page avec marges trop étroites](images/assetguidance15.png)









