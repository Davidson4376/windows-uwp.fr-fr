---
author: mijacobs
Description: How to create app icons/logos that represent your app in the Start menu, app tiles, the taskbar, the Microsoft Store, and more.
title: Les logos et icônes d’application
template: detail.hbs
ms.author: mijacobs
ms.date: 04/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e947b00c3a070a8d95a21e38c56bda07cd45d3c4
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "4020413"
---
# <a name="app-icons-and-logos"></a>Les logos et icônes d’application 

Chaque application dispose d’un icône/logo qui le représente, et elle apparaît dans plusieurs emplacements dans l’interpréteur de commandes Windows: 

:::row:::
    :::column:::
        * La barre de titre de la fenêtre de votre application * la liste des applications dans le menu Démarrer * le Gestionnaire de la barre des tâches et tâche * les vignettes de votre application * écran de démarrage de votre application * dans le Microsoft Store :::column-end:::
    :::column:::
        ![Écran d’accueil et vignettes de Windows10](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

Cet article décrit les principes fondamentaux de la création d’icônes d’application, l’utilisation de Visual Studio pour les gérer et comment les gérer manuellement, si nécessaire.
 
(Cet article est spécifiquement pour les icônes qui représentent l’application proprement dite; pour obtenir des conseils généraux icône, consultez l’article [icônes](icons.md) .)

## <a name="icon-types-locations-and-scale-factors"></a>Types d’icônes, emplacements et facteurs d’échelle

Par défaut, Visual Studio stocke vos ressources d’icônes dans un sous-répertoire de ressources. Voici une liste des différents types d’icônes, où ils s’affichent, et qu’ils sont appelés. 

| Nom de l’icône | S’affiche dans | Nom de fichier de ressources |
| ---      | ---        | --- |
| Petite vignette | Menu Démarrer |  SmallTile.png  |
| Vignette moyenne |Menu Démarrer, Microsoft Store listing\ *  |  Square150x150Logo.png |
| Vignette large  | Menu Démarrer   | Wide310x150Logo.png |
| Grande vignette   | Menu Démarrer, Microsoft Store listing\ * |  LargeTile.png  |
| Icône de l’application | Liste des applications dans le menu Démarrer, barre des tâches, Gestionnaire des tâches | Square44x44Logo.png |
| Écran de démarrage | Écran de démarrage de l’application | SplashScreen.png  |
| Logo du badge | Vignettes de votre application | BadgeLogo.png  |
| Logo de logo/Store de package | Programme d’installation application, le centre de développement, l’option «Signaler une application» dans le Windows Store, l’option «Donner votre avis» dans le Windows Store | StoreLogo.png  |

\ * Utilisé, sauf si vous choisissez [d’Afficher uniquement les images dans le Windows Store chargées](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store). 

Pour vous assurer que ces icônes paraissent sur chaque écran, vous pouvez créer plusieurs versions de la même icône pour afficher différents facteurs d’échelle. 

Le facteur d’échelle détermine la taille des éléments d’interface utilisateur, tel que le texte. Plage de facteurs mise à l’échelle de 100 % et 400 %. Les valeurs créent des éléments d’interface utilisateur plus grandes, ce qui les rend plus visible sur des affichages haute résolution. 

:::row:::
    :::column:::
        Windows définit automatiquement le facteur d’échelle pour chaque affichage en fonction de son nombre de PPP (points par pouce) et la distance de visualisation de l’appareil. 

        (Users can override the default value by going to the **Settings &gt; Display &gt; Scale and layout** page.)
    :::column-end:::
    :::column:::
        ![](images/icons/display-settings-screen.png)
    :::column-end:::
:::row-end:::  


Dans la mesure où les ressources d’icône d’application sont des images bitmap et images bitmap évoluent peu, nous vous recommandons de fournir une version chaque ressource d’icône pour chaque facteur d’échelle: 100 %, 125 %, 150 %, 200 % et 400 %. C’est un grand nombre d’icônes! Fortunatly, Visual Studio fournit un outil qui facilite la générer et mettre à jour ces icônes. 

## <a name="microsoft-store-listing-image"></a>Image de description dans le Microsoft Store

«Comment puis-je spécifier des images pour la description de mon application dans le Microsoft Store?»

Par défaut, nous utilisons certaines images à partir de vos packages dans le Windows Store, comme décrit dans le tableau en haut de cette page (ainsi que d’autres [images que vous fournissez au cours du processus de soumission](https://docs.microsoft.com/en-us/windows/uwp/publish/app-screenshots-and-images)). Toutefois, vous avez la possibilité d’empêcher le Store d’utiliser les images de logo disponibles dans les packages de votre application lorsque vous présentez votre description aux clients sur Windows 10 (y compris Xbox) et à la place le Windows Store utilise uniquement les images que vous chargez. Cela vous donne davantage de contrôle sur l’apparence de votre application sur différents affichages partout dans le Store. (Notez que si votre produit prend en charge les versions antérieures du système d’exploitation, les clients peuvent toujours voir des images à partir de vos packages, même si vous utilisez cette option). Vous pouvez le faire dans la section **logos Windows Store** de l’étape du processus de soumission de **description dans le Store** .

![En spécifiant les logos Windows Store au cours du processus de soumission d’application](images/app-icons/storelogodisplay.png)

Lorsque vous cochez cette case, une nouvelle section appelée **magasin afficher des images** s’affiche. Ici, vous pouvez charger 3 tailles d’image que le Windows Store utilise à la place d’images de logo dans les packages de votre application: 71 x 71, 150 x 150 et 300 x 300 pixels. Seule la taille de 300 x 300 est nécessaire, bien que nous vous recommandons de fournir tous les 3 tailles.

Pour plus d’informations, voir [l’affichage chargé uniquement les images de logo disponibles dans le Windows Store](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store).

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>La gestion des icônes d’application avec le Concepteur de manifeste de Visual Studio

Visual Studio fournit un outil très utile pour la gestion de vos icônes d’application appelés le **Concepteur de manifeste**. 

> Si vous n’avez pas encore Visual Studio 2017, il existe plusieurs versions disponibles, y compris une version gratuite, (Visual Studio 2017 Community Edition), et les autres versions de proposent des essais gratuits. Vous pouvez les télécharger ici:[https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


Pour lancer le Concepteur de manifeste:
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
        2. dans l' **Explorateur de solutions**, double-cliquez sur le fichier Package.appmxanifest.
    :::column-end:::
    :::column:::
        ![Le Concepteur de manifeste Visual Studio 2017](images/icons/vs-solution-explorer.png)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
            Visual Studio affiche le Concepteur de manifeste.
    :::column-end:::
    :::column:::
            ![L’onglet ressources visuelles](images/icons/vs-manfiest-designer.png)
    :::column-end:::
:::row-end:::    
:::row:::
    :::column:::
        3. Cliquez sur l’onglet **Ressources visuelles** . :::column-end:::
    :::column:::
        ![L’onglet ressources visuelles](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>Génération de toutes les ressources à la fois

Le premier élément de menu dans l’onglet **Ressources visuelles** , **Toutes les ressources visuelles**, est exactement ce que son nom l’indique: génère toutes les ressources visuelles votre application a besoin d’appui sur un bouton.

![Générer tous les éléments visuels dans Visual Studio](images/app-icons/all-visual-assets.png)

Il vous souhaitez bloc est une image unique, et Visual Studio génère la petite vignette, vignette moyenne, grande vignette, vignette large, grande vignette, icône de l’application, écran de démarrage et ressources de logo pour chaque facteur d’échelle de package.

Pour générer toutes les ressources à la fois:
1. Cliquez sur le bouton **...** en regard du champ **Source** , puis sélectionnez l’image que vous souhaitez utiliser. Si vous utilisez une image bitmap, assurez-vous qu’il est au moins de 400 par 400 pixels afin que vous obtenez des résultats arêtes. Images vectorielles fonctionnent mieux; Visual Studio vous permet d’utiliser l’IA (Adobe Illustrator) et les fichiers PDF. 
2. (Facultatif). Dans la section **Paramètres d’affichage** , configurer les options suivantes:

    a.  **Nom court**: spécifiez un nom court de votre application.

    b.  **Afficher le nom**: indiquer si vous souhaitez afficher le nom court sur les vignettes moyenne, large ou grand. 

    c. **Vignette en arrière-plan**: spécifiez la valeur hexadécimale ou un nom de couleur pour la couleur d’arrière-plan de vignette. Exemple: `#464646`. La valeur par défaut est `transparent`.

    d. **Arrière-plan de l’écran NVIDIA**: spécifiez le nom de valeur ou de couleur hexadécimal pour l’arrière-plan d’écran NVIDIA. 

3. Cliquez sur **Générer**. 

Visual Studio génère des fichiers de votre image et les ajoute au projet. Si vous souhaitez modifier vos ressources, il suffit de répéter le processus. 

Ressources d’icônes à l’échelle suivent cette convention d’affectation de noms de fichier:

*nom de fichier*- scale -*facteur d’échelle*.png

Par exemple:

Square150x150Logo-scale-100.png, Square150x150Logo-scale-200.png, Square150x150Logo-scale-400.png

Notez que Visual Studio ne génère pas un logo du badge par défaut. C’est parce que votre logo du badge est unique et probablement ne doit pas correspondre à vos autres icônes d’application. Pour plus d’informations, voir les [notifications de Badge pour article d’applications UWP](/windows/uwp/design/shell/tiles-and-notifications/badges). 


## <a name="more-about-app-icon-assets"></a>En savoir plus sur les ressources d’icône d’application
Visual Studio ne génère pas toutes les ressources de l’icône de l’application requis par votre projet, mais si vous souhaitez les personnaliser, il est utile de comprendre la façon dont elles sont différentes à partir d’autres ressources d’application. 

La ressource d’icône application s’affiche dans un grand nombre d’emplacements: la barre des tâches de Windows, l’affichage des tâches, ALT + TAB et le coin inférieur droit des vignettes de démarrage. Étant donné que la ressource d’icône application apparaît peu partout, elle a certains dimensionnement supplémentaires et placage options n’ont pas les autres ressources: «taille de la cible» actifs et ressources «sans plaque». 

### <a name="target-size-app-icon-assets"></a>Ressources d’icône d’application de la taille de la cible
Outre les tailles de facteur d’échelle standard («square44x44logo.Scale-400.png»), nous vous recommandons également de création de ressources de la «taille de la cible». Nous appelons ces ressources-taille de la cible, car elles ciblent des tailles spécifiques, telles que 16 pixels, au lieu des facteurs d’échelle spécifique, par exemple, 400. Ressources de taille de la cible sont destinées aux surfaces qui n’utilisent pas le système de plateau de mise à l’échelle:

* Liste des raccourcis du menu Démarrer (bureau)
* Coin inférieur de la vignette du menu Démarrer (bureau)
* Raccourcis (bureau)
* Panneau de configuration (bureau)

Voici la liste des ressources de taille de la cible:


| Taille de la ressource | Exemple de nom de fichier                  |
|------------|------------------------------------|
| 16x16\*    | Square44x44Logo.targetsize-16.png  |
| 24x24\*    | Square44x44Logo.targetsize-24.png  |
| 32x32\*    | Square44x44Logo.targetsize-32.png  |
| 48x48\*    | Square44x44Logo.targetsize-48.png  |
| 256x256\*  | Square44x44Logo.targetsize-256.png |
| 20x20      | Square44x44Logo.targetsize-20.png  |
| 30x30      | Square44x44Logo.targetsize-30.png  |
| 36x36      | Square44x44Logo.targetsize-36.png  |
| 40x40      | Square44x44Logo.targetsize-40.png  |
| 60x60      | Square44x44Logo.targetsize-60.png  |
| 64x64      | Square44x44Logo.targetsize-64.png  |
| 72x72      | Square44x44Logo.targetsize-72.png  |
| 80x80      | Square44x44Logo.targetsize-80.png  |
| 96x96      | Square44x44Logo.targetsize-96.png  |

\ * Au minimum, nous vous recommandons de fournir ces tailles. 

Vous n’avez pas besoin d’ajouter du remplissage à ces ressources ; Windows s’en charge le cas échéant. Ces ressources doivent présenter un encombrement minimal de 16 pixels. 

Voici un exemple de ces ressources telles qu’elles apparaissent dans les icônes de la barre des tâches Windows:

![Ressources dans la barre des tâches Windows](images/assetguidance21.png)

### <a name="unplated-assets"></a>Ressources sans plaque
Par défaut, Windows utilise une ressource basée sur la cible sur une plaque de fond en couleur par défaut. Si vous le souhaitez, vous pouvez fournir une ressource basée sur la cible sans plaque. «Sans plaque» signifie que la ressource s’affichera sur un arrière-plan transparent. N’oubliez pas que ces ressources seront affiche sur une variété de couleurs d’arrière-plan. 

![Ressources avec et sans plaque](images/assetguidance22.png)

Voici les surfaces qui utilisent les ressources d’icône d’application sans plaque:
* Barre des tâches et miniature de la barre des tâches (bureau)
* Liste des raccourcis de la barre des tâches
* Applications actives
* Alt+Tab


### <a name="target-and-unplated-sizing"></a>Cible et dimensionnement sans plaque

Voici les recommandations de dimensionnement pour les ressources basées sur les cibles, à l’échelle 100 %:

![Dimensionnement des ressources basées sur la cible à l’échelle 100%](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>En savoir plus sur les ressources d’écran de démarrage
Pour plus d’informations sur les écrans de démarrage, consultez l' [article d’écrans de démarrage UWP](/windows/uwp/launch-resume/splash-screens).

## <a name="more-about-badge-logo-assets"></a>En savoir plus sur les ressources de logo de badge

Lorsque vous utilisez le Générateur de ressources pour générer toutes les ressources que vous avez besoin, il existe une raison pourquoi il ne génère pas de badge logos par défaut: elles sont très différentes à partir d’autres ressources d’application. Le logo du badge est une image d’état qui s’affiche dans les notifications et sur les vignettes de l’application. 

Pour plus d’informations, voir les [notifications de Badge pour article d’applications UWP](/windows/uwp/design/shell/tiles-and-notifications/badges).


## <a name="customizing-asset-padding"></a>Personnalisation de remplissage de ressource

Par défaut, Générateur de ressources de Visual Studio applique recommandée remplissage à une image. Si vos images contiennent déjà remplissage ou les images pleine qui s’étendent à la fin de la vignette, vous pouvez désactiver cette fonctionnalité en désactivant la case à cocher **Appliquer recommandé de remplissage** . 

### <a name="tile-padding-recommendations"></a>Recommandations en matière de remplissage de vignette
Si vous voulez fournir votre propre remplissage, Voici nos recommandations pour les vignettes. 

Il existe des tailles de vignettes 4: petite (71 x 71), moyenne (150 x 150), l’échelle (310 x 150) et grande (310 x 310). 

Chaque ressource de vignette a la même taille que la vignette sur laquelle elle est positionnée.

![Pleine illustrant de vignette](images/app-icons/tile-assets1.png)

Si vous ne voulez pas que votre icône d’étendre vers le bord de la vignette, vous pouvez utiliser des pixels transparents dans vos ressources pour créer le remplissage. 

![Vignette et plaque de fond](images/assetguidance05.png)

Pour les petites vignettes, limitez la largeur et la hauteur de l’icône à 66% de la taille de la vignette:

![Ratios de dimensionnement d’une petite vignette](images/assetguidance09.png)

Pour les vignettes moyennes, limitez la largeur de l’icône à 66% et la hauteur de l’icône à 50% de la taille de la vignette. Cela empêche le chevauchement des éléments dans la barre de marque:

![Ratios de dimensionnement d’une vignette moyenne](images/assetguidance10.png)

Pour les vignettes larges, limitez la largeur de l’icône à 66% et la hauteur de l’icône à 50% de la taille de la vignette. Cela empêche le chevauchement des éléments dans la barre de marque:

![Ratios de dimensionnement d’une vignette large](images/assetguidance11.png)

Pour les grandes vignettes, limitez la largeur de l’icône à 66% et la hauteur de l’icône à 50% de la taille de la vignette:

![Ratios de dimensionnement d’une grande vignette](images/assetguidance12.png)

Certaines icônes sont conçues pour être positionnées à l’horizontale ou à la verticale, tandis que certaines ont des formes plus complexes qui les empêchent de respecter les dimensions cibles. Les icônes qui semblent centrées peuvent déborder d’un côté. Dans ce cas, certaines parties de l’icône peuvent se retrouver hors de l’emplacement recommandé, à condition que l’icône offre le même attrait visuel qu’une icône aux bonnes dimensions:

![Trois icônes centrées](images/assetguidance13.png)

Pour les ressources pleine page, prenez en compte les éléments qui interagissent avec les marges et les bords des vignettes. Conservez des marges équivalentes à au moins 16 % de la hauteur ou de la largeur de la vignette. Ce pourcentage représente le double de la largeur des marges pour la plus petite taille de vignette:

![Vignette pleine page avec marges](images/assetguidance14.png)

Dans cet exemple, les marges sont trop étroites:

![Vignette pleine page avec marges trop étroites](images/assetguidance15.png)









