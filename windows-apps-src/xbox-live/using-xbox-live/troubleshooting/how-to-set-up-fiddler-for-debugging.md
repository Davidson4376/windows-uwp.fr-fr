---
title: Résolution des problèmes de Xbox Live à l’aide de Fiddler
description: Découvrez comment utiliser Fiddler pour résoudre les appels de service Xbox Live.
ms.assetid: 7d76e444-027b-4659-80d5-5b2bf56d199e
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, fiddler, appels de service, résoudre les problèmes
ms.localizationpriority: medium
ms.openlocfilehash: 84c6717a4f9f5aff9fd3ff1f68c870fdd9174865
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606714"
---
# <a name="troubleshooting-xbox-live-using-fiddler"></a>Résolution des problèmes de Xbox Live à l’aide de Fiddler

Fiddler est un proxy qui enregistre le trafic de tous les HTTP et HTTPS entre votre appareil et Internet de débogage web. Vous allez l’utiliser pour vous connecter et d’inspecter le trafic vers et depuis les services Xbox Live et une partie de confiance services web tiers, à comprendre et à déboguer des appels de service web.

## <a name="for-windows-uwp-pc-apps"></a>Pour les applications Windows UWP PC

1. Assurez-vous que l’utilisateur actuel est dans le groupe administrateur sur le PC
1. Télécharger Fiddler à partir de [https://www.telerik.com/fiddler](https://www.telerik.com/fiddler)
1. Veillez à sélectionner la version « Repose pour .NET 4 »
1. Une fois installé, accédez à outils -> Options de Fiddler et activer Capture HTTPS CONNECTs et HTTPS de déchiffrer le trafic.  Toutes les communications entre le runtime et les services Xbox LIVE sont chiffrées avec SSL.  Sans cette option, vous ne verrez quelque chose d’utile.  Acceptez toutes les boîtes de dialogue que Fiddler s’affiche (doivent être des boîtes de 5 dialogue y compris UAC)
1. Accédez à « WinConfig », « Exempter tout » et « Enregistrer les modifications ».  Sinon Fiddler ne fonctionnera pas avec les applications de Store.
1. Si vous exécutez votre application doit maintenant apparaître les demandes/réponses entre l’application, le runtime et le Xbox LIVE.

Pour déboguer les appels au niveau du système d’exploitation UWP qui n’est pas nécessaire en général, mais peut être utile lors du débogage des événements de connexion et dans le jeu, vous devez configurer Fiddler pour capturer le trafic de WinHTTP.
Cela est possible avec :
```cpp
    netsh winhttp set proxy 127.0.0.1:8888 "<-loopback>"
```
où le port 8888 correspond au port que vous avez configuré sur les outils Fiddlers | Options | Onglet Connexions qui est 8888 par défaut.
Pour annuler cette opération, procédez comme :
```cpp
    netsh winhttp reset proxy
```

## <a name="for-xbox-one-uwp-based-projects"></a>Pour les projets basés sur des UWP une Xbox

Suivez les étapes décrites ici [https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/uwp-fiddler](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/uwp-fiddler)

## <a name="for-xbox-one-xdk-based-projects"></a>Pour les projets de Xbox XDK un en fonction

En fonctionnement normal, une console qui communique via un proxy court le risque que ses communications soient modifiées par celui-ci, ce qui permet des triches éventuelles des joueurs. Par conséquent, les consoles sont conçus pour permettre la communication via un proxy. SI vous utilisez Fiddler avec votre kit de développement Xbox One, vous devez effectuer une configuration spéciale sur le kit de développement pour lui permettre d’utiliser le proxy Fiddler.

Fiddler est un logiciel gratuit, disponible au téléchargement sur le [site web de Fiddler](https://www.telerik.com/fiddler/).

Fiddler peut modifier l’état du réseau signalé par la console. Si une connexion en amont est désactivée à partir de l’ordinateur exécutant Fiddler, la console peut ne pas détecter cette déconnexion jusqu’à ce que l’authentification de la console expire. Si vous utilisez Fiddler, veillez à fermer la connexion entre la console et l’ordinateur exécutant Fiddler, plutôt que d’utiliser Fiddler pour simuler une déconnexion. Encore mieux, utiliser la contrainte de réseau outils simulent par conséquent, la déconnexion à des fins de test.

Installation et activation de Fiddler sur le PC de développement

Suivez ces étapes pour installer et activer Fiddler surveiller le trafic à partir de votre kit de développement.

1. Installez Fiddler sur votre PC de développement en suivant les instructions du [site web Fiddler](https://www.telerik.com/fiddler/).
1. Lancer Fiddler et sélectionnez les Options de Fiddler dans le menu Outils.
1. Sélectionnez l’onglet Connexions et vérifiez que les ordinateurs distants autoriser à se connecter de case à cocher est cochée.
1. Cliquez sur OK pour accepter votre changement dans les paramètres. Une boîte de dialogue s’affiche alors, indiquant que Fiddler doit être redémarré pour que les modifications prennent effet, et que vous devrez configurer votre pare-feu manuellement. Cliquez sur OK dans cette boîte de dialogue, mais ne redémarrez pas encore de Fiddler.
1. Configurez la règle de pare-feu requise pour permettre à des ordinateurs distants de se connecter. Démarrez l’applet de commande Panneau de configuration Pare-feu Windows. Cliquez sur Paramètres avancés et règle de trafic entrant puis. Recherchez la règle appelée « FiddlerProxy », puis faites défiler vers la droite, en vérifiant que chacun des paramètres suivants s’affiche pour cette règle.

| Paramètre          | Valeur préférée                |
|------------------|--------------------------------|
| Nom             | FiddlerProxy                   |
| Group            | (ne définissez pas une valeur pour le groupe) |
| Profil          | Tous                            |
| Activé          | Oui                            |
| Action           | Autoriser                          |
| Remplacer         | Non                             |
| Programme          | chemin d’accès au fiddler.exe            |
| LocalAddress     | Indéfini                            |
| RemoteAddress    | Indéfini                            |
| Protocole         | TCP                            |
| LocalPort        | Indéfini                            |
| RemotePort       | Indéfini                            |
| AllowedUsers     | Indéfini                            |
| AllowedComputers | Indéfini                            |


1. Configurez Fiddler pour capturer et de déchiffrer le trafic HTTPS.
    1. Pour activer les meilleures performances, définissez Fiddler pour utiliser le Mode de diffusion en continu en cliquant sur le bouton de Stream dans la barre de bouton.
    1. Dans Fiddler, sélectionnez Outils, puis les Options de Fiddler et HTTPS.
    1. Case à cocher HTTPS de déchiffrer le trafic. Si un élément messagebox vous demande s’il faut configurer Windows pour approuver le certificat d’autorité de certification, cliquez sur non.
    1. Cliquez sur le certificat racine exporter au bouton du bureau.
1. Quitter Fiddler et démarrez-le à nouveau.

### <a name="to-configure-a-dev-kit-to-use-fiddler-as-its-proxy-to-the-internet"></a>Configurer un kit de développement pour utiliser Fiddler comme proxy pour Internet
Configuration de Fiddler sur le kit de développement a été simplifiée à partir de la méthode utilisée dans les versions précédentes.

1. Copiez le certificat racine Fiddler que vous avez exporté sur le bureau, le Kit de développement en tant que``` xs:\Microsoft\Cert\FiddlerRoot.cer```.  Vous pouvez utiliser la commande suivante :  ```xbcp [local Fiddler Root directory]\FiddlerRoot.cer xs:\Microsoft\Cert\FiddlerRoot.cer```
1. Créez un fichier texte nommé ```ProxyAddress.txt```, avec l’adresse IP ou le nom d’hôte de l’ordinateur de développement en cours d’exécution Fiddler et le numéro de port où Fiddler écoute en tant que seul le contenu dans le fichier. Mettre en forme le nom/adresse IP et port comme suit : « HÔTE : PORT ». (Par défaut, Fiddler utilise le port 8888). Par exemple, « 10.124.220.250:8888 » ou « my_dev_pc.contoso.com:8888 ». Copiez ce fichier dans le kit de développement en tant que xs:\Microsoft\Fiddler\ProxyAddress.txt.  Vous pouvez utiliser la commande suivante :  ```xbcp [local Proxy Address file directory]\ProxyAddress.txt xs:\Microsoft\Fiddler\ProxyAddress.txt```
1. Redémarrer le kit de développement en tapant ```xbreboot``` dans l’invite de commandes

### <a name="to-stop-using-fiddler"></a>Arrêter l’utilisation de Fiddler

Pour arrêter à l’aide de Fiddler comme un proxy vers Internet et par conséquent, le suivi de tout trafic de réseau du kit de développement dans Fiddler, simplement renommer ou supprimer le fichier FiddlerRoot.cer sur le kit de développement, puis redémarrez le kit de développement.

### <a name="how-it-works"></a>Fonctionnement

La présence d’un fichier xs:\Microsoft\Cert\FiddlerRoot.cer et un fichier xs:\Microsoft\Fiddler\ProxyAddress.txt au moment du démarrage entraîne le kit de développement pour se configurer lui-même pour utiliser le serveur proxy spécifié dans ProxyAddress.txt. Si des fichiers sont manquant, puis le kit de développement configurera pas de fonctionner à travers un proxy Fiddler.

Chaque PC doté de Fiddler utilise un certificat racine Fiddler différent. Si vous avez plusieurs PC qui peut être utilisé pour fournir un proxy Fiddler pour votre kit de développement, vous pouvez copier le certificat racine de Fiddler chaque PC dans le répertoire xs:\Microsoft\Cert, tant qu’un d’eux est nommé FiddlerRoot.cer. Tous les certificats dans le répertoire de certificat sont activés lorsqu’un proxy Fiddler est authentifié. L’instance de Fiddler sur l’adresse quelle que soit la PC est en ProxyAddress.txt sera utilisée en tant que proxy, tant que son certificat est présent dans le répertoire du certificat.
