---
author: Mtoepke
title: "Problèmes connus avec UWP dans le programme pour les développeurs Xbox One"
description: 
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: a7b82570-1f99-4bc3-ac78-412f6360e936
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 4b13b9bbbc75de47ed69112680894d5e3f34d8a1
ms.lasthandoff: 02/08/2017

---

# <a name="known-issues-with-uwp-on-xbox-developer-program"></a>Problèmes connus avec UWP dans le programme pour les développeurs Xbox

Cette rubrique décrit les problèmes connus liés à la plateforme UWP dans le programme pour les développeurs Xbox. Pour plus d’informations sur ce programme, voir [UWP sur Xbox](index.md). 

\[Si vous avez accédé à cette page à partir d’un lien dans une rubrique de référence d’une API et que vous recherchez des informations sur les API pour les appareils universels, consultez [Fonctionnalités UWP non encore prises en charge sur Xbox](http://go.microsoft.com/fwlink/?LinkID=760755).\]

La liste ci-après répertorie certains problèmes connus que vous pourriez rencontrer, mais cette liste n’est pas exhaustive. 

**Vos commentaires nous intéressent**. Par conséquent, n’hésitez pas à nous faire part de vos éventuels problèmes sur le forum [Développement d’applications de plateforme Windows universelle](https://social.msdn.microsoft.com/forums/windowsapps/home?forum=wpdevelop). 

Si vous êtes bloqué, lisez les informations présentées dans cette rubrique, consultez le [Forum aux questions](frequently-asked-questions.md) et utilisez les forums pour demander de l’aide.


<!--## Developing games-->

## <a name="issue-when-leaving-dev-mode"></a>Problème lors de la fermeture du mode développeur
Il arrive parfois que vous ne puissiez pas quitter le mode développeur ni en utilisant DevHome ni dans Paramètres de développeur.
Il existe deux solutions possibles pour contourner ce problème : 
1. Lorsque vous quittez le mode développeur, décochez la case **Delete side loaded games and apps**.
2. Accédez à Mes jeux et applications et désinstallez les applications de développeur que vous avez éventuellement installées sur votre console.
 
<!--## Memory limits for background apps are partially enforced
 
The maximum memory footprint for apps running in the background is 128 megabytes. In the current version of UWP on Xbox One, your app will be suspended if it is above this limit when it is moved to the background. This limit is not currently enforced if your app exceeds the limit while it is already running in the background—this means that if your app exceeds 128 MB while running in the background, it will still be able to allocate memory.
 
There is currently no workaround for this issue. Apps should govern their memory usage accordingly and continue to stay under the 128 MB limit while running in the background.-->
 
## <a name="deploying-from-vs-fails-with-parental-controls-turned-on"></a>Le déploiement à partir de Visual Studio échoue lorsque les paramètres de contrôle parental sont activés

Le lancement de votre application à partir de Visual Studio n’aboutit pas si le contrôle parental est activé dans les paramètres.

Pour contourner ce problème, désactivez ces paramètres temporairement ou procédez comme suit :
1. Déployez votre application sur la console en désactivant les paramètres de contrôle parental.
2. Activez ces paramètres.
3. Lancez votre application à partir de la console.
4. Saisissez un mot de passe ou un code confidentiel pour permettre à l’application de se lancer.
5. L’application se lance.
6. Fermez l’application.
7. Effectuez le lancement à partir de Visual Studio à l’aide de la touche F5 ; l’application s’ouvre sans afficher d’invite.

À ce stade, l’autorisation est _rémanente_ jusqu’à ce que vous fermiez la session de l’utilisateur, même si vous désinstallez l’application et la réinstallez.
 
Il existe un autre type d’exemption, uniquement disponible pour les comptes enfant. Un compte enfant requiert un parent, qui doit se connecter pour accorder l’autorisation adéquate. Lorsqu’il se connecte, le parent peut choisir d’autoriser systématiquement l’enfant à lancer l’application (paramètre **Toujours**). Cette exemption est stockée dans le cloud et est persistante, même si l’enfant se déconnecte et se reconnecte.   

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

## <a name="directx-12-support"></a>Prise en charge de DirectX 12

UWP sur Xbox One prend en charge la fonctionnalité DirectX 11 niveau 10. DirectX 12 n’est pas pris en charge pour l’instant. 

À l’instar de toutes les consoles de jeu traditionnelles, Xbox One est un matériel spécialisé qui requiert un Kit de développement logiciel (SDK) spécifique pour fonctionner au maximum de ses capacités. Si vous travaillez sur un jeu sollicitant les capacités maximales du matériel Xbox One, vous pouvez vous inscrire au programme [ID@XBOX](http://www.xbox.com/Developers/id) pour accéder à ce Kit SDK, qui inclut la prise en charge de DirectX 12.

<!-- ### Xbox One Developer Preview disables game streaming to Windows 10

Activating the Xbox One Developer Preview on your console will prevent you from streaming games from your Xbox One to the Xbox app on Windows 10, even if your console is set to retail mode. 
To restore the game streaming feature, you must leave the developer preview. -->

<!--## System resources for UWP apps and games on Xbox One

UWP apps and games running on Xbox One share resources with the system and other apps, and so the system governs the resources that are available to any one game or app. 
If you are running into memory or performance issues, this may be why. 
For more details, see [System resources for UWP apps and games on Xbox One](system-resource-allocation.md).-->

<!--
## Networking using traditional sockets

In this developer preview, inbound and outbound network access from the console that uses traditional TCP/UDP sockets (WinSock, Windows.Networking.Sockets) is not available. 
Developers can still use HTTP and WebSockets.
--> 

## <a name="blocked-networking-ports-on-xbox-one"></a>Ports réseau bloqués sur Xbox One

Les applications de plateforme Windows universelle (UWP) sur les appareils Xbox One ne sont pas autorisées à établir une liaison aux ports dans la plage [57344, 65535]&nbsp;(numéros de port inclus). Même si la liaison à ces ports semble réussir au moment de l’exécution, le trafic réseau peut être annulé sans avertissement avant d’atteindre votre application. Votre application doit si possible établir une liaison au port  0, ce qui permet au système de sélectionner le port local. Si vous avez besoin d’utiliser un port spécifique, le numéro de port doit être dans la plage [1025, 49151]. Vous devez vérifier dans le Registre IANA et éviter les conflits. Pour plus d’informations, voir le [Registre des noms de services et des numéros de ports des protocoles de transport](http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml).

## <a name="uwp-api-coverage"></a>Couverture des API UWP

Les API UWP ne sont pas toutes prises en charge sur Xbox. Pour obtenir la liste des API dont nous savons qu’elles ne fonctionnent pas, voir [Fonctionnalités UWP qui ne sont pas encore prises en charge sur Xbox](http://go.microsoft.com/fwlink/p/?LinkId=760755). Si vous rencontrez des problèmes avec d’autres API, signalez-les sur les forums. 

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


<!--## Windows Device Portal (WDP) preview-->

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

## <a name="navigating-to-wdp-causes-a-certificate-warning"></a>Avertissement de sécurité déclenché par l’accès à WDP

Vous recevrez un avertissement concernant le certificat fourni, semblable à la capture d’écran ci-dessous, car le certificat de sécurité signé par votre console Xbox One n’est pas considéré comme un éditeur approuvé bien connu. Pour accéder à Windows Device Portal, cliquez sur **Poursuivre sur ce site web**.

![Avertissement concernant le certificat de sécurité d’un site web](images/security_cert_warning.jpg)

<!--## Dev Home

Occasionally, selecting the “Manage Windows Device Portal” option in Dev Home will cause Dev Home to silently exit to the Home screen. 
This is caused by a failure in the WDP infrastructure on the console and can be resolved by restarting the console.-->

## <a name="knownfoldersmediaserverdevices-caveat-on-xbox"></a>Risque KnownFolders.MediaServerDevices sur Xbox

Sur le Bureau, les serveurs multimédias sont associés au PC, et le service d’association de périphérique identifie en temps réel les serveurs en ligne, afin qu’une requête initiale de système de fichiers puisse immédiatement renvoyer une liste des serveurs associés.

Xbox ne présente aucune interface utilisateur permettant d’ajouter ou de supprimer des serveurs. En conséquence, la requête initiale de système de fichiers renvoie toujours une réponse vide. Vous devez créer une requête, vous abonner à l’événement ContentsChanged, et actualiser la requête à chaque fois que vous recevez une notification. Les serveurs apparaîtront petit à petit ; la plupart d’entre eux seront découverts dans un intervalle de 3 secondes.

Exemple de code simple :

```
namespace TestDNLA {

    public sealed partial class MainPage : Page {
        public MainPage() {
            this.InitializeComponent();
        }

        private async void FindFiles_Click(object sender, RoutedEventArgs e) {
            try {
                StorageFolder library = KnownFolders.MediaServerDevices;
                var folderQuery = library.CreateFolderQuery();
                folderQuery.ContentsChanged += FolderQuery_ContentsChanged;
                IReadOnlyList<StorageFolder> rootFolders = await folderQuery.GetFoldersAsync();
                if (rootFolders.Count == 0) {
                    Debug.WriteLine("No Folders found");
                } else {
                    Debug.WriteLine("Folders found");
                }
            } catch (Exception ex) {
                Debug.WriteLine("Error: " + ex.Message);
            } finally {
                Debug.WriteLine("Done");
            }
        }

        private async void FolderQuery_ContentsChanged(Windows.Storage.Search.IStorageQueryResultBase sender, object args) {
            Debug.WriteLine("Folder added " + sender.Folder.Name);
            IReadOnlyList<StorageFolder> topLevelFolders = await sender.Folder.GetFoldersAsync();
            foreach (StorageFolder topLevelFolder in topLevelFolders) {
                Debug.WriteLine(topLevelFolder.Name);
            }
        }
    }
}
```

## <a name="see-also"></a>Voir également
- [Forum Aux Questions](frequently-asked-questions.md)
- [UWP sur Xbox One](index.md)

