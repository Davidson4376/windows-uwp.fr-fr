---
ms.assetid: ''
title: Prise en charge de Surface Dial (et autres périphériques à molette) dans votre application UWP
description: Un didacticiel détaillé qui explique comment ajouter la prise en charge de Surface Dial (et autres périphériques à molette) à votre application UWP.
keywords: radial, dial, didacticiel
ms.date: 01/25/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d8729826c2f372b3d3b5607ce828aaf515e47f3d
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8933910"
---
# <a name="tutorial-support-the-surface-dial-and-other-wheel-devices-in-your-uwp-app"></a>Didacticiel: Prise en charge de Surface Dial (et autres périphériques à molette) dans votre application UWP

![Image de Surface Dial avec Surface Studio](images/radialcontroller/dial-pen-studio-600px.png)  
*Surface Dial avec Surface Studio et stylet Surface* (disponible à l’achat auprès de la [Boutique Microsoft](https://aka.ms/purchasesurfacedial)).

Ce didacticiel décrit comment personnaliser les expériences d’interaction utilisateur prises en charge par les périphériques à molette tels que Surface Dial. Nous utilisons des extraits de code à partir d’un exemple d’application, que vous pouvez télécharger à partir de GitHub (voir [Exemple de code](#sample-code)), pour illustrer les différentes fonctionnalités et les API [**RadialController**](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) associées dans chaque étape.

Nous nous concentrons sur les éléments suivants:
* Spécification des outils intégrés qui figurent dans le menu [**RadialController**](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller)
* Ajout d’un outil personnalisé au menu
* Contrôle du retour haptique
* Personnalisation des interactions par clic
* Personnalisation des interactions de rotation

Pour plus d’informations sur l’implémentation de ces fonctionnalités (ou d'autres), voir [Interactions avec Surface Dial dans les applications UWP](windows-wheel-interactions.md).

## <a name="introduction"></a>Introduction

Surface Dial est un périphérique d’entrée secondaire qui permet aux utilisateurs d’être plus productifs lorsqu’ils l'utilisent avec un périphérique d’entrée principal tel qu'un stylet, des fonctions tactiles ou la souris. En tant que périphérique d’entrée secondaire, Surface Dial est généralement utilisé avec la main non dominante pour fournir l’accès à la fois aux commandes système et aux autres outils et fonctionnalités, plus contextuels. 

Surface Dial prend en charge trois mouvements de base: 
- Appuyez longuement pour afficher le menu de commandes intégré.
- Faire pivoter pour mettre en surbrillance un élément de menu (si le menu est actif) ou pour modifier l’action en cours dans l’application (si le menu n’est pas actif).
- Cliquez pour sélectionner l’élément de menu en surbrillance (si le menu est actif) ou pour invoquer une commande dans l’application (si le menu n’est pas actif).

## <a name="prerequisites"></a>Prérequis

* Un ordinateur (ou une machine virtuelle) exécutant Windows10Creators Update ou une version ultérieure
* [Visual Studio 2017 (10.0.15063.0)](https://developer.microsoft.com/windows/downloads)
* [Windows 10 SDK (10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Un appareil à molette (uniquement les [Surface Dial](https://aka.ms/purchasesurfacedial) pour l’instant)
* Si vous débutez dans le développement d’applications de plateforme Windows universelle (UWP) avec Visual Studio, consultez les rubriques suivantes avant de démarrer ce didacticiel:  
    * [Préparation](https://docs.microsoft.com/windows/uwp/get-started/get-set-up)
    * [Créer une application «Hello World» (XAML)](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)

## <a name="set-up-your-devices"></a>Configurer vos périphériques

1. Vérifier que votre appareil Windows est activé.
2. Accédez à **Démarrer**, sélectionnez **Paramètres** > **Périphériques** > **Bluetooth et autres périphériques**, puis activez l’option **Bluetooth**.
3. Retirez la partie inférieure de Surface Dial pour ouvrir le compartiment à piles et assurez-vous qu’il y a bien deux piles AAA à l’intérieur.
4. Si l’onglet de la batterie se trouve sur la partie inférieure du Surface Dial, retirez-le.
5. Maintenez le petit bouton incrusté en regard de la batterie enfoncé jusqu'à ce que le voyant du Bluetooth clignote.
6. De nouveau sur votre appareil Windows, sélectionnez **Ajouter un périphérique Bluetooth ou autre**.
7. Dans la boîte de dialogue **Ajouter un périphérique**, sélectionnez **Bluetooth** > **Surface Dial**. Votre Surface Dial doit maintenant se connecter et être ajouté à la liste des périphériques sous **Souris, clavier et stylet** sur la page des paramètres **Bluetooth et autres périphériques**.
8. Testez Surface Dial en appuyant longuement dessus pendant quelques secondes pour afficher le menu intégré.
9. Si le menu n’est pas affiché sur votre écran (surface Dial devrait également vibrer), accédez aux paramètres Bluetooth, supprimez l’appareil, puis réessayez de vous connecter l’appareil.

> [!NOTE]
> Les appareils à molette peuvent être configurés via les paramètres **Molette**:
> 1. Dans le menu **Démarrer**, sélectionnez **Paramètres**.
> 2. Sélectionnez **Périphériques** > **Molette**.    
> ![Écran des paramètres de la molette](images/radialcontroller/wheel-settings.png)

Vous êtes maintenant prêt à commencer ce didacticiel. 

## <a name="sample-code"></a>Exemple de code
Tout au long de ce didacticiel, nous utilisons un exemple d’application pour illustrer les concepts et les fonctionnalités décrites.

Téléchargez cet exemple Visual Studio et ce code source à partir de [GitHub](https://github.com/) à [windows-appsample-get-started-radialcontroller sample](https://aka.ms/appsample-radialcontroller):

1. Sélectionnez le bouton vert **Clonage ou téléchargement**.  
![Clonage du référentiel](images/radialcontroller/wheel-clone.png)
2. Si vous avez un compte GitHub, vous pouvez cloner le référentiel sur votre ordinateur local en sélectionnant **Ouvrir dans Visual Studio**. 
3. Si vous n’avez pas de compte GitHub, ou si vous souhaitez simplement obtenir une copie locale du projet, sélectionnez **Télécharger le fichier ZIP** (vous devrez régulièrement vous assurer de télécharger les dernières mises à jour).

> [!IMPORTANT]
> La plupart du code de l’exemple est commenté. À mesure que vous avancerez dans cette rubrique, vous serez invité à supprimer les commentaires des diverses sections du code. Dans Visual Studio, mettez simplement en surbrillance les lignes de code et appuyez sur CTRL-K, puis sur CTRL-U.

## <a name="components-that-support-wheel-functionality"></a>Composants prenant en charge la fonctionnalité de molette

Ces objets fournissent la majeure partie de l’expérience d’appareil à molette pour les applications UWP.

| Composant | Description |
| --- | --- |
| [Catégorie **RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) et rubriques connexes | Représente un périphérique de saisie à molette ou un accessoire tel que Surface Dial. |
| [**IRadialControllerConfigurationInterop**](https://msdn.microsoft.com/library/windows/desktop/mt790709) / [**IRadialControllerInterop**](https://msdn.microsoft.com/library/windows/desktop/mt790711)<br/>Nous n'abordons pas cette fonctionnalité ici; pour plus d’informations, voir l'[Exemple du bureau classique Windows](https://aka.ms/radialcontrollerclassicsample). | Permet l’interopérabilité avec une application UWP. |

## <a name="step-1-run-the-sample"></a>Étape1: exécution de l’exemple

Une fois que vous avez téléchargé l’exemple d’application RadialController, vérifiez qu’elle s’exécute:
1. Ouvrez l’exemple de projet dans Visual Studio.
2. Définissez la liste déroulante **Plateformes de solution** sur une sélection non ARM.
3. Appuyez sur F5 pour compiler, déployer et exécuter. 

> [!NOTE]
> Vous pouvez également sélectionner l'élément de menu **Déboguer** > **Démarrer le débogage**, ou sélectionnez le bouton d'exécution sur **Ordinateur local** illustré ici: ![bouton de projet Générer sur Visual Studio](images/radialcontroller/wheel-vsrun.png)

Ouvre la fenêtre d’application, et une fois qu'un écran de démarrage se sera affiché pendant quelques secondes, vous verrez cet écran initial.

![Application vide](images/radialcontroller/wheel-app-step1-empty.png)

OK, nous vous proposons désormais l’application UWP de base que nous allons utiliser tout au long de ce didacticiel. Dans les étapes suivantes, nous ajoutons notre fonctionnalité **RadialController**.

## <a name="step-2-basic-radialcontroller-functionality"></a>Étape2: Fonctionnalités de base RadialController

Exécutez l’application au premier plan et maintenez la touche Surface Dial enfoncée pour afficher le menu ** RadialController **.

Nous n’avons pas encore personnalisé notre application pour le moment, par conséquent le menu contient un ensemble d'outils contextuels par défaut. 

Ces images montrent deux variantes du menu par défaut. (Il en existe beaucoup d’autres, y compris des outils système de base uniquement lorsque le bureau Windows est actif et qu'aucune application n'est exécutée au premier plan, des outils d'entrée manuscrite supplémentaires lorsqu’un contrôle InkToolbar est disponible et des outils de mappage lorsque vous utilisez l’application Cartes.

| Menu RadialController (par défaut)  | Menu RadialController (par défaut avec lecture du support)  |
|---|---|
| ![Menu RadialController par défaut](images/radialcontroller/wheel-app-step2-basic-default.png) | ![Menu RadialController par défaut avec de la musique](images/radialcontroller/wheel-app-step2-basic-withmusic.png) |

Nous allons maintenant commencer par une personnalisation de base.

## <a name="step-3-add-controls-for-wheel-input"></a>Étape3: Ajouter des contrôles pour les entrées avec molette

Tout d’abord, nous allons ajouter l’interface utilisateur de notre application:

1. Ouvrez le fichier MainPage_Basic.xaml.
2. Recherchez le code marqué avec le titre de cette étape («\<!--Étape3: Ajouter des contrôles pour les entrées avec molette -->»).
3. Supprimez les commentaires des lignes suivantes.

    ```xaml
    <Button x:Name="InitializeSampleButton" 
            HorizontalAlignment="Center" 
            Margin="10" 
            Content="Initialize sample" />
    <ToggleButton x:Name="AddRemoveToggleButton"
                    HorizontalAlignment="Center" 
                    Margin="10" 
                    Content="Remove Item"
                    IsChecked="True" 
                    IsEnabled="False"/>
    <Button x:Name="ResetControllerButton" 
            HorizontalAlignment="Center" 
            Margin="10" 
            Content="Reset RadialController menu" 
            IsEnabled="False"/>
    <Slider x:Name="RotationSlider" Minimum="0" Maximum="10"
            Width="300"
            HorizontalAlignment="Center"/>
    <TextBlock Text="{Binding ElementName=RotationSlider, Mode=OneWay, Path=Value}"
                Margin="0,0,0,20"
                HorizontalAlignment="Center"/>
    <ToggleSwitch x:Name="ClickToggle"
                    MinWidth="0" 
                    Margin="0,0,0,20"
                    HorizontalAlignment="center"/>
    ```
À ce stade, seuls le bouton, le curseur et le bouton bascule **Initialiser l'exemple** sont activés. Les autres boutons sont utilisés dans les étapes ultérieures pour ajouter et supprimer les éléments de menu **RadialController** qui permettent d’accéder au curseur et au bouton bascule.

![Interface utilisateur de l’exemple d’application](images/radialcontroller/wheel-app-step3-basicui.png)

## <a name="step-4-customize-the-basic-radialcontroller-menu"></a>Étape4: Personnaliser le menu RadialController de base

Maintenant, nous allons ajouter le code requis pour activer l'accès **RadialController** à nos contrôles.

1. Ouvrez le fichier MainPage_Basic.xaml.cs.
2. Recherchez le code marqué avec le titre de cette étape («//Étape4: Personnalisation du menu de base RadialController»).
3. Supprimez les commentaires des lignes suivantes:
    - Les références de type [Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input) et [Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) sont utilisées pour la fonctionnalité dans les étapes suivantes:  
    
        ```csharp
        // Using directives for RadialController functionality.
        using Windows.UI.Input;
        ```

    - Ces objets globaux ([RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller), [RadialControllerConfiguration](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration), [RadialControllerMenuItem](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem)) sont utilisés dans l’ensemble de notre application.
    
        ```csharp
        private RadialController radialController;
        private RadialControllerConfiguration radialControllerConfig;
        private RadialControllerMenuItem radialControllerMenuItem;
        ```

    - Ici, nous spécifions le gestionnaire **Clic** pour le bouton qui active nos contrôles et initialise notre élément de menu **RadialController** personnalisé.

        ```csharp
        InitializeSampleButton.Click += (sender, args) =>
        { InitializeSample(sender, args); };
        ``` 

    - Ensuite, nous initialisons notre objet [RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) et configurons des gestionnaires pour les événements [RotationChanged](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationChanged) et [ButtonClicked](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ButtonClicked).

        ```csharp
        // Set up the app UI and RadialController.
        private void InitializeSample(object sender, RoutedEventArgs e)
        {
            ResetControllerButton.IsEnabled = true;
            AddRemoveToggleButton.IsEnabled = true;

            ResetControllerButton.Click += (resetsender, args) =>
            { ResetController(resetsender, args); };
            AddRemoveToggleButton.Click += (togglesender, args) =>
            { AddRemoveItem(togglesender, args); };

            InitializeController(sender, e);
        }
        ```

    - Nous allons maintenant initialiser notre élément de menu RadialController personnalisé. Nous utilisons [CreateForCurrentView](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.CreateForCurrentView) pour obtenir une référence à notre objet [RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller), nous définissons la sensibilité à la rotation sur «1» à l’aide de la propriété [RotationResolutionInDegrees](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationResolutionInDegrees), nous créons nos [RadialControllerMenuItem](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem) à l’aide de [CreateFromFontGlyph](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem.CreateFromFontGlyph), nous ajoutons l’élément de menu à la collection d’éléments de menu **RadialController** et enfin, nous utilisons [SetDefaultMenuItems](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration.setdefaultmenuitems) pour effacer les éléments de menu par défaut et conserver uniquement notre outil personnalisé. 

        ```csharp
        // Configure RadialController menu and custom tool.
        private void InitializeController(object sender, RoutedEventArgs args)
        {
            // Create a reference to the RadialController.
            radialController = RadialController.CreateForCurrentView();
            // Set rotation resolution to 1 degree of sensitivity.
            radialController.RotationResolutionInDegrees = 1;

            // Create the custom menu items.
            // Here, we use a font glyph for our custom tool.
            radialControllerMenuItem =
                RadialControllerMenuItem.CreateFromFontGlyph("SampleTool", "\xE1E3", "Segoe MDL2 Assets");

            // Add the item to the RadialController menu.
            radialController.Menu.Items.Add(radialControllerMenuItem);

            // Remove built-in tools to declutter the menu.
            // NOTE: The Surface Dial menu must have at least one menu item. 
            // If all built-in tools are removed before you add a custom 
            // tool, the default tools are restored and your tool is appended 
            // to the default collection.
            radialControllerConfig =
                RadialControllerConfiguration.GetForCurrentView();
            radialControllerConfig.SetDefaultMenuItems(
                new RadialControllerSystemMenuItemKind[] { });

            // Declare input handlers for the RadialController.
            // NOTE: These events are only fired when a custom tool is active.
            radialController.ButtonClicked += (clicksender, clickargs) =>
            { RadialController_ButtonClicked(clicksender, clickargs); };
            radialController.RotationChanged += (rotationsender, rotationargs) =>
            { RadialController_RotationChanged(rotationsender, rotationargs); };
        }

        // Connect wheel device rotation to slider control.
        private void RadialController_RotationChanged(
            object sender, RadialControllerRotationChangedEventArgs args)
        {
            if (RotationSlider.Value + args.RotationDeltaInDegrees >= RotationSlider.Maximum)
            {
                RotationSlider.Value = RotationSlider.Maximum;
            }
            else if (RotationSlider.Value + args.RotationDeltaInDegrees < RotationSlider.Minimum)
            {
                RotationSlider.Value = RotationSlider.Minimum;
            }
            else
            {
                RotationSlider.Value += args.RotationDeltaInDegrees;
            }
        }

        // Connect wheel device click to toggle switch control.
        private void RadialController_ButtonClicked(
            object sender, RadialControllerButtonClickedEventArgs args)
        {
            ClickToggle.IsOn = !ClickToggle.IsOn;
        }
        ```
4. Exécutez de nouveau l’application.
5. Sélectionnez le bouton**Initialiser le contrôleur radial**.  
6. Exécutez l’application au premier plan et maintenez la touche Surface Dial enfoncée pour afficher le menu. Notez que tous les outils par défaut ont été supprimés (à l’aide de la méthode **RadialControllerConfiguration.SetDefaultMenuItems**) et seul reste l’outil personnalisé. Voici le menu avec notre outil personnalisé. 

| Menu RadialController (personnalisé)  | 
|---|
| ![Menu RadialController personnalisé](images/radialcontroller/wheel-app-step3-custom.png) |

7. Sélectionnez l’outil personnalisé et testez les interactions désormais prises en charge par le biais de Surface Dial:
    * Une action de rotation déplace le curseur. 
    * Un clic définit le bouton bascule sur la position activée ou désactivée.

OK, nous allons raccorder ces boutons.

## <a name="step-5-configure-menu-at-runtime"></a>Étape5: Configurer le menu lors de l’exécution

Dans cette étape, nous raccordons l'**élément Ajouter/Supprimer** et les boutons du **menu Réinitialiser RadialController** pour vous montrer comment vous pouvez personnaliser dynamiquement le menu.

1. Ouvrez le fichier MainPage_Basic.xaml.cs.
2. Recherchez le code marqué avec le titre de cette étape («//Étape5: Configurer le menu lors de l’exécution»).
3. Supprimez les commentaires du code dans les méthodes suivantes et réexécutez l’application, mais ne sélectionnez pas les boutons (vous ferez cela lors de l’étape suivante).  

    ``` csharp
    // Add or remove the custom tool.
    private void AddRemoveItem(object sender, RoutedEventArgs args)
    {
        if (AddRemoveToggleButton?.IsChecked == true)
        {
            AddRemoveToggleButton.Content = "Remove item";
            if (!radialController.Menu.Items.Contains(radialControllerMenuItem))
            {
                radialController.Menu.Items.Add(radialControllerMenuItem);
            }
        }
        else if (AddRemoveToggleButton?.IsChecked == false)
        {
            AddRemoveToggleButton.Content = "Add item";
            if (radialController.Menu.Items.Contains(radialControllerMenuItem))
            {
                radialController.Menu.Items.Remove(radialControllerMenuItem);
                // Attempts to select and activate the previously selected tool.
                // NOTE: Does not differentiate between built-in and custom tools.
                radialController.Menu.TrySelectPreviouslySelectedMenuItem();
            }
        }
    }

    // Reset the RadialController to initial state.
    private void ResetController(object sender, RoutedEventArgs arg)
    {
        if (!radialController.Menu.Items.Contains(radialControllerMenuItem))
        {
            radialController.Menu.Items.Add(radialControllerMenuItem);
        }
        AddRemoveToggleButton.Content = "Remove item";
        AddRemoveToggleButton.IsChecked = true;
        radialControllerConfig.SetDefaultMenuItems(
            new RadialControllerSystemMenuItemKind[] { });
    }
    ```
4. Sélectionnez le bouton **Supprimer cet élément** et appuyez longuement sur Surface Dial pour afficher le menu à nouveau.

    Notez que le menu contient désormais la collection d'outils par défaut. N’oubliez pas que, à l’étape3, lors de la configuration de notre menu personnalisé, nous avons supprimé tous les outils par défaut et ajouté simplement notre outil personnalisé. Nous avons également remarqué que, lorsque le menu est défini sur une collection vide, les éléments par défaut pour le contexte actuel sont rétablis. (Nous avons ajouté notre outil personnalisé avant de supprimer les outils par défaut).

5. Sélectionnez le bouton **Ajouter un élément** et appuyez longuement sur Surface Dial.

    Notez que le menu contient désormais la collection par défaut des outils et notre outil personnalisé.

6. Sélectionnez le bouton du **menu Réinitialiser RadialController** et appuyez longuement sur Surface Dial.

    Notez que le menu retrouve son état d’origine.

## <a name="step-6-customize-the-device-haptics"></a>Étape6: Personnaliser le retour haptique du périphérique
Surface Dial, et les autres périphériques à molette, peuvent fournir aux utilisateurs un retour haptique correspondant à l’interaction en cours (basée sur les actions de clic ou de rotation).

Dans cette étape, nous vous montrons comment vous pouvez personnaliser le retour haptique en associant nos contrôles de curseur et de bouton bascule et en les utilisant pour spécifier le comportement de retour haptique de manière dynamique. Pour cet exemple, le bouton bascule doit être défini sur la position activée pour activer le retour, tandis que la valeur du curseur spécifiera la fréquence à laquelle le retour de clic est répété. 

> [!NOTE]
> Le retour haptique peut être désactivé par l’utilisateur à la page  **Paramètres** >  **Périphériques** > **Molette**.

1. Ouvrez le fichier App.xaml.cs
2. Recherchez le code marqué avec le titre de cette étape («Étape6: personnaliser le retour haptique de l'appareil»).
3. Commenter les première et troisième lignes («MainPage_Basic» et «MainPage») et supprimez les commentaires de la deuxième ligne («MainPage_Haptics»).  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
4. Ouvrez le fichier MainPage_Haptics.xaml.
5. Recherchez le code marqué avec le titre de cette étape («\<!-- Étape6: personnaliser le retour haptique de l'appareil -->»).
6. Supprimez les commentaires des lignes suivantes. (Ce code d’interface utilisateur indique simplement quelles fonctionnalités de retour haptique sont prises en charge par l’appareil actuel).    

    ```xaml
    <StackPanel x:Name="HapticsStack" 
                Orientation="Vertical" 
                HorizontalAlignment="Center" 
                BorderBrush="Gray" 
                BorderThickness="1">
        <TextBlock Padding="10" 
                    Text="Supported haptics properties:" />
        <CheckBox x:Name="CBDefault" 
                    Content="Default" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsChecked="True" />
        <CheckBox x:Name="CBIntensity" 
                    Content="Intensity" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPlayCount" 
                    Content="Play count" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPlayDuration" 
                    Content="Play duration" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBReplayPauseInterval" 
                    Content="Replay/pause interval" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBBuzzContinuous" 
                    Content="Buzz continuous" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBClick" 
                    Content="Click" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPress" 
                    Content="Press" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBRelease" 
                    Content="Release" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBRumbleContinuous" 
                    Content="Rumble continuous" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
    </StackPanel>
    ```
7. Ouvrez le fichier MainPage_Haptics.xaml.cs
8. Recherchez le code marqué avec le titre de cette étape («Étape6: Personnalisation de la fonction de retour haptique»).
9. Supprimez les commentaires des lignes suivantes:  

    - La référence de type [Windows.Devices.Haptics](https://docs.microsoft.com/uwp/api/windows.devices.haptics) est utilisée pour la fonctionnalité dans les étapes suivantes.  
    
        ```csharp
        using Windows.Devices.Haptics;
        ```

    - Ici, nous spécifions le gestionnaire pour l'événement [ControlAcquired](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ControlAcquired) qui est déclenché lorsque notre élément de menu **RadialController** est sélectionné.

        ```csharp
        radialController.ControlAcquired += (rc_sender, args) =>
        { RadialController_ControlAcquired(rc_sender, args); };
        ``` 

    - Ensuite, nous définissons le gestionnaire [ControlAcquired](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ControlAcquired), où nous désactivons le retour haptique par défaut et initialisons notre interface utilisateur de retour haptique.

        ```csharp
        private void RadialController_ControlAcquired(
            RadialController rc_sender,
            RadialControllerControlAcquiredEventArgs args)
        {
            // Turn off default haptic feedback.
            radialController.UseAutomaticHapticFeedback = false;

            SimpleHapticsController hapticsController =
                args.SimpleHapticsController;

            // Enumerate haptic support.
            IReadOnlyCollection<SimpleHapticsControllerFeedback> supportedFeedback =
                hapticsController.SupportedFeedback;

            foreach (SimpleHapticsControllerFeedback feedback in supportedFeedback)
            {
                if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.BuzzContinuous)
                {
                    CBBuzzContinuous.IsEnabled = true;
                    CBBuzzContinuous.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Click)
                {
                    CBClick.IsEnabled = true;
                    CBClick.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Press)
                {
                    CBPress.IsEnabled = true;
                    CBPress.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Release)
                {
                    CBRelease.IsEnabled = true;
                    CBRelease.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.RumbleContinuous)
                {
                    CBRumbleContinuous.IsEnabled = true;
                    CBRumbleContinuous.IsChecked = true;
                }
            }

            if (hapticsController?.IsIntensitySupported == true)
            {
                CBIntensity.IsEnabled = true;
                CBIntensity.IsChecked = true;
            }
            if (hapticsController?.IsPlayCountSupported == true)
            {
                CBPlayCount.IsEnabled = true;
                CBPlayCount.IsChecked = true;
            }
            if (hapticsController?.IsPlayDurationSupported == true)
            {
                CBPlayDuration.IsEnabled = true;
                CBPlayDuration.IsChecked = true;
            }
            if (hapticsController?.IsReplayPauseIntervalSupported == true)
            {
                CBReplayPauseInterval.IsEnabled = true;
                CBReplayPauseInterval.IsChecked = true;
            }
        }
        ```

    - Dans nos gestionnaires d'événement [RotationChanged](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationChanged) et [ButtonClicked](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ButtonClicked), nous connectons les contrôles de bouton curseur et bascule correspondants à notre retour haptique personnalisé. 

        ```csharp
        // Connect wheel device rotation to slider control.
        private void RadialController_RotationChanged(
            object sender, RadialControllerRotationChangedEventArgs args)
        {
            ...
            if (ClickToggle.IsOn && 
                (RotationSlider.Value > RotationSlider.Minimum) && 
                (RotationSlider.Value < RotationSlider.Maximum))
            {
                SimpleHapticsControllerFeedback waveform = 
                    FindWaveform(args.SimpleHapticsController, 
                    KnownSimpleHapticsControllerWaveforms.BuzzContinuous);
                if (waveform != null)
                {
                    args.SimpleHapticsController.SendHapticFeedback(waveform);
                }
            }
        }

        private void RadialController_ButtonClicked(
            object sender, RadialControllerButtonClickedEventArgs args)
        {
            ...

            if (RotationSlider?.Value > 0)
            {
                SimpleHapticsControllerFeedback waveform = 
                    FindWaveform(args.SimpleHapticsController, 
                    KnownSimpleHapticsControllerWaveforms.Click);

                if (waveform != null)
                {
                    args.SimpleHapticsController.SendHapticFeedbackForPlayCount(
                        waveform, 1.0, 
                        (int)RotationSlider.Value, 
                        TimeSpan.Parse("1"));
                }
            }
        }
        ```
    - Enfin, nous obtenons le **[Waveform](https://docs.microsoft.com/uwp/api/windows.devices.haptics.simplehapticscontrollerfeedback.Waveform)** demandé (si celui-ci est pris en charge) pour le retour haptique. 

        ```csharp
        // Get the requested waveform.
        private SimpleHapticsControllerFeedback FindWaveform(
            SimpleHapticsController hapticsController,
            ushort waveform)
        {
            foreach (var hapticInfo in hapticsController.SupportedFeedback)
            {
                if (hapticInfo.Waveform == waveform)
                {
                    return hapticInfo;
                }
            }
            return null;
        }
        ```

Exécutez maintenant l’application à nouveau pour tester le retour haptique personnalisé en modifiant la valeur du curseur et l'état du bouton bascule.

## <a name="step-7-define-on-screen-interactions-for-surface-studio-and-similar-devices"></a>Étape7: Définir les interactions à l’écran pour Surface Studio et périphériques similaires
Utilisé conjointement avec Surface Studio, Surface Dial peut fournir une expérience utilisateur encore plus originale. 

Outre l’expérience de menu d’appui prolongé par défaut décrite, Surface Dial peut également être placé directement sur l’écran de Surface Studio. Cela permet d’afficher un menu «à l’écran» spécial. 

Le système détecte à la l’emplacement de contact et les limites de Surface Dial pour traiter l’occlusion par l’appareil et afficher une version plus grande du menu encerclant la partie extérieure de Surface Dial. Ces mêmes informations peuvent également être utilisées par votre application pour adapter l’interface utilisateur à la présence de l’appareil et à son utilisation prévue, notamment au placement de la main et du bras de l’utilisateur. 

L’exemple associé à ce didacticiel comprend un exemple légèrement plus complexe qui illustre certaines de ces fonctionnalités.

Pour voir cela en action (vous aurez besoin d’un Surface Studio), procédez comme suit:

1. Téléchargez l’exemple sur un appareil Surface Studio (avec Visual Studio installé)
2. Ouvrez l’exemple dans Visual Studio
3. Ouvrez le fichier App.xaml.cs
4. Trouvez le code marqué avec le titre de cette étape («Étape7: Définir l'interaction à l’écran de Surface Studio et des périphériques similaires»)
5. Commenter les première et seconde lignes («MainPage_Basic» et «MainPage_Haptics») et supprimez les commentaires de la troisième ligne («MainPage_Haptics»).  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
6. Exécutez l’application et placez Surface Dial dans chacune des deux zones de contrôle, en alternant entre les deux.    
![RadialController à l'écran](images/radialcontroller/wheel-app-step5-onscreen2.png) 

    Voici une vidéo de cet exemple en action:  

    <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="600" height="400" allowFullScreen frameBorder="0"></iframe>  

## <a name="summary"></a>Résumé

Félicitations, vous avez terminé le *Didacticiel de prise en main: Prise en charge de Surface Dial (et autres périphériques à molette) dans votre application UWP*! Nous vous avons présenté le code de base requis pour prendre en charge un appareil à molette dans vos applications UWP et comment fournir une expérience utilisateur enrichie prise en charge par les API **RadialController**.
