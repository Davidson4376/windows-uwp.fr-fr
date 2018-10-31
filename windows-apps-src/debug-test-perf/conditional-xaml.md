---
author: jwmsft
title: XAML conditionnel
description: Utiliser de nouvelles API dans le balisage XAML tout en conservant la compatibilité avec les versions précédentes
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 46954968f11f000025ee352676d3f0d17ecb9621
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5816429"
---
# <a name="conditional-xaml"></a>XAML conditionnel

Le *XAML conditionnel* permet d'utiliser la méthode [ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent) dans le balisage XAML. Cela vous permet de définir des propriétés et d'instancier des objets dans le balisage en fonction de la présence d’une API, sans avoir à utiliser le code-behind. Il analyse de façon sélective les éléments ou attributs afin de déterminer s'ils seront disponibles au moment de l’exécution. Les instructions conditionnelles sont évaluées au moment de l’exécution, et les éléments qualifiés associés à une baliseXAML conditionnelle sont analysés s’ils sont évalués comme **True** (vrais); sinon, ils sont ignorés.

Le XAML conditionnel est disponible à partir de CreatorsUpdate (version1703, build15063). Pour utiliser le XAML conditionnel, la version minimale de votre projet VisualStudio doit être définie sur la build15063 (CreatorsUpdate) ou version ultérieure, et la version cible doit être définie sur une version ultérieure à la version minimale. Voir [Applications adaptatives de version](version-adaptive-apps.md) pour plus d’informations sur la configuration de votre projet VisualStudio.

> [!NOTE]
> Pour créer une application adaptative de version avec une version minimale inférieure à la build15063, vous devez utiliser le [code adaptatif de version](version-adaptive-code.md), et non le XAML.

Pour obtenir des informations générales importantes sur ApiInformation et les contrats API, voir [Applications adaptatives de version](version-adaptive-apps.md).

## <a name="conditional-namespaces"></a>Espaces de noms conditionnels

Pour utiliser une méthode conditionnelle en XAML, vous devez d’abord déclarer un [espace de nomsXAML](../xaml-platform/xaml-namespaces-and-namespace-mapping.md) conditionnel en haut de votre page. Voici un exemple de pseudo-code d’un espace de noms conditionnel:

```xaml
xmlns:myNamespace="schema?conditionalMethod(parameter)"
```

Un espace de noms conditionnel peut être divisé en deux parties séparées par le délimiteur «?». 

- Le contenu qui précède le délimiteur indique l’espace de noms ou le schéma qui contient l’API référencée. 
- Le contenu qui suit le délimiteur «?» représente la méthode conditionnelle qui détermine si l’espace de noms conditionnel a la valeur **true** ou **false**.

Dans la plupart des cas, le schéma sera l’espace de noms XAML par défaut:

```xaml
xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
```

Le XAML conditionnel prend en charge les méthodes conditionnelles suivantes:

Méthode | Inverse
------ | -------
IsApiContractPresent(ContractName, VersionNumber) | IsApiContractNotPresent(ContractName, VersionNumber)
IsTypePresent(ControlType) | IsTypeNotPresent(ControlType)
IsPropertyPresent(ControlType, PropertyName) | IsPropertyNotPresent(ControlType, PropertyName)

Nous discuterons de ces méthodes plus en détail dans les sections suivantes de cet article.

> [!NOTE]
> Nous vous recommandons d’utiliser IsApiContractPresent et IsApiContractNotPresent. Les autres instructions conditionnelles ne sont pas intégralement prises en charge dans l’expérience de conception VisualStudio.

## <a name="create-a-namespace-and-set-a-property"></a>Créer un espace de noms et définir une propriété

Dans cet exemple, vous allez afficher «Bonjour, XAMLconditionnel» comme contenu d’un bloc de texte si l’application s’exécute sur FallCreatorsUpdate ou version ultérieure. Par défaut, aucun contenu ne sera affiché si l’application s’exécute sur une version précédente.

Tout d’abord, définissez un espace de noms personnalisé avec le préfixe «contract5Present» et utilisez l’espace de nomsXAML par défaut http://schemas.microsoft.com/winfx/2006/xaml/presentation)comme schéma contenant la propriété [TextBlock.Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.Text). Pour rendre cet espace de noms conditionnel, ajoutez le séparateur «?». après le schéma.

