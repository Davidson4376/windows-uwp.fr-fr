---
Description: Comment créer des icônes et logos représentant votre application dans le menu Démarrer, les vignettes, la barre des tâches, le Microsoft Store et bien plus encore.
title: Icônes et logos d’application
template: detail.hbs
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0801ba9036f69aef340881b9c92807e80af6b09f
ms.sourcegitcommit: e43bc20c2f6e9375f61931c2fce95f06fd1f31df
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2019
ms.locfileid: "70212069"
---
# <a name="app-icons-and-logos"></a>Icônes et logos d’application 

Chaque application dispose d’une icône ou d’un logo associés qui la représentent, et apparaissent à plusieurs endroits dans l’environnement Windows : 

:::row:::
    :::column:::
        * Liste des applications dans le menu Démarrer
        * Barre des tâches et Gestionnaire des tâches
        * Vignettes de l’application
        * Écran de démarrage de l’application
        * Microsoft Store
    :::column-end:::
    :::column:::
        ![windows 10 start and tiles](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

Cet article décrit les concepts de base de la création d’icônes d’application, comment utiliser Visual Studio pour gérer celles-ci et comment les gérer manuellement si nécessaire
 
(cet article a spécialement trait aux icônes qui représentent l’application proprement dite ; pour des conseils concernant les icônes en général, voir l’article [Icônes](icons.md)).

## <a name="icon-types-locations-and-scale-factors"></a>Types, emplacements et facteurs d’échelle des icônes

Par défaut, Visual Studio stocke vos ressources d’icône dans un sous-répertoire de ressources. Voici la liste des différents types d’icônes, des emplacements où elles apparaissent et de leur noms. 

| Nom de l’icône | Apparaît dans | Nom du fichier de ressource |
| ---      | ---        | --- |
| Petite vignette | Menu Démarrer |  SmallTile.png  |
| Vignette moyenne |Menu Démarrer, Description dans le Microsoft Store\*  |  Square150x150Logo.png |
| Vignette large  | Menu Démarrer   | Wide310x150Logo.png |
| Grande vignette   | Menu Démarrer, Description dans le Microsoft Store\* |  LargeTile.png  |
| Icône d’application | Liste d’applications dans le menu Démarrer, barre des tâches, Gestionnaire des tâches | Square44x44Logo.png |
| Écran de démarrage | Écran de démarrage de l’application | SplashScreen.png  |
| Logo du badge | Vignettes de l’application | BadgeLogo.png  |
| Logo de package/Logo du Store | Programme d’installation, Espace partenaires, option « Signaler une application » dans le Store, option « Rédiger un avis » dans le Store | StoreLogo.png  |

\* Utilisé, sauf si vous choisissez d’[afficher uniquement des images chargées dans le Store](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store). 

Pour garantir que ces icônes s’affichent nettement sur chaque écran, vous pouvez créer plusieurs versions d’une même icône en fonction de différents facteurs d’échelle. 

Le facteur d’échelle détermine la taille d’éléments d’interface, tels que le texte. Les facteurs d’échelle sont compris entre 100 % et 400 %. Plus la valeur est élevée, plus les éléments d’interface utilisateur sont volumineux, ce qui augmente leur visibilité sur des écrans haute résolution. 

:::row:::
   :::column:::
      Windows définit automatiquement un facteur d’échelle pour chaque affichage en fonction de son nombre de PPP (points par pouce) et de la distance de visualisation de l’appareil. 
      (Les utilisateurs peuvent remplacer la valeur par défaut en accédant à la page **Paramètres &gt; Afficher &gt; Mise à l’échelle et disposition**.)
   :::column-end:::
   :::column:::
      ![](images/icons/display-settings-screen.png)
   :::column-end:::
:::row-end:::  


Étant donné que les ressources d’icône d’application sont des images bitmap dont l’échelle ne s’adapte pas bien, nous vous recommandons de fournir une version de chaque icône pour chaque facteur d’échelle : 100 %, 125 %, 150 %, 200 % et 400 %. Cela fait beaucoup d’icônes ! Heureusement, Visual Studio fournit un outil qui facilite la génération et la mise à jour de ces icônes. 

## <a name="microsoft-store-listing-image"></a>Image de description dans le Microsoft Store

« Comment spécifier des images pour mes applications dans la Description dans le Store ? »

Par défaut, nous utilisons certaines images de vos packages dans le Store, comme décrit dans le tableau en haut de cette page (ainsi que d’autres [images que vous fournissez pendant le processus de soumission](https://docs.microsoft.com/en-us/windows/uwp/publish/app-screenshots-and-images)). Cependant, lorsque vous présentez votre description aux clients sur Windows 10 (y compris Xbox), vous avez également la possibilité d’empêcher le Store d’utiliser les images de logo disponibles dans les packages de votre application et de faire en sorte qu’il utilise uniquement les images que vous chargez. Cela vous permet de mieux contrôler l’apparence des différents affichages de votre application au sein du Store (notez que, si votre produit prend en charge des versions antérieures du système d’exploitation, certains clients pourraient continuer à voir les images extraites de vos packages, même si vous utilisez cette option). Vous pouvez faire cela dans la section **Logos du Store** de l’étape **Description dans le Store** du processus de soumission.

![Spécification des Logos du Store pendant le processus de soumission d’application](images/app-icons/storelogodisplay.png)

Lorsque vous activez cette case, une nouvelle section appelée **Affichage des images par le Windows Store** s’affiche. Vous pouvez y charger 3 tailles d’image que le Store utilisera à la place des images de logo extraites des packages de votre application : 300 x 300, 150 x 150 et 71 x 71 pixels. Seule la taille de 300 x 300 est requise, mais nous vous conseillons de fournir les 3 tailles.

Pour plus d’informations, voir [Afficher uniquement des images de logo chargées dans le Store](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store).

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>Gestion des icônes d’application avec le Concepteur de manifeste Visual Studio

Visual Studio fournit un outil très utile pour la gestion des icônes d’application appelé **Concepteur de manifeste**. 

> Si vous ne disposez pas de Visual Studio 2019, plusieurs versions sont disponibles, dont une gratuite (Visual Studio 2019 Community Edition), les autres versions proposant des essais gratuits. Vous pouvez les télécharger ici : [https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


Pour lancer le Concepteur de manifeste :
<!-- 1. Use Visual Studio to open a UWP project.
2. In the **Solution Explorer**, double-click the package.appmanifest file. 

    ![The Visual Studio 2019 Solution Explorer](images/icons/vs-solution-explorer.png)

    Visual Studio displays the manifest designer.

    ![The Visual Studio 2019 manifest designer](images/icons/vs-manfiest-designer.png)
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
        2. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier Package.appmxanifest.
    :::column-end:::
    :::column:::
        ![The Visual Studio 2019 Manifest Designer](images/icons/vs-solution-explorer.png)
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
        3. Cliquez sur l’onglet **Ressources visuelles**.
    :::column-end:::
    :::column:::
        ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>Génération de toutes les ressources à la fois

La première option de menu sous l’onglet **Ressources visuelles**, **Toutes les ressources visuelles**, fait exactement ce que son nom indique. Elle vous permet de générer toutes ressources visuelles dont votre application a besoin en appuyant sur un bouton.

![Générer toutes les ressources visuelles dans Visual Studio](images/app-icons/all-visual-assets.png)

Il vous suffit de fournir une image. Ensuite, Visual Studio génère la petite vignette, la vignette moyenne, la grande vignette, la vignette large, l’icône de l’application, l’écran de démarrage et les ressources de logo du package pour chaque facteur d’échelle.

Pour générer toutes les ressources à la fois :
1. Cliquez sur **...**  en regard du champ **Source**, puis sélectionnez l’image à utiliser. Si vous utilisez une image bitmap, assurez-vous que sa taille est d’au moins 400 x 400 pixels afin d’obtenir des résultats nets. Ce sont les images vectorielles qui fonctionnent le mieux. Visual Studio vous permet d’utiliser l’intelligence artificielle (Adobe Illustrator) et des fichiers PDF. 
2. (Facultatif) Dans la section **Paramètres d’affichage**, configurez les options suivantes :

    a.  **Nom court** :  spécifiez un nom court pour votre application.

    b.  **Afficher le nom** : indiquez si vous souhaitez afficher le nom court sur les vignettes de petite taille, de taille et moyenne ou de grande taille. 

    c. **Arrière-plan de la vignette** : spécifiez la valeur hexadécimale ou le nom de la couleur d’arrière-plan de la vignette. Exemple : `#464646`. La valeur par défaut est `transparent`.

    d. **Arrière-plan de l’écran de démarrage** : spécifiez la valeur hexadécimale ou le nom de la couleur d’arrière-plan de l’écran d’accueil. 

3. Cliquez sur **Générer**. 

Visual Studio génère les fichiers de votre image et les ajoute au projet. Si vous souhaitez modifier vos ressources, répétez simplement le processus. 

Les ressources d’icône mises à l’échelle suivent cette convention d’affectation de noms de fichier :

*filename*-scale-*scale factor*.png

Par exemple,

Square150x150Logo-scale-100.png, Square150x150Logo-scale-200.png, Square150x150Logo-scale-400.png

Notez que Visual Studio ne génère pas de logo du badge par défaut. C’est parce que votre logo du badge est unique et ne doit probablement pas correspondre à vos autres icônes d’application. Pour plus d’informations, voir l’article [Notifications de badge pour les applications UWP](/windows/uwp/design/shell/tiles-and-notifications/badges). 


## <a name="more-about-app-icon-assets"></a>Informations supplémentaires sur les ressources d’icônes d’application
Visual Studio va générer toutes les ressources d’icônes de l’application requises pour votre projet mais, si vous souhaitez les personnaliser, il est utile de comprendre en quoi elles diffèrent d’autres ressources de l’application. 

La ressource d’icône d’application apparaît dans de nombreux endroits : barre des tâches Windows, affichage des tâches, Alt+Tab et angle inférieur droit des vignettes de démarrage. Étant donné que la ressource d’icône d’application apparaît dans tant d’emplacements, elle offre des options de dimensionnement et de plaque supplémentaires que n’ont pas les autres ressources : des ressources de « taille cible » et de ressources « sans plaque ». 

### <a name="target-size-app-icon-assets"></a>Ressources d’icône application de taille cible
Outre les tailles de facteur d’échelle standard (« Square44x44Logo.scale-400.png »), nous vous suggérons de créer des ressources de « taille cible ». Nous les appelons de cette manière parce qu’elles ciblent des tailles spécifiques, telles 16 pixels, plutôt que des facteurs d’échelle spécifiques, tels que 400. Les ressources de taille cible sont destinées aux surfaces qui n’utilisent pas le système de plateau de mise à l’échelle :

* Liste des raccourcis du menu Démarrer (bureau)
* Coin inférieur de la vignette du menu Démarrer (bureau)
* Raccourcis (bureau)
* Panneau de configuration (bureau)

Voici la liste des ressources de taille cible :


| Taille de la ressource | Exemple de nom de fichier                  |
|------------|------------------------------------|
| 16 x 16\*    | Square44x44Logo.targetsize-16.png  |
| 24 x 24\*    | Square44x44Logo.targetsize-24.png  |
| 32 x 32\*    | Square44x44Logo.targetsize-32.png  |
| 48 x 48\*    | Square44x44Logo.targetsize-48.png  |
| 256 x 256\*  | Square44x44Logo.targetsize-256.png |
| 20 x 20      | Square44x44Logo.targetsize-20.png  |
| 30 x 30      | Square44x44Logo.targetsize-30.png  |
| 36 x 36      | Square44x44Logo.targetsize-36.png  |
| 40 x 40      | Square44x44Logo.targetsize-40.png  |
| 60 x 60      | Square44x44Logo.targetsize-60.png  |
| 64 x 64      | Square44x44Logo.targetsize-64.png  |
| 72 x 72      | Square44x44Logo.targetsize-72.png  |
| 80 x 80      | Square44x44Logo.targetsize-80.png  |
| 96 x 96      | Square44x44Logo.targetsize-96.png  |

\* Nous vous conseillons de fournir au minimum ces tailles. 

Vous n’avez pas besoin d’ajouter du remplissage à ces ressources ; Windows s’en charge le cas échéant. Ces ressources doivent présenter un encombrement minimal de 16 pixels. 

Voici un exemple de ces ressources telles qu’elles apparaissent dans les icônes de la barre des tâches Windows :

![Ressources dans la barre des tâches Windows](images/assetguidance21.png)

### <a name="unplated-assets"></a>Ressources sans plaque
Par défaut, Windows utilise une ressource basée sur une cible superposée à une plaque de couleur. Si vous le souhaitez, vous pouvez fournir une ressource sans plaque basée sur une cible. « Sans plaque » signifie que la ressource s’affiche sur un arrière-plan transparent. N’oubliez pas que ces ressources apparaîtront sur une diverses couleurs d’arrière-plan. 

![Ressources avec et sans plaque](images/assetguidance22.png)

Voici les surfaces qui utilisent des ressources d’icône d’application sans plaque :
* Barre des tâches et miniature de la barre des tâches (bureau)
* Liste des raccourcis de la barre des tâches
* Applications actives
* ALT+TAB

### <a name="unplated-assets-and-themes"></a>Ressources et thèmes sans plaque

Le thème sélectionné de l’utilisateur détermine la couleur de la barre des tâches. Si l’élément multimédia sans plaque n’est pas spécifiquement qualifié pour le thème actuel, le système vérifie la ressource pour le contraste. Si le contraste avec la barre des tâches est suffisant, le système utilise la ressource. Sinon, le système recherche une version hautement contrastée de la ressource. S’il n’en trouve pas, le système dessine la forme sur plaque de la ressource à la place. 


### <a name="target-and-unplated-sizing"></a>Dimensionnement cible et sans plaque

Voici des recommandations de taille pour les ressources basées sur une cible, à l’échelle de 100 % :

![Dimensionnement des ressources basées sur la cible à l’échelle 100 %](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>Informations supplémentaires sur les ressources d’écran de démarrage
Pour plus d’informations sur les écrans de démarrage, voir l’article [Écrans de démarrage UWP](/windows/uwp/launch-resume/splash-screens).

## <a name="more-about-badge-logo-assets"></a>Informations supplémentaires sur les ressources de logo de badge

Lorsque vous utilisez le Générateur de ressources pour générer toutes les ressources dont vous avez besoin, il ne génère pas les logos de badge par défaut  parce qu’ils sont très différents d’autres ressources d’application. Le logo de badge est une image d’état qui s’affiche dans les notifications et sur les vignettes de l’application. 

Pour plus d’informations, voir l’article [Notifications de badge pour les applications UWP](/windows/uwp/design/shell/tiles-and-notifications/badges).


## <a name="customizing-asset-padding"></a>Personnalisation de la marge intérieure de ressource

Par défaut, le Générateur de ressources Visual Studio recommande d’appliquer une marge intérieure à toute image. Si vos images contiennent déjà une marge intérieure ou si vous souhaitez des images pleine page qui s’étendent jusqu’au bord de la vignette, vous pouvez désactiver cette fonctionnalité en décochant la case à cocher **Appliquer le remplissage recommandé**. 

### <a name="tile-padding-recommendations"></a>Recommandations de marge intérieure de vignette
Si vous souhaitez spécifier votre propre marge intérieure, voici nos recommandations pour les vignettes. 

Il existe 4 tailles de vignettes : petite (71 x 71), moyenne (150 x 150), large (310 x 150) et grande (310 x 310). 

Chaque ressource de vignette a la même taille que la vignette sur laquelle elle est positionnée.

![Vignette pleine page](images/app-icons/tile-assets1.png)

Si vous ne souhaitez pas que votre icône s’étende jusqu’au bord de la vignette, vous pouvez utiliser des pixels transparents dans votre ressource pour créer une marge intérieure. 

![Vignette et plaque de fond](images/assetguidance05.png)

Pour les petites vignettes, limitez la largeur et la hauteur de l’icône à 66 % de la taille de la vignette :

![Ratios de dimensionnement d’une petite vignette](images/assetguidance09.png)

Pour les vignettes moyennes, limitez la largeur de l’icône à 66 % et la hauteur de l’icône à 50 % de la taille de la vignette. Cela empêche le chevauchement des éléments dans la barre de marque :

![Ratios de dimensionnement d’une vignette moyenne](images/assetguidance10.png)

Pour les vignettes larges, limitez la largeur de l’icône à 66 % et la hauteur de l’icône à 50 % de la taille de la vignette. Cela empêche le chevauchement des éléments dans la barre de marque :

![Ratios de dimensionnement d’une vignette large](images/assetguidance11.png)

Pour les grandes vignettes, limitez la largeur de l’icône à 66 % et la hauteur à 50 % de la taille de la vignette :

![Ratios de dimensionnement d’une grande vignette](images/assetguidance12.png)

Certaines icônes sont conçues pour être positionnées à l’horizontale ou à la verticale, tandis que certaines ont des formes plus complexes qui les empêchent de respecter les dimensions cibles. Les icônes qui semblent centrées peuvent déborder d’un côté. Dans ce cas, certaines parties de l’icône peuvent se retrouver hors de l’emplacement recommandé, à condition que l’icône offre le même attrait visuel qu’une icône aux bonnes dimensions :

![Trois icônes centrées](images/assetguidance13.png)

Pour les ressources pleine page, prenez en compte les éléments qui interagissent avec les marges et les bords des vignettes. Conservez des marges équivalentes à au moins 16 % de la hauteur ou de la largeur de la vignette. Ce pourcentage représente le double de la largeur des marges pour la plus petite taille de vignette :

![Vignette pleine page avec marges](images/assetguidance14.png)

Dans cet exemple, les marges sont trop étroites :

![Vignette pleine page avec marges trop étroites](images/assetguidance15.png)


## <a name="optimizing-for-specific-themes-languages-and-other-conditions"></a>Optimisation pour des thèmes, langues et autres conditions spécifiques 

Cet article décrit comment créer des ressources pour des facteurs d’échelle spécifiques, mais vous pouvez également créer des ressources pour un vaste éventail de conditions et combinaisons de conditions. Par exemple, vous pouvez peut créer des icônes pour des écrans très contrastés ou pour des thèmes clairs et sombres. Vous pouvez même créer des ressources pour des langues spécifiques.

Pour en savoir plus, voir [Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md).













