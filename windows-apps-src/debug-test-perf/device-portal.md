---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Vue d’ensemble du portail d’appareil Windows
description: Découvrez comment Windows Device Portal vous permet de configurer et de gérer à distance votre appareil par le biais d’une connexion réseau ou USB.
ms.date: 4/9/2019
ms.topic: article
keywords: Windows 10, uwp, le portail de l’appareil
ms.localizationpriority: medium
ms.openlocfilehash: 59e7e46ea68f6bb5fe7fd63e6ac35b9256103c38
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317902"
---
# <a name="windows-device-portal-overview"></a>Vue d’ensemble du portail d’appareil Windows

Windows Device Portal vous permet de configurer et de gérer à distance votre appareil par le biais d’une connexion réseau ou USB. Il fournit également des outils de diagnostic avancés pour vous aider à résoudre les problèmes et afficher les performances en temps réel de votre appareil Windows.

Windows Device Portal est un serveur web sur votre appareil, et vous pouvez vous connecter à partir d’un navigateur web sur un PC. Si votre appareil a un navigateur web, vous pouvez également vous connecter localement avec le navigateur sur l’appareil.

Windows Device Portal est disponible sur chaque famille de périphériques, mais les fonctionnalités et le programme d’installation varient en fonction des exigences de chaque appareil. Cet article fournit une description générale de Device Portal et des liens vers des articles contenant des informations plus spécifiques pour chaque famille d’appareils.

La fonctionnalité de la Windows Device Portal est implémentée avec [API REST](device-portal-api-core.md) que vous pouvez utiliser directement pour accéder aux données et de contrôler par programmation de votre appareil.

## <a name="setup"></a>Installation

Chaque appareil possède des instructions spécifiques concernant la connexion à Device Portal. Toutefois, chacun nécessite d’effectuer les étapes générales suivantes.

1. Activer le Mode développeur et le portail de l’appareil sur votre appareil (configuré dans l’application paramètres).

2. Connectez votre appareil et les PC via un réseau local ou USB.

3. Accéder à la page Device Portal dans votre navigateur. Ce tableau montre les ports et protocoles utilisés par chaque famille de périphériques.

Famille d’appareils | Activé par défaut ? | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | Oui, en mode de développement | 80 (par défaut) | 443 (par défaut) | http://127.0.0.1:10080
IoT | Oui, en mode de développement | 8080 | Activer via la clé de registre | N/A
Xbox | Activer dans le mode de développement | Désactivé | 11443 | N/A
Bureau| Activer dans le mode de développement | 50080\* | 50043\* | N/A
Phone | Activer dans le mode de développement | 80| 443 | http://127.0.0.1:10080

