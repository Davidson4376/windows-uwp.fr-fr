---
Description: Découvrez comment Fluent mouvement notions de base sont réunies dans votre application.
title: Mouvement en pratique - Animation dans les applications UWP
label: Motion in practice
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8cf010533d2d62559bb8dc0d214e04ab917e62bd
ms.sourcegitcommit: d534f81590d881a18d677a648c59913029837a84
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67535438"
---
# <a name="bringing-it-together"></a>Synthèse

Le minutage, l'accélération, la direction et la gravité fonctionnent ensemble pour former la base du mouvement Fluent. Chacun doit être pris en compte par rapport aux autres et correctement appliqué dans le contexte de votre application.

Voici 3 manières d’appliquer les principes de base du mouvement Fluent dans votre application.

:::row:::
    :::column:::
**Animation implicite** interpolation automatique et minutage entre des valeurs d’une modification de paramètre pour obtenir le mouvement de Fluent très simple à l’aide des valeurs normalisées.
    :::column-end:::
    :::column:::
**Animation intégrée** composants du système, tels que les contrôles communs et partagée motion, sont « Fluent par défaut ». Principes de base ont été appliqués de manière cohérente avec leur utilisation implicite.
    :::column-end:::
    :::column:::
**Animation personnalisée que vous suiviez les recommandations des conseils** il peut arriver lorsque le système ne fournit pas encore une solution de mouvement exacte pour votre scénario. Dans ce cas, utilisez les recommandations fondamentales de base comme point de départ pour vos expériences.
    :::column-end:::
:::row-end:::

**Exemple de transition**

![Animation fonctionnelle](images/pageRefresh.gif)

:::row:::
    :::column:::
<b>Direction de sortie vers l’avant :</b><br>
Fondu : 150m ; Accélération : Par défaut accélérer <b>Direction à suivre dans :</b><br>
Faites glisser les 150 px : 300 millisecondes ; Accélération : Par défaut décélérer
    :::column-end:::
    :::column:::
<b>Direction descendante Out :</b><br>
Faites glisser vers le bas 150 px : 150ms ; Accélération : Par défaut accélérer <b>Direction descendante dans :</b><br>
Apparition en fondu : 300 millisecondes ; Accélération : Par défaut décélérer
    :::column-end:::
:::row-end:::

**Exemple d’objet**

 ![Mouvement de 300 ms](images/control.gif)

:::row:::
    :::column:::
<b>Développez de direction :</b><br>
La croissance : 300 millisecondes ; Accélération : Standard
    :::column-end:::
    :::column:::
<b>Contrat de direction :</b><br>
La croissance : 150ms ; Accélération : Accélérer la valeur par défaut
    :::column-end:::
:::row-end:::

## <a name="examples"></a>Exemples

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si vous avez le <strong style="font-weight: semi-bold">galerie de contrôles XAML</strong> application installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/ImplicitTransition">ouvrez l’application et voir les Transitions implicites en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="implicit-animations"></a>Animations implicites

> Animations implicites requièrent Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure.

Animations implicites sont un moyen simple d’atteindre le mouvement Fluent par l’interpolation automatiquement entre les valeurs anciennes et nouvelles pendant un changement de paramètre.

Vous pouvez animer implicitement les modifications apportées aux propriétés suivantes :

- [UIElement](/uwp/api/windows.ui.xaml.uielement)
  - **Opacity**
  - **Rotation**
  - **Échelle**
  - **Translation**

- [Bordure](/uwp/api/windows.ui.xaml.controls.border), [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter), ou [Panneau de configuration](/uwp/api/windows.ui.xaml.controls.panel)
  - **Arrière-plan**

Chaque propriété qui peut avoir des modifications implicitement animées correspond un _transition_ propriété. Pour animer la propriété, vous affectez un type de transition correspondant _transition_ propriété. Le tableau suivant répertorie les _transition_ propriétés et le type de transition à utiliser pour chacun d'entre eux.

| Propriété animée | Propriété de transition | Type de transition implicite |
| -- | -- | -- |
| [UIElement.Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | [OpacityTransition](/uwp/api/windows.ui.xaml.uielement.opacitytransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | [RotationTransition](/uwp/api/windows.ui.xaml.uielement.rotationtransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Scale](/uwp/api/windows.ui.xaml.uielement.scale) | [ScaleTransition](/uwp/api/windows.ui.xaml.uielement.scaletransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [UIElement.Translation](/uwp/api/windows.ui.xaml.uielement.translation) | [TranslationTransition](/uwp/api/windows.ui.xaml.uielement.translationtransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [Border.Background](/uwp/api/windows.ui.xaml.controls.border.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.border.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [ContentPresenter.Background](/uwp/api/windows.ui.xaml.controls.contentpresenter.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.contentpresenter.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [Panel.Background](/uwp/api/windows.ui.xaml.controls.panel.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.panel.backgroundtransition)  | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |

Cet exemple montre comment utiliser la propriété d’opacité et de transition pour créer un bouton apparaître lorsque le contrôle est activé et disparaître en fondu lorsqu’il est désactivé.

```xaml
<Button x:Name="SubmitButton"
        Content="Submit"
        Opacity="{x:Bind OpaqueIfEnabled(SubmitButton.IsEnabled), Mode=OneWay}">
    <Button.OpacityTransition>
        <ScalarTransition />
    </Button.OpacityTransition>
</Button>
```

```csharp
public double OpaqueIfEnabled(bool IsEnabled)
{
    return IsEnabled ? 1.0 : 0.2;
}
```

## <a name="related-articles"></a>Articles connexes

- [Présentation de Motion](index.md)
- [Accélération et d’échéance](timing-and-easing.md)
- [Une direction et la gravité](directionality-and-gravity.md)
