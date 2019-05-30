---
description: C++/WinRT peut vous aider à créer des composants COM classiques, comme il vous aide à créer des classes Windows Runtime.
title: Créer des composants COM avec C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: Windows 10, uwp, standard, c ++, cpp, winrt, projection, auteur, COM, composant
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3badcd59155bc4bb5ef8d9e29271b853c245c24e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360321"
---
# <a name="author-com-components-with-cwinrt"></a>Créer des composants COM avec C++/WinRT

[C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) peut vous aider à créer classique composant COM (Object Model) composants (ou coclasses), tout comme il vous aide à créer des classes de Windows Runtime. Voici une situation de façon simple, vous pouvez tester si vous collez le code dans le `pch.h` et `main.cpp` d’un nouveau **Application de Console Windows (C++ / c++ / WinRT)** projet.

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

struct __declspec(uuid("ddc36e02-18ac-47c4-ae17-d420eece2281")) IMyComInterface : ::IUnknown
{
    virtual HRESULT __stdcall Call() = 0;
};

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();

    struct MyCoclass : winrt::implements<MyCoclass, IPersist, IStringable, IMyComInterface>
    {
        HRESULT __stdcall Call() noexcept override
        {
            return S_OK;
        }

        HRESULT __stdcall GetClassID(CLSID* id) noexcept override
        {
            *id = IID_IPersist; // Doesn't matter what we return, for this example.
            return S_OK;
        }

        winrt::hstring ToString()
        {
            return L"MyCoclass as a string";
        }
    };

    auto mycoclass_instance{ winrt::make<MyCoclass>() };
    CLSID id{};
    winrt::check_hresult(mycoclass_instance->GetClassID(&id));
    winrt::check_hresult(mycoclass_instance.as<IMyComInterface>()->Call());
}
```

Consultez également [composants COM consommer avec C / c++ / WinRT](consume-com.md).

## <a name="a-more-realistic-and-interesting-example"></a>Un exemple plus réaliste et intéressant

Le reste de cette rubrique explique comment créer un projet d’application console minimal qui utilise C++ / c++ / WinRT pour implémenter une fabrique de classe et de base coclasse (le composant COM ou classe COM). L’exemple d’application montre comment remettre une notification toast avec un bouton de rappel sur celui-ci et la coclasse (qui implémente le **INotificationActivationCallback** interface COM) permet à l’application d’être lancé et appelé à l’époque où l’utilisateur clique sur ce bouton sur le toast.

Vous trouverez plus d’informations sur la zone de fonctionnalité de notification toast dans [envoyer une notification toast local](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast). Aucun des exemples de code dans cette section de la documentation utiliser C++ / c++ / WinRT, cependant, nous recommandons donc que vous préférez le code présenté dans cette rubrique.

## <a name="create-a-windows-console-application-project-toastandcallback"></a>Créer un projet d’Application de Console Windows (ToastAndCallback)

Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créer un **Application de Console Windows (C++/WinRT)** de projet et nommez-le *ToastAndCallback*.

Open `pch.h`et ajoutez `#include <unknwn.h>` avant l’inclut pour n’importe quel C / c++ / WinRT en-têtes. Voici le résultat ; Vous pouvez remplacer le contenu de votre `pch.h` avec la liste.

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

Ouvrez `main.cpp`et supprimer les à l’aide de directives qui génère le modèle de projet. À leur place, insérez le code suivant (ce qui nous donne de bibliothèques, des en-têtes et des noms de types dont nous avons besoin). Voici le résultat ; Vous pouvez remplacer le contenu de votre `main.cpp` avec cette annonce (nous avons également supprimé le code à partir de `main` dans la liste ci-dessous, étant donné que nous allons remplacer cette fonction plus tard).

