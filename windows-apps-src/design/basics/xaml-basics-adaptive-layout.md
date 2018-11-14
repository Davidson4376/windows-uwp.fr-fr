---
author: muhsinking
title: Didacticiel Créer des dispositions adaptatives
description: Cet article décrit les principes fondamentaux de la disposition adaptative en XAML
keywords: XAML, UWP, Prise en main
ms.author: mukin
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 000aa2d8f3684aa813b85076d9124a87a71b6a8c
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6163076"
---
# <a name="tutorial-create-adaptive-layouts"></a>Didacticiel: Créer des dispositions adaptatives

Ce didacticiel décrit les principes fondamentaux de l’utilisation des fonctionnalités de dispositions adaptatives et personnalisées du code XAML. Celles-ci vous permettent de créer des applications qui s'adaptent à n'importe quel appareil. Vous allez apprendre à créer un nouveau DataTemplate, à ajouter des points d’ancrage de fenêtre et à adapter la disposition de votre application à l’aide des éléments VisualStateManager et AdaptiveTrigger. Nous allons utiliser ces outils pour optimiser un programme d’édition d’images pour les appareils à petit écran. 

Le programme d’édition d’images sur lequel vous allez travailler possède deuxpages/écrans:

La **page principale**, qui présente un affichage de galerie de photos, ainsi que des informations sur chaque fichier d’image.

![MainPage](../basics/images/xaml-basics/mainpage.png)

La **page de détails**, qui affiche une seule photo une fois qu'elle a été sélectionnée. Un menu d'édition volant permet de modifier la photo, de la renommer et de l'enregistrer.

