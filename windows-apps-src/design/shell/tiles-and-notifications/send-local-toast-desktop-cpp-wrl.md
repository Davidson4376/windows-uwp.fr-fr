---
Description: Découvrez comment les applications Win32 C++ WRL peuvent envoyer des notifications toast local et gérer l’utilisateur clique sur le toast.
title: Envoyer une notification toast locale depuis des applications WRL de bureau en C++
label: Send a local toast notification from desktop C++ WRL apps
template: detail.hbs
ms.date: 03/07/2018
ms.topic: article
keywords: windows 10, uwp, win32, bureau, notifications toast, envoyer un toast, envoyer un toast local, pont du bureau, C++, cpp, cplusplus, WRL
ms.localizationpriority: medium
ms.openlocfilehash: 82de349009350c970fce923a2aa503df0801c3b7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57609844"
---
# <a name="send-a-local-toast-notification-from-desktop-c-wrl-apps"></a>Envoyer une notification toast locale depuis des applications WRL de bureau en C++

Les applications de bureau (Pont du bureau et Win32 classique) peuvent envoyer des notifications toast interactives, tout comme les applications de plateforme Windows universelle (UWP). Toutefois, il existe quelques étapes spéciales pour les applications de bureau en raison des différents schémas d’activation et de l’absence potentielle d’identité de package si vous n’utilisez pas le Pont du bureau.

