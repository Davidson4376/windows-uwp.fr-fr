---
author: QuinnRadich
title: Piste d'apprentissage - Créer et configurer un formulaire
description: Découvrez ce que vous devez faire pour créer un formulaire à la structure robuste dans votre application.
ms.author: quradic
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: prise en main, uwp, windows10, piste d'apprentissage, disposition, formulaire
ms.localizationpriority: medium
ms.openlocfilehash: c2a851a442cabca4529cd202c90db692c43adcb5
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "4022977"
---
# <a name="create-and-customize-a-form"></a>Créer et personnaliser un formulaire

Si vous créez une application pour laquelle les utilisateurs doivent saisir une quantité d’informations importante, vous voudrez probablement créer un formulaire à remplir. Cet article vous montre ce que vous devez connaître pour créer un formulaire utile et robuste.

Il ne s'agit pas d'un didacticiel. Si vous en souhaitez un, consultez notre [didacticiel sur la disposition adaptative](../design/basics/xaml-basics-adaptive-layout.md) qui vous fournira une expérience interactive pas à pas.

Nous verrons quels **contrôles XAML** utiliser dans un formulaire, comment les organiser au mieux sur votre page et comment optimiser votre formulaire en fonction des différentes tailles d’écran. Mais puisque la position relative des éléments visuels est importante dans un formulaire, commençons par examiner la disposition de la page avec XAML.

## <a name="what-do-you-need-to-know"></a>Ce que vous devez savoir

UWP ne dispose pas d’un contrôle de formulaire explicite que vous pouvez ajouter à votre application et configurer. À la place, vous devez créer un formulaire en organisant une collection d’éléments d’interface utilisateur sur une page.

Pour ce faire, vous devez connaître les **panneaux de disposition**. Ces conteneurs contiennent les éléments d'interface utilisateur de votre application et permettent de les organiser et de les regrouper. Placer des panneaux de disposition au sein d’autres panneaux de disposition permet de mieux contrôler l'endroit où vos éléments apparaissent, ainsi que leur affichage les uns par rapport aux autres. De plus, cela facilite grandement l'adaptation de votre application à différentes tailles d’écran.

Lisez [cette documentation sur les panneaux de disposition](../design/layout/layout-panels.md). Étant donné que les formulaires s'affichent généralement dans une ou plusieurs colonnes verticales, vous feriez bien de regrouper les éléments similaires dans un **StackPanel** et, au besoin, de les organiser au sein d'un **RelativePanel**. Commencez maintenant à assembler quelques panneaux. Si vous avez besoin d’une référence, voici une infrastructure de disposition de base pour un formulaire à deux colonnes:

```xaml
<RelativePanel>
    <StackPanel x:Name="Customer" Margin="20">
        <!--Content-->
    </StackPanel>
    <StackPanel x:Name="Associate" Margin="20" RelativePanel.RightOf="Customer">
        <!--Content-->
    </StackPanel>
    <StackPanel x:Name="Save" Orientation="Horizontal" RelativePanel.Below="Customer">
        <!--Save and Cancel buttons-->
    </StackPanel>
</RelativePanel>
```

## <a name="what-goes-in-a-form"></a>Contenu d'un formulaire

Vous devez remplir votre formulaire avec une sélection de [contrôles XAML](../design/controls-and-patterns/controls-and-events-intro.md). Vous les connaissez sans doute déjà, mais n’hésitez pas à les passer en revue pour rafraîchir vos connaissances. En particulier, vos contrôles devront permettent à votre utilisateur de saisir du texte ou de choisir une valeur dans une liste. Il s’agit d’une liste d’options que vous pouvez ajouter de base: vous n’avez pas besoin de lire tous les concernant, il suffit de savoir quoi elles ressemblent et comment elles fonctionnent.

