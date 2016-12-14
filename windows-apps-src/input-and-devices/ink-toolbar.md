---
author: Karl-Bridge-Microsoft
Description: "Ajoutez un élément InkToolbar par défaut à une application d’entrée manuscrite de plateforme Windows universelle (UWP), ajoutez un bouton de stylet personnalisé à l’élément InkToolbar et liez le bouton de stylet personnalisé à une définition de stylet personnalisé."
title: "Ajouter un élément InkToolbar à une application d’entrée manuscrite de plateforme Windows universelle (UWP)"
label: Add an InkToolbar to a Universal Windows Platform (UWP) inking app
template: detail.hbs
keywords: "Windows Ink, entrée manuscrite Windows Ink, DirectInk, InkPresenter, InkCanvas, InkToolbar, plateforme Windows universelle, UWP"
translationtype: Human Translation
ms.sourcegitcommit: 2b6b1d7b1755aad4d75a29413d989c6e8112128a
ms.openlocfilehash: 1b810a42166c48c1359dcf9adfba84184234b42c

---

# <a name="add-an-inktoolbar-to-a-universal-windows-platform-uwp-inking-app"></a>Ajouter un élément InkToolbar à une application d’entrée manuscrite de plateforme Windows universelle (UWP)

Il existe deux contrôles différents qui facilitent l’entrée manuscrite dans les applications de plateforme Windows universelle (UWP) : [**InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) et [**InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

Le contrôle [**InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) fournit les fonctionnalités Windows Ink de base. Utilisez-le pour restituer une entrée de stylet sous la forme d’un trait d’encre (via les paramètres par défaut de couleur et d’épaisseur) ou d’un trait d’effacement.

> Pour plus d’informations sur l’implémentation du contrôle InkCanvas, consultez [Interactions avec le stylo et le stylet dans les applications UWP](pen-and-stylus-interactions.md).

En tant que superposition totalement transparente, le contrôle InkCanvas ne fournit pas d’interface utilisateur intégrée pour configurer les propriétés relatives aux traits d’encre. Si vous souhaitez modifier l’expérience d’entrée manuscrite par défaut, permettre aux utilisateurs de configurer les propriétés relatives aux traits d’encre et prendre en charge d’autres fonctionnalités d’entrée manuscrite personnalisées, deux options s’offrent à vous :

- Dans le code-behind, utilisez l’objet [**InkPresenter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx) sous-jacent lié au contrôle InkCanvas.

  Les API InkPresenter prennent en charge une personnalisation complète de l’expérience d’entrée manuscrite. Pour plus d’informations, consultez [Interactions avec le stylo et le stylet dans les applications UWP](pen-and-stylus-interactions.md).

- Liez un contrôle [**InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) au contrôle InkCanvas. Par défaut, le contrôle InkToolbar fournit une interface utilisateur de base pour l’activation des fonctionnalités d’entrée manuscrite et la configuration des propriétés relatives aux traits d’encre, telles que la taille du trait, la couleur de l’encre et la forme de la pointe du stylet.

  Cette rubrique est dédiée au contrôle InkToolbar.

## <a name="important-apis"></a>API importantes

  -   [**Classe InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx)
  -   [**Classe InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)
  -   [**Classe InkPresenter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx)
  -   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)


## <a name="default-inktoolbar"></a>Contrôle InkToolbar par défaut

Par défaut, le contrôle InkToolbar comprend des boutons pour dessiner, effacer, surligner et afficher une règle. Selon la fonctionnalité, d’autres paramètres et commandes tels que la couleur de l’encre, l’épaisseur du trait, la suppression totale, sont fournis dans un menu volant.

![InkToolbar](.\images\ink\ink-tools-invoked-toolbar-small.png)  
*Barre d’outils Windows Ink par défaut*

Pour ajouter un contrôle InkToolbar de base par défaut :
1. Dans MainPage.xaml, déclarez un objet de conteneur (ici, nous utilisons un contrôle Grid) pour la surface d’entrée manuscrite.
2. Déclarez un objet InkCanvas en tant qu’enfant du conteneur. (La taille du contrôle InkCanvas est héritée du conteneur).
3. Déclarer un contrôle InkToolbar et utilisez l’attribut TargetInkCanvas pour le lier au contrôle InkCanvas.
  Vérifiez que le contrôle InkToolbar est déclaré après le contrôle InkCanvas. Si ce n’est pas le cas, la superposition du contrôle InkCanvas rend inaccessible le contrôle InkToolbar.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
    </Grid>
