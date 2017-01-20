---
author: mukin
Description: "Cet article se présente sous forme de didacticiel conçu pour guider le lecteur tout au long des étapes nécessaires pour créer l’interface utilisateur d’une application simple. Il explique et montre comment utiliser Grid et StackPanel, deux des éléments XAML les plus courants."
title: "Créer une application Météo simple à l’aide de Grid et de StackPanel"
translationtype: Human Translation
ms.sourcegitcommit: c30aed9681d7c091e9831df3f53d8779dfacce38
ms.openlocfilehash: 1990a3ad4e56fa039d024c53a04b021e60bed145

---

# <a name="create-a-simple-weather-app-by-using-grid-and-stackpanel"></a>Créer une application Météo simple à l’aide de Grid et de StackPanel

Utilisez XAML pour créer la disposition d’une application Météo simple à l’aide des éléments **Grid** et **StackPanel**. Ces outils vous permettent de créer des applications esthétiques qui fonctionnent sur tous les appareils exécutant Windows 10. Ce didacticiel dure 10 à 20 minutes.

## <a name="prerequisites"></a>Conditions préalables
- Windows 10 et Microsoft Visual Studio 2015. [Cliquez ici pour savoir comment prendre en main Visual Studio](../get-started/get-set-up.md).
- Savoir créer une application « Hello World » de base à l’aide de XAML et de C#. En cas de besoin, [cliquez ici pour savoir comment créer une application « Hello World »](https://msdn.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).

## <a name="step-1-create-a-blank-app"></a>Étape 1 : Créer une application vide
1. Dans Visual Studio, sélectionnez **Fichier** > **Nouveau projet**.
2. Dans le volet gauche de la boîte de dialogue **Nouveau projet**, choisissez le nœud **Visual C#** > **Windows** > **Universel** ou **Visual C++** > **Windows** > **Universel**.
3. Dans le volet central, sélectionnez **Application vide**.
4. Dans la zone **Nom** entrez **WeatherPanel**, puis sélectionnez **OK**.
5. Pour exécuter le programme, choisissez **Déboguer** > **Démarrer le débogage** dans le menu, ou appuyez sur F5.

## <a name="step-2-define-a-grid"></a>Étape 2 : Définir une grille
Dans le code XAML, une **grille** comprend une série de lignes et de colonnes. En spécifiant la ligne et la colonne d’un élément dans une **grille**, vous pouvez placer et espacer d’autres éléments dans une interface utilisateur. Les lignes et colonnes sont définies avec les éléments **RowDefinition** et **ColumnDefinition**.

Pour commencer à créer une disposition, ouvrez **MainPage.xaml** à l’aide de l’**Explorateur de solutions** et remplacez l’élément **Grid** généré automatiquement par ce code.

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="3*"/>
        <ColumnDefinition Width="5*"/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="2*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
</Grid>
```

Le nouvel élément **Grid** crée un ensemble de deux lignes et colonnes, qui définit la disposition de l’interface de l’application. La première colonne a une **largeur** de « 3\* », tandis que la seconde a une largeur de « 5\* », divisant l’espace horizontal entre les deux colonnes à un rapport de 3:5. De la même manière, les deux lignes ont une **hauteur** de « 3\* » et « \* » respectivement, l’élément **Grid** allouant trois fois plus d’espace à la première ligne qu’à la seconde (« \* » est identique à « 1\* »). Ces rapports sont conservés même si la fenêtre est redimensionnée ou si l’appareil est modifié.

Pour en savoir plus sur les autres méthodes de dimensionnement des lignes et des colonnes, consultez [Définir des dispositions avec XAML](https://msdn.microsoft.com/windows/uwp/layout/layouts-with-xaml#layout-properties).

Si vous exécutez l’application maintenant, vous ne verrez rien d’autre qu’une page vide, car aucune des zones de la **grille** ne contient d’informations. Pour afficher la **grille**, donnons-lui de la couleur.

## <a name="step-3-color-the-grid"></a>Étape 3 : Colorier la grille
Pour colorier la **grille**, nous ajoutons trois éléments **Border**, chacun avec une couleur d’arrière-plan différente. Chacun est également attribué à une ligne et une colonne dans la **grille** parente, à l’aide des attributs **Grid.Row** et **Grid.Column**. Comme ces attributs ont la valeur 0 par défaut, vous n’avez pas besoin de les affecter au premier élément **Border**. Ajoutez le code suivant à l’élément **Grid** après les définitions de lignes et de colonnes.

```xml
<Border Background="#2f5cb6"/>
<Border Grid.Column ="1" Background="#1f3d7a"/>
<Border Grid.Row="1" Grid.ColumnSpan="2" Background="#152951"/>
```

Notez que pour le troisième élément **Border**, nous utilisons un attribut supplémentaire, **Grid.ColumnSpan**, qui étend cet élément **Border** sur deux colonnes sur la ligne inférieure. Vous pouvez utiliser **Grid.RowSpan** de la même façon. Ensemble, ces attributs vous permettent d’étendre un élément sur n’importe quel nombre de lignes et colonnes. L’angle supérieur gauche d’une extension de ce type est égal aux paramètres **Grid.Column** et **Grid.Row** spécifiés dans les attributs de l’élément.

Si vous exécutez l’application, elle se présente comme suit.

![Coloration de la grille](images/grid-weather-1.png)

## <a name="step-4-organize-content-by-using-stackpanel-elements"></a>Étape 4 : Organiser le contenu à l’aide d’éléments StackPanel
**StackPanel** est le deuxième élément d’interface utilisateur que nous allons utiliser pour créer notre application Météo. L’élément **StackPanel** est une partie essentielle de nombreuses dispositions d’application de base, car il vous permet d’empiler des éléments verticalement ou horizontalement.

Dans le code suivant, nous allons créer deux éléments **StackPanel** et les remplir avec trois éléments **TextBlock**. Ajoutez ces éléments **StackPanel** à la **grille** sous les éléments **Border** de l’étape 3. Le rendu des éléments **TextBlock** apparaît au-dessus de la **grille** de couleur que nous avons créée auparavant.

```xml
<StackPanel Grid.Column="1" Margin="40,0,0,0" VerticalAlignment="Center">
    <TextBlock Foreground="White" FontSize="25" Text="Today - 64° F"/>
    <TextBlock Foreground="White" FontSize="25" Text="Partially Cloudy"/>
    <TextBlock Foreground="White" FontSize="25" Text="Precipitation: 25%"/>
