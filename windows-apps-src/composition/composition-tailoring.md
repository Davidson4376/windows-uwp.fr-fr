---
author: daneuber
title: Personnalisation de la composition
description: Utilisez les API de Composition à adapter votre interface utilisateur, optimiser les performances et prendre en charge des paramètres utilisateur et les caractéristiques de l’appareil.
ms.author: jimwalk
ms.date: 07/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 66384c4df3195ae0fff35ae5dd7e1b1983204068
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/19/2018
ms.locfileid: "4058393"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>Personnalisation des effets et des expériences à l’aide de l’interface utilisateur Windows

Interface utilisateur Windows fournit des nombreux effets magnifiques, les animations et les moyens de différenciation. Toutefois, les attentes des utilisateurs pour des performances et possibilités de personnalisation de la réunion sont toujours un élément nécessaire de créer des applications réussies. La plateforme Windows universelle prend en charge une famille volumineux et variée d’appareils, qui ont des fonctionnalités différentes. Pour fournir une expérience inclusive pour tous vos utilisateurs, vous devez afin de garantir votre échelle d’applications sur différents appareils et de respecter les préférences de l’utilisateur. Personnalisation de l’interface utilisateur peut fournir un moyen efficace pour tirer parti des fonctionnalités de l’appareil et garantir une expérience utilisateur agréable et plus inclusive.

Personnalisation de l’interface utilisateur est une catégorie large cernée de travail pour performante, l’interface utilisateur magnifique ce qui concerne les zones suivantes:

- En respectant et en adaptant les paramètres utilisateur pour les effets
- Prise en charge des paramètres utilisateur pour les animations
- L’optimisation de l’interface utilisateur pour les capacités matérielles donnée

Ici, nous allons décrire comment personnaliser les effets et les animations avec la couche visuelle dans les zones ci-dessus, mais il existe de nombreux autres moyens d’adapter votre application afin de garantir une expérience optimale à l’utilisateur final. Documentation de conseils est disponibles sur la façon d' [adapter votre interface utilisateur](/design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) aux différents appareils et [créer une interface utilisateur réactive](/design/layout/responsive-design.md).

## <a name="user-effects-settings"></a>Paramètres utilisateur effets

Les utilisateurs peuvent personnaliser leur expérience de Windows pour diverses raisons, dont les applications doivent respecter les et s’adapte à. Les utilisateurs finaux peuvent contrôler les zones change les types d’effets qu'ils voient s’afficher utilisés tout au long de leur système.

### <a name="transparency-effects-settings"></a>Paramètres des effets de transparence

Un tel paramètre d’effet les utilisateurs peuvent personnaliser est l’activation des effets de transparence marche/arrêt. Cela se trouvent dans l’application paramètres sous Personnalisation > couleurs, ou via l’application Paramètres > d’ergonomie > affichage.

![Option de transparence dans les paramètres](images/tailoring-transparency-setting.png)

Lorsque activé, un effet qui utilise la transparence s’affiche comme prévu. Cela s’applique à l’ACRYLIQUE, HostBackdropBrush ou n’importe quel graphique de l’effet personnalisé qui n’est pas une couleur totalement opaque.

Quand il sont désactivés, matière ACRYLIQUE automatiquement reviennent à une couleur unie, car le pinceau ACRYLIQUE du code XAML a pris en compte pour cet événement par défaut. Ici, nous voir l’application Calculatrice correctement revenir à une couleur unie lorsque les effets de transparence ne sont pas activées:

![Calculatrice avec l’ACRYLIQUE](images/tailoring-acrylic.png)
![Calculatrice avec l’ACRYLIQUE répondre aux paramètres de transparence](images/tailoring-acrylic-fallback.png)

Toutefois, pour appliquer des effets personnalisés, l’application doit répondre à la propriété de [UISettings.AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) ou d’un événement de [AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) et éliminer le graphique d’effet/effet d’utiliser un effet qui n’a aucun transparence. Un exemple est ci-dessous:

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool advancedEffects = uisettings.AdvancedEffectsEnabled;
   uisettings.AdvancedEffectsEnabledChanged += Uisettings_AdvancedEffectsEnabledChanged;
}