</Grid>
```

## <a name="basic-customization"></a>Personnalisation de base

Dans cette section, nous allons aborder quelques scénarios de personnalisation de barre d’outils Windows Ink de base.

### <a name="specify-the-selected-button"></a>Spécifier le bouton sélectionné  
![Bouton de crayon sélectionné lors de l’initialisation](.\images\ink\ink-tools-default-toolbar.png)  
*Barre d’outils Windows Ink avec le bouton de crayon sélectionné lors de l’initialisation*

Par défaut, le premier bouton (ou celui le plus à gauche) est sélectionné lorsque votre application est lancée et que la barre d’outils est initialisée. Dans la barre d’outils Windows Ink par défaut, il s’agit du bouton de stylo à bille.

Étant donné que l’infrastructure définit l’ordre des boutons intégrés, le premier bouton peut ne pas être pas le stylet ou l’outil que vous souhaitez activer par défaut.

Vous pouvez remplacer ce comportement par défaut et spécifier le bouton sélectionné dans la barre d’outils.

Ici, nous initialisons la barre d’outils par défaut avec le bouton de crayon sélectionné et le crayon activé (au lieu du stylo à bille).

1. Utilisez la déclaration XAML pour les contrôles InkCanvas et InkToolbar de l’exemple précédent.
2. Dans le code-behind, configurez un gestionnaire pour l’événement [Loaded](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx) de l’objet [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loaded += inkToolbar_Loaded;
  }
  ```

3. Dans le gestionnaire pour l’événement [Loaded](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx) :
  1. Obtenez une référence pour l’élément [InkToolbarPencilButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) intégré.

    Transmettre un objet [InkToolbarTool.Pencil](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbartool.aspx) dans une méthode [GetToolButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.gettoolbutton.aspx) renvoie un objet [InkToolbarToolButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbartoolbutton.aspx) pour l’élément [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx).

  2. Définissez [ActiveTool](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.activetool.aspx) sur l’objet renvoyé à l’étape précédente.

```CSharp
/// <summary>
/// Handle the Loaded event of the InkToolbar.
/// By default, the active tool is set to the first tool on the toolbar.
/// Here, we set the active tool to the pencil button.
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void inkToolbar_Loaded(object sender, RoutedEventArgs e)
{
    InkToolbarToolButton pencilButton = inkToolbar.GetToolButton(InkToolbarTool.Pencil);
    inkToolbar.ActiveTool = pencilButton;
}
```

### <a name="specify-the-built-in-buttons"></a>Spécifier les boutons intégrés

![Boutons spécifiques inclus lors de l’initialisation](.\images\ink\ink-tools-specific.png)  
*Boutons spécifiques inclus lors de l’initialisation*

Comme mentionné précédemment, la barre d’outils Windows Ink comprend une collection de boutons intégrés par défaut. Ces boutons sont affichés dans l’ordre suivant (de gauche à droite) :

- [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx)
- [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx)
- [InkToolbarHighlighterButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarhighlighterbutton.aspx)
- [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx)
- [InkToolbarRulerButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarrulerbutton.aspx)

Ici, nous initialisons la barre d’outils avec les boutons intégrés de stylo à bille, crayon et gomme uniquement.

Vous pouvez effectuer cette opération à l’aide de la déclaration XAML ou du code-behind.

**XAML**

Modifiez la déclaration XAML pour les contrôles InkCanvas et InkToolbar du premier exemple.
- Ajouter un attribut [InitialControls](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx)et définissez sa valeur sur « [None](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx) ». Cela permet d’effacer la collection de boutons intégrés par défaut.
- Ajoutez les boutons InkToolbar spécifiques requis par votre application. Ici, nous ajoutons uniquement les boutons [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx), [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) et [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx).
> [!NOTE]
> Les boutons sont ajoutés à la barre d’outils dans l’ordre défini par votre infrastructure et non dans l’ordre indiqué ici.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <!-- Clear the default InkToolbar buttons by setting InitialControls to None. -->
        <!-- Set the active tool to the pencil button. -->
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
                    VerticalAlignment="Top"
                    TargetInkCanvas="{x:Bind inkCanvas}"
                    InitialControls="None">
            <!--
             Add only the ballpoint pen, pencil, and eraser.
             Note that the buttons are added to the toolbar in the order
             defined by the framework, not the order we specify here.
            -->
            <InkToolbarEraserButton />
            <InkToolbarBallpointPenButton />
            <InkToolbarPencilButton/>
        </InkToolbar>
    </Grid>
