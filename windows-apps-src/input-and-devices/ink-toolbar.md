---
author: Karl-Bridge-Microsoft
Description: "Ajoutez un élément InkToolbar par défaut à une application d’entrée manuscrite de plateforme Windows universelle (UWP), ajoutez un bouton de stylet personnalisé à l’élément InkToolbar et liez le bouton de stylet personnalisé à une définition de stylet personnalisé."
title: "Ajouter un élément InkToolbar à une application d’entrée manuscrite de plateforme Windows universelle (UWP)"
label: Add an InkToolbar to a Universal Windows Platform (UWP) inking app
template: detail.hbs
keyword: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas, InkToolbar, Universal Windows Platform, UWP
translationtype: Human Translation
ms.sourcegitcommit: 743ce43be6220d96d7e9bc295ab72bdec4905edc
ms.openlocfilehash: 4c50c8ded127b2261df901d9da3210ff397b00ca

---

# Ajouter un élément InkToolbar à une application d’entrée manuscrite de plateforme Windows universelle (UWP)

Il existe deux contrôles différents qui facilitent l’entrée manuscrite dans les applications de plateforme Windows universelle (UWP): [**InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) et [**InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

Le contrôle [**InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) fournit les fonctionnalités Windows Ink de base. Utilisez-le pour restituer une entrée de stylet sous la forme d’un trait d’encre (via les paramètres par défaut de couleur et d’épaisseur) ou d’un trait d’effacement.

> Pour plus d’informations sur l’implémentation du contrôle InkCanvas, consultez [Interactions avec le stylo et le stylet dans les applicationsUWP](pen-and-stylus-interactions.md).

En tant que superposition totalement transparente, le contrôle InkCanvas ne fournit pas d’interface utilisateur intégrée pour configurer les propriétés relatives aux traits d’encre. Si vous souhaitez modifier l’expérience d’entrée manuscrite par défaut, permettre aux utilisateurs de configurer les propriétés relatives aux traits d’encre et prendre en charge d’autres fonctionnalités d’entrée manuscrite personnalisées, deux options s’offrent à vous:

- Dans le code-behind, utilisez l’objet [**InkPresenter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx) sous-jacent lié au contrôle InkCanvas.

  Les API InkPresenter prennent en charge une personnalisation complète de l’expérience d’entrée manuscrite. Pour plus d’informations, consultez [Interactions avec le stylo et le stylet dans les applicationsUWP](pen-and-stylus-interactions.md).

- Liez un contrôle [**InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) au contrôle InkCanvas. Par défaut, le contrôle InkToolbar fournit une interface utilisateur de base pour l’activation des fonctionnalités d’entrée manuscrite et la configuration des propriétés relatives aux traits d’encre, telles que la taille du trait, la couleur de l’encre et la forme de la pointe du stylet.

  Cette rubrique est dédiée au contrôle InkToolbar.

## API importantes

  -   [**Classe InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx)
  -   [**Classe InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)
  -   [**Classe InkPresenter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx)
  -   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)


## Contrôle InkToolbar par défaut

Par défaut, le contrôle InkToolbar comprend des boutons pour dessiner, effacer, surligner et afficher une règle. Selon la fonctionnalité, d’autres paramètres et commandes tels que la couleur de l’encre, l’épaisseur du trait, la suppression totale, sont fournis dans un menu volant.

![InkToolbar](.\images\ink\ink-tools-invoked-toolbar-small.png)  
*Barre d’outils WindowsInk par défaut*

Pour ajouter un contrôle InkToolbar de base par défaut:
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

## Personnalisation de base

Dans cette section, nous allons aborder quelques scénarios de personnalisation de barre d’outils Windows Ink de base.

### Spécifier le bouton sélectionné  
![Bouton de crayon sélectionné lors de l’initialisation](.\images\ink\ink-tools-default-toolbar.png)  
*Barre d’outilsWindowsInk avec le bouton de crayon sélectionné lors de l’initialisation*

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

3. Dans le gestionnaire pour l’événement [Loaded](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx):
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

### Spécifier les boutons intégrés

![Boutons spécifiques inclus lors de l’initialisation](.\images\ink\ink-tools-specific.png)  
*Boutons spécifiques inclus lors de l’initialisation*

Comme mentionné précédemment, la barre d’outils Windows Ink comprend une collection de boutons intégrés par défaut. Ces boutons sont affichés dans l’ordre suivant (de gauche à droite):

- [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx)
- [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx)
- [InkToolbarHighlighterButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarhighlighterbutton.aspx)
- [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx)
- [InkToolbarRulerButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarrulerbutton.aspx)

