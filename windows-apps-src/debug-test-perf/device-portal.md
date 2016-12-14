---
author: mcleblanc
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: "Vue d’ensemble de Windows Device Portal"
description: "Découvrez comment Windows Device Portal vous permet de configurer et de gérer à distance votre appareil par le biais d’une connexion réseau ou USB."
translationtype: Human Translation
ms.sourcegitcommit: 8dee2c7bf5ec44f913e34f1150223c1172ba6c02
ms.openlocfilehash: 6c697782683bca6671c01aa0941a78bc66fb052a

---
# <a name="windows-device-portal-overview"></a>Vue d’ensemble de Windows Device Portal

Windows Device Portal vous permet de configurer et de gérer à distance votre appareil par le biais d’une connexion réseau ou USB. Il fournit également des outils de diagnostic avancés pour vous permettre de résoudre les problèmes et d’afficher les performances en temps réel de votre appareil Windows.

Device Portal est un serveur web sur l’appareil auquel vous pouvez vous connecter à partir d’un navigateur web sur votre PC. Si votre appareil dispose d’un navigateur web, vous pouvez également vous connecter localement avec le navigateur sur votre appareil.

Windows Device Portal est disponible sur chaque famille d’appareils. Toutefois, les fonctionnalités et la configuration varient en fonction des exigences de l’appareil. Cet article fournit une description générale de Device Portal et des liens vers des articles contenant des informations plus spécifiques pour chaque famille d’appareils.

Dans Windows Device Portal, tout repose sur les [API REST](device-portal-api-core.md) que vous pouvez utiliser pour accéder aux données et contrôler votre appareil par programme.

## <a name="setup"></a>Installation

Chaque appareil possède des instructions spécifiques concernant la connexion à Device Portal. Toutefois, chacun nécessite d’effectuer les étapes générales suivantes.
1. Activez le mode développeur et Device Portal sur votre appareil.
2. Connectez votre appareil et votre PC via USB ou un réseau local.
3. Accéder à la page Device Portal dans votre navigateur. Le tableau suivant répertorie les ports et protocoles utilisés par chaque famille d’appareils.

Famille d’appareils | Activé par défaut ? | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | Oui, en mode de développement | 80 (par défaut) | 443 (par défaut) | http://127.0.0.1:10080
IoT | Oui, en mode de développement | 8080 | Activer via la clé de registre | Non applicable
Xbox | Activer dans le mode de développement | Désactivé | 11443 | Non applicable
Bureau| Activer dans le mode de développement | 50080\* | 50043\* | Non applicable
Téléphone | Activer dans le mode de développement | 80| 443 | http://127.0.0.1:10080