\* Cela n’est pas toujours le cas, comme le portail de l’appareil sur le bureau de revendications de ports dans la plage éphémère (> 50 000) pour empêcher les collisions avec les revendications de port existant sur l’appareil. Pour plus d’informations, consultez la section [Paramètres de port](device-portal-desktop.md#registry-based-configuration-for-device-portal) pour le bureau.  

Pour obtenir des instructions d’installation propres à chaque appareil, consultez :

- [Portail de l’appareil pour HoloLens](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens)
- [Portail de périphérique pour IoT](https://go.microsoft.com/fwlink/?LinkID=616499)
- [Portail des appareils mobiles](device-portal-mobile.md)
- [Portail des appareils pour Xbox](../xbox-apps/device-portal-xbox.md)
- [Portail des appareils pour Desktop](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>Fonctionnalités

### <a name="toolbar-and-navigation"></a>Barre d’outils et navigation

La barre d’outils en haut de la page donne accès aux fonctionnalités couramment utilisées.

- **Alimentation**: Options d’alimentation de l’accès.
  - **Arrêt**: Désactive l’appareil.
  - **Redémarrez**: Cycles mise sous tension l’appareil.
- **Aide**: Ouvre la page d’aide.

Utilisez les liens du volet de navigation sur le côté gauche de la page pour naviguer vers les outils d’analyse et de gestion disponibles pour votre appareil.

Les outils qui sont communes à des familles d’appareils sont décrits ici. D’autres options peuvent être disponibles selon l’appareil. Pour plus d’informations, consultez la page spécifique de votre type d’appareil.

### <a name="apps-manager"></a>App Manager (Gestionnaire d’applications)

Le Gestionnaire d’applications fournit l’installation/désinstallation et des fonctionnalités de gestion pour l’application des packages et d’offres groupées sur l’appareil hôte.

![Page de gestionnaire de périphérique des applications de portail](images/device-portal/WDP_AppsManager2.png)

* **Déployer des applications**: Déployer des applications empaquetées local, réseau ou hôtes web et enregistrer les fichiers libres à partir de partages réseau.
* **Applications installées**: Utilisez le menu déroulant pour supprimer ou démarrer des applications qui sont installées sur l’appareil.
* **Exécution d’applications**: Obtenir des informations sur les applications qui sont en cours d’exécution et de les fermer en fonction des besoins.

#### <a name="install-sideload-an-app"></a>Installer (chargement de version test) une application

Vous pouvez supprimer des applications pendant le développement à l’aide de Windows Device Portal :

1. Lorsque vous avez créé un package d’application, vous pouvez l’installer à distance sur votre appareil. Une fois créé dans Visual Studio, un dossier de sortie est généré.

    ![Installation d’applications](images/device-portal/iot-installapp0.png)

2. Dans Windows Device Portal, accédez à la **Gestionnaire d’applications** page.

3. Dans le **déployer des applications** section, sélectionnez **stockage Local**.

4. Sous **sélectionner le package d’application**, sélectionnez **choisir un fichier** et accédez au package d’application que vous souhaitez charger une version test.

5. Sous **fichier de sélection d’un certificat (.cer) utilisé pour signer le package d’application**, sélectionnez **choisir un fichier** et recherchez le certificat associé à ce package d’application.

6. Cochez les cases correspondants si vous souhaitez installer facultatif ou packages de framework, ainsi que l’installation de l’application, puis sélectionnez **suivant** pour les sélectionner.

7. Sélectionnez **installer** pour lancer l’installation.

8. Si l’appareil est en cours d’exécution Windows 10 en mode de S, et il est la première fois que le certificat donné a été installé sur l’appareil, redémarrez l’appareil.

#### <a name="install-a-certificate"></a>Installer un certificat

Vous pouvez également installer le certificat par le biais de Windows Device Portal et installer l’application par d’autres moyens :

1. Dans Windows Device Portal, accédez à la **Gestionnaire d’applications** page.

2. Dans le **déployer des applications** section, sélectionnez **installer le certificat**.

3. Sous **fichier de sélection d’un certificat (.cer) permet de signer un package d’application**, sélectionnez **choisir un fichier** et naviguez jusqu’au certificat associé au package d’application que vous souhaitez charger une version test.

4. Sélectionnez **installer** pour lancer l’installation.

5. Si l’appareil est en cours d’exécution Windows 10 en mode de S, et il est la première fois que le certificat donné a été installé sur l’appareil, redémarrez l’appareil.

#### <a name="uninstall-an-app"></a>Désinstaller une application

1. Assurez-vous que votre application n’est pas en cours d’exécution.
2. Dans le cas, accédez à **applications en cours d’exécution** et fermez-le. Si vous tentez de désinstaller pendant que l’application est en cours d’exécution, cela entraînera des problèmes lorsque vous essayez de réinstaller l’application.
3. Sélectionnez l’application à partir de la liste déroulante et cliquez sur **supprimer**.

### <a name="running-processes"></a>Processus en cours d’exécution

Cette page affiche les détails sur les processus en cours d’exécution sur l’appareil hôte. Cela comprend les processus relatifs aux applications au système. Sur certaines plateformes (bureau, IoT et HoloLens), vous pouvez terminer le processus.

![En cours d’exécution portail appareil traite de page](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>Explorateur de fichiers

Cette page permet d’afficher et manipuler des fichiers stockés par les versions test d’applications. Consultez le [à l’aide de l’Explorateur de fichiers d’application](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/) billet de blog pour en savoir plus sur l’Explorateur de fichiers et comment l’utiliser.

![Page de l’Explorateur de périphérique fichier du portail](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>Performances

La page de Performance présente les graphiques en temps réel des informations de diagnostic de système comme la consommation d’énergie, fréquence d’images, et la charge d’UC.

Voici les mesures disponibles :

- **PROCESSEUR**: % D’utilisation totale du processeur disponible
- **Mémoire**: Total, en cours d’utilisation, disponible et validée, paginées et réserve non paginée
- **E/S**: Lire et écrire des quantités de données
- **Réseau** : Données envoyées et reçues
- **GPU**: % Du total disponible l’utilisation du moteur GPU

![Page de performances du portail de périphérique](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>Journalisation des événements de suivi pour Windows (ETW)

La page de la journalisation ETW gère les informations de suivi d’événements pour Windows (ETW) en temps réel sur l’appareil.

![Page journalisation ETW du portail de l’appareil](images/device-portal/mob-device-portal-etw.png)

Cochez la case **Masquer les fournisseurs** pour n’afficher que la liste des événements.

- **Inscrit les fournisseurs**: Sélectionnez le fournisseur d’événements et le niveau de suivi. Le niveau de suivi est une des valeurs suivantes :
  1. Sortie ou arrêt anormal
  2. Erreurs graves
  3. Avertissements
  4. Avertissements sans erreur
  5. Suivi détaillé

  Cliquez ou appuyez sur **Activer** pour démarrer le suivi. Le fournisseur est ajouté à la liste déroulante **Fournisseurs activés**.
- **Fournisseurs personnalisés**: Sélectionnez un fournisseur ETW personnalisé et le niveau de suivi. Identifiez le fournisseur par son GUID. N’incluez pas les crochets dans le GUID.
- **Les fournisseurs activés**: Ce rapport répertorie les fournisseurs activés. Sélectionnez un fournisseur dans la liste déroulante, puis cliquez sur ou appuyez sur **Désactiver** pour arrêter le suivi. Cliquez ou appuyez sur **Arrêter tout** pour suspendre tout le suivi.
- **Historique de fournisseurs**: Cela montre les fournisseurs ETW qui ont été activées pendant la session active. Cliquez ou appuyez sur **Activer** pour activer un fournisseur qui a été désactivé. Cliquez ou appuyez sur **Effacer** pour supprimer l’historique.
- **Filtres / événements**: Le **événements** section répertorie les événements ETW à partir de fournisseurs sélectionnés sous forme de tableau. La table est mise à jour en temps réel. Utilisez le **filtres** menu pour configurer des filtres personnalisés pour lequel les événements seront affichés. Cliquez sur le **clair** bouton pour supprimer tous les événements ETW de la table. Cela ne désactive pas les fournisseurs. Vous pouvez cliquer sur **enregistrer dans le fichier** pour exporter les événements ETW actuellement collectées dans un fichier CSV local.

Pour plus d’informations sur l’utilisation de la journalisation ETW, consultez le [pour afficher les journaux de débogage, utilisez le portail appareil](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/) billet de blog.

### <a name="performance-tracing"></a>Suivi des performances

La page de suivi des performances vous permet d’afficher le [enregistreur de Performance Windows (WPR)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448205(v=win.10)) traces à partir de l’appareil hôte.

![Page de suivi de performances de portail appareil](images/device-portal/mob-device-portal-perf-tracing.png)

- **Profils disponibles**: Sélectionnez le profil WPR dans la liste déroulante, cliquez sur ou sur **Démarrer** pour démarrer le suivi.
- **Profils personnalisés**: Cliquez ou appuyez sur **Parcourir** pour choisir un profil WPR à partir de votre PC. Cliquez ou appuyez sur **Charger et démarrer** pour commencer le suivi.

Pour arrêter le suivi, cliquez sur **Arrêter**. Rester sur cette page jusqu'à ce que le fichier de trace (. ETL) est téléchargé.

Capturées. Fichiers ETL peuvent être ouverts pour l’analyse dans le [Windows Performance Analyzer](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448170(v=win.10)).

### <a name="device-manager"></a>Gestionnaire de périphériques

La page Gestionnaire de périphériques énumère tous les périphériques connectés à votre appareil. Vous pouvez cliquer sur les icônes de paramètres pour afficher les propriétés de chacun d’eux.

![Page de gestionnaire de périphérique appareil du portail](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>Mise en réseau

La page de mise en réseau gère les connexions réseau sur l’appareil. Sauf si vous êtes connecté au portail de l’appareil via USB, modification de ces paramètres est probablement vous déconnecter de portail de l’appareil.

- **Les réseaux disponibles**: Montre les réseaux Wi-Fi disponibles sur l’appareil. Appuyez ou cliquez sur un réseau pour vous y connecter et fournir une clé d’accès si nécessaire. Portail de l’appareil ne prend pas en charge l’authentification d’entreprise. Vous pouvez également utiliser le **profils** liste déroulante pour tenter de se connecter à aucun des profils Wi-Fi connus à l’appareil.
- **Configuration IP**: Affiche l’adresse sur chacun des ports de réseau de l’appareil hôte.

![Page de portail de mise en réseau de périphérique](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>Notes de publication et les fonctionnalités de Service

### <a name="dns-sd"></a>DNS-SD

Device Portal signale sa présence sur le réseau local à l’aide de DNS-SD. Toutes les instances Device Portal, quel que soit le type d’appareil, sont signalées sous « WDP._wdp._tcp.local ». Les enregistrements TXT relatifs à l’instance de service fournissent les éléments suivants :

Touche | type | Description
----|------|-------------
S | entier | Port sécurisé pour Device Portal. Si la valeur est égale à 0 (zéro), Device Portal ne détecte pas les connexions HTTPS.
D | chaîne | Type d’appareil. Cette information sera au format « Windows.* », par exemple Windows.Xbox ou Windows.Desktop
A | chaîne | Architecture d’appareil. Par exemple ARM, x 86 ou AMD64.  
T | liste de chaînes délimitées par des caractères Null | Balises appliquées par l’utilisateur pour l’appareil. Reportez-vous à l’API REST Tags pour apprendre à l’utiliser. La liste se termine par deux caractères Null.  

La connexion au port HTTPS est suggérée, car les appareils ne sont pas tous détectés sur le port HTTP signalé par l’enregistrement DNS-SD.

### <a name="csrf-protection-and-scripting"></a>Protection CSRF et écriture de scripts

Afin d’offrir une protection contre les [attaques CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery), un jeton unique est requis pour toutes les demandes non GET. Ce jeton, qui est l’en-tête de demande X-CSRF-Token, dérive d’un cookie de session, CSRF-Token. Dans l’interface utilisateur Device Portal, le cookie CSRF-Token est copié dans l’en-tête X-CSRF-Token à chaque demande.

> [!IMPORTANT]
> Cette protection empêche les utilisations de l’API REST à partir d’un client autonome (par exemple, les utilitaires de ligne de commande). Cette situation peut être résolue de 3 manières différentes :
> - Utilisez un nom d’utilisateur « auto- ». Les clients qui font précéder leur nom d’utilisateur du préfixe « auto-» contournent la protection CSRF. Ce nom d’utilisateur ne doit pas servir à se connecter à Device Portal par le biais du navigateur, car il rend le service vulnérable aux attaques CSRF. Exemple : Si le nom d’utilisateur du portail de l’appareil est « admin », ```curl -u auto-admin:password <args>``` doit être utilisé pour ignorer la protection de la falsification de requête Intersites.
> - Implémentez le schéma de type cookie vers en-tête dans le client. Cette opération nécessite une requête GET afin d’établir le cookie de session, puis l’inclusion de l’en-tête et du cookie sur toutes les requêtes ultérieures.
> - Désactivez l’authentification et utilisez le protocole HTTP. La protection CSRF s’applique uniquement aux points de terminaison HTTPS : les connexions au niveau des points de terminaison HTTP n’ont donc pas besoin de satisfaire les conditions ci-dessus.

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>Protection CSWSH (Cross-Site WebSocket Hijacking)

Afin d’éliminer les risques d’[attaques CSWSH](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html), tous les clients ouvrant une connexion WebSocket à Device Portal doivent également fournir un en-tête Origin correspondant à l’en-tête Host. Cela prouve à Device Portal que la requête provient soit de l’interface utilisateur de Device Portal, soit d’une application cliente valide. Si la requête ne présente pas d’en-tête Origin, elle sera rejetée.