Définissez ensuite une instruction conditionnelle qui renvoie **true** sur les appareils exécutant FallCreatorsUpdate ou une version ultérieure Utilisez la méthode ApiInformation **IsApiContractPresent** pour vérifier la version5 de UniversalApiContract. La version5 de UniversalApiContract a été publiée avec FallCreatorsUpdate (SDK 16299).

```xaml
xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
```

Une fois l’espace de noms défini, ajoutez le préfixe d’espace de noms à la propriété Text de votre contrôle TextBox afin de la qualifier en tant que propriété devant être définie de manière conditionnelle lors de l’exécution.

```xaml
<TextBlock contract5Present:Text="Hello, Conditional XAML"/>
```

Voici le code XAML complet.

```xaml
<Page
    x:Class="ConditionalTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock contract5Present:Text="Hello, Conditional XAML"/>
    </Grid>
</Page>
```

Lorsque vous exécutez cet exemple sur FallCreatorsUpdate, le texte «Bonjour, XAML conditionnel» s’affiche. Lorsque vous l’exécutez sur CreatorsUpdate, aucun texte n’apparaît.

Le XAML conditionnel vous permet d’effectuer les vérifications API que vous pouvez faire sinon en code dans votre balisage. Voici le code équivalent pour cette vérification.

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    textBlock.Text = "Hello, Conditional XAML";
}
```

Notez que même si la méthode IsApiContractPresent utilise une chaîne pour le paramètre *contractName*, vous n'utilisez pas de guillemets («») dans la déclaration d’espace de noms XAML.

## <a name="use-ifelse-conditions"></a>Utiliser des conditions if/else

Dans l’exemple précédent, la propriété Text est définie uniquement lorsque l’application s’exécute sur FallCreatorsUpdate. Mais comment faire pour afficher du texte différent lorsqu’elle s’exécute sur CreatorsUpdate? Vous pouvez essayer de définir la propriété Text sans qualificateur conditionnel, comme suit.

```xaml
<!-- DO NOT USE -->
<TextBlock Text="Hello, World" contract5Present:Text="Hello, Conditional XAML"/>
```

Cela fonctionnera lorsqu’elle sera exécutée sur CreatorsUpdate. En revanche, lors d’une exécution sur FallCreatorsUpdate, vous recevrez un message d’erreur indiquant que la propriété Text est définie plusieurs fois.

Pour définir un texte différent lorsque l’application s’exécute sur différentes versions de Windows10, vous avez besoin d’une autre condition. Le XAML conditionnel fournit l'inverse de chaque méthode ApiInformation prise en charge pour vous permettre de créer des scénarios conditionnels if/else comme suit.

La méthode IsApiContractPresent renvoie **true** si l'appareil courant contient le contrat et le numéro de version spécifiés. Supposons, par exemple, que votre application est en cours d’exécution sur CreatorsUpdate, qui intègre la version4 du contrat API universel.

Plusieurs appels à IsApiContractPresent renverraient ces résultats:

- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 5) = **false**
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 4) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 3) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 2) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 1) = true.

IsApiContractNotPresent renvoie le résultat inverse de IsApiContractPresent. Les appels à IsApiContractPresent renverraient ces résultats:

- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 5) = **true**
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 4) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 3) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 2) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 1) = false

Pour utiliser la condition inverse, créez un deuxième espace de noms XAML conditionnel utilisant l’instruction conditionnelle **IsApiContractNotPresent**. Le préfixe «contract5NotPresent» est utilisé ici.

```xaml
xmlns:contract5NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)"

xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
```

Une fois les deux espaces de noms définis, vous pouvez définir deux fois la propriété Text à condition toutefois de les faire précéder de qualificateurs qui garantiront qu’un seul paramètre de propriété sera utilisé lors de l’exécution, comme suit:

```xaml
<TextBlock contract5NotPresent:Text="Hello, World"
           contract5Present:Text="Hello, Fall Creators Update"/>
```

Voici un autre exemple qui définit l’arrière-plan d’un bouton. La fonctionnalité [matériau Acrylique](../design/style/acrylic.md) n’est disponible qu’à partir de FallCreatorsUpdate. Vous ne pourrez donc utiliser un arrière-plan acrylique que si l’application s’exécute sur FallCreatorsUpdate. Comme l’arrière-plan acrylique n’est pas disponible sur les versions antérieures, définissez un arrière-plan rouge qui s’appliquera le cas échéant.

```xaml
<Button Content="Button"
        contract5NotPresent:Background="Red"
        contract5Present:Background="{ThemeResource SystemControlAcrylicElementBrush}"/>
