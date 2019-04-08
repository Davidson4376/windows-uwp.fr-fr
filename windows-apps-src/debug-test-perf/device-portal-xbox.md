---
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: Device Portal pour Xbox
description: Découvrez comment activer Device Portal pour Xbox.
ms.date: 02/12/2017
ms.topic: article
keywords: Windows 10, uwp, le portail de l’appareil
ms.localizationpriority: medium
ms.openlocfilehash: 42077756beff4269cc91624502fb9958c580bbc0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635714"
---
# <a name="device-portal-for-xbox"></a>Device Portal pour Xbox

## <a name="set-up-device-portal-on-xbox"></a>Configurer Device Portal pour Xbox

Les étapes suivantes montrent comment activer le portail d’appareil Xbox, qui vous permet d’accéder à distance à votre Xbox de développement.

1. Ouvrez l’outil Accueil du développeur, Dev Home. Cet outil s’ouvre normalement par défaut lorsque vous démarrez votre Xbox de développement, mais vous pouvez également l’ouvrir à partir de l’écran d’accueil.

    ![Device Portal DevHome](images/device-portal-xbox-1.png)

2. Dans Accueil du développeur, sur l’onglet **Accueil** de **Accès à distance**, sélectionnez **Remote Access Settings** (Paramètres d’accès à distance).

    ![Outil de gestion à distance de Device Portal](images/device-portal-xbox-15.png)

3. Cochez le paramètre **Enable Xbox Device Portal**(Activer le portail d’appareil Xbox).

4. Sous **Authentification**, sélectionnez **Set username and password** (Définir le nom d’utilisateur et mot de passe). Entrez le **nom d’utilisateur** et le **mot de passe** à utiliser pour authentifier l’accès à votre kit de développement à partir d’un navigateur, puis **enregistrez**-les.

5. **Fermez** la page **Accès à distance** et notez l’URL indiquée sous **Accès à distance** sur l’onglet **Accueil**.

6. Entrez l’URL dans votre navigateur, puis connectez-vous avec les informations d’identification que vous avez configurées.

7. Vous recevrez un avertissement concernant le certificat fourni, similaire à celui illustré ci-dessous. Dans Edge, cliquez sur **Détails**, puis sur **Go on to the webpage** (Accéder à la page Web) pour accéder au portail d’appareil Xbox. Dans la boîte de dialogue qui s’affiche, entrez le nom d’utilisateur et mot de passe que vous avez entrés précédemment sur votre Xbox.

    ![Erreur de certificat Device Portal](images/device-portal-xbox-3.png)

## <a name="device-portal-pages"></a>Pages Device Portal

Le portail d’appareil Xbox fournit un ensemble de pages standard similaires à ce qui est disponible sur le portail d’appareil Windows, ainsi que plusieurs pages uniques. Pour en obtenir une description détaillée, voir [Vue d’ensemble du portail d’appareil Windows](device-portal.md). Les sections suivantes décrivent les pages qui sont propres au portail d’appareil Xbox.

### <a name="home"></a>Accueil

Similaire à la page **Apps manager** (Gestionnaire d’applications) du portail d’appareil Windows, la page **Accueil** du portail d’appareil Xbox répertorie les applications et les jeux installés sous **Mes jeux et applications**. Vous pouvez cliquer sur le nom d’un jeu ou d’une application pour afficher plus d’informations à son sujet, comme le **nom de la famille de packages**. Dans le menu déroulant **Actions**, vous pouvez agir sur le jeu ou sur l’application, notamment en la **lançant**.

Sous **Xbox Live test accounts** (Comptes de test Xbox Live), vous pouvez gérer les comptes associés à votre Xbox. Vous pouvez ajouter des comptes utilisateurs et invités, créer des utilisateurs, connecter et déconnecter des utilisateurs, et supprimer des comptes.

![Accueil](images/device-portal-xbox-16.png)

### <a name="xbox-live-game-saves"></a>Xbox Live (Enregistrements de jeux)

