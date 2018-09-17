---
author: stevewhims
description: C++ / WinRT peut vous aider à créer des composants COM classiques, tout comme il vous aide à créer des classes Windows Runtime.
title: Créer des composants COM avec C++ / WinRT
ms.author: stwhi
ms.date: 09/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, standard, c++, cpp, winrt, projection, auteur, COM, composant
ms.localizationpriority: medium
ms.openlocfilehash: 729cfae39f302ae6b5bae275d9e28a39f3d9503b
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/17/2018
ms.locfileid: "3985780"
---
# <a name="author-com-components-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Créer des composants COM avec [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

C++ / WinRT peut vous aider à créer de classique modèle COM (Component Object) composants (ou coclasses), tout comme il vous aide à créer des classes Windows Runtime. Voici une illustration très simple, qui vous pouvez tester si vous le collez dans le `main.cpp` d’une nouvelle **Windows Console Application (C++ / WinRT)** projet.

```cppwinrt
// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;

int main()
{
    init_apartment();

    struct MyCoclass : winrt::implements<MyCoclass, IPersist>
    {
        HRESULT STDMETHODCALLTYPE GetClassID(CLSID* id) noexcept override
        {
            *id = IID_IPersist; // Doesn't matter what we return, for this example.
            return S_OK;
        }
    };

    auto mycoclass_instance{ winrt::make<MyCoclass>() };
    CLSID id{};
    winrt::check_hresult(mycoclass_instance->GetClassID(&id));
}
```

Consultez également [composants COM consommer avec C++ / WinRT](consume-com.md).

## <a name="a-more-realistic-and-interesting-example"></a>Un exemple plus réaliste et intéressant

Le reste de cette rubrique vous guide tout au processus de création d’un projet d’application de console minimal qui utilise C++ / WinRT pour implémenter une fabrique coclasse et de la classe de base. L’exemple d’application montre comment fournir une notification toast avec un bouton de rappel dessus et la coclasse (qui implémente l’interface **INotificationActivationCallback** COM) permet à l’application d’être lancée et appelée d’arrière-plan lorsque l’utilisateur clique sur ce bouton du toast.

Vous trouverez des explications plus détaillées sur la zone de fonctionnalité de notification toast à [Envoyer une notification toast locale](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast). Aucun des exemples de code dans cette section de la documentation utiliser C++ / WinRT, cependant, nous vous conseillons donc que vous préférez le code présenté dans cette rubrique.

## <a name="create-a-windows-console-application-project-toastandcallback"></a>Créez un projet d’Application Console Windows (ToastAndCallback)

Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créer un **Visual C++** > **Windows Desktop** > **Windows Console Application (C++ / WinRT)** de projet et nommez-le *ToastAndCallback*.

Ouvrez `main.cpp`et supprimer les using-directives qui génère le modèle de projet. À leur place, collez le code suivant (ce qui nous obtenons l’et les bibliothèques, les en-têtes et les noms de type que nous devons).

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

En C++ / WinRT, vous implémentez coclasses et les fabriques de classe, en dérivant de la structure de base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) . Immédiatement après les trois-directives using ci-dessus (et avant `main`), collez ce code pour implémenter votre composant d’activateur de notification COM toast.

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

L’implémentation de la coclasse ci-dessus suit le même modèle qui est présenté dans [créer des API avec C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class). Notez que vous pouvez utiliser cette technique non seulement pour les interfaces Windows Runtime (toute interface qui finalement dérive [**IInspectable**](https://msdn.microsoft.com/library/br205821)), mais aussi pour implémenter les interfaces COM (toute interface qui finalement dérive de [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)).

Dans la coclasse dans le code ci-dessus, nous implémentez la méthode **INotificationActivationCallback::Activate** , qui est la fonction qui est appelée lorsque l’utilisateur clique sur le bouton de rappel sur une notification toast. Toutefois, avant que cette fonction peut être appelée, une instance de la coclasse doit être créé, et c’est le rôle de la fonction **IClassFactory::CreateInstance** .

La coclasse que nous avons simplement implémentées est appelée l' *activateur COM* pour les notifications, et qui présente son id de classe (CLSID) sous la forme de la `callback_guid` identificateur (de type **GUID**) que vous voyez ci-dessus. Nous allons utiliser cet identificateur plus tard, sous la forme d’un raccourci du menu Démarrer et une entrée de Registre Windows. L’activateur COM CLSID et le chemin d’accès à son serveur COM associé (qui est le chemin d’accès de l’exécutable que nous créons ici) est le mécanisme par lequel une notification toast sait quel de classe pour créer une instance de lorsque son rappel de bouton est cliqué (si le notification est effectuée dans le centre de notifications ou non).

## <a name="best-practices-for-implementing-com-methods"></a>Meilleures pratiques pour l’implémentation des méthodes de COM

Techniques de gestion des erreurs et de gestion de ressources peuvent accéder en pair. Il est plus pratique et plus pratique d’utiliser les exceptions que les codes d’erreur. Et si vous n’utilisiez l’idiome (RAII) de ressource acquisition-est-d’initialisation, puis vous pouvez éviter explicitement vérification des codes d’erreur et en libérant explicitement les ressources. Ces contrôles explicite que votre code alambiqué profiter plus que nécessaire, et vous permet de bogues beaucoup d’endroits à masquer. Au lieu de cela, utilisez RAII et lever/catch exceptions. De cette façon, votre allocations de ressources sont à toute exception, et votre code est simple.

Toutefois, vous up ne doit pas autoriser les exceptions comme caractère d’échappement vos implémentations de la méthode COM. Vous pouvez vous assurer qu’à l’aide de la `noexcept` spécificateur sur vos méthodes COM. Il est OK pour les exceptions à lever n’importe où dans le graphique des appels de votre méthode, tant que vous les gérez avant la fermeture de votre méthode. Si vous utilisez `noexcept`, mais que vous autorisez ensuite une exception d’échappement votre méthode, alors votre application se termine.

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

Le modèle de projet génère un `main` fonction pour vous. Supprimez cet `main` fonctionner et à sa place collez ce code description, qui inclut le code pour inscrire votre coclasse, puis à fournir un toast capable d’appeler de nouveau votre application.

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
    init_apartment();

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

## <a name="how-to-test-the-example-application"></a>Comment faire pour tester l’exemple d’application

Générer l’application et ensuite l’exécuter au moins une fois en tant qu’administrateur entraîner l’inscription, ainsi que les autres paramètres, exécution de code. Vous exécutez en tant qu’administrateur ou non, puis appuyez sur la touche est utilisée» pour provoquer un toast à afficher. Vous pouvez ensuite cliquer sur le bouton **rappeler ToastAndCallback** soit directement à partir de la notification toast que POP vers le haut, ou à partir du centre de maintenance et que votre application est lancée, la coclasse instanciée et le **INotificationActivationCallback :: Activer les** méthode exécutée.

## <a name="important-apis"></a>API importantes
* [Interface IInspectable](https://msdn.microsoft.com/library/br205821)
* [Interface IUnknown](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [Modèle de structure winrt::implements](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>Rubriquesassociées
* [Créer des API avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [Utiliser les composants COM avec C++ / WinRT](consume-com.md)
* [Envoyer une notification toast locale](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
