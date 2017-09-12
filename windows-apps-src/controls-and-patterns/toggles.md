---
author: Jwmsft
Description: "Le bouton bascule représente un commutateur physique qui permet à l’utilisateur d’activer ou de désactiver des options."
title: "Recommandations en matière de boutons bascule"
ms.assetid: 753CFEA4-80D3-474C-B4A9-555F872A3DEF
label: Toggle switches
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.openlocfilehash: 7cc1d11035d072fdd52bdfa4a1d0c6e66926ba9f
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/22/2017
---
# <a name="toggle-switches"></a>Boutons bascule
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Le [bouton bascule](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.aspx) représente un commutateur physique qui permet à l’utilisateur d’activer ou de désactiver des options. Utilisez des contrôles ToggleSwitch pour présenter aux utilisateurs deux options qui s’excluent mutuellement (comme activé/désactivé), de sorte que la sélection d’une option produise immédiatement un résultat. 

Pour créer un contrôle de bouton bascule, utilisez la [classe ToggleSwitch](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.aspx).

> **API importantes**: [classe ToggleSwitch](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.aspx), [propriété IsOn](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.ison.aspx), [événement Toggled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.toggled.aspx)


## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Utilisez un bouton bascule pour les opérations binaires qui prennent effet dès que l’utilisateur change l’état du bouton.

![Bouton bascule WiFi, activé et désactivé](images/toggleswitches01.png)

Imaginez le bouton bascule comme le bouton marche/arrêt d’un appareil: vous l’actionnez quand vous souhaitez activer ou désactiver l’action de l’appareil.

Pour rendre le bouton bascule facile à comprendre, utilisez des étiquettes contenant un ou deux mots, de préférence des noms, qui décrivent la fonctionnalité contrôlée. Par exemple, «WiFi» ou «Lumières cuisine».  


### <a name="choosing-between-toggle-switch-and-check-box"></a>Choix entre le bouton bascule et la case à cocher

Le bouton bascule et la case à cocher peuvent tous deux convenir à certaines actions. Pour déterminer le contrôle le mieux approprié, suivez ces conseils :

-   Utilisez un bouton bascule pour les paramètres binaires impliquant des modifications qui deviennent immédiatement effectives une fois que l’utilisateur les a apportées.

    ![Bouton bascule ou case à cocher](images/toggleswitches02.png)

    Dans cet exemple, il est évident dans le cas du bouton bascule que la fonctionnalité sans fil est activée. Mais pour la case à cocher, l’utilisateur doit se demander si la fonctionnalité sans fil est en marche ou s’il doit activer la case à cocher pour la mettre en marche.

-   Utilisez les cases à cocher pour les éléments facultatifs («bonus»). 
-   Utilisez une case à cocher lorsque l’utilisateur doit réaliser des étapes supplémentaires afin de rendre effectives les modifications. Par exemple, si l’utilisateur doit cliquer sur un bouton «Envoyer» ou «Suivant» pour appliquer les modifications, utilisez une case à cocher.
-   Utilisez les cases à cocher lorsque l’utilisateur peut sélectionner plusieurs éléments liés à un paramètre ou une fonctionnalité unique. 

## <a name="toggle-switches-in-the-the-windows-ui"></a>Boutons bascule dans l’interface utilisateur Windows

Ces images montrent comment l’interface utilisateur Windows utilise les boutons bascule. Voici comment l’écran Paramètres Smart Storage utilise les boutons bascule:

![Boutons bascule dans Smart Storage](images/SmartStorageToggle.png)

Cet exemple est tiré de la page Paramètres d’éclairage nocturne:

![Boutons bascule dans les paramètres du menu Démarrer dans Windows](images/NightLightToggle.png)

## <a name="create-a-toggle-switch"></a>Créer un bouton bascule

Voici comment créer un bouton bascule simple. Ce code XAML crée le bouton bascule WiFi présenté précédemment.

```xaml
<ToggleSwitch x:Name="wiFiToggle" Header="Wifi"/>
```
Voici comment créer le même bouton bascule à l’aide de code.

