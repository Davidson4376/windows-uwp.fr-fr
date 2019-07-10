---
description: C++/WinRT peut vous aider à créer des composants COM classiques, comme il vous aide à créer des classes Windows Runtime.
title: Créer des composants COM avec C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, créer, COM, composant
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3badcd59155bc4bb5ef8d9e29271b853c245c24e
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66360321"
---
# <a name="author-com-components-with-cwinrt"></a>Créer des composants COM avec C++/WinRT

[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) peut vous aider à créer des composants Component Object Model (COM) (ou coclasses) classiques, comme il vous aide à créer des classes Windows Runtime. Voici un exemple simple que vous pouvez tester si vous collez le code dans les éléments `pch.h` et `main.cpp` d’un nouveau projet **Application console Windows (C++/WinRT)** .

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

Voir aussi [Consommer des composants COM avec C++/WinRT](consume-com.md).

## <a name="a-more-realistic-and-interesting-example"></a>Un exemple plus réaliste et intéressant

Le reste de cette rubrique aborde la création d'un projet d'application console minimal qui utilise C++/WinRT pour implémenter une coclasse de base (composant COM, ou classe COM) et une fabrique de classes. L’exemple d’application montre comment fournir une notification toast avec un bouton de rappel, et la coclasse (qui implémente l'interface COM **INotificationActivationCallback**) permet de lancer et de rappeler l'application lorsque l'utilisateur clique sur ce bouton sur le toast.

Pour plus d'informations sur la fonction de notification toast, consultez la section [Envoyer une notification toast locale](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast). Aucun des exemples de code dans cette section de la documentation n'utilise C++/WinRT. Nous vous recommandons donc de privilégier le code montré dans cette rubrique.

## <a name="create-a-windows-console-application-project-toastandcallback"></a>Créer un projet Application console Windows (ToastAndCallback)

Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créez un projet **Application console Windows (C++/WinRT)** et nommez-le *ToastAndCallback*.

Ouvrez `pch.h` et ajoutez `#include <unknwn.h>` avant les éléments inclus pour tous les en-têtes C++/WinRT. Voici le résultat : vous pouvez remplacer le contenu de votre `pch.h` par ce listing.

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

Ouvrez `main.cpp` et supprimez les directives d’utilisation que génère le modèle de projet. À la place, insérez le code suivant (qui nous donne les bibliothèques, les en-têtes et les noms de types nécessaires). Voici le résultat : vous pouvez remplacer le contenu de votre `main.cpp` par ce listing (nous avons également supprimé le code de `main` dans le listing ci-dessous, puisque nous allons remplacer cette fonction plus tard).

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

Le projet n’est pas encore généré ; après avoir terminé l’ajout du code, vous serez invité à le générer et à l’exécuter.

## <a name="implement-the-coclass-and-class-factory"></a>Implémenter la coclasse et la fabrique de classes

Dans C++/WinRT, vous implémentez des coclasses et des fabriques de classes en les dérivant de la structure de base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements). Immédiatement après les trois directives d’utilisation ci-dessus (et avant `main`), collez ce code pour implémenter votre composant activateur COM de notification toast.

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