</Grid>
```

**Code-behind**
1. Utilisez la déclaration XAML pour les contrôles InkCanvas et InkToolbar du premier exemple.

  ```xaml
  <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <Grid.RowDefinitions>
          <RowDefinition Height="Auto"/>
          <RowDefinition Height="*"/>
      </Grid.RowDefinitions>
      <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
          <TextBlock x:Name="Header"
                     Text="Basic ink sample"
                     Style="{ThemeResource HeaderTextBlockStyle}"
                     Margin="10,0,0,0" />
      </StackPanel>
      <Grid Grid.Row="1">
          <Image Source="Assets\StoreLogo.png" />
          <InkCanvas x:Name="inkCanvas" />
          <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
      </Grid>
  </Grid>
  ```

2. Dans le code-behind, configurez un gestionnaire pour l’événement [Loading](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loading.aspx) de l’objet [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loading += inkToolbar_Loading;
  }
  ```

3. Définissez [InitialControls](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx) sur « [None](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx) ».
4. Créez des références d’objet pour les boutons requis par votre application. Ici, nous ajoutons uniquement les boutons [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx), [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) et [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx).
  > [!NOTE]
  > Les boutons sont ajoutés à la barre d’outils dans l’ordre défini par votre infrastructure et non dans l’ordre indiqué ici.

5. [Ajoutez](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.dependencyobjectcollection.add.aspx) les boutons au contrôle InkToolbar.

  ```csharp
  /// <summary>
  /// Handles the Loading event of the InkToolbar.
  /// Here, we identify the buttons to include on the InkToolbar.
  /// </summary>
  /// <param name="sender">The InkToolbar</param>
  /// <param name="args">The InkToolbar event data.
  /// If there is no event data, this parameter is null</param>
  private void inkToolbar_Loading(FrameworkElement sender, object args)
  {
      // Clear all built-in buttons from the InkToolbar.
      inkToolbar.InitialControls = InkToolbarInitialControls.None;

      // Add only the ballpoint pen, pencil, and eraser.
      // Note that the buttons are added to the toolbar in the order
      // defined by the framework, not the order we specify here.
      InkToolbarBallpointPenButton ballpoint = new InkToolbarBallpointPenButton();
      InkToolbarPencilButton pencil = new InkToolbarPencilButton();
      InkToolbarEraserButton eraser = new InkToolbarEraserButton();
      inkToolbar.Children.Add(eraser);
      inkToolbar.Children.Add(ballpoint);
      inkToolbar.Children.Add(pencil);
  }
  ```

<!--
### Support touch input
By default, the InkToolbar supports both pen and mouse input, you have to enable support for touch input.
-->

## <a name="custom-buttons-and-inking-features"></a>Fonctionnalités d’entrée manuscrite et boutons personnalisés

Vous pouvez personnaliser et étendre la collection de boutons (et de fonctionnalités d’entrée manuscrite associées) qui sont fournis via le contrôle InkToolbar.

Le contrôle InkToolbar se compose de deux groupes de types de boutons :

1. Un groupe de boutons « outil » comprenant les boutons intégrés de dessin, d’effacement et de surlignage. Les outils et stylets personnalisés sont ajoutés ici.
> **Remarque**  La sélection de fonctionnalités est mutuellement exclusive.

2. Un groupe de boutons « bascules » contenant le bouton intégré de règle. Les boutons bascules personnalisés sont ajoutés ici.
> **Remarque**  Les fonctionnalités ne s’excluent pas mutuellement et peuvent être utilisées simultanément avec d’autres outils actifs.

En fonction de votre application et de la fonctionnalité d’entrée manuscrite requise, vous pouvez ajouter n’importe lequel des boutons suivants (liés à vos fonctionnalités d’entrée manuscrite personnalisées) au contrôle InkToolbar :

- Stylet personnalisé : un stylet dont les propriétés de palette de couleurs de l’encre et de pointe de stylet, telles que la forme, la rotation et la taille, sont définies par l’application hôte.
- Outil personnalisé : un outil autre qu’un stylet, défini par l’application hôte.
- Bascule personnalisée : définit l’état d’une fonctionnalité définie par l’application sur activé ou désactivé. Lorsqu’elle est activée, la fonctionnalité fonctionne conjointement avec l’outil actif.

