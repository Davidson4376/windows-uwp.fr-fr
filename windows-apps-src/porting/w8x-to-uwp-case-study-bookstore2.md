---
author: mcleblanc
ms.assetid: 333f67f5-f012-4981-917f-c6fd271267c6
description: "Cette étude de cas, qui repose sur les informations fournies dans Bookstore1, commence par une application 8.1 universelle qui affiche des données groupées dans un contrôle SemanticZoom."
title: "Étude de cas de portage d’application Windows Runtime 8.x vers UWP : Bookstore2"
translationtype: Human Translation
ms.sourcegitcommit: 9dc441422637fe6984f0ab0f036b2dfba7d61ec7
ms.openlocfilehash: 34762d74ba34ed3c5cee4da4809c2c509f3932e9

---

# <a name="windows-runtime-8x-to-uwp-case-study-bookstore2"></a>Étude de cas de portage d’application Windows Runtime 8.x vers UWP : Bookstore2

\[ Mise à jour pour les applications UWP sur Windows&nbsp;10. Pour les articles sur Windows 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Cette étude de cas, qui repose sur les informations fournies dans [Bookstore1](w8x-to-uwp-case-study-bookstore1.md), commence par une application 8.1 universelle qui affiche des données groupées dans un contrôle [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601). Dans le modèle d’affichage, chaque instance de la classe **Author** représente l’ensemble des livres écrits par l’auteur en question ; dans l’élément **SemanticZoom**, nous pouvons afficher la liste des livres regroupés par auteur ou nous pouvons effectuer un zoom arrière pour afficher une liste de raccourcis relatifs aux auteurs. Grâce à cette liste, vous pouvez vous déplacer beaucoup plus rapidement que si vous faisiez défiler la liste des ouvrages. Nous suivons la procédure de portage de l’application vers une application de plateforme Windows universelle (UWP) Windows&nbsp;10.

**Remarque** Lorsque vous ouvrez Bookstore2Universal\_10 dans Visual Studio, si vous voyez apparaître le message suivant : « Mise à jour de Visual Studio requise », suivez les étapes de la section [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md).

## <a name="downloads"></a>Téléchargements

[Téléchargez l’application 8.1 universelle Bookstore2\_81](http://go.microsoft.com/fwlink/?linkid=532951).

[Téléchargez l’application Windows 10 Bookstore2Universal\_10](http://go.microsoft.com/fwlink/?linkid=532952).

## <a name="the-universal-81-app"></a>Application&nbsp;8.1 universelle

Voici à quoi ressemble Bookstore2\_81, l’application que nous allons porter. Il s’agit d’un contrôle [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) à défilement horizontal (ou vertical sur Windows Phone), qui affiche des livres regroupés par auteur. Vous pouvez effectuer un zoom arrière vers la liste de raccourcis et, à partir de cette dernière, revenir à n’importe quel groupe. Cette application comporte deux parties principales : le modèle d’affichage, qui fournit la source de données groupées, et l’interface utilisateur, qui est liée à ce modèle d’affichage. Comme nous allons le voir, ces deux parties sont facilement portées depuis la technologie WinRT&nbsp;8.1 vers Windows&nbsp;10.

![Application bookstore2\-81 sur Windows (vue avec zoom avant)](images/w8x-to-uwp-case-studies/c02-01-win81-zi-how-the-app-looks.png)

Application Bookstore2\_81 sur Windows (vue avec zoom avant)
 

![Application bookstore2\-81 sur Windows (vue avec zoom arrière)](images/w8x-to-uwp-case-studies/c02-02-win81-zo-how-the-app-looks.png)

Application Bookstore2\_81 sur Windows (vue avec zoom arrière)

![Application bookstore2\-81 sur Windows&nbsp;Phone (vue avec zoom avant)](images/w8x-to-uwp-case-studies/c02-03-wp81-zi-how-the-app-looks.png)

Application Bookstore2\_81 sur Windows&nbsp;Phone (vue avec zoom avant)

![Application bookstore2\-81 sur Windows&nbsp;Phone (vue avec zoom arrière)](images/w8x-to-uwp-case-studies/c02-04-wp81-zo-how-the-app-looks.png)

Application Bookstore2\_81 sur Windows&nbsp;Phone (vue avec zoom arrière)

##  <a name="porting-to-a-windows-10-project"></a>Portage d’une application vers un projet Windows&nbsp;10

La solution Bookstore2\_81 est un projet d’application universelle pour la version&nbsp;8.1. Le projet Bookstore2\_81.Windows crée le package d’application pour Windows&nbsp;8.1 ; le projet Bookstore2\_81.WindowsPhone génère quant à lui le package d’application pour Windows&nbsp;Phone&nbsp;8.1. Enfin, le projet Bookstore2\_81.Shared contient le code source, les fichiers de balisage et d’autres composants et ressources qui sont utilisés par les deux autres projets.

Comme dans l’étude de cas précédente, l’option que nous allons sélectionner (parmi celles qui sont décrites à la section [Si vous disposez d’une application 8.1 universelle](w8x-to-uwp-root.md)) consiste à porter le contenu du projet partagé vers une application Windows 10 qui cible la famille d’appareils universelle.

Commencez par créer un projet Application vide (universelle Windows). Appelez-le «&nbsp;Bookstore2Universal\_10&nbsp;». Il s’agit des fichiers à copier de Bookstore2\_81 dans Bookstore2Universal\_10.

**Dans le projet partagé**

-   Copiez le dossier contenant les fichiers PNG d’image de couverture de livre (dossier \\Assets\\CoverImages). Une fois le dossier copié, dans l’**Explorateur de solutions**, assurez-vous que l’option **Afficher tous les fichiers** est activée. Cliquez avec le bouton droit de la souris sur le dossier que vous avez copié et sélectionnez **Inclure dans le projet**. Cette commande correspond à ce que nous avons voulu dire par «&nbsp;insertion&nbsp;» de fichiers ou de dossiers dans un projet. Chaque fois que vous copiez un fichier ou un dossier, pour chaque copie, sélectionnez **Actualiser** dans **Explorateur de solutions**, puis incluez le fichier ou dossier dans le projet. Cette opération n’est pas nécessaire pour les fichiers dont vous modifiez la destination.
-   Copiez le dossier contenant le fichier source de modèle d’affichage (dossier \\ViewModel).
-   Copiez le fichier MainPage.xaml et remplacez le fichier dans la destination.

**Dans le projet Windows**

-   Copiez le fichier BookstoreStyles.xaml. Nous l’utiliserons comme point de départ, car l’ensemble des clés de ressources figurant dans ce fichier sera résolu dans une application Windows 10, alors que certaines clés de ressources du fichier WindowsPhone correspondant ne le seront pas.
-   Copiez les fichiers SeZoUC.xaml et SeZoUC.xaml.cs. Nous allons commencer par la version de cet affichage pour Windows, qui est appropriée pour les fenêtres étendues. Ensuite, nous adapterons ce contenu à des fenêtres plus petites et, de ce fait, à des appareils plus petits.

Modifiez les fichiers de code source et de balisage que vous venez de copier et remplacez toutes les références à l’espace de noms Bookstore2\_81 par «&nbsp;Bookstore2Universal\_10&nbsp;». Une méthode rapide consiste à utiliser la fonctionnalité **Remplacer dans les fichiers**. Aucune modification du code n’est nécessaire dans le modèle d’affichage, ni dans tout autre code impératif. Toutefois, pour simplifier l’identification de la version de l’application en cours d’exécution, remplacez la valeur renvoyée par la propriété **Bookstore2Universal\_10.BookstoreViewModel.AppName**, à savoir « Bookstore2\_81 », par « BOOKSTORE2UNIVERSAL\_10 ».

Vous pouvez maintenant générer l’application et l’exécuter. Voici à quoi ressemble notre nouvelle application UWP, sans aucun effort de portage vers Windows&nbsp;10 pour l’instant.

![Application Windows&nbsp;10 avec le code source initial modifié, exécutée sur un appareil de bureau (vue avec zoom avant)](images/w8x-to-uwp-case-studies/c02-05-desk10-zi-initial-source-code-changes.png)

Application Windows&nbsp;10 avec le code source initial modifié, exécutée sur un appareil de bureau (vue avec zoom avant)

![Application Windows&nbsp;10 avec le code source initial modifié, exécutée sur un appareil de bureau (vue avec zoom arrière)](images/w8x-to-uwp-case-studies/c02-06-desk10-zo-initial-source-code-changes.png)

Application Windows&nbsp;10 avec le code source initial modifié, exécutée sur un appareil de bureau (vue avec zoom arrière)

Le modèle d’affichage et les vues avec zoom avant et arrière fonctionnent ensemble correctement. Cependant, il existe certains problèmes, qui les rendent difficiles à voir. Exemple de problème : l’élément [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) ne défile pas. En effet, dans Windows 10, le style par défaut d’un élément [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) entraîne son affichage dans une disposition verticale ; or, les instructions de conception de Windows 10 nous invitent à utiliser cet élément dans cette disposition dans les applications, tant nouvelles que portées. Toutefois, les paramètres de défilement horizontal du modèle de panneau d’éléments personnalisé que nous avons copié du projet Bookstore2\_81 (conçu pour l’application&nbsp;8.1) sont en conflit avec les paramètres de défilement vertical du style par défaut de Windows&nbsp;10, appliqués suite au portage de notre application vers Windows&nbsp;10. Deuxième problème&nbsp;: l’application ne peut pas encore adapter son interface utilisateur à l’appareil de l’utilisateur, afin de lui offrir la meilleure expérience possible, quelle que soit la taille des fenêtres et de l’appareil. Enfin, les styles et pinceaux corrects ne sont pas encore utilisés, ce qui entraîne l’invisibilité d’une partie du texte (cela inclut les en-têtes de groupe sur lesquels vous pouvez cliquer pour effectuer un zoom arrière). Ainsi, dans les trois sections suivantes ([Modifications de conception des éléments SemanticZoom et GridView](#semanticzoom-and-gridview-design-changes), [L’interface utilisateur adaptative](#adaptive-ui), et [Stylisation universelle](#universal-styling)) nous allons remédier à ces problèmes.

## <a name="semanticzoom-and-gridview-design-changes"></a>Modifications de conception des éléments SemanticZoom et GridView

Les modifications apportées à la conception du contrôle [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) dans Windows 10 sont décrites à la section [Modifications SemanticZoom](w8x-to-uwp-porting-xaml-and-ui.md). Nous n’avons aucune tâche à effectuer dans cette section suite à ces modifications.

Les modifications apportées à l’élément [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) sont décrites dans la section [Modifications GridView/ListView](w8x-to-uwp-porting-xaml-and-ui.md). Nous devons effectuer quelques réglages mineurs pour nous adapter à ces modifications, comme décrit ci-dessous.

-   Dans l’élément `ZoomedInItemsPanelTemplate` du fichier SeZoUC.xaml, définissez les paramètres `Orientation="Horizontal"` et `GroupPadding="0,0,0,20"`.
-   Toujours dans ce fichier, supprimez l’élément `ZoomedOutItemsPanelTemplate`, puis supprimez l’attribut `ItemsPanel` de la vue avec zoom arrière.

C’est tout&nbsp;!

## <a name="adaptive-ui"></a>L’interface utilisateur adaptative

Une fois cette modification effectuée, la disposition de l’interface utilisateur proposée par le fichier SeZoUC.xaml est très utile lorsque l’application s’exécute dans une fenêtre large (opération uniquement possible sur un appareil doté d’un grand écran). Par contre, lorsque la fenêtre d’application est étroite (comme sur un appareil de petite taille, voire sur certains appareils plus grands), l’interface utilisateur proposée par l’application du Windows Phone Store est sans aucun doute la plus appropriée.

Nous pouvons utiliser la fonction Gestionnaire d’état visuel adaptative pour y parvenir. Nous allons définir des propriétés sur les éléments visuels afin que, par défaut, l’interface utilisateur présente une disposition étroite à l’aide des plus petits modèles que nous utilisions dans l’application du Windows Phone Store. Ensuite, nous détecterons les cas où la fenêtre de l’application est d’une largeur égale ou supérieure à une taille spécifique (mesurée en [pixels effectifs](w8x-to-uwp-porting-xaml-and-ui.md)), et nous modifierons alors les propriétés des éléments visuels comme il convient afin d’obtenir une disposition plus grande et plus large. Nous affecterons à ces changements de propriété un état visuel ; nous utiliserons un déclencheur adaptatif pour effectuer un suivi en continu et déterminer si l’état visuel doit ou non être appliqué, selon la largeur de la fenêtre en pixels effectifs. Dans ce cas précis, nous baserons le déclenchement sur la largeur de la fenêtre, mais il est également possible de choisir la hauteur de la fenêtre comme déclencheur.

Une largeur minimale de 548 epx est appropriée pour cet exemple d’utilisation, car cette valeur correspond à la taille de l’appareil le plus petit auquel nous voulons appliquer la disposition d’écran large. En général, les téléphones présentent une largeur inférieure à 548 epx. Ainsi, sur ce type d’appareil de petite taille, nous conserverons la disposition étroite par défaut. Sur un PC, par défaut, la fenêtre s’ouvre sur une largeur suffisante pour déclencher le passage à l’état large. À partir de là, vous serez en mesure de faire glisser la fenêtre afin qu’elle soit suffisamment étroite pour afficher deux colonnes pour les éléments d’une taille de 250 x 250. Si vous utilisez une largeur inférieure, le déclencheur sera désactivé, l’état visuel large sera supprimé, et la disposition étroite par défaut continuera d’être appliquée.

Ainsi, quelles sont les propriétés que nous devons définir—et modifier—pour bénéficier de ces deux dispositions&nbsp;? Il existe deux solutions, qui font chacune appel à une méthode différente.

1.  Nous pouvons placer deux contrôles [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) dans notre balisage. L’un de ces contrôles correspond à une copie du balisage que nous avons utilisé dans l’application du Windows Store (en utilisant des contrôles [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) à l’intérieur de cette copie), réduite par défaut. Quant à l’autre, il correspond à une copie du balisage que nous avons utilisé dans l’application du Windows Phone Store (en utilisant des contrôles [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) à l’intérieur de cette copie), visible par défaut. L’état visuel fait alors basculer les propriétés de visibilité des deux contrôles **SemanticZoom**. L’exécution de cette technique nécessite peu d’efforts, mais ses performances sont généralement moyennes. Ainsi, si vous l’utilisez, vous devez profiler votre application et vous assurer qu’elle répond toujours à vos objectifs en termes de performances.
2.  Nous pouvons utiliser un seul élément [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) contenant des contrôles [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878). Pour obtenir nos deux dispositions, dans l’état visuel large, nous modifions les propriétés des contrôles **ListView**, y compris des modèles qui leur sont appliqués, pour entraîner leur disposition de la même manière que celle d’un élément [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705). Cette solution offre des performances sans doute meilleures, mais se révèle également la plus difficile à appliquer, à cause du nombre de petites différences entre les styles et modèles des éléments **GridView** et **ListView**, et entre leurs divers types d’éléments. Cette solution est étroitement associée au mode de conception des styles et modèles par défaut à ce moment précis&nbsp;; nous obtenons une solution fragile et sensible aux éventuelles modifications futures des valeurs par défaut.

Dans cette étude de cas, nous allons choisir la première solution. Cependant, vous pouvez choisir la deuxième méthode, si elle vous convient mieux. Voici les étapes à suivre pour implémenter cette première solution.

-   Dans l’élément [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) du balisage du nouveau projet, définissez `x:Name="wideSeZo"` et `Visibility="Collapsed"`.
-   Revenez au projet Bookstore2\_81.WindowsPhone et ouvrez le fichier SeZoUC.xaml. Copiez le balisage de l’élément [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) de ce fichier et collez-le juste après l’élément `wideSeZo` dans votre nouveau projet. Définissez `x:Name="narrowSeZo"` sur l’élément que vous venez de coller.
-   Toutefois, l’élément `narrowSeZo` nécessite quelques styles que nous n’avons pas encore copiés. Dans Bookstore2\_81.WindowsPhone, copiez les deux styles (`AuthorGroupHeaderContainerStyle` et `ZoomedOutAuthorItemContainerStyle`) du fichier SeZoUC.xaml et collez-les dans le fichier BookstoreStyles.xaml de votre nouveau projet.
-   Le nouveau fichier SeZoUC.xaml inclut désormais deux nouveaux éléments [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601). Incluez ces deux éléments dans un élément **Grid**.
-   Dans le fichier BookstoreStyles.xaml de votre nouveau projet, ajoutez le mot `Wide` à ces trois clés de ressources (et à leurs références dans le fichier SeZoUC.xaml, mais uniquement aux références à l’intérieur de l’élément `wideSeZo`) : `AuthorGroupHeaderTemplate`, `ZoomedOutAuthorTemplate`, et `BookTemplate`.
-   Dans le projet Bookstore2\_81.WindowsPhone, ouvrez le fichier BookstoreStyles.xaml. À partir de ce fichier, copiez ces trois ressources (mentionnées ci-dessus), ainsi que les deux convertisseurs d’élément de liste de raccourcis et la déclaration de préfixe d’espace de noms Windows\_UI\_Xaml\_Controls\_Primitives, puis collez-les dans le fichier BookstoreStyles.xaml de votre nouveau projet.
-   Enfin, dans le fichier SeZoUC.xaml de votre nouveau projet, ajoutez le balisage de gestionnaire d’état visuel approprié pour l’élément **Grid** que vous avez ajouté ci-dessus.

```xml
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="WideState">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="548"/>
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Target="wideSeZo.Visibility" Value="Visible"/>
                        <Setter Target="narrowSeZo.Visibility" Value="Collapsed"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

    ...

    </Grid>
```

## <a name="universal-styling"></a>Stylisation universelle

Nous allons désormais résoudre certains problèmes de style, y compris celui que nous avons introduit ci-dessus lors de l’opération de copie à partir de l’ancien projet.

-   Dans le fichier MainPage.xaml, remplacez l’arrière-plan de l’élément `LayoutRoot` par `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`.
-   Dans le fichier BookstoreStyles.xaml, définissez la valeur de la ressource `TitlePanelMargin` sur `0` (ou sur toute valeur qui vous convienne).
-   Dans le fichier SeZoUC.xaml, définissez la marge de l’élément `wideSeZo` sur `0` (ou sur toute valeur qui vous convienne).
-   Dans le fichier BookstoreStyles.xaml, supprimez l’attribut Margin de l’élément `AuthorGroupHeaderTemplateWide`.
-   Supprimez l’attribut FontFamily des éléments `AuthorGroupHeaderTemplate` et `ZoomedOutAuthorTemplate`.
-   Le projet Bookstore2\_81 utilisait les clés de ressources `BookTemplateTitleTextBlockStyle`, `BookTemplateAuthorTextBlockStyle` et `PageTitleTextBlockStyle` comme une indirection, afin qu’une seule clé inclue des implémentations différentes dans les deux applications. Nous n’avons plus besoin de cette indirection&nbsp;; nous pouvons référencer directement les styles du système. Par conséquent, dans toute l’application, remplacez ces références par `TitleTextBlockStyle`, `CaptionTextBlockStyle` et `HeaderTextBlockStyle`, respectivement. Vous pouvez également utiliser la fonction **Remplacer dans les fichiers** de Visual Studio pour effectuer cette opération rapidement et de manière précise. Vous pouvez ensuite supprimer ces trois ressources inutilisées.
-   Dans `AuthorGroupHeaderTemplate`, remplacez l’élément `PhoneAccentBrush` par `SystemControlBackgroundAccentBrush` et définissez `Foreground="White"` sur l’élément **TextBlock** afin qu’il apparaisse correctement lors de l’exécution sur la famille d’appareils mobiles.
-   Dans l’élément `BookTemplateWide`, copiez l’attribut Foreground du deuxième élément **TextBlock** vers le premier.
-   Dans l’élément `ZoomedOutAuthorTemplateWide`, remplacez la référence à `SubheaderTextBlockStyle` (un peu trop importante, désormais) par une référence à `SubtitleTextBlockStyle`.
-   La vue avec zoom arrière (liste de raccourcis) ne se superpose plus à la vue avec zoom avant dans la nouvelle plateforme. De ce fait, nous pouvons supprimer l’attribut `Background` de la vue avec zoom arrière de l’élément `narrowSeZo`.
-   Pour que tous les styles et modèles figurent dans un seul fichier, retirez l’élément `ZoomedInItemsPanelTemplate` du fichier SeZoUC.xaml et placez-le dans le fichier BookstoreStyles.xaml.

Une fois cette séquence d’opérations de stylisation effectuée, l’application ressemble à ceci.

![Application Windows&nbsp;10 portée, exécutée sur un appareil de bureau (vue avec zoom avant et deux tailles de fenêtres)](images/w8x-to-uwp-case-studies/c02-07-desk10-zi-ported.png)

Application Windows&nbsp;10 portée, exécutée sur un appareil de bureau (vue avec zoom avant et deux tailles de fenêtres)

![Application Windows&nbsp;10 portée, exécutée sur un appareil de bureau (vue avec zoom arrière et deux tailles de fenêtres)](images/w8x-to-uwp-case-studies/c02-08-desk10-zo-ported.png)

Application Windows&nbsp;10 portée, exécutée sur un appareil de bureau (vue avec zoom arrière et deux tailles de fenêtres)

![Application Windows&nbsp;10 portée, exécutée sur un appareil mobile (vue avec zoom avant)](images/w8x-to-uwp-case-studies/c02-09-mob10-zi-ported.png)

Application Windows&nbsp;10 portée, exécutée sur un appareil mobile (vue avec zoom avant)

![Application Windows&nbsp;10 portée, exécutée sur un appareil mobile (vue avec zoom arrière)](images/w8x-to-uwp-case-studies/c02-10-mob10-zo-ported.png)

Application Windows&nbsp;10 portée, exécutée sur un appareil mobile (vue avec zoom arrière)

## <a name="conclusion"></a>Conclusion

Cette étude de cas reposait sur une interface utilisateur plus ambitieuse que celle de l’étude précédente. Comme pour l’étude de cas précédente, ce modèle d’affichage particulier n’a requis aucun effort. Nous avons surtout dû nous concentrer sur la refactorisation de l’interface utilisateur. Certaines modifications inévitables ont été le fruit de la combinaison de deux projets en un seul, qui devait toujours prendre en charge de nombreux facteurs de forme (en fait, nettement plus qu’auparavant). Certaines de ces modifications étaient liées aux changements apportés à la plateforme.

Dans l’étude de cas suivante, [QuizGame](w8x-to-uwp-case-study-quizgame.md), nous examinons l’accès aux données groupées et leur affichage.



<!--HONumber=Dec16_HO1-->


