---
author: stevewhims
Description: Design your app to support the layouts and fonts of multiple languages, including RTL (right-to-left) flow direction.
title: Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.author: stwhi
ms.date: 05/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, uwp, adaptabilité, localisation, DàG, GàD
ms.localizationpriority: medium
ms.openlocfilehash: 52495740f76f59d5976e98d28147d05fca1e7a13
ms.sourcegitcommit: 834992ec14a8a34320c96e2e9b887a2be5477a53
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2018
ms.locfileid: "1880810"
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG
Concevez votre application pour prendre en charge les dispositions et les polices de plusieurs langues, notamment le sens du flux DàG (de droite à gauche). Le sens du flux désigne le sens dans lequel le script est rédigé et affiché, et le sens dans lequel les éléments de l'interface utilisateur de la page sont numérisés par l'œil.

## <a name="layout-guidelines"></a>Recommandations en matière de disposition
Certaines langues comme l'allemand et le finnois utilisent typiquement davantage de caractères que l'anglais. Les polices de l'Extrême-Orient nécessitent plus de hauteur. Et pour certaines langues, telles que l’arabe et l’hébreu, les volets de disposition et les éléments de texte doivent être dans le sens de lecture de droite à gauche (DàG).

En raison de ces variations des mesures du texte traduit, nous vous recommandons de ne pas intégrer de positionnement absolu, de largeurs ou de hauteurs fixes dans votre interface utilisateur (IU). Tirez plutôt parti des mécanismes de disposition dynamique qui sont intégrés dans les éléments d’interface utilisateur Windows. Par exemple, les contrôles de contenu (tels que les boutons), les contrôles d’éléments (par exemple, les affichages Liste et Grille) et les volets de disposition (tels que les grilles et les StackPanels) se redimensionnent automatiquement et s'ajustent dynamiquement par défaut en fonction de leur contenu. Pseudo-localisez votre application pour couvrir tous les cas de bord problématiques dans lesquels les éléments de l'interface utilisateur ne sont pas correctement dimensionnés par rapport au contenu.

La disposition dynamique est recommandée et vous pourrez l’utiliser dans la plupart des cas. Il est moins souhaitable, mais toujours préférable à l'intégration des tailles dans votre balisage d’interface utilisateur, de définir des valeurs de disposition en fonction de la langue. Voici un exemple de possibilité d'exposition d'une propriété Width dans votre application en tant que ressource que les localisateurs peuvent définir en fonction de la langue. Tout d’abord, dans le fichier de ressources (.resw) de votre application, ajoutez un identificateur de propriété nommé «TitleText.Width» (au lieu de «TitleText», vous pouvez utiliser n’importe quel nom). Puis, utilisez un **x:Uid** pour associer un ou plusieurs éléments d'interface utilisateur à cet identificateur de propriété.

```xaml
<TextBlock x:Uid="TitleText">
```

Pour plus d’informations sur les fichiers de ressources (.resw), les identificateurs de propriété et **x:Uid**, consultez [Localiser les chaînes dans l'interface utilisateur et le manifeste du package d'application](../../app-resources/localize-strings-ui-manifest.md).

## <a name="fonts"></a>Polices
Utilisez la classe de mappage de polices [**LanguageFont**](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live) pour permettre l’accès par programmation à la famille de polices et aux attributs de police (taille, épaisseur et style) recommandés pour une langue. La classe **LanguageFont** assure l’accès aux informations de police appropriées pour diverses catégories de contenu, notamment les en-têtes d’interface utilisateur, les notifications, le texte de corps et les polices de corps de document modifiables par l’utilisateur.

## <a name="mirroring-images"></a>Mise en miroir des images
Si votre application possède des images qui doivent être mises en miroir (c’est-à-dire qu’une même image peut être renversée) pour une disposition DàG, vous pouvez utiliser la propriété **FlowDirection**.