> **Remarque**  Vous ne pouvez pas modifier l’ordre d’affichage des boutons intégrés. L’ordre d’affichage par défaut est le suivant : stylo à bille, crayon, surligneur, gomme et règle. Les stylets personnalisés sont ajoutés au dernier stylet par défaut, les boutons d’outil personnalisé sont ajoutés entre le dernier bouton de stylet et le bouton de gomme, et les boutons de bascule personnalisés sont ajoutés après le bouton de règle. (Les boutons personnalisés sont ajoutés dans l’ordre que vous avez spécifié.)

### <a name="custom-pen"></a>Stylet personnalisé

Vous pouvez créer un stylet personnalisé (activé par le biais d’un bouton de stylet personnalisé) dans lequel vous définissez la palette de couleurs d’entrée manuscrite et les propriétés de la pointe du stylet, telles que la forme, la rotation et la taille.

![Bouton de stylo de calligraphie personnalisé](.\images\ink\ink-tools-custompen.png)  
*Bouton de stylo de calligraphie personnalisé*

Ici, nous définissons un stylet personnalisé à pointe large qui permet de tracer des traits de calligraphie de base. Nous personnalisons également la collection de pinceaux dans la palette affichée dans le menu volant de bouton.

**Code-behind**

Tout d’abord, nous définissons notre stylet personnalisé et spécifions les attributs de dessin dans le code-behind. Nous ferons référence à ce stylet personnalisé à partir de la déclaration XAML ultérieurement.

1. Cliquez avec le bouton droit sur le projet dans l’Explorateur de solutions, puis sélectionnez Ajouter &gt; Nouvel élément.
2. Sous Visual C# -&gt; Code, ajoutez un nouveau fichier de classe et nommez-le CalligraphicPen.cs.
3. Dans le fichier Calligraphic.cs, remplacez le bloc  « using » par défaut par ce qui suit :
```csharp
using System.Numerics;
using Windows.UI;
using Windows.UI.Input.Inking;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
```

4. Spécifiez que la classe CalligraphicPen est dérivée de l’élément [InkToolbarCustomPen](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.aspx).
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
}
```

5. Remplacez l’élément [CreateInkDrawingAttributesCore](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.createinkdrawingattributescore.aspx) pour spécifier votre propre pinceau et taille de trait.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
    }
}
```

6. Créez un objet [InkDrawingAttributes](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.aspx) et définissez la [forme de la pointe du stylet](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentip.aspx), la [rotation de la pointe](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentiptransform.aspx), la [taille des traits](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.size.aspx), et la [couleur de l’encre](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.color.aspx).
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
        InkDrawingAttributes inkDrawingAttributes =
          new InkDrawingAttributes();
        inkDrawingAttributes.PenTip = PenTipShape.Circle;
        inkDrawingAttributes.Size =
          new Windows.Foundation.Size(strokeWidth, strokeWidth * 20);
        SolidColorBrush solidColorBrush = brush as SolidColorBrush;
        if (solidColorBrush != null)
        {
            inkDrawingAttributes.Color = solidColorBrush.Color;
        }
        else
        {
            inkDrawingAttributes.Color = Colors.Black;
        }

        Matrix3x2 matrix = Matrix3x2.CreateRotation(45);
        inkDrawingAttributes.PenTipTransform = matrix;

        return inkDrawingAttributes;
    }
}
```

**XAML**

Ensuite, nous ajoutons les références nécessaires au stylet personnalisé dans le fichier MainPage.xaml.

1. Nous déclarons un dictionnaire de ressources de page local qui crée une référence au stylet personnalisé (`CalligraphicPen`), définie dans le fichier CalligraphicPen.cs, ainsi qu’une [collection de pinceaux](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.UI.Xaml.Media.BrushCollection.aspx) prise en charge par le stylet personnalisé (`CalligraphicPenPalette`).
```xaml
<Page.Resources>
    <!-- Add the custom CalligraphicPen to the page resources. -->
    <local:CalligraphicPen x:Key="CalligraphicPen" />
    <!-- Specify the colors for the palette of the custom pen. -->
    <BrushCollection x:Key="CalligraphicPenPalette">
        <SolidColorBrush Color="Blue" />
        <SolidColorBrush Color="Red" />
    </BrushCollection>