Ici, nous initialisons la barre d’outils avec les boutons intégrés de stylo à bille, crayon et gomme uniquement.

Vous pouvez effectuer cette opération à l’aide de la déclaration XAML ou du code-behind.

**XAML**

Modifiez la déclaration XAML pour les contrôles InkCanvas et InkToolbar du premier exemple.
- Ajouter un attribut [InitialControls](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx)et définissez sa valeur sur «[None](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)». Cela permet d’effacer la collection de boutons intégrés par défaut.
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

3. Définissez [InitialControls](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx) sur «[None](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)».
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

## Fonctionnalités d’entrée manuscrite et boutons personnalisés

Vous pouvez personnaliser et étendre la collection de boutons (et de fonctionnalités d’entrée manuscrite associées) qui sont fournis via le contrôle InkToolbar.

Le contrôle InkToolbar se compose de deux groupes de types de boutons:

1. Un groupe de boutons «outil» comprenant les boutons intégrés de dessin, d’effacement et de surlignage. Les outils et stylets personnalisés sont ajoutés ici.
> **Remarque**&nbsp;&nbsp;La sélection de fonctionnalités est mutuellement exclusive.

2. Un groupe de boutons «bascules» contenant le bouton intégré de règle. Les boutons bascules personnalisés sont ajoutés ici.
> **Remarque**&nbsp;&nbsp;Les fonctionnalités ne s’excluent pas mutuellement et peuvent être utilisées simultanément avec d’autres outils actifs.

En fonction de votre application et de la fonctionnalité d’entrée manuscrite requise, vous pouvez ajouter n’importe lequel des boutons suivants (liés à vos fonctionnalités d’entrée manuscrite personnalisées) au contrôle InkToolbar:

- Stylet personnalisé: un stylet dont les propriétés de palette de couleurs de l’encre et de pointe de stylet, telles que la forme, la rotation et la taille, sont définies par l’application hôte.
- Outil personnalisé: un outil autre qu’un stylet, défini par l’application hôte.
- Bascule personnalisée: définit l’état d’une fonctionnalité définie par l’application sur activé ou désactivé. Lorsqu’elle est activée, la fonctionnalité fonctionne conjointement avec l’outil actif.

> **Remarque**&nbsp;&nbsp;Vous ne pouvez pas modifier l’ordre d’affichage des boutons intégrés. L’ordre d’affichage par défaut est le suivant: stylo à bille, crayon, surligneur, gomme et règle. Les stylets personnalisés sont ajoutés au dernier stylet par défaut, les boutons d’outil personnalisé sont ajoutés entre le dernier bouton de stylet et le bouton de gomme, et les boutons de bascule personnalisés sont ajoutés après le bouton de règle. (Les boutons personnalisés sont ajoutés dans l’ordre que vous avez spécifié.)

### Stylet personnalisé

![Bouton de stylo de calligraphie personnalisé](.\images\ink\ink-tools-custompen.png)  
*Bouton de stylo de calligraphie personnalisé*

Ici, nous définissons un stylet personnalisé à pointe large qui permet de tracer des traits de calligraphie de base. Nous personnalisons également la collection de pinceaux dans la palette affichée dans le menu volant de bouton.

**Code-behind**

Tout d’abord, nous définissons notre stylet personnalisé et spécifions les attributs de dessin dans le code-behind. Nous ferons référence à ce stylet personnalisé à partir de la déclaration XAML ultérieurement.

1. Cliquez avec le bouton droit sur le projet dans l’Explorateur de solutions, puis sélectionnez Ajouter &gt; Nouvel élément.
2. Sous Visual C# -&gt; Code, ajoutez un nouveau fichier de classe et nommez-le CalligraphicPen.cs.
3. Dans le fichier Calligraphic.cs, remplacez le bloc «using» par défaut par ce qui suit:
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

  Le bouton de stylet personnalisé inclut les deux références de ressources statiques déclarées dans les ressources de la page: `CalligraphicPen` et `CalligraphicPenPalette`.

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

<!--
### Custom toggle

Enable touch inking

>**Note**&nbsp;&nbsp;InkToolbar supports pen and mouse input and can be configured to recognize touch input.
-->


## Articles connexes

* [Interactions avec le stylo et le stylet](pen-and-stylus-interactions.md)

**Exemples**
* [Exemple d’entrée manuscrite](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Exemple d’entrée manuscrite simple](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Exemple d’entrée manuscrite complexe](http://go.microsoft.com/fwlink/p/?LinkID=620314)



<!--HONumber=Aug16_HO3-->