```cppwinrt
// main.cpp : Defines the entry point for the console application.

#include "pch.h"

#pragma comment(lib, "advapi32")
#pragma comment(lib, "ole32")
#pragma comment(lib, "shell32")

#include <iomanip>
#include <iostream>
#include <notificationactivationcallback.h>
#include <propkey.h>
#include <propvarutil.h>
#include <shlobj.h>
#include <winrt/Windows.UI.Notifications.h>
#include <winrt/Windows.Data.Xml.Dom.h>

using namespace winrt;
using namespace Windows::Data::Xml::Dom;
using namespace Windows::UI::Notifications;

int main() { }
```

La génération du projet ne sont pas encore ; une fois que nous avons terminé l’ajout de code, vous devrez faire pour générer et exécuter.

## <a name="implement-the-coclass-and-class-factory"></a>Implémentez la fabrique de classe de coclasse

En C / c++ / WinRT, vous implémentez les coclasses et les fabriques de classes, en dérivant le [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements) struct de base. Immédiatement après les trois à l’aide de directives ci-dessus (et avant `main`), collez ce code pour implémenter votre composant d’activateur de COM de notification toast.

```cppwinrt
static constexpr GUID callback_guid // BAF2FA85-E121-4CC9-A942-CE335B6F917F
{
    0xBAF2FA85, 0xE121, 0x4CC9, {0xA9, 0x42, 0xCE, 0x33, 0x5B, 0x6F, 0x91, 0x7F}
};

std::wstring const this_app_name{ L"ToastAndCallback" };

struct callback : winrt::implements<callback, INotificationActivationCallback>
{
    HRESULT __stdcall Activate(
        LPCWSTR app,
        LPCWSTR args,
        [[maybe_unused]] NOTIFICATION_USER_INPUT_DATA const* data,
        [[maybe_unused]] ULONG count) noexcept final
    {
        try
        {
            std::wcout << this_app_name << L" has been called back from a notification." << std::endl;
            std::wcout << L"Value of the 'app' parameter is '" << app << L"'." << std::endl;
            std::wcout << L"Value of the 'args' parameter is '" << args << L"'." << std::endl;
            return S_OK;
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }
};

struct callback_factory : implements<callback_factory, IClassFactory>
{
    HRESULT __stdcall CreateInstance(
        IUnknown* outer,
        GUID const& iid,
        void** result) noexcept final
    {
        *result = nullptr;

        if (outer)
        {
            return CLASS_E_NOAGGREGATION;
        }

        return make<callback>()->QueryInterface(iid, result);
    }

    HRESULT __stdcall LockServer(BOOL) noexcept final
    {
        return S_OK;
    }
};
```

