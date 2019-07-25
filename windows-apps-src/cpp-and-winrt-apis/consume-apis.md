---
description: Cette rubrique montre comment utiliser des API C++/WinRT, qu’elles soient implémentées par Windows, un fournisseur de composants tiers ou par vous-même.
title: Utiliser des API avec C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projeté, projection, implémentation, classe runtime, activation
ms.localizationpriority: medium
ms.openlocfilehash: 5a3d4b554fafeb2053e4e6af831c224b5eacd151
ms.sourcegitcommit: ba4a046793be85fe9b80901c9ce30df30fc541f9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68328871"
---
# <a name="consume-apis-with-cwinrt"></a>Utiliser des API avec C++/WinRT

Cette rubrique montre comment utiliser des API [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), qu’elles fassent partie de Windows ou qu’elles soient implémentées par un fournisseur de composants tiers ou par vous-même.

## <a name="if-the-api-is-in-a-windows-namespace"></a>Si l’API se trouve dans un espace de noms Windows
Il s'agit du scénario le plus courant dans lequel vous utiliserez une API Windows Runtime. Pour chaque type dans un espace de noms Windows défini dans les métadonnées, C++/WinRT définit un équivalent compatible C++ (appelé le *type projeté*). Le type projeté a le même nom complet que le type Windows, mais il est placé dans l'espace de noms C++ **winrt** à l’aide de la syntaxe C++. Par exemple, [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) est projeté en C++/WinRT sous la forme **winrt::Windows::Foundation::Uri**.

Voici un exemple de code simple. Si vous souhaitez copier-coller les exemples de code suivants directement dans le fichier de code source principal d’un projet **Application console Windows (C++/WinRT)** , définissez d’abord **Sans utiliser les en-têtes précompilés** dans les propriétés du projet.

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };
    Uri combinedUri = contosoUri.CombineUri(L"products");
}
```

L’en-tête `winrt/Windows.Foundation.h` inclus fait partie du SDK, présent dans le dossier `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\`. Les en-têtes dans ce dossier contiennent des types d'espace de noms Windows projetés en C++/WinRT. Dans cet exemple, `winrt/Windows.Foundation.h` contient **winrt::Windows::Foundation::Uri** qui est le type projeté de la classe runtime [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri).

> [!TIP]
> Chaque fois que vous souhaitez utiliser un type à partir d’un espace de noms Windows, incluez l’en-tête C++/WinRT correspondant à cet espace de noms. Les directives `using namespace` sont facultatives, mais bien pratiques.

Dans l’exemple de code ci-dessus, après l’initialisation de C++/WinRT, nous empilons-allouons une valeur du type projeté **winrt::Windows::Foundation::Uri** via l’un de ses constructeurs publiquement documentés ([**Uri(String)** ](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_), dans cet exemple). Pour celui-ci, le cas le plus courant, c’est généralement tout que vous avez à faire. Une fois que vous avez une valeur du type projeté C++/WinRT, vous pouvez la traiter comme s’il s’agissait d’une instance du type Windows Runtime réel, car il a les mêmes membres.

En fait, cette valeur projetée est un proxy ; il s'agit essentiellement d'un pointeur intelligent vers un objet de sauvegarde. Le ou les constructeurs de la valeur projetée appellent [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) pour créer une instance de la classe Windows Runtime de sauvegarde (**Windows.Foundation.Uri** dans le cas présent) et stocker l'interface par défaut de cet objet à l’intérieur de la nouvelle valeur projetée. Comme illustré ci-dessous, vos appels aux membres de la valeur projetée sont en réalité délégués, par le biais du pointeur intelligent, à l’objet de sauvegarde ; dans lequel les changements d’état se produisent.

![Le type projeté Windows::Foundation::Uri](images/uri.png)

Lorsque la valeur `contosoUri` se trouve hors de portée, elle se détruit et publie sa référence à l’interface par défaut. Si cette référence est la dernière référence à l'objet Windows Runtime **Windows.Foundation.Uri** de sauvegarde, l'objet de sauvegarde se détruit également.

> [!TIP]
> Un *type projeté* est un wrapper sur une classe runtime, à des fins d’utilisation de ses API. Une *interface projetée* est un wrapper sur une interface Windows Runtime.

## <a name="cwinrt-projection-headers"></a>En-têtes de projection C++/WinRT
Pour utiliser les API d'espace de noms Windows de C++/WinRT, vous incluez les en-têtes du dossier `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. Il est courant qu'un type dans un espace de noms subordonné référence des types dans son espace de noms parent immédiat. Par conséquent, chaque en-tête de projection C++/WinRT inclut automatiquement son fichier d’en-tête d'espace de noms parent ; vous n'avez donc pas *besoin* de l'inclure explicitement. Toutefois, si vous le faites, aucune erreur n’est générée.

