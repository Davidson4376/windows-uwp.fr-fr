---
description: Cette rubrique montre comment porter du code C++/CX vers son équivalent en C++/WinRT.
title: Transférer vers C++/WinRT à partir de C++/CX
ms.date: 01/17/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, porter, migrer, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: fe988bffbf024308fb5d43da7ed538e5330b58de
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635074"
---
# <a name="move-to-cwinrt-from-ccx"></a>Transférer vers C++/WinRT à partir de C++/CX

Cette rubrique montre comment déplacer le code dans un [C++ / c++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx) projet en son équivalent dans [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="porting-strategies"></a>Portage des stratégies

Si vous souhaitez déplacer progressivement votre C + c++ / code CX à C++ / c++ / WinRT, vous pouvez. C++ / c++ / CX et c++ / WinRT code peut coexister dans le même projet, à l’exception de la prise en charge du compilateur XAML et de composants Windows Runtime. Pour ces deux exceptions, vous devez cibler soit C + c++ / CX ou c++ / WinRT dans le même projet.

> [!IMPORTANT]
> Si votre projet est généré une application XAML, puis un flux de travail que nous recommandons est d’abord créer un nouveau projet dans Visual Studio à l’aide d’une du C++ / c++ / WinRT les modèles de projet (consultez [prise en charge de Visual Studio pour C / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Commencez à copier le code source et le balisage sur à partir du C++ / c++ / projet CX. Vous pouvez ajouter de nouvelles pages XAML avec **projet** \> **ajouter un nouvel élément...** \> **Visual C++** > **Page vierge (C++ / c++ / WinRT)**.
>
> Vous pouvez également utiliser un composant d’exécution de Windows vers le code de facteur hors XAML C++ / c++ / CX projet comme vous le port. Déplacez autant C + c++ / CX de code que vous pouvez dans un composant, puis remplacez le XAML du projet C++ / c++ / WinRT. Ou sinon laissez le projet XAML en tant que C++ / c++ / CX, créez un nouveau c++ / WinRT, composant et à commencer le portage C++ / c++ / code CX hors du projet XAML et dans le composant. Vous pouvez également avoir un C + c++ / projet du composant en même temps que C++ / CX c++ / projet de composant WinRT dans la même solution, les deux référencer à partir de votre projet d’application et le port progressivement à partir d’un à l’autre. Consultez [interopérabilité entre C++ / c++ / WinRT et C / c++ / CX](interop-winrt-cx.md) pour plus d’informations sur l’utilisation de projections de deux langage dans le même projet.

> [!NOTE]
> [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) et le SDK Windows déclarent tous les deux les types dans l’espace de noms racine **Windows**. Un type Windows projeté en C++/WinRT a le même nom complet que le type Windows, mais il est placé dans l'espace de noms C++ **winrt**. Ces espaces de noms distincts vous permettent de porter le code C++/CX vers C++/WinRT à votre propre rythme.

Sans oublier que les exceptions mentionnées ci-dessus, la première étape lors du portage C++ / c++ / projet CX à C++ / c++ / WinRT consiste à ajouter manuellement C + c++ / prise en charge de WinRT lui (consultez [prise en charge de Visual Studio pour C++ / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Pour ce faire, vous devez installer le [Microsoft.Windows.CppWinRT NuGet package](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) dans votre projet. Ouvrez le projet dans Visual Studio, cliquez sur **projet** \> **gérer les Packages NuGet...** \> **Parcourir**, tapez ou collez **Microsoft.Windows.CppWinRT** dans la zone de recherche, sélectionnez l’élément dans les résultats de recherche, puis cliquez sur **installer** pour installer le package pour ce projet. Un effet de cette modification est la désactivation de la prise en charge de C++/CX dans le projet. Il est judicieux de laisser de prise en charge désactivée afin que les messages de génération vous aider à rechercher (et port) toutes vos dépendances sur C++ / c++ / CX, ou vous pouvez activer la prise en charge de retour (dans les propriétés du projet, **C/C++** \> **général** \> **Consommer l’Extension Windows Runtime** \> **Oui (/ZW)**) et le port progressivement.

Vérifiez cette propriété de projet **général** \> **Version de la plateforme cible** a la valeur 10.0.17134.0 (Windows 10, version 1803) ou supérieur.

Dans votre fichier d’en-tête précompilé (généralement `pch.h`), incluez `winrt/base.h`.

```cppwinrt
#include <winrt/base.h>
```

Si vous incluez des en-têtes d'API Windows projetées en C++/WinRT (par exemple, `winrt/Windows.Foundation.h`), vous n’avez pas besoin d’inclure `winrt/base.h` explicitement ainsi, car il sera automatiquement inclus pour vous.

Si votre projet utilise également des types [Bibliothèque de modèles C++ Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl), consultez [Passer de WRL à C++/WinRT](move-to-winrt-from-wrl.md).

## <a name="parameter-passing"></a>Passage de paramètres
Lorsque vous écrivez C + c++ / le code source CX, vous passez C + c++ / types CX en tant que paramètres de fonction en tant que hat (\^) références.

```cppcx
void LogPresenceRecord(PresenceRecord^ record);
```

En C++/WinRT, pour les fonctions synchrones, vous devez utiliser les paramètres `const&` par défaut. Cela évitera une surcharge liées aux copies et imbrications. Mais vos coroutines doivent utiliser passer par valeur pour être sûr qu’elles capturent par valeur et éviter les problèmes de durée de vie (pour plus d’informations, voir [Opérations concurrentes et asynchrones avec C++/WinRT](concurrency.md)).

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

L'objet C++/WinRT est fondamentalement une valeur qui contient un pointeur d’interface vers l'objet Windows Runtime de sauvegarde. Lorsque vous copiez un objet C++/WinRT, le compilateur copie le pointeur d’interface encapsulé, en incrémentant son nombre de références. Une destruction éventuelle de la copie implique de décrémenter le nombre de références. Par conséquent, ne faites subir la surcharge liée à une copie que lorsque c'est nécessaire.

## <a name="variable-and-field-references"></a>Références de variable et de champ
Lorsque vous écrivez C + c++ / le code source CX, vous utilisez hat (\^) variables peuvent faire référence les objets Windows Runtime et la flèche (-&gt;) opérateur pour déréférencer une variable hat.

```cppcx
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

Lors du portage vers C++ équivalent / c++ / WinRT, code, vous pouvez obtenir des progrès considérables en supprimant les chapeaux et modification de l’opérateur de flèche (-&gt;) à l’opérateur point (.). C++ / c++ / WinRT projetée sont de types valeurs et non des pointeurs.

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

Le constructeur par défaut pour C / c++ / pointeur de hat CX initialise à null. Voici un C + c++ / exemple de code CX dans lequel nous créer un champ/variable du type correct, mais celui qui a non initialisé. En d’autres termes, il n’initialement fait pas référence à un **TextBlock**; nous avons l’intention de lui assigner une référence plus tard.

```cppcx
TextBlock^ textBlock;

class MyClass
{
    TextBlock^ textBlock;
};
```

Pour l’équivalent en C / c++ / WinRT, consultez [l’initialisation différée](consume-apis.md#delayed-initialization).

## <a name="properties"></a>Propriétés
Les extensions de langage C++/CX incluent le concept de propriétés. Lorsque vous écrivez du code source C++/CX, vous pouvez accéder à une propriété comme s’il s’agissait d’un champ. Le code C++ standard n’a pas de concept de propriété, donc, en C++/WinRT, vous appelez les fonctions get et set.

Dans les exemples qui suivent, **XboxUserId**, **UserState**, **PresenceDeviceRecords** et **Taille** sont toutes des propriétés.

### <a name="retrieving-a-value-from-a-property"></a>Récupération de la valeur d’une propriété
Voici comment obtenir une valeur de propriété en C++/CX.

```cppcx
void Sample::LogPresenceRecord(PresenceRecord^ record)
{
    auto id = record->XboxUserId;
    auto state = record->UserState;
    auto size = record->PresenceDeviceRecords->Size;
}
```

Le code source C++/WinRT équivalent appelle une fonction qui a le même nom que la propriété, mais sans paramètres.

```cppwinrt
void Sample::LogPresenceRecord(PresenceRecord const& record)
{
    auto id = record.XboxUserId();
    auto state = record.UserState();
    auto size = record.PresenceDeviceRecords().Size();
}
```

Notez que la fonction **PresenceDeviceRecords** renvoie un objet Windows Runtime qui a lui-même une fonction **Taille**. Comme l’objet renvoyé est également un type projeté C++/WinRT, nous supprimons la référence à l’aide de l’opérateur point pour appeler **Taille**.

### <a name="setting-a-property-to-a-new-value"></a>Définition d’une propriété sur une nouvelle valeur
La définition d’une propriété sur une nouvelle valeur suit un modèle similaire. Tout d'abord, en C++/CX.

```cppcx
record->UserState = newValue;
```

Pour effectuer l’équivalent en C++ /WinRT, vous appelez une fonction portant le même nom que la propriété et vous passez un argument.

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>Création d’une instance d’une classe
Vous travaillez avec un C / c++ / objet CX via un handle, communément appelé un chapeau (\^) référence. Vous créez un objet via le mot clé `ref new`, qui à son tour appelle [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646) pour activer une nouvelle instance de la classe runtime.

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

L'objet C++/WinRT est une valeur ; par conséquent, vous pouvez l’allouer sur la pile, ou en tant que champ d’un objet. Vous n'utilisez *jamais*`ref new` (ni `new`) pour allouer un objet C++/WinRT. En arrière-plan, **RoActivateInstance** est toujours appelé.

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

Si une ressource est coûteuse à initialiser, il est courant de retarder son initialisation jusqu'à ce qu'elle soit réellement nécessaire.

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
public:
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer^ m_gamerPicBuffer;
};
```

Le même code porté en C++/WinRT. Notez l’utilisation du constructeur `nullptr`. Pour plus d’informations sur ce constructeur, voir [Utiliser des API avec C++/WinRT](consume-apis.md).

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

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
```

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>Conversion à partir d’une classe runtime de base vers une dérivée
Il est courant d’avoir une-à-base de référence que vous connaissez fait référence à un objet d’un type dérivé. En C / c++ / CX, vous utilisez `dynamic_cast` à *cast* la référence à base dans une référence à dérivé. Le `dynamic_cast` est simplement un appel masqué à [ **QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521). Voici un exemple typique&mdash;vous traitez un événement de modification de propriété de dépendance, et que vous souhaitez effectuer un cast du **DependencyObject** vers le type réel qui possède la propriété de dépendance.

```cppcx
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject^ d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e)
{
    BgLabelControl^ theControl{ dynamic_cast<BgLabelControl^>(d) };

    if (theControl != nullptr)
    {
        // succeeded ...
    }
}
```

Équivalents C++ / c++ / WinRT code remplace le `dynamic_cast` avec un appel à la [ **IUnknown::try_as** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function) (fonction), qui encapsule **QueryInterface**. Vous avez également la possibilité d’appeler [ **IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function), au lieu de cela, qui lève une exception si l’interrogation de l’interface requise (l’interface par défaut du type que vous demandez) n’est pas retournée. Voici un C + c++ / exemple de code de WinRT.

```cppwinrt
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // succeeded ...
    }

    try
    {
        BgLabelControlApp::BgLabelControl theControl{ d.as<BgLabelControlApp::BgLabelControl>() };
        // succeeded ...
    }
    catch (winrt::hresult_no_interface const&)
    {
        // failed ...
    }
}
```

## <a name="event-handling-with-a-delegate"></a>Gestion des événements avec un délégué
Voici un exemple type de gestion d’un événement en C++/CX, en utilisant une fonction lambda en tant que délégué dans ce cas.

```cppcx
auto token = myButton->Click += ref new RoutedEventHandler([=](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

C’est l’équivalent en C++/WinRT.

```cppwinrt
auto token = myButton().Click([=](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

Au lieu d’une fonction lambda, vous pouvez choisir d’implémenter votre délégué sous forme d’une fonction libre ou en tant que pointeur-vers-fonction-membre. Pour plus d'informations, voir [Gérer des événements en utilisant des délégués en C++/WinRT](handle-events.md).

Si vous effectuez un portage à partir d'un code base C++/CX où les événements et les délégués sont utilisés en interne (pas sur les fichiers binaires), [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) vous aidera à répliquer ce modèle en C++/WinRT. Consultez également [paramétrables délégués, les signaux simples et les rappels dans un projet](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project).

## <a name="revoking-a-delegate"></a>Révocation d’un délégué
Dans C++/CX, vous utilisez l'opérateur `-=` pour révoquer l'inscription d'un événement préalable.

```cppcx
myButton->Click -= token;
```

C’est l’équivalent en C++/WinRT.

```cppwinrt
myButton().Click(token);
```

Pour plus d’informations et d'options, voir [Révoquer un délégué inscrit](handle-events.md#revoke-a-registered-delegate).

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>Mappage de types **Platform** C++/CX sur des types C++/WinRT
C++/CX fournit plusieurs types de données dans l'espace de noms **Platform**. Ces types ne sont pas en C++ standard, donc vous ne pouvez les utiliser que lorsque vous activez les extensions de langage Windows Runtime (propriété de projet Visual Studio **C/C++** > **Général** > **Consommer l'extension Windows Runtime** > **Oui (/ZW)**). Le tableau ci-dessous vous aide à porter des types **Platform** vers leurs équivalents en C++/WinRT. Une fois que c'est fait, puisque C++/WinRT est en C++ standard, vous pouvez désactiver l'option `/ZW`.

| C++/CX | C++/WinRT |
| ---- | ---- |
| **Platform::Agile\^** | [**WinRT::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform::Array\^** | Consultez [Port **Platform::Array\^**](#port-platformarray) |
| **Platform::exception\^** | [**WinRT::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Exception Platform::InvalidArgumentException\^** | [**WinRT::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform::Object\^** | **WinRT::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**WinRT::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagileref"></a>Port **Platform::Agile\^**  à **winrt::agile_ref**
Le **Platform::Agile\^**  type en C / c++ / CX représente une classe Windows Runtime qui est accessible à partir de n’importe quel thread. C++ / c++ / WinRT équivalent est [ **winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref).

En C++/CX.

```cppcx
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

En C++/WinRT.

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformarray"></a>Port **Platform::Array\^**
Vos options incluent l’utilisation d’une liste d’initialiseurs, un **std::array**, ou un **std::vector**. Pour plus d’informations et d’exemples de code, consultez [listes d’initialiseurs Standard](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists) et [tableaux Standard et les vecteurs](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-arrays-and-vectors).

### <a name="port-platformexception-to-winrthresulterror"></a>Port **Platform::Exception\^**  à **winrt::hresult_error**
Le **Platform::Exception\^**  type est généré en C / c++ / CX lorsqu’une API de Runtime Windows renvoie un S non\_OK HRESULT. L’équivalent en C++/WinRT est [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error).

Vers le port C + c++ / WinRT, modifiez tout code qui utilise **Platform::Exception\^**  à utiliser **winrt::hresult_error**.

En C++/CX.

```cppcx
catch (Platform::Exception^ ex)
```

En C++/WinRT.

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT fournit ces classes d’exceptions.

| Type d’exception | Classe de base | HRESULT |
| ---- | ---- | ---- |
| [**WinRT::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | appel de [**hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function) |
| [**WinRT::hresult_access_denied**](/uwp/cpp-ref-for-winrt/error-handling/hresult-access-denied) | **WinRT::hresult_error** | E_ACCESSDENIED |
| [**WinRT::hresult_canceled**](/uwp/cpp-ref-for-winrt/error-handling/hresult-canceled) | **WinRT::hresult_error** | ERROR_CANCELLED |
| [**WinRT::hresult_changed_state**](/uwp/cpp-ref-for-winrt/error-handling/hresult-changed-state) | **WinRT::hresult_error** | E_CHANGED_STATE |
| [**WinRT::hresult_class_not_available**](/uwp/cpp-ref-for-winrt/error-handling/hresult-class-not-available) | **WinRT::hresult_error** | CLASS_E_CLASSNOTAVAILABLE |
| [**WinRT::hresult_illegal_delegate_assignment**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-delegate-assignment) | **WinRT::hresult_error** | E_ILLEGAL_DELEGATE_ASSIGNMENT |
| [**WinRT::hresult_illegal_method_call**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-method-call) | **WinRT::hresult_error** | E_ILLEGAL_METHOD_CALL |
| [**WinRT::hresult_illegal_state_change**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-state-change) | **WinRT::hresult_error** | E_ILLEGAL_STATE_CHANGE |
| [**WinRT::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) | **WinRT::hresult_error** | E_INVALIDARG |
| [**WinRT::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) | **WinRT::hresult_error** | E_NOINTERFACE |
| [**WinRT::hresult_not_implemented**](/uwp/cpp-ref-for-winrt/error-handling/hresult-not-implemented) | **WinRT::hresult_error** | E_NOTIMPL |
| [**WinRT::hresult_out_of_bounds**](/uwp/cpp-ref-for-winrt/error-handling/hresult-out-of-bounds) | **WinRT::hresult_error** | E_BOUNDS |
| [**WinRT::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) | **WinRT::hresult_error** | RPC_E_WRONG_THREAD |

Notez que chaque classe (via la classe de base **hresult_error**) fournit une fonction [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function) qui retourne le HRESULT de l’erreur, et une fonction [**message**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrormessage-function) qui retourne la représentation de chaîne de ce HRESULT.

Voici un exemple de levée d'exception en C++/CX.

```cppcx
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

Et voici l’équivalent en C++/WinRT.

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>Port **Platform::Object\^**  à **winrt::Windows::Foundation::IInspectable**
Comme tous les types C++/WinRT, **winrt::Windows::Foundation::IInspectable** est un type de valeur. Voici comment vous initialisez une variable de ce type sur la valeur Null.

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>Port **Platform::String\^**  à **winrt::hstring**
**Platform::String\^**  est équivalente au type de l’interface ABI de Windows Runtime HSTRING. Pour C++/WinRT, l’équivalent est [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). Mais avec C++/WinRT, vous pouvez appeler des API Windows Runtime à l’aide de types de chaînes étendues de la bibliothèque C++ standard comme **std::wstring** et/ou des littéraux de chaîne étendue. Pour obtenir plus d’informations et des exemples de code, voir [Gestion des chaînes en C++/WinRT](strings.md).

Avec C++ / c++ / CX, vous pouvez accéder à la [ **Platform::String::Data** ](https://docs.microsoft.com/en-us/cpp/cppcx/platform-string-class#data) propriété pour récupérer la chaîne en tant que C-style **const wchar_t\***  tableau (par exemple, pour passer à **std::wcout**).

```cppcx
auto var{ titleRecord->TitleName->Data() };
```

Pour faire de même avec C++/WinRT, vous pouvez utiliser la fonction [**hstring::c_str**](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) pour obtenir une version de la chaîne de style C terminée par Null, exactement comme avec **std::wstring**.

```cppwinrt
auto var{ titleRecord.TitleName().c_str() };
```

Lorsqu’il s’agit à l’implémentation des API qui utilisent ou renvoient des chaînes, vous modifiez généralement n’importe quel C + c++ / code CX qui utilise **Platform::String\^**  à utiliser **winrt::hstring** à la place.

Voici un exemple d'API C++/CX qui utilise une chaîne.

```cppcx
void LogWrapLine(Platform::String^ str);
```

Pour C++/WinRT, vous pouvez déclarer cette API dans [MIDL 3.0](/uwp/midl-3) comme suit.

```idl
// LogType.idl
void LogWrapLine(String str);
```

La chaîne d’outils C++/WinRT génère pour vous le code source, qui se présente comme suit.

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

#### <a name="tostring"></a>ToString()

C++ / c++ / CX fournit le [Object::ToString](/cpp/cppcx/platform-object-class?view=vs-2017#tostring) (méthode).

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

C++ / c++ / WinRT ne fournit pas directement cette fonctionnalité, mais vous pouvez activer des alternatives.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

## <a name="important-apis"></a>API importantes
* [modèle de struct WinRT::Delegate](/uwp/cpp-ref-for-winrt/delegate)
* [WinRT::hresult_error struct](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [WinRT::hstring struct](/uwp/cpp-ref-for-winrt/hstring)
* [Espace de noms winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Rubriques connexes
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Créer des événements en C / c++ / WinRT](author-events.md)
* [Concurrence et des opérations asynchrones avec C / c++ / WinRT](concurrency.md)
* [Utiliser des API avec C++/WinRT](consume-apis.md)
* [Gérer les événements à l’aide de délégués en C / c++ / WinRT](handle-events.md)
* [Interopérabilité entre C++/WinRT et C++/CX](interop-winrt-cx.md)
* [Référence de Microsoft Interface Definition Language 3.0](/uwp/midl-3)
* [Passer de WRL à C++/WinRT](move-to-winrt-from-wrl.md)
* [Chaîne gère en C / c++ / WinRT](strings.md)