![DetailPage](../basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>Éléments prérequis

* Visual Studio2017: [Télécharger Visual Studio Community2017 (gratuit)](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&campaign=WinDevCenter&ocid=wdgcx-windevcenter-community-download) 
* SDK Windows10 (10.0.15063.468 ou version ultérieure): [Télécharger le dernier SDK Windows (gratuit)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Émulateur Windows mobile: [Télécharger l’Émulateur Windows10 mobile (gratuit)](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive)

## <a name="part-0-get-the-starter-code-from-github"></a>Partie 0: Obtenir le code de démarrage à partir de github

Dans ce didacticiel, vous allez commencer avec une version simplifiée de l’exemple PhotoLab. 

1. Accédez à [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab). Vous accédez à la page GitHub de l’exemple. 
2. Ensuite, vous devez cloner ou télécharger l’exemple. Cliquez sur le bouton **Cloner ou télécharger**. Un sous-menu s’affiche.
    <figure>
        <img src="../basics/images/xaml-basics/clone-repo.png" alt="The Clone or download menu on GitHub">
        <figcaption>Le menu <b>Clonage ou téléchargement</b> sur la page GitHub de l’exemple PhotoLab.</figcaption>
    </figure>

    **Si vous n’êtes pas familiarisé avec GitHub:**
    
    a. Cliquez sur **Télécharger le ZIP** et enregistrez le fichier localement. Ceci télécharge un fichier .zip contenant tous les fichiers de projet dont vous avez besoin.
    b. Extrayez le fichier. Utilisez l’Explorateur de fichiers pour accéder au fichier .zip que vous venez de télécharger, cliquez dessus avec le bouton droit et sélectionnez **Extraire tout...**. c. Naviguez vers votre copie locale de l’exemple et accédez au répertoire `Windows-appsample-photo-lab-master\xaml-basics-starting-points\adaptive-layout`.    

    **Si vous maîtrisez GitHub:**

    a. Clonez la branche maître du référentiel localement.
    b. Naviguez vers le répertoire `Windows-appsample-photo-lab\xaml-basics-starting-points\adaptive-layout`.

3. Ouvrez le projet en cliquant sur `Photolab.sln`.

## <a name="part-1-run-the-mobile-emulator"></a>Partie1: Exécuter l’émulateur mobile

Dans la barre d’outils Visual Studio, vérifiez que votre plateforme de solution est définie sur x86 ou x64 (et non ARM) et remplacez votre appareil cible Ordinateur local par l'un des émulateurs mobiles que vous avez installés (par exemple, Mobile Emulator 10.0.15063 WVGA 5pouces 1Go). Essayez d’exécuter l’application Galerie de photos dans l’émulateur mobile que vous avez sélectionné en appuyant sur **F5**.

Dès le démarrage de l’application et pendant son fonctionnement, vous constaterez probablement qu'elle ne s’adapte pas parfaitement à une si petite fenêtre d’affichage. L’élément de grille fluide tente de prendre en compte l’espace limité de l'écran en réduisant le nombre de colonnes affichées, mais le résultat est une disposition qui semble mal conçue et mal adaptée à cette petite fenêtre d’affichage.

![Disposition Mobile: après](../basics/images/xaml-basics/adaptive-layout-mobile-before.png)

## <a name="part-2-build-a-tailored-mobile-layout"></a>Partie2: générer une disposition personnalisée mobile
Pour que l'aspect de votre application soit correct sur de petits appareils, nous allons créer un ensemble de styles dans notre page XAML qui sera utilisé uniquement si un appareil mobile est détecté.

### <a name="create-a-new-datatemplate"></a>Créer un DataTemplate
Nous allons adapter l’affichage galerie de l’application en créant un DataTemplate pour les images. Ouvrez MainPage.xaml à partir de l’Explorateur de solutions, puis ajoutez le code suivant à l'intérieur des balises **Page.Resources**.

```XAML
<DataTemplate x:Key="ImageGridView_MobileItemTemplate"
              x:DataType="local:ImageFileInfo">

    <!-- Create image grid -->
    <Grid Height="{Binding ItemSize, ElementName=page}"
          Width="{Binding ItemSize, ElementName=page}">
        
        <!-- Place image in grid, stretching it to fill the pane-->
        <Image x:Name="ItemImage"
               Source="{x:Bind ImagePreview}"
               Stretch="UniformToFill">
        </Image>

    </Grid>
</DataTemplate>
```

Ce modèle de galerie économise l'espace de l'écran en éliminant la bordure autour des images et en éliminant les métadonnées d’image (nom de fichier, évaluations, etc.) situées sous chaque vignette. Chaque vignette s'affiche alors sous forme de simple carré.

### <a name="add-metadata-to-a-tooltip"></a>Ajouter des métadonnées à une info-bulle
Nous voulons que l’utilisateur puisse encore accéder aux métadonnées de chaque image, donc nous allons ajouter une info-bulle à chaque élément d’image. Ajoutez le code suivant à l'intérieur des balises **Image** de l’objet DataTemplate que vous venez de créer.

```XAML
<Image ...>

    <!-- Add a tooltip to the image that displays metadata -->
    <ToolTipService.ToolTip>
        <ToolTip x:Name="tooltip">

            <!-- Arrange tooltip elements vertically -->
            <StackPanel Orientation="Vertical"
                        Grid.Row="1">

                <!-- Image title -->
                <TextBlock Text="{x:Bind ImageTitle, Mode=OneWay}"
                           HorizontalAlignment="Center"
                           Style="{StaticResource SubtitleTextBlockStyle}" />

                <!-- Arrange elements horizontally -->
                <StackPanel Orientation="Horizontal"
                            HorizontalAlignment="Center">

                    <!-- Image file type -->
                    <TextBlock Text="{x:Bind ImageFileType}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}" />

                    <!-- Image dimensions -->
                    <TextBlock Text="{x:Bind ImageDimensions}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}"
                               Margin="8,0,0,0" />
                </StackPanel>
            </StackPanel>
        </ToolTip>
    </ToolTipService.ToolTip>
</Image>
```

Quand vous pointerez avec la souris (ou appuierez longuement dans le cas d'un écran tactile) sur la vignette, le titre, le type de fichier et les dimensions de l'image s'afficheront.

### <a name="add-a-visualstatemanager-and-statetrigger"></a>Ajouter un VisualStateManager et un StateTrigger

Nous avons maintenant créé une nouvelle disposition pour nos données, mais l’application n’a actuellement aucun moyen de savoir quand utiliser cette disposition au lieu des styles par défaut. Pour résoudre ce problème, nous devons ajouter un **VisualStateManager**. Ajoutez le code suivant à l’élément racine de la page, **RelativePanel**.

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>

        <!-- Add a new VisualState for mobile devices -->
        <VisualState x:Key="Mobile">

            <!-- Trigger visualstate when a mobile device is detected -->
            <VisualState.StateTriggers>
                <local:MobileScreenTrigger InteractionMode="Touch" />
            </VisualState.StateTriggers>

        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

Cela ajoute de nouveaux éléments **VisualState** et **StateTrigger** qui se déclenchent lorsque l’application détecte qu’elle s’exécute sur un appareil mobile (vous trouverez la logique de cette opération dans MobileScreenTrigger.cs, fourni dans le répertoire PhotoLab). Lorsque le **StateTrigger** démarre, l’application utilise les attributs de disposition qui sont affectés à ce **VisualState**.

### <a name="add-visualstate-setters"></a>Ajouter des méthodes setter VisualState
Ensuite, nous allons utiliser des méthodes setter **VisualState** pour indiquer au **VisualStateManager** les attributs à appliquer lorsque l’état est déclenché. Chaque méthode setter cible une propriété d’un élément XAML particulier et lui affecte la valeur donnée. Ajoutez ce code au **VisualState** mobile que vous venez de créer, en dessous de l'élément **VisualState.StateTriggers**. 

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>

        <VisualState x:Key="Mobile">
            ...

            <!-- Add setters for mobile visualstate -->
            <VisualState.Setters>

                <!-- Move GridView about the command bar -->
                <Setter Target="ImageGridView.(RelativePanel.Above)"
                        Value="MainCommandBar" />

                <!-- Switch to mobile layout -->
                <Setter Target="ImageGridView.ItemTemplate"
                        Value="{StaticResource ImageGridView_MobileItemTemplate}" />

                <!-- Switch to mobile container styles -->
                <Setter Target="ImageGridView.ItemContainerStyle"
                        Value="{StaticResource ImageGridView_MobileItemContainerStyle}" />

                <!-- Move command bar to bottom of the screen -->
                <Setter Target="MainCommandBar.(RelativePanel.AlignBottomWithPanel)"
                        Value="True" />
                <Setter Target="MainCommandBar.(RelativePanel.AlignLeftWithPanel)"
                        Value="True" />
                <Setter Target="MainCommandBar.(RelativePanel.AlignRightWithPanel)"
                        Value="True" />

                <!-- Adjust the zoom slider to fit mobile screens -->
                <Setter Target="ZoomSlider.Minimum"
                        Value="80" />
                <Setter Target="ZoomSlider.Maximum"
                        Value="180" />
                <Setter Target="ZoomSlider.TickFrequency"
                        Value="20" />
                <Setter Target="ZoomSlider.Value"
                        Value="100" />
            </VisualState.Setters>

        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>

```

Ces méthodes setter affectent à la propriété **ItemTemplate** de la galerie d’images le nouvel objet **DataTemplate** que nous avons créé dans la première partie et aligne la barre de commandes et le curseur de zoom sur le bas de l’écran, afin qu'ils soient plus faciles à utiliser avec le pouce sur un écran de téléphone mobile.

### <a name="run-the-app"></a>Exécuter l’application
Essayez maintenant d’exécuter l’application à l’aide d’un émulateur mobile. La nouvelle disposition s’affiche-t-elle correctement? Vous devez voir une grille de petites vignettes comme illustré ci-dessous. Si vous voyez toujours l'ancienne disposition, il peut y avoir une faute de frappe dans votre code **VisualStateManager**.

![Disposition Mobile: après](../basics/images/xaml-basics/adaptive-layout-mobile-after.png)

## <a name="part-3-adapt-to-multiple-window-sizes-on-a-single-device"></a>Partie 3: s’adapter à différentes tailles de fenêtre sur un seul appareil
La création d’une disposition personnalisée résout le problème de conception réactive pour les appareils mobiles, mais qu'en est-il des ordinateurs de bureau et des tablettes? L’application peut présenter un aspect correct en plein écran, mais si l’utilisateur réduit la fenêtre, l'interface peut paraître inadaptée. Nous pouvons garantir que l'interface utilisateur présente toujours une apparence correcte en utilisant le **VisualStateManager** pour s’adapter à différentes tailles de fenêtre sur un même appareil.

![Petite fenêtre: avant](../basics/images/xaml-basics/adaptive-layout-small-before.png)

### <a name="add-window-snap-points"></a>Ajouter des points d’ancrage de fenêtre
La première étape consiste à définir les «points d’ancrage» auxquels différents **VisualStates** seront déclenchés. Ouvrez App.xaml à partir de l’Explorateur de solutions, puis ajoutez le code suivant entre les balises **Application**.

```XAML
<Application.Resources>
    <!--  window width adaptive snap points  -->
    <x:Double x:Key="MinWindowSnapPoint">0</x:Double>
    <x:Double x:Key="MediumWindowSnapPoint">641</x:Double>
    <x:Double x:Key="LargeWindowSnapPoint">1008</x:Double>
</Application.Resources>
```

Nous obtenons trois points d’ancrage, qui nous permettent de créer des **VisualStates** pour trois plages de tailles de fenêtre:
+ Petite (0 à 640pixels de large)
+ Moyenne (641 à 1007pixels de large)
+ Grande (>1007pixels de large)

### <a name="create-new-visualstates-and-statetriggers"></a>Créer des VisualStates et des StateTriggers
Ensuite, nous créons les **VisualStates** et **StateTriggers** qui correspondent à chaque point d’ancrage. Dans MainPage.xaml, ajoutez le code suivant au **VisualStateManager** que vous venez de créer dans la Partie2.

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState x:Key="LargeWindow">

            <!-- Large window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource LargeWindowSnapPoint}"/>
            </VisualState.StateTriggers>
     
        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState x:Key="MediumWindow">

            <!-- Medium window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource MediumWindowSnapPoint}"/>
            </VisualState.StateTriggers>
        
        </VisualState>

        <!-- Small window VisualState -->
        <VisualState x:Key="SmallWindow">

            <!-- Small window trigger -->
            <VisualState.StateTriggers >
                <AdaptiveTrigger MinWindowWidth="{StaticResource MinWindowSnapPoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

### <a name="add-setters"></a>Ajouter des méthodes setter
Enfin, ajoutez ces méthodes setter à l'état **SmallWindow**.

```XAML

<VisualState x:Key="SmallWindow">
    ...

    <!-- Small window setters -->
    <VisualState.Setters>

        <!-- Apply mobile itemtemplate and styles -->
        <Setter Target="ImageGridView.ItemTemplate"
                Value="{StaticResource ImageGridView_MobileItemTemplate}" />
        <Setter Target="ImageGridView.ItemContainerStyle"
                Value="{StaticResource ImageGridView_MobileItemContainerStyle}" />

        <!-- Adjust the zoom slider to fit small windows-->
        <Setter Target="ZoomSlider.Minimum"
                Value="80" />
        <Setter Target="ZoomSlider.Maximum"
                Value="180" />
        <Setter Target="ZoomSlider.TickFrequency"
                Value="20" />
        <Setter Target="ZoomSlider.Value"
                Value="100" />
    </VisualState.Setters>

</VisualState>

```

Ces méthodes setter appliquent le **DataTemplate** et les styles mobiles à l’application de bureau, chaque fois que la largeur de la fenêtre d’affichage est inférieure à 641pixels. Elles optimisent également le curseur de zoom pour l'adapter au petit écran.

### <a name="run-the-app"></a>Exécuter l’application

Dans la barre d’outils Visual Studio définissez l’appareil cible sur **Ordinateur local** et exécuter l’application. Lors du chargement de l’application, essayez de modifier la taille de la fenêtre. Lorsque vous réduisez la fenêtre à une petite taille, vous devez voir l’application passer à la disposition mobile que vous avez créée dans la partie2.

![Petite fenêtre: après](../basics/images/xaml-basics/adaptive-layout-small-after.png)

## <a name="going-further"></a>Aller plus loin

Maintenant que vous avez terminé ce laboratoire, vous avez suffisamment de connaissances en disposition adaptative pour expérimenter par vous-même. Essayez d’ajouter un contrôle des évaluations à l’info-bulle pour mobile uniquement que vous avez ajouté précédemment. Pour aller plus loin, vous pouvez également essayer d’optimiser la disposition pour des tailles d’écran supérieures (comme un écran de TV ou de Surface Studio)

Si vous êtes bloqué, vous trouverez des instructions supplémentaires dans les sections suivantes de [Définir des dispositions de pages avec XAML](../layout/layouts-with-xaml.md).

+ [États visuels et déclencheurs d’état](https://docs.microsoft.com/en-us/windows/uwp/layout/layouts-with-xaml#visual-states-and-state-triggers)
+ [Dispositions personnalisées](https://docs.microsoft.com/en-us/windows/uwp/layout/layouts-with-xaml#tailored-layouts)

Si vous souhaitez en savoir plus sur la manière dont l’application d’édition de photos initiale a été générée, vous pouvez également consulter les didacticiels suivants sur XAML [Interfaces utilisateur](../basics/xaml-basics-ui.md) et [Liaison de données](../../data-binding/xaml-basics-data-binding.md).

## <a name="get-the-final-version-of-the-photolab-sample"></a>Obtenir la version définitive de l’exemple PhotoLab

Ce didacticiel ne génère pas l’application d’édition de photos complète, donc veillez à examiner la [version définitive](https://github.com/Microsoft/Windows-appsample-photo-lab) pour voir d'autres fonctionnalités, telles que les animations personnalisées et le support téléphonique.