</Page.Resources>
```

2. Nous ajoutons ensuite un contrôle InkToolbar à un élément [InkToolbarCustomPenButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompenbutton.aspx) enfant.

  Le bouton de stylet personnalisé inclut les deux références de ressources statiques déclarées dans les ressources de la page : `CalligraphicPen` et `CalligraphicPenPalette`.

  Nous spécifions également l’étendue du curseur de taille de trait ([MinStrokeWidth](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.minstrokewidth.aspx), [MaxStrokeWidth](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.maxstrokewidth.aspx) et [SelectedStrokeWidth](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedstrokewidthproperty.aspx)), le pinceau sélectionné ([SelectedBrushIndex](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedbrushindex.aspx)) et l’icône du bouton de stylet personnalisé ([SymbolIcon](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.symbolicon.aspx)).
```xaml
<Grid Grid.Row="1">
    <InkCanvas x:Name="inkCanvas" />
    <InkToolbar x:Name="inkToolbar"
                VerticalAlignment="Top"
                TargetInkCanvas="{x:Bind inkCanvas}">
        <InkToolbarCustomPenButton
            CustomPen="{StaticResource CalligraphicPen}"
            Palette="{StaticResource CalligraphicPenPalette}"
            MinStrokeWidth="1" MaxStrokeWidth="3" SelectedStrokeWidth="2"
            SelectedBrushIndex ="1">
            <SymbolIcon Symbol="Favorite" />
            <InkToolbarCustomPenButton.ConfigurationContent>
                <InkToolbarPenConfigurationControl />
            </InkToolbarCustomPenButton.ConfigurationContent>
        </InkToolbarCustomPenButton>
    </InkToolbar>
</Grid>
```

### <a name="custom-toggle"></a>Bascule personnalisée

Vous pouvez créer une bascule personnalisée (activée par le biais d’un bouton bascule personnalisé) pour activer ou désactiver une fonction définie par l’application. Lorsqu’elle est activée, la fonctionnalité fonctionne conjointement avec l’outil actif.

Dans cet exemple, nous définissons un bouton bascule personnalisé qui permet l’entrée manuscrite et l’entrée tactile (par défaut, l’entrée manuscrite tactile n’est pas activée).

> [!NOTE]  
> Si vous avez besoin de prendre en charge l’entrée manuscrite avec une interface tactile, il est recommandé de l’activer à l’aide d’un bouton bascule personnalisé, avec l’icône et l’info-bulle spécifiés dans cet exemple.

En règle générale, l’entrée tactile est utilisée pour la manipulation directe d’un objet ou l’interface utilisateur de l’application. Pour illustrer les différences de comportement lorsque l’entrée manuscrite tactile est activée, nous plaçons InkCanvas au sein d’un conteneur ScrollViewer, et nous définissions les dimensions de ScrollViewer de manière à ce qu’elles soient inférieures à celles de InkCanvas. 

Au démarrage de l’application, seule l’entrée manuscrite à l’aide du stylet est prise en charge et la fonction tactile est utilisée pour faire un panoramique ou un zoom sur la surface d’entrée manuscrite. Lorsque l’entrée manuscrite tactile est activée, il n’est pas possible de faire de panoramique ou de zoom sur la surface d’entrée manuscrite par le biais de l’entrée tactile.

> [!NOTE]
> Voir [Contrôles pour l’entrée manuscrite](..\controls-and-patterns\inking-controls.md) pour des recommandations en matière d’expérience utilisateur sur [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas) et [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar). Les recommandations suivantes sont pertinentes pour cet exemple :
> - [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar) et l’entrée manuscrite en règle générale sont conseillés avec un stylet actif. Toutefois, l’entrée manuscrite avec une souris et la fonctionnalité tactile peut être prise en charge si votre application l’impose. 
> - En cas de prise en charge de l’entrée manuscrite avec la fonctionnalité tactile, nous recommandons d’utiliser l’icône « ED5F » de la police « Segoe MLD2 Assets » pour le bouton bascule, avec une info-bulle « écriture tactile ». 

**XAML**

1. Tout d’abord, nous déclarons un élément [**InkToolbarCustomToggleButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbarCustomToggleButton) (toggleButton) avec un écouteur d’événements Click qui spécifie le gestionnaire d’événements (Toggle_Custom).

```xaml 
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <StackPanel Grid.Row="0" 
                x:Name="HeaderPanel" 
                Orientation="Horizontal">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10" />
    </StackPanel>

    <ScrollViewer Grid.Row="1" 
                  HorizontalScrollBarVisibility="Auto" 
                  VerticalScrollBarVisibility="Auto">
        
        <Grid HorizontalAlignment="Left" VerticalAlignment="Top">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            
            <InkToolbar Grid.Row="0" 
                        Margin="10"
                        x:Name="inkToolbar" 
                        VerticalAlignment="Top"
                        TargetInkCanvas="{x:Bind inkCanvas}">
                <InkToolbarCustomToggleButton 
                x:Name="toggleButton" 
                Click="CustomToggle_Click" 
                ToolTipService.ToolTip="Touch Writing">
                    <SymbolIcon Symbol="{x:Bind TouchWritingIcon}"/>
                </InkToolbarCustomToggleButton>
            </InkToolbar>
            
            <ScrollViewer Grid.Row="1" 
                          Height="500"
                          Width="500"
                          x:Name="scrollViewer" 
                          ZoomMode="Enabled" 
                          MinZoomFactor=".1" 
                          VerticalScrollMode="Enabled" 
                          VerticalScrollBarVisibility="Auto" 
                          HorizontalScrollMode="Enabled" 
                          HorizontalScrollBarVisibility="Auto">
                
                <Grid x:Name="outputGrid" 
                      Height="1000"
                      Width="1000"
                      Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}">
                    <InkCanvas x:Name="inkCanvas"/>
                </Grid>
                
            </ScrollViewer>
        </Grid>
    </ScrollViewer>