Par exemple, pour l'espace de noms [**Windows::Security::Cryptography::Certificates**](/uwp/api/windows.security.cryptography.certificates), les définitions de type C++/WinRT équivalentes se trouvent dans `winrt/Windows.Security.Cryptography.Certificates.h`. Les types présents dans **Windows::Security::Cryptography::Certificates** requièrent des types dans l'espace de noms **Windows::Security::Cryptography** parent ; et les types présents dans cet espace de noms peuvent nécessiter des types dans son propre parent, **Windows::Security**.

Par conséquent, lorsque vous incluez `winrt/Windows.Security.Cryptography.Certificates.h`, ce fichier inclut à son tour `winrt/Windows.Security.Cryptography.h` et `winrt/Windows.Security.Cryptography.h` inclut `winrt/Windows.Security.h`. C'est là que la piste s’arrête, car il n'existe aucun `winrt/Windows.h`. Ce processus d’inclusion transitive s’arrête à l’espace de noms de second niveau.

Ce processus inclut de manière transitive les fichiers d’en-tête qui fournissent les *déclarations* et *implémentations* nécessaires pour les classes définies dans les espaces de noms parents.

Un membre d’un type dans un espace de noms peut faire référence à un ou plusieurs types dans d'autres espaces de noms indépendants. Pour compiler les définitions de ces membres avec succès, le compilateur doit voir les déclarations de type pour la fermeture de tous ces types. Par conséquent, chaque en-tête de projection C++/WinRT inclut les en-têtes d’espace de noms nécessaires pour *déclarer* tous les types dépendants. À la différence des espaces de noms parents, ce processus ne *pas* extraire les *implémentations* pour les types référencés.

> [!IMPORTANT]
> Lorsque vous souhaitez effectivement *utiliser* un type (instancier, appeler des méthodes, etc.) déclaré dans un espace de noms indépendant, vous devez inclure le fichier d’en-tête d'espace de noms approprié pour ce type. Seules les *déclarations*, et non les *implémentations*, sont automatiquement incluses.

Par exemple, si vous incluez uniquement `winrt/Windows.Security.Cryptography.Certificates.h`, cela entraîne l'extraction des déclarations à partir de ces espaces de noms (et ainsi de suite, de manière transitive).

- Windows.Foundation
- Windows.Foundation.Collections
- Windows.Networking
- Windows.Storage.Streams
- Windows.Security.Cryptography

En d’autres termes, certaines API sont déclarées en avance dans un en-tête que vous avez inclus. Mais leurs définitions se trouvent dans un en-tête que vous n’avez pas encore inclus. Par conséquent, si vous appelez ensuite [**Windows::Foundation::Uri::RawUri**](/uwp/api/windows.foundation.uri.rawuri), vous recevrez une erreur d'éditeur de liens indiquant que le membre n’est pas défini. La solution consiste à explicitement `#include <winrt/Windows.Foundation.h>`. En règle générale, lorsque vous voyez une telle erreur d'éditeur de liens, incluez l’en-tête nommé pour l’espace de noms de l’API et régénérez.