L’implémentation de la coclasse ci-dessus suit le même modèle que celui qui est illustré dans [API auteur avec C / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class). Par conséquent, vous pouvez utiliser la même technique pour implémenter les interfaces COM, ainsi que les interfaces Windows Runtime. Composants COM et des classes Windows Runtime exposent leurs fonctionnalités via des interfaces. Toutes les interfaces COM dérivent en dernier lieu le [ **interface IUnknown** ](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown) interface. Le Runtime de Windows est basé sur COM&mdash;une distinction en cours que Windows Runtime interfaces finalement dérivent le [ **interface IInspectable** ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (et **IInspectable**  dérive **IUnknown**).

Dans la coclasse dans le code ci-dessus, nous implémentons le **INotificationActivationCallback::Activate** (méthode), qui est la fonction est appelée lorsque l’utilisateur clique sur le bouton de rappel sur une notification toast. Mais avant que cette fonction peut être appelée, une instance de la coclasse doit être créé, et qui est le travail de le **IClassFactory::CreateInstance** (fonction).

La coclasse que nous avons implémenté uniquement est connue comme le *activateur de COM* pour les notifications et il a son id de classe (CLSID) de la forme de la `callback_guid` identificateur (de type **GUID**) que vous voyez ci-dessus. Nous allons utiliser cet identificateur plus tard, sous la forme d’un raccourci du menu Démarrer et d’une entrée de Registre de Windows. L’activateur de COM CLSID et le chemin d’accès à son serveur COM associé (qui est le chemin d’accès de l’exécutable que nous créons ici) est le mécanisme par lequel une notification toast sait ce qui classe pour créer une instance de cas de clic sur le bouton de rappel (si le notification utilisateur a cliqué dans le centre de maintenance ou non).

## <a name="best-practices-for-implementing-com-methods"></a>Meilleures pratiques pour implémenter les méthodes COM

Techniques pour la gestion des erreurs et de gestion de ressources peuvent accéder dans pair. Il est plus pratique et plus pratique d’utiliser des exceptions que les codes d’erreur. Et si vous employez l’idiome (RAII) de ressource-acquisition-is-initialization, puis vous pouvez éviter la vérification explicitement pour les codes d’erreur et libérer explicitement les ressources. Ce type de contrôle explicite rendre votre code plus compliqué que nécessaire, et vous offre des bogues beaucoup d’endroits à masquer. Au lieu de cela, utilisez RAII et throw/catch exceptions. De cette façon, vos allocations de ressources sont sécurisées à l’exception, et votre code est simple.

Toutefois, vous ne doit pas autoriser les exceptions à échapper vos implémentations de méthode COM. Vous pouvez vous assurer qu’à l’aide de la `noexcept` spécificateur sur vos méthodes COM. Il est OK pour les exceptions à lever n’importe où dans le graphique des appels de votre méthode, tant que vous gérez avant la fermeture de votre méthode. Si vous utilisez `noexcept`, mais vous permettre ensuite à une exception échapper votre méthode, puis votre application s’arrête.

## <a name="add-helper-types-and-functions"></a>Ajouter des fonctions et types d’assistance

Dans cette étape, nous allons ajouter utilisent certains types d’assistance et les fonctions qui rend le reste du code. Par conséquent, immédiatement avant `main`, ajoutez le code suivant.

```cppwinrt
struct prop_variant : PROPVARIANT
{
    prop_variant() noexcept : PROPVARIANT{}
    {
    }

    ~prop_variant() noexcept
    {
        clear();
    }

    void clear() noexcept
    {
        WINRT_VERIFY_(S_OK, ::PropVariantClear(this));
    }
};

struct registry_traits
{
    using type = HKEY;

    static void close(type value) noexcept
    {
        WINRT_VERIFY_(ERROR_SUCCESS, ::RegCloseKey(value));
    }

    static constexpr type invalid() noexcept
    {
        return nullptr;
    }
};

using registry_key = winrt::handle_type<registry_traits>;

std::wstring get_module_path()
{
    std::wstring path(100, L'?');
    uint32_t path_size{};
    DWORD actual_size{};

    do
    {
        path_size = static_cast<uint32_t>(path.size());
        actual_size = ::GetModuleFileName(nullptr, path.data(), path_size);

        if (actual_size + 1 > path_size)
        {
            path.resize(path_size * 2, L'?');
        }
    } while (actual_size + 1 > path_size);

    path.resize(actual_size);
    return path;
}

std::wstring get_shortcut_path()
{
    std::wstring format{ LR"(%ProgramData%\Microsoft\Windows\Start Menu\Programs\)" };
    format += (this_app_name + L".lnk");

    auto required{ ::ExpandEnvironmentStrings(format.c_str(), nullptr, 0) };
    std::wstring path(required - 1, L'?');
    ::ExpandEnvironmentStrings(format.c_str(), path.data(), required);
    return path;
}
```

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>Implémenter les fonctions restantes et la fonction de point d’entrée wmain

Supprimer votre `main` fonction et à la place, collez ce code répertoriant, qui inclut le code pour inscrire votre coclasse, puis de fournir un toast capable d’appeler dans votre application.

```cppwinrt
void register_callback()
{
    DWORD registration{};

    winrt::check_hresult(::CoRegisterClassObject(
        callback_guid,
        make<callback_factory>().get(),
        CLSCTX_LOCAL_SERVER,
        REGCLS_SINGLEUSE,
        &registration));
}

void create_shortcut()
{
    auto link{ winrt::create_instance<IShellLink>(CLSID_ShellLink) };
    std::wstring module_path{ get_module_path() };
    winrt::check_hresult(link->SetPath(module_path.c_str()));

    auto store = link.as<IPropertyStore>();
    prop_variant value;
    winrt::check_hresult(::InitPropVariantFromString(this_app_name.c_str(), &value));
    winrt::check_hresult(store->SetValue(PKEY_AppUserModel_ID, value));
    value.clear();
    winrt::check_hresult(::InitPropVariantFromCLSID(callback_guid, &value));
    winrt::check_hresult(store->SetValue(PKEY_AppUserModel_ToastActivatorCLSID, value));

    auto file{ store.as<IPersistFile>() };
    std::wstring shortcut_path{ get_shortcut_path() };
    winrt::check_hresult(file->Save(shortcut_path.c_str(), TRUE));

    std::wcout << L"In " << shortcut_path << L", created a shortcut to " << module_path << std::endl;
}

void update_registry()
{
    std::wstring key_path{ LR"(SOFTWARE\Classes\CLSID\{????????-????-????-????-????????????})" };
    ::StringFromGUID2(callback_guid, key_path.data() + 23, 39);
    key_path += LR"(\LocalServer32)";
    registry_key key;

    winrt::check_win32(::RegCreateKeyEx(
        HKEY_CURRENT_USER,
        key_path.c_str(),
        0,
        nullptr,
        0,
        KEY_WRITE,
        nullptr,
        key.put(),
        nullptr));
    ::RegDeleteValue(key.get(), nullptr);

    std::wstring path{ get_module_path() };

    winrt::check_win32(::RegSetValueEx(
        key.get(),
        nullptr,
        0,
        REG_SZ,
        reinterpret_cast<BYTE const*>(path.c_str()),
        static_cast<uint32_t>((path.size() + 1) * sizeof(wchar_t))));

    std::wcout << L"In " << key_path << L", registered local server at " << path << std::endl;
}

void create_toast()
{
    XmlDocument xml;

    std::wstring toastPayload
    {
        LR"(
<toast>
  <visual>
    <binding template='ToastGeneric'>
      <text>)"
    };
    toastPayload += this_app_name;
    toastPayload += LR"(
      </text>
    </binding>
  </visual>
  <actions>
    <action content='Call back )";
    toastPayload += this_app_name;
    toastPayload += LR"(
' arguments='the_args' activationKind='Foreground' />
  </actions>
</toast>)";
    xml.LoadXml(toastPayload);

    ToastNotification toast{ xml };
    ToastNotifier notifier{ ToastNotificationManager::CreateToastNotifier(this_app_name) };
    notifier.Show(toast);
    ::Sleep(50); // Give the callback chance to display.
}

void LaunchedNormally(HANDLE, INPUT_RECORD &, DWORD &);
void LaunchedFromNotification(HANDLE, INPUT_RECORD &, DWORD &);

int wmain(int argc, wchar_t * argv[], wchar_t * /* envp */[])
{
    winrt::init_apartment();

    register_callback();

    HANDLE consoleHandle{ ::GetStdHandle(STD_INPUT_HANDLE) };
    INPUT_RECORD buffer{};
    DWORD events{};
    ::FlushConsoleInputBuffer(consoleHandle);

    if (argc == 1)
    {
        LaunchedNormally(consoleHandle, buffer, events);
    }
    else if (argc == 2 && wcscmp(argv[1], L"-Embedding") == 0)
    {
        LaunchedFromNotification(consoleHandle, buffer, events);
    }
}

void LaunchedNormally(HANDLE consoleHandle, INPUT_RECORD & buffer, DWORD & events)
{
    try
    {
        bool runningAsAdmin{ ::IsUserAnAdmin() == TRUE };
        std::wcout << this_app_name << L" is running" << (runningAsAdmin ? L" (administrator)." : L" (NOT as administrator).") << std::endl;

        if (runningAsAdmin)
        {
            create_shortcut();
            update_registry();
        }

        std::wcout << std::endl << L"Press 'T' to display a toast notification (press any other key to exit)." << std::endl;

        ::ReadConsoleInput(consoleHandle, &buffer, 1, &events);
        if (towupper(buffer.Event.KeyEvent.uChar.UnicodeChar) == L'T')
        {
            create_toast();
        }
    }
    catch (winrt::hresult_error const& e)
    {
        std::wcout << L"Error: " << e.message().c_str() << L" (" << std::hex << std::showbase << std::setw(8) << static_cast<uint32_t>(e.code()) << L")" << std::endl;
    }
}

void LaunchedFromNotification(HANDLE consoleHandle, INPUT_RECORD & buffer, DWORD & events)
{
    ::Sleep(50); // Give the callback chance to display its message.
    std::wcout << std::endl << L"Press any key to exit." << std::endl;
    ::ReadConsoleInput(consoleHandle, &buffer, 1, &events);
}
```

## <a name="how-to-test-the-example-application"></a>Guide pratique pour tester l’exemple d’application

Générez l’application et exécuter au moins une fois en tant qu’administrateur pour provoquer l’inscription et la configuration des autres, exécution de code. Un moyen d’y parvenir consiste à exécuter Visual Studio en tant qu’administrateur et exécutez l’application à partir de Visual Studio. Avec le bouton droit de Visual Studio dans la barre des tâches pour afficher la liste de liens, avec le bouton droit de Visual Studio sur la liste de liens et puis cliquez sur **exécuter en tant qu’administrateur**. Acceptez l’invite, puis ouvrez le projet. Lorsque vous exécutez l’application, un message s’affiche qui indique si l’application s’exécute en tant qu’administrateur. Si elle n’est pas le cas, puis l’inscription et l’autre programme d’installation ne s’exécute. Cette inscription et autres le programme d’installation doit s’exécuter au moins une fois afin que l’application fonctionne correctement.

Si vous exécutez l’application en tant qu’administrateur, appuyez sur t » pour provoquer un toast à afficher. Vous pouvez ensuite cliquer sur le **rappeler ToastAndCallback** bouton directement à partir de la notification toast qui s’affiche, ou à partir du centre de maintenance et que votre application est lancée, la coclasse instanciée et le  **INotificationActivationCallback::Activate** méthode exécutée.

## <a name="in-process-com-server"></a>Serveur COM in-process

Le *ToastAndCallback* exemple d’application ci-dessus fonctionne comme un serveur COM local (ou out-of-process). Cela est indiqué par le [LocalServer32](/windows/desktop/com/localserver32) clé de Registre de Windows que vous utilisez pour inscrire le CLSID de la coclasse. Un serveur COM local héberge son coclass(es) à l’intérieur d’un fichier exécutable binaire (un `.exe`).

Vous pouvez également (et sans doute plus probable), vous pouvez choisir d’héberger votre coclass(es) à l’intérieur d’une bibliothèque de liens dynamiques (un `.dll`). Un serveur COM sous la forme d’une DLL est connu comme un serveur de COM in-process, et il est indiqué par le CLSID en cours d’inscription à l’aide de la [InprocServer32](/windows/desktop/com/inprocserver32) clé de Registre de Windows.

### <a name="create-a-dynamic-link-library-dll-project"></a>Créer un projet de bibliothèque de liens dynamiques (DLL)

Vous pouvez commencer la tâche de création d’un serveur de COM in-process en créant un nouveau projet dans Microsoft Visual Studio. Créer un **Visual C++**  > **Windows Desktop** > **bibliothèque de liens dynamiques (DLL)** projet.

Pour ajouter C + c++ / WinRT le support pour le nouveau projet, suivez les étapes décrites dans [modifier un projet d’application de bureau de Windows pour ajouter C + c++ / WinRT support](/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>Implémenter la coclasse, fabrique de classe et des exportations de serveur in-process

Ouvrez `dllmain.cpp`et lui ajouter la liste de code indiquée ci-dessous.

Si vous disposez déjà d’une DLL qui implémente C + c++ / WinRT Windows Runtime des classes, vous avez déjà la **DllCanUnloadNow** fonction indiquée ci-dessous. Si vous souhaitez ajouter des coclasses à cette DLL, vous pouvez ensuite ajouter le **DllGetClassObject** (fonction).

Si n’avez pas créé [bibliothèque de modèles Windows Runtime C++ (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) parties de code que vous souhaitez rester compatible avec, puis vous pouvez supprimer la bibliothèque WRL à partir du code indiqué.

```cppwinrt
// dllmain.cpp

struct MyCoclass : winrt::implements<MyCoclass, IPersist>
{
    HRESULT STDMETHODCALLTYPE GetClassID(CLSID* id) noexcept override
    {
        *id = IID_IPersist; // Doesn't matter what we return, for this example.
        return S_OK;
    }
};

struct __declspec(uuid("85d6672d-0606-4389-a50a-356ce7bded09"))
    MyCoclassFactory : winrt::implements<MyCoclassFactory, IClassFactory>
{
    HRESULT STDMETHODCALLTYPE CreateInstance(IUnknown *pUnkOuter, REFIID riid, void **ppvObject) noexcept override
    {
        try
        {
            *ppvObject = winrt::make<MyCoclass>().get();
            return S_OK;
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }

    HRESULT STDMETHODCALLTYPE LockServer(BOOL fLock) noexcept override
    {
        // ...
        return S_OK;
    }

    // ...
};

HRESULT __stdcall DllCanUnloadNow()
{
#ifdef _WRL_MODULE_H_
    if (!::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().Terminate())
    {
        return S_FALSE;
    }
#endif

    if (winrt::get_module_lock())
    {
        return S_FALSE;
    }

    winrt::clear_factory_cache();
    return S_OK;
}

HRESULT __stdcall DllGetClassObject(GUID const& clsid, GUID const& iid, void** result)
{
    try
    {
        *result = nullptr;

        if (clsid == __uuidof(MyCoclassFactory))
        {
            return winrt::make<MyCoclassFactory>()->QueryInterface(iid, result);
        }

#ifdef _WRL_MODULE_H_
        return ::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().GetClassObject(clsid, iid, result);
#else
        return winrt::hresult_class_not_available().to_abi();
#endif
    }
    catch (...)
    {
        return winrt::to_hresult();
    }
}
```

### <a name="support-for-weak-references"></a>Prise en charge des références faibles

Consultez également [fait référence à faible en C / c++ / WinRT](weak-references.md#weak-references-in-cwinrt).

C++ / c++ / WinRT (plus précisément, le [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements) modèle du struct de base) implémente [ **IWeakReferenceSource** ](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) pour vous si votre type implémente [ **IInspectable** ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (ou n’importe quelle interface qui dérive de **IInspectable**).

Il s’agit, car **IWeakReferenceSource** et [ **IWeakReference** ](/windows/desktop/api/weakreference/nn-weakreference-iweakreference) sont conçus pour les types Windows Runtime. Par conséquent, vous pouvez activer la prise en charge de la référence faible pour votre coclasse en ajoutant simplement **winrt::Windows::Foundation::IInspectable** (ou une interface qui dérive de **IInspectable**) à votre implémentation.

```cppwinrt
struct MyCoclass : winrt::implements<MyCoclass, IMyComInterface, winrt::Windows::Foundation::IInspectable>
{
    //  ...
};
```

## <a name="important-apis"></a>API importantes
* [Interface IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [Interface IUnknown](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)
* [modèle de struct WinRT::Implements](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>Rubriques connexes
* [Créer des API avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [Utiliser des composants COM avec C++/WinRT](consume-com.md)
* [Envoyer une notification toast locale](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