</Grid>
```

**Code-behind**

2. Dans l’extrait de code précédent, nous avons déclaré un écouteur d’événements Click et le gestionnaire (Toggle_Custom) sur le bouton bascule personnalisé pour les entrées manuscrites tactiles (toggleButton). Ce gestionnaire bascule simplement la prise en charge de CoreInputDeviceTypes.Touch sur la propriété InputDeviceTypes de InkPresenter.

   Nous avons également spécifié une icône pour le bouton à l’aide de l’élément SymbolIcon et de l’extension de balisage {x:Bind} qui l’associe à un champ défini dans le fichier code-behind (TouchWritingIcon).

   L’extrait de code suivant inclut à la fois le gestionnaire d’événements Click et la définition de TouchWritingIcon.

```csharp 
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomToggle : Page
    {
        Symbol TouchWritingIcon = (Symbol)0xED5F;

        public MainPage_AddCustomToggle()
        {
            this.InitializeComponent();
        }

        // Handler for the custom toggle button that enables touch inking.
        private void CustomToggle_Click(object sender, RoutedEventArgs e)
        {
            if (toggleButton.IsChecked == true)
            {
                inkCanvas.InkPresenter.InputDeviceTypes |= CoreInputDeviceTypes.Touch;
            }
            else
            {
                inkCanvas.InkPresenter.InputDeviceTypes &= ~CoreInputDeviceTypes.Touch;
            }
        }
    }
}
```

### <a name="custom-tool"></a>Outil personnalisé

Vous pouvez créer un bouton d’outil personnalisé pour appeler un outil autre qu’un stylet défini par votre application.

Par défaut, [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkPresenter) traite toutes les entrées sous forme de trait d’encre ou d’effacement. Cela inclut les entrées modifiées par une affordance de matériel secondaire, telle qu’un bouton de stylet, un bouton droit de souris ou un élément similaire. Toutefois, [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkPresenter) peut être configuré pour laisser des entrées spécifiques non traitées, qui peuvent ensuite être transmises à votre application pour un traitement personnalisé.

Dans cet exemple, nous définissons un bouton d’outil personnalisé qui, lorsqu’il est sélectionné, entraîne le traitement des traits ultérieurs et leur rendu sous forme de lasso de sélection (ligne en pointillé) et non d’entrée manuscrite. Tous les traits d’entrée manuscrite dans les limites de la zone de sélection sont définis sur [**Selected**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkStroke.Selected).

> [!NOTE]
> Voir Contrôles pour l’entrée manuscrite pour des recommandations en matière d’expérience utilisateur sur InkCanvas et InkToolbar. La recommandation suivante est pertinente pour cet exemple :
> - Dans le cas d’une sélection du trait, nous recommandons d’utiliser l’icône « EF20 » de la police « Segoe MLD2 Assets » pour le bouton outil, avec une info-bulle « outil de sélection ». 
 
**XAML**

1. Tout d’abord, nous déclarons un élément [**InkToolbarCustomToolButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) (customToolButton) avec un écouteur d’événements Click qui spécifie le gestionnaire d’événements (customToolButton_Click) dans lequel la sélection de traits est configurée. (Nous avons également ajouté un ensemble de boutons pour copier, couper et coller la sélection de traits).

2. En outre, nous ajoutons un élément de zone de dessin pour dessiner notre trait de sélection. Une couche distincte permet de dessiner le trait de sélection en s’abstenant de modifier l’élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas) ou son contenu. 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10,0,0,0" />
    </StackPanel>
    <StackPanel x:Name="ToolPanel" Orientation="Horizontal" Grid.Row="1">
        <InkToolbar x:Name="inkToolbar" 
                    VerticalAlignment="Top" 
                    TargetInkCanvas="{x:Bind inkCanvas}">
            <InkToolbarCustomToolButton 
                x:Name="customToolButton" 
                Click="customToolButton_Click" 
                ToolTipService.ToolTip="Selection tool">
                <SymbolIcon Symbol="{x:Bind SelectIcon}"/>
            </InkToolbarCustomToolButton>
        </InkToolbar>
        <Button x:Name="cutButton" 
                Content="Cut" 
                Click="cutButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="copyButton" 
                Content="Copy"  
                Click="copyButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="pasteButton" 
                Content="Paste"  
                Click="pasteButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
    </StackPanel>
    <Grid Grid.Row="2" x:Name="outputGrid" 
              Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}" 
              Height="Auto">
        <!-- Canvas for displaying selection UI. -->
        <Canvas x:Name="selectionCanvas"/>
        <!-- Canvas for displaying ink. -->
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

**Code-behind**

2. Ensuite, nous gérons l’événement Click [**InkToolbarCustomToolButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) dans le fichier code-behind MainPage.xaml.cs

   Ce gestionnaire configure [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkPresenter) pour transmettre les entrées non traitées à l’application. 

   Pour une procédure plus détaillée de ce code, voir la section Entrée directe pour traitement avancé de [Interactions avec le stylet et Windows Ink dans les applications UWP](pen-and-stylus-interactions.md).

   Nous avons également spécifié une icône pour le bouton à l’aide de l’élément SymbolIcon et de l’extension de balisage {x:Bind} qui l’associe à un champ défini dans le fichier code-behind (SelectIcon).

   L’extrait de code suivant inclut à la fois le gestionnaire d’événements Click et la définition de SelectIcon.

```csharp
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomTool : Page
    {
        // Icon for custom selection tool button.
        Symbol SelectIcon = (Symbol)0xEF20;

        // Stroke selection tool.
        private Polyline lasso;
        // Stroke selection area.
        private Rect boundingRect;

        public MainPage_AddCustomTool()
        {
            this.InitializeComponent();

            // Listen for new ink or erase strokes to clean up selection UI.
            inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
                StrokeInput_StrokeStarted;
            inkCanvas.InkPresenter.StrokesErased +=
                InkPresenter_StrokesErased;
        }

        private void customToolButton_Click(object sender, RoutedEventArgs e)
        {
            // By default, the InkPresenter processes input modified by 
            // a secondary affordance (pen barrel button, right mouse 
            // button, or similar) as ink.
            // To pass through modified input to the app for custom processing 
            // on the app UI thread instead of the background ink thread, set 
            // InputProcessingConfiguration.RightDragAction to LeaveUnprocessed.
            inkCanvas.InkPresenter.InputProcessingConfiguration.RightDragAction =
                InkInputRightDragAction.LeaveUnprocessed;

            // Listen for unprocessed pointer events from modified input.
            // The input is used to provide selection functionality.
            inkCanvas.InkPresenter.UnprocessedInput.PointerPressed +=
                UnprocessedInput_PointerPressed;
            inkCanvas.InkPresenter.UnprocessedInput.PointerMoved +=
                UnprocessedInput_PointerMoved;
            inkCanvas.InkPresenter.UnprocessedInput.PointerReleased +=
                UnprocessedInput_PointerReleased;
        }

        // Handle new ink or erase strokes to clean up selection UI.
        private void StrokeInput_StrokeStarted(
            InkStrokeInput sender, Windows.UI.Core.PointerEventArgs args)
        {
            ClearSelection();
        }

        private void InkPresenter_StrokesErased(
            InkPresenter sender, InkStrokesErasedEventArgs args)
        {
            ClearSelection();
        }

        private void cutButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
            inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            ClearSelection();
        }

        private void copyButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
        }

        private void pasteButton_Click(object sender, RoutedEventArgs e)
        {
            if (inkCanvas.InkPresenter.StrokeContainer.CanPasteFromClipboard())
            {
                inkCanvas.InkPresenter.StrokeContainer.PasteFromClipboard(
                    new Point(0, 0));
            }
            else
            {
                // Cannot paste from clipboard.
            }
        }

        // Clean up selection UI.
        private void ClearSelection()
        {
            var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
            foreach (var stroke in strokes)
            {
                stroke.Selected = false;
            }
            ClearBoundingRect();
        }

        private void ClearBoundingRect()
        {
            if (selectionCanvas.Children.Any())
            {
                selectionCanvas.Children.Clear();
                boundingRect = Rect.Empty;
            }
        }

        // Handle unprocessed pointer events from modifed input.
        // The input is used to provide selection functionality.
        // Selection UI is drawn on a canvas under the InkCanvas.
        private void UnprocessedInput_PointerPressed(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Initialize a selection lasso.
            lasso = new Polyline()
            {
                Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                StrokeThickness = 1,
                StrokeDashArray = new DoubleCollection() { 5, 2 },
            };

            lasso.Points.Add(args.CurrentPoint.RawPosition);

            selectionCanvas.Children.Add(lasso);
        }

        private void UnprocessedInput_PointerMoved(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Add a point to the lasso Polyline object.
            lasso.Points.Add(args.CurrentPoint.RawPosition);
        }

        private void UnprocessedInput_PointerReleased(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Add the final point to the Polyline object and 
            // select strokes within the lasso area.
            // Draw a bounding box on the selection canvas 
            // around the selected ink strokes.
            lasso.Points.Add(args.CurrentPoint.RawPosition);

            boundingRect =
                inkCanvas.InkPresenter.StrokeContainer.SelectWithPolyLine(
                    lasso.Points);

            DrawBoundingRect();
        }

        // Draw a bounding rectangle, on the selection canvas, encompassing 
        // all ink strokes within the lasso area.
        private void DrawBoundingRect()
        {
            // Clear all existing content from the selection canvas.
            selectionCanvas.Children.Clear();

            // Draw a bounding rectangle only if there are ink strokes 
            // within the lasso area.
            if (!((boundingRect.Width == 0) ||
                (boundingRect.Height == 0) ||
                boundingRect.IsEmpty))
            {
                var rectangle = new Rectangle()
                {
                    Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                    StrokeThickness = 1,
                    StrokeDashArray = new DoubleCollection() { 5, 2 },
                    Width = boundingRect.Width,
                    Height = boundingRect.Height
                };

                Canvas.SetLeft(rectangle, boundingRect.X);
                Canvas.SetTop(rectangle, boundingRect.Y);

                selectionCanvas.Children.Add(rectangle);
            }
        }
    }
}
```



### <a name="custom-ink-rendering"></a>Restitution d’une entrée manuscrite personnalisée

Par défaut, l’entrée manuscrite est traitée sur un thread d’arrière-plan de faible latence et restituée « humide » comme elle est dessinée. Lorsque le trait est terminé (stylet ou doigt relevé, ou bouton de la souris relâché), le trait est traité sur le thread de l’interface utilisateur et restitué « sec » à la couche [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) (au-dessus du contenu de l’application et en remplaçant l’encre humide).

La plateforme d’entrée manuscrite vous permet de remplacer ce comportement et de personnaliser entièrement l’expérience d’entrée manuscrite par un séchage personnalisé de cette dernière.

Pour plus d’informations sur le séchage personnalisé, voir [Interactions avec le stylet et Windows Ink dans les applications UWP](https://msdn.microsoft.com/en-us/windows/uwp/input-and-devices/pen-and-stylus-interactions#custom-ink-rendering).

> [!NOTE]
> Séchage personnalisé et [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)  
> Si votre application remplace le comportement par défaut du rendu d’entrée manuscrite de l’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) par une implémentation de séchage personnalisé, les traits d’encre restitués ne sont plus disponibles pour l’élément InkToolbar et les commandes d’effacement intégrées de l’élément InkToolbar ne fonctionneront pas comme prévu. Pour fournir des fonctionnalités d’effacement, vous devez gérer tous les événements de pointeur, effectuer le test de positionnement sur chaque trait et remplacer la commande intégrée « Effacer toutes les entrées manuscrites ».

## <a name="related-articles"></a>Articles connexes

* [Interactions avec le stylo et le stylet](pen-and-stylus-interactions.md)

**Exemples**
* [Exemple d’entrée manuscrite](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Exemple d’entrée manuscrite simple](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Exemple d’entrée manuscrite complexe](http://go.microsoft.com/fwlink/p/?LinkID=620314)



<!--HONumber=Dec16_HO1-->