</StackPanel>
<StackPanel Grid.Row="1" Grid.ColumnSpan="2" Orientation="Horizontal"
            HorizontalAlignment="Center" VerticalAlignment="Center">
    <TextBlock Foreground="White" FontSize="25" Text="High: 66°" Margin="0,0,20,0"/>
    <TextBlock Foreground="White" FontSize="25" Text="Low: 43°" Margin="0,0,20,0"/>
    <TextBlock Foreground="White" FontSize="25" Text="Feels like: 63°"/>
</StackPanel>
```

Dans le premier élément **Stackpanel**, chaque **TextBlock** s’empile verticalement sous le suivant. Comme il s’agit du comportement par défaut d’un élément StackPanel, il est inutile de définir l’attribut **Orientation**. Dans le second élément StackPanel, nous voulons que les éléments enfants s’empilent horizontalement de gauche à droite. Donc, nous définissons l’attribut **Orientation** sur « Horizontal ». Nous devons également régler l’attribut **Grid.ColumnSpan** sur « 2 », afin de centrer le texte dans l’élément **Border** inférieur.

Si vous exécutez l’application maintenant, elle se présente comme suit.

![Ajout d’éléments StackPanel](images/grid-weather-2.png)

## <a name="step-5-add-an-image-icon"></a>Étape 5 : Ajouter une icône d’image

Enfin, nous allons remplir la section vide notre **grille** avec une image qui représente la météo du jour - une formulation du type « partiellement nuageux ».

Téléchargez l’image ci-dessous et enregistrez-la comme une image PNG nommée « partiellement-nuageux ».

![Partiellement nuageux](images/partially-cloudy.PNG)

Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le dossier **Assets** et sélectionnez **Ajouter** -> **Élément existant...** Recherchez le fichier partiellement-nuageux.png dans le navigateur qui s’affiche, sélectionnez-le, puis cliquez sur **Ajouter**.

Ensuite, dans **MainPage.xaml**, ajoutez l’élément **Image** suivant sous les éléments StackPanel de l’étape 4.

```xml
<Image Margin="20" Source="Assets/partially-cloudy.png"/>
```

Comme nous voulons inclure l’image dans la première ligne et la première colonne, il est inutile de définir ses attributs **Grid.Row** ou **Grid.Column** (ils gardent leur valeur par défaut « 0 »).

C’est tout ! Vous venez de créer la disposition d’une application Météo simple. Si vous exécutez l’application en appuyant sur **F5**, elle doit se présenter comme ceci :

![Exemple de volet météo](images/grid-weather-3.PNG)

Si vous le souhaitez, testez la disposition ci-dessus et explorez les différentes façons de représenter des données météorologiques.

## <a name="related-articles"></a>Articles connexes
Pour une introduction à la conception de dispositions d’application UWP, consultez [Introduction à la conception d’une application UWP](https://msdn.microsoft.com/windows/uwp/layout/design-and-ui-intro).

Pour apprendre à créer des dispositions réactives adaptables à différentes tailles d’écran, consultez [Définir des dispositions de pages avec XAML](https://msdn.microsoft.com/windows/uwp/layout/layouts-with-xaml).



<!--HONumber=Jan17_HO1-->