private void Uisettings_AdvancedEffectsEnabledChanged(UISettings sender, object args)
{
    // TODO respond to sender.AdvancedEffectsEnabled
}
```

## <a name="animations-settings"></a>Paramètres d’animations

De même, les applications doivent écouter et répondre à la propriété [UISettings.AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled) pour garantir des animations sont activées ou désactivé en fonction des paramètres utilisateur Paramètres > d’ergonomie > affichage.

![Option d’animations dans les paramètres](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>En exploitant les fonctionnalités API

En tirant parti de l’API [CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) , vous pouvez détecter la composition et des fonctionnalités disponibles performante sur un matériel donné et personnaliser la conception pour les utilisateurs finaux d’obtenir une expérience esthétiques et le performante sur n’importe quel appareil. Les API fournissent un moyen de vérifier les fonctionnalités du système matériel afin d’implémenter effet progressif mise à l’échelle sur divers facteurs de forme. Cela facilite l’adapter en fonction de l’application pour créer un esthétiques et l’expérience utilisateur transparente.

Cette API fournit des méthodes et un écouteur d’événements qui peut être utilisé pour rendre l’effet de mise à l’échelle de décisions pour l’interface utilisateur de l’application. La fonctionnalité détecte la façon dont le système peut gérer composition complexe et opérations de rendu, puis renvoie les informations dans un modèle facile à utiliser pour les développeurs à utiliser.

### <a name="using-composition-capabilities"></a>À l’aide des fonctionnalités de composition

La fonctionnalité CompositionCapabilities est déjà en cours optimisée pour des fonctionnalités telles que matière ACRYLIQUE, où le matériau revient à l’effet plus performante selon le scénario et le matériel.

L’API peut être ajoutée au code existant en quelques étapes simples.

1. Acquérir l’objet des fonctionnalités dans le constructeur de votre application.

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. Inscrire un écouteur d’événements ont été modifiés de fonctionnalités pour votre application.

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. Ajouter du contenu à la méthode de rappel d’événement pour gérer les différents niveaux de fonctionnalités. Cela peut ou ne peut pas être similaire à l’étape suivante ci-dessous.
1. Lorsque vous utilisez des effets, vérifiez d’abord l’objet des fonctionnalités. Envisagez d’utiliser les vérifications conditionnelles ou basculer les instructions de contrôle, en fonction de la façon dont vous souhaitez personnaliser les effets.

    ```cs
    if (_capabilities.AreEffectsSupported())
    {
        // Add incremental effects updates here

        if (_capabilities.AreEffectsFast())
        {
            // Add more advanced effects here where applicable
        }
    }
    ```

Exemple de code complet sont accessibles dans le [référentiel Github de l’interface utilisateur Windows](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2015063/CompCapabilities).

## <a name="fast-vs-slow-effects"></a>Rapide et effets lentes

Fonction des commentaires à partir des méthodes [AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported) et [AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) fournis dans l’API CompositionCapabilties, l’application peut décider de coûteux ou non pris en charge les effets d’échange pour d’autres effets de leur choix qui sont optimisées pour l’appareil. Certains effets sont connues pour être toujours davantage de ressources que d’autres et doivent être utilisées avec parcimonie, et autres effets peuvent être utilisés plus librement. Pour tous les effets, toutefois, soins doit être utilisé lorsque le chaînage et l’animation en tant que certains scénarios ou des combinaisons peuvent changer les caractéristiques de performances du graphique d’effet. Voici certaines caractéristiques de performances de règle générale pour les effets individuels:

- Effets qui sont connues pour que l’impact de hautes performances sont comme suit: Flou gaussien, masque d’ombre, BackDropBrush, HostBackDropBrush et couche visuelle. Elles ne sont pas recommandés pour les appareils de bas de gamme [(niveau de fonctionnalité 9.1-9.3)](https://msdn.microsoft.com/library/windows/desktop/ff476876(v=vs.85).aspx)et doivent être utilisées judicieusement sur les appareils de haut de gamme.
- Effets avec un impact sur les performances moyennes incluent la matrice de couleur, certaines BlendModes d’effet de fusion (luminosité, la couleur, la Saturation et teinte), projecteur, SceneLightingEffect et (selon le scénario) BorderEffect. Ces effets peuvent fonctionner avec certains scénarios sur les périphériques d’entrée de gamme, mais soins doit être utilisé lorsque le chaînage et l’animation. Recommandons inférieure ou égale à deux pour limiter l’utilisation et l’animation de transitions uniquement.
- Tous les autres effets ont d’impact sur les performances de faible et dans tous les scénarios raisonnables lors de l’animation et le chaînage.

## <a name="related-articles"></a>Articles associés

- [Techniques de conception réactive UWP](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [Personnalisation de périphérique UWP](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
