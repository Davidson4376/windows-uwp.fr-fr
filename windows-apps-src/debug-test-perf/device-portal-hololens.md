---
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: Device Portal pour HoloLens
description: Découvrez comment Windows Device Portal pour HoloLens vous permet de configurer et de gérer à distance votre appareil HoloLens.
ms.date: 01/03/2019
ms.topic: article
keywords: Windows 10, uwp, le portail de l’appareil
ms.localizationpriority: medium
ms.openlocfilehash: 92f4cc44f93348e1dba0f45b780e2efdda6949a7
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67713889"
---
# <a name="device-portal-for-hololens"></a>Device Portal pour HoloLens


## <a name="set-up-device-portal-on-hololens"></a>Configurer Device Portal sur HoloLens

### <a name="enable-device-portal"></a>Activer Device Portal

1. Mettez HoloLens sous tension et allumez l’appareil.
2. Effectuer le geste qui consiste à [maintenir sa paume de main vers le haut en serrant ses doigts et à les ouvrir](https://developer.microsoft.com/mixed-reality#Bloom) pour lancer le menu principal.
3. Regardez la vignette **Paramètres** et effectuez le geste consistant à [appuyer](https://developer.microsoft.com/mixed-reality#Press_and_release). Appuyez de nouveau pour placer l’application dans votre environnement. L’application Paramètres démarre une fois placée.
4. Sélectionnez l’élément de menu **Mettre à jour**.
5. Sélectionnez l’élément de menu **Pour les développeurs**.
6. Activez **Mode développeur**.
7. [Faites défiler](https://developer.microsoft.com/mixed-reality#Navigation) la liste et activez Device Portal.


### <a name="pair-your-device"></a>Jumeler votre appareil

#### <a name="connect-over-wi-fi"></a>Se connecter via Wi-Fi 

1. Connectez votre HoloLens au Wi-Fi.
2. Rechercher l’adresse IP de votre périphérique. Rechercher l’adresse IP sur l’appareil sous **Paramètres > réseau & Internet > Wi-Fi > Propriétés de matériel**. Vous pouvez également demander : « Hey Cortana, quelle est mon adresse IP ? »

3. À partir d’un navigateur web sur votre PC, accédez à `https://<YOUR_HOLOLENS_IP_ADDRESS>`
    - Le navigateur affiche le message suivant : « Il est un problème avec le certificat de sécurité de ce site Web ». Cela se produit car le certificat envoyé à Device Portal est un certificat de test. Vous pouvez ignorer cette erreur de certificat pour le moment et continuer.

#### <a name="connect-over-usb"></a>Se connecter via USB 

1. Installez les outils pour vous assurer que Visual Studio Update 1 et les outils de développement Windows 10 sont installés sur votre PC. Cela permet d’activer la connectivité USB.
2. Connectez votre HoloLens au PC à l’aide d’un câble micro-USB.
3. À partir d’un navigateur web sur votre PC, accédez à `http://127.0.0.1:10080`.

> [!IMPORTANT]
> Si votre PC est impossible de trouver l’appareil, essayez d’utiliser l’adresse IP de réseau réelle de l’appareil HoloLens, plutôt que `http://127.0.0.1:10080`.

#### <a name="connect-to-an-emulator"></a>Se connecter à un émulateur 

Vous pouvez également utiliser Device Portal avec votre émulateur. Pour vous connecter à Device Portal, utilisez la barre d’outils. Cliquez sur cette icône :
- Ouvrez le portail de l’appareil : ouvre le Portail d’appareil Windows pour le système d’exploitation HoloLens dans l’émulateur.

#### <a name="create-a-username-and-password"></a>Créer un nom d’utilisateur et un mot de passe 

Vous devrez créer un nom d’utilisateur et un mot de passe sur Device Portal de votre HoloLens lors de votre première connexion.
1. Dans un navigateur web sur votre PC, entrez l’adresse IP de l’HoloLens. La page d’accès à la configuration s’affiche.
2. Cliquez ou appuyez sur Demander un code confidentiel et regardez l’écran HoLolens pour obtenir le code confidentiel généré.
3. Entrez le code confidentiel dans la zone de texte prévue à cet effet de votre appareil.
4. Entrez le nom d’utilisateur que vous utiliserez pour vous connecter à Device Portal. Il n’est pas nécessaire qu’il s’agisse d’un nom de compte Microsoft (MSA) ou de domaine.
5. Entrez un mot de passe et confirmez-le. Le mot de passe doit comporter au moins sept caractères. Il n’est pas nécessaire qu’il s’agisse d’un mot de passe de compte Microsoft (MSA) ou de domaine.
6. Cliquez sur Coupler pour se connecter à Windows Device Portal sur le HoloLens.

Si vous souhaitez modifier ce nom d’utilisateur ou ce mot de passe, vous pouvez à tout moment répéter ce processus en accédant à la page de sécurité de l’appareil en cliquant sur le lien Sécurité situé en haut à droite ou en accédant à : `https://<YOUR_HOLOLENS_IP_ADDRESS>/devicesecurity.htm`.

#### <a name="security-certificate"></a>Certificat de sécurité 

Si une « erreur de certificat » s’affiche dans votre navigateur, vous pouvez la résoudre en créant une relation d’approbation avec l’appareil.

Chaque HoloLens génère un certificat auto-signé unique pour sa connexion SSL. Par défaut, ce certificat n’est pas approuvé par le navigateur web de votre PC, c’est la raison pour laquelle vous obtiendrez peut-être une « erreur de certificat ». En téléchargeant ce certificat à partir de votre HoloLens (via une connexion USB ou Wi-Fi approuvée) et en l’approuvant sur votre PC, vous pouvez vous connecter en toute sécurité à votre appareil.
1. Vérifiez que vous vous trouvez sur un réseau sécurisé (USB ou réseau Wi-Fi approuvé).
2. Télécharger les certificats de ce périphérique à partir de la page « Sécurité » sur le portail de l’appareil. - soit lier à partir de la liste supérieure droite des icônes de la sécurité ou accédez à, cliquez sur : `https://<YOUR_HOLOLENS_IP_ADDRESS>/devicesecurity.htm`

3. Installer le certificat dans le « Trusted Root Certification Authorities » stocker sur votre PC. - dans le menu Windows, tapez : Gérer les certificats d’ordinateur et démarrer l’application.
    - Développez le dossier Autorité de certification racine de confiance.
    - Cliquez sur le dossier Certificats.
    - Dans le menu Action, sélectionnez : Toutes les tâches > Importer...
    - Terminez l’Assistant Importation de certificat en utilisant le fichier de certificat que vous avez téléchargé à partir de Device Portal.

4. Redémarrez le navigateur.


## <a name="device-portal-pages"></a>Pages de Device Portal 

### <a name="home"></a>Dossier de base 

Votre session Device Portal démarre sur la page d’accueil. Accédez à d’autres pages à partir de la barre de navigation située sur le côté gauche de la page d’accueil.

La barre d’outils se trouvant en haut de la page permet d’accéder aux fonctionnalités et aux états couramment utilisés.
- **En ligne**: Indique si l’appareil est connecté au réseau Wi-Fi.
- **Arrêt**: Désactive l’appareil.
- **Redémarrez**: Cycles mise sous tension l’appareil.
- **Sécurité** : Ouvre la page sécurité de l’appareil.
- **Froid**: Indique la température de l’appareil.
- **MISE SOUS TENSION**: Indique si l’appareil est branché et de charge.
- **Aide**: Ouvre la page de documentation d’interface REST.

La page d’accueil affiche les informations suivantes :
- État de l’**appareil** : surveille l’état de votre appareil et signale les erreurs critiques.
- **Informations Windows** : affiche le nom du casque HoloLens et de la version de Windows installée actuellement.
- La section **Préférences** comprend les paramètres suivants :
    - **IPD**: Définit la distance interpupillary (IPD), qui est la distance, en millimètres, entre le centre d’élèves de l’utilisateur lors de la recherche directement à l’avance. Le paramètre prend immédiatement effet. La valeur par défaut a été calculée automatiquement lors de la configuration de votre appareil.
    - **Nom de l’appareil**: Attribuez un nom pour le HoloLens. Vous devez redémarrer l’appareil après avoir modifié cette valeur afin qu’elle soit prise en compte. Après avoir cliqué sur Enregistrer, une boîte de dialogue vous demande si vous voulez redémarrer l’appareil immédiatement ou ultérieurement.
    - **Paramètres de mise en veille**: Définit la longueur du délai d’attente avant que l’appareil se met en veille lorsqu’il est branché et lorsqu’il est sur batterie.

### <a name="3d-view"></a>Vue 3D 

Utilisez la page Vue 3D pour voir comment HoloLens interprète votre environnement. Naviguez dans la vue à l’aide de la souris :
- **Pivoter** : clic gauche + souris ;
- **Mouvement panoramique** : clic droit + souris ;
- **Zoom** : défilement à l’aide de la souris.
- **Options de suivi des**: Allumez le suivi visuel continu en vérifiant le suivi visuel de Force. La mise en pause arrête le suivi visuel.
- **Afficher les options**: Définir les options sur l’affichage 3D : - suivi : Indique si le suivi visuel est actif.
- **Afficher le plancher**: Affiche un plan d’étage carreaux.
- **Afficher frustum**: Affiche le frustum de vue.
- **Afficher le plan de stabilisation**: Affiche le plan HoloLens utilise pour la stabilisation du mouvement.
- **Afficher le maillage**: Affiche le maillage de surface de mappage qui représente votre environnement.
- **Afficher les détails**: Affiche hand positions, les quaternions rotation principal et le vecteur d’origine appareil mesure qu’elles changent en temps réel.
- **Bouton plein écran**: Affiche la vue 3D en mode plein écran. Appuyez sur la touche ÉCHAP pour quitter le mode plein écran.

- Reconstruction de l’aire de conception : Cliquez ou appuyez sur la mise à jour pour afficher le maillage de mappage spatial plus récente à partir de l’appareil. Un passage complet peut nécessiter un certain temps, pouvant aller jusqu’à quelques secondes. Le maillage ne se met pas à jour automatiquement dans la vue 3D. Vous devez cliquer sur Mettre à jour pour obtenir le tout dernier maillage à partir de l’appareil. Cliquez sur Enregistrer pour enregistrer le maillage de mappage spatial actuel en tant que fichier obj sur votre PC.

### <a name="mixed-reality-capture"></a>MRC (Mixed Reality Capture) 

Utilisez la page MRC pour enregistrer les flux multimédias issus du casque HoloLens.
- Paramètres : Contrôler les flux multimédias qui sont capturés en vérifiant les paramètres suivants :-Vntana : Capture le contenu HOLOGRAPHIQUE dans le flux vidéo. Les hologrammes font l’objet d’un rendu en mono et non en stéréo.
- **Appareil photo PV**: Capture le flux vidéo à partir de la caméra vidéo/photo.
- **MIC Audio**: Capture audio à partir du tableau de microphone.
- **Application Audio**: Capture des données audio à partir de l’application en cours d’exécution.
- **Qualité de la version préliminaire de Live**: Sélectionnez la résolution d’écran, la fréquence d’images et le taux de diffusion en continu de l’aperçu en direct.

- Cliquez ou appuyez sur le bouton d’aperçu instantané pour afficher le flux de capture. Arrêter l’aperçu instantané arrête le flux de capture.
- Cliquez ou appuyez sur Enregistrer pour commencer à enregistrer le flux MRC à l’aide des paramètres spécifiés. Arrêter l’enregistrement arrête l’enregistrement et l’enregistre.
- Cliquez ou appuyez sur Prendre une photo pour prendre une image fixe à partir du flux de capture.
- **Photos et vidéos**: Affiche une liste de captures vidéo et photo effectuée sur l’appareil.

Notez que les applications HoloLens ne sont pas en mesure de capturer de photo ou de vidéo MRC en cours d’enregistrement ou de diffusion en continu d’un aperçu instantané à partir de Device Portal.

### <a name="system-performance"></a>Performances du système 

L’outil Performances du système sur HoloLens propose trois mesures supplémentaires qu’il est possible d’enregistrer. 

Voici les mesures disponibles :
- **SoC power**: Consommation d’énergie de système sur puce instantanée, moyenne par minute
- **Alimentation du système**: Consommation d’énergie système instantanée, moyenne par minute
- **Fréquence d’images**: Images par seconde, manqué VBlanks par seconde et consécutifs VBlanks manquée

### <a name="app-crash-dumps-page"></a>Page relative aux vidages sur incident des applications 

Cette page vous permet de recueillir les vidages sur incident de vos applications chargées de manière indépendante. Cochez la case Activation des vidages sur incident pour chaque application pour laquelle vous souhaitez recueillir des vidages sur incident. Revenez à cette page pour recueillir les vidages sur incident. Les fichiers de vidage peuvent être ouverts dans Visual Studio pour le débogage.

### <a name="kiosk-mode"></a>Mode plein écran 

Active le mode plein écran, qui limite la capacité de l’utilisateur à lancer de nouvelles applications ou à modifier l’application en cours d’exécution. Lorsque le mode plein écran est activé, le mouvement correspondant à une paume de main qui s’ouvre et Cortana sont désactivés et les applications placées ne sont pas affichées dans l’environnement de l’utilisateur.

Cochez la case Activer le mode plein écran pour placer le casque HoloLens en mode plein écran. Sélectionnez l’application dans la liste déroulante Application de démarrage. Cliquez ou appuyez sur Enregistrer pour valider les paramètres.

Notez que l’application s’exécute au démarrage même si le mode plein écran n’est pas activé. Sélectionnez Aucune pour qu’aucune application ne s’exécute au démarrage.

### <a name="simulation"></a>Simulation 

Vous permet d’enregistrer et de lire des données d’entrée pour le test.
- **Capturer la salle**: Utilisé pour télécharger un fichier de pièce simulé qui contient la maille de mappage spatial pour l’environnement de l’utilisateur. Nommez la pièce, puis cliquez sur Capturer pour enregistrer les données sous forme de fichier .xef sur votre PC. Ce fichier de pièce peut être chargé dans l’émulateur HoloLens.
- **L’enregistrement**: Vérifier les flux de données pour enregistrer, le nom de l’enregistrement, puis cliquez ou appuyez sur l’enregistrement pour commencer l’enregistrement. Effectuer des actions avec votre HoloLens, puis cliquez sur Arrêter pour enregistrer les données sous forme de fichier .xef sur votre PC. Ce fichier peut être chargé dans l’émulateur ou l’appareil HoloLens.
- **Lecture**: Cliquez ou appuyez sur l’enregistrement pour sélectionner un fichier xef à partir de votre PC et d’envoyer les données à la HoloLens de téléchargement.
- **Mode de contrôle**: Sélectionnez par défaut ou Simulation dans la liste déroulante, puis cliquez ou appuyez sur le bouton Définir pour sélectionner le mode sur le HoloLens. Choisissez Simulation pour désactiver les capteurs réels de votre casque HoloLens et utiliser les données simulées à la place. Si vous passez à Simulation, votre casque HoloLens ne répondra pas à l’utilisateur réel tant que vous ne serez pas revenu à l’utilisateur Par défaut.


### <a name="virtual-input"></a>Entrée virtuelle 

Envoie la saisie au clavier de l’ordinateur distant au casque HoloLens.

Cliquez ou appuyez sur la zone située sous le clavier virtuel pour permettre l’envoi de séquences de touches au casque HoloLens. Saisissez du texte dans la zone de saisie de texte, puis cliquez ou appuyez sur Envoyer pour envoyer les séquences de touches à l’application active.

## <a name="see-also"></a>Voir aussi

* [Vue d’ensemble de Windows Device Portal](device-portal.md)
* [Référence sur les API principales du portail d’appareil](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core) (API communes à tous les appareils Windows 10)
* [Référence sur les API à réalité mixte du portail d’appareil](https://docs.microsoft.com/windows/mixed-reality/device-portal-api-reference) (liste de toutes les API REST disponibles pour HoloLens)
