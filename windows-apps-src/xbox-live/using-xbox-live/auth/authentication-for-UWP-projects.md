---
title: Authentification pour les projets UWP
author: aablackm
description: Découvrez comment connecter des utilisateurs de Xbox Live dans un titre de la plateforme universelle Windows (UWP).
ms.assetid: e54c98ce-e049-4189-a50d-bb1cb319697c
ms.author: aablackm
ms.date: 03/19/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, l’authentification, connectez-vous
ms.localizationpriority: medium
ms.openlocfilehash: 5473b7ede7731d7d07b7e5bfd72857fdb64f1c89
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628384"
---
# <a name="authentication-for-uwp-projects"></a>Authentification pour les projets UWP

Pour tirer parti des fonctionnalités Xbox Live dans les jeux, un utilisateur doit créer un profil Xbox Live pour s’identifier auprès de la Communauté Xbox Live.  Les services Xbox Live suivi de jeu lié activités à l’aide de ce profil Xbox Live, telles que l’utilisateur gamertag et gamer image, qui sont des amis de jeu de l’utilisateur, ce que l’utilisateur des jeux a joué, les primes que l’utilisateur qui a déverrouillé, où l’utilisateur est le classement pour un, etc. de jeu, particulier.

Lorsqu’un utilisateur souhaite accéder aux services Xbox Live dans un jeu particulier sur un appareil particulier, l’utilisateur doit d’abord authentifier.  Le jeu peut appeler des API de Live Xbox pour lancer le processus d’authentification.  Dans certains cas, l’utilisateur s’affiche avec une interface pour fournir des informations supplémentaires, telles que saisir le nom d’utilisateur et le mot de passe du Account Microsoft à utiliser, de consentement d’autorisation au jeu, résolution des problèmes de compte, accepter les nouvelles conditions d’utilisation, et ainsi de suite

Une fois authentifié, l’utilisateur est associé à cet appareil jusqu'à ce qu’ils explicitement Déconnectez-vous de Xbox Live à partir de l’application Xbox.  Un seul joueur est autorisé à être authentifié sur un appareil autre qu’une console à la fois (pour toutes les Xbox Live games) ;  pour un nouveau lecteur doit être authentifié sur un appareil autre qu’une console, le joueur authentifié existant doit se connecter en premier.

## <a name="steps-to-sign-in"></a>Étapes pour se connecter

À un niveau élevé, vous utilisez l’API Live Xbox en suivant ces étapes :

1. Créer un objet XboxLiveUser pour représenter l’utilisateur
2. Connectez-vous en mode silencieux à Xbox Live au démarrage
3. Tenter de se connecter avec l’expérience utilisateur si nécessaire
4. Création d’un contexte de Xbox Live en fonction de l’interaction utilisateur
5. Utiliser le contexte de Xbox Live pour accéder aux services Xbox Live
6. Lorsque les issues de jeux ou de l’utilisateur se connecte à la sortie, la version de l’objet de XboxLiveUser et l’objet de XboxLiveContext en leur affectant la valeur null

### <a name="creating-an-xboxliveuser-object"></a>Création d’un objet XboxLiveUser

La plupart des activités de Xbox Live sont liées à l’utilisateur de Live Xbox.  En tant que développeur de jeux, vous devez d’abord créer un objet XboxLiveUser pour représenter l’utilisateur local.

C++ :

```cpp
auto xboxUser = std::make_shared<xbox_live_user>(Windows::System::User^ windowsSystemUser);
```

C++ / c++ / CX (WinRT) :

```cpp
XboxLiveUser xboxUser = ref new XboxLiveUser(Windows::System::User^ windowsSystemUser);
```

C#(WinRT) :

```csharp
XboxLiveUser xboxUser = new XboxLiveUser(Windows.System.User windowsSystemUser);
```

