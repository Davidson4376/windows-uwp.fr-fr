---
title: Portage de code Xbox Live à partir du XDK vers UWP
description: Découvrez comment porter code Xbox Live à partir de la plateforme du Kit de développement Xbox (XDK) pour la plateforme Windows universelle (UWP).
ms.assetid: 69939f95-44ad-4ffd-851f-59b0745907c8
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, xdk, portage
ms.localizationpriority: medium
ms.openlocfilehash: c6e8a6ebe716f1e062940066184e9f734441371b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590814"
---
# <a name="porting-xbox-live-code-from-the-xbox-developer-kit-xdk-to-universal-windows-platform-uwp"></a>Portage de code Xbox Live à partir du Kit de développeur Xbox (XDK) pour plateforme universelle Windows (UWP)

## <a name="introduction"></a>Introduction

Cet article vise à aider les développeurs qui ont utilisé le XDK une Xbox pour commencer la migration de leur code Xbox Live à la Windows 10 Universal Windows Platform (UWP).

Partie de cette migration inclut le passage du XSAPI 1.0 (Xbox Live Services API, inclus dans le XDK une Xbox jusqu’en août 2015) vers XSAPI 2.0 (inclus dans le XDK une Xbox à compter de novembre 2015 et est également disponible dans le Kit de développement Xbox Live. Les fonctionnalités de ces API sont virtuellement identiques, mais il existe des différences d’implémentation important.

Autres sujets qui sont abordés dans cet article incluent la préparation de votre ordinateur de développement Windows et l’installation d’autres API généralement nécessaires lors de l’utilisation des services Xbox Live, telles que l’API de Sockets Secure, ainsi que l’API de stockage connecté pour la gestion reposant sur le cloud enregistre le jeu.

<a name="_Setting_up_and"></a>

## <a name="setting-up-and-configuring-your-project-in-partner-center-and-xdp"></a>Définition et configuration de votre projet dans l’espace partenaires et XDP

Un titre UWP qui utilise la Xbox Live services doit être configuré dans [partenaires](https://partner.microsoft.com/dashboard). Pour plus d’informations, consultez [Ajout Xbox Live à un projet UWP existant ou nouveau](../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) la Xbox Live Guide de programmation inclus avec le [Xbox Live SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx).

Rubriques de cette page incluent ces étapes pour l’utilisation de services Xbox Live dans votre titre :

-   Créer le projet d’application UWP dans Partner Center.

-   Utilisez XDP pour configurer votre projet pour l’utilisation de Xbox Live.

-   Liez votre produit de partenaires à votre produit XDP.

-   Créer des comptes de développeurs dans XDP (nécessaire pour le titre de la Xbox Live en cours d’exécution dans votre bac à sable).

Si vos titres de prendre en charge multijoueur, certains paramètres supplémentaires peuvent être nécessaire dans vos modèles de session multijoueurs. Tous les titres de Windows 10 qui utilisent la Xbox Live multijoueurs et écrire dans un MPSD (document multijoueur) nécessitent ce nouveau champ dans la liste des « fonctionnalités » dans vos modèles de session : ```userAuthorizationStyle: true```.

### <a name="enabling-cross-play"></a>L’activation de cross-play

Si vous prendra en charge « cross-play » (une Xbox Live configuration partagée entre les jeux Xbox One et PC, ce qui permet des jeux multijoueur entre les périphériques), vous devrez également ajouter cette fonctionnalité à vos modèles de session : **crossPlay : true**.

Pour plus d’informations sur la prise en charge de cross-play et que les configurations requises dans XDP, consultez « Ingestion XDK et UWP Cross-Play titres dans XDP » dans le Guide de programmation de Xbox Live.

En outre, pour certaines considérations relatives à la programmation, consultez la section ultérieure [prenant en charge cross-multijoueur entre le PC et Xbox One](#_Supporting_multiplayer_cross-play).

## <a name="setting-up-your-windows-development-environment"></a>Configuration de votre environnement de développement Windows

1.  [Téléchargez la dernière version **Xbox Live SDK** ](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx) et extraire localement.

2.  [Installer le **Xbox Live Platform Extensions SDK** ](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx) si vous avez besoin de l’API de Sockets Secure et/ou de l’API Game enregistrer (également appelé stockage connecté) pour UWP.

3.  Ajouter la prise en charge de Xbox Live à votre projet d’application Windows universelle dans Visual Studio. Vous pouvez ajouter la source complète ou référencer les fichiers binaires en installant le package NuGet dans votre projet Visual Studio. Les packages sont disponibles pour C++ et WinRT. Pour plus d’informations, consultez [Ajout Xbox Live à un projet UWP nouveau ou existant](../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)

4.  Configurer votre ordinateur de développement pour utiliser votre bac à sable. Il existe un script de ligne de commande dans le répertoire Tools de Xbox Live SDK que vous pouvez utiliser à partir d’une invite de commandes administrateur (par exemple : SwitchSandbox.cmd XDKS.1).

  **Remarque** pour revenir à la sandbox de vente au détail, vous pouvez supprimer la clé de Registre qui modifie le script, ou vous pouvez basculer vers le bac à sable appelé vente au détail.

1.  Ajouter un compte de développeur à votre ordinateur de développement. Un compte de développeur créé dans XDP est requis pour interagir avec les services Xbox Live lors de l’exécution lorsque vous développez à votre sandbox attribué ou de l’exécution des exemples. Pour ajouter un ou plusieurs comptes à Windows :

    1.  Ouvrez **paramètres** (raccourci : Clé de Windows + I).

    2.  Ouvrez **comptes**.

    3.  Sur le **Your Account** , cliquez sur **ajouter un compte Microsoft**.

    4.  Entrez l’e-mail de compte de développeur et le mot de passe.

### <a name="appxmanifest-changes"></a>Modifications de AppxManifest

Les modifications entre les versions de Xbox et UWP du fichier appxmanifest.xml les plus courantes sont :

1. Identité du package est important dans UWP, même pendant le développement. Le nom de l’identité et le serveur de publication *doit correspondre à* ce qui a été défini dans l’espace partenaires pour votre application UWP.

1. Une section de dépendance de Package est obligatoire. Exemple :

```xml
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Universal" MinVersion="10.0.10240.0" MaxVersionTested="10.0.10240.0" />
  </Dependencies>
```

1.  Pour les autres sections du manifeste d’application qui ont des exigences spécifiques pour voir un manifeste d’application UWP exemple (par exemple, un des échantillons UWP inclus avec le Kit de développement Xbox Live ou d’un projet d’application Windows universelle par défaut créés dans Visual Studio) UWP, tel que ```<VisualElements>```.

1.  Titre et SCID sont définis dans le fichier xboxservices.config (voir la [section suivante](#_Define_your_title)) au lieu de dans la catégorie d’extension « xbox.live ».

1.  La catégorie d’extension « xbox.system.resources » n’est pas nécessaire.

1.  Secure Sockets sont définis dans le fichier networkmanifest.xml (consultez [Secure Sockets Layer](#_Secure_sockets)) au lieu de dans la catégorie « windows.xbox.networking ».

1.  Une catégorie d’extension « windows.protocol » doit être définie afin de recevoir des invitations de Xbox Live dans votre titre UWP (consultez [envoi et réception d’invitations](#_Sending_and_receiving)).

1.  Si vous utilisez l’API GameChat, vous souhaiterez ajouter la fonctionnalité microphone de l’appareil à l’intérieur du ```<Capabilities>``` élément. Exemple :

  ```<DeviceCapability Name="microphone">```

<a name="_Define_your_title"></a>

### <a name="define-your-title-and-scid-for-the-xbox-live-sdk-in-a-config-file"></a>Définir le titre et la SCID pour Xbox Live SDK dans un fichier de configuration

Le Kit de développement Xbox Live doit connaître le titre de votre ID et SCID, qui ne sont plus inclus dans le appxmanifest.xml pour les titres UWP. Au lieu de cela, vous créez un fichier texte nommé **xboxservices.config** dans votre projet répertoire racine et ajoutez les champs suivants, en remplaçant les valeurs avec les informations pour votre titre :

```
{
  "TitleId": 123456789,
  "PrimaryServiceConfigId": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
}
```

> [!NOTE]
> Toutes les valeurs à l’intérieur de xboxservices.config respectent la casse.

Inclure ce fichier de configuration en tant que contenu dans votre projet afin qu’il soit disponible dans la sortie de génération.

**Remarque** ces valeurs seront disponibles par programme au sein de votre titre à l’aide de l’API suivante :

```cpp
Microsoft::Xbox::Services::XboxLiveAppConfiguration^ xblConfig = xblContext->AppConfig;
unsigned int titleId = xblConfig->TitleId;
Platform::String^ scid = xblConfig->ServiceConfigurationId;
```

### <a name="api-namespace-mapping"></a>Mappage d’espace de noms API

Tableau 1. Mappage de Namespace à partir du XDK vers UWP.

<table>
  <tr>
    <td></td>
    <td><b>Une XDK Xbox</b></td><td><b>UWP</b></td>
    <td><b>API est disponible avec...</b></td>
  </tr>
  <tr>
    <td>Services Xbox API (XSAPI)</td>
    <td>Microsoft::Xbox::Services</td>
    <td>Microsoft::Xbox::services (<i>aucune modification</i>)</td>
    <td>Xbox Live SDK (Utilisez NuGet binaire ou source)</td>
  </tr>
  <tr>
    <td>GameChat</td>
    <td>
Microsoft::Xbox::GameChat Windows::Xbox::Chat </td>
    <td>
Microsoft::Xbox::GameChat (*aucune modification*) Microsoft::Xbox::ChatAudio </td>
    <td>
Xbox Live SDK (Utilisez NuGet binaire) </td>
  </tr>
  <tr>
    <td>SecureSockets</td>
    <td>Windows::Xbox::Networking</td>
    <td>Windows::Networking::XboxLive</td>
    <td>Xbox Live Extensions Platform SDK</td>
  </tr>
  <tr>
    <td>Stockage connecté</td>
    <td>Windows::Xbox::Storage</td>
    <td>Windows::Gaming::XboxLive::Storage</td>
    <td>Xbox Live Extensions Platform SDK</td>
  </tr>
</table>

### <a name="multiplayer-subscriptions-and-event-handling"></a>Gestion des événements et des abonnements multijoueurs

Une des modifications avec rupture de XSAPI 1.0 vers la version 2.0 XSAPI que titres plus multijoueurs rencontrerez est le déplacement de plusieurs méthodes et événements à partir de la **RealTimeActivityService** à la **MultiplayerService**.

Exemple :

-   **EnableMultiplayerSubscriptions()\***  (méthode)

-   **DisableMultiplayerSubscriptions()** (méthode)

-   **MultiplayerSessionChanged** événement

-   **MultiplayerSubscriptionLost** événement

-   **MultiplayerSubscriptionsEnabled** propriété

**Remarque de l’implémentation important** même si vous utilisez ne peut-être pas explicitement rien d’autre dans le **RealTimeActivityService** après le déplacement de ces événements et des méthodes vers le **MultiplayerService**, vous devez toujours appeler **xblContext -&gt;RealTimeActivityService -&gt;Activate()** avant d’appeler **EnableMultiplayerSubscriptions()** Étant donné que les abonnements multijoueurs nécessitent le service d’agrégation RTA.

## <a name="whats-handled-differently-in-uwp"></a>Ce qui est géré différemment dans UWP

Voici une liste très haut niveau des sections de code qui aura probablement des différences entre le XDK et UWP, comme rencontré dans le nouveau [NetRumble exemple](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx) (qui inclut les versions XDK et UWP) :

-   L’accès aux informations d’ID et SCID du titre

-   Activation de prélancement (nouveau pour UWP)

-   Gestion des PLM de Suspend/resume

-   Exécution étendue (nouveau pour UWP)

-   Xbox **utilisateur** objet et les différences de gestion de l’utilisateur

    -   Gestion de connexion et de déconnexion

    -   Contrôleur de jumelage (uniquement géré sur Xbox)

    -   Gestion des Gamepad

-   Vérification des privilèges multijoueurs

-   Prise en charge de cross-multijoueur entre le PC et Xbox One

-   Envoi des invitations jeu

    -   Possibilité d’ouvrir l’application tierce à partir de la partie - n/a sur UWP

    -   Possibilité d’énumérer les membres de tiers à partir de la partie - n/a sur UWP

-   Affiche le profil de joueur

-   Sécuriser des modifications de surface d’API de Socket

-   Initiation de mesure de qualité de service et de traitement des résultats

-   Écriture d’événements de jeu

-   GameChat : les événements, les paramètres et les ChatUser objet

-   Connecté modifications surfaces d’API de stockage

-   Événements de PIX (uniquement sur Xbox ; non traitées dans ce livre blanc)

-   Des différences de rendu

Les sections suivantes présentent davantage de détails sur la plupart de ces différences.

### <a name="accessing-title-id-and-scid-info"></a>L’accès aux informations d’ID et SCID du titre

Dans la plateforme Windows universelle, votre ID de titre et l’ID de configuration de service sont accessibles via la propriété AppConfig sur une instance d’un **XboxLiveContext**.

```cpp
Microsoft::Xbox::Services::XboxLiveAppConfiguration^ xblConfig = xblContext->AppConfig;
unsigned int titleId = xblConfig->TitleId;
Platform::String^ scid = xblConfig->ServiceConfigurationId;
```

**Remarque** dans le XDK, vous pouvez obtenir ces ID à l’aide de ces nouvelles propriétés ou les anciennes propriétés statiques dans **Windows::Xbox::Services::XboxLiveConfiguration**.

### <a name="prelaunch-activation"></a>Activation de prélancement

Les titres fréquemment utilisés dans Windows 10 peuvent être prelaunched lorsque l’utilisateur se connecte. Pour gérer cette situation, votre titre doit avoir le code qui vérifie les arguments de lancement pour **PreLaunchActivated**. Par exemple, probablement ne voulez-vous charger toutes vos ressources au cours de ce type d’activation. Pour plus d’informations, consultez l’article MSDN [application Handle prélancement](https://msdn.microsoft.com/library/windows/apps/mt593297.ASPx).

### <a name="suspendresume-plm-handling"></a>Gestion des PLM de Suspend/resume

Suspendre et reprendre et PLM en général, fonctionnent de la même façon dans une application Windows universelle à la façon dont elles fonctionnent sur Xbox One ; Toutefois, il existe quelques différences importantes à prendre en compte :

-   Il existe aucune **Constrained** état sur le PC, il s’agit d’un concept exclusif Xbox One.

-   La suspension commence immédiatement lorsque le titre est réduit ; un moyen de contourner ce problème, consultez la section [étendue d’exécution](#_Extended_execution).

-   Le minutage est différent : vous avez 5 secondes pour suspendre sur un PC au lieu de la seconde 1 sur la console.

Un autre élément important si vous utilisez le stockage connecté est le nouveau **ContainersChangedSinceLastSync** propriété dans la version UWP de cette API. Lors du traitement d’un événement de reprise, vous pouvez vérifier cette propriété afin de vérifier si tous les conteneurs dans le cloud tandis que le titre a été suspendu. Cela peut se produire si le joueur suspendu le jeu sur un PC, lu ailleurs et ensuite renvoyé au premier ordinateur. Si vous aviez lire les données à partir de ces conteneurs en mémoire avant que vous aviez suspendue, vous souhaitez probablement les lire à nouveau pour identifier ce qui a modifié et gérer les modifications en conséquence.

Pour plus d’informations sur la gestion des PLM dans une application UWP sur Windows 10, consultez l’article MSDN [Launching, resuming et les tâches en arrière-plan](https://msdn.microsoft.com/library/windows/apps/xaml/mt227652.aspx).

Vous pouvez également trouver la [PLM pour Xbox One](https://developer.xboxlive.com/en-us/platform/development/education/Documents/PLM%20for%20Xbox%20One.aspx) livre blanc sur GDN utile, car il a été écrit avec des jeux à l’esprit et la plupart des concepts pour gérer le cycle de vie d’application s’appliquent toujours sur un PC.

<a name="_Extended_execution"></a>

### <a name="extended-execution"></a>Exécution étendue

En réduisant une UWP application sur un PC entraîne généralement il immédiatement commencer à suspendre. À l’aide de l’exécution étendue, vous avez la possibilité de retarder ce processus. Exemple d’implémentation :

```cpp
using namespace Windows::ApplicationModel::ExtendedExecution;
//If this goes out of scope the request is nullified
ExtendedExecutionSession^ session;
void App::RequestExtension()
{
       if (!session)
       {
              session = ref new ExtendedExecutionSession();
       }
       session->Reason = ExtendedExecutionReason::Unspecified;
       session->Description = "foo";
       session->Revoked += ref new TypedEventHandler<Platform::Object^, ExtendedExecutionRevokedEventArgs^>(this, &App::ExtensionRevokedHandler);
       IAsyncOperation<ExtendedExecutionResult>^ request = session->RequestExtensionAsync();
       //At this point the request has been made. When the IAsyncOperation request completes, verify that the ExtendedExecutionResult == Allowed and you will not suspend for up to 10 minutes while minimized.
}

void App::ExtensionRevokedHandler(Platform::Object^ obj, ExtendedExecutionRevokedEventArgs^ args)
{
       if (args->Reason == Windows::ApplicationModel::ExtendedExecutionRevokedReason::Resumed)
       {
              //Request the extension again in preparation for the next suspend.
RequestExtension();
       }
       //The app will either complete suspending if the extension was revoked by system policy or resume running if the user has switched back to the app.
}

```

Après le **ExtensionRevokedHandler** a été appelée, une nouvelle extension doit être demandées pour les futures suspensions potentielles. Le **ExtensionRevokedHandler** est appelée lorsque la mémoire est sollicitée dans le système, 10 minutes se sont écoulées, ou l’utilisateur bascule vers le jeu tandis que le jeu est réduit. Par conséquent, **RequestExtension()** doit probablement être appelée à ces moments :

-   Lors du démarrage.

-   Dans le **ExtensionRevokedHandler** lorsque args -&gt;raison == reprise (l’utilisateur à onglets automatique lorsque le jeu a été réduit avant l’expiration de la minuterie de 10 minutes).

-   Dans le **OnResuming** gestionnaire (si le titre a été interrompu en raison d’une sollicitation de la mémoire ou le minuteur de 10 minutes).

### <a name="handling-users-and-controllers"></a>Gestion des utilisateurs et des contrôleurs

Sur Windows, vous travaillez avec un utilisateur connecté à la fois. Dans le Xbox Live SDK, vous créez tout d’abord un **XboxLiveUser** de l’objet, connecter à Xbox Live, puis créez **XboxLiveContext** objets à partir de cet utilisateur.

Auparavant, sur l’une XDK Xbox :

1.  Acquérir un utilisateur (à partir de l’interaction du boîtier de commande, par exemple).
2.  Créer un **XboxLiveContext** à partir de cet utilisateur :
  ```
  ref new Microsoft::Xbox::Services::XboxLiveContext( Windows::Xbox::System::User^ user )
  ```
1.  Gérer un **SignOut** événement :
  ```
  Windows::Xbox::System::User::SignOutStarted
  ```
1.  Gérer/contrôleur gamepad jumelage à l’aide de :
  ```
  Windows::Xbox::Input::Controller::ControllerRemoved
  Windows::Xbox::Input::Controller::ControllerPairingChanged
  ```

À présent, UWP/Xbox Live SDK :

1.  Créer un **XboxLiveUser**:

  ```
  auto xblUser = ref new Microsoft::Xbox::Services::System::XboxLiveUser();
  ```

1.  Essayez de les connecter en utilisant le dernier compte Microsoft utilisés, sans déranger les avec l’interface utilisateur :

  ```
  xblUser->SignInSilentlyAsync();
  ```

1.  Si vous obtenez **SignInResult::Success** dans le résultat de cette opération asynchrone, vous devez créer le **XboxLiveContext**, et puis que vous avez terminé :

  ```
  auto xblContext = ref new Microsoft::Xbox::Services::XboxLiveContext( xblUser );
  ```

1.  Si au lieu de cela, vous obtenez **SignInResult::UserInteractionRequired**, vous devez appeler la méthode de connexion interactive qui fait apparaître l’interface utilisateur du système :

  ```
  xblUser->SignInAsync();
  ```

1.  À partir de là, vous pouvez obtenir **SignInResult::UserCancel**, auquel cas vous n’avez pas un utilisateur connecté et vous devez envisager de fournir une option de menu pour qu’ils puissent tenter de se reconnecter.

  **Remarque** lorsque vous fournissez des options de menu, il est judicieux de leur donner la possibilité de basculer vers un autre compte Microsoft :

  ```
  xblUser->SwitchAccountAsync( nullptr );
  ```

1.  Une fois que vous avez un utilisateur connecté, vous pouvez souhaiter raccorder le **XboxLiveUser::SignOutCompleted** événement afin que vous pouvez réagir à l’utilisateur de déconnexion :

  ```
  xblUser->SignOutCompleted += ref new Windows::Foundation::EventHandler<Microsoft::Xbox::Services::System::SignOutCompletedEventArgs^>( &OnSignOutCompleted );
  ```

1.  Il n’existe aucun contrôleur de jumelage pour gérer dans Windows 10.

Il s’agit d’un exemple simplifié pour C++ / WinRT. Pour obtenir un exemple plus détaillé, consultez « Xbox Live d’authentification dans Windows 10 » dans le Guide de programmation de Xbox Live. Vous pouvez également trouver l’exemple plus large à « Ajout de Xbox Live à un nouveau projet UWP » utile.

### <a name="checking-multiplayer-privileges"></a>Vérification des privilèges multijoueurs

L’équivalent à **CheckPrivilegeAsync()** n’est pas encore disponible dans le Kit de développement Xbox Live. Pour l’instant, vous devez rechercher le privilège que vous avez besoin dans la liste de chaînes retournée par la **des privilèges** propriété pour un **XboxLiveUser**. Par exemple, pour vérifier les privilèges multijoueurs, recherchez le privilège « 254. » Utilisation de la documentation de XDK, vous trouverez une liste de tous les privilèges Xbox Live dans le **Windows::Xbox::ApplicationModel::Store::KnownPrivileges** énumération.

Pour une discussion sur ce sujet, consultez le billet [xsapi & utilisateur privilèges](https://forums.xboxlive.com/questions/48513/xsapi-user-privileges.html).

<a name="_Supporting_multiplayer_cross-play"></a>

### <a name="supporting-multiplayer-cross-play-between-xbox-one-and-pc-uwp"></a>Prise en charge de cross-multijoueur entre Xbox One et UWP de PC

Outre la configuration requise de modèle de nouvelle session dans XDP (consultez [paramétrage et la configuration de votre projet dans l’espace partenaires et XDP](#_Setting_up_and)), entre-play est fourni avec nouvelles restrictions sur la capacité de jointure de session. Vous ne pouvez plus utiliser « None » comme une restriction de jointure de session. Vous devez utiliser « Suivies » ou « Local » (la restriction par défaut est « Local »).

En outre, les restrictions de lecture et de jointure par défaut « Local » en raison de l’élément requis **userAuthorizationStyle** fonctionnalité pour le mode multijoueur Windows 10.

Cet article de forum, [est possible de créer une session multijoueur publique](https://forums.xboxlive.com/questions/46781/is-it-possible-to-create-public-multiplayer-sessio.html), contient des informations supplémentaires.

Des informations et des exemples sont accessibles dans les organigrammes de développeur multijoueur mis à jour, l’exemple multijoueur activé cross-play NetRumble, ou à partir de votre gestionnaire de compte de développeur (DAM).

<a name="_Sending_and_receiving"></a>

### <a name="sending-and-receiving-invites"></a>Envoyer et recevoir des invitations

L’API pour afficher l’interface utilisateur pour l’envoi d’invitations est désormais **Microsoft :: Xbox::Services::System::TitleCallableUI::ShowGameInviteUIAsync()**. Vous passez dans une session -&gt; **SessionReference** objet à partir de votre session de l’activité (en général, votre salle d’attente). Vous pouvez éventuellement passer un deuxième paramètre que références personnalisé invite des ID de chaîne qui est définie dans la configuration de votre service dans XDP. La chaîne que vous définissez il apparaîtra dans la notification de toast envoyée aux joueurs invités. Notez que ce que vous transmettez comme paramètre à cette méthode est le numéro d’ID, et il doit être mis en forme correctement pour le service. Par exemple, ID de la chaîne « 1 » doit être transmise dans en tant que « / / / 1 ».

Si vous souhaitez envoyer des invitations directement à l’aide du service multijoueur (autrement dit, sans afficher d’interface utilisateur), vous pouvez toujours utiliser l’autre méthode d’invitation, **Microsoft :: Xbox::Services::Multiplayer::MultiplayerService::SendInvitesAsync()** à partir de l’utilisateur **XboxLiveContext**.

Pour autoriser pour invitations arrivant dans Windows protocole-activer votre titre, vous devez ajouter cette extension pour le **&lt;Application&gt;** élément dans le fichier appxmanifest :

```xml
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-xbl-multiplayer" />
  </uap:Extension>
</Extensions>
```

Vous pouvez ensuite gérer l’invitation que vous avez exécutée auparavant sur Xbox One lorsque votre **CoreApplication** Obtient un **Activated** événement et l’activation de type est un **ActivationKind::Protocol**.

### <a name="showing-the-gamer-profile-card"></a>Affichage de la carte de visite gamer

Pour afficher la carte de visite gamer sur UWP, utilisez **Microsoft :: Xbox::Services::System::TitleCallableUI::ShowProfileCardUIAsync()**, en passant le XUID pour l’utilisateur cible.

<a name="_Secure_sockets"></a>

### <a name="secure-sockets"></a>Secure Sockets Layer

L’API Socket sécurisé est inclus dans la fonction [Xbox Live Platform Extensions SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx).

Consultez ce billet pour l’utilisation de l’API : [Configuration SecureDeviceAssociation pour multiplateforme](https://forums.xboxlive.com/answers/45722/view.html).

**Remarque** pour UWP, le **SocketDescriptions** section a été déplacé en dehors de l’appxmanifest et dans son propre [networkmanifest.xml](https://forums.xboxlive.com/storage/attachments/410-networkmanifestxml.txt). Le format à l’intérieur de la &lt;SocketDescriptions&gt; élément est pratiquement identique, mais sans le **mx :** préfixe.

De la lecture croisée entre Xbox et Windows 10, être *vraiment* tout défini *identique* entre les deux types de manifestes (Package.appxmanifest pour Xbox One et networkmanifest.xml pour Windows 10). Le nom de socket, le protocole, le etc. doit correspondre à *exactement*.

Également pour cross-play, vous devez définir les utilisations SDA quatre suivantes à l’intérieur de la ```<AllowedUsages>``` élément dans *à la fois* le Package.appxmanifest une Xbox et le networkmanifest.xml Windows 10 :

```xml
<SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
<SecureDeviceAssociationUsage Type="AcceptOnMicrosoftConsole" />
<SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
<SecureDeviceAssociationUsage Type="AcceptOnWindowsDesktop" />
```

### <a name="multiplayer-qos-measurements"></a>Mesures de qualité de service mode multijoueur

Outre la modification d’espace de noms dans l’API de Sockets sécuriser, certains des noms d’objets et des valeurs ont changé, trop. Le mappage pour l’état de mesure généralement utilisé se trouve dans le tableau suivant.

Tableau 2. Généralement utilisé de mappage d’état de mesure.

| XDK (Windows::Xbox::Networking::QualityOfServiceMeasurementStatus)  | UWP (Windows::Networking::XboxLive::XboxLiveQualityOfServiceMeasurementStatus)  |
|------------------------------------|--------------------------------------------|
| HostUnreachable                    | NoCompatibleNetworkPaths                   |
| MeasurementTimedOut                | TimedOut                                   |
| PartialResults                     | InProgressWithProvisionalResults           |
| Réussite                            | Réussite                                  |

Les étapes impliquées dans *mesure* QoS (qualité de service) et *traitement des résultats* sont en principe les mêmes lorsque vous comparez les versions XDK et UWP de l’API. Toutefois, étant donné les changements de nom et quelques modifications de conception, le code résultant est différent dans certains endroits.

Pour mesurer la qualité de service pour le **XDK**, vous avez créé une collection d’adresses de l’appareil sécurisé et une collection de mesures et que vous passé dans le **MeasureQualityOfServiceAsync()** (méthode).

Pour mesurer la qualité de service pour **UWP**, vous créez un nouveau **XboxLiveQualityOfServiceMeasurement()** de l’objet, appelez **Append()** à son **métriques** et **DeviceAddresses** propriétés et d’appelez ensuite l’objet **MeasureAsync()** (méthode).

Exemple :

```cpp
auto qosMeasurement = ref new Windows::Networking::XboxLive::XboxLiveQualityOfServiceMeasurement();
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageInboundBitsPerSecond);
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageOutboundBitsPerSecond);
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageLatencyInMilliseconds);
qosMeasurement->NumberOfProbesToAttempt = myDefaultQosProbeCount;
qosMeasurement->TimeoutInMilliseconds = myDefaultQosMeasurementTimeout;

// Add secure addresses for each session member
for (const auto& member : session->GetMembers())
{
    if (!member->IsCurrentUser)
    {
        auto sda = member->SecureDeviceAddressBase64;

        if (!sda->IsEmpty())
        {
qosMeasurement->DeviceAddresses->Append(Windows::Networking::XboxLive::XboxLiveDeviceAddress::CreateFromSnapshotBase64(sda));
        }
    }
}

if (qosMeasurement->DeviceAddresses->Size > 0)
{
    qosMeasurement->MeasureAsync();
}

```

Pour plus d’exemples, consultez le **MatchmakingSession::MeasureQualityOfService()** et **MatchmakingSession::ProcessQosMeasurements()** fonctions dans l’exemple NetRumble.

### <a name="writing-game-events"></a>Écriture d’événements de jeu

Envoi d’événements de jeu qui sont configurés dans la Configuration du Service de votre titre a une API différente dans UWP. Le Kit de développement Xbox Live utilise le **EventsService** et un modèle de sac de propriété.

Exemple :

```cpp
auto properties = ref new Windows::Foundation::Collections::PropertySet();
properties->Insert("RoundId", m_roundId);
properties->Insert("SectionId", safe_cast<Platform::Object^>(0));
properties->Insert("MultiplayerCorrelationId", m_multiplayerCorrelationId);
properties->Insert("GameplayModeId", safe_cast<Platform::Object^>(0));
properties->Insert("MatchTypeId", safe_cast<Platform::Object^>(0));
properties->Insert("DifficultyLevelId", safe_cast<Platform::Object^>(0));

auto measurements = ref new Windows::Foundation::Collections::PropertySet();

xblContext->EventsService->WriteInGameEvent("MultiplayerRoundStart", properties, measurements);

```

Pour plus d’informations, consultez la documentation de Xbox Live SDK.

**Conseil** vous pouvez utiliser la **xcetool.exe** fourni avec le Xbox Live SDK (situé dans le répertoire Tools) pour convertir le fichier events.man que vous avez téléchargé à partir de XDP dans un fichier d’en-tête .h. Utilisez le '-x' option pour générer cet en-tête C++ à l’aide du nouveau schéma de sac de propriété v2. Cet en-tête contient des fonctions C++ que vous pouvez appeler pour tous les événements configurés ; par exemple, **EventWriteMultiplayerRoundStart()**. Si vous préférez utiliser une interface WinRT, vous pouvez toujours faire référence à ce fichier d’en-tête pour voir comment les propriétés et les mesures sont construits pour chacun de vos événements.

### <a name="game-chat"></a>Conversation de jeu

GameChat dans UWP est inclus dans le Kit de développement Xbox Live comme package NuGet binaire. Consultez les instructions dans le Guide de programmation de Xbox Live pour savoir comment ajouter ce package NuGet à votre projet.

L’utilisation de base est pratiquement identique entre le XDK et les versions UWP. Quelques différences dans l’API sont les suivantes :

1.  Le **User::AudioDeviceAdded** événement pas nécessairement être raccordé par un titre UWP. L’appareil de handles de bibliothèque conversation sous-jacent ajoute et supprime.

2.  **ChatUser** s’appelle désormais **GameChatUser**.

3.  **Microsoft::Xbox::GameChat** espace de noms est inchangée, mais la **Windows::Xbox::Chat** espace de noms est devenu **Microsoft::Xbox::ChatAudio**.

4.  **AddLocalUserToChatChannelAsync()** prend une XUID ou sur un **ChatAudio::IChatUser ^** au lieu d’un **XboxUser**.

5.  **RemoveLocalUserFromChatChannelAsync()** nécessite un **ChatAudio::IChatUser ^** au lieu d’un **XboxUser**. Vous pouvez obtenir un **IChatUser** à partir d’un **GameChatUser**-&gt;**utilisateur**.

### <a name="connected-storage"></a>Stockage connecté

L’API de stockage connecté est fourni dans le distinct [Xbox Live Platform Extensions SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx). Documentation est incluse dans la documentation de Xbox Live SDK.

Le flux global est le même que sur Xbox One, avec l’ajout de la **ContainersChangedSinceLastSync** propriété dans la version UWP. Cette propriété doit être vérifiée lorsque votre titre gère un événement de reprise, après avoir appelé **GetForUserAsync()** là encore, pour voir ce que les conteneurs modifié dans le cloud, tout votre titre a été suspendu. Si vous avez des données sont chargées en mémoire à partir d’un des conteneurs qui a changent, vous souhaitez probablement lire les données à nouveau pour voir ce que modifié et gérer les modifications en conséquence.

Autres différences notables dans la version UWP sont notamment :

1.  Namespace diffère **Windows::Xbox::Storage** à **Windows::Gaming::XboxLive::Storage**.

2.  **ConnectedStorageSpace** est renommé **GameSaveProvider**.

3.  **Windows::System::User** est utilisée dans **GetForUserAsync()** au lieu d’un **XboxUser**, et le SCID est désormais obligatoire.

4.  Aucun stockage local « ordinateurs » (autrement dit, **GetForMachineAsync()** a été supprimée). Envisagez d’utiliser **Windows::Storage::ApplicationData** à la place pour votre non itinérant, local enregistrer les données.

5.  Async résultats sont retournés dans un sans exception \*objet de type de résultat (par exemple, **GameSaveProviderGetResult**) ; à partir de ce que vous pouvez vérifier le **état** propriété, et si aucune erreur n’est, lire l’objet retourné à partir de la **valeur** propriété.

6.  **ConnectedStorageErrorStatus enum** est renommé **GameSaveErrorStatus** et est retourné dans le **état** propriété d’un résultat. Les anciennes valeurs existent, et quelques nouveaux ont été ajoutés :

-   Abandonner

-   ObjectExpired

-   OK

-   UserHasNoXboxLiveInfo

Consultez l’exemple GameSave ou l’exemple NetRumble par exemple l’utilisation.

**Remarque** Gamesaveutil.exe est l’équivalent de xbstorage.exe (l’utilitaire de ligne de commande développeur inclus avec le XDK). Après avoir installé le kit SDK Extensions Platform Xbox Live, cet utilitaire peut trouver ici : C:\\Program Files (x86)\\Windows Kits\\10\\SDK d’Extension\\XboxLive\\1.0\\Bin\\x64

## <a name="summary"></a>Résumé

Les modifications de l’API et les nouvelles exigences décrites dans ce livre blanc sont ceux que vous êtes susceptible de rencontrer lors du portage du code de jeu existant à partir du XDK une Xbox à la nouvelle plateforme Windows universelle. ACCENT ont été attribuée à l’application et configuration de l’environnement, ainsi que les zones de fonctionnalité relatives aux services Xbox Live, tels que le stockage multijoueur et connecté. Pour plus d’informations, suivez les liens fournis dans cet article et dans les références suivantes et veillez à consulter la section « Windows 10 » de la [forums de développeurs](https://forums.xboxlive.com) pour plus d’aide, des réponses et actualités.

## <a name="references"></a>Références

-   [Portage de Xbox One vers Windows 10](https://developer.xboxlive.com/en-us/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx)

-   [Livres blancs de Xbox One](https://developer.xboxlive.com/en-us/platform/development/education/Pages/WhitePapers.aspx)

-   [Exemples](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx)
