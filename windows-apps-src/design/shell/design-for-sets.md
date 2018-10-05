---
author: jwmsft
description: Découvrez comment optimiser votre application pour fournir la meilleure expérience possible dans l’interface utilisateur des Jeux.
title: Conception de Jeux
template: detail.hbs
ms.author: jimwalk
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, uwp, barre de titre
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 7c3e0e6ec7331e860c9153e2a2e29a51fb5848bd
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4390758"
---
# <a name="designing-for-sets"></a>Conception de Jeux

> [!IMPORTANT]
> Cet article décrit des fonctionnalités qui n’ont pas encore été publiées et sont susceptibles d’être considérablement modifiées d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.

À partir de WindowsInsiderPreview, la fonctionnalité Jeux est disponible pour les utilisateurs de votre application. Avec Jeux, votre application est intégrée dans une fenêtre qui peut être partagée avec d’autres applications et chaque application obtient son propre onglet dans la barre de titre.

Dans cet article, nous examinons les aspects pour lesquels vous pouvez optimiser votre application afin de fournir la meilleure expérience possible dans l’interface utilisateur Jeux.

> [!TIP]
> Pour plus d’informations sur Jeux, voir le billet de blog [Présentation de Jeux](https://insider.windows.com/en-us/articles/introducing-sets/) et l'exposé [Développement de Jeux](https://developer.microsoft.com/events/build/content/developing-for-sets-on-windows-10) présenté lors de la conférence MicrosoftBuild2018.

## <a name="customizing-tab-visuals"></a>Personnalisation des visuels d'onglet

Par défaut, le système essaie de sélectionner des couleurs appropriées pour le texte et l'icône de l’onglet de votre application lorsque celui-ci est actif. Cela vous permet d'obtenir une apparence correcte pour l'onglet de votre application avec un minimum d’effort de votre part, ou même sans apporter aucune optimisation à Jeux.

Toutefois, dans certains cas, il peut être préférable de personnaliser la couleur de l’onglet de votre application. Dans cette section, nous expliquons les comportements d'onglet par défaut et vous indiquons les cas où vous devez ou ne devez pas les modifier pour votre application.

### <a name="tab-states"></a>États d'onglet

Lorsque votre application se trouve dans un Jeu, son onglet peut être dans l'un des trois états suivants: sélectionné et actif, sélectionné et inactif, ou non sélectionné et inactif.

- **Sélectionné-actif**: onglet sélectionné dans un jeu de fenêtres groupées et qui se trouve dans la fenêtre active au premier plan.

    (Dans ce document, toute discussion relative à une modification de l'onglet _actif_ implique que l’onglet se trouve dans l'état Sélectionné-actif.)
- **Sélectionné-inactif**: onglet sélectionné dans un jeu de fenêtres groupées, mais qui ne se trouve pas dans la fenêtre active au premier plan.
- **Non sélectionné-inactif**: onglet qui n’est pas sélectionné dans un ensemble de fenêtres groupées.

La couleur des onglets inactifs est mise à jour et gérée par le système en fonction du thème système. Il n’existe aucun moyen de l'influencer à partir de votre application.

Par défaut, l’onglet sélectionné et actif respecte la couleur du thème système spécifiée par l’utilisateur dans les paramètres Windows. Vous pouvez personnaliser la couleur de l’onglet pour votre application uniquement lorsque l’onglet est actif.

![États d'onglet dans Jeux](images/sets-tab-states.jpg)

### <a name="coloring-of-active-tabs"></a>Coloration des onglets actifs

La couleur de l’onglet actif est déterminée par les valeurs que vous définissez dans votre application ou par les paramètres système. La couleur de l’onglet utilisée lorsque votre application est active est déterminée comme suit:

- Si vous spécifiez une couleur d'onglet dans votre application, celle-ci a la priorité la plus élevée. La couleur d’onglet que vous spécifiez dans votre application est utilisée lorsque votre application est active, indépendamment des paramètres système.
- Autrement, si dans les paramètres Windows, l’utilisateur sélectionne l’option d'afficher la couleur d’accentuation sur les barres de titre, la couleur d’accentuation système est utilisée.
  - Ce paramètre est disponible dans l’application Paramètres Windows sous Personnalisation > Couleurs > Afficher la couleur d’accentuation sur les surfaces suivantes: Barres de titre.
- Enfin, si aucun paramètre d’application ou utilisateur n’est appliqué, l’onglet utilise la couleur du thème système actuel.

### <a name="considerations-when-you-modify-tab-colors"></a>Éléments à prendre en compte lorsque vous modifiez les couleurs de l’onglet

Nous abordons ici les situations dans lesquelles vous souhaitez modifier la couleur de l'onglet de votre application, ainsi que les éléments que vous devez prendre en compte dans ce cas. Nous allons également aborder quelques situations où il est recommandé de ne pas modifier la couleur de l’onglet mais de laisser le système la gérer.

#### <a name="match-your-brand-colors"></a>Harmoniser les couleurs avec celles de votre marque

En règle générale, le facteur déterminant de votre choix de modifier la couleur de l’onglet est le souhait de la faire correspondre à la couleur de votre marque. Lorsque vous modifiez l'onglet de votre application pour le faire correspondre à la couleur de votre marque, vous devez tester son aspect en tenant compte des autres considérations présentées dans cette section, telles que la disposition de votre application, ou les différentes couleurs du thème système.

#### <a name="horizontal-layout"></a>Disposition horizontale

Si la disposition de votre application inclut une bande de couleur unie (non acrylique) qui s'étend horizontalement dans la partie supérieure, en général, utiliser une même couleur pour coordonner votre application avec son onglet représente une bonne solution. Choisissez une couleur unie pour votre onglet qui rappelle votre application, dans l’idéal, en assurant une continuité avec la couleur présente en haut de votre application.

#### <a name="horizontal-layout-with-acrylic"></a>Disposition horizontale avec l'acrylique

Si votre application utilise une bande de matière acrylique qui s’étend horizontalement dans la partie supérieure, nous vous recommandons de laisser le système déterminer la couleur de l’onglet.

Nous vous recommandons également ici d'utiliser l'acrylique dans l’application, au lieu d'utiliser un arrière-plan acrylique, afin de laisser l'arrière-plan de votre application s'étendre à cette zone, plutôt que de créer un effet de bande laissant voir l’application ou le bureau derrière elle.

Notez que, dans ce cas, l'utilisateur peut configurer ces onglets pour utiliser la couleur d’accentuation, afin qu’ils apparaissent soit en thème clair/sombre, soit dans la couleur d’accentuation.

#### <a name="vertical-layout"></a>Disposition verticale

Si la disposition de votre application inclut un volet vertical de couleur unie qui s’étend verticalement, nous vous recommandons de ne pas personnaliser les couleurs de l’onglet. La position de l’onglet au-dessus de votre application peut être modifiée à tout moment par l’utilisateur, donc vous ne pouvez pas compter sur une continuité de couleur entre la partie supérieure de votre application et l’onglet. Le système utilise d'autres signaux visuels, comme des ombres, pour harmoniser l’onglet avec votre application.

### <a name="how-to-modify-tab-colors"></a>Comment modifier les couleurs de l’onglet

La couleur de l’onglet actif utilise les API de personnalisation de la barre de titre existantes. Si vous avez déjà personnalisé la couleur de la barre de titre de votre application, la modification s’applique également à l’onglet application quand votre application est dans un Jeu.

Pour modifier les couleurs de l’onglet, définissez les propriétés [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) pour spécifier:

- une couleur unie d’arrière-plan de l'onglet;
- une couleur unie de premier plan pour le texte de votre onglet.

Cet exemple montre comment obtenir une instance de ApplicationViewTitleBar et définir ses propriétés de couleur.

```csharp
// using Windows.UI.ViewManagement;
var titleBar = ApplicationView.GetForCurrentView().TitleBar;

// Set active window colors
titleBar.ForegroundColor = Windows.UI.Colors.White;
titleBar.BackgroundColor = Windows.UI.Colors.Green;
```

Pour plus d’informations, voir la section _Personnalisation simple des couleurs_ de l'article [Personnalisation de la barre de titre](title-bar.md#simple-color-customization) et [Exemple de personnalisation de la barre de titre](http://go.microsoft.com/fwlink/p/?LinkId=620613).

### <a name="considerations-for-full-title-bar-customization"></a>Éléments à prendre en compte pour une personnalisation complète de la barre de titre

Vous pouvez également personnaliser entièrement la barre de titre de votre application, comme décrit dans la section _Personnalisation complète_ de l'article [Personnalisation de la barre de titre](title-bar.md#full-customization). En général, vous pouvez pour ce faire [étendre l'acrylique dans la barre de titre](../style/acrylic.md#extend-acrylic-into-the-title-bar) ou placer du contenu personnalisé dans la barre de titre. Dans ce cas, veillez à suivre les recommandations pour le mode plein écran et le mode tablette et n'afficher votre contenu personnalisé de la barre de titre que si [CoreApplicationViewTitleBar.IsVisible](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.isvisible) est **true**.

Lorsque votre application s’exécute dans un Jeu, CoreApplicationViewTitleBar.IsVisible est **false** et le contenu de barre de titre ne doit pas s’afficher. Toutefois, si vous ne suivez pas ces conseils pour masquer le contenu de la barre de titre personnalisé, celui-ci s'affiche sous l’onglet de votre application et non dans la zone de barre de titre.

Si vous avez placé le contenu ou des fonctionnalités dans l’interface utilisateur de votre barre de titre personnalisée, envisagez de les rendre disponible dans une autre surface de l’interface utilisateur de votre application.

### <a name="how-to-modify-the-tab-icon"></a>Modification de l'icône de l'onglet

Pour être sûr que l’icône de votre application s'affiche de façon optimale dans un Jeu, vous devez fournir une autre icône sans plaque pour votre application. (L’icône d'application utilisée dans l’onglet de votre application est la même que celle qui figure dans la barre des tâches.) Le but d'une autre icône est d'obtenir un aspect correct avec toutes les couleurs de l'arrière-plan. L’autre icône sera utilisée si elle est disponible.

Dans le manifeste de l’application, spécifiez une icône sans plaque sous une autre forme en plus de votre icône normale. Pour plus d’informations, voir les [logos et icônes d’application](/windows/uwp/design/style/app-icons-and-logos). L’icône à spécifier est documentée sous «ressources taille de la cible sans plaque» dans la section [en savoir plus sur les ressources d’icône d’application](/windows/uwp/design/style/app-icons-and-logos#more-about-app-icon-assets) de l’article.

Si vous ne spécifiez pas d'autre icône dans le manifeste de l'application, le système redonnera une plaque à l’icône de vignette avec la couleur de l’onglet et l’utilisera.

![Icônes utilisées dans les Jeux](images/sets-icons.png)

> La même icône est utilisée dans la barre des tâches et dans l’onglet d'application.

## <a name="restore-previous-sets-with-user-activities"></a>Restaurer les Jeux précédents avec les activités de l’utilisateur

Un avantage des Jeux est qu’ils permettent à vos utilisateurs de restaurer les onglets d'applications et de contenu web précédemment ouverts lorsqu’ils lancent une application ou ouvrent un document. (Voir la vidéo dans le billet de blog [Présentation des Jeux](https://insider.windows.com/en-us/articles/introducing-sets/) pour en savoir plus.) Cette option est activée par le biais des _activités utilisateur_.

Par défaut, le système crée des activités utilisateur pour le compte de votre application, ce qui permet à votre application d’être restaurée dans un onglet lorsqu’un utilisateur lance une application ou ouvre un document. Toutefois, les activités utilisateur par défaut créées par le système ne peuvent lancer votre application que dans son état par défaut. Elles ne peuvent pas restaurer votre application à son état précédent dans le cadre du Jeu.

Vous pouvez optimiser votre application pour les Jeux en fournissant des activités utilisateur personnalisées. L’activité utilisateur pour laquelle vous fournissez des liens ciblés dans votre application pour la restaurer à son dernier état dans le cadre du Jeu en cours de restauration.

Pour fournir une activité utilisateur personnalisée:

- Répondez à l'[UserActivityRequest](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityrequest)  initiée par le système d’exploitation avec une [UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity) appropriée.
- L'UserActivity contient un URI de lien ciblé d’activation que le système utilise pour lancer votre application avec un contexte spécifique.

Pour plus d’informations, voir l'[événement UserActivityRequestManager.UserActivityRequested](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityrequestmanager.useractivityrequested), [Gérer l’activation des URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation) et [Poursuivre l’activité utilisateur, même sur différents appareils](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities).

## <a name="enable-multi-instance-for-uwp-apps"></a>Activer les instances multiples pour les applications UWP

À compter de Windows10, version1803, les applications UWP prennent en charge l'instanciation multiple. Les applications UWP sont toujours à instance unique par défaut, et vous devez explicitement choisir dans chaque application d'activer l'instanciation multiple.

Si vous activez l'instanciation multiple pour votre application, vous donnez à vos utilisateurs l’avantage de pouvoir exécuter votre application dans plus d’un Jeu à la fois. Une application à instance unique ne peut s’exécuter que dans un Jeu à la fois.

Pour plus d’informations sur l’activation des instances multiples pour votre application UWP, voir [Créer une application Windows universelle à instances multiples](https://docs.microsoft.com/windows/uwp/launch-resume/multi-instance-uwp).

## <a name="use-an-in-app-back-button"></a>Utiliser un bouton précédent dans l’application

Pour implémenter la navigation vers l’arrière dans votre application, nous vous conseillons de placer un bouton Précédent dans l’interface utilisateur de votre application conformément aux [Recommandations pour un bouton Précédent](../basics/navigation-history-and-backwards-navigation.md). Si votre application utilise le contrôle NavigationView, vous devez utiliser le bouton Précédent intégré de NavigationView.

Si votre application utilise le bouton précédent du système, vous devez le remplacer par un bouton précédent dans l’application. Cela garantit aux utilisateurs une expérience de bouton Précédent cohérente, indépendamment du fait que l’application s’exécute ou non dans un Jeu, et permet également que les éléments visuels du bouton Précédent restent cohérents dans toutes les applications.

Pour obtenir des instructions détaillées sur l’intégration d’un bouton Précédent dans l’application, voir [Navigation dans l’historique et navigation vers l’arrière](../basics/navigation-history-and-backwards-navigation.md).

### <a name="support-for-the-system-back-button-in-sets"></a>Prise en charge du bouton Précédent système dans les Jeux

Si votre application utilise le bouton Précédent système au lieu d'un bouton dans l’application, l’interface utilisateur système affichera toujours le rendu du bouton Précédent système pour garantir la compatibilité descendante

- Si votre application ne se trouve pas dans un Jeu, le bouton Précédent s'affichera dans la barre de titre. Les interactions utilisateur et l'expérience visuelle du bouton Précédent restent inchangées.
- Si votre application se trouve dans un Jeu, le bouton Précédent s'affiche dans la barre Précédent système.

La barre Précédent système est une «bande» qui est insérée entre la bande d'onglet et la zone de contenu de l’application. La bande s'étend sur toute la largeur de l’application et le bouton Précédent se trouve sur son bord gauche. La hauteur verticale de la bande est suffisamment large pour garantir une taille de cible tactile adéquate pour le bouton Précédent.

![Barre Précédent système des Jeux](images/sets-system-back-bar.png)

> Barre Précédent système affichée dans une application.

La barre Précédent système s'affiche de façon dynamique, en fonction de la visibilité du bouton Précédent. Lorsque le bouton précédent est visible, la barre Précédent système s'insère et déplace le contenu de l’application vers le bas sous la bande d'onglet. Lorsque le bouton Précédent est masqué, la barre Précédent système est supprimée de manière dynamique, le contenu de l'application se décale vers le haut contre la bande d'onglet.

Pour éviter que l’interface utilisateur de votre application se déplace vers le haut ou vers le bas, nous vous recommandons d'utiliser un bouton Précédent dans l’application au lieu du bouton Précédent système.

Les personnalisations de la barre de titre s’appliquent à la fois à l’onglet de l’application et à la barre Précédent système. Si vous utilisez les API ApplicationViewTitleBar pour spécifier les propriétés de couleur du premier plan et de l'arrière-plan, les couleurs s'appliquent à l’onglet et la barre Précédent système.

## <a name="related-articles"></a>Articles associés

- [Personnalisation de la barre de titre](title-bar.md)
- [Navigation dans l’historique et navigation vers l’arrière](../basics/navigation-history-and-backwards-navigation.md)
- [Couleur](../style/color.md)