* **windowsSystemUser** l’objet d’utilisateur système windows à utiliser pour associer des utilisateurs en direct de xbox. Impossible d’être nullptr si l’application est un application(SUA) utilisateur unique.
  * Pour plus d’informations sur Application(SUA) d’utilisateur unique et multiple utilisateur Application(MUA), veuillez vérifier [Introduction aux applications des utilisateurs multiples](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/multi-user-applications#single-user-applications)
  * Pour plus d’informations sur l’obtention de Windows::System::User ^ à partir de Windows, vérifiez [récupération utilisateur de système de windows sur UWP](retrieving-windows-system-user-on-UWP.md)

### <a name="sign-in-silently-to-xbox-live-at-startup"></a>Connectez-vous en mode silencieux à Xbox Live au démarrage ###

Votre jeu doit commencer à authentifier l’utilisateur à la Xbox Live dès que possible après avoir lancé, avant même de vous présenter l’interface utilisateur, pour Prérécupérer des données à partir des services Xbox Live.

Pour authentifier l’utilisateur local en mode silencieux, appelez

C++ :

```cpp
pplx::task<xbox_live_result<sign_in_result>> xbox_live_user::signin_silently(Platform::Object^ coreDispatcher)
```

C++ / c++ / CX (WinRT) :

```cpp
Windows::Foundation::IAsyncOperation<SignInResult^>^ XboxLiveUser::SignInSilentlyAsync(Platform::Object^ coreDispatcher)
```

C#(WinRT) :

```csharp
Microsoft.Xbox.Services.System.SignInResult XboxLiveUser.SignInSilentlyAsync(Windows.UI.Core.CoreDispatcher coreDispatcher);
```

* **coreDispatcher**

  Distributeur de thread est utilisé pour la communication entre les threads. Bien que l’API de connexion en mode silencieux ne va pas afficher l’interface utilisateur, XSAPI doit toujours le répartiteur de thread d’interface utilisateur pour obtenir des informations relatives aux paramètres régionaux de votre appx. Vous pouvez obtenir le static distributeur de thread d’interface utilisateur en appelant Windows::UI::Core::CoreWindow::GetForCurrentThread() -> répartiteur dans le thread d’interface utilisateur. Ou, si vous êtes certain que cette API est appelée sur le thread d’interface utilisateur, vous pouvez passer dans nullptr (par exemple sur JS UWA).


3 issues sont possibles à partir de la tentative de connexion en mode silencieux

* **Réussite**

  Si l’appareil est en ligne, cela signifie que l’utilisateur authentifié avec succès à la Xbox Live, et nous sommes parvenus à obtenir un jeton valide.

  Si l’appareil est hors connexion, cela signifie que l’utilisateur a est authentifié précédemment sur Xbox Live avec succès et n’a pas explicitement connecté à la sortie à partir de ce titre.  Remarque Dans ce cas il existe aucune garantie que titre n’a accès à un jeton valide, elle est garantie uniquement que l’identité l’utilisateur est connue et qu’il a été vérifiée.    L’identité de l’utilisateur ne connaît le titre par le biais de leur id d’utilisateur xbox (xuid) et le gamertag.

* **UserInteractionRequired**

  Cela signifie que le runtime n’a pas pu vous connecter à l’utilisateur en mode silencieux.  Le jeu doit appeler `xbox_live_user::sign_in` qui appelle le fournisseur d’identité Xbox pour montrer le flux de l’expérience utilisateur nécessaire à l’utilisateur de connexion-inscription/connectez-vous.  Problèmes courants sont :

  * Utilisateur ne dispose pas d’un Account Microsoft
  * Utilisateur n’a pas défini un Account Microsoft préféré pour les jeux
  * Le Account Microsoft sélectionné n’a pas un profil Xbox Live
  * Utilisateur doit accepter le consentement de Account Microsoft

* **Autres erreurs**

  Le runtime n’a pas pu se connecter en raison d’autres raisons.  En général, ces problèmes ne sont pas exploitables par le jeu ou l’utilisateur. Lorsque vous utilisez c ++ API, vous devez vérifier l’erreur en vérifiant xbox_live_result <> .err() ; sur WinRT, vous devez intercepter Platform::Exception ^.

### <a name="attempt-to-sign-in-with-ux-if-required"></a>Tenter de se connecter avec l’expérience utilisateur si nécessaire ###

Votre jeu doit authentifier l’utilisateur à Xbox Live avec l’expérience utilisateur activée lors de l’authentification en mode silencieux a échoué, et vous êtes prêt à présenter l’interface utilisateur.

Pour authentifier l’utilisateur local avec l’expérience utilisateur, appelez

C++ :

```cpp
pplx::task<xbox_live_result<sign_in_result>> xbox_live_user::signin(Platform::Object^ coreDispatcher)
```


C++ / c++ / CX (WinRT) :

```cpp
Windows::Foundation::IAsyncOperation<SignInResult^>^ XboxLiveUser::SignInAsync(Platform::Object^ coreDispatcher)
```

C#(WinRT) :

```csharp
Microsoft.Xbox.Services.System.SignInResult  XboxLiveUser.SignInAsync(Windows.UI.Core.CoreDispatcher coreDispatcher);
```

* **coreDispatcher**

  Distributeur de thread est utilisé pour la communication entre les threads. Authentification dans l’API nécessite le répartiteur de l’interface utilisateur afin qu’il peut afficher le signe dans l’interface utilisateur et obtenir les informations relatives aux paramètres régionaux de votre appx. Vous pouvez obtenir le static distributeur de thread d’interface utilisateur en appelant Windows::UI::Core::CoreWindow::GetForCurrentThread() -> répartiteur dans le thread d’interface utilisateur. Ou, si vous êtes certain que cette API est appelée sur le thread d’interface utilisateur, vous pouvez passer dans nullptr (par exemple sur JS UWA).

3 issues sont possibles à partir de la tentative de connexion avec l’expérience utilisateur :

* **Réussite**

  Si l’appareil est en ligne, cela signifie que l’utilisateur authentifié avec succès à la Xbox Live, et nous sommes parvenus à obtenir un jeton valide.

  Si l’appareil est hors connexion, cela signifie que l’utilisateur a est authentifié précédemment sur Xbox Live avec succès et n’a pas explicitement connecté à la sortie à partir de ce titre.  Remarque Dans ce cas il existe aucune garantie que titre n’a accès à un jeton valide, elle est garantie uniquement que l’identité l’utilisateur est connue et qu’il a été vérifiée.    L’identité de l’utilisateur ne connaît l’id d’utilisateur xbox titre (xuid) et le gamertag.

* **UserCancel**

  Cela signifie que l’utilisateur a annulé l’opération de connexion avant la fin.  Dans ce cas, le jeu ne devrait pas automatiquement relancer connectez-vous avec l’expérience utilisateur.  Au lieu de cela, elle doit présenter l’expérience utilisateur dans le jeu qui permet à l’utilisateur retenter l’opération de connexion.  (Par exemple, un bouton de connexion)

* **Autres erreurs**

  Le runtime n’a pas pu se connecter en raison d’autres raisons.  En général, ces problèmes ne sont pas exploitables par le jeu ou l’utilisateur. Lorsque vous utilisez c ++ API, vous devez vérifier l’erreur en vérifiant xbox_live_result <> .err() ; sur WinRT, vous devez intercepter Platform::Exception ^.

## <a name="sign-in-code-examples"></a>Exemples de Code de connexion

### <a name="c"></a>C++

```cpp

#include "xsapi\services.h" // contains the xbox::services::system namespace

using namespace xbox::services::system; // contains definitions necessary for sign-in

void SignInSample::SignIn()
{
    //1. Create an xbox_live_user object
    m_user = std::make_shared<xbox::services::system::xbox_live_user>(); // m_user declared in header file

    //2. Sign-In silently to Xbox Live at startup
    m_user->signin_silently()
        .then([this](xbox::services::xbox_live_result<xbox::services::system::sign_in_result> result)
    {
        if (!result.err())
        {
            auto rPayload = result.payload();
            switch (rPayload.status())
            {
            case xbox::services::system::sign_in_status::success:
                // sign-in successful
                signIn = true;
                break;
            case xbox::services::system::sign_in_status::user_interaction_required:
                // 3. Attempt to sign-in with UX if required
                m_user->signin(Windows::UI::Core::CoreWindow::GetForCurrentThread()->Dispatcher)
                    .then([this](xbox::services::xbox_live_result<xbox::services::system::sign_in_result> loudResult) // use task_continuation_context::use_current() to make the continuation task running in current apartment 
                {
                    if (!loudResult.err())
                    {
                        auto resPayload = loudResult.payload();
                        switch (resPayload.status())
                        {
                        case xbox::services::system::sign_in_status::success:
                            // sign-in successful
                            signIn = true;
                            break;
                        case xbox::services::system::sign_in_status::user_cancel:
                            // user cancelled sign in 
                            // present in-game UX that allows the user to retry the sign-in operation. (For example, a sign-in button)
                            break;
                        }
                    }
                    else
                    {
                        //login has failed at this point
                    }
                }, concurrency::task_continuation_context::use_current());
                break;
            }
        }
    });
    if (signIn)
    {
        // 4. Create an Xbox Live context based on the interacting user
        m_xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>(m_user); // m_xboxLiveContext declared in header file

        // add sign out event handler
        AddSignOut();
    }
}

void SignInSample::AddSignOut()
{
    xbox::services::system::xbox_live_user::add_sign_out_completed_handler(
        [this](const xbox::services::system::sign_out_completed_event_args&)

    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        m_user = NULL;
        m_xboxLiveContext = NULL;
    });
}

```

### <a name="c-winrt"></a>C# (WinRT)

```csharp

using System.Diagnostics;
using Microsoft.Xbox.Services.System;
using Microsoft.Xbox.Services;

public async Task SignIn()
{
    bool signedIn = false;

    // Get a list of the active Windows users.
    IReadOnlyList<Windows.System.User> users = await Windows.System.User.FindAllAsync();

    // Acquire the CoreDispatcher which will be required for SignInSilentlyAsync and SignInAsync.
    Windows.UI.Core.CoreDispatcher UIDispatcher = Windows.UI.Xaml.Window.Current.CoreWindow.Dispatcher; 

    try
    {
        // 1. Create an XboxLiveUser object to represent the user
        XboxLiveUser primaryUser = new XboxLiveUser(users[0]);

        // 2. Sign-in silently to Xbox Live
        SignInResult signInSilentResult = await primaryUser.SignInSilentlyAsync(UIDispatcher);
        switch (signInSilentResult.Status)
        {
            case SignInStatus.Success:
                signedIn = true;
                break;
            case SignInStatus.UserInteractionRequired:
                //3. Attempt to sign-in with UX if required
                SignInResult signInLoud = await primaryUser.SignInAsync(UIDispatcher);
                switch(signInLoud.Status)
                {
                    case SignInStatus.Success:
                        signedIn = true;
                        break;
                    case SignInStatus.UserCancel:
                        // present in-game UX that allows the user to retry the sign-in operation. (For example, a sign-in button)
                        break;
                    default:
                        break;
                }
                break;
            default:
                break;
        }
        if(signedIn)
        {
            // 4. Create an Xbox Live context based on the interacting user
            Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(user);

            //add the sign out event handler
            XboxLiveUser.SignOutCompleted += OnSignOut;
        }
    }
    catch (Exception e)
    {
        Debug.WriteLine(e.Message);
    }

}

public void OnSignOut(object sender, SignOutCompletedEventArgs e)
    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        primaryUser = null;
        xboxLiveContext = null;
    }
```

## <a name="sign-out"></a>Se déconnecter

### <a name="handling-user-sign-out-completed-event"></a>Événement terminé déconnexion utilisateur de gestion

L’utilisateur sera déconnexion à partir d’un titre si un des éléments suivants se produit :

1. L’utilisateur connecté à la sortie à partir de l’application de Xbox (Windows 10) ou d’un interpréteur de commandes de console (Xbox One). Déconnexion affectera toutes les applications Xbox Live est activée pour cet utilisateur.
2. L’utilisateur basculé vers un autre Account Microsoft
3. L’utilisateur connecté dans le même titre à partir d’un autre appareil

Dans tous ces cas, le titre reçoit un événement à partir de la `xbox_live_user::add_sign_out_completed_handler` ou `XboxLiveUser::SignOutCompleted` gestionnaires.  Le jeu doit gérer l’événement terminé de déconnexion de façon appropriée :

1. Le jeu doit afficher indique clairement à l’utilisateur qu’il a déconnecté de Xbox Live.
2. Le jeu ne peut pas appeler n’importe quel service Xbox Live API dans le Gestionnaire d’événements, étant donné que l’utilisateur a déjà déconnecté et aucun jeton d’autorisation est disponible.

## <a name="sign-out-handler-code-samples"></a>Déconnectez-vous des exemples de Code de gestionnaire

### <a name="c"></a>C++

```cpp

xbox::services::system::xbox_live_user::add_sign_out_completed_handler(
        [this](const xbox::services::system::sign_out_completed_event_args&)

    {
        // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
        m_user = NULL;
        m_xboxLiveContext = NULL;
    });

```

### <a name="c-winrt"></a>C# (WinRT)

```csharp
XboxLiveUser.SignOutCompleted += OnUserSignOut;

public void OnSignOut(object sender, SignOutCompletedEventArgs e)
        {
            // 6. When the game exits or the user signs-out, release the XboxLiveUser object and XboxLiveContext object by setting them to null
            primaryUser = null;
            xboxLiveContext = null;
        }
```

## <a name="determining-if-the-device-is-offline"></a>Déterminer si l’appareil est hors connexion

Authentification dans API sera toujours réussie lorsque hors connexion si l’utilisateur connecté une seule fois, et le dernier compte connecté s’affichera.  

Si aucun utilisateur n’a été signée avant, connectez-vous en mode hors connexion ne sera pas réalisable.

Si le titre peut être lu en mode hors connexion (d’une campagne en mode, etc.), le titre peut autoriser l’utilisateur à jouer et progression jeu enregistrement via les API WriteInGameEvent et API de stockage connectés, les deux fonctionnent correctement tandis que l’appareil est hors connexion.

Si le titre ne peut pas être lu en mode hors connexion (multijoueur ou jeu de serveur en fonction, etc.), le titre doit appeler l’API de GetNetworkConnectivityLevel pour déterminer si l’appareil est hors connexion et informe l’utilisateur sur l’état et les solutions possibles (par exemple, « vous devez se connecter à Internet pour continuer... »).

## <a name="online-status-code-samples"></a>Exemples de Code d’état en ligne

### <a name="c"></a>C++

```cpp

using namespace Windows::Networking::Connectivity;

//Retrieve the ConnectionProfile
ConnectionProfile^ InternetConnectionProfile = NetworkInformation::GetInternetConnectionProfile();

NetworkConnectivityLevel connectionLevel = InternetConnectionProfile->GetNetworkConnectivityLevel();

switch (connectionLevel)
{
case NetworkConnectivityLevel::InternetAccess:
    // User is connected to the internet.
    break;
case NetworkConnectivityLevel::ConstrainedInternetAccess: //Limited Internet Access Possible Authentication Required
     // display error message for user.
    LogConnectivityLine("Game Offline: Limited internet access, browser authentication may be required. "); //function writes to UI
    break;
default:
    LogConnectivityLine("Game Offline: No internet access.");
    break;
}

```

### <a name="c-winrt"></a>C# (WinRT)

```csharp
using Windows.Networking.Connectivity;

//Retrieve the ConnectionProfile
string connectionProfileInfo = string.Empty;
ConnectionProfile InternetConnectionProfile = NetworkInformation.GetInternetConnectionProfile();

NetworkConnectivityLevel connectionLevel = InternetConnectionProfile.GetNetworkConnectivityLevel();

switch(connectionLevel)
    {
        case NetworkConnectivityLevel.InternetAccess:
            // User is connected to the internet.
            break;
        case NetworkConnectivityLevel.ConstrainedInternetAccess: //Limited Internet Access Possible Authentication Required
            // display error message for user.
            LogConnectivityLine("Game Offline: Limited internet access, browser authentication may be required. "); //function writes to UI
            break;
        default:
            LogConnectivityLine("Game Offline: No internet access.");
            break;
    }
```