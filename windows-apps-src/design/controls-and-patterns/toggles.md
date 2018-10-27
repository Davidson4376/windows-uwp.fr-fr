---
author: QuinnRadich
Description: The toggle switch represents a physical switch that allows users to turn things on or off.
title: Recommandations en matière de boutons bascule
ms.assetid: 753CFEA4-80D3-474C-B4A9-555F872A3DEF
label: Toggle switches
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d166e872af0824c9eb5df15510f9597d0303f483
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5692881"
---
# <a name="toggle-switches"></a>Boutons bascule

Le bouton bascule représente un commutateur physique qui permet à l’utilisateur d’activer ou de désactiver des options. Utilisez des contrôles ToggleSwitch pour présenter aux utilisateurs deux options qui s’excluent mutuellement (comme activé/désactivé), de sorte que la sélection d’une option produise immédiatement un résultat.

Pour créer un contrôle de bouton bascule, utilisez la [classe ToggleSwitch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch).

> **API importantes**: [classe ToggleSwitch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch), [propriété IsOn](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.ison), [événement Toggled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.toggled)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Utilisez un bouton bascule pour les opérations binaires qui prennent effet dès que l’utilisateur change l’état du bouton.

![Bouton bascule WiFi, activé et désactivé](images/toggleswitches01.png)

Imaginez le bouton bascule comme le bouton marche/arrêt d’un appareil: vous l’actionnez quand vous souhaitez activer ou désactiver l’action de l’appareil.

Pour rendre le bouton bascule facile à comprendre, utilisez des étiquettes contenant un ou deux mots, de préférence des noms, qui décrivent la fonctionnalité contrôlée. Par exemple, «WiFi» ou «Lumières cuisine». 

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour ouvrir l’application et voir l'objet <a href="xamlcontrolsgallery:/item/ToggleSwitch">ToggleSwitch</a> ou <a href="xamlcontrolsgallery:/item/ToggleButton">ToggleButton</a> en action.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="choosing-between-toggle-switch-and-check-box"></a>Choix entre le bouton bascule et la case à cocher

Le bouton bascule et la case à cocher peuvent tous deux convenir à certaines actions. Pour déterminer le contrôle le mieux approprié, suivez ces conseils :

- Utilisez un bouton bascule pour les paramètres binaires impliquant des modifications qui deviennent immédiatement effectives une fois que l’utilisateur les a apportées.

    ![Bouton bascule ou case à cocher](images/toggleswitches02.png)

    Dans cet exemple, il est évident dans le cas du bouton bascule que les lumières de la cuisine sont allumées. Mais pour la case à cocher, l’utilisateur doit se demander si les lumières sont allumées ou s’il doit activer la case à cocher pour allumer les lumières.

- Utilisez les cases à cocher pour les éléments facultatifs («bonus»).
- Utilisez une case à cocher lorsque l’utilisateur doit réaliser des étapes supplémentaires afin de rendre effectives les modifications. Par exemple, si l’utilisateur doit cliquer sur un bouton «Envoyer» ou «Suivant» pour appliquer les modifications, utilisez une case à cocher.
- Utilisez les cases à cocher lorsque l’utilisateur peut sélectionner plusieurs éléments liés à un paramètre ou une fonctionnalité unique.

## <a name="toggle-switches-in-the-windows-ui"></a>Boutons bascule dans l’interface utilisateur Windows

Ces images montrent comment l’interface utilisateur Windows utilise les boutons bascule. Voici comment l’écran Paramètres Smart Storage utilise les boutons bascule:

![Boutons bascule dans Smart Storage](images/SmartStorageToggle.png)

Cet exemple est tiré de la page Paramètres d’éclairage nocturne:

![Boutons bascule dans les paramètres du menu Démarrer dans Windows](images/NightLightToggle.png)

## <a name="create-a-toggle-switch"></a>Créer un bouton bascule

Voici comment créer un bouton bascule simple. Ce code XAML crée le bouton bascule présenté précédemment.

```xaml
<ToggleSwitch x:Name="lightToggle" Header="Kitchen Lights"/>
```

Voici comment créer le même bouton bascule à l’aide de code.

```csharp
ToggleSwitch lightToggle = new ToggleSwitch();
lightToggle.Header = "Kitchen Lights";

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(lightToggle);
```

### <a name="ison"></a>IsOn

Le commutateur peut être activé ou désactivé. Utilisez la propriété [IsOn](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.ison) pour déterminer l’état du commutateur. Lorsque le commutateur est utilisé pour contrôler l’état d’une autre propriété binaire, vous pouvez utiliser une liaison, comme illustré ici.

```xaml
<StackPanel Orientation="Horizontal">
    <ToggleSwitch x:Name="ToggleSwitch1" IsOn="True"/>
    <ProgressRing IsActive="{x:Bind ToggleSwitch1.IsOn, Mode=OneWay}" Width="130"/>
</StackPanel>
```

### <a name="toggled"></a>Toggled

Dans les autres cas, vous pouvez gérer l’événement [Toggled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.toggled) afin de répondre aux modifications de l’état.

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

Par défaut, le bouton bascule inclut des étiquettes On et Off littérales, qui sont localisées automatiquement. Vous pouvez remplacer ces étiquettes en définissant les propriétés [OnContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.oncontent) et [OffContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.offcontent).

Cet exemple remplace les étiquettes On/Off par des étiquettes Show/Hide.

```xaml
<ToggleSwitch x:Name="imageToggle" Header="Show images"
              OffContent="Show" OnContent="Hide"
              Toggled="ToggleSwitch_Toggled"/>
```

Vous pouvez également utiliser un contenu plus complexe en définissant les propriétés [OnContentTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.oncontenttemplate) et [OffContentTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.offcontenttemplate).

## <a name="recommendations"></a>Recommandations

- Utilisez les étiquettes On et Off par défaut lorsque vous le pouvez; remplacez-les uniquement lorsque cela est nécessaire pour rendre le bouton bascule compréhensible. Si vous les remplacez, utilisez le mot qui décrit le plus précisément le bouton bascule. En règle générale, si les mots «On» et «Off» ne décrivent pas l’action liée à un bouton bascule, essayez éventuellement un autre contrôle.
- Évitez de remplacer les étiquettes On et Off, sauf si vous y êtes tenu; utilisez les étiquettes par défaut, à moins que la situation n’appelle des étiquettes personnalisées.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

- [Classe ToggleSwitch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch)
- [Cases d’option](radio-button.md)
- [Boutons bascule](toggles.md)
- [Cases à cocher](checkbox.md)
