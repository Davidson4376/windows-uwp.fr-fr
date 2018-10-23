---
author: stevewhims
description: C++ / WinRT peut vous aider à créer des composants COM classiques, tout comme il vous aide à créer des classes Windows Runtime.
title: Créer des composants COM avec C++/WinRT
ms.author: stwhi
ms.date: 09/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, standard, c++, cpp, winrt, projection, auteur, COM, composant
ms.localizationpriority: medium
ms.openlocfilehash: 24fd024ee25a6868b8c25b77ce31edc6662bbb6d
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/23/2018
ms.locfileid: "5432725"
---
# <a name="author-com-components-with-cwinrt"></a>Créer des composants COM avec C++/WinRT

[C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) peuvent vous aider à créer des classique modèle COM (Component Object) composants (ou coclasses), tout comme il vous aide à créer des classes Windows Runtime. Voici une illustration simple, ce qui vous pouvez tester si vous collez le code dans les `pch.h` et `main.cpp` d’une nouvelle **Windows Console Application (C++ / WinRT)** projet.

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

Consultez également [des composants de consommer COM avec C++ / WinRT](consume-com.md).

## <a name="a-more-realistic-and-interesting-example"></a>Un exemple plus réaliste et intéressant

Le reste de cette rubrique vous guide tout au processus de création d’un projet d’application de console minimal qui utilise C++ / WinRT pour implémenter une fabrique de classe et de base coclasse (composant COM ou classe COM). L’exemple d’application montre comment fournir une notification toast avec un bouton de rappel dessus et la coclasse (qui implémente l’interface **INotificationActivationCallback** COM) permet à l’application d’être lancée et appelée d’arrière-plan lorsque l’utilisateur clique sur ce bouton du toast.

Vous trouverez plus d’informations sur la zone de fonctionnalité de notification toast à [Envoyer une notification toast locale](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast). Aucun des exemples de code dans cette section de la documentation utilisent C++ / WinRT, cependant, nous vous conseillons donc que vous préférez le code présenté dans cette rubrique.

## <a name="create-a-windows-console-application-project-toastandcallback"></a>Créez un projet d’Application Console Windows (ToastAndCallback)

Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créer un **Visual C++** > **Windows Desktop** > **Windows Console Application (C++ / WinRT)** de projet et nommez-le *ToastAndCallback*.

Ouvrez `pch.h`et ajoutez `#include <unknwn.h>` avant l’inclut pour n’importe quel C++ / WinRT en-têtes.

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

Ouvrez `main.cpp`et supprimer les using-directives qui génère le modèle de projet. À leur place, collez le code suivant (ce qui nous obtenons les bibliothèques, les en-têtes et les noms de type dont nous avons besoin).

```cppwinrt
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
```

## <a name="implement-the-coclass-and-class-factory"></a>Implémentez la fabrique de classe et coclasse

En C++ / WinRT, vous implémentez coclasses et les fabriques de classes, en dérivant de la structure de base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) . Immédiatement après les trois-directives using ci-dessus (et avant `main`), collez ce code pour implémenter votre composant d’activateur de notification COM toast.

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

L’implémentation de la coclasse ci-dessus suit le même modèle qui est présenté dans [créer des API avec C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class). Par conséquent, vous pouvez utiliser la même technique pour implémenter les interfaces COM, ainsi que des interfaces Windows Runtime. Des composants COM et les classes Windows Runtime exposent leurs fonctionnalités via des interfaces. Chaque interface COM dérive en fin de compte de l’interface [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) . Windows Runtime est basé sur COM&mdash;une distinction étant des interfaces Windows Runtime au final dérivent de l' [**interface IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (et **IInspectable** dérive de **IUnknown**).

Dans la coclasse dans le code ci-dessus, nous implémentez la méthode **INotificationActivationCallback::Activate** , qui est la fonction qui est appelée lorsque l’utilisateur clique sur le bouton de rappel dans une notification toast. Toutefois, avant que cette fonction peut être appelée, une instance de la coclasse doit être créé, et c’est le rôle de la fonction **IClassFactory::CreateInstance** .

La coclasse que nous avons simplement implémentées est appelée l' *activateur COM* pour les notifications, et qui présente son id de classe (CLSID) sous la forme de la `callback_guid` identificateur (de type **GUID**) que vous voyez ci-dessus. Nous allons utiliser cet identificateur plus tard, sous la forme d’un raccourci du menu Démarrer et une entrée de Registre Windows. L’activateur COM CLSID et le chemin d’accès à son serveur COM associé (qui est le chemin d’accès de l’exécutable que nous créons ici) est le mécanisme par lequel une notification toast sait quel classe pour créer une instance de lorsque son bouton de rappel est cliqué (si le notification est effectuée dans le centre de notifications ou non).

## <a name="best-practices-for-implementing-com-methods"></a>Meilleures pratiques pour l’implémentation de méthodes COM.

Techniques de gestion des erreurs et de gestion de ressources peuvent accéder en pair. Il est plus pratique et plus pratique d’utiliser les exceptions que les codes d’erreur. Et si vous n’utilisiez l’idiome (RAII) de ressource acquisition-est-d’initialisation, puis vous pouvez éviter explicitement vérification des codes d’erreur et puis en libérant explicitement les ressources. Ces contrôles explicites que votre code alambiqué profiter plus que nécessaire, et vous permet de bogues beaucoup d’endroits pour masquer. Au lieu de cela, utilisez RAII et lever/catch exceptions. De cette façon, votre allocations de ressources sont à toute exception, et votre code est simple.

Toutefois, vous up ne doit pas autoriser les exceptions comme caractère d’échappement vos implémentations de méthode COM. Vous pouvez vous assurer qu’à l’aide de la `noexcept` spécificateur sur vos méthodes COM. Il est OK pour les exceptions à lever n’importe où dans le graphique des appels de votre méthode, tant que vous les gérez avant la fermeture de votre méthode. Si vous utilisez `noexcept`, mais que vous autorisez ensuite une exception comme caractère d’échappement votre méthode, alors votre application s’arrêtera.

## <a name="add-helper-types-and-functions"></a>Ajouter des fonctions et des types d’assistance

Dans cette étape, nous allons ajouter certains types d’assistance et les fonctions qui rend le reste du code utilisent de. C’est le cas, avant de `main`, ajoutez le code suivant.

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

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>Implémentez les fonctions restantes et la fonction de point d’entrée wmain

Le modèle de projet génère un `main` fonction pour vous. Supprimez cet `main` fonctionne et à sa place collez ce code de description, qui inclut le code pour inscrire votre coclasse, puis à fournir un toast capable d’appeler de nouveau votre application.

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
        std::wcout << this_app_name << L" is running" << (runningAsAdmin ? L" (Administrator)." : L".") << std::endl;

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

## <a name="how-to-test-the-example-application"></a>Comment tester l’exemple d’application

Générer l’application et ensuite l’exécuter au moins une fois en tant qu’administrateur pour forcer l’inscription et autre programme d’installation, exécution de code. Si vous exécutez en tant qu’administrateur, puis appuyez sur est utilisée» pour provoquer un toast à afficher. Vous pouvez alors cliquer le bouton **rappeler ToastAndCallback** soit directement à partir de la notification toast que POP vers le haut, ou à partir du centre de maintenance et que votre application est lancée, la coclasse instanciée et le **INotificationActivationCallback :: Activer les** méthode exécutée.

## <a name="in-process-com-server"></a>Serveur COM in-process

L’application d’exemple *ToastAndCallback* ci-dessus fonctionne comme un serveur COM local (ou out-of-process). Cela est indiqué par la clé de Registre de Windows [LocalServer32](/windows/desktop/com/localserver32) que vous utilisez pour inscrire le CLSID de sa coclasse. Un serveur COM local héberge son coclass(es) à l’intérieur d’un binaire exécutable (une `.exe`).

Vous pouvez également (et sans doute plus probable), vous pouvez choisir d’héberger votre coclass(es) à l’intérieur d’une bibliothèque de liens dynamiques (un `.dll`). Un serveur COM sous la forme d’une DLL est appelé un serveur de COM in-process, et il est indiqué par le CLSID en cours d’inscription à l’aide de la clé de Registre de Windows [InprocServer32](/windows/desktop/com/inprocserver32) .

### <a name="create-a-dynamic-link-library-dll-project"></a>Créez un projet de bibliothèque de liens dynamiques (DLL)

Vous pouvez commencer la tâche de création d’un serveur de COM in-process en créant un nouveau projet dans Microsoft Visual Studio. Créer un **Visual C++** > **Windows Desktop** > projet de**Bibliothèque de liens dynamiques (DLL)** .

Pour ajouter C++ / WinRT prise en charge pour le nouveau projet, suivez les étapes décrites dans [Modifier un projet d’application de bureau Windows pour ajouter C++ / WinRT support](/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>Implémentez la coclasse, fabrique de classe et des exportations de serveur in-process

Ouvrez `dllmain.cpp`et à y ajouter le listing du code indiqué ci-dessous.

Si vous disposez déjà d’une DLL qui implémente C++ / WinRT Windows Runtime classes, puis vous avez déjà la fonction **DllCanUnloadNow** illustrée ci-dessous. Si vous souhaitez ajouter des coclasses à cette DLL, vous pouvez ajouter la fonction **DllGetClassObject** .

Si dépourvus de code de [Bibliothèque de modèles Windows Runtime C++ (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) existant que vous voulez rester compatible avec, puis vous pouvez supprimer les parties WRL à partir du code indiqué.

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

Consultez également [références faibles en C++ / WinRT](weak-references.md#weak-references-in-cwinrt).

C++ / WinRT (plus précisément, le modèle de structure de base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) ) implémente [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) pour vous si votre type implémente [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (ou n’importe quelle interface qui dérive de **IInspectable**).

Il s’agit dans la mesure où **IWeakReferenceSource** et [**IWeakReference**](/windows/desktop/api/weakreference/nn-weakreference-iweakreference) sont conçus pour les types Windows Runtime. Par conséquent, vous pouvez activer la prise en charge de la référence faible pour votre coclasse en ajoutant simplement **winrt::Windows::Foundation::IInspectable** (ou une interface qui dérive de **IInspectable**) à votre implémentation.

```cppwinrt
struct MyCoclass : winrt::implements<MyCoclass, IMyComInterface, winrt::Windows::Foundation::IInspectable>
{
    //  ...
};
```

## <a name="important-apis"></a>API importantes
* [Interface IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [Interface IUnknown](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [Modèle de structure winrt::implements](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>Rubriquesassociées
* [Créer des API avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [Utiliser des composants COM avec C++/WinRT](consume-com.md)
* [Envoyer une notification toast locale](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
