---
author: stevewhims
Description: Design your app to support the layouts and fonts of multiple languages, including RTL (right-to-left) flow direction.
title: Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.author: stwhi
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, uwp, adaptabilité, localisation, DàG, GàD
ms.localizationpriority: medium
ms.openlocfilehash: cf3a2d781dc916fbda9a9d6386dee4e2e6144873
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
ms.locfileid: "1673976"
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG

Concevez votre application pour prendre en charge les dispositions et les polices de plusieurs langues, notamment le sens du flux DàG (de droite à gauche). Le sens du flux désigne le sens dans lequel le script est rédigé et affiché, et le sens dans lequel les éléments de l'interface utilisateur de la page sont numérisés par l'œil.

## <a name="layout-guidelines"></a>Recommandations en matière de disposition

Certaines langues comme l'allemand et le finnois utilisent typiquement davantage de caractères que l'anglais. Les polices de l'Extrême-Orient nécessitent plus de hauteur. Et pour certaines langues, telles que l’arabe et l’hébreu, les volets de disposition et les éléments de texte doivent être dans le sens de lecture de droite à gauche (DàG).

En raison de la longueur variable du texte traduit, vous devez utiliser des mécanismes de disposition dynamiques d'interface utilisateur au lieu du positionnement absolu, des largeurs fixes ou des hauteurs fixes. La pseudo-localisation de votre application couvrira tous les cas de bord problématiques dans lesquels les éléments de l'interface utilisateur ne sont pas correctement dimensionnés par rapport au contenu.

Pour les langues DàG, utilisez la propriété [**FrameworkElement.FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) et définissez le remplissage symétrique et les marges. Les volets de disposition tels que [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid?branch=live) sont mis à l’échelle et pivotent automatiquement avec la valeur de **FlowDirection** que vous définissez.

Voici un exemple de possibilité d'exposition d'une propriété FlowDirection dans votre application en tant que ressource que les localisateurs peuvent définir correctement.

Tout d’abord, dans le fichier de ressources (.resw) de votre application, ajoutez un identificateur de propriété nommé «MainPage.FlowDirection» (au lieu de «MainPage», vous pouvez utiliser n’importe quel nom).

Puis, utilisez un **x:Uid** pour associer votre élément **Page** à cet identificateur de propriété.

```xaml
<Page x:Uid="MainPage">
```

Pour plus d’informations sur les fichiers de ressources (.resw), les identificateurs de propriété et **x:Uid**, consultez [Localiser les chaînes dans l'interface utilisateur et le manifeste du package d'application](../../app-resources/localize-strings-ui-manifest.md).

Vous devez éviter de définir des valeurs de disposition absolues sur n’importe quel élément d’interface utilisateur en fonction de langue. Mais si vous ne pouvez absolument pas l'éviter, vous pouvez créer un identificateur de propriété sous la forme «TitleText.Width».

```xaml
<TextBlock x:Uid="TitleText">
```

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

## <a name="best-practices-for-handling-right-to-left-rtl-languages"></a>Bonnes pratiques en matière de gestion de langues se lisant de droite à gauche (DàG)

Lorsque votre application est traduite dans des langues se lisant de droite à gauche (DàG), utilisez les API pour définir l’orientation de texte par défaut pour le volet de disposition racine de votre page. De cette façon, tous les contrôles contenus dans le volet racine répondent correctement à l’orientation de texte par défaut. Quand plusieurs langues sont prises en charge, utilisez l’élément `LayoutDirection` de la langue d'exécution de l'application pour définir la propriété [**FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) (consultez l'exemple de code ci-dessous). La plupart des contrôles inclus dans Windows utilisent déjà **FlowDirection**. Si vous implémentez des contrôles personnalisés, ils doivent utiliser **FlowDirection** pour apporter les modifications de disposition appropriées pour les langues DàG et GàD.

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

### <a name="rtl-faq"></a>FAQ sur les langues DàG 

**Q:** La propriété **FlowDirection** est-elle automatiquement définie en fonction de la langue sélectionnée? Par exemple, si la langue sélectionnée est l’anglais, le texte s’affiche-t-il de gauche à droite? Inversement, si l’arabe est sélectionné, s’affiche-t-il de droite à gauche?

> **R:** **FlowDirection** ne prend pas en compte la langue. Vous devez définir **FlowDirection** en fonction de la langue active. Consultez l’exemple de code ci-dessus.

**Q:** Je ne m'y connais pas vraiment en traduction. Les ressources contiennent-elles déjà le sens du déroulement? Est-il possible de déterminer le sens du déroulement à partir de la langue active?

> **R:** Si vous mettez en application les bonnes pratiques actuelles, alors les ressources ne contiennent pas directement le sens du déroulement. Vous devez déterminer le sens du déroulement de la langue active. Voici deux façons d’y remédier.
> 
> La méthode à privilégier consiste à utiliser l’élément **LayoutDirection** pour la langue préférée de façon à définir la propriété **FlowDirection** de l’élément RootFrame. Tous les contrôles de l’élément RootFrame héritent de la propriété FlowDirection de l’élément RootFrame.
> 
> L’autre méthode consiste à définir la propriété FlowDirection dans le fichier .resw pour les langues DàG que vous traduisez. Par exemple, vous pouvez avoir un fichier .resw arabe et un fichier .resw hébreu. Dans ces fichiers, vous pouvez utiliser x:UID pour définir la propriété FlowDirection. Cette méthode est cependant plus exposée aux erreurs que la méthode par programmation.

## <a name="important-apis"></a>API importantes

* [FrameworkElement.FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection)
* [LanguageFont](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live)

## <a name="related-topics"></a>Rubriquesconnexes

* [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](../../app-resources/localize-strings-ui-manifest.md)
* [Personnaliser vos ressources pour la langue, l’échelle et d’autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Comprendre les langues de profil utilisateur et les langues du manifeste de l’application](manage-language-and-region.md)