```xaml
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

Si votre application nécessite une image différente pour renverser correctement l'image, vous pouvez utiliser le système de gestion de ressource avec le qualificateur `LayoutDirection` (consultez la section [Adapter vos ressources pour la langue, l'échelle et d'autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md#layoutdirection)). Le système choisit une image nommée `file.layoutdir-rtl.png` lorsque la langue d'exécution de l'application (consultez [Comprendre les langues du profil utilisateur et les langues du manifeste de l'application](manage-language-and-region.md)) est définie sur une langue DàG. Cette approche peut s’avérer nécessaire lorsqu’une certaine partie de l’image est renversée, alors qu’une autre partie ne l’est pas.

## <a name="handling-right-to-left-rtl-languages"></a>Gestion des langues se lisant de droite à gauche (DàG)
Lorsque votre application est traduite dans des langues se lisant de droite à gauche (DàG), utilisez la propriété [**FrameworkElement.FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) et définissez le remplissage symétrique et les marges. Les volets de disposition tels que [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid?branch=live) sont mis à l’échelle et pivotent automatiquement avec la valeur de **FlowDirection** que vous définissez.

Définissez **FlowDirection** sur le volet (ou cadre) de disposition racine de votre Page, ou sur la Page elle-même. Ainsi, tous les contrôles situés à l'intérieur héritent de cette propriété.

> [!IMPORTANT]
> Toutefois, **FlowDirection** n'est *pas* défini automatiquement en fonction de la langue d’affichage sélectionnée par l’utilisateur dans les paramètres Windows; ni ne change dynamiquement en réponse à un changement de langue d’affichage par l'utilisateur. Si, par exemple, l’utilisateur passe les paramètres Windows de l’anglais à l'arabe, la propriété **FlowDirection** de gauche à droite ne sera *pas* automatiquement remplacée par de droite à gauche. En tant que développeur de l'application, vous devez définir **FlowDirection** en fonction de la langue active.

La technique de programmation consiste à utiliser la propriété `LayoutDirection` de la langue d’affichage préférée de l'utilisateur pour définir la propriété [**FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) (voir l’exemple de code ci-dessous). La plupart des contrôles inclus dans Windows utilisent déjà **FlowDirection**. Si vous implémentez un contrôle personnalisé, il doit utiliser **FlowDirection** pour apporter les modifications de disposition appropriées pour les langues DàG et GàD.

```csharp    
this.languageTag = Windows.Globalization.ApplicationLanguages.Languages[0];

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

var flowDirectionSetting = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["LayoutDirection"];
if (flowDirectionSetting == "LTR")
{
    this.layoutRoot.FlowDirection = Windows.UI.Xaml.FlowDirection.LeftToRight;
}
else
{
    this.layoutRoot.FlowDirection = Windows.UI.Xaml.FlowDirection.RightToLeft;
}
```

```cppwinrt
#include <winrt/Windows.ApplicationModel.Resources.Core.h>
#include <winrt/Windows.Globalization.h>
...

m_languageTag = Windows::Globalization::ApplicationLanguages::Languages().GetAt(0);

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

auto flowDirectionSetting = Windows::ApplicationModel::Resources::Core::ResourceContext::GetForCurrentView().QualifierValues().Lookup(L"LayoutDirection");
if (flowDirectionSetting == L"LTR")
{
    layoutRoot().FlowDirection(Windows::UI::Xaml::FlowDirection::LeftToRight);
}
else
{
    layoutRoot().FlowDirection(Windows::UI::Xaml::FlowDirection::RightToLeft);
}
```

```cpp
this->languageTag = Windows::Globalization::ApplicationLanguages::Languages->GetAt(0);

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

auto flowDirectionSetting = Windows::ApplicationModel::Resources::Core::ResourceContext::GetForCurrentView()->QualifierValues->Lookup("LayoutDirection");
if (flowDirectionSetting == "LTR")
{
    this->layoutRoot->FlowDirection = Windows::UI::Xaml::FlowDirection::LeftToRight;
}
else
{
    this->layoutRoot->FlowDirection = Windows::UI::Xaml::FlowDirection::RightToLeft;
}
```

La technique ci-dessus fait de **FlowDirection** une fonction de la propriété `LayoutDirection` de la langue d’affichage préférée de l'utilisateur. Si, pour une raison quelconque, vous ne souhaitez pas cette logique, vous pouvez exposer une propriété FlowDirection dans votre application en tant que ressource que les traducteurs peuvent définir en fonction de chaque langue de traduction.

Tout d’abord, dans le fichier de ressources (.resw) de votre application, ajoutez un identificateur de propriété nommé «MainPage.FlowDirection» (au lieu de «MainPage», vous pouvez utiliser n’importe quel nom). Puis, utilisez un **x:Uid** pour associer votre élément **Page** principal (ou son volet ou cadre de disposition racine) à cet identificateur de propriété.

```xaml
<Page x:Uid="MainPage">
```

Au lieu d’une seule ligne de code pour toutes les langues, le code dépend du traducteur qui «traduit» cette ressource de propriété en fonction de chaque langue de traduction. Par conséquent, n’oubliez pas que l'utilisation de cette technique est susceptible d'entraîner des erreurs humaines.

## <a name="important-apis"></a>API importantes
* [FrameworkElement.FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection)
* [LanguageFont](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live)

## <a name="related-topics"></a>Rubriquesconnexes
* [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](../../app-resources/localize-strings-ui-manifest.md)
* [Personnaliser vos ressources pour la langue, l’échelle et d’autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Comprendre les langues de profil utilisateur et les langues du manifeste de l’application](manage-language-and-region.md)