Les portails d’appareil Windows et Xbox possèdent tous deux une page **Xbox Live**. Toutefois, le portail d’appareil Xbox comporte une section unique, **Xbox Live game saves** (Enregistrements de jeux Xbox Live), où vous pouvez enregistrer des données pour les jeux installés sur votre Xbox. Entrez l’**ID de configuration du service (SCID)** (voir [Configuration du service Xbox Live](../xbox-live/xbox-live-service-configuration.md#get-your-ids) pour plus d’informations), le **nom de membre (MSA)** et le **nom de la famille de packages (PFN)** associés au titre et à l’enregistrement du jeu. Ensuite, recherchez le **fichier d’entrée (.json ou .xml)**, puis sélectionnez l’un des boutons suivants (**Réinitialiser**, **Importer**, **Exporter** et **Supprimer**) pour exploiter les données enregistrées.

Dans la section **Générer**, vous pouvez générer des données factices et les enregistrer dans le fichier d’entrée spécifié. Entrez simplement les **conteneurs (par défaut 2)**, les **objets BLOB (par défaut 3)** et la **taille de l’objet BLOB (par défaut 1024)**, puis sélectionnez **Générer**.

![Xbox Live](images/device-portal-xbox-17.png)

### <a name="http-monitor"></a>Moniteur HTTP

Le moniteur HTTP permet de consulter le trafic HTTP et HTTPS déchiffré provenant de votre application ou de votre jeu lorsqu’il s’exécute sur votre Xbox One.

![Moniteur HTTP](images/device-portal-xbox-18.png)

Pour l’activer, ouvrez l’outil d’accueil du développeur sur votre Xbox One, accédez à l’onglet **Paramètres** et, dans la zone **HTTP Monitor Settings** (Paramètres du moniteur HTTP), cochez l’option (Activer le moniteur HTTP)**.**

![Page d’accueil dev : Mise en réseau](images/device-portal-xbox-14.png)

Une fois l’activation effectuée, dans le portail d’appareil Xbox, vous pouvez **arrêter**, **effacer** et **enregistrer dans un fichier** le trafic HTTP et HTTPS en sélectionnant les boutons respectifs.

### <a name="network-fiddler-tracing"></a>Réseau (Suivi de Fiddler)

La page **Network** (Réseau) du portail d’appareil Xbox est presque identique à la page **Réseaux** du portail d’appareil Windows à l’exception du **suivi de Fiddler**, qui est propre au portail d’appareil Xbox. Vous pouvez ainsi exécuter Fiddler sur votre PC pour consigner et inspecter le trafic HTTP et HTTPS entre votre Xbox One et Internet. Pour plus d’informations, voir [Utilisation de Fiddler avec Xbox One lors du développement pour UWP](../xbox-apps/uwp-fiddler.md).

![Network (Réseau)](images/device-portal-xbox-19.png)

### <a name="media-capture"></a>Capture multimédia

Sur la page **Capture multimédia**, vous pouvez sélectionner **Capture Screenshot** (Capturer l’écran) pour prendre une capture d’écran de votre Xbox One. Votre navigateur vous invite alors à télécharger le fichier. Vous pouvez cocher l’option **Prefer HDR** (Mode HDR) pour effectuer la capture d’écran en mode HDR (si la console le prend en charge).

![Capture multimédia](images/device-portal-xbox-12.png)

### <a name="settings"></a>Paramètres

Sur la page **Paramètres**, vous pouvez afficher et modifier plusieurs paramètres de votre Xbox One. En haut, vous pouvez sélectionner **Importer** pour importer les paramètres à partir d’un fichier et **Exporter** pour exporter les paramètres actuels dans un fichier .txt. L’importation des paramètres facilite les modifications en bloc, en particulier lors de la configuration de plusieurs consoles. Pour créer un fichier de paramètres à importer, modifiez les paramètres à votre convenance, puis exportez-les. Vous pouvez ensuite utiliser ce fichier pour importer les paramètres rapidement et facilement pour les autres consoles.

Plusieurs sections contenant des paramètres d’affichage et/ou de modification différents sont présentées ci-dessous.

![Paramètres](images/device-portal-xbox-20.png)

![Paramètres](images/device-portal-xbox-21.png)

#### <a name="device-information"></a>Informations sur l’appareil

* **Nom de l’appareil**: Le nom de l’appareil. Pour le changer, modifiez le nom figurant dans la zone, puis sélectionnez **Enregistrer**.

* **Version du système d’exploitation**: En lecture seule. Numéro de version du système d’exploitation.

* **Édition du système d’exploitation**: En lecture seule. Nom de la principale version du système d’exploitation.

* **ID d’appareil Xbox Live**: En lecture seule.

* **ID de la console**: En lecture seule.

* **Numéro de série**: En lecture seule.

* **Type de la console**: En lecture seule. Type d’appareil Xbox One (Xbox One, Xbox One S ou Xbox One X).

* **Mode de développement**: En lecture seule. Mode de développement dans lequel se trouve l’appareil.

#### <a name="audio-settings"></a>Paramètres audio

* **Format de flux binaire audio**: Le format des données audio.

* **Audio HDMI**: Le type de signal audio via le port HDMI.

* **Format de casque**: Le format de l’audio est fourni via un casque.

* **Audio optique**: Le type de données audio via le port optique.

* **Utiliser le casque audio optique ou HDMI**: Cochez cette case si vous utilisez un casque connecté via HDMI ou optique.

#### <a name="display-settings"></a>Paramètres d'affichage

* **Profondeur de couleur**: Le nombre de bits utilisés pour chaque composant de couleur d’un pixel unique.

* **Espace de couleurs**: La gamme de couleurs disponible pour l’affichage.

* **Résolution d’affichage**: La résolution de l’affichage.

* **Afficher la connexion**: Le type de connexion à l’affichage.

* **Autoriser la plage dynamique (HDR)**: Active HDR sur l’affichage. Disponible uniquement sur les écrans compatibles.

* **Autoriser les 4K**: Permet la résolution 4K sur l’affichage. Disponible uniquement sur les écrans compatibles.

* **Autoriser la fréquence d’actualisation de Variable (VRR)**: Activer VRR sur l’affichage. Disponible uniquement sur les écrans compatibles.

#### <a name="kinect-settings"></a>Paramètres Kinect

Un capteur Kinect doit être connecté à la console afin de modifier ces paramètres.

* **Activer Kinect**: Activer le capteur Kinect attaché.

* **Force Kinect recharger sur la modification de l’application**: Rechargez le capteur Kinect attaché à chaque exécution d’une autre application ou un jeu.

#### <a name="localization-settings"></a>Paramètres de localisation

* **Région géographique**: La région géographique que le périphérique est défini sur. Ce paramètre doit correspondre au code du pays sur 2 caractères (par exemple, **FR** pour la France).

* **Par défaut ou les langues**: La langue a la valeur de l’appareil.

* **Fuseau horaire**: Le périphérique est défini sur le fuseau horaire.

#### <a name="network-settings"></a>Paramètres réseau

* **Paramètres de la radio sans fil**: Les paramètres sans fil de l’appareil (si certains aspects tels que les réseaux locaux sans fil sont activées ou désactivées).

#### <a name="power-settings"></a>Paramètres d’alimentation

* **Cas d’inactivité, dim écran après (minutes)**: L’écran s’estompe après que l’appareil a été inactif pendant ce laps de temps. Définissez ce paramètre sur **0** pour ne jamais assombrir l’écran.

* **Cas d’inactivité, désactiver après**: L’appareil s’arrête après que qu’elle a été inactif pendant ce laps de temps.

* **Mode d’alimentation**: Le mode d’alimentation de l’appareil. Pour plus d’informations, voir [À propos des modes d’alimentation Économie d’énergie et Démarrage instantané](https://support.xbox.com/xbox-one/console/learn-about-power-modes).

* **Console automatiquement démarrage lorsqu’elle est sous tension**: L’appareil redémarre automatiquement lorsqu’il est connecté à une source d’alimentation.

#### <a name="preference-settings"></a>Paramètres de préférence

* **Par défaut foyer**: Définit l’écran d’accueil s’affiche lorsque l’appareil est allumé.

* **Autoriser les connexions à partir de l’application Xbox**: L’application Xbox sur un autre appareil (par exemple, un PC Windows 10) peut se connecter à cette console.

* **Traiter les applications UWP en tant que jeux par défaut**: Jeux et les applications obtient différentes ressources qui leur est allouées sur Xbox. Si vous cochez cette case, tous les packages UWP seront identifiés comme des jeux et obtiendront ainsi davantage de ressources.

#### <a name="user-settings"></a>Paramètres utilisateur

* **Auto de connexion utilisateur**: Se connecte automatiquement l’utilisateur sélectionné lors de l’appareil est allumé.

* **Auto de contrôleur de l’utilisateur de connexion**: Associe automatiquement un type de contrôleur spécifique à un utilisateur particulier.

#### <a name="xbox-live-sandbox"></a>Bac à sable Xbox Live

Vous pouvez modifier ici le bac à sable Xbox Live dans lequel se trouve l’appareil. Entrez le nom du bac à sable dans la zone, puis sélectionnez **Modifier**.

### <a name="scratch"></a>Vide

Espace de travail vide que vous pouvez personnaliser à votre convenance. Vous pouvez utiliser le menu (cliquez sur le bouton de menu en haut à gauche) pour ajouter des outils. Sélectionnez alors **Ajouter les outils à l’espace de travail**, puis les outils que vous souhaitez ajouter, et cliquez enfin sur **Ajouter**). Notez que vous pouvez utiliser ce menu pour ajouter des outils à n’importe quel espace de travail, ainsi que pour gérer les espaces de travail eux-mêmes.

![Ajouter des outils à l’espace de travail](images/device-portal-xbox-13.png)

### <a name="game-event-data"></a>Données d’événement de jeu

Sur le **jeux de données d’événement** , vous pouvez afficher un graphique en temps réel qui diffuse en continu dans le nombre d’événements de jeu de suivi d’événements pour Windows (ETW) actuellement enregistrés sur votre Xbox One. S’il existe des événements de jeu enregistrées sur le système, vous pouvez également afficher les détails (nom de l’événement occurrence d’événement et le titre du jeu) décrivant chaque événement dans une table de données sous le graphique de données. La table est uniquement disponible s’il existe des événements enregistrés.

![Données d’événement de jeu](images/device-portal-xbox-22.PNG)

## <a name="see-also"></a>Voir également

* [Vue d’ensemble de Windows Device Portal](device-portal.md)
* [Core de portail appareil référence de l’API](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)