```csharp
ToggleSwitch wiFiToggle = new ToggleSwitch();
wiFiToggle.Header = "WiFi";

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(wiFiToggle);
```

### <a name="ison"></a>IsOn

Le commutateur peut être activé ou désactivé. Utilisez la propriété [IsOn](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.ison.aspx) pour déterminer l’état du commutateur. Lorsque le commutateur est utilisé pour contrôler l’état d’une autre propriété binaire, vous pouvez utiliser une liaison, comme illustré ici.

```
<StackPanel Orientation="Horizontal">
    <ToggleSwitch x:Name="ToggleSwitch1" IsOn="True"/>
    <ProgressRing IsActive="{x:Bind ToggleSwitch1.IsOn, Mode=OneWay}" Width="130"/>
</StackPanel>
```

### <a name="toggled"></a>Toggled

Dans les autres cas, vous pouvez gérer l’événement [Toggled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.toggled.aspx) afin de répondre aux modifications de l’état.

Cet exemple indique comment ajouter un gestionnaire d’événements Toggled en XAML et à l’aide de code. L’événement Toggled est géré de façon à activer ou désactiver un anneau de progression et à en modifier la visibilité.

```xaml
<ToggleSwitch x:Name="toggleSwitch1" IsOn="True"
              Toggled="ToggleSwitch_Toggled"/>
```

Voici comment créer le même bouton bascule à l’aide de code.

```csharp
// Create a new toggle switch and add a Toggled event handler.
ToggleSwitch toggleSwitch1 = new ToggleSwitch();
toggleSwitch1.Toggled += ToggleSwitch_Toggled;

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(toggleSwitch1);
```

Voici le gestionnaire de l’événement Toggled.

```csharp
private void ToggleSwitch_Toggled(object sender, RoutedEventArgs e)
{
    ToggleSwitch toggleSwitch = sender as ToggleSwitch;
    if (toggleSwitch != null)
    {
        if (toggleSwitch.IsOn == true)
        {
            progress1.IsActive = true;
            progress1.Visibility = Visibility.Visible;
        }
        else
        {
            progress1.IsActive = false;
            progress1.Visibility = Visibility.Collapsed;
        }
    }
}
```

### <a name="onoff-labels"></a>Étiquettes On/Off

Par défaut, le bouton bascule inclut des étiquettes On et Off littérales, qui sont localisées automatiquement. Vous pouvez remplacer ces étiquettes en définissant les propriétés [OnContent](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.oncontent.aspx) et [OffContent](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.offcontent.aspx).

Cet exemple remplace les étiquettes On/Off par des étiquettes Show/Hide.  

```xaml
<ToggleSwitch x:Name="imageToggle" Header="Show images"
              OffContent="Show" OnContent="Hide"
              Toggled="ToggleSwitch_Toggled"/>
```

Vous pouvez également utiliser un contenu plus complexe en définissant les propriétés [OnContentTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.oncontenttemplate.aspx) et [OffContentTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.offcontenttemplate.aspx).

## <a name="recommendations"></a>Recommandations

-    Utilisez les étiquettes On et Off par défaut lorsque vous le pouvez; remplacez-les uniquement lorsque cela est nécessaire pour rendre le bouton bascule compréhensible. Si vous les remplacez, utilisez le mot qui décrit le plus précisément le bouton bascule. En règle générale, si les mots «On» et «Off» ne décrivent pas l’action liée à un bouton bascule, essayez éventuellement un autre contrôle.
-    Évitez de remplacer les étiquettes On et Off, sauf si vous y êtes tenu; utilisez les étiquettes par défaut, à moins que la situation n’appelle des étiquettes personnalisées.


## <a name="related-articles"></a>Articles connexes

- [Classe ToggleSwitch](https://msdn.microsoft.com/library/windows/apps/hh701411)
- [Cases d’option](radio-button.md)
- [Boutons bascule](toggles.md)
- [Cases à cocher](checkbox.md)