\ * Cela n’est pas toujours le cas, car Device Portal sur le bureau revendique des ports dans la plage éphémère (&gt; 50 000) afin d’éviter les collisions avec les déclarations de port existant sur l’appareil.  Pour plus d’informations, consultez la section [Paramètres de port](device-portal-desktop.md#setting-port-numbers) pour le bureau.  

Pour obtenir des instructions d’installation propres à chaque appareil, consultez :
- [Device Portal pour HoloLens](https://dev.windows.com/holographic/using_the_windows_device_portal)
- [Device Portal pour IoT](https://go.microsoft.com/fwlink/?LinkID=616499)
- [Device Portal pour appareils mobiles](device-portal-mobile.md)
- [Device Portal pour Xbox](device-portal-xbox.md)
- [Device Portal pour Bureau](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>Fonctionnalités

### <a name="toolbar-and-navigation"></a>Barre d’outils et navigation

La barre d’outils se trouvant en haut de la page permet d’accéder aux fonctionnalités et aux états couramment utilisés.
- **Arrêt** : éteint l’appareil.
- **Redémarrer** : mise sous tension de l’appareil par cycle.
- **Aide** : ouvre la page d’aide.

Utilisez les liens du volet de navigation sur le côté gauche de la page pour naviguer vers les outils d’analyse et de gestion disponibles pour votre appareil.

Les outils courants des appareils sont décrits ici. D’autres options peuvent être disponibles selon l’appareil. Pour plus d’informations, voir la page spécifique de votre appareil.

### <a name="home"></a>Accueil

Votre session Device Portal démarre sur la page d’accueil. La page d’accueil affiche généralement des informations sur l’appareil, comme le nom, la version du système d’exploitation et les préférences que vous pouvez définir pour l’appareil.

### <a name="apps"></a>Applications

Propose des fonctionnalités d’installation/de désinstallation et de gestion pour les packages AppX et les offres groupées sur votre appareil

![Device Portal pour appareils mobiles](images/device-portal/mob-device-portal-apps.png)

- **Applications installées** : supprime et démarre les applications.
- **Applications en cours d’exécution** : répertorie les applications actuellement en cours d’exécution.
- **Installer l’application** : sélectionnez les packages d’application à installer à partir d’un dossier de votre ordinateur ou de votre réseau.
- **Dépendance** : ajoutez des dépendances pour l’application que vous souhaitez installer.
- **Déployer** : déployez l’application sélectionnée et les dépendances sur votre appareil.

**Pour installer une application**

1.  Lorsque vous avez [créé un package d’application](https://msdn.microsoft.com/library/windows/apps/xaml/hh454036(v=vs.140).aspx), vous pouvez l’installer à distance sur votre appareil. Une fois créé dans Visual Studio, un dossier de sortie est généré.

    ![Installation d’applications](images/device-portal/iot-installapp0.png)
2.  Cliquez sur Parcourir et recherchez votre package d’application (.appx).
3.  Cliquez sur Parcourir et recherchez le fichier de certificat (.cer). (Non requis sur tous les appareils.)
4.  Ajoutez des dépendances. Si vous avez plusieurs objets, ajoutez chacun d’eux individuellement.     
5.  Sous **Déployer**, cliquez sur **OK**. 
6.  Pour installer une autre application, cliquez sur le bouton **Réinitialiser** pour effacer les champs.


**Pour désinstaller une application**

1.  Assurez-vous que votre application n’est pas en cours d’exécution. 
2.  Si c’est le cas, passez à Applications en cours d’exécution et fermez l’application en question. Si vous essayez de désinstaller une application en cours d’exécution, celle-ci provoquera des problèmes lors de sa réinstallation. 
3.  Dès que vous êtes prêt, cliquez sur **Désinstaller**.

### <a name="processes"></a>Processus

Affiche les détails concernant les processus en cours d’exécution. Cela comprend les processus relatifs aux applications au système.

Comme le Gestionnaire de tâches sur votre PC, cette page vous permet de voir les processus en cours d’exécution, ainsi que leur utilisation de la mémoire.  Sur certaines plateformes (Desktop, IoT et HoloLens), vous pouvez mettre fin aux processus.

![Device Portal pour appareils mobiles](images/device-portal/mob-device-portal-processes.png)

### <a name="performance"></a>Performances

Présente des graphiques en temps réel des informations de diagnostic système, telles que la consommation d’énergie, la fréquence d’images et la charge du processeur.

Voici les mesures disponibles :
- **Processeur** : pourcentage du total disponible
- **Mémoire** : totale, en cours d’utilisation, disponible validée, paginée et non paginée
- **GPU** : utilisation du moteur GPU, pourcentage du total disponible
- **E/S** : lectures et écritures
- **Réseau** : réceptions et envois

![Device Portal pour appareils mobiles](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw"></a>Suivi d’événements pour Windows (ETW)

Gère en temps réel le suivi d’événements pour Windows (ETW) sur l’appareil.

![Device Portal pour appareils mobiles](images/device-portal/mob-device-portal-etw.png)

Cochez la case **Masquer les fournisseurs** pour n’afficher que la liste des événements.
- **Fournisseurs enregistrés** sélectionnez le fournisseur ETW et le niveau de suivi. Le niveau de suivi est l’une des valeurs suivantes :
    1. Sortie ou arrêt anormal
    2. Erreurs graves
    3. Avertissements
    4. Avertissements sans erreur
    5. Suivi détaillé (*)

Cliquez ou appuyez sur **Activer** pour démarrer le suivi. Le fournisseur est ajouté à la liste déroulante **Fournisseurs activés**.
- **Fournisseurs personnalisés** sélectionnez un fournisseur ETW personnalisé et le niveau de suivi. Identifiez le fournisseur par son GUID. N’insérez pas de crochets dans le GUID.
- **Fournisseurs activés** : répertorie les fournisseurs activés. Sélectionnez un fournisseur dans la liste déroulante, puis cliquez sur ou appuyez sur **Désactiver** pour arrêter le suivi. Cliquez ou appuyez sur **Arrêter tout** pour suspendre tout le suivi.
- **Historique des fournisseurs** : affiche les fournisseurs ETW activés pendant la session active. Cliquez ou appuyez sur **Activer** pour activer un fournisseur qui a été désactivé. Cliquez ou appuyez sur **Effacer** pour supprimer l’historique.
- **Événements** : répertorie les événements ETW des fournisseurs sélectionnés sous forme de tableau. Le tableau suivant est mis à jour en temps réel. En dessous du tableau, cliquez sur le bouton **Effacer** pour supprimer tous les événements ETW du tableau. Cela ne désactive pas les fournisseurs. Vous pouvez cliquer sur **Enregistrer dans le fichier** pour exporter vers un fichier CSV en local les événements ETW actuellement collectés.

Pour plus d’informations sur l’utilisation du suivi ETW, consultez le [billet de blog](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/) dédié à l’utilisation de cette fonctionnalité pour collecter des journaux en temps réel à partir de votre application. 

### <a name="performance-tracing"></a>Suivi des performances

Capturez les suivis de l’[Enregistreur de performance Windows](https://msdn.microsoft.com/library/windows/hardware/hh448205.aspx) (WPR) à partir de votre appareil.

![Device Portal pour appareils mobiles](images/device-portal/mob-device-portal-perf-tracing.png)

- **Profils disponibles** : sélectionnez le profil WPR dans la liste déroulante, puis cliquez ou appuyez sur **Démarrer** pour commencer le suivi.
- **Profils personnalisés** : cliquez ou appuyez sur **Parcourir** pour choisir un profil WPR depuis votre PC. Cliquez ou appuyez sur **Charger et démarrer** pour commencer le suivi.

Pour arrêter le suivi, cliquez sur **Arrêter**. Restez sur cette page jusqu’à ce que le fichier de suivi (. ETL) a terminé le téléchargement.

Les fichiers ETL capturés peuvent être ouverts pour analyse dans [Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/hardware/hh448170.aspx).

### <a name="devices"></a>Appareils

Énumère tous les périphériques connectés à votre appareil.

![Device Portal pour appareils mobiles](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>Mise en réseau

Gère les connexions réseau sur l’appareil.  Sauf si vous êtes connecté à Device Portal via USB, la modification de ces paramètres entraînera certainement la déconnexion de Device Portal.
- **Profils** : sélectionnez un autre profil Wi-Fi à utiliser.  
- **Réseaux disponibles** : réseaux Wi-Fi disponibles sur l’appareil. Appuyez ou cliquez sur un réseau pour vous y connecter et fournir une clé d’accès si nécessaire. Remarque : Device Portal ne gère pas encore l’authentification en entreprise. 

![Device Portal pour appareils mobiles](images/device-portal/mob-device-portal-network.png)

### <a name="app-file-explorer"></a>Explorateur de fichiers de l’application

Permet d’afficher et de manipuler les fichiers stockés par vos applications chargées de manière indépendante.  Il s’agit d’une nouvelle version interplateforme de l’[outil d’exploration des stockages isolés](https://msdn.microsoft.com/library/windows/apps/hh286408(v=vs.105).aspx) de Windows Phone 8.1. Pour en savoir plus sur l’explorateur de fichiers de l’application, voir [ce billet de blog](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/). 

![Device Portal pour appareils mobiles](images/device-portal/mob-device-portal-AppFileExplorer.png)

## <a name="service-features-and-notes"></a>Remarques et fonctionnalités de service

### <a name="dns-sd"></a>DNS-SD

Device Portal signale sa présence sur le réseau local à l’aide de DNS-SD.  Toutes les instances Device Portal, quel que soit le type d’appareil, sont signalées sous « WDP._wdp._tcp.local ». Les enregistrements TXT relatifs à l’instance de service fournissent les éléments suivants :

Clé | Type | Description 
----|------|-------------
S | entier | Port sécurisé pour Device Portal.  Si la valeur est égale à 0 (zéro), Device Portal ne détecte pas les connexions HTTPS. 
D | chaîne | Type d’appareil.  Cette information sera au format « Windows.* », par exemple Windows.Xbox ou Windows.Desktop
A | chaîne | Architecture d’appareil.  Par exemple ARM, x 86 ou AMD64.  
T | liste de chaînes délimitées par des caractères Null | Balises appliquées par l’utilisateur pour l’appareil. Reportez-vous à l’API REST Tags pour apprendre à l’utiliser. La liste se termine par deux caractères Null.  

La connexion au port HTTPS est suggérée, car les appareils ne sont pas tous détectés sur le port HTTP signalé par l’enregistrement DNS-SD. 

### <a name="csrf-protection-and-scripting"></a>Protection CSRF et écriture de scripts

Afin d’offrir une protection contre les [attaques CSRF](https://wikipedia.org/wiki/Cross-site_request_forgery), un jeton unique est requis pour toutes les demandes non GET. Ce jeton, qui est l’en-tête de demande X-CSRF-Token, dérive d’un cookie de session, CSRF-Token. Dans l’interface utilisateur Device Portal, le cookie CSRF-Token est copié dans l’en-tête X-CSRF-Token à chaque demande.

**Important** Cette protection empêche toute utilisation des API REST à partir d’un client autonome (par exemple, utilitaires de ligne de commande). Cette situation peut être résolue de 3 manières différentes : 

1. Utilisez le nom d’utilisateur « auto- ». Les clients qui font précéder leur nom d’utilisateur du préfixe « auto-» contournent la protection CSRF. Ce nom d’utilisateur ne doit pas servir à se connecter à Device Portal par le biais du navigateur, car il rend le service vulnérable aux attaques CSRF. Exemple : Si le nom d’utilisateur Device Portal est « admin », ```curl -u auto-admin:password <args>``` doit être utilisé pour contourner la protection CSRF. 

2. Implémentez le schéma de type cookie vers en-tête dans le client. Cette opération nécessite une requête GET afin d’établir le cookie de session, puis l’inclusion de l’en-tête et du cookie sur toutes les requêtes ultérieures. 
 
3. Désactivez l’authentification et utilisez le protocole HTTP. La protection CSRF s’applique uniquement aux points de terminaison HTTPS : les connexions au niveau des points de terminaison HTTP n’ont donc pas besoin de satisfaire les conditions ci-dessus. 

**Remarque** : un nom d’utilisateur qui commence par « auto- » ne permet pas de se connecter à Device Portal par le biais du navigateur.  

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>Protection CSWSH (Cross-Site WebSocket Hijacking)

Afin d’éliminer les risques d’[attaques CSWSH](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html), tous les clients ouvrant une connexion WebSocket à Device Portal doivent également fournir un en-tête Origin correspondant à l’en-tête Host.  Cela prouve à Device Portal que la requête provient soit de l’interface utilisateur de Device Portal, soit d’une application cliente valide.  Si la requête ne présente pas d’en-tête Origin, elle sera rejetée. 



<!--HONumber=Dec16_HO1-->