## <a name="accessing-members-via-the-object-via-an-interface-or-via-the-abi"></a>Accès aux membres via l’objet, via une interface ou via l’ABI
Avec la projection C++/WinRT, la représentation d’exécution d’une classe Windows Runtime se limite aux interfaces ABI sous-jacentes. Toutefois, pour votre commodité, vous pouvez coder en fonction des classes comme prévu par leur auteur. Par exemple, vous pouvez appeler la méthode **ToString** d’un [**Uri**](/uwp/api/windows.foundation.uri) comme s’il s’agissait d’une méthode de la classe (il s'agit en réalité d'une méthode sur l'interface **IStringable** séparée).

`WINRT_ASSERT` est une définition de macro, qui se développe en [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
Uri contosoUri{ L"http://www.contoso.com" };
WINRT_ASSERT(contosoUri.ToString() == L"http://www.contoso.com/"); // QueryInterface is called at this point.
```

Cette commodité est obtenue via une requête pour l’interface appropriée. Mais vous gardez toujours le contrôle. Vous pouvez choisir d'abandonner un peu de cet aspect pratique pour y gagner un peu en performances en récupérant l’interface IStringable vous-même et en l'utilisant directement. Dans l’exemple de code ci-dessous, vous obtenez un pointeur d’interface IStringable réel en cours d’exécution (via une requête à usage unique). Après cela, votre appel à **ToString** est direct et permet d’éviter tout autre appel à **QueryInterface**.

```cppwinrt
...
IStringable stringable = contosoUri; // One-off QueryInterface.
WINRT_ASSERT(stringable.ToString() == L"http://www.contoso.com/");
```

Vous pouvez choisir cette technique si vous savez que vous allez appeler plusieurs méthodes sur la même interface.

Par ailleurs, si vous souhaitez accéder aux membres du niveau ABI, vous le pouvez. L’exemple de code ci-dessous montre comment procéder et vous trouverez plus de détails et d’exemples de code dans [Interopérabilité entre C++/WinRT et ABI](interop-winrt-abi.md).

```cppwinrt
#include <Windows.Foundation.h>
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
using namespace winrt::Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };

    int port{ contosoUri.Port() }; // Access the Port "property" accessor via C++/WinRT.

    winrt::com_ptr<ABI::Windows::Foundation::IUriRuntimeClass> abiUri{
        contosoUri.as<ABI::Windows::Foundation::IUriRuntimeClass>() };
    HRESULT hr = abiUri->get_Port(&port); // Access the get_Port ABI function.
}
```

## <a name="delayed-initialization"></a>Initialisation différée

Dans C++/WinRT, chaque type projeté possède un constructeur **std::nullptr_t** C++/WinRT spécial. À l’exception de ce dernier, tous les constructeurs de type projeté, y compris le constructeur par défaut, entraînent la création d’un objet Windows Runtime de sauvegarde et vous donnent un pointeur intelligent vers celui-ci. Par conséquent, cette règle s’applique à tous les emplacements où le constructeur par défaut est utilisé. Cela inclut notamment les variables locales non initialisées, les variables globales non initialisées et les variables de membre non initialisées.

En revanche, si vous souhaitez créer une variable d’un type projeté sans qu’il construise à son tour un objet Windows Runtime de sauvegarde (de sorte que vous pouvez reporter cette tâche à plus tard), vous le pouvez. Déclarez votre variable ou votre champ à l’aide du constructeur **std::nullptr_t** C++/WinRT spécial (que la projection C++/WinRT injecte dans chaque classe runtime). Nous utilisons ce constructeur spécial avec *m_gamerPicBuffer* dans l’exemple de code ci-dessous.

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>
using namespace winrt::Windows::Storage::Streams;

#define MAX_IMAGE_SIZE 1024

struct Sample
{
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer m_gamerPicBuffer{ nullptr };
};

int main()
{
    winrt::init_apartment();
    Sample s;
    // ...
    s.DelayedInit();
}
```

