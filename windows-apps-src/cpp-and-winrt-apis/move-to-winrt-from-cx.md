---
description: Cette rubrique montre comment porter du code C++/CX vers son équivalent en C++/WinRT.
title: Passer de C++/CX à C++/WinRT
ms.date: 10/18/2018
ms.topic: article
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, porter, migrer, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 5a6a778f1efe16d56c24e437a0c25a8b8c5e3bc7
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7989141"
---
# <a name="move-to-cwinrt-from-ccx"></a>Transférer vers C++/WinRT à partir de C++/CX

Cette rubrique montre comment porter du code dans un [C++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx) projet vers son équivalent en [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="porting-strategies"></a>Portage des stratégies

Si vous souhaitez porter progressivement votre C + / code CX vers C++ / WinRT, vous pouvez. C++ / CX et C++ / WinRT code peut coexister dans le même projet, à l’exception de la prise en charge du compilateur XAML et de composants Windows Runtime. Pour ces deux exceptions, vous devez cibler soit C++ / CX ou C++ / WinRT au sein du même projet.

> [!IMPORTANT]
> Si votre projet génère une application XAML, puis un flux de travail que nous vous recommandons est d’abord créer un nouveau projet dans Visual Studio à l’aide d’un de ces C++ / WinRT les modèles de projet (voir [prise en charge de Visual Studio pour C++ / WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)). Ensuite, démarrez copier le code source et balisage dessus à partir de la C + / projet CX. Vous pouvez ajouter des pages XAML nouveau **projet** \> **Ajouter un nouvel élément …**  \>  **Visual C++** > **Page vierge (C++ / WinRT)**.
>
> Sinon, vous pouvez utiliser un composant Windows Runtime au code facteur en dehors du XAML C++ / CX de projet comme vous le porter. Déplacez autant C++ / CX de code que vous pouvez dans un composant, puis modifiez le projet XAML vers C++ / WinRT. Ou else laisser le projet XAML en tant que C++ / CX, créez un nouveau C + / composant WinRT et commencer le portage C++ / code CX du projet XAML et placez-le dans le composant. Vous pouvez également avoir C++ / projet de composant CX en même temps que C++ / projet de composant WinRT au sein de la même solution, les deux référencer à partir de votre projet d’application, mais progressivement les ports d’un utilisateur à l’autre. Voir [l’interopérabilité entre C++ / WinRT et C++ / CX](interop-winrt-cx.md) pour plus d’informations sur l’utilisation des deux projections de langage dans le même projet.

> [!NOTE]
> [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) et le SDK Windows déclarent tous les deux les types dans l’espace de noms racine **Windows**. Un type Windows projeté en C++/WinRT a le même nom complet que le type Windows, mais il est placé dans l'espace de noms C++ **winrt**. Ces espaces de noms distincts vous permettent de porter le code C++/CX vers C++/WinRT à votre propre rythme.

Compte tenu des exceptions mentionnées ci-dessus, la première étape lors du portage C++ / projet CX vers C++ / WinRT consiste à ajouter manuellement C++ / WinRT prise en charge à sa (voir [prise en charge de Visual Studio pour C++ / WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)). Pour ce faire, vous devez modifier votre fichier `.vcxproj`, rechercher `<PropertyGroup Label="Globals">` et, à l’intérieur de ce groupe de propriétés, définir la propriété `<CppWinRTEnabled>true</CppWinRTEnabled>`. Un effet de cette modification est la désactivation de la prise en charge de C++/CX dans le projet. Il est conseillé de laisser prise en charge désactivée afin que les messages de génération vous aider à trouver (et le port) toutes les dépendances sur C++ / CX, ou vous pouvez réactiver prise en charge (dans les propriétés du projet, **C/C++** \> **Général** \> **consommer Windows Runtime Extension** \> **Oui (/ZW)**) et le port progressivement.

Assurez-vous de cette propriété de projet **Général** \> **Version de la plateforme cible** est défini sur 10.0.17134.0 (Windows 10, version 1803) ou une version ultérieure.

Dans votre fichier d’en-tête précompilé (généralement `pch.h`), incluez `winrt/base.h`.

```cppwinrt
#include <winrt/base.h>
```

Si vous incluez des en-têtes d'API Windows projetées en C++/WinRT (par exemple, `winrt/Windows.Foundation.h`), vous n’avez pas besoin d’inclure `winrt/base.h` explicitement ainsi, car il sera automatiquement inclus pour vous.

Si votre projet utilise également des types [Bibliothèque de modèles C++ Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl), consultez [Passer de WRL à C++/WinRT](move-to-winrt-from-wrl.md).

## <a name="parameter-passing"></a>Passage de paramètres
Lorsque vous écrivez du code source C++/CX, vous transmettez des types C++/CX en tant que paramètres de fonction sous forme de références accent circonflexe (\^).

```cpp
void LogPresenceRecord(PresenceRecord^ record);
```

En C++/WinRT, pour les fonctions synchrones, vous devez utiliser les paramètres `const&` par défaut. Cela évitera une surcharge liées aux copies et imbrications. Mais vos coroutines doivent utiliser passer par valeur pour être sûr qu’elles capturent par valeur et éviter les problèmes de durée de vie (pour plus d’informations, voir [Opérations concurrentes et asynchrones avec C++/WinRT](concurrency.md)).

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

L'objet C++/WinRT est fondamentalement une valeur qui contient un pointeur d’interface vers l'objet Windows Runtime de sauvegarde. Lorsque vous copiez un objet C++/WinRT, le compilateur copie le pointeur d’interface encapsulé, en incrémentant son nombre de références. Une destruction éventuelle de la copie implique de décrémenter le nombre de références. Par conséquent, ne faites subir la surcharge liée à une copie que lorsque c'est nécessaire.

## <a name="variable-and-field-references"></a>Références de variable et de champ
Lorsque vous écrivez du code source C++/CX, vous utilisez des variables accent circonflexe (\^) pour référencer des objets Windows Runtime et l'opérateur flèche (-&gt;) pour supprimer la référence à une variable accent circonflexe.

```cpp
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

Lors du portage vers l’équivalent C++ / WinRT code, vous supprimez les chapeaux accents et que vous modifiez l’opérateur flèche (-&gt;) pour l’opérateur point (.), dans la mesure où C++ / WinRT projetés types sont des valeurs et non des pointeurs.

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

## <a name="properties"></a>Propriétés
Les extensions de langage C++/CX incluent le concept de propriétés. Lorsque vous écrivez du code source C++/CX, vous pouvez accéder à une propriété comme s’il s’agissait d’un champ. Le code C++ standard n’a pas de concept de propriété, donc, en C++/WinRT, vous appelez les fonctions get et set.

Dans les exemples qui suivent, **XboxUserId**, **UserState**, **PresenceDeviceRecords** et **Taille** sont toutes des propriétés.

### <a name="retrieving-a-value-from-a-property"></a>Récupération de la valeur d’une propriété
Voici comment obtenir une valeur de propriété en C++/CX.

```cpp
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

```cpp
record->UserState = newValue;
```

Pour effectuer l’équivalent en C++ /WinRT, vous appelez une fonction portant le même nom que la propriété et vous passez un argument.

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>Création d’une instance d’une classe
Vous travaillez avec un objet C++/CX via un handle vers lui, communément appelé référence accent circonflexe (\^). Vous créez un objet via le mot clé `ref new`, qui à son tour appelle [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646) pour activer une nouvelle instance de la classe runtime.

```cpp
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

L'objet C++/WinRT est une valeur; par conséquent, vous pouvez l’allouer sur la pile, ou en tant que champ d’un objet. Vous n'utilisez *jamais* `ref new` (ni `new`) pour allouer un objet C++/WinRT. En arrière-plan, **RoActivateInstance** est toujours appelé.

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

Si une ressource est coûteuse à initialiser, il est courant de retarder son initialisation jusqu'à ce qu'elle soit réellement nécessaire.

```cpp
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
Il est courant d’avoir une-à-base de référence que vous connaissez fait référence à un objet d’un type dérivé. En C++ / CX, vous utilisez `dynamic_cast` à *cast* la-à-base de référence dans une référence vers dérivé. Le `dynamic_cast` est vraiment simplement un appel masqué à [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521). Voici un exemple type&mdash;vous gérez un événement de modification de propriété de dépendance, et vous souhaitez effectuer un cast de **DependencyObject** vers le type réel qui possède la propriété de dépendance.

```cpp
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject^ d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e)
{
    BgLabelControl^ theControl{ dynamic_cast<BgLabelControl^>(d) };

    if (theControl != nullptr)
    {
        // succeeded ...
    }
}
```

L’équivalent C++ / WinRT code remplace le `dynamic_cast` avec un appel à la fonction [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function) , qui englobe **QueryInterface**. Vous avez également la possibilité d’appeler [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function), au lieu de cela, ce qui lève une exception si l’interrogation de l’interface requis (l’interface par défaut du type que vous demandez) n’est pas renvoyé. Voici C++ / WinRT les exemple de code.

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

```cpp
auto token = myButton->Click += ref new RoutedEventHandler([&](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
});
```

C’est l’équivalent en C++/WinRT.

```cppwinrt
auto token = myButton().Click([&](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
});
```

Au lieu d’une fonction lambda, vous pouvez choisir d’implémenter votre délégué sous forme d’une fonction libre ou en tant que pointeur-vers-fonction-membre. Pour plus d'informations, voir [Gérer des événements en utilisant des délégués en C++/WinRT](handle-events.md).

Si vous effectuez un portage à partir d'un code base C++/CX où les événements et les délégués sont utilisés en interne (pas sur les fichiers binaires), [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) vous aidera à répliquer ce modèle en C++/WinRT. Consultez également [paramétrée délégués, les signaux simples et les rappels au sein d’un projet](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project).

## <a name="revoking-a-delegate"></a>Révocation d’un délégué
Dans C++/CX, vous utilisez l'opérateur `-=` pour révoquer l'inscription d'un événement préalable.

```cpp
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
| **Platform:: Agile\ ^** | [**WinRT::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagileref"></a>Port **Platform:: Agile\ ^** à **winrt::agile_ref**
Le **Platform:: Agile\ ^** type en C++ / CX représente une classe Windows Runtime qui sont accessibles à partir de n’importe quel thread. C++ / WinRT équivalent est [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref).

En C++/CX.

```cpp
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

En C++/WinRT.

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformexception-to-winrthresulterror"></a>Porter **Platform::Exception\^** vers **winrt::hresult_error**
Le type **Platform::Exception\^** se produit en C++/CX quand une API Windows Runtime retourne un HRESULT autre que S\_OK. L’équivalent en C++/WinRT est [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error).

Pour le portage vers C++/WinRT, modifiez tout le code qui utilise **Platform::Exception\^** afin d'utiliser **winrt::hresult_error**.

En C++/CX.

```cpp
catch (Platform::Exception^ ex)
```

En C++/WinRT.

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT fournit ces classes d’exceptions.

| Type d'exception | Classe de base | HRESULT |
| ---- | ---- | ---- |
| [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | appel de [**hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function) |
| [**winrt::hresult_access_denied**](/uwp/cpp-ref-for-winrt/error-handling/hresult-access-denied) | **winrt::hresult_error** | E_ACCESSDENIED |
| [**winrt::hresult_canceled**](/uwp/cpp-ref-for-winrt/error-handling/hresult-canceled) | **winrt::hresult_error** | ERROR_CANCELLED |
| [**winrt::hresult_changed_state**](/uwp/cpp-ref-for-winrt/error-handling/hresult-changed-state) | **winrt::hresult_error** | E_CHANGED_STATE |
| [**winrt::hresult_class_not_available**](/uwp/cpp-ref-for-winrt/error-handling/hresult-class-not-available) | **winrt::hresult_error** | CLASS_E_CLASSNOTAVAILABLE |
| [**winrt::hresult_illegal_delegate_assignment**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-delegate-assignment) | **winrt::hresult_error** | E_ILLEGAL_DELEGATE_ASSIGNMENT |
| [**winrt::hresult_illegal_method_call**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-method-call) | **winrt::hresult_error** | E_ILLEGAL_METHOD_CALL |
| [**winrt::hresult_illegal_state_change**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-state-change) | **winrt::hresult_error** | E_ILLEGAL_STATE_CHANGE |
| [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) | **winrt::hresult_error** | E_INVALIDARG |
| [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) | **winrt::hresult_error** | E_NOINTERFACE |
| [**winrt::hresult_not_implemented**](/uwp/cpp-ref-for-winrt/error-handling/hresult-not-implemented) | **winrt::hresult_error** | E_NOTIMPL |
| [**winrt::hresult_out_of_bounds**](/uwp/cpp-ref-for-winrt/error-handling/hresult-out-of-bounds) | **winrt::hresult_error** | E_BOUNDS |
| [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) | **winrt::hresult_error** | RPC_E_WRONG_THREAD |

Notez que chaque classe (via la classe de base **hresult_error**) fournit une fonction [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function) qui retourne le HRESULT de l’erreur, et une fonction [**message**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrormessage-function) qui retourne la représentation de chaîne de ce HRESULT.

Voici un exemple de levée d'exception en C++/CX.

```cpp
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

Et voici l’équivalent en C++/WinRT.

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>Porter **Platform::Object\^** vers **winrt::Windows::Foundation::IInspectable**
Comme tous les types C++/WinRT, **winrt::Windows::Foundation::IInspectable** est un type de valeur. Voici comment vous initialisez une variable de ce type sur la valeur Null.

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>Porter **Platform::String\^** vers **winrt::hstring**
**Platform::String\^** équivaut au type WindowsRuntimeHSTRINGABI. Pour C++/WinRT, l’équivalent est [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). Mais avec C++/WinRT, vous pouvez appeler des API Windows Runtime à l’aide de types de chaînes étendues de la bibliothèque C++ standard comme **std::wstring** et/ou des littéraux de chaîne étendue. Pour obtenir plus d’informations et des exemples de code, voir [Gestion des chaînes en C++/WinRT](strings.md).

Avec C++/CX, vous pouvez accéder à la propriété [**Platform::String::Data**](https://docs.microsoft.com/en-us/cpp/cppcx/platform-string-class#data) pour récupérer la chaîne en tant que tableau **const wchar_t\*** de style C (par exemple, pour le passer à **std::wcout**).

```C++
auto var = titleRecord->TitleName->Data();
```

Pour faire de même avec C++/WinRT, vous pouvez utiliser la fonction [**hstring::c_str**](/uwp/api/windows.foundation.uri#hstringcstr-function) pour obtenir une version de la chaîne de style C terminée par Null, exactement comme avec **std::wstring**.

```C++
auto var = titleRecord.TitleName().c_str();
```

Lorsqu’il s’agit d'implémenter des API qui utilisent ou retournent des chaînes, généralement, vous modifiez tout code C++/CX qui utilise **Platform::String\^** pour utiliser **winrt::hstring** à la place.

Voici un exemple d'API C++/CX qui utilise une chaîne.

```cpp
void LogWrapLine(Platform::String^ str);
```

Pour C++/WinRT, vous pouvez déclarer cette API dans [MIDL3.0](/uwp/midl-3) comme suit.

```idl
// LogType.idl
void LogWrapLine(String str);
```

La chaîne d’outils C++/WinRT génère pour vous le code source, qui se présente comme suit.

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

## <a name="important-apis"></a>API importantes
* [winrt::delegate struct template](/uwp/cpp-ref-for-winrt/delegate)
* [Structure winrt::hresult_error](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [Structure winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Espace de noms winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Rubriquesassociées
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Créer des événements en C++/WinRT](author-events.md)
* [Opérations concurrentes et asynchrones avec C++/WinRT](concurrency.md)
* [Utiliser des API avec C++/WinRT](consume-apis.md)
* [Gérer des événements en utilisant des délégués en C++/WinRT](handle-events.md)
* [Interopérabilité entre C++/WinRT et C++/CX](interop-winrt-cx.md)
* [Référence de MicrosoftInterface Definition Language3.0](/uwp/midl-3)
* [Passer de WRL à C++/WinRT](move-to-winrt-from-wrl.md)
* [Gestion des chaînes en C++/WinRT](strings.md)
