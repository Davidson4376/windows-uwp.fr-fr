---
author: jwmsft
description: Explique comment définir et implémenter des propriétés de dépendance personnalisées pour une application Windows Runtime en C++, C# ou Visual Basic.
title: Propriétés de dépendance personnalisées
ms.assetid: 5ADF7935-F2CF-4BB6-B1A5-F535C2ED8EF8
ms.author: jimwalk
ms.date: 07/12/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 3b52c801ffcf1cfe916e41cfd102a37397fadfee
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5980307"
---
# <a name="custom-dependency-properties"></a>Propriétés de dépendance personnalisées

Nous expliquons ici comment définir et implémenter vos propres propriétés de dépendance pour une application Windows Runtime en C++, C# ou Visual Basic. Nous listons ici les raisons pour lesquelles les développeurs et les auteurs de composants peuvent souhaiter créer des propriétés de dépendance personnalisées. Nous décrivons les étapes d’implémentation de propriété de dépendance personnalisée et certaines meilleures pratiques susceptibles d’améliorer les performances, la simplicité d’utilisation ou la polyvalence de la propriété de dépendance.

## <a name="prerequisites"></a>Prérequis

Nous supposons que vous avez lu la [vue d’ensemble des propriétés de dépendance](dependency-properties-overview.md) et que vous comprenez ce que sont les propriétés de dépendance du point de vue d’un consommateur de propriétés de dépendance existantes. Pour suivre les exemples de cette rubrique, vous devez également comprendre le langage XAML et savoir comment écrire une application Windows Runtime de base en C++, C# ou Visual Basic.

## <a name="what-is-a-dependency-property"></a>Qu’est-ce qu’une propriété de dépendance?

Pour prendre en charge des styles, des liaison de données, des animations et des valeurs de propriété par défaut, vous devez mettre en œuvre une propriété de dépendance. Les valeurs de propriété de dépendance ne sont pas conservées en tant que champs sur la classe, mais sont stockées par l’infrastructure xaml. Elles sont référencées à l’aide d’une clé extraite lors de l’inscription de la propriété auprès du système de propriétés WindowsRuntime en appelant la méthode [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829).   Les propriétés de dépendance peuvent être utilisées uniquement par les types dérivés de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). Toutefois, **DependencyObject** étant relativement haut dans la hiérarchie de classes, la plupart des classes destinées à la prise en charge de l’interface utilisateur et de la présentation peuvent prendre en charge des propriétés de dépendance. Pour plus d’informations sur les propriétés de dépendance et certains termes et conventions utilisés pour les décrire dans cette documentation, voir [Vue d’ensemble des propriétés de dépendance](dependency-properties-overview.md).