Tous les constructeurs du type projeté, *sauf* le constructeur **std::nullptr_t**, entraînent la création d’un objet Windows Runtime de stockage. Le constructeur **std::nullptr_t** est essentiellement un no-op. Il attend que l’objet projeté soit initialisé à un moment ultérieur. Par conséquent, qu'une classe runtime ait un constructeur par défaut ou non, vous pouvez utiliser cette technique pour obtenir une initialisation différée efficace.

Cette considération affecte d'autres endroits où vous invoquez le constructeur par défaut, comme dans les vecteurs et les cartes. Prenons cet exemple de code, pour lequel vous aurez besoin d'un projet **Blank App (C++/WinRT)** .

```cppwinrt
std::map<int, TextBlock> lookup;
lookup[2] = value;
```

L’assignation crée un nouveau **TextBlock** et le remplace aussitôt par `value`. Voici la solution.

```cppwinrt
std::map<int, TextBlock> lookup;
lookup.insert_or_assign(2, value);
```

Consultez également [Comment le constructeur par défaut affecte-t-il les collections ?](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx#how-the-default-constructor-affects-collections).

### <a name="dont-delay-initialize-by-mistake"></a>Ne pas retarder l’initialisation par erreur

Veillez à ne pas appeler le constructeur **std::nullptr_t** par erreur. La fonctionnalité de résolution de conflits du compilateur le rend prioritaire face aux constructeurs de fabrique. Par exemple, prenons deux définitions de classe runtime.

```idl
// GiftBox.idl
runtimeclass GiftBox
{
    GiftBox();
}

// Gift.idl
runtimeclass Gift
{
    Gift(GiftBox giftBox); // You can create a gift inside a box.
}
```

Nous souhaitons créer un **Gift** qui ne se trouve pas dans une boîte (un **Gift** construit avec un **GiftBox** non initialisé). Commençons par examiner la *mauvaise* façon de procéder. Nous savons qu’il existe un constructeur **Gift** qui accepte un **GiftBox**. Mais si nous sommes tentés de passer un **GiftBox** ayant une valeur null (en appelant le constructeur du **Gift** via une initialisation uniforme, comme nous le faisons ci-dessous), nous n’allons *pas* obtenir le résultat souhaité.

```cppwinrt
// These are *not* what you intended. Doing it in one of these two ways
// actually *doesn't* create the intended backing Windows Runtime Gift object;
// only an empty smart pointer.

Gift gift{ nullptr };
auto gift{ Gift(nullptr) };
```

Vous obtenez ici un **Gift** non initialisé. Vous n’obtenez pas un **Gift** avec un **GiftBox** non initialisé. Voici la *bonne* façon de procéder.

```cppwinrt
// Doing it in one of these two ways creates an initialized
// Gift with an uninitialized GiftBox.

Gift gift{ GiftBox{ nullptr } };
auto gift{ Gift(GiftBox{ nullptr }) };
```

Dans l’exemple incorrect, le passage d’un littéral `nullptr` est résolu en faveur du constructeur à initialisation tardive. Pour permettre une résolution en faveur du constructeur de fabrique, le type du paramètre doit être celui d’un **GiftBox**. Vous pouvez toujours passer un **GiftBox** à initialisation tardive de manière explicite, comme indiqué dans le bon exemple.

L’exemple suivant est *également* correct, car le paramètre a le type GiftBox et non **std::nullptr_t**.

```cppwinrt
GiftBox giftBox{ nullptr };
Gift gift{ giftBox }; // Calls factory constructor.
```

L’ambiguïté se pose uniquement quand vous passez un littéral `nullptr`.

## <a name="dont-copy-construct-by-mistake"></a>N’utilisez pas de constructeur de copie par erreur.

Cette mise en garde est similaire à celle décrite dans la section [Ne pas retarder l’initialisation par erreur](#dont-delay-initialize-by-mistake) ci-dessus.

En plus du constructeur à initialisation tardive, la projection C++/WinRT injecte également un constructeur de copie dans chaque classe runtime. Il s’agit d’un constructeur à un seul paramètre qui accepte le même type que l’objet construit. Le pointeur intelligent résultant pointe vers le même objet Windows Runtime de stockage que celui vers lequel pointe son paramètre de constructeur. Il en résulte deux objets pointeurs intelligents qui pointent vers le même objet de stockage.

Voici une définition de classe runtime que nous allons utiliser dans les exemples de code.

```idl
// GiftBox.idl
runtimeclass GiftBox
{
    GiftBox(GiftBox biggerBox); // You can place a box inside a bigger box.
}
```

Supposons que nous souhaitions construire un **GiftBox** dans un **GiftBox** plus grand.

```cppwinrt
GiftBox bigBox{ ... };

// These are *not* what you intended. Doing it in one of these two ways
// copies bigBox's backing-object-pointer into smallBox.
// The result is that smallBox == bigBox.

GiftBox smallBox{ bigBox };
auto smallBox{ GiftBox(bigBox) };
```

La *bonne* façon de procéder consiste à appeler explicitement la fabrique d’activation.

```cppwinrt
GiftBox bigBox{ ... };

// These two ways call the activation factory explicitly.

GiftBox smallBox{
    winrt::get_activation_factory<GiftBox, IGiftBoxFactory>().CreateInstance(bigBox) };
auto smallBox{
    winrt::get_activation_factory<GiftBox, IGiftBoxFactory>().CreateInstance(bigBox) };
```

## <a name="if-the-api-is-implemented-in-a-windows-runtime-component"></a>Si l’API est implémentée dans un composant Windows Runtime
Cette section s’applique, que vous ayez créé le composant vous-même ou qu’il provienne d’un fournisseur.

> [!NOTE]
> Pour plus d’informations sur l’installation et l’utilisation de l’extension VSIX (Visual Studio Extension) C++/WinRT et du package NuGet (qui fournissent ensemble la prise en charge des modèles et des builds de projet), consultez [Prise en charge de Visual Studio pour C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Dans votre projet d’application, référencez le fichier de métadonnées Windows Runtime (`.winmd`) du composant Windows Runtime et lancez le processus de génération. Lors de la génération, l’outil `cppwinrt.exe` génère une bibliothèque C++ standard qui décrit en détail &mdash; ou *projette* &mdash; la surface d’API pour le composant. En d’autres termes, la bibliothèque générée contient les types projetés pour le composant.

Ensuite, comme pour un type d’espace de noms Windows, incluez un en-tête et construisez le type projeté via l’un de ses constructeurs. Le code de démarrage de votre projet d’application inscrit la classe runtime, et le constructeur du type projeté appelle [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) pour activer la classe runtime à partir du composant référencé.

```cppwinrt
#include <winrt/BankAccountWRC.h>

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount bankAccount;
    ...
};
```

Pour obtenir plus d’informations, du code ainsi que la procédure à suivre pour utiliser des API implémentées dans un composant Windows Runtime, voir [Créer des événements en C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component).

## <a name="if-the-api-is-implemented-in-the-consuming-project"></a>Si l’API est implémentée dans le projet d’utilisation
Un type qui est utilisé à partir de l’interface utilisateur XAML doit être une classe runtime, même s’il est dans le même projet que le code XAML.

Pour ce scénario, vous allez générer un type projeté à partir des métadonnées Windows Runtime de la classe runtime (`.winmd`). Là encore, vous incluez un en-tête, mais cette fois vous construisez le type projeté via son constructeur **std::nullptr_t**. Ce constructeur n’effectuant aucune initialisation, vous devez ensuite affecter une valeur à l’instance via la fonction d’assistance [**winrt::make**](/uwp/cpp-ref-for-winrt/make), en transmettant tous les arguments constructeur nécessaires. Une classe runtime implémentée dans le même projet que le code d’utilisation n’a pas besoin d’être inscrite, ni instanciée via l’activation de Windows Runtime/COM.

Vous aurez besoin d’un projet **Blank App (C++/WinRT)** pour cet exemple de code.

```cppwinrt
// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
    ...
    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
        ...
    };
}
...
// MainPage.cpp
...
#include "BookstoreViewModel.h"

MainPage::MainPage()
{
    m_mainViewModel = winrt::make<Bookstore::implementation::BookstoreViewModel>();
    ...
}
```

Pour obtenir plus d’informations, du code et la procédure d’utilisation d’une classe runtime implémentée dans le projet d’utilisation, voir [Contrôles XAML ; liaison à une propriété C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage).

## <a name="instantiating-and-returning-projected-types-and-interfaces"></a>Instanciation et retour des types et interfaces projetés
Voici un exemple de ce à quoi peuvent ressembler les types et interfaces projetés dans votre projet d’utilisation. Rappelez-vous qu'un type projeté (tel que celui de cet exemple), est généré par un outil, et n'est pas un élément que vous auriez créé vous-même.

```cppwinrt
struct MyRuntimeClass : MyProject::IMyRuntimeClass, impl::require<MyRuntimeClass,
    Windows::Foundation::IStringable, Windows::Foundation::IClosable>
```

**MyRuntimeClass** est un type projeté ; les interfaces projetées incluent **IMyRuntimeClass**, **IStringable** et **IClosable**. Cette rubrique vous a montré les différentes façons dont vous pouvez instancier un type projeté. Voici un résumé et un rappel utilisant **MyRuntimeClass** comme exemple.

```cppwinrt
// The runtime class is implemented in another compilation unit (it's either a Windows API,
// or it's implemented in a second- or third-party component).
MyProject::MyRuntimeClass myrc1;

// The runtime class is implemented in the same compilation unit.
MyProject::MyRuntimeClass myrc2{ nullptr };
myrc2 = winrt::make<MyProject::implementation::MyRuntimeClass>();
```

- Vous pouvez accéder aux membres de toutes les interfaces d’un type projeté.
- Vous pouvez retourner un type projeté à un appelant.
- Les types et interfaces projetés dérivent de [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown). Par conséquent, vous pouvez appeler [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) sur un type ou une interface projetés pour rechercher d’autres interfaces projetées, que vous pouvez également utiliser ou retourner à un appelant. La fonction membre **as** fonctionne comme [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)).

```cppwinrt
void f(MyProject::MyRuntimeClass const& myrc)
{
    myrc.ToString();
    myrc.Close();
    IClosable iclosable = myrc.as<IClosable>();
    iclosable.Close();
}
```

## <a name="activation-factories"></a>Fabriques d'activation
Une manière pratique et directe de créer un objet C++/WinRT se présente comme suit.

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
CurrencyFormatter currency{ L"USD" };
```

Mais vous devez parfois créer la fabrique d'activation vous-même, puis créer des objets à partir de celle-ci à votre convenance. Voici quelques exemples de la manière de procéder, en utilisant le modèle de fonction [**winrt::get_activation_factory**](/uwp/cpp-ref-for-winrt/get-activation-factory).

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
auto factory = winrt::get_activation_factory<CurrencyFormatter, ICurrencyFormatterFactory>();
CurrencyFormatter currency = factory.CreateCurrencyFormatterCode(L"USD");
```

```cppwinrt
using namespace winrt::Windows::Foundation;
...
auto factory = winrt::get_activation_factory<Uri, IUriRuntimeClassFactory>();
Uri account = factory.CreateUri(L"http://www.contoso.com");
```

Les classes dans les deux exemples ci-dessus sont des types provenant d’un espace de noms Windows. Dans l’exemple suivant, **BankAccountWRC::BankAccount** est un type personnalisé implémenté dans un composant Windows Runtime.

```cppwinrt
auto factory = winrt::get_activation_factory<BankAccountWRC::BankAccount>();
BankAccountWRC::BankAccount account = factory.ActivateInstance<BankAccountWRC::BankAccount>();
```

## <a name="membertype-ambiguities"></a>Ambiguïtés membre/type

Lorsqu’une fonction membre a le même nom qu’un type, il y a une ambiguïté. Pour C++, la recherche de noms non qualifiés dans les fonctions membres est effectuée dans la classe avant les espaces de noms en raison des règles appliquées. La règle *Substitution failure is not an error* (L’échec de substitution n’est pas une erreur, ou SFINAE) ne s’applique pas (elle s’applique lors de la résolution de surcharge des modèles de fonction). Par conséquent, si le nom à l’intérieur de la classe n’a pas de sens, le compilateur ne continue pas à rechercher une meilleure correspondance : il signale simplement une erreur.

```cppwinrt
struct MyPage : Page
{
    void DoWork()
    {
        // This doesn't compile. You get the error
        // "'winrt::Windows::Foundation::IUnknown::as':
        // no matching overloaded function found".
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<Style>() };
    }
}
```

Ci-dessus, le compilateur pense que vous transmettez [**FrameworkElement.Style()** ](/uwp/api/windows.ui.xaml.frameworkelement.style) (qui est une fonction membre dans C++/WinRT) à [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) comme un paramètre de modèle. La solution consiste à forcer le nom `Style` à être interprété comme étant de type [**Windows::UI::Xaml::Style**](/uwp/api/windows.ui.xaml.style).

```cppwinrt
struct MyPage : Page
{
    void DoWork()
    {
        // One option is to fully-qualify it.
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<Windows::UI::Xaml::Style>() };

        // Another is to force it to be interpreted as a struct name.
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<struct Style>() };

        // If you have "using namespace Windows::UI;", then this is sufficient.
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<Xaml::Style>() };

        // Or you can force it to be resolved in the global namespace (into which
        // you imported the Windows::UI::Xaml namespace when you did
        // "using namespace Windows::UI::Xaml;".
        auto style = Application::Current().Resources().
            Lookup(L"MyStyle").as<::Style>();
    }
}
```

La recherche de noms non qualifiés possède une exception spéciale. Si le nom est suivi de `::`, les fonctions, les variables et les valeurs enum sont ignorées. Cela vous permet d’effectuer des opérations similaires à ce qui suit.

```cppwinrt
struct MyPage : Page
{
    void DoSomething()
    {
        Visibility(Visibility::Collapsed); // No ambiguity here (special exception).
    }
}
```

L’appel de `Visibility()` correspond au nom de la fonction membre [**UIElement.Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility). Toutefois, le paramètre `Visibility::Collapsed` suit le mot `Visibility` avec `::`. Le nom de la méthode est alors ignoré et le compilateur recherche la classe enum.

## <a name="important-apis"></a>API importantes
* [Interface QueryInterface](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [Fonction RoActivateInstance](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance)
* [Classe Windows::Foundation::Uri](/uwp/api/windows.foundation.uri)
* [Modèle de fonction winrt::get_activation_factory](/uwp/cpp-ref-for-winrt/get-activation-factory)
* [Modèle de fonction winrt::make](/uwp/cpp-ref-for-winrt/make)
* [struct winrt::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>Rubriques connexes
* [Créer des événements en C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)
* [Interopérabilité entre C++/WinRT et ABI](interop-winrt-abi.md)
* [Présentation de C++/WinRT](intro-to-using-cpp-with-winrt.md)
* [Contrôles XAML ; liaison à une propriété C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