* [TextBox](../design/controls-and-patterns/text-box.md) permet à un utilisateur d’entrée de texte dans votre application.
* [ToggleSwitch](../design/controls-and-patterns/toggles.md) permet à un utilisateur de choisir entre deux options.
* [DatePicker](../design/controls-and-patterns/date-picker.md) permet à un utilisateur de sélectionner une valeur de date.
* [TimePicker](../design/controls-and-patterns/time-picker.md) permet à un utilisateur de sélectionner une valeur d'heure.
* [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) se développe pour afficher une liste d’éléments sélectionnables. Vous trouverez davantage de détails [ici](../design/controls-and-patterns/lists.md#drop-down-lists)

Vous pouvez également ajouter des [boutons](../design/controls-and-patterns/buttons.md) pour que l’utilisateur puisse enregistrer ou annuler.

## <a name="format-controls-in-your-layout"></a>Contrôles du format dans votre disposition

Vous savez comment organiser des panneaux de disposition et disposez d'éléments à ajouter, mais comment devez-vous les formater? La page [formulaires](../design/controls-and-patterns/forms.md) fournit quelques instructions de conception spécifiques. Lisez les conseils utiles qui se trouvent dans les sections concernant les **Types de formulaires** et la **disposition**. Nous allons plus brièvement aborder l’accessibilité et la disposition relative.

En tenant compte de ces conseils, vous devez commencer à ajouter les contrôles de votre choix dans votre disposition, en veillant à leur donner des étiquettes et à les espacer correctement. Par exemple, voici un simple aperçu de formulaire sur une seule page conçu à l'aide de la disposition, des contrôles et des conseils de conception indiqués ci-dessus:

```xaml
<RelativePanel>
    <StackPanel x:Name="Customer" Margin="20">
        <TextBox x:Name="CustomerName" Header= "Customer Name" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" HorizontalAlignment="Left" />
            <RelativePanel>
                <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" HorizontalAlignment="Left" />
                <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0" RelativePanel.RightOf="City">
                    <!--List of valid states-->
                </ComboBox>
            </RelativePanel>
    </StackPanel>
    <StackPanel x:Name="Associate" Margin="20" RelativePanel.Below="Customer">
        <TextBox x:Name="AssociateName" Header= "Associate" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <DatePicker x:Name="TargetInstallDate" Header="Target install Date" HorizontalAlignment="Left" Margin="0,24,0,0"></DatePicker>
        <TimePicker x:Name="InstallTime" Header="Install Time" HorizontalAlignment="Left" Margin="0,24,0,0"></TimePicker>
    </StackPanel>
    <StackPanel x:Name="Save" Orientation="Horizontal" RelativePanel.Below="Associate">
        <Button Content="Save" Margin="24" />
        <Button Content="Cancel" Margin="24" />
    </StackPanel>
</RelativePanel>
```

N’hésitez pas à personnaliser vos contrôles avec davantage de propriétés pour obtenir une meilleure expérience visuelle.

## <a name="make-your-layout-responsive"></a>Rendez votre disposition dynamique

Les utilisateurs peuvent afficher votre interface utilisateur sur une grande variété d'appareils avec des largeurs d’écran différentes. Pour être certain de leur offrir une expérience optimale quel que soit leur écran, vous devez utiliser une [conception réactive](../design/layout/responsive-design.md). Lisez les conseils de cette page sur les philosophies de conception à garder à l'esprit en travaillant.

La page [Dispositions dynamiques avec XAML](../design/layout/layouts-with-xaml.md) offre une présentation détaillée de leur implémentation. Pour le moment, nous allons nous concentrer sur les **dispositions fluides** et les **états visuels dans XAML**.

L'aperçu de formulaire de base que nous avons créé constitue déjà une **disposition fluide**, car il dépend principalement de la position relative des contrôles et n'utilise qu'un minimum de positions et de tailles de pixels spécifiques. N’oubliez cependant pas ces conseils pour les interfaces utilisateur que vous créerez à l'avenir.

Les **états visuels** sont encore plus importants que les dispositions dynamiques. L'état visuel définit les valeurs de propriété qui sont appliquées à un élément donné lorsqu'une condition particulière est vraie. [Lisez comment procéder en XAML](../design/layout/layouts-with-xaml.md#set-visual-states-in-xaml-markup) et implémentez cette procédure dans votre formulaire. En voici un aperçu *très* basique dans notre exemple précédent:

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState>
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="640" />
            </VisualState.StateTriggers>

            <VisualState.Setters>
                <Setter Target="Associate.(RelativePanel.RightOf)" Value="Customer"/>
                <Setter Target="Associate.(RelativePanel.Below)" Value=""/>
                <Setter Target="Save.(RelativePanel.Below)" Value="Customer"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>

<RelativePanel>
    <!--Previous 3 stack panels-->
</RelativePanel>
```

Il n’est pas pratique de créer des états visuels pour un large éventail de tailles d’écran et, au-delà de deux, ils n'ont probablement pas d'impact significatif sur l’expérience utilisateur de votre application. Nous vous recommandons plutôt de concevoir une application pour plusieurs points d’arrêt principaux. Vous pouvez [en savoir plus ici](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md).

## <a name="add-accessibility-support"></a>Ajouter la prise en charge de l'accessibilité

Maintenant que vous avez obtenu une disposition bien conçue qui s'adapte aux changements de tailles d'écran, une dernière chose à faire pour améliorer l’expérience utilisateur consiste à [rendre votre application accessible](../design/accessibility/accessibility-overview.md). C'est un vaste programme, mais dans un formulaire tel que celui-ci, c'est plus simple qu’il n'y paraît. Concentrez-vous sur les éléments suivants:

* Prise en charge du clavier: vérifiez si l’ordre des éléments de vos panneaux d’interface utilisateur correspond à leur affichage à l’écran, de sorte qu'un utilisateur puisse facilement les parcourir à l'aide de la touche Tab.
* Prise en charge du lecteur d’écran: vérifiez que tous vos contrôles ont un nom descriptif.

Si vous créez des dispositions plus complexes avec plusieurs éléments visuels, vous pouvez consulter la [liste de vérification de l’accessibilité](../design/accessibility/accessibility-checklist.md) pour plus d’informations. Après tout, l'accessibilité n’est pas indispensable pour une application, mais elle permet d'atteindre et d'engager un plus large public.

## <a name="going-further"></a>Aller plus loin

Bien qu'il s'agisse ici de la création d'un formulaire, les concepts de dispositions et de contrôles s'appliquent à toutes les interfaces utilisateur XAML que vous pouvez créer. N’hésitez pas à passer en revue les documents, nous vous permettent de faire des essais avec le formulaire dont vous disposez, ajouter de nouvelles fonctionnalités de l’interface utilisateur et à améliorer davantage l’expérience utilisateur. Si vous souhaitez obtenir des instructions pas à pas sur les fonctionnalités de disposition plus détaillées, consultez notre [didacticiel de disposition adaptative](../design/basics/xaml-basics-adaptive-layout.md)

Les formulaires n'existent pas nécessairement en dehors de tout contexte: vous pouvez aller plus loin et incorporer le vôtre dans un [modèle Maître/Détails](../design/controls-and-patterns/master-details.md) ou un [contrôle pivot](../design/controls-and-patterns/tabs-pivot.md). Ou si vous souhaitez vous attaquer au code-behind de votre formulaire, vous pouvez commencer avec notre [vue d’ensemble des événements](../xaml-platform/events-and-routed-events-overview.md).

## <a name="useful-apis-and-docs"></a>API et documents utiles

Voici un résumé rapide des API et des autres documents utiles pour vous aider à commencer à utiliser la Liaison de données.

### <a name="useful-apis"></a>API utiles

| API | Description |
|------|---------------|
| [Contrôles utiles pour les formulaires](../design/controls-and-patterns/forms.md#input-controls) | Liste de contrôles de saisie utiles pour la création de formulaires et conseils de base sur les endroits où les utiliser. |
| [Grid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) | Panneau permettant d’organiser des éléments dans des dispositions constituées de plusieurs lignes et colonnes. |
| [RelativePanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) | Panneau pour organiser des éléments en relation avec d'autres éléments et jusqu'aux limites du panneau. |
| [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) | Panneau pour organiser des éléments dans une seule ligne horizontale ou verticale. |
| [VisualState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) | Vous permet de définir l’apparence des éléments d’interface utilisateur lorsqu'ils se trouvent dans des états particuliers. |

### <a name="useful-docs"></a>Documents utiles

| Sujet | Description |
|-------|----------------|
| [Vue d’ensemble de l’accessibilité](../design/accessibility/accessibility-overview.md) | Vue d’ensemble très générale des options d’accessibilité dans les applications. |
| [Liste de vérification de l’accessibilité](../design/accessibility/accessibility-checklist.md) | Liste de vérification pratique pour vous assurer que votre application est conforme aux normes d’accessibilité. |
| [Vue d’ensemble des événements](../xaml-platform/events-and-routed-events-overview.md) | Détails sur l'ajout et la configuration d'une structure des événements pour gérer des actions de l’interface utilisateur. |
| [Formulaires](../design/controls-and-patterns/forms.md) | Conseils généraux sur la création de formulaires. |
| [Panneaux de disposition](../design/layout/layout-panels.md) | Fournit une vue d’ensemble des types de panneaux de disposition et des endroits où les utiliser. |
| [Modèle Maître/Détails](../design/controls-and-patterns/master-details.md) | Modèle de conception qui peut être implémenté dans un ou plusieurs formulaires. |
| [Contrôle Pivot](../design/controls-and-patterns/tabs-pivot.md) | Contrôle qui peut contenir un ou plusieurs formulaires. |
| [Conception réactive](../design/layout/responsive-design.md) | Vue d’ensemble des principes généraux de la conception réactive. | 
| [Dispositions dynamiques avec XAML](../design/layout/layouts-with-xaml.md) | Informations spécifiques sur les états visuels et d’autres implémentations de conception réactive. |
| [Tailles d’écran pour une conception réactive](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) | Conseils sur les tailles d'écran à prendre en compte pour les dispositions dynamiques. |

## <a name="useful-code-samples"></a>Exemples de code utiles

| Exemple de code | Description |
|-----------------|---------------|
| [Didacticiel sur la disposition adaptative](../design/basics/xaml-basics-adaptive-layout.md) | Expérience interactive pas à pas sur les dispositions adaptatives et la conception réactive. | 
| [Base de données de commandes clients](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | Observez la disposition et les formulaires en action sur un exemple professionnel de plusieurs pages. |
| [Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) | Découvrez une sélection de contrôles XAML et la manière dont ils sont implémentés. |
| [Exemples de code supplémentaires](https://developer.microsoft.com//windows/samples) | Choisissez **Contrôles, disposition et texte** dans la liste déroulante des catégories pour voir des exemples de code connexes. |
