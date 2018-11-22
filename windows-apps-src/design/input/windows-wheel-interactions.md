---
author: Karl-Bridge-Microsoft
Description: Incorporate speech into your apps using Cortana voice commands, speech recognition, and speech synthesis.
title: Interactions avec Surface Dial
label: Surface Dial interactions
template: detail.hbs
keywords: Surface Dial, Windows Wheel, RadialController, contrôleur Radial, interaction utilisateur, entrées
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.assetid: e7deb1d6-feeb-471e-9a83-26386d1aaf37
ms.localizationpriority: medium
ms.openlocfilehash: 0a360bf936843450f5646c0e4e03ad9c3bac34d2
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7568314"
---
# <a name="surface-dial-interactions"></a>Interactions avec Surface Dial

![Image de Surface Dial avec Surface Studio](images/windows-wheel/dial-pen-studio-600px.png)  
*Surface Dial avec Surface Studio et stylet* (disponible à l’achat auprès de la [Boutique Microsoft](https://aka.ms/purchasesurfacedial)).

## <a name="overview"></a>Vue d’ensemble

Les appareils Windows wheel, tels que Surface Dial, sont une nouvelle catégorie d’appareils d’entrée permettant un éventail d’expériences d’interaction uniques et attrayantes pour Windows et les applications Windows. 

> [!IMPORTANT]
> Dans cette rubrique, nous faisons spécifiquement référence aux interactions avec Surface Dial, mais les informations sont applicables à tous les appareils Windows wheel. 

| Vidéos |   |
| --- | --- |
| <iframe src="https://www.youtube-nocookie.com/embed/WMklcdzcNcU" width="300" height="200" allowFullScreen="true" frameBorder="0"></iframe> | <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="300" height="200" allowFullScreen="true" frameBorder="0"></iframe> |
| *Partenaires d’applications Surface Dial* | *Surface Dial pour les développeurs* |

Avec un format appelant à une action de *rotation* (ou de mouvement), Surface Dial est conçu à la manière d’un appareil d’entrée secondaire multimode venant compléter la saisie à partir d’un appareil principal. Dans la plupart des cas, l’utilisateur manipule l’appareil avec sa main non dominante tout en effectuant une tâche avec sa main dominante (par exemple, l’entrée manuscrite avec un stylet). L’appareil n’est pas conçu pour effectuer des entrées d’un pointeur de précision (par exemple, interaction tactile, stylet ou souris). 

Surface Dial prend également en charge les actions *Appui prolongé* et *Clic*. L’appui prolongé a une fonction unique: afficher un menu de commandes. Si le menu est actif, l’entrée rotation et clic est traitée par le menu. Dans le cas contraire, l’entrée est transmise à votre application pour le traitement. 

**Comme avec tous les appareils d’entrée Windows, vous pouvez personnaliser l’expérience d’interaction Surface Dial pour l’adapter aux fonctionnalités dans vos applications.**

> [!TIP]
> Utilisés conjointement, Surface Dial et le nouveau Surface Studio peuvent fournir une expérience utilisateur encore plus originale.  
>
>Outre l’expérience de menu d’appui prolongé par défaut décrite, Surface Dial peut également être placé directement sur l’écran de Surface Studio. Cela permet d’afficher un menu «à l’écran» spécial. 
>
>Le système utilise les informations de détection de l’emplacement de contact et des limites de Surface Dial pour traiter l’occlusion par l’appareil et afficher une version plus grande du menu encerclant la partie extérieure de Surface Dial. Ces mêmes informations peuvent également être utilisées par votre application pour adapter l’interface utilisateur à la présence de l’appareil et à son utilisation prévue, notamment au placement de la main et du bras de l’utilisateur.

| Menu hors écran Surface Dial | | Menu à l’écran Surface Dial |
| --- | --- | --- |
| ![Menu hors écran Surface Dial](images/windows-wheel/surface-dial-menu-offscreen.png) | | ![Menu à l’écran Surface Dial](images/windows-wheel/surface-dial-menu-onscreen.png) |

## <a name="system-integration"></a>Intégration du système

Surface Dial est étroitement intégré à Windows et prend en charge un ensemble d’outils intégrés dans le menu: volume du système, défilement, zoom avant/arrière et fonctions annuler/rétablir.

Cet ensemble d’outils intégrés s’adapte au contexte du système actuel afin d’inclure:
- Un outil de luminosité du système lorsque l’utilisateur est sur le bureau Windows
- Un outil de piste précédente/suivante lors de la lecture de contenu multimédia

Outre cette prise en charge de la plateforme générale, Surface Dial est également étroitement intégré avec les contrôles de la plateforme Windows Ink ([**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas) et [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar)).

![Surface Dial avec stylet Surface](images/windows-wheel/dial-and-pen-400px.png)  
*Surface Dial avec stylet Surface*

Lorsqu’ils sont utilisés avec Surface Dial, ces contrôles activent des fonctionnalités supplémentaires pour modifier les attributs d’entrée manuscrite et contrôler le gabarit de la règle de la barre d’outils d’entrée manuscrite.

Lorsque vous ouvrez le menu Surface Dial dans une application d’entrée manuscrite qui utilise la barre d’outils d’entrée manuscrite, le menu inclut désormais des outils permettant de contrôler le type de stylet et l’épaisseur du pinceau. Lorsque la règle est activée, un outil correspondant est ajouté au menu, permettant à l’appareil de contrôler la position et l’angle de la règle.

![Menu Surface Dial avec outil de sélection de stylet pour la barre d’outils Windows Ink](images/windows-wheel/surface-dial-menu-inktoolbar-pen.png)  
*Menu Surface Dial avec outil de sélection de stylet pour la barre d’outils Windows Ink*

![Menu Surface Dial avec outil de taille du trait pour la barre d’outils Windows Ink](images/windows-wheel/surface-dial-menu-inktoolbar-strokesize.png)  
*Menu Surface Dial avec outil de taille du trait pour la barre d’outils Windows Ink*

![Menu Surface Dial avec règle pour la barre d’outils Windows Ink](images/windows-wheel/surface-dial-menu-inktoolbar-ruler.png)  
*Menu Surface Dial avec règle pour la barre d’outils Windows Ink*

## <a name="user-customization"></a>Personnalisation de l’utilisateur

Les utilisateurs peuvent personnaliser certains aspects de leur expérience Surface Dial à travers la page **Paramètres Windows -&gt; Appareils -&gt; Wheel**, notamment les outils par défaut, la vibration (ou retour haptique) et la main d’écriture (ou dominante). 

Durant la personnalisation de l’expérience utilisateur Surface Dial, vous devez toujours vous assurer qu’une fonction ou un comportement spécifiques sont disponibles et activés par l’utilisateur.

## <a name="custom-tools"></a>Outils personnalisés

Ici, nous abordons des recommandations en matière d’expérience utilisateur et des conseils aux développeurs afin de personnaliser les outils exposés dans le menu Surface Dial.

### <a name="ux-guidance"></a>Recommandations en matière d’expérience utilisateur

**S’assurer que vos outils correspondent au contexte actuel** Lorsque les fonctions d’un outil sont claires et intuitives pour vous et que vous comprenez le fonctionnement de l’interaction Surface Dial, vous aidez les utilisateurs à apprendre rapidement et à rester concentrés sur leur tâche.

**Réduire autant que possible le nombre d’outils de l’application**  
Le menu Surface Dial peut accueillir sept éléments. S’il y a huit éléments ou plus, l’utilisateur doit tourner Surface Dial pour voir les outils disponibles dans un menu volant de dépassement, ce qui complique la navigation dans le menu, ainsi que l’accès et la sélection des outils.

Nous vous recommandons de fournir un seul outil personnalisé pour votre application ou contexte de l’application. Cela vous permet de définir cet outil en fonction de ce que fait l’utilisateur, sans exiger qu’il active le menu Surface Dial, puis sélectionne un outil. 

**Mettre à jour dynamiquement la collection d’outils**  
Étant donné que les éléments de menu Surface Dial ne prennent pas en charge un état désactivé, vous devez dynamiquement ajouter et supprimer des outils (y compris les outils intégrés, par défaut) basés sur le contexte utilisateur (affichage actuel ou fenêtre ciblée). Si un outil n’est pas pertinent pour l’activité en cours ou s’il est redondant, supprimez-le.

> [!IMPORTANT]
> Lorsque vous ajoutez un élément au menu, assurez-vous que l’élément n’existe pas déjà.

**Ne pas supprimer l’outil de réglage du volume système intégré**  
Généralement, le contrôle du volume est toujours requis par l’utilisateur. En effet, il peut écouter de la musique tout en utilisant votre application; les outils de volume et de piste suivante doivent donc toujours être accessibles à partir du menu Surface Dial. (L’outil de piste suivante est automatiquement ajouté au menu lors de la lecture de contenu multimédia.)

**Être cohérent dans l’organisation de menu**  
Cela aide les utilisateurs à découvrir et à apprendre quels outils sont disponibles lorsqu’ils utilisent votre application, et cela permet d’améliorer leur efficacité lorsqu’ils changent d’outils.

**Fournir des icônes de qualité, cohérentes avec les icônes intégrées**  
Les icônes transmettent un message de professionnalisme et d’excellence et suscitent la confiance des utilisateurs.
- Fournissez une image PNG 64x64pixels de qualité (44x44 est le plus petit format pris en charge)
- Assurez-vous que l’arrière-plan est transparent
- L’icône doit remplir la majeure partie de l’image
- Une icône blanche doit avoir un contour noir pour être visible en mode de contraste élevé

|   |   |   |
| --- | --- | --- |
| ![Icône avec arrière-plan alpha](images/windows-wheel/surface-dial-menu-icon1.png) | ![Icône affichée dans le menu wheel avec le thème par défaut](images/windows-wheel/surface-dial-menu-icon2.png) | ![Menu à l’écran Surface Dial](images/windows-wheel/surface-dial-menu-icon3.png) |
| *Icône avec arrière-plan alpha* | *Icône affichée dans le menu wheel avec le thème par défaut* | *Icône affichée dans le menu wheel avec le thème contraste blanc élevé par défaut* |

**Utiliser des noms concis et descriptifs**  
Le nom de l’outil s’affiche dans le menu des outils, de même que l’icône de l’outil et est également utilisé par les lecteurs d’écran. 
- Les noms doivent être courts pour tenir dans le cercle central du menu wheel
- Les noms doivent clairement identifier l’action principale (une action complémentaire peut être impliquée):
  - Le défilement indique l’effet des deux sens de rotation
  - La fonction Annuler spécifie une action principale, mais la fonction Rétablir (l’action complémentaire) peut être déduite et facilement détectée par l’utilisateur

### <a name="developer-guidance"></a>Conseils aux développeurs

Vous pouvez personnaliser l’expérience Surface Dial pour compléter les fonctionnalités dans vos applications par le biais d’un ensemble complet [d’API Windows Runtime](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController). 

Comme indiqué précédemment, le menu Surface Dial par défaut est prérempli avec un ensemble d’outils intégrés couvrant un large éventail de fonctionnalités de base du système (volume du système, luminosité du système, défilement, zoom, fonction Annuler et contrôle multimédia lorsque le système détecte des données une lecture audio ou vidéo en cours). Toutefois, ces outils par défaut peuvent ne pas offrir les fonctionnalités requises par votre application. 

Dans les sections suivantes, nous décrivons comment ajouter un outil personnalisé au menu Surface Dial, et comment spécifier les outils intégrés exposés.

**Ajouter un outil personnalisé**

Dans cet exemple, nous ajoutons un outil personnalisé de base qui transmet les données d’entrée à partir des événements de rotation et de clic à certains contrôles de l’interface utilisateur XAML.

1. Tout d’abord, nous déclarons notre interface utilisateur (un curseur et un bouton bascule) en XAML.

   ![Image de l’interface utilisateur de l’exemple d’application](images/windows-wheel/surface-dial-snippet-customtool1.png)  
   *L’interface utilisateur de l’exemple d’application*

    ```Xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
      </Grid.RowDefinitions>
      <StackPanel x:Name="HeaderPanel" 
        Orientation="Horizontal" 
        Grid.Row="0">
          <TextBlock x:Name="Header"
            Text="RadialController customization sample"
            VerticalAlignment="Center"
            Style="{ThemeResource HeaderTextBlockStyle}"
            Margin="10,0,0,0" />
      </StackPanel>
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center"
        Grid.Row="1">
          <!-- Slider for rotation input -->
          <Slider x:Name="RotationSlider"
            Width="300"
            HorizontalAlignment="Left"/>
          <!-- Switch for click input -->
          <ToggleSwitch x:Name="ButtonToggle"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    ```

2. Ensuite, en code-behind, nous ajoutons un outil personnalisé au menu Surface Dial et déclarons les gestionnaires d’entrée [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController). 

   Nous obtenons une référence à l’objet [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) pour Surface Dial (myController) en appelant [**CreateForCurrentView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.CreateForCurrentView).

   Ensuite, nous créons une instance de [**RadialControllerMenuItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuItem) (myItem) en appelant [**RadialControllerMenuItem.CreateFromIcon**](https://msdn.microsoft.com/library/windows/apps/mt759255). 

   Ensuite, nous ajoutons cet élément à la collection d’éléments de menu.

   Nous déclarons les gestionnaires d’événements d’entrée ([**ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked) et [**RotationChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationChanged)) pour l’objet [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController).

   Enfin, nous définissons les gestionnaires d’événements.

    ```csharp
    public sealed partial class MainPage : Page
    {
        RadialController myController;

        public MainPage()
        {
            this.InitializeComponent();
            // Create a reference to the RadialController.
            myController = RadialController.CreateForCurrentView();

            // Create an icon for the custom tool.
            RandomAccessStreamReference icon =
              RandomAccessStreamReference.CreateFromUri(
                new Uri("ms-appx:///Assets/StoreLogo.png"));

            // Create a menu item for the custom tool.
            RadialControllerMenuItem myItem =
              RadialControllerMenuItem.CreateFromIcon("Sample", icon);

            // Add the custom tool to the RadialController menu.
            myController.Menu.Items.Add(myItem);

            // Declare input handlers for the RadialController.
            myController.ButtonClicked += MyController_ButtonClicked;
            myController.RotationChanged += MyController_RotationChanged;
        }

        // Handler for rotation input from the RadialController.
        private void MyController_RotationChanged(RadialController sender,
          RadialControllerRotationChangedEventArgs args)
        {
            if (RotationSlider.Value + args.RotationDeltaInDegrees > 100)
            {
                RotationSlider.Value = 100;
                return;
            }
            else if (RotationSlider.Value + args.RotationDeltaInDegrees < 0)
            {
                RotationSlider.Value = 0;
                return;
            }
            RotationSlider.Value += args.RotationDeltaInDegrees;
        }

        // Handler for click input from the RadialController.
        private void MyController_ButtonClicked(RadialController sender,
          RadialControllerButtonClickedEventArgs args)
        {
            ButtonToggle.IsOn = !ButtonToggle.IsOn;
        }
    }
    ```

Lorsque nous exécutons l’application, nous utilisons Surface Dial pour interagir avec celle-ci. Tout d’abord, nous appuyons de manière prolongée pour ouvrir le menu et sélectionner notre outil personnalisé. Une fois que l’outil personnalisé est activé, le contrôle du curseur peut être ajusté en tournant Surface Dial, et le commutateur peut être basculé en cliquant sur Surface Dial.

![Image de l’interface utilisateur de l’exemple d’application activée à l’aide de l’outil personnalisé de Surface Dial](images/windows-wheel/surface-dial-snippet-customtool2.png)  
*L’interface utilisateur de l’exemple d’application activée à l’aide de l’outil personnalisé de Surface Dial*

**Spécifier les boutons intégrés**

Vous pouvez utiliser la classe [**RadialControllerConfiguration**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerConfiguration) pour personnaliser la collection d’éléments de menu intégrés pour votre application.

Par exemple, si votre application n’a pas de région de défilement ou de zoom et ne requiert pas de fonctionnalité Annuler/Rétablir, ces outils peuvent être supprimés du menu. Cela ouvre l’espace sur le menu permettant d’ajouter des outils personnalisés pour votre application. 

> [!IMPORTANT] 
> Le menu Surface Dial doit contenir au moins un élément de menu. Si tous les outils par défaut sont supprimés avant que vous n’ajoutiez l’un de vos outils personnalisés, les outils par défaut sont restaurés et votre outil est ajouté à la collection par défaut.

Conformément aux recommandations en matière de conception, nous déconseillons de supprimer les outils de contrôle multimédia (volume et piste précédente/suivante), car les utilisateurs écoutent souvent de la musique en arrière-plan tout en effectuant d’autres tâches.

Ici, nous montrons comment configurer le menu Surface Dial pour inclure uniquement des contrôles multimédias de volume et de piste précédente/suivante.

```csharp
public MainPage()
{
  ...
  //Remove a subset of the default system tools
  RadialControllerConfiguration myConfiguration = 
  RadialControllerConfiguration.GetForCurrentView();
  myConfiguration.SetDefaultMenuItems(new[] 
  {
    RadialControllerSystemMenuItemKind.Volume,
      RadialControllerSystemMenuItemKind.NextPreviousTrack
  });
}
```

## <a name="custom-interactions"></a>Interactions personnalisées

Comme indiqué, Surface Dial prend en charge trois mouvements (appui prolongé, rotation, clic) avec les interactions par défaut correspondantes. 

Assurez-vous que toutes les interactions personnalisées basées sur ces mouvements sont cohérentes pour l’action ou l’outil sélectionnés. 

> [!NOTE]
> L’expérience d’interaction dépend de l’état du menu Surface Dial. Si le menu est actif, il traite l’entrée; dans le cas contraire, votre application s’en charge.

### <a name="press-and-hold"></a>Appuyer de manière prolongée

Ce mouvement active et affiche le menu Surface Dial; aucune fonctionnalité d’application n’est associée à ce mouvement. 

Par défaut, le menu est affiché au centre de l’écran de l’utilisateur. Toutefois, l’utilisateur peut le prendre et le déplacer à l’emplacement de son choix.

> [!NOTE]
> Lorsque Surface Dial est placé sur l’écran de Surface Studio, le menu est centré sur l’emplacement à l’écran de Surface Dial.

### <a name="rotate"></a>Faire pivoter

Surface Dial est essentiellement conçu pour prendre en charge la rotation pour les interactions impliquant des ajustements fluides et incrémentiels aux valeurs ou contrôles analogiques.

L’appareil peut être pivoté dans le sens des aiguilles d’une montre et dans le sens inverse, et peut également fournir un retour haptique indiquant les distances discrètes.

> [!NOTE]
> Le retour haptique peut être désactivé par l’utilisateur à la page **Paramètres Windows -&gt; Appareils -&gt; Wheel**.

#### <a name="ux-guidance"></a>Recommandations en matière d’expérience utilisateur

**Les outils dotés d’une sensibilité continue ou rotative élevée doivent désactiver le retour haptique**

Le retour haptique correspond à la sensibilité rotative de l’outil actif. Nous recommandons de désactiver le retour haptique pour les outils dotés d’une sensibilité continue ou rotative élevée, car l’expérience utilisateur peut devenir désagréable. 

**La main dominante ne doit pas affecter les interactions basées sur la rotation**

Surface Dial ne peut pas détecter la main utilisée, mais l’utilisateur peut définir la main d’écriture (ou main dominante) dans **Paramètres Windows -&gt; Appareil -&gt; Stylet et Windows Ink**.

**Les paramètres régionaux doivent être pris en compte dans toutes les interactions de rotation**

Optimisez la satisfaction client en tenant compte et en adaptant vos interactions aux paramètres régionaux et aux dispositions de droite à gauche.

Les outils et commandes intégrés au menu Surface Dial suivent ces recommandations pour les interactions basées sur la rotation:

|   |   |   |
| --- | --- | --- |
| Vers la gauche<br/>Vers le haut<br/>Vers l’extérieur | ![Image de Surface Dial](images/windows-wheel/surface-dial-rotate.png) | Vers la droite<br/>Vers le bas<br/>Vers l’intérieur |
|   |   |   |

| Direction conceptuelle | Mappage sur Surface Dial | Rotation dans le sens des aiguilles d’une montre | Rotation dans le sens inverse des aiguilles d’une montre |
| --- | --- | --- | --- |
| Horizontale | Mappage gauche et droit basé sur le haut de Surface Dial | Vers la droite | Vers la gauche |
| Verticale | Mappage supérieur et inférieur basé sur le côté gauche de Surface Dial | Vers le bas | Vers le haut |
| AxeZ | Vers l’intérieur (plus près), mappage sur le haut/la droite<br/>Vers l’extérieur (plus loin), mappage sur le bas/la gauche | Vers l’intérieur | Vers l’extérieur |

#### <a name="developer-guidance"></a>Conseils aux développeurs

Lorsque l’utilisateur fait pivoter l’appareil, les événements [**RadialController.RotationChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationChanged) sont déclenchés selon un delta ([**RadialControllerRotationChangedEventArgs.RotationDeltaInDegrees**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerRotationChangedEventArgs.RotationDeltaInDegrees)) relatif à la direction de la rotation. La sensibilité (ou résolution) des données peut être définie avec la propriété [**RadialController.RotationResolutionInDegrees**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationResolutionInDegrees).

> [!NOTE]
> Par défaut, un événement d’entrée rotatif est transmis à un objet [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) uniquement lorsque l’appareil est pivoté de 10degrés au moins. Chaque événement d’entrée entraîne une vibration de l’appareil.

En règle générale, nous recommandons de désactiver le retour haptique lorsque la résolution de la rotation présente une valeur inférieure à 5degrés. Cela offre une expérience plus fluide pour les interactions continues. 

Vous pouvez activer et désactiver le retour haptique pour les outils personnalisés en définissant la propriété [**RadialController.UseAutomaticHapticFeedback**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.UseAutomaticHapticFeedback).

> [!NOTE]
> Vous ne pouvez pas remplacer le comportement haptique pour les outils système tels que le contrôle du volume. Pour ces outils, le retour haptique peut uniquement être désactivé par l’utilisateur à partir de la page de paramètres Wheel.

Voici un exemple illustrant comment personnaliser la résolution des données de rotation, et comment activer ou désactiver le retour haptique.

```csharp
private void MyController_ButtonClicked(RadialController sender, 
  RadialControllerButtonClickedEventArgs args)
{
  ButtonToggle.IsOn = !ButtonToggle.IsOn;

  if(ButtonToggle.IsOn)
  {
    //high resolution mode
    RotationSlider.LargeChange = 1;
    myController.UseAutomaticHapticFeedback = false;
    myController.RotationResolutionInDegrees = 1;
  }
  else
  {
    //low resolution mode
    RotationSlider.LargeChange = 10;
    myController.UseAutomaticHapticFeedback = true;
    myController.RotationResolutionInDegrees = 10;
  }
}
```

### <a name="click"></a>Cliquer

Cliquer sur Surface Dial équivaut à cliquer sur le bouton gauche de la souris (l’état de rotation de l’appareil ne produit aucun effet sur cette action).

#### <a name="ux-guidance"></a>Recommandations en matière d’expérience utilisateur

**Ne pas mapper une action ou une commande à ce mouvement si l’utilisateur ne peut pas facilement récupérer à partir du résultat**

Toute action effectuée par votre application suite au clic d’un utilisateur sur Surface Dial doit être réversible. Autorisez toujours l’utilisateur à facilement traverser la pile arrière de l’application et à restaurer l’état précédent de l’application.

Les opérations binaires comme les fonctions désactiver les notifications/réactiver les notifications ou afficher/masquer fournissent des expériences utilisateur de qualité avec le clic.

**Les outils de mode ne doivent pas être activés ou désactivés en cliquant sur Surface Dial**

Certains modes d’application/d’outil peuvent entrer en conflit avec, ou désactiver, les interactions s’appuyant sur la rotation. Des outils tels que la règle dans la barre d’outils Windows Ink, doivent être activés ou désactivés par le biais d’autres affordances de l’interface utilisateur (la barre d’outils d’entrée manuscrite fournit un contrôle [**ToggleButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.ToggleButton) intégré).

Pour les outils de mode, mappez l’élément de menu Surface Dial actif à l’outil cible ou à l’élément de menu précédemment sélectionné.

#### <a name="developer-guidance"></a>Conseils aux développeurs

Lorsqu’un clic est effectué sur Surface Dial, un événement [**RadialController.ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked) est déclenché. Les [**RadialControllerButtonClickedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs) incluent une propriété [**Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs.Contact) qui contient l’emplacement et la surface englobante de contact Surface Dial sur l’écran Surface Studio. Si Surface Dial n’est pas en contact avec l’écran, cette propriété est null. 

### <a name="on-screen"></a>À l’écran

Comme indiqué précédemment, Surface Dial peut être utilisé en association avec Surface Studio pour afficher le menu Surface Dial dans un mode spécial à l’écran. 

Lorsque ce mode est actif, vous pouvez intégrer et personnaliser plus avant vos expériences d’interaction entre Surface Dial et vos applications. Exemples d’expériences uniques seulement possibles avec Surface Dial et Surface Studio:
- Affichage d’outils contextuels (par exemple, une palette de couleurs) basé sur la position de Surface Dial, qui facilite la recherche et l’utilisation des outils
- Définition de l’outil actif en fonction de l’interface utilisateur sur laquelle Surface Dial est placé
- Agrandissement d’une zone de l’écran en fonction de l’emplacement de Surface Dial
- Interactions de jeu uniques en fonction de l’emplacement à l’écran

#### <a name="ux-guidance"></a>Recommandations en matière d’expérience utilisateur

**Les applications doivent répondre lorsque Surface Dial est détecté à l’écran**

Le retour visuel permet d’indiquer aux utilisateurs que votre application a détecté l’appareil sur l’écran de Surface Studio.

**Ajuster l’interface utilisateur liée à Surface Dial en fonction de l’emplacement de l’appareil**

L’appareil (et le corps de l’utilisateur) peut masquer l’interface utilisateur critique en fonction de l’endroit où l’utilisateur le place.

**Ajuster l’interface utilisateur liée à Surface Dial en fonction de l’interaction utilisateur**

Outre l’occlusion matérielle, la main et le bras d’un utilisateur peuvent masquer une partie de l’écran pendant l’utilisation de l’appareil. 

La zone masquée dépend de la main utilisée avec l’appareil. Comme l’appareil est conçu pour être utilisé principalement avec la main non dominante, l’interface utilisateur liée à Surface Dial doit s’ajuster pour la main opposée spécifiée par l’utilisateur (**Paramètres Windows &gt; Appareils &gt; Stylet et Windows Ink &gt; Indiquer avec quelle main vous écrivez**).

**Les interactions doivent répondre à la position de Surface Dial plutôt qu’au mouvement**

Le pied de l’appareil est conçu pour coller à l’écran plutôt que pour glisser dessus, car il ne s’agit pas d’un dispositif de pointage de précision. Par conséquent, nous pensons que les utilisateurs auront davantage tendance à soulever et à placer Surface Dial, plutôt qu’à le faire glisser sur l’écran.

**Utiliser la position sur l’écran pour déterminer l’intention de l’utilisateur**

Définir l’outil actif en fonction du contexte de l’interface utilisateur, notamment la proximité d’un contrôle, d’une zone de dessin ou d’une fenêtre, peut améliorer l’expérience utilisateur en réduisant les étapes nécessaires pour effectuer une tâche.

#### <a name="developer-guidance"></a>Conseils aux développeurs

Lorsque Surface Dial est placé sur la surface d’un numériseur de Surface Studio, un événement [**RadialController.ScreenContactStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ScreenContactStarted) se déclenche, et les coordonnées ([**RadialControllerScreenContactStartedEventArgs.Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs.Contact)) sont fournies à votre application.

De même, si un clic est effectué sur Surface Dial lorsqu’il est en contact avec la surface d’un numériseur de Surface Studio, un événement [**RadialController.ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked) se déclenche, et les coordonnées ([**RadialControllerButtonClickedEventArgs.Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs.Contact)) sont fournies à votre application. 

Les coordonnées ([**RadialControllerScreenContact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact)) comprennent les coordonnées X/Y du centre de Surface Dial dans l’espace de coordonnées de l’application ([**RadialControllerScreenContact.Position**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact.Position)), ainsi que le rectangle englobant ([**RadialControllerScreenContact.Bounds**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact.Bounds)) en DIP (pixels indépendants des appareils). Ces informations sont très utiles pour l’envoi de contexte à l’outil actif et la fourniture d’un retour visuel lié à l’appareil à l’utilisateur.

Dans l’exemple suivant, nous avons créé une application de base avec quatre sections différentes. Chacune d’entre elles comprend un curseur et un bouton bascule. Ensuite, nous utilisons la position sur l’écran de Surface Dial pour déterminer la façon dont l’ensemble de curseurs et boutons bascule est contrôlé par Surface Dial.

1. Tout d’abord, nous déclarons notre interface utilisateur (quatre sections, chacune avec un curseur et un bouton bascule) en XAML.

   ![Image de l’interface utilisateur de l’exemple d’application](images/windows-wheel/surface-dial-snippet-customtool3.png)  
   *L’interface utilisateur de l’exemple d’application*

   ```xaml 
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
  <Grid.RowDefinitions>
    <RowDefinition Height="Auto"/>
    <RowDefinition Height="*"/>
  </Grid.RowDefinitions>
  <StackPanel x:Name="HeaderPanel" 
        Orientation="Horizontal" 
        Grid.Row="0">
    <TextBlock x:Name="Header"
      Text="RadialController customization sample"
      VerticalAlignment="Center"
      Style="{ThemeResource HeaderTextBlockStyle}"
      Margin="10,0,0,0" />
  </StackPanel>
  <Grid Grid.Row="1" x:Name="RootGrid">
    <Grid.RowDefinitions>
      <RowDefinition Height="*"/>
      <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
      <ColumnDefinition Width="*"/>
      <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <Grid x:Name="Grid0"
      Grid.Row="0"
      Grid.Column="0">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider0"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle0"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid1"
      Grid.Row="0"
      Grid.Column="1">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider1"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle1"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid2"
      Grid.Row="1"
      Grid.Column="0">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider2"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle2"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid3"
      Grid.Row="1"
      Grid.Column="1">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider3"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle3"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
  </Grid>
</Grid>
   ```

2. Voici le code-behind avec des gestionnaires définis pour la position sur l’écran de Surface Dial.

```csharp
Slider ActiveSlider;
ToggleSwitch ActiveSwitch;
Grid ActiveGrid;

public MainPage()
{
  ...

  myController.ScreenContactStarted += 
    MyController_ScreenContactStarted;
  myController.ScreenContactContinued += 
    MyController_ScreenContactContinued;
  myController.ScreenContactEnded += 
    MyController_ScreenContactEnded;
  myController.ControlLost += MyController_ControlLost;

  //Set initial grid for Surface Dial input.
  ActiveGrid = Grid0;
  ActiveSlider = RotationSlider0;
  ActiveSwitch = ButtonToggle0;
}

private void MyController_ScreenContactStarted(RadialController sender, 
  RadialControllerScreenContactStartedEventArgs args)
{
  //find grid at contact location, update visuals, selection
  ActivateGridAtLocation(args.Contact.Position);
}

private void MyController_ScreenContactContinued(RadialController sender, 
  RadialControllerScreenContactContinuedEventArgs args)
{
  //if a new grid is under contact location, update visuals, selection
  if (!VisualTreeHelper.FindElementsInHostCoordinates(
    args.Contact.Position, RootGrid).Contains(ActiveGrid))
  {
    ActiveGrid.Background = new 
      SolidColorBrush(Windows.UI.Colors.White);
    ActivateGridAtLocation(args.Contact.Position);
  }
}

private void MyController_ScreenContactEnded(RadialController sender, object args)
{
  //return grid color to normal when contact leaves screen
  ActiveGrid.Background = new 
  SolidColorBrush(Windows.UI.Colors.White);
}

private void MyController_ControlLost(RadialController sender, object args)
{
  //return grid color to normal when focus lost
  ActiveGrid.Background = new 
    SolidColorBrush(Windows.UI.Colors.White);
}

private void ActivateGridAtLocation(Point Location)
{
  var elementsAtContactLocation = 
    VisualTreeHelper.FindElementsInHostCoordinates(Location, 
      RootGrid);

  foreach (UIElement element in elementsAtContactLocation)
  {
    if (element as Grid == Grid0)
    {
      ActiveSlider = RotationSlider0;
      ActiveSwitch = ButtonToggle0;
      ActiveGrid = Grid0;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid1)
    {
      ActiveSlider = RotationSlider1;
      ActiveSwitch = ButtonToggle1;
      ActiveGrid = Grid1;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid2)
    {
      ActiveSlider = RotationSlider2;
      ActiveSwitch = ButtonToggle2;
      ActiveGrid = Grid2;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid3)
    {
      ActiveSlider = RotationSlider3;
      ActiveSwitch = ButtonToggle3;
      ActiveGrid = Grid3;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
  }
}
```

Lorsque nous exécutons l’application, nous utilisons Surface Dial pour interagir avec celle-ci. Tout d’abord, nous plaçons l’appareil sur l’écran de Surface Studio. L’application le détecte et l’associe à la section inférieure droite (voir image). Ensuite, nous appuyons de manière prolongée sur Surface Dial pour ouvrir le menu et sélectionner notre outil personnalisé. Une fois que l’outil personnalisé est activé, le contrôle du curseur peut être ajusté en tournant Surface Dial, et le commutateur peut être basculé en cliquant sur Surface Dial.

![Image de l’interface utilisateur de l’exemple d’application activée à l’aide de l’outil personnalisé de Surface Dial](images/windows-wheel/surface-dial-snippet-customtool4.png)  
*L’interface utilisateur de l’exemple d’application activée à l’aide de l’outil personnalisé de Surface Dial*

## <a name="summary"></a>Résumé

Cette rubrique fournit une vue d’ensemble de l’appareil d’entrée Surface Dial, assortie de recommandations en matière d’expérience utilisateur et de conseils aux développeurs sur la manière de personnaliser l’expérience utilisateur pour des scénarios hors écran et sur l’écran, lors de l’utilisation avec Surface Studio.

## <a name="feedback"></a>Commentaires

Veuillez envoyer vos questions, suggestions et commentaires à l'adresse [radialcontroller@microsoft.com](mailto:radialcontroller@microsoft.com).

## <a name="related-articles"></a>Articles connexes

### <a name="api-reference"></a>Informations de référence sur les API

- [Classe **RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController)
- [Classe **RadialControllerButtonClickedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs)
- [Classe **RadialControllerConfiguration**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerConfiguration) 
- [Classe **RadialControllerControlAcquiredEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerControlAcquiredEventArgs) 
- [Classe **RadialControllerMenu**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenu) 
- [Classe **RadialControllerMenuItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuItem) 
- [Classe **RadialControllerRotationChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerRotationChangedEventArgs) 
- [Classe **RadialControllerScreenContact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact) 
- [Classe **RadialControllerScreenContactContinuedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactContinuedEventArgs) 
- [Classe **RadialControllerScreenContactStartedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs)
- [Énumération **RadialControllerMenuKnownIcon**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuKnownIcon) 
- [Énumération **RadialControllerSystemMenuItemKind**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerSystemMenuItemKind) 

### <a name="samples"></a>Exemples

[Exemples de la plateforme Windows universelle (C# et C++)](https://go.microsoft.com/fwlink/?linkid=832713)

[Exemple du bureau classique Windows](https://aka.ms/radialcontrollerclassicsample)