[**Control.Background**](https://msdn.microsoft.com/library/windows/apps/br209395), [**FrameworkElement.Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) et [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/br209702) sont des exemples de propriétés de dépendance Windows Runtime.

Par convention, chaque propriété de dépendance exposée par une classe possède une propriété **public static readonly** correspondante de type [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362), qui est exposée sur cette même classe qui fournit l’identificateur de la propriété de dépendance. Le nom de l’identificateur respecte la convention suivante: le nom de la propriété de dépendance, avec la chaîne «Property» ajoutée à la fin du nom. Par exemple, l’identificateur **DependencyProperty** correspondant pour la propriété **Control.Background** est [**Control.BackgroundProperty**](https://msdn.microsoft.com/library/windows/apps/br209396). L’identificateur stocke les informations sur la propriété de dépendance dès qu’elle a été inscrite et peut ensuite être utilisé pour d’autres opérations impliquant la propriété de dépendance, telles que l’appel de [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361).

## <a name="property-wrappers"></a>Wrappers de propriétés

Les propriétés de dépendance ont en général une implémentation de wrapper. Sans le wrapper, le seul moyen d’obtenir ou de définir les propriétés consisterait à utiliser les méthodes d’utilitaire de propriété de dépendance [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) et [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) et à leur passer l’identificateur comme paramètre. Il s’agirait d’une utilisation plutôt contre nature pour un objet qui n’est somme toute qu’une propriété. Mais avec le wrapper, votre code et tout autre code qui fait référence à la propriété de dépendance peut utiliser une syntaxe de propriété-objet simple et naturelle pour le langage que vous utilisez.

Si vous implémentez une propriété de dépendance personnalisée vous-même et que vous souhaitez qu’elle soit publique et simple à appeler, définissez également les wrappers de propriété. Les wrappers de propriété sont également utiles pour fournir des informations de base concernant la propriété de dépendance aux processus d’analyse statique ou de réflexion. Plus spécifiquement, le wrapper est l’endroit où l’on place des attributs tels que [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011).

## <a name="when-to-implement-a-property-as-a-dependency-property"></a>Quand implémenter une propriété en tant que propriété de dépendance ?

Quand vous implémentez une propriété publique en lecture/écriture sur une classe, tant que votre classe dérive de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356), vous pouvez faire en sorte que votre propriété fonctionne comme propriété de dépendance. Parfois, la technique par défaut consistant à seconder votre propriété à l’aide d’un champ privé est adéquate. La définition de votre propriété personnalisée en tant que propriété de dépendance n’est pas toujours nécessaire ou convenable. Le choix dépendra des scénarios que votre propriété doit prendre en charge.

Il peut être souhaitable d’implémenter une propriété comme propriété de dépendance si vous voulez qu’elle prenne en charge une ou plusieurs de ces fonctionnalités Windows Runtime ou des applications Windows Runtime :

- définition de la propriété par le biais d’un [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849);
- fonctionnement en tant que propriété cible valide pour la liaison de données à l’aide de [**{Binding}**](binding-markup-extension.md);
- prise en charge de valeurs animées par le biais d’un [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490);
- avertissement en cas de modification de la valeur de la propriété par:
  - des actions exécutées par le système de propriétés lui-même;
  - l’environnement ;
  - des actions de l’utilisateur ;
  - la lecture et l’écriture de styles.

## <a name="checklist-for-defining-a-dependency-property"></a>Liste de vérification pour la définition d’une propriété de dépendance

La définition d’une propriété de dépendance peut être envisagée d’un point de vue conceptuel. Ces concepts ne sont pas nécessairement des étapes procédurales, car plusieurs concepts peuvent être abordés sur une même ligne de code dans l’implémentation. La liste suivante constitue simplement une vue d’ensemble. Vous trouverez plus loin dans cette rubrique une explication plus détaillée de chacun des concepts, ainsi que des exemples de code dans plusieurs langages.

- Inscrivez le nom de la propriété auprès du système de propriétés (appelez [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)), en spécifiant un type de propriétaire et le type de la valeur de propriété.
  - Il existe un paramètre obligatoire pour [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) qui attend des métadonnées de propriétés. Spécifiez la valeur **null** ou, si vous souhaitez disposer d’un comportement de modification de propriété ou d’une valeur par défaut basée sur des métadonnées qui peut être restaurée en appelant [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357), spécifiez une instance de [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.propertymetadata).
- Définissez un identificateur [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) en tant que membre de propriété **public static readonly** sur le type de propriétaire.
- Définissez une propriété wrapper, en respectant le modèle d’accesseur de propriété utilisé dans le langage que vous implémentez. Le nom de la propriété wrapper doit correspondre à la chaîne *name* que vous avez utilisée dans [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829). Implémentez les accesseurs**get** et **set** pour connecter le wrapper à la propriété de dépendance qu’il enveloppe en appelant [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) et [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) et en passant l’identificateur de votre propre propriété en tant que paramètre.
- (Facultatif) Placez des attributs tels que [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011) sur le wrapper.

> [!NOTE]
> Si vous définissez une propriété jointe personnalisée, on omet en général le wrapper. Au lieu de cela, on écrit un style d’accesseur différent utilisable par un processeur XAML. Voir [Propriétés jointes personnalisées](custom-attached-properties.md). 

## <a name="registering-the-property"></a>Inscription de la propriété

Pour que votre propriété soit une propriété de dépendance, vous devez l’inscrire dans une banque de propriétés conservée par le système de propriétés Windows Runtime.  Pour inscrire la propriété, vous devez appeler la méthode [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829).

Pour les langages Microsoft .NET (C# et Microsoft Visual Basic), vous devez appeler [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) dans le corps de votre classe (à l’intérieur de la classe, mais à l’extérieur des définitions de membres). L’identificateur est fourni par l’appel de la méthode [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829), en tant que valeur de retour. L’appel [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) est généralement effectué en tant que constructeur statique ou dans le cadre de l’initialisation d’une propriété **public static readonly** de type [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) en tant que partie intégrante de votre classe. Cette propriété expose l’identificateur de votre propriété de dépendance. Voici quelques exemples d’appel [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829).

> [!NOTE]
> Inscription de la propriété de dépendance dans le cadre de l’identificateur de définition de la propriété est l’implémentation par défaut, mais vous pouvez aussi inscrire une propriété de dépendance dans le constructeur statique de classe. Cette approche est logique si vous avez besoin de plusieurs lignes de code pour initialiser la propriété de dépendance.

Pour C++ / CX, vous disposez des options pour le fractionnement de l’implémentation entre l’en-tête et le fichier de code. Le fractionnement par défaut consiste à déclarer l’identificateur proprement dit en tant que propriété **public static** dans l’en-tête, avec une implémentation **get** mais sans **set**. L’implémentation **get** fait référence à un champ privé, qui est une instance [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) non initialisée. Vous pouvez aussi déclarer les wrappers et les implémentations **get** et **set** du wrapper. Dans ce cas, l’en-tête comprend une implémentation minimale. Si le wrapper a besoin d’une attribution Windows Runtime, attribuez également l’en-tête. Placez l’appel à la propriété [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) dans le fichier de code, au sein d’une fonction d’assistance qui s’exécute uniquement lorsque l’application est initialisée pour la première fois. Utilisez la valeur de retour de la méthode **Register** pour remplir les identificateurs statiques mais non initialisés que vous avez déclarés dans l’en-tête, auquel vous avez initialement attribué la valeur **nullptr** au niveau de l’étendue racine du fichier d’implémentation.

```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(String),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null)
);
```

```vb
Public Shared ReadOnly LabelProperty As DependencyProperty = 
    DependencyProperty.Register("Label", 
      GetType(String), 
      GetType(ImageWithLabelControl), 
      New PropertyMetadata(Nothing))
```

```cppwinrt
// ImageWithLabelControl.idl
namespace ImageWithLabelControlApp
{
    runtimeclass ImageWithLabelControl : Windows.UI.Xaml.Controls.Control
    {
        ImageWithLabelControl();
        static Windows.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}

// ImageWithLabelControl.h
...
private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
...

// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ nullptr }
);
...
```

```cpp
//.h file
//using namespace Windows::UI::Xaml::Controls;
//using namespace Windows::UI::Xaml::Interop;
//using namespace Windows::UI::Xaml;
//using namespace Platform;

public ref class ImageWithLabelControl sealed : public Control
{
private:
    static DependencyProperty^ _LabelProperty;
...
public:
    static void RegisterDependencyProperties();
    static property DependencyProperty^ LabelProperty
    {
        DependencyProperty^ get() {return _LabelProperty;}
    }
...
};

//.cpp file
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml.Interop;

DependencyProperty^ ImageWithLabelControl::_LabelProperty = nullptr;

// This function is called from the App constructor in App.xaml.cpp
// to register the properties
void ImageWithLabelControl::RegisterDependencyProperties()
{ 
    if (_LabelProperty == nullptr)
    { 
        _LabelProperty = DependencyProperty::Register(
          "Label", Platform::String::typeid, ImageWithLabelControl::typeid, nullptr);
    } 
}
```

> [!NOTE]
> C++ / CX de code, la raison pour laquelle la raison pour laquelle vous disposez d’un champ privé et une propriété publique en lecture seule qui couvre les [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) est afin qu’autres appelants qui utilisent votre propriété de dépendance puissent également utiliser l’utilitaire de système de propriétés API qui nécessitent le identificateur soit public. Si l’identificateur demeure privé, personne ne pourra utiliser ces API d’utilitaire. Parmi ces API et scénarios, on peut citer [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) ou [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) par choix, [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357), [**GetAnimationBaseValue**](https://msdn.microsoft.com/library/windows/apps/br242358), [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257) et [**Setter.Property**](https://msdn.microsoft.com/library/windows/apps/br208836). Vous ne pouvez pas utiliser de champ public pour cela, car les règles de métadonnées Windows Runtime n’autorisent pas les champs publics.

## <a name="dependency-property-name-conventions"></a>Conventions d’affectation de noms des propriétés de dépendance

Il existe des conventions d’affectation de noms pour les propriétés de dépendance; elles doivent être respectées en permanence, sauf cas exceptionnel. La propriété de dépendance proprement dite a un nom de base (« Label » dans l’exemple précédent) qui est donné comme premier paramètre de [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829). Ce nom doit être unique dans chaque type d’inscription et l’exigence du caractère unique s’applique également à tout membre hérité. Les propriétés de dépendance héritées par le biais de types de base sont considérées comme faisant déjà partie du type d’inscription; les noms des propriétés héritées ne peuvent pas être inscrits de nouveau.

> [!WARNING]
> Bien que le nom fourni qu'ici peut être tout identificateur de chaîne qui est valide en programmation pour la langue de votre choix, vous souhaitez généralement être en mesure de définir la propriété de dépendance en XAML également. Pour être défini en XAML, le nom de propriété que vous choisissez doit être un nom XAML valide. Pour plus d’informations, voir [Vue d’ensemble du langage XAML](xaml-overview.md).

Lors de la création de la propriété identificatrice, combinez le nom de la propriété telle que vous l’avez inscrite avec le suffixe «Property» («LabelProperty», par exemple). Cette propriété est votre identificateur pour la propriété de dépendance et elle est utilisée comme entrée pour les appels [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) et [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) que vous effectuez dans vos propres wrappers de propriétés. Elle est aussi utilisée par le système de propriétés et par d’autres processeurs XAML tels que [**{x:Bind}**](x-bind-markup-extension.md).

## <a name="implementing-the-wrapper"></a>Implémentation du wrapper

Votre wrapper de propriété doit appeler [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) dans l’implémentation **get** et [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) dans l’implémentation **set**.

> [!WARNING]
> Dans de rares cas hormis exceptionnels, vos implémentations de wrappers doivent effectuer uniquement les opérations [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) et [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) . Sinon, vous obtiendrez un comportement différent lorsque votre propriété sera définie par le biais de XAML et par le biais de code. Pour plus d’efficacité, l’analyseur XAML contourne les wrappers lors de la définition des propriétés de dépendance, et échange avec le magasin de stockage à l’aide de **SetValue**.

```csharp
public String Label
{
    get { return (String)GetValue(LabelProperty); }
    set { SetValue(LabelProperty, value); }
}
```

```vb
Public Property Label() As String
    Get
        Return DirectCast(GetValue(LabelProperty), String) 
    End Get 
    Set(ByVal value As String)
        SetValue(LabelProperty, value)
    End Set
End Property
```

```cppwinrt
// ImageWithLabelControl.h
...
winrt::hstring Label()
{
    return winrt::unbox_value<winrt::hstring>(GetValue(m_labelProperty));
}

void Label(winrt::hstring const& value)
{
    SetValue(m_labelProperty, winrt::box_value(value));
}
...
```

```cpp
//using namespace Platform;
public:
...
  property String^ Label
  {
    String^ get() {
      return (String^)GetValue(LabelProperty);
    }
    void set(String^ value) {
      SetValue(LabelProperty, value);
    }
  }
```

## <a name="property-metadata-for-a-custom-dependency-property"></a>Métadonnées de propriété pour une propriété de dépendance personnalisée

Lorsque des métadonnées de propriété sont assignées à une propriété de dépendance, les mêmes métadonnées sont appliquées à cette propriété pour chaque instance du type de propriétaire de propriété ou ses sous-classes. Dans les métadonnées de propriété, vous pouvez spécifier deux comportements:

- une valeur par défaut que le système de propriétés assigne à tous les cas de la propriété;
- une méthode de rappel statique qui est appelée automatiquement dans le système de propriétés chaque fois qu’un changement de valeur de propriété est détecté.

### <a name="calling-register-with-property-metadata"></a>Appel du Registre à l’aide de métadonnées de propriété

Dans les exemples précédents d’appel de [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829), nous avons transmis une valeur Null pour le paramètre *propertyMetadata*. Pour permettre à une propriété de dépendance de fournir une valeur par défaut ou d’utiliser un rappel de modification de propriété, vous devez définir une instance de [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) qui fournit l’une et/ou l’autre de ces fonctionnalités.

En règle générale, vous fournissez [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) en tant qu’instance créée inline dans les paramètres de [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829).

> [!NOTE]
> Si vous définissez une implémentation [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) , vous devez utiliser la méthode d’utilitaire [**PropertyMetadata.Create**](https://msdn.microsoft.com/library/windows/apps/hh702099) plutôt que d’appeler un constructeur [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) pour définir l’instance **PropertyMetadata** .

L’exemple suivant modifie les exemples de [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) présentés auparavant en référençant une instance de [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) avec une valeur [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770). L’implémentation du rappel «OnLabelChanged» est décrite plus loin dans cette section.

```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(String),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null,new PropertyChangedCallback(OnLabelChanged))
);
```

```vb
Public Shared ReadOnly LabelProperty As DependencyProperty =
    DependencyProperty.Register("Label",
      GetType(String),
      GetType(ImageWithLabelControl),
      New PropertyMetadata(
        Nothing, new PropertyChangedCallback(AddressOf OnLabelChanged)))
```

```cppwinrt
// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ nullptr, Windows::UI::Xaml::PropertyChangedCallback{ &ImageWithLabelControl::OnLabelChanged } }
);
...
```

```cpp
DependencyProperty^ ImageWithLabelControl::_LabelProperty =
    DependencyProperty::Register("Label",
    Platform::String::typeid,
    ImageWithLabelControl::typeid,
    ref new PropertyMetadata(nullptr,
      ref new PropertyChangedCallback(&ImageWithLabelControl::OnLabelChanged))
    );
```

### <a name="default-value"></a>Valeur par défaut

Vous pouvez spécifier une valeur par défaut pour une propriété de dépendance afin que la propriété retourne toujours une valeur par défaut particulière quand sa définition est annulée. Cette valeur peut être différente de la valeur par défaut inhérente au type de cette propriété.

Si aucune valeur par défaut n’est spécifiée, la valeur par défaut d’une propriété de dépendance est null pour un type de référence ou a la valeur par défaut du type pour un type de valeur ou une primitive de langage (par exemple, 0 pour un entier ou une chaîne vide pour une chaîne). La principale raison pour laquelle on définit une valeur par défaut est le fait que cette valeur est restaurée quand vous appelez [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357) sur la propriété. Il peut être plus commode de définir une valeur par défaut sur la base de chaque propriété que de définir des valeurs par défaut dans des constructeurs, en particulier pour les types de valeurs. Toutefois, pour les types de référence, assurez-vous que la définition d’une valeur par défaut n’entraîne pas la création accidentelle d’un modèle de singleton. Pour plus d’informations, voir [Meilleures pratiques](#best-practices) plus loin dans cette rubrique.

```cppwinrt
// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Windows::UI::Xaml::PropertyChangedCallback{ &ImageWithLabelControl::OnLabelChanged } }
);
...
```

> [!NOTE]
> N’effectuez pas une inscription avec la valeur par défaut [**UnsetValue**](https://msdn.microsoft.com/library/windows/apps/br242371). Cela prêterait à confusion pour les consommateurs de propriété et aurait des conséquences inattendues dans le système de propriétés.

### <a name="createdefaultvaluecallback"></a>CreateDefaultValueCallback

Dans certains scénarios, vous définissez des propriétés de dépendance pour les objets qui sont utilisés sur plusieurs threads d’interface utilisateur. Cela peut être le cas si vous définissez un objet de données utilisé par plusieurs applications ou un contrôle que vous utilisez dans plusieurs applications. Vous pouvez activer l’échange de l’objet entre différents threads d’interface utilisateur en fournissant une implémentation de [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) à la place d’une instance de valeur par défaut, qui est liée au thread ayant inscrit la propriété. Fondamentalement, [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) définit une fabrique pour les valeurs par défaut. La valeur retournée par **CreateDefaultValueCallback** est toujours associée au thread **CreateDefaultValueCallback** de l’interface utilisateur actuelle, qui utilise l’objet.

Pour définir les métadonnées qui spécifient [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812), vous devez appeler [**PropertyMetadata.Create**](https://msdn.microsoft.com/library/windows/apps/hh702115) afin de retourner une instance de métadonnées ; les constructeurs [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) n’ont pas de signature qui comporte un paramètre **CreateDefaultValueCallback**.

Le modèle d’implémentation classique de [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) consiste à créer une classe [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356), à définir la valeur de propriété spécifique de chaque propriété de **DependencyObject** en fonction de la valeur par défaut appropriée, puis à retourner la nouvelle classe en tant que référence **Object** via la valeur de retour de la méthode **CreateDefaultValueCallback**.

### <a name="property-changed-callback-method"></a>Méthode de rappel de modification de propriété

Vous pouvez définir une méthode de rappel de modification de propriété pour définir les interactions de votre propriété avec d’autres propriétés de dépendance ou pour mettre à jour un état ou une propriété interne de votre objet chaque fois que la propriété change. Si votre rappel est effectué, le système de propriétés a déterminé qu’il existe un changement de valeur de propriété. La méthode de rappel étant statique, le paramètre *d* du rappel est important car il indique l’instance de la classe qui a signalé le changement. Une implémentation par défaut utilise la propriété [**NewValue**](https://msdn.microsoft.com/library/windows/apps/br242364) des données d’événement et traite cette valeur d’une certaine manière, généralement en apportant une autre modification à l’objet transmis comme *d*. Il existe d’autres réponses à une modification de propriété, par exemple le rejet de la valeur signalée par **NewValue**, la restauration de [**OldValue**](https://msdn.microsoft.com/library/windows/apps/br242365) ou la définition de la valeur sur une contrainte de programmation appliquée à **NewValue**.

L’exemple suivant illustre une implémentation [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770). Il implémente la méthode référencée dans les exemples [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) précédents, dans le cadre des arguments de construction pour [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771). Le scénario géré par ce rappel implique que la classe possède également une propriété en lecture seule calculée nommée «HasLabelValue» (implémentation non illustrée). Chaque fois que la propriété «Label» est réévaluée, cette méthode de rappel est appelée et le rappel permet à la valeur calculée dépendante de rester synchronisée avec les modifications apportées à la propriété de dépendance.

```csharp
private static void OnLabelChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    ImageWithLabelControl iwlc = d as ImageWithLabelControl; //null checks omitted
    String s = e.NewValue as String; //null checks omitted
    if (s == String.Empty)
    {
        iwlc.HasLabelValue = false;
    } else {
        iwlc.HasLabelValue = true;
    }
}
```

```vb
    Private Shared Sub OnLabelChanged(d As DependencyObject, e As DependencyPropertyChangedEventArgs)
        Dim iwlc As ImageWithLabelControl = CType(d, ImageWithLabelControl) ' null checks omitted
        Dim s As String = CType(e.NewValue,String) ' null checks omitted
        If s Is String.Empty Then
            iwlc.HasLabelValue = False
        Else
            iwlc.HasLabelValue = True
        End If
    End Sub
```

```cppwinrt
void ImageWithLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    auto iwlc{ d.as<ImageWithLabelControlApp::ImageWithLabelControl>() };
    auto s{ winrt::unbox_value<winrt::hstring>(e.NewValue()) };
    iwlc.HasLabelValue(s.size() != 0);
}
```

```cpp
static void OnLabelChanged(DependencyObject^ d, DependencyPropertyChangedEventArgs^ e)
{
    ImageWithLabelControl^ iwlc = (ImageWithLabelControl^)d;
    Platform::String^ s = (Platform::String^)(e->NewValue);
    if (s->IsEmpty()) {
        iwlc->HasLabelValue=false;
    }
}
```

### <a name="property-changed-behavior-for-structures-and-enumerations"></a>Comportement des modifications de propriétés pour les structures et les énumérations

Si le type de [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) est une énumération ou une structure, le rappel peut être invoqué même si les valeurs internes de la structure ou la valeur d’énumération n’ont pas changé. Cela diffère d’un système primitif tel qu’une chaîne, où il est uniquement invoqué si la valeur a changé. Il s’agit d’une conséquence des opérations boxing et unboxing sur ces valeurs en interne. Si vous avez une méthode [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770) pour une propriété où votre valeur est une énumération ou une structure, vous devez comparer [**OldValue**](https://msdn.microsoft.com/library/windows/apps/br242365) et [**NewValue**](https://msdn.microsoft.com/library/windows/apps/br242364) en effectuant vous-même une conversion de type (transtypage) des valeurs et en utilisant les opérateurs de comparaison surchargés disponibles pour les valeurs de transtypage. À défaut, si aucun opérateur n’est disponible (ce qui peut être le cas pour une structure personnalisée), vous devrez peut-être comparer les valeurs individuelles. En principe, ne faites rien si les valeurs n’ont pas changé au final.

```csharp
private static void OnVisibilityValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    if ((Visibility)e.NewValue != (Visibility)e.OldValue)
    {
        //value really changed, invoke your changed logic here
    } // else this was invoked because of boxing, do nothing
}
```

```vb
Private Shared Sub OnVisibilityValueChanged(d As DependencyObject, e As DependencyPropertyChangedEventArgs)
    If CType(e.NewValue,Visibility) != CType(e.OldValue,Visibility) Then
        '  value really changed, invoke your changed logic here
    End If
    '  else this was invoked because of boxing, do nothing
End Sub
```

```cppwinrt
static void OnVisibilityValueChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    auto oldVisibility{ winrt::unbox_value<Windows::UI::Xaml::Visibility>(e.OldValue()) };
    auto newVisibility{ winrt::unbox_value<Windows::UI::Xaml::Visibility>(e.NewValue()) };

    if (newVisibility != oldVisibility)
    {
        // The value really changed; invoke your property-changed logic here.
    }
    // Otherwise, OnVisibilityValueChanged was invoked because of boxing; do nothing.
}
```

```cpp
static void OnVisibilityValueChanged(DependencyObject^ d, DependencyPropertyChangedEventArgs^ e)
{
    if ((Visibility)e->NewValue != (Visibility)e->OldValue)
    {
        //value really changed, invoke your changed logic here
    } 
    // else this was invoked because of boxing, do nothing
    }
}
```

## <a name="best-practices"></a>Meilleures pratiques

Lors de la définition de votre propriété de dépendance, veillez à respecter les recommandations suivantes.

### <a name="dependencyobject-and-threading"></a>DependencyObject et threads

Toutes les instances de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) doivent être créées sur le thread d’interface utilisateur associé au [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) actuel qui est affiché par une application Windows Runtime. Bien qu’il soit indispensable de créer chaque **DependencyObject** sur le thread d’interface utilisateur principal, les objets sont accessibles à l’aide d’une référence de répartiteur en provenance des autres threads, via l’appel de [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br230616).

Les aspects relatifs aux threads de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) sont importants, car en règle générale, seul le code qui s’exécute sur le thread d’interface utilisateur peut modifier ou même lire la valeur d’une propriété de dépendance. Les problèmes de threads peuvent généralement être évités dans le code d’interface utilisateur classique qui utilise correctement les modèles **async** et les threads de travail d’arrière-plan. En règle générale, vous rencontrez des problèmes de threads relatifs à **DependencyObject** uniquement si vous définissez vos propres types **DependencyObject** et tentez de les utiliser pour des sources de données ou d’autres scénarios avec lesquels **DependencyObject** n’est pas nécessairement approprié.

### <a name="avoiding-unintentional-singletons"></a>Éviter les singletons accidentels

Il existe un risque de création accidentelle de singleton si vous déclarez une propriété de dépendance qui prend un type de référence et que vous appelez un constructeur pour ce type de référence dans le cadre du code qui établit votre [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771). Toutes les utilisations de la propriété de dépendance partagent une seule instance de **PropertyMetadata** et essayent ainsi de partager le type de référence unique que vous avez construit. Toute sous-propriété de ce type de valeur que vous définissez par le biais de votre propriété de dépendance est alors propagée à d’autres objets d’une manière que vous n’aviez probablement pas prévue.

Vous pouvez utiliser des constructeurs de classe pour définir des valeurs initiales pour une propriété de dépendance de type référence si vous voulez une valeur non-nulle, mais sachez que cela serait considéré comme une valeur locale pour les besoins de la [vue d’ensemble des propriétés de dépendance](dependency-properties-overview.md). Il peut être préférable d’utiliser un modèle, si votre classe les prend en charge. Une autre manière d’éviter les modèles de singleton tout en fournissant une valeur par défaut utile consiste à exposer une propriété statique sur le type de référence qui fournit une valeur par défaut convenable pour les valeurs de cette classe.

### <a name="collection-type-dependency-properties"></a>Propriétés de dépendance de type collection

Avec les propriétés de dépendance de type collection, certains aspects supplémentaires liés à l’implémentation doivent être pris en compte.

Les propriétés de dépendance de type collection sont relativement rares dans l’API Windows Runtime. Dans la plupart des cas, vous pouvez utiliser des collections dans lesquelles les éléments sont une sous-classe [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356), mais où la propriété de collection proprement dite est implémentée sous forme de propriété CLR ou C++ conventionnelle. Cela est dû au fait que les collections ne conviennent pas nécessairement à certains scénarios classiques où des propriétés de dépendance entrent en jeu. Par exemple :

- Vous n’animez généralement pas une collection.
- Vous ne préremplissez généralement pas les éléments d’une collection avec des styles ou un modèle.
- Bien que la liaison à des collections soit un scénario majeur, il n’est pas obligatoire qu’une collection soit une propriété de dépendance pour être une source de liaison. Pour les cibles de liaison, il est plus courant d’utiliser des sous-classes de [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/br242803) ou [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348) pour prendre en charge les éléments de collection ou pour utiliser des modèles d’affichage. Pour plus d’informations sur la liaison vers et à partir de collections, voir [Présentation détaillée de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt210946).
- Il est préférable de gérer les notifications de modification de collection par le biais d’interfaces telles que **INotifyPropertyChanged** ou **INotifyCollectionChanged**, ou en dérivant le type de collection de [**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/ms668604.aspx).

Néanmoins, il existe certains scénarios impliquant des propriétés de dépendance de type collection. Les trois sections qui suivent fournissent quelques recommandations quant à la manière d’implémenter une propriété de dépendance de type collection.

### <a name="initializing-the-collection"></a>Initialisation de la collection

Lorsque vous créez une propriété de dépendance, vous pouvez établir une valeur par défaut au moyen de métadonnées de propriété de dépendance. Mais prenez soin de ne pas utiliser de collection statique de singletons comme valeur par défaut. Au lieu de cela, vous devez affecter comme valeur de collection une collection (d’instance) unique dans le cadre de la logique de constructeur de classe pour la classe propriétaire de la propriété de collection.

### <a name="change-notifications"></a>Notifications de modifications

Le fait de définir la collection en tant que propriété de dépendance ne procure pas automatiquement de notification de modification pour les éléments de la collection grâce à l’appel de la méthode de rappel «PropertyChanged» par le système de propriétés. Si vous voulez obtenir des notifications pour des collections ou des éléments de collection (par exemple pour un scénario de liaison de données), implémentez l’interface **INotifyPropertyChanged** ou **INotifyCollectionChanged**. Pour plus d’informations, voir [Présentation détaillée de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt210946).

### <a name="dependency-property-security-considerations"></a>Considérations relatives à la sécurité des propriétés de dépendance

Déclarez les propriétés de dépendance comme propriétés publiques. Déclarez les identificateurs de propriétés de dépendance comme membres **public static readonly**. Même si vous essayez de déclarer d’autres niveaux d’accès autorisés par un langage (tels que **protected**), une propriété de dépendance est toujours accessible par le biais de l’identificateur combiné aux API du système de propriétés. La déclaration de l’identificateur de propriété de dépendance comme interne ou privé ne fonctionne pas, car dans ce cas le système ne peut pas opérer correctement.

Les propriétés wrappers ne sont là qu’à titre de commodité. Les mécanismes de sécurité appliqués aux wrappers peuvent être contournés en appelant [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) ou [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361). Il faut donc que les propriétés wrappers soient publiques, sinon vous ne faites que rendre votre propriété plus difficile à utiliser pour les appelants légitimes sans apporter de réel avantage en matière de sécurité.

Windows Runtime ne permet pas d’inscrire une propriété de dépendance personnalisée en lecture seule.

### <a name="dependency-properties-and-class-constructors"></a>Propriétés de dépendance et constructeurs de classe

Il existe un principe général qui veut que les constructeurs de classe ne doivent pas appeler de méthodes virtuelles. Cela est dû au fait que les constructeurs peuvent être appelés pour accomplir une initialisation de base d’un constructeur de classe dérivé et que l’entrée dans la méthode virtuelle par le biais du constructeur peut se produire quand l’instance d’objet en construction n’est pas encore complètement initialisée. Quand vous dérivez d’une classe quelconque qui dérive déjà de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356), souvenez-vous que le système de propriétés lui-même appelle et expose des méthodes virtuelles en interne dans le cadre de ses services. Pour éviter tout problème d’initialisation au moment de l’exécution, ne définissez pas de valeurs de propriétés de dépendance dans des constructeurs de classes.

### <a name="registering-the-dependency-properties-for-ccx-apps"></a>Inscription des propriétés de dépendance pour les applications C++/CX

L’inscription d’une propriété en C++/CX est plus compliquée à implémenter qu’en C#, non seulement en raison de la séparation en-tête/fichier d’implémentation, mais aussi parce que l’initialisation au niveau de l’étendue racine du fichier d’implémentation est une pratique déconseillée. (Les extensions de composant Visual c++ (C++ / CX) injecte du code de l’initialiseur statique à partir de l’étendue racine directement dans **DllMain**, tandis que les compilateurs c# affectent les initialiseurs statiques à classes, donc éviter les problèmes de verrouillage de charge **DllMain** .). Dans le cas présent, la meilleure pratique consiste à déclarer une fonction d’assistance qui se charge de toutes les inscriptions de vos propriétés de dépendance pour une classe, une fonction par classe. Ensuite, pour chaque classe personnalisée que votre application consomme, vous devez faire référence à la fonction d’inscription d’assistance qui est exposée par chaque classe personnalisée que vous souhaitez utiliser. Appelez chaque fonction d’inscription d’assistance dans le cadre de la méthode [**Application constructor**](https://msdn.microsoft.com/library/windows/apps/br242325) (`App::App()`), avant `InitializeComponent`. Ce constructeur s’exécute uniquement lorsque l’application est vraiment référencée pour la première fois (ainsi, il ne s’exécute pas une nouvelle fois lors de la reprise d’une application suspendue par exemple). Par ailleurs, comme vous pouvez le voir dans l’exemple d’inscription précédent en C++, la vérification **nullptr** autour de chaque appel [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) est importante, car elle garantit qu’un appelant de la fonction ne peut inscrire la propriété deux fois. Sans une telle vérification, un deuxième appel d’inscription entraînerait probablement le blocage de votre application en raison de la duplication du nom de la propriété. Ce modèle d’implémentation est présenté dans l’[Exemple de contrôles personnalisés et utilisateur XAML](http://go.microsoft.com/fwlink/p/?linkid=238581) (examinez le code correspondant à la version C++/CX de l’exemple).

## <a name="related-topics"></a>Rubriques connexes

- [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)
- [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)
- [Vue d’ensemble des propriétés de dépendance](dependency-properties-overview.md)
- [Exemple de contrôles personnalisés et utilisateur XAML](http://go.microsoft.com/fwlink/p/?linkid=238581)
 