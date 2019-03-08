---
title: Composition de personnalisation
description: Utiliser les API de Composition pour adapter votre interface utilisateur, d’optimiser les performances et de prendre en charge les paramètres utilisateur et les caractéristiques de l’appareil.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bcc9a6d89a143d8fd03d73dbd83b832ed9513ee2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644414"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>Personnalisation des effets et des expériences à l’aide de l’interface utilisateur de Windows

L’interface utilisateur de Windows fournit de nombreux effets magnifiques, animations et moyens de différenciation. Toutefois, répondant aux attentes des utilisateurs pour les performances et les possibilités de personnalisation est toujours une partie nécessaire de créer des applications réussies. La plateforme Windows universelle prend en charge une famille volumineux et variée d’appareils qui ont des fonctionnalités différentes. Afin d’offrir une expérience incluse pour tous vos utilisateurs, vous devez vous assurer d’évoluer vos applications sur des appareils et respecter les préférences de l’utilisateur. Personnalisation de l’interface utilisateur peut fournir un moyen efficace pour exploiter les fonctionnalités d’un appareil et garantir une expérience utilisateur agréable et inclusif.

Personnalisation de l’interface utilisateur est une vaste catégorie entourant le travail pour performante, belle interface utilisateur en ce qui concerne les domaines suivants :

- En respectant et l’adaptation aux paramètres utilisateur pour les effets
- Prise en charge des paramètres utilisateur pour les animations
- Optimisation de l’interface utilisateur pour les capacités matérielles donné

Ici, nous aborderons comment adapter les effets et les animations avec la couche visuelle dans les domaines ci-dessus, mais il existe de nombreux autres moyens d’adapter votre application pour garantir une expérience utilisateur. Docs de conseils sont disponibles sur la façon [adapter votre interface utilisateur](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design) pour différents appareils et [créer une interface utilisateur réactive](/windows/uwp/design/layout/responsive-design).

## <a name="user-effects-settings"></a>Paramètres utilisateur des effets

Les utilisateurs peuvent personnaliser leur expérience de Windows pour diverses raisons, dont les applications doivent respecter et s’adapter aux. Une zone, les utilisateurs finaux peuvent contrôler change les types d’effets qu'ils voient utilisés tout au long de leur système.

### <a name="transparency-effects-settings"></a>Paramètres des effets de transparence

Un tel paramètre effet que les utilisateurs peuvent personnaliser qui tente d’activer ou désactiver les effets de transparence. Cela vous trouverez dans l’application de paramètres sous Personnalisation > couleurs, ou via l’application Paramètres > facilité d’accès > affichage.

![Option de transparence dans les paramètres](images/tailoring-transparency-setting.png)

Lorsque activé, n’importe quel effet qui utilise la transparence s’affiche comme prévu. Cela s’applique aux ACRYLIQUE, HostBackdropBrush ou n’importe quel graphique effet personnalisé qui n’est pas entièrement opaque.

Mise hors tension, matériau ACRYLIQUE automatiquement reviendra à une couleur unie, car le pinceau ACRYLIQUE du XAML a pris en compte cet événement par défaut. Nous voyons ici, l’application Calculatrice correctement revenir à une couleur unie lors de l’effet de transparence n’est pas activés :

![Calculatrice avec ACRYLIQUE](images/tailoring-acrylic.png)
![Calculatrice avec ACRYLIQUE répond aux paramètres de transparence](images/tailoring-acrylic-fallback.png)

Toutefois, pour l’application d’effets personnalisés doit répondre à la [UISettings.AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) propriété ou [AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) événement et switch out l’effet/effet graphique à utiliser un effet qui l’a pas de transparence. Un exemple de ceci est ci-dessous :

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

## <a name="animations-settings"></a>Paramètres des animations

De même, les applications doivent écouter et répondre à la [UISettings.AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled) propriété afin de garantir les animations sont activés ou désactivés en fonction des paramètres utilisateur dans les paramètres > facilité d’accès > affichage.