> [!IMPORTANT]
> Si vous écrivez une application UWP, consultez la [documentation UWP](send-local-toast.md). Pour les autres langues de bureau, consultez [Applications de bureau en C#](send-local-toast-desktop.md).


## <a name="step-1-enable-the-windows-10-sdk"></a>Étape 1 : Activer Windows 10 SDK

Vous devez d’abord activer le SDK Windows 10 pour votre application Win32 si ce n’est pas déjà fait. Quelques étapes sont importantes...

1. Ajoutez `runtimeobject.lib` aux **Dépendances supplémentaires**
2. Ciblez le SDK Windows 10

Cliquez avec le bouton droit sur votre projet et sélectionnez **Propriétés**.

Dans le menu **Configuration** dans la partie supérieure, sélectionnez **Toutes les configurations** afin que la modification suivante soit appliquée aux configurations de débogage et de publication.

Sous **Éditeur de liens -> Entrée**, ajoutez `runtimeobject.lib` aux **Dépendances supplémentaires**.

Puis, sous **Général**, assurez-vous que la **Version du SDK Windows** est définie sur 10.0 ou une version ultérieure (pas Windows 8.1).


## <a name="step-2-copy-compat-library-code"></a>Étape 2 : Copiez le code de bibliothèque de compatibilité

Copiez le fichier [DesktopNotificationManagerCompat.h](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.h) et [DesktopNotificationManagerCompat.cpp](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.cpp) de GitHub dans votre projet. La bibliothèque de compatibilité fait abstraction en grande partie de la complexité des notifications de bureau. Les instructions suivantes requièrent la bibliothèque de compatibilité.

Si vous utilisez des en-têtes précompilés, veillez à `#include "stdafx.h"` comme première ligne du fichier DesktopNotificationManagerCompat.cpp.


## <a name="step-3-include-the-header-files-and-namespaces"></a>Étape 3 : Inclure les fichiers d’en-tête et les espaces de noms

Incluez le fichier d’en-tête de la bibliothèque de compatibilité, ainsi que les fichiers d’en-tête et les espaces de noms associés à l’utilisation des API de toast UWP.

```cpp
#include "DesktopNotificationManagerCompat.h"
#include <NotificationActivationCallback.h>
#include <windows.ui.notifications.h>

using namespace ABI::Windows::Data::Xml::Dom;
using namespace ABI::Windows::UI::Notifications;
using namespace Microsoft::WRL;
```


## <a name="step-4-implement-the-activator"></a>Étape 4 : Implémenter l’activateur

Vous devez implémenter un gestionnaire pour l’activation des toasts de manière à ce que votre application puisse effectuer une action lorsque l’utilisateur clique sur votre toast. Cela est nécessaire pour que votre toast soit conservé dans le centre de notifications (car l’utilisateur peut cliquer sur le toast des jours plus tard lorsque votre application est fermée). Cette classe peut être placée n’importe où dans votre projet.

Implémentez l’interface **INotificationActivationCallback** comme illustré ci-dessous, y compris un UUID, et appelez également **CoCreatableClass** pour marquer votre classe comme pouvant être créée par COM. Pour votre UUID, créez un GUID unique à l’aide de l’un des nombreux générateurs GUID en ligne. Ce CLSID GUID (identificateur de classe) permet au centre de notifications de savoir quelle classe activer par COM.

```cpp
// The UUID CLSID must be unique to your app. Create a new GUID if copying this code.
class DECLSPEC_UUID("replaced-with-your-guid-C173E6ADF0C3") NotificationActivator WrlSealed WrlFinal
    : public RuntimeClass<RuntimeClassFlags<ClassicCom>, INotificationActivationCallback>
{
public:
    virtual HRESULT STDMETHODCALLTYPE Activate(
        _In_ LPCWSTR appUserModelId,
        _In_ LPCWSTR invokedArgs,
        _In_reads_(dataCount) const NOTIFICATION_USER_INPUT_DATA* data,
        ULONG dataCount) override
    {
        // TODO: Handle activation
    }
};

// Flag class as COM creatable
CoCreatableClass(NotificationActivator);
```


## <a name="step-5-register-with-notification-platform"></a>Étape 5 : Inscrire avec la plateforme de notification

Vous devez ensuite vous inscrire sur la plateforme de notification. Il existe différentes étapes selon que vous utilisez le Pont du bureau ou Win32 classique. Si vous prenez en charge les deux, vous devez effectuer les deux étapes. Toutefois, il n’est pas nécessaire de répliquer votre code, notre bibliothèque s’en occupe pour vous !


### <a name="desktop-bridge"></a>Pont du bureau

Si vous utilisez le Pont du bureau (ou si vous prenez en charge les deux), dans votre fichier **Package.appxmanifest**, ajoutez :

1. Déclaration de **xmlns:com**
2. Déclaration de **xmlns:desktop**
3. Dans l’attribut **IgnorableNamespaces**, **com** et **desktop**
4. **com:Extension** pour l'activateur COM à l’aide du GUID de l’étape 4. Veillez à inclure l’objet `Arguments="-ToastActivated"` afin que vous sachiez que votre lancement provenait d’un toast
5. **desktop:Extension** pour **windows.toastNotificationActivation** pour déclarer le CLSID de votre activateur de toast (le GUID de l’étape 4).

**Package.appxmanifest**

```xml
<Package
  ...
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="... com desktop">
  ...
  <Applications>
    <Application>
      ...
      <Extensions>

        <!--Register COM CLSID LocalServer32 registry key-->
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="YourProject\YourProject.exe" Arguments="-ToastActivated" DisplayName="Toast activator">
              <com:Class Id="replaced-with-your-guid-C173E6ADF0C3" DisplayName="Toast activator"/>
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>

        <!--Specify which CLSID to activate when toast clicked-->
        <desktop:Extension Category="windows.toastNotificationActivation">
          <desktop:ToastNotificationActivation ToastActivatorCLSID="replaced-with-your-guid-C173E6ADF0C3" /> 
        </desktop:Extension>

      </Extensions>
    </Application>
  </Applications>
 </Package>
```


### <a name="classic-win32"></a>Win32 classique

Si vous utilisez Win32 classique (ou si vous prenez en charge les deux), vous devez déclarer l’ID de modèle utilisateur de l’application (AUMID) et le CLSID de l’activateur de toast (le GUID de l’étape 4) dans le raccourci de votre application dans le menu Démarrer.

Indiquez un AUMID unique qui identifie votre application Win32. Il se présente généralement sous la forme [CompanyName].[AppName], mais vous devez veiller à ce qu’il soit unique dans toutes les applications (vous pouvez ajouter des chiffres à la fin).

#### <a name="step-51-wix-installer"></a>Étape 5.1 : Programme d’installation WiX

Si vous utilisez WiX pour votre programme d’installation, modifiez le fichier **Product.wxs** pour ajouter les deux propriétés de raccourci au raccourci du menu Démarrer, comme illustré ci-dessous. Assurez-vous que le GUID de l’étape 4 est placé entre `{}`, comme indiqué ci-dessous.

**Product.wxs**

```xml
<Shortcut Id="ApplicationStartMenuShortcut" Name="Wix Sample" Description="Wix Sample" Target="[INSTALLFOLDER]WixSample.exe" WorkingDirectory="INSTALLFOLDER">
                    
    <!--AUMID-->
    <ShortcutProperty Key="System.AppUserModel.ID" Value="YourCompany.YourApp"/>
    
    <!--COM CLSID-->
    <ShortcutProperty Key="System.AppUserModel.ToastActivatorCLSID" Value="{replaced-with-your-guid-C173E6ADF0C3}"/>
    
</Shortcut>
```

> [!IMPORTANT]
> Pour utiliser réellement les notifications, vous devez installer votre application via le programme d’installation une fois avant le débogage normal, afin que le raccourci du menu Démarrer avec votre AUMID et CLSID soit présent. Une fois que le raccourci du menu Démarrer est présent, vous pouvez déboguer à l’aide de F5 dans Visual Studio.


#### <a name="step-52-register-aumid-and-com-server"></a>Étape 5.2 : Inscrire le serveur COM et identifiant AUMID

Ensuite, quel que soit votre programme d’installation, dans le code de démarrage de votre application (avant d’appeler des API de notification), appelez la méthode **RegisterAumidAndComServer**, en spécifiant la classe de votre activateur de notification à l’étape 4 et l’AUMID utilisé ci-dessus.

```cpp
// Register AUMID and COM server (for Desktop Bridge apps, this no-ops)
hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"YourCompany.YourApp", __uuidof(NotificationActivator));
```

Si vous prenez en charge le Pont du bureau et Win32 classique, vous pouvez appeler cette méthode avec l’application de votre choix. Si vous utilisez le Pont du bureau, cette méthode est retournée immédiatement. Il n'est pas nécessaire de répliquer votre code.

Cette méthode vous permet d’appeler les API de compatibilité pour envoyer et gérer les notifications sans avoir à fournir constamment votre AUMID. La clé de Registre LocalServer32 est également insérée pour le serveur COM.


## <a name="step-6-register-com-activator"></a>Étape 6 : Inscrire l’activateur de COM

Pour les applications Pont du bureau et Win32 classique, vous devez enregistrer votre type d’activateur de notification, afin de pouvoir gérer les activations de toast.

Dans le code de démarrage de votre application, appelez la méthode **RegisterActivator** suivante. Elle doit être appelée afin de pouvoir recevoir des activations de toast.

```cpp
// Register activator type
hr = DesktopNotificationManagerCompat::RegisterActivator();
```


## <a name="step-7-send-a-notification"></a>Étape 7 : Envoyer une notification

L’envoi d’une notification est identique aux applications UWP, sauf que vous utilisez **DesktopNotificationManagerCompat** pour créer un objet **ToastNotifier**. La bibliothèque de compatibilité gère automatiquement la différence entre le Pont du bureau et Win32 classique, vous n’avez donc pas besoin de répliquer votre code. Pour Win32 classique, la bibliothèque de compatibilité met en cache l’AUMID que vous avez fourni lors de l’appel de **RegisterAumidAndComServer** de manière à ce que vous n’ayez pas à vous soucier de fournir ou non l’AUMID.

Assurez-vous d’utiliser la liaison **ToastGeneric** comme indiqué ci-dessous, car les modèles de notification toast Windows 8.1 hérités n’activent pas l’activateur de notification COM que vous avez créé à l’étape 4.

> [!IMPORTANT]
> Les images http sont uniquement prises en charge dans les applications Pont du bureau qui disposent de la fonctionnalité Internet dans leur manifeste. Les applications Win32 classiques ne prennent pas en charge les images http ; vous devez télécharger l’image dans vos données d’application locales et la référencer localement.

```cpp
// Construct XML
ComPtr<IXmlDocument> doc;
hr = DesktopNotificationManagerCompat::CreateXmlDocumentFromString(
    L"<toast><visual><binding template='ToastGeneric'><text>Hello world</text></binding></visual></toast>",
    &doc);
if (SUCCEEDED(hr))
{
    // See full code sample to learn how to inject dynamic text, buttons, and more

    // Create the notifier
    // Classic Win32 apps MUST use the compat method to create the notifier
    ComPtr<IToastNotifier> notifier;
    hr = DesktopNotificationManagerCompat::CreateToastNotifier(&notifier);
    if (SUCCEEDED(hr))
    {
        // Create the notification itself (using helper method from compat library)
        ComPtr<IToastNotification> toast;
        hr = DesktopNotificationManagerCompat::CreateToastNotification(doc.Get(), &toast);
        if (SUCCEEDED(hr))
        {
            // And show it!
            hr = notifier->Show(toast.Get());
        }
    }
}
```

> [!IMPORTANT]
> Les applications Win32 classiques ne peuvent pas utiliser les modèles de notifications toast hérités (comme ToastText02). L’activation des modèles hérités échoue lorsque le CLSID COM est spécifié. Vous devez utiliser les modèles de Windows 10 ToastGeneric comme indiqué ci-dessus.


## <a name="step-8-handling-activation"></a>Étape 8 : Gestion de l’activation

Lorsque l’utilisateur clique sur votre toast ou sur les boutons du toast, la méthode **Activate** de votre classe **NotificationActivator** est appelée.

Dans la méthode Activate, vous pouvez analyser les arguments que vous avez spécifiés dans le toast et obtenir l’entrée utilisateur que l’utilisateur a saisi ou sélectionné, puis activer votre application en conséquence.

> [!NOTE]
> La méthode **Activate** est appelée sur un thread distinct de votre thread principal.

```cpp
// The GUID must be unique to your app. Create a new GUID if copying this code.
class DECLSPEC_UUID("replaced-with-your-guid-C173E6ADF0C3") NotificationActivator WrlSealed WrlFinal
    : public RuntimeClass<RuntimeClassFlags<ClassicCom>, INotificationActivationCallback>
{
public: 
    virtual HRESULT STDMETHODCALLTYPE Activate(
        _In_ LPCWSTR appUserModelId,
        _In_ LPCWSTR invokedArgs,
        _In_reads_(dataCount) const NOTIFICATION_USER_INPUT_DATA* data,
        ULONG dataCount) override
    {
        std::wstring arguments(invokedArgs);
        HRESULT hr = S_OK;

        // Background: Quick reply to the conversation
        if (arguments.find(L"action=reply") == 0)
        {
            // Get the response user typed.
            // We know this is first and only user input since our toasts only have one input
            LPCWSTR response = data[0].Value;

            hr = DesktopToastsApp::SendResponse(response);
        }

        else
        {
            // The remaining scenarios are foreground activations,
            // so we first make sure we have a window open and in foreground
            hr = DesktopToastsApp::GetInstance()->OpenWindowIfNeeded();
            if (SUCCEEDED(hr))
            {
                // Open the image
                if (arguments.find(L"action=viewImage") == 0)
                {
                    hr = DesktopToastsApp::GetInstance()->OpenImage();
                }

                // Open the app itself
                // User might have clicked on app title in Action Center which launches with empty args
                else
                {
                    // Nothing to do, already launched
                }
            }
        }

        if (FAILED(hr))
        {
            // Log failed HRESULT
        }

        return S_OK;
    }

    ~NotificationActivator()
    {
        // If we don't have window open
        if (!DesktopToastsApp::GetInstance()->HasWindow())
        {
            // Exit (this is for background activation scenarios)
            exit(0);
        }
    }
};

// Flag class as COM creatable
CoCreatableClass(NotificationActivator);
```

Pour assurer une prise en charge correcte du lancement lorsque votre application est fermée, dans la fonction WinMain, vous pouvez déterminer si le lancement s’effectue à partir d’un toast ou non. Si le lancement s’effectue à partir d’un toast, l’argument de lancement « -ToastActivated » est utilisé. Lorsque cet argument s’affiche, vous devez arrêter l’exécution de tout code d’activation de lancement normal et autoriser **NotificationActivator** à gérer les fenêtres de lancement si nécessaire.

```cpp
// Main function
int WINAPI wWinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE, _In_ LPWSTR cmdLineArgs, _In_ int)
{
    RoInitializeWrapper winRtInitializer(RO_INIT_MULTITHREADED);

    HRESULT hr = winRtInitializer;
    if (SUCCEEDED(hr))
    {
        // Register AUMID and COM server (for Desktop Bridge apps, this no-ops)
        hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"WindowsNotifications.DesktopToastsCpp", __uuidof(NotificationActivator));
        if (SUCCEEDED(hr))
        {
            // Register activator type
            hr = DesktopNotificationManagerCompat::RegisterActivator();
            if (SUCCEEDED(hr))
            {
                DesktopToastsApp app;
                app.SetHInstance(hInstance);

                std::wstring cmdLineArgsStr(cmdLineArgs);

                // If launched from toast
                if (cmdLineArgsStr.find(TOAST_ACTIVATED_LAUNCH_ARG) != std::string::npos)
                {
                    // Let our NotificationActivator handle activation
                }

                else
                {
                    // Otherwise launch like normal
                    app.Initialize(hInstance);
                }

                app.RunMessageLoop();
            }
        }
    }

    return SUCCEEDED(hr);
}
```


### <a name="activation-sequence-of-events"></a>Ordre d’activation des événements

L’ordre d’activation est le suivant...

Si votre application est déjà en cours d’exécution :

1. **Activate** dans votre **NotificationActivator** est appelé

Si votre application n’est pas en cours d’exécution :

1. Votre application est lancée par EXE, vous obtenez « -ToastActivated » comme arguments de ligne de commande
2. **Activate** dans votre **NotificationActivator** est appelé


### <a name="foreground-vs-background-activation"></a>Activation au premier plan et en arrière-plan
Pour les applications de bureau, l’activation au premier plan et en arrière-plan est gérée de manière identique : votre activateur COM est appelé. C’est le code de votre application qui décide d’afficher une fenêtre ou d’exécuter simplement des tâches et de quitter. Par conséquent, spécifier **background** comme **activationType** dans le contenu du toast ne modifie pas le comportement.


## <a name="step-9-remove-and-manage-notifications"></a>Étape 9 : Supprimer et gérer les notifications

La suppression et la gestion des notifications sont identiques aux applications UWP. Toutefois, nous vous recommandons d’utiliser notre bibliothèque de compatibilité pour obtenir un objet **DesktopNotificationHistoryCompat** ; vous n’avez donc pas à vous soucier de fournir l’AUMID si vous utilisez Win32 classique.

```cpp
std::unique_ptr<DesktopNotificationHistoryCompat> history;
auto hr = DesktopNotificationManagerCompat::get_History(&history);
if (SUCCEEDED(hr))
{
    // Remove a specific toast
    hr = history->Remove(L"Message2");

    // Clear all toasts
    hr = history->Clear();
}
```


## <a name="step-10-deploying-and-debugging"></a>Étape 10 : Déploiement et débogage

Pour déployer et déboguer votre application Pont du bureau, consultez [Exécuter, déboguer et tester une application de bureau empaquetée](/windows/uwp/porting/desktop-to-uwp-debug).

Pour déployer et déboguer votre application Win32 classique, vous devez installer votre application via le programme d’installation une fois avant le débogage normal, afin que le raccourci du menu Démarrer avec votre AUMID et CLSID soit présent. Une fois que le raccourci du menu Démarrer est présent, vous pouvez déboguer à l’aide de F5 dans Visual Studio.

Si vos notifications ne parviennent pas à s’afficher dans votre application Win32 classique (et aucune exception n’est levée), cela signifie probablement que le raccourci du menu Démarrer n’est pas présent (installez votre application via le programme d’installation), ou l’AUMID que vous avez utilisé dans le code ne correspond pas à l’AUMID dans le raccourci du menu Démarrer.

Si vos notifications s’affichent, mais ne sont pas conservées dans le centre de notifications (disparaissent une fois que la fenêtre contextuelle est ignorée), cela signifie que vous n’avez pas implémenté l’activateur COM correctement.

Si vous avez installé votre application Pont du bureau et Win32 classique, notez que l’application Pont du bureau remplace l’application Win32 classique lors de la gestion des activations du toast. Cela signifie que les toasts de l’application Win32 classique lancent toujours l’application Pont du bureau lorsque l’utilisateur clique dessus. La désinstallation de l’application Pont du bureau rétablit les activations sur l’application Win32 classique.

Si vous recevez `HRESULT 0x800401f0 CoInitialize has not been called.`, veillez à appeler `CoInitialize(nullptr)` dans votre application avant d’appeler les API.

Si vous recevez `HRESULT 0x8000000e A method was called at an unexpected time.` lors de l’appel des APIs de comptabilité, cela signifie probablement que vous n’avez pas pu appeler les méthodes Register requises (ou dans le cas d’une application Pont du bureau, vous n’exécutez pas actuellement votre application dans le contexte du Pont du bureau).

Si vous obtenez plusieurs erreurs de compilation `unresolved external symbol`, vous avez probablement oublié d’ajouter `runtimeobject.lib` aux **Dépendances supplémentaires** à l’étape 1 (ou vous l’avez uniquement ajouté à la configuration de débogage, et non à la configuration de publication).


## <a name="handling-older-versions-of-windows"></a>Gestion des versions plus anciennes de Windows

Si vous prenez en charge Windows 8.1 ou une version inférieure, vous pouvez vérifier au moment de l’exécution si vous exécutez Windows 10 avant d’appeler des API **DesktopNotificationManagerCompat** ou d’envoyer des toasts ToastGeneric.

Windows 8 a introduit les notifications toast, mais a utilisé les [modèles de toast hérités](https://docs.microsoft.com/en-us/previous-versions/windows/apps/hh761494(v=win.10)), comme ToastText01. L’activation a été gérée par l’événement **Activated** en mémoire de la classe **ToastNotification**, car les toasts étaient uniquement de brèves fenêtres contextuelles qui n’ont pas été conservées. Windows 10 a introduit les [toasts ToastGeneric interactifs](adaptive-interactive-toasts.md) et a également introduit le centre de notifications où les notifications sont conservées pendant plusieurs jours. L’introduction du centre de notifications a nécessité l’introduction d’un activateur COM, afin que votre toast peut être activé plusieurs jours après sa création.

| Système d’exploitation | ToastGeneric | Activateur COM | Modèles de toast hérités |
| -- | ------------ | ------------- | ---------------------- |
| Windows 10 | Prise en charge | Prise en charge | Pris en charge (mais n’active pas le serveur COM) |
| Windows 8.1 / 8 | Non applicable | Non applicable | Prise en charge |
| Windows 7 et versions inférieures | Non applicable | Non applicable | Non applicable |

Pour vérifier si vous exécutez Windows 10, incluez l’en-tête `<VersionHelpers.h>` et vérifiez la méthode **IsWindows10OrGreater**. Si elle retourne true, continuez à appeler les méthodes décrites dans cette documentation ! 

```cpp
#include <VersionHelpers.h>

if (IsWindows10OrGreater())
{
    // Running Windows 10, continue with sending Windows 10 toasts!
}
```


## <a name="known-issues"></a>Problèmes connus

**PROBLÈME RÉSOLU : Application ne prendre le focus après avoir cliqué sur toast**: Dans les versions 15063 et versions antérieures, les droits de premier plan n’ont pas été transférées vers votre application lorsque nous avons activé le serveur COM. Par conséquent, votre application clignote simplement lorsque vous avez tenté de la déplacer au premier plan. Aucune solution n’était disponible pour ce problème. Nous avons résolu ce problème dans les builds 16299 et les versions ultérieures.


## <a name="resources"></a>Ressources

* [Exemple de code complet sur GitHub](https://github.com/WindowsNotifications/desktop-toasts)
* [Notifications toast à partir d’applications de bureau](toast-desktop-apps.md)
* [Documentation de contenu de toast](adaptive-interactive-toasts.md)