L’implémentation de la coclasse ci-dessus suit le même modèle que celui illustré dans [Créer des API avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class). Par conséquent, vous pouvez utiliser la même technique pour implémenter des interfaces COM et des interfaces Windows Runtime. Les composants COM et classes Windows Runtime exposent leurs fonctionnalités via des interfaces. Chaque interface COM est dérivée en dernier lieu de l’interface [**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown). Windows Runtime est basé sur COM&mdash;à la différence que les interfaces Windows Runtime sont dérivées en dernier lieu de l’[**interface IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (et **IInspectable**  dérive de **IUnknown**).

Dans la coclasse du code ci-dessus, nous implémentons la méthode **INotificationActivationCallback::Activate**, qui représente la fonction appelée lorsque l’utilisateur clique sur le bouton de rappel sur une notification toast. Mais avant de pouvoir appeler cette fonction, une instance de la coclasse doit être créée, et cette tâche incombe à la fonction **IClassFactory::CreateInstance**.

La coclasse que nous venons d’implémenter est appelée *activateur COM* pour les notifications, et elle comporte un ID de classe (CLSID) sous la forme de l’identificateur `callback_guid` (de type **GUID**) que vous voyez ci-dessus. Nous utiliserons cet identificateur plus tard, sous la forme d’un raccourci du menu Démarrer et d’une entrée du Registre Windows. Le CLSID de l’activateur COM, et le chemin d’accès à son serveur COM associé (chemin d’accès de l’exécutable que nous créons ici), est le mécanisme permettant à une notification toast d’identifier la classe à partir de laquelle créer une instance lorsque vous cliquez sur le bouton de rappel (si cette option est activée ou non dans le centre de notifications).

## <a name="best-practices-for-implementing-com-methods"></a>Meilleures pratiques pour implémenter des méthodes COM

Les techniques de gestion des erreurs et des ressources peuvent fonctionner de pair. Il est plus pratique d’utiliser des exceptions que des codes d’erreur. Et si vous employez l’idiome resource-acquisition-is-initialization (RAII), vous pouvez éviter la vérification explicite des codes d’erreur et libérer explicitement des ressources. Ce type de contrôle explicite rend votre code plus compliqué que nécessaire, et risque de générer de nombreux bogues difficiles à détecter. Utilisez plutôt RAII et des exceptions throw/catch. De cette façon, vos allocations de ressources sont sécurisées pour les exceptions, et votre code reste simple.

Toutefois, vous ne doit pas autoriser les exceptions à échapper à vos implémentations de la méthode COM. Pour vous en assurer, utilisez le spécificateur `noexcept` sur vos méthodes COM. Les exceptions peuvent être levées n'importe où dans le graphique des appels de votre méthode, tant que vous les gérez avant que votre méthode n'existe. Si vous utilisez `noexcept`, mais permettez à une exception d’échapper à votre méthode, votre application s’arrêtera.

## <a name="add-helper-types-and-functions"></a>Ajouter des types et des fonctions d’assistance

Dans cette étape, nous allons ajouter des types et des fonctions d’assistance que le reste du code utilisera. Par conséquent, immédiatement avant `main`, ajoutez les éléments suivants.

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

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>Implémenter les fonctions restantes et la fonction du point d’entrée wmain

Supprimez votre fonction `main` et, à la place, collez ce listing de code, qui inclut le code pour inscrire votre coclasse, puis fournissez un toast capable de rappeler votre application.

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

Générez l’application puis exécutez-la au moins une fois en tant qu’administrateur afin d’exécuter le code pour l’inscription et d’autres tâches de configuration. Un moyen d’y parvenir consiste à exécuter Visual Studio en tant qu’administrateur et à exécuter l’application à partir de Visual Studio. Cliquez avec le bouton droit dans la barre des tâches de Visual Studio pour afficher la liste de raccourcis, cliquez avec le bouton droit sur Visual Studio, puis cliquez sur **Exécuter en tant qu’administrateur**. Acceptez l’invite, puis ouvrez le projet. Lorsque vous exécutez l’application, un message indique si l’application s’exécute en tant qu’administrateur. Si ce n’est pas le cas, l’inscription et les autres tâches de configuration ne s’exécuteront pas. Cette inscription et les autres tâches de configuration doivent s’exécuter au moins une fois pour que l’application fonctionne correctement.

Que vous exécutiez l’application en tant qu’administrateur ou non, appuyez sur 'T' pour afficher une notification toast. Vous pouvez ensuite cliquer sur le bouton **Rappeler ToastAndCallback** directement depuis la notification toast qui s’affiche, ou à partir du centre de maintenance, pour lancer votre application, instancier la coclasse et exécuter la méthode **INotificationActivationCallback::Activate**.

## <a name="in-process-com-server"></a>Serveur COM in-process

L’exemple d’application *ToastAndCallback* ci-dessus fonctionne comme un serveur COM local (ou out-of-process). Cela est indiqué par la clé de Registre Windows [LocalServer32](/windows/desktop/com/localserver32) que vous utilisez pour inscrire le CLSID de sa coclasse. Un serveur COM local héberge ses coclasses à l’intérieur d’un binaire exécutable (un `.exe`).

Vous pouvez également (et ce sera le cas le plus probable) choisir d’héberger vos coclasses à l’intérieur d’une bibliothèque de liens dynamiques (`.dll`). Un serveur COM sous la forme d’une DLL est connu comme un serveur COM in-process, et il est indiqué par le CLSID en cours d’inscription à l’aide de la clé de Registre Windows [InprocServer32](/windows/desktop/com/inprocserver32).

### <a name="create-a-dynamic-link-library-dll-project"></a>Créer un projet de bibliothèque de liens dynamiques (DLL)

Vous pouvez commencer la tâche de création d’un serveur COM in-process en créant un projet dans Microsoft Visual Studio. Créez un projet **Visual C++**  > **Windows Desktop** > **Dynamic-Link Library (DLL)** .

Pour ajouter la prise en charge de C++/WinRT au nouveau projet, suivez les étapes décrites dans [Modifier un projet d’application de bureau Windows pour ajouter la prise en charge de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>Implémenter la coclasse, la fabrique de classes et des exportations de serveur in-process

Ouvrez `dllmain.cpp` et ajoutez-y le listing de code indiqué ci-dessous.

Si vous disposez déjà d’une DLL qui implémente des classes Windows Runtime C++/WinRT, vous avez déjà la fonction **DllCanUnloadNow** indiquée ci-dessous. Si vous souhaitez ajouter des coclasses à cette DLL, vous pouvez ajouter la fonction **DllGetClassObject**.

Si n’avez pas de code [Bibliothèque de modèles C++ Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) existant avec lequel vous souhaitez assurer la compatibilité, vous pouvez supprimer les parties WRL du code affiché.

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

Voir également [Références faibles dans C++/WinRT](weak-references.md#weak-references-in-cwinrt).

C++/WinRT (plus précisément le modèle de structure de base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)) implémente [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) pour vous si votre type implémente [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (ou n’importe quelle interface dérivée de **IInspectable**).

Cela est dû au fait que **IWeakReferenceSource** et [**IWeakReference**](/windows/desktop/api/weakreference/nn-weakreference-iweakreference) sont conçus pour les types Windows Runtime. Vous pouvez donc activer la prise en charge des références faibles pour votre coclasse en ajoutant simplement **winrt::Windows::Foundation::IInspectable** (ou une interface dérivée de **IInspectable**) à votre implémentation.

```cppwinrt
struct MyCoclass : winrt::implements<MyCoclass, IMyComInterface, winrt::Windows::Foundation::IInspectable>
{
    //  ...
};
```

## <a name="important-apis"></a>API importantes
* [Interface IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [Interface IUnknown](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)
* [Modèle de structure winrt::implements](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>Rubriques connexes
* [Créer des API avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [Utiliser des composants COM avec C++/WinRT](consume-com.md)
* [Envoyer une notification toast locale](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