```

## <a name="create-controls-and-bind-properties"></a>Créer des contrôles et lier des propriétés

Jusqu’ici, vous avez vu comment définir des propriétés à l’aide du XAML conditionnel, mais vous pouvez également instancier de manière conditionnelle des contrôles basés sur le contrat API disponible lors de l’exécution.

Ici, un contrôle [ColorPicker](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker) est instancié lorsque l’application s’exécute sur FallCreatorsUpdate où le contrôle est disponible. ColorPicker n’étant pas disponible avant FallCreatorsUpdate, utilisez une [zone de liste déroulante](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) lorsque l’application s’exécute sur des versions antérieures, afin de proposer des choix de couleurs simplifiés à l’utilisateur.

```xaml
<contract5Present:ColorPicker x:Name="colorPicker"
                              Grid.Column="1"
                              VerticalAlignment="Center"/>

<contract5NotPresent:ComboBox x:Name="colorComboBox"
                              PlaceholderText="Pick a color"
                              Grid.Column="1"
                              VerticalAlignment="Center">
```

Vous pouvez utiliser des qualificateurs conditionnels avec différentes formes de [syntaxe de propriété XAML](../xaml-platform/xaml-syntax-guide.md). Ici, la propriété Fill du rectangle est définie à l’aide de la syntaxe d’élément de propriété pour FallCreatorsUpdate et de la syntaxe d’attribut pour les versions précédentes.

```xaml
<Rectangle x:Name="colorRectangle" Width="200" Height="200"
           contract5NotPresent:Fill="{x:Bind ((SolidColorBrush)((FrameworkElement)colorComboBox.SelectedItem).Tag), Mode=OneWay}">
    <contract5Present:Rectangle.Fill>
        <SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
    </contract5Present:Rectangle.Fill>
</Rectangle>
```

Lorsque vous liez une propriété à une autre propriété qui dépend d’un espace de noms conditionnel, vous devez utiliser la même condition sur les deux propriétés. Ici, `colorPicker.Color` dépend de l’espace de noms conditionnel «contract5Present», vous devez donc également placer le préfixe «contract5Present» sur la propriété SolidColorBrush.Color. (Vous pouvez également placer le préfixe «contract5Present» sur SolidColorBrush et non sur la propriété Color). À défaut, vous obtiendrez une erreur de compilation.

```xaml
<SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
```

Voici le code XAML complet qui illustre ces scénarios. Cet exemple contient un rectangle et une interface utilisateur qui vous permet de définir la couleur du rectangle.

Lorsque l’application s’exécute sur FallCreatorsUpdate, vous utilisez un contrôle ColorPicker pour permettre à l’utilisateur de définir la couleur. ColorPicker n’étant pas disponible avant FallCreatorsUpdate, utilisez une zone de liste déroulante lorsque l’application s’exécute sur des versions antérieures, afin de proposer des choix de couleurs simplifiés à l’utilisateur.

```xaml
<Page
    x:Class="ConditionalTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
    xmlns:contract5NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <Rectangle x:Name="colorRectangle" Width="200" Height="200"
                   contract5NotPresent:Fill="{x:Bind ((SolidColorBrush)((FrameworkElement)colorComboBox.SelectedItem).Tag), Mode=OneWay}">
            <contract5Present:Rectangle.Fill>
                <SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
            </contract5Present:Rectangle.Fill>
        </Rectangle>

        <contract5Present:ColorPicker x:Name="colorPicker"
                                      Grid.Column="1"
                                      VerticalAlignment="Center"/>

        <contract5NotPresent:ComboBox x:Name="colorComboBox"
                                      PlaceholderText="Pick a color"
                                      Grid.Column="1"
                                      VerticalAlignment="Center">
            <ComboBoxItem>Red
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Red"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
            <ComboBoxItem>Blue
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Blue"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
            <ComboBoxItem>Green
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Green"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
        </contract5NotPresent:ComboBox>
    </Grid>
</Page>
```

## <a name="related-articles"></a>Articles connexes

- [Guide des applicationsUWP](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [Détection dynamique des fonctionnalités avec les contrats API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
- [Contrats API](https://channel9.msdn.com/Events/Build/2015/3-733) (Build2015 vidéo)