---
author: PatrickFarley
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: Device Portal pour Xbox
description: Découvrez comment activer Device Portal pour XboxOne.
ms.author: pafarley
ms.date: 02/12/2017
ms.topic: article
keywords: Windows 10, uwp, le portail d’appareil
ms.localizationpriority: medium
ms.openlocfilehash: 7182f67a7bffc303b9c3067d8b746f1b4f27b809
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6147333"
---
# <a name="device-portal-for-xbox"></a>Device Portal pour Xbox

## <a name="set-up-device-portal-on-xbox"></a>Configurer Device Portal pour Xbox

Les étapes suivantes montrent comment activer le portail d’appareilXbox, qui vous permet d’accéder à distance à votre Xbox de développement.

1. Ouvrez l’outil Accueil du développeur, DevHome. Cet outil s’ouvre normalement par défaut lorsque vous démarrez votre Xbox de développement, mais vous pouvez également l’ouvrir à partir de l’écran d’accueil.

    ![Device Portal DevHome](images/device-portal-xbox-1.png)

2. Dans Accueil du développeur, sur l’onglet **Accueil** de **Accès à distance**, sélectionnez **Remote Access Settings** (Paramètres d’accès à distance).

    ![Outil de gestion à distance de DevicePortal](images/device-portal-xbox-15.png)

3. Cochez le paramètre **Enable Xbox Device Portal**(Activer le portail d’appareilXbox).

4. Sous **Authentification**, sélectionnez **Set username and password** (Définir le nom d’utilisateur et mot de passe). Entrez le **nom d’utilisateur** et le **mot de passe** à utiliser pour authentifier l’accès à votre kit de développement à partir d’un navigateur, puis **enregistrez**-les.

5. **Fermez** la page **Accès à distance** et notez l’URL indiquée sous **Accès à distance** sur l’onglet **Accueil**.

6. Entrez l’URL dans votre navigateur, puis connectez-vous avec les identifiants que vous avez configurés.

7. Vous recevrez un avertissement concernant le certificat fourni, similaire à celui illustré ci-dessous. Dans Edge, cliquez sur **Détails**, puis sur **Go on to the webpage** (Accéder à la page Web) pour accéder au portail d’appareilXbox. Dans la boîte de dialogue qui s’affiche, entrez le nom d’utilisateur et mot de passe que vous avez entrés précédemment sur votre Xbox.

    ![Erreur de certificat Device Portal](images/device-portal-xbox-3.png)

## <a name="device-portal-pages"></a>Pages DevicePortal

Le portail d’appareilXbox fournit un ensemble de pages standard similaires à ce qui est disponible sur le portail d’appareilWindows, ainsi que plusieurs pages uniques. Pour en obtenir une description détaillée, voir [Vue d’ensemble du portail d’appareilWindows](device-portal.md). Les sections suivantes décrivent les pages qui sont propres au portail d’appareilXbox.

### <a name="home"></a>Accueil

Similaire à la page **Apps manager** (Gestionnaire d’applications) du portail d’appareilWindows, la page **Accueil** du portail d’appareilXbox répertorie les applications et les jeux installés sous **Mes jeux et applications**. Vous pouvez cliquer sur le nom d’un jeu ou d’une application pour afficher plus d’informations à son sujet, comme le **nom de la famille de packages**. Dans le menu déroulant **Actions**, vous pouvez agir sur le jeu ou sur l’application, notamment en la **lançant**.

Sous **Xbox Live test accounts** (Comptes de test XboxLive), vous pouvez gérer les comptes associés à votre Xbox. Vous pouvez ajouter des comptes utilisateurs et invités, créer des utilisateurs, connecter et déconnecter des utilisateurs, et supprimer des comptes.

![Accueil](images/device-portal-xbox-16.png)

### <a name="xbox-live-game-saves"></a>Xbox Live (Enregistrements de jeux)