![Option d’animations dans les paramètres](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>Exploiter les fonctionnalités API

En tirant parti de la [CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) API, vous pouvez détecter quels composition fonctionnalités sont disponibles et performante sur un matériel donné et adapter la conception afin de garantir aux utilisateurs finaux un performant et un superbe expérience sur n’importe quel appareil. Les API fournissent un moyen de vérifier les fonctionnalités de système de matériel afin d’implémenter l’effet sans perte de données mise à l’échelle sur une large gamme de facteurs de forme. Cela facilite l’adapter correctement l’application pour créer une belle et l’expérience utilisateur transparente.

Cette API fournit des méthodes et un écouteur d’événements qui peut être utilisé pour rendre l’effet des décisions de l’interface utilisateur de l’application de mise à l’échelle. La fonctionnalité détecte le degré le système peut gérer la composition complexe et les opérations de rendu, puis retourne les informations dans un modèle à utiliser pour les développeurs à utiliser.

### <a name="using-composition-capabilities"></a>À l’aide des fonctionnalités de composition

La fonctionnalité CompositionCapabilities est déjà utilisée pour les fonctionnalités telles que des documents ACRYLIQUE, où le matériel revient à l’effet plus performante selon le scénario et le matériel.

L’API peut être ajouté au code existant en quelques étapes simples.

1. Acquérir l’objet des fonctionnalités dans le constructeur de votre application.

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. Enregistrement d’un écouteur d’événement modifié de fonctionnalités pour votre application.

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. Ajouter du contenu à la méthode de rappel d’événement pour gérer différents niveaux de fonctionnalités. Cela peut ou ne peut pas être similaire à l’étape suivante ci-dessous.
1. Lorsque vous utilisez des effets, vérifiez d’abord l’objet des fonctionnalités. Envisagez d’utiliser des vérifications conditionnelles ou basculez des instructions de contrôle, en fonction de la façon dont vous souhaitez personnaliser les effets.

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

Exemple de code complet, consultez la [référentiel Github d’interface utilisateur Windows](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2015063/CompCapabilities).

## <a name="fast-vs-slow-effects"></a>Rapide et effets lentes

En fonction des commentaires fournis [AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported) et [AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) méthodes dans l’API CompositionCapabilities, l’application peut décider à échanger les effets de coûteux ou non pris en charge pour autres effets de leur choix qui sont optimisés pour l’appareil. Certains effets sont connus pour être constamment plus de ressources que d’autres et doivent être utilisés avec parcimonie, et autres effets peuvent être utilisés plus librement. Pour tous les effets, toutefois, soins doit utiliser quand le chaînage et l’animation en tant que certains scénarios ou des combinaisons peuvent changer les caractéristiques de performances du graphe d’effet. Voici certaines caractéristiques de performances de règle générale pour les effets individuels :

- Les effets sont connus pour affecter les performances élevées sont comme suit : Flou gaussien, masque de clichés instantanés, BackDropBrush, HostBackDropBrush et couche Visual. Elles ne sont pas recommandées pour les appareils de bas de gamme [(fonctionnalité niveau 9.1-9.3)](https://msdn.microsoft.com/library/windows/desktop/ff476876(v=vs.85).aspx)et doit être utilisé judicieusement sur les appareils de haut de gamme.
- Effets avec un impact sur les performances moyennes incluent la matrice des couleurs, certains BlendModes d’effet Blend (luminosité, la couleur, Saturation et Hue), actualités, SceneLightingEffect et (en fonction du scénario) BorderEffect. Ces effets peuvent fonctionner avec certains scénarios sur les appareils de bas de gamme, mais les soins doit être utilisée lorsque le chaînage et l’animation. Recommandons inférieure ou égale à deux pour limiter l’utilisation et l’animation sur les transitions uniquement.
- Tous les autres effets ont impact sur les performances faible et fonctionnent dans tous les scénarios raisonnables lors de l’animation et le chaînage.

## <a name="related-articles"></a>Articles connexes

- [Techniques de conception réactives UWP](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [Personnalisation de périphérique UWP](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
