---
author: Mtoepke
title: "Problèmes connus avec UWP sur la version prélim. pour dév. XboxOne"
description: 
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: e016be20af9a0d7a67fa383cbdc93083d12a1113

---

# Problèmes connus avec UWP sur la version préliminaire pour développeurs de Xbox

Cette rubrique décrit les problèmes connus liés à la plateforme UWP sur la version préliminaire pour développeurs de Xbox. Pour plus d’informations sur cette version préliminaire pour développeurs, voir [UWP sur Xbox](index.md). 

\[Si vous avez accédé à cette page à partir d’un lien dans une rubrique de référence d’une API et que vous recherchez des informations sur les API pour la famille d’appareils universels, consultez [Fonctionnalités UWP qui ne sont pas encore prises en charge sur Xbox](http://go.microsoft.com/fwlink/?LinkID=760755).\]

La Mise à jour système de la version préliminaire pour développeurs du programme Xbox inclut des versions logicielles préliminaires anticipées et expérimentales. Cela signifie que certains jeux et applications courants ne fonctionneront pas comme prévu, et que vous risquez d’être confronté à des blocages occasionnels et à une perte de données. Si vous quittez la version d’évaluation pour développeurs, votre console procédera à une réinitialisation aux paramètres d’usine. Vous devrez, par conséquent, réinstaller l’ensemble de vos jeux, de vos applications et de votre contenu.

Pour les développeurs, cela implique que certains outils et API de développement ne fonctionneront pas correctement. Les fonctionnalités destinées à la version finale ne seront pas toutes incluses ou de qualité de mise en production. 
**En conséquence, les performances système de cette version préliminaire ne refléteront pas celles de la version finale.**

La liste ci-après met en relief certains problèmes connus que vous pouvez rencontrer dans cette version, mais cette liste n’est pas exhaustive. 

**Vos commentaires nous intéressent**. Par conséquent, n’hésitez pas à nous faire part de vos éventuels problèmes sur le forum [Développement d’applications Windows universelles](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

Si vous êtes bloqué, lisez les informations présentées dans cette rubrique, consultez le [Forum aux questions](frequently-asked-questions.md) et utilisez les forums pour demander de l’aide.


<!--## Developing games-->

## Prise en charge du mode souris

À partir de cette version d’évaluation, le _mode souris_ est activé par défaut pourXAML et les applicationsweb hébergées. Toutes les applications qui ne le refusent pas recevront un pointeur de souris semblable à celui du navigateurMicrosoftEdge Xbox.

**Nous recommandons vivement aux développeurs de désactiver le modesouris et d’optimiser l’application pour la navigation de contrôleur (X-Y).**

Pour désactiver ce mode dans le code XAML, appuyez-vous sur l’exemple suivant:

```code
public App() {
    this.InitializeComponent();
    this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
    this.Suspending += OnSuspending;
}
```

Pour désactiver ce mode dans une applicationHTML/JavaScript, appuyez-vous sur l’exemple suivant:

```code
// Turn off mouse mode
navigator.gamepadInputEmulation = "keyboard";
```

Pour plus d’informations, notamment sur la manière d’activer la navigation directionnelle dans une application HTML/JavaScript, voir [Comment désactiver le mode souris](how-to-disable-mouse-mode.md#html).

> **Remarque**&nbsp;&nbsp;Dans cette version préliminaire pour développeurs, lorsque le mode souris est activé, un mouvement panoramique effectué avec une manette de jeu sur le contrôleur peut entraîner le blocage de la console. Si vous rencontrez ce problème, vous devez redémarrer votre console.

Pour en savoir plus sur la prise en charge du modesouris, voir [Conception pour Xbox et télévision](https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv?f=255&MSPPError=-2147217396#mouse-mode). Cette rubrique inclut des informations sur l’activation et la désactivation du modesouris, afin de vous permettre de choisir le comportement approprié pour votre application.

## Vous devez disposer d’un utilisateur connecté afin de déployer une application (erreur0x87e10008)

Les applications requièrent désormais l’ouverture d’une session par un utilisateur pour pouvoir être lancées (un utilisateur doit être connecté pour que vous puissiez démarrer le débogage à partir de Visual Studio2015 (toucheF5)). Le message d’erreur actuellement affiché par VS n’est pas limpide:
 
![Impossible d’activer l’application WindowsStore](images/windows-store-app-activation-error.jpg)
 
Pour contourner ce problème, connectez-vous à l’aide d’un utilisateur à partir de l’interpréteur de commandesXbox ou DevHome avant de déployer votre application.
 
## Les limites de mémoire des applications en arrière-plan ne sont pas encore appliquées
 
La limite de 128Mo pour les applications qui s’exécutent en arrière-plan n’est pas appliquée dans cette version d’évaluation. Ainsi, si votre application dépasse la limite de 128Mo lors de son exécution en arrière-plan, elle sera toujours en mesure d’allouer de la mémoire.
 
Il n’existe actuellement aucun moyen de contourner ce problème. Vous devez contrôler l’utilisation de la mémoire de manière appropriée. Dans une version d’évaluation à venir, votre application rencontrera des échecs d’allocation de mémoire si elle dépasse la limite de 128Mo.
 
## Le déploiement à partir de VisualStudio échoue lorsque les paramètres de contrôle parental sont activés

Le lancement de votre application à partir de Visual Studio n’aboutit pas si le contrôle parental est activé dans les paramètres.

Pour contourner ce problème, désactivez ces paramètres temporairement ou procédez comme suit:
1. Déployez votre application sur la console en désactivant les paramètres de contrôle parental.
2. Activez ces paramètres.
3. Lancez votre application à partir de la console.
4. Saisissez un mot de passe ou un codeconfidentiel pour permettre à l’application de se lancer.
5. L’application se lance.
6. Fermez l’application.
7. Effectuez le lancement à partir de Visual Studio à l’aide de la toucheF5; l’application s’ouvre sans afficher d’invite.

À ce stade, l’autorisation est _rémanente_ jusqu’à ce que vous fermiez la session de l’utilisateur, même si vous désinstallez l’application et la réinstallez.
 
Il existe un autre type d’exemption, uniquement disponible pour les comptesenfant. Un compteenfant requiert un parent, qui doit se connecter pour accorder l’autorisation adéquate. Lorsqu’il se connecte, le parent peut choisir d’autoriser systématiquement l’enfant à lancer l’application (paramètre **Toujours**). Cette exemption est stockée dans le cloud et est persistante, même si l’enfant se déconnecte et se reconnecte.   

<!--### x86 vs. x64

By the time we release later this year, we will have great support for both x86 and x64, and we do support x86 in this preview. 
However, x64 has had much more testing to date (the Xbox shell and all of the apps running on the console today are x64), and so we recommend using x64 for your projects. 
This is particularly true for games.

If you decide to use x86, please report any issues you see on the forum.

Also see [Switching build flavors can cause deployment failures](known-issues.md#switching-build-flavors-can-cause-deployment-failures) later on this page.-->

<!--### Game engines

We have tested some popular game engines, but not all of them, and our test coverage for this preview has not been comprehensive. 
Your mileage may vary. 

The following game engines have been confirmed to work:
* [Construct 2](https://www.scirra.com/)

There are likely others that are working too. We would love to get your feedback on what you find. 
Please use the forum to report any issues you see.-->

## Prise en charge de DirectX12

UWP sur XboxOne prend en charge la fonctionnalité DirectX11 niveau10. DirectX12 n’est pas pris en charge pour l’instant. À l’instar de toutes les consoles de jeu traditionnelles, XboxOne est un matériel spécialisé qui requiert un Kit de développement logiciel (SDK) spécifique pour fonctionner au maximum de ses capacités. Si vous travaillez sur un jeu sollicitant les capacités maximales du matériel Xbox One, vous pouvez vous inscrire auprès du programme [ID@XBOX](http://www.xbox.com/Developers/id) pour accéder à ce Kit de développement logiciel, qui inclut la prise en charge de DirectX 12.

<!-- ### Xbox One Developer Preview disables game streaming to Windows 10

Activating the Xbox One Developer Preview on your console will prevent you from streaming games from your Xbox One to the Xbox app on Windows 10, even if your console is set to retail mode. 
To restore the game streaming feature, you must leave the developer preview. -->

## Problème connu avec la zone adaptée à l’écran de TV

Par défaut, la zone d’affichage des applications UWP sur Xbox doit être insérée dans la zone adaptée à l’écran de TV. Toutefois, il existe un bogue connu dans la version préliminaire pour développeurs de XboxOne: la zone adaptée à l’écran de TV commence à [0, 0] plutôt qu’à [_décalage_, _décalage_].

> **Remarque**&nbsp;&nbsp;Cela s’applique uniquement aux applicationsUWP qui utilisentJavaScript.

La meilleure façon de contourner ce problème consiste à désactiver la zone adaptée à l’écran de TV, comme illustré dans l’exempleJavaScript suivant.

    var applicationView = Windows.UI.ViewManagement.ApplicationView.getForCurrentView();

    applicationView.setDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.useCoreWindow);

Pour en savoir plus sur cette zone, voir [Conception pour Xbox et télévision](https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv).

<!--## System resources for UWP apps and games on Xbox One

UWP apps and games running on Xbox One share resources with the system and other apps, and so the system governs the resources that are available to any one game or app. 
If you are running into memory or performance issues, this may be why. 
For more details, see [System resources for UWP apps and games on Xbox One](system-resource-allocation.md).-->

<!--
## Networking using traditional sockets

In this developer preview, inbound and outbound network access from the console that uses traditional TCP/UDP sockets (WinSock, Windows.Networking.Sockets) is not available. 
Developers can still use HTTP and WebSockets.
--> 


## Couverture des API UWP

Les APIUWP ne sont pas toutes prises en charge sur Xbox. Pour obtenir la liste des API dont nous savons qu’elles ne fonctionnent pas, consultez [Fonctionnalités UWP qui ne sont pas encore prises en charge sur Xbox](http://go.microsoft.com/fwlink/p/?LinkId=760755). Si vous rencontrez des problèmes avec d’autres API, signalez-les sur les forums. 

<!--## XAML controls do not look like or behave like the controls in the Xbox One shell

In this developer preview, the XAML controls are not in their final form. In particular:
* Gamepad X-Y navigation does not work reliably for all controls.
* Controls do not look like controls in the Xbox shell. This includes the control focus rectangle.
* Navigating between controls does not automatically make “navigation sounds.”

These issues will be addressed in a future developer preview.-->

<!--## Visual Studio and deployment issues

### Switching build flavors can cause deployment failures

Switching between Debug and Release builds, or between x86 and x64, or between Managed and .Net Native builds, can cause deployment failures. 

The simplest way to avoid these issues for this preview is to stick to Debug and one architecture. 

If you do hit this issue, uninstalling your app in the Collections app on your Xbox One will typically resolve it.

> ****&nbsp;&nbsp;Uninstalling your app from Windows Device Portal (WDP) will not resolve the issue.

If your issues persist, uninstall your app or game in the Collections app, leave Developer Mode, restart to Retail Mode and then switch back to Developer Mode.
You may also need to restart Visual Studio and clean your solution.

For more information, see the “Fixing deployment failures” section in [Frequently asked questions](frequently-asked-questions.md).

### Uninstalling an app while you are debugging it in Visual Studio will cause it to fail silently

Attempting to uninstall an app that is running under the debugger via the WDP “Installed Apps” tool will cause it to silently fail. 
The workaround is to stop debugging the app in Visual Studio before attempting to remove it via WDP.

### Visual Studio/Xbox PIN pairing failures

It is possible to get into a state where the PIN pairing between Visual Studio and your Xbox One gets out of sync. 
If PIN pairing fails, use the “Remove all pairings” button in Dev Home, restart Xbox One, restart your development PC, and then try again.--> 


## Version préliminaire de WindowsDevicePortal (WDP)

<!--### Starting WDP from Dev Home crashes Dev Home

When you start WDP in Dev Home, it will cause Dev Home to crash after you have entered your user name and password and selected **Save**. 
The credentials are saved but WDP is not started. 
You can start WDP by restarting Xbox One.--> 

<!--### Disabling WDP in Dev Home does not work

If you disable WDP in Dev Home, it will be turned off. 
However, when you restart your Xbox One, WDP will be started again. 
You can work around this issue by using **Reset and keep my games & apps** to delete any stored state on your Xbox One. 
Go to Settings > System > Console info & updates > Reset console, and then select the **Reset and keep my games & apps** button.

> **Caution**&nbsp;&nbsp;Doing this will delete all saved settings on your Xbox One including wireless settings, user accounts and any game progress that has not been saved to cloud storage.

> **Caution**&nbsp;&nbsp;DO NOT select the **Reset and remove everything** button.
This will delete all of your games, apps, settings and content, deactivate Developer Mode, and remove you console from the Developer Preview group.

### The columns in the “Running Apps” table do not update predictably. 

Sometimes this is resolved by sorting a column on the table.-->

### Affichage incorrect de l’interface utilisateur de WDP dans InternetExplorer7 

Par défaut, l’interface utilisateur de WDP ne s’affiche pas correctement dans le navigateur lorsque vous utilisez InternetExplorer7. Vous pouvez résoudre ce problème en désactivant l’Affichage de compatibilité d’Internet Explorer7 pour WDP.

### Avertissement de sécurité déclenché par l’accès à WDP

Vous recevrez un avertissement concernant le certificat fourni, semblable à la capture d’écran ci-dessous, car le certificat de sécurité signé par votre console XboxOne n’est pas considéré comme un éditeur approuvé bien connu. Cliquez sur «Poursuivre sur ce site web» pour accéder à Windows Device Portal.

![Avertissement concernant le certificat de sécurité d’un site web](images/security_cert_warning.jpg)

<!--## Dev Home

Occasionally, selecting the “Manage Windows Device Portal” option in Dev Home will cause Dev Home to silently exit to the Home screen. 
This is caused by a failure in the WDP infrastructure on the console and can be resolved by restarting the console.-->

## Voir aussi
- [Forum Aux Questions](frequently-asked-questions.md)
- [UWP sur XboxOne](index.md)



<!--HONumber=Jul16_HO2-->