Les portails d’appareil Windowset Xbox possèdent tous deux une page **XboxLive**. Toutefois, le portail d’appareilXbox comporte une section unique, **Xbox Live game saves** (Enregistrements de jeux XboxLive), où vous pouvez enregistrer des données pour les jeux installés sur votre Xbox. Entrez l’**ID de configuration du service (SCID)** (voir [Configuration du service XboxLive](../xbox-live/xbox-live-service-configuration.md#get-your-ids) pour plus d’informations), le **nom de membre (MSA)** et le **nom de la famille de packages(PFN) **associés au titre et à l’enregistrement du jeu. Ensuite, recherchez le **fichier d’entrée (.json ou .xml)**, puis sélectionnez l’un des boutons suivants (**Réinitialiser**, **Importer**, **Exporter** et **Supprimer**) pour exploiter les données enregistrées.

Dans la section **Générer**, vous pouvez générer des données factices et les enregistrer dans le fichier d’entrée spécifié. Entrez simplement les **conteneurs (par défaut2)**, les **objets BLOB (par défaut3)** et la **taille de l’objet BLOB (par défaut1024)**, puis sélectionnez **Générer**.

![XboxLive](images/device-portal-xbox-17.png)

### <a name="http-monitor"></a>MoniteurHTTP

Le moniteurHTTP permet de consulter le trafic HTTP et HTTPS déchiffré provenant de votre application ou de votre jeu lorsqu’il s’exécute sur votre XboxOne.

![MoniteurHTTP](images/device-portal-xbox-18.png)

Pour l’activer, ouvrez l’outil d’accueil du développeur sur votre XboxOne, accédez à l’onglet **Paramètres** et, dans la zone **HTTP Monitor Settings** (Paramètres du moniteurHTTP), cochez l’option **** (Activer le moniteurHTTP).

![Accueil du développeur: Réseaux](images/device-portal-xbox-14.png)

Une fois l’activation effectuée, dans le portail d’appareilXbox, vous pouvez **arrêter**, **effacer** et **enregistrer dans un fichier** le trafic HTTP et HTTPS en sélectionnant les boutons respectifs.

### <a name="network-fiddler-tracing"></a>Réseau (Suivi de Fiddler)

La page **Network** (Réseau) du portail d’appareilXbox est presque identique à la page **Réseaux** du portail d’appareilWindows à l’exception du **suivi de Fiddler**, qui est propre au portail d’appareilXbox. Vous pouvez ainsi exécuter Fiddler sur votre PC pour consigner et inspecter le trafic HTTP et HTTPS entre votre XboxOne et Internet. Pour plus d’informations, voir [Utilisation de Fiddler avec XboxOne lors du développement pour UWP](../xbox-apps/uwp-fiddler.md).

![Network (Réseau)](images/device-portal-xbox-19.png)

### <a name="media-capture"></a>Capture multimédia

Sur la page **Capture multimédia**, vous pouvez sélectionner **Capture Screenshot** (Capturer l’écran) pour prendre une capture d’écran de votre XboxOne. Votre navigateur vous invite alors à télécharger le fichier. Vous pouvez cocher l’option **Prefer HDR** (ModeHDR) pour effectuer la capture d’écran en modeHDR (si la console le prend en charge).

![Capture multimédia](images/device-portal-xbox-12.png)

### <a name="settings"></a>Paramètres

Sur la page **Paramètres**, vous pouvez afficher et modifier plusieurs paramètres de votre XboxOne. En haut, vous pouvez sélectionner **Importer** pour importer les paramètres à partir d’un fichier et **Exporter** pour exporter les paramètres actuels dans un fichier.txt. L’importation des paramètres facilite les modifications en bloc, en particulier lors de la configuration de plusieurs consoles. Pour créer un fichier de paramètres à importer, modifiez les paramètres à votre convenance, puis exportez-les. Vous pouvez ensuite utiliser ce fichier pour importer les paramètres rapidement et facilement pour les autres consoles.

Plusieurs sections contenant des paramètres d’affichage et/ou de modification différents sont présentées ci-dessous.

![Paramètres](images/device-portal-xbox-20.png)

![Paramètres](images/device-portal-xbox-21.png)

#### <a name="device-information"></a>Informations sur l’appareil

* **Nom de l’appareil**: nom de l’appareil. Pour le changer, modifiez le nom figurant dans la zone, puis sélectionnez **Enregistrer**.

* **Version du système d’exploitation**: en lecture seule. Numéro de version du système d’exploitation.

* **Édition du système d’exploitation**: en lecture seule. Nom de la principale version du système d’exploitation.

* **ID de l’appareil XboxLive**: en lecture seule.

* **ID de la console**: en lecture seule.

* **Numéro de série**: en lecture seule.

* **ID de la console**: en lecture seule. Type d’appareil XboxOne (XboxOne, XboxOneS ou XboxOneX).

* **Dev Mode**: en lecture seule. Mode de développement dans lequel se trouve l’appareil.

#### <a name="audio-settings"></a>Paramètres audio

* **Audio bitstream format** (Format audio compressé): format des données audio.

* **AudioHDMI**: type de données audio transitant par le portHDMI.

* **Headset format** (Format du casque): le format de l’audio est fourni par le biais d’un casque.

* **Optique audio**: type de données audio transitant par le portoptique.

* **Use HDMI or optical audio headset** (Utiliser HDMI ou le casque audio optique): cochez cette case si vous utilisez un casque connecté via HDMI ou via une connexion optique.

#### <a name="display-settings"></a>Paramètres d’affichage

* **Profondeur de couleurs**: nombre de bits utilisés pour chaque composant de couleur d’un même pixel.

* **Espace de couleur**: gamme de couleurs disponible à l’affichage.

* **Résolution d’affichage**: résolution de l’affichage.

* **Display connection** (Connexion de l’écran): type de connexion de l’écran.

* **Allow high dynamic range (HDR)** (Autoriser la plage dynamique étendue): active le modeHDR à l’écran. Disponible uniquement sur les écrans compatibles.

* **Autoriser la 4K**: active la résolution 4Ko à l’écran. Disponible uniquement sur les écrans compatibles.

* **Allow Variable Refresh Rate (VRR)** (Autoriser la fréquence de rafraîchissement variable): active la VRR à l’écran. Disponible uniquement sur les écrans compatibles.

#### <a name="kinect-settings"></a>Paramètres Kinect

Un capteur Kinect doit être connecté à la console afin de modifier ces paramètres.

* **Enable Kinect** (Activer la Kinect): active le capteur Kinect joint.

* **Force Kinect reload on app change** (Forcer le rechargement de Kinect en cas de modification de l’application): recharge le capteur Kinect joint chaque fois qu’une application ou qu’un jeu est exécuté.

#### <a name="localization-settings"></a>Paramètres de localisation

* **Geographic region** (Région géographique): zone géographique définie sur l’appareil. Ce paramètre doit correspondre au code du pays sur 2caractères (par exemple, **FR** pour la France).

* **Preferred language(s)** (Langue(s) préférée(s)): langue définie sur l’appareil.

* **Fuseau horaire**: fuseau horaire défini sur l’appareil.

#### <a name="network-settings"></a>Paramètres réseau

* **Wireless radio settings** (Paramètres radio sans fil): paramètres sans fil de l’appareil (activation ou non de certains aspects comme un réseau local sans fil).

#### <a name="power-settings"></a>Paramètres d’alimentation

* **When idle, dim screen after (minutes)** (En cas d’inactivité, assombrir l’écran après(minutes)): assombrit l’écran après le temps d’inactivité défini pour l’appareil. Définissez ce paramètre sur **0** pour ne jamais assombrir l’écran.

* **When idle, turn off after** (En cas d’inactivité, éteindre après(minutes)): éteint l’appareil après le délai d’inactivité défini.

* **Mode d’alimentation**: mode d’alimentation de l’appareil. Pour plus d’informations, voir [À propos des modes d’alimentation Économie d’énergie et Démarrage instantané](http://support.xbox.com/xbox-one/console/learn-about-power-modes).

* **Automatically boot console when connected to power** (Démarrer automatiquement la console en cas de branchement à l’alimentation): l’appareil s’allume automatiquement lorsqu’il est connecté à une source d’alimentation.

#### <a name="preference-settings"></a>Paramètres de préférence

* **Default home experience** (Expérience domestique par défaut): définit l’écran d’accueil qui s’affiche lorsque l’appareil est allumé.

* **Allow connections from the Xbox app** (Autoriser les connexions à partir de l’application Xbox): l’applicationXbox installée sur un autre appareil (par exemple, un PC Windows10) peut se connecter à cette console.

* **Treat UWP apps as games by default** (Traiter les applicationsUWP comme des jeux par défaut): les jeux et les applications sont associés à des ressources différentes sur la Xbox. Si vous cochez cette case, tous les packagesUWP seront identifiés comme des jeux et obtiendront ainsi davantage de ressources.

#### <a name="user-settings"></a>Paramètres utilisateur

* **Auto sign in user** (Connexion automatique de l’utilisateur): connecte automatiquement l’utilisateur sélectionné lorsque l’appareil est sous tension.

* **Auto sign in user controller** (Connexion automatique du contrôleur de l’utilisateur): associe automatiquement un type de contrôleur donné à un utilisateur donné.

#### <a name="xbox-live-sandbox"></a>Bac à sable XboxLive

Vous pouvez modifier ici le bac à sableXboxLive dans lequel se trouve l’appareil. Entrez le nom du bac à sable dans la zone, puis sélectionnez **Modifier**.

### <a name="scratch"></a>Vide

Espace de travail vide que vous pouvez personnaliser à votre convenance. Vous pouvez utiliser le menu (cliquez sur le bouton de menu en haut à gauche) pour ajouter des outils. Sélectionnez alors **Ajouter les outils à l’espace de travail**, puis les outils que vous souhaitez ajouter, et cliquez enfin sur **Ajouter**). Notez que vous pouvez utiliser ce menu pour ajouter des outils à n’importe quel espace de travail, ainsi que pour gérer les espaces de travail eux-mêmes.

![Ajouter des outils à l’espace de travail](images/device-portal-xbox-13.png)

### <a name="game-event-data"></a>Données d’événement de jeu

Sur la page de **données d’événement de jeu** , vous pouvez afficher un graphique en temps réel à ce flux dans le nombre d’événements de jeu de suivi d’événements pour Windows (ETW) actuellement enregistrée sur votre console Xbox One. S’il existe des événements du jeu enregistrés sur le système, vous pouvez également afficher les détails (nom de l’événement, occurrence de l’événement et le titre de jeu) qui décrit chaque événement dans une table de données sous le graphique de données. Le tableau est uniquement disponible si l’événement n’est enregistré.

![Données d’événement de jeu](images/device-portal-xbox-22.PNG)

## <a name="see-also"></a>Voir aussi

* [Vue d’ensemble du portail d’appareil Windows](device-portal.md)
* [Informations de référence sur les API principales du portail d’appareilWindows](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)