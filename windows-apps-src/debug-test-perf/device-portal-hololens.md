---
author: mcleblanc
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: Device Portal pour HoloLens
description: "Découvrez comment Windows Device Portal pour HoloLens vous permet de configurer et de gérer à distance votre appareil HoloLens."
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: bd1ae8ccdd81f319fc36ca04b8b839cb313b2854

---
# Device Portal pour HoloLens


## Configurer Device Portal sur HoloLens

### Activer Device Portal

**Pour activer Device Portal**

1. Mettez HoloLens sous tension et allumez l’appareil.
2. Effectuer le geste qui consiste à [maintenir sa paume de main vers le haut en serrant ses doigts et à les ouvrir](https://dev.windows.com/holographic/Gestures.html#Bloom) pour lancer le menu principal.
3. Regardez la vignette **Paramètres** et effectuez le geste consistant à [appuyer](https://dev.windows.com/holographic/Gestures.html#Press_and_release). Appuyez de nouveau pour placer l’application dans votre environnement. L’application Paramètres démarre une fois placée.
4. Sélectionnez l’élément de menu **Mettre à jour**.
5. Sélectionnez l’élément de menu **Pour les développeurs**.
6. Activez **Mode développeur**.
7. [Faites défiler](https://dev.windows.com/holographic/Gestures.html#Navigation) la liste et activez Device Portal.


### Jumeler votre appareil

### Se connecter via Wi-Fi 

1. Connectez votre HoloLens au Wi-Fi.
2. Recherchez l’adresse IP de votre appareil. Recherchez l’adresse IP de l’appareil sous Paramètres &gt; Réseau et Internet &gt; Wi-Fi &gt; Options avancées.
    Vous pouvez également demander : « Hey Cortana, quelle est mon adresse IP ? »

3. À partir d’un navigateur web sur votre PC, accédez à `https://<YOUR_HOLOLENS_IP_ADDRESS>`
    - Le navigateur affiche le message suivant : « Le certificat de sécurité de ce site web pose problème. ». Cela se produit car le certificat envoyé à Device Portal est un certificat de test. Vous pouvez ignorer cette erreur de certificat pour le moment et continuer.

### Se connecter via USB 

1. Installez les outils pour vous assurer que Visual Studio Update 1 et les outils de développement Windows 10 sont installés sur votre PC. Cela permet d’activer la connectivité USB.
2. Connectez votre HoloLens au PC à l’aide d’un câble micro-USB.
3. À partir d’un navigateur web sur votre PC, accédez à `http://127.0.0.1:10080`.

### Se connecter à un émulateur 

Vous pouvez également utiliser Device Portal avec votre émulateur. Pour vous connecter à Device Portal, utilisez la barre d’outils. Cliquez sur cette icône :
- Ouvrir Device Portal : ouvre Windows Device Portal pour le système d’exploitation HoloLens dans l’émulateur.

### Créer un nom d’utilisateur et un mot de passe 

Vous devrez créer un nom d’utilisateur et un mot de passe sur Device Portal de votre HoloLens lors de votre première connexion.
1. Dans un navigateur web sur votre PC, entrez l’adresse IP de l’HoloLens. La page d’accès à la configuration s’affiche.
2. Cliquez ou appuyez sur Demander un code confidentiel et regardez l’écran HoLolens pour obtenir le code confidentiel généré.
3. Entrez le code confidentiel dans la zone de texte prévue à cet effet de votre appareil.
4. Entrez le nom d’utilisateur que vous utiliserez pour vous connecter à Device Portal. Il n’est pas nécessaire qu’il s’agisse d’un nom de compte Microsoft (MSA) ou de domaine.
5. Entrez un mot de passe et confirmez-le. Le mot de passe doit comporter au moins sept caractères. Il n’est pas nécessaire qu’il s’agisse d’un mot de passe de compte Microsoft (MSA) ou de domaine.
6. Cliquez sur Coupler pour se connecter à Windows Device Portal sur le HoloLens.

Si vous souhaitez modifier ce nom d’utilisateur ou ce mot de passe, vous pouvez à tout moment répéter ce processus en accédant à la page de sécurité de l’appareil en cliquant sur le lien Sécurité situé en haut à droite ou en accédant à : `https://<YOUR_HOLOLENS_IP_ADDRESS>/devicesecurity.htm`.

### Certificat de sécurité 

Si une « erreur de certificat » s’affiche dans votre navigateur, vous pouvez la résoudre en créant une relation d’approbation avec l’appareil.

Chaque HoloLens génère un certificat auto-signé unique pour sa connexion SSL. Par défaut, ce certificat n’est pas approuvé par le navigateur web de votre PC, c’est la raison pour laquelle vous obtiendrez peut-être une « erreur de certificat ». En téléchargeant ce certificat à partir de votre HoloLens (via une connexion USB ou Wi-Fi approuvée) et en l’approuvant sur votre PC, vous pouvez vous connecter en toute sécurité à votre appareil.
1. Vérifiez que vous vous trouvez sur un réseau sécurisé (USB ou réseau Wi-Fi approuvé).
2. Téléchargez le certificat de cet appareil à partir de la page Sécurité sur Device Portal soit en cliquant sur le lien Sécurité à partir de la liste des icônes située en haut à droite, soit en accédant à : `https://<YOUR_HOLOLENS_IP_ADDRESS>/devicesecurity.htm`

3. Installez le certificat dans le magasin Autorités de certification racines de confiance sur votre PC. À partir du menu Windows, tapez : Gérer les certificats d’ordinateur et démarrer l’applet.
    - Développez le dossier Autorité de certification racine de confiance.
    - Cliquez sur le dossier Certificats.
    - Dans le menu Action, sélectionnez : Toutes les tâches &gt; Importer...
    - Terminez l’Assistant Importation de certificat en utilisant le fichier de certificat que vous avez téléchargé à partir de Device Portal.

4. Redémarrez le navigateur.


## Pages de Device Portal 

### Accueil 

Votre session Device Portal démarre sur la page d’accueil. Accédez à d’autres pages à partir de la barre de navigation située sur le côté gauche de la page d’accueil.

La barre d’outils se trouvant en haut de la page permet d’accéder aux fonctionnalités et aux états couramment utilisés.
- **En ligne** : indique si l’appareil est connecté au Wi-Fi.
- **Arrêt** : éteint l’appareil.
- **Redémarrer** : mise sous tension de l’appareil par cycle.
- **Sécurité** : ouvre la page de sécurité de l’appareil.
- **Froid** : indique la température de l’appareil.
- **Secteur** : indique si l’appareil est branché sur le secteur et en cours de chargement.
- **Aide** : ouvre la page relative à la documentation de l’interface REST.

La page d’accueil affiche les informations suivantes :
- État de l’**appareil** : surveille l’état de votre appareil et signale les erreurs critiques.
- **Informations Windows** : affiche le nom du casque HoloLens et de la version de Windows installée actuellement.
- La section **Préférences** comprend les paramètres suivants :
    - **IPD** : définit l’écart pupillaire correspondant à la distance, exprimée en millimètres, séparant le centre des pupilles de l’utilisateur lorsqu’il regarde droit devant lui. Le paramètre prend immédiatement effet. La valeur par défaut a été calculée automatiquement lors de la configuration de votre appareil.
    - **Nom de l’appareil** : attribuez un nom au casque HoloLens. Vous devez redémarrer l’appareil après avoir modifié cette valeur afin qu’elle soit prise en compte. Après avoir cliqué sur Enregistrer, une boîte de dialogue vous demande si vous voulez redémarrer l’appareil immédiatement ou ultérieurement.
    - **Paramètres de la veille** : définit le délai d’attente avant la mise en veille de l’appareil lorsque celui-ci est branché et sur batterie.

### Vue 3D 

Utilisez la page Vue 3D pour voir comment HoloLens interprète votre environnement. Naviguez dans la vue à l’aide de la souris :
- **Pivoter** : clic gauche + souris ;
- **Mouvement panoramique** : clic droit + souris ;
- **Zoom** : défilement à l’aide de la souris.
- **Options de suivi** : activez le suivi visuel en continu en cochant suivi visuel Forcer le suivi visuel. La mise en pause arrête le suivi visuel.
- **Options d’affichage** : définit les options relatives à la vue 3D : Suivi : indique si le suivi visuel est actif.
- **Afficher le sol** : affiche un plan de sol en damier.
- **Afficher le tronc de cône** : affiche le tronc de cône de l’affichage.
- **Afficher le plan de stabilisation** : affiche le plan utilisé par HoloLens pour stabiliser le mouvement.
- **Afficher le maillage** : affiche le maillage de mappage de surface qui représente votre environnement.
- **Afficher les détails** : affiche la position des mains, les quaternions de rotation de la tête et le vecteur d’origine à mesure de leur changement en temps réel.
- **Bouton plein écran** : affiche la vue 3D en mode plein écran. Appuyez sur la touche ÉCHAP pour quitter le mode plein écran.

- Reconstruction de surface : cliquez ou appuyez sur Mettre à jour pour afficher le tout dernier maillage de mappage spatial à partir de l’appareil. Un passage complet peut nécessiter un certain temps, pouvant aller jusqu’à quelques secondes. Le maillage ne se met pas à jour automatiquement dans la vue 3D. Vous devez cliquer sur Mettre à jour pour obtenir le tout dernier maillage à partir de l’appareil. Cliquez sur Enregistrer pour enregistrer le maillage de mappage spatial actuel en tant que fichier obj sur votre PC.

### MRC (Mixed Reality Capture) 

Utilisez la page MRC pour enregistrer les flux multimédias issus du casque HoloLens.
- Paramètres : contrôlez les flux multimédias capturés en cochant les paramètres suivants : - Hologrammes : capture le contenu holographique dans le flux vidéo. Les hologrammes font l’objet d’un rendu en mono et non en stéréo.
- **Caméra PV** : capture le flux vidéo à partir de la caméra photo/vidéo.
- **Son du micro** : capture les données audio à partir du réseau de microphones.
- **Son de l’application** : capture les données audio à partir de l’application en cours d’exécution.
- **Qualité de l’aperçu instantané** : sélectionnez la résolution d’écran, la fréquence d’images et la vitesse de diffusion en continu de l’aperçu instantané.

- Cliquez ou appuyez sur le bouton d’aperçu instantané pour afficher le flux de capture. Arrêter l’aperçu instantané arrête le flux de capture.
- Cliquez ou appuyez sur Enregistrer pour commencer à enregistrer le flux MRC à l’aide des paramètres spécifiés. Arrêter l’enregistrement arrête l’enregistrement et l’enregistre.
- Cliquez ou appuyez sur Prendre une photo pour prendre une image fixe à partir du flux de capture.
- **Photos et vidéos** : affiche une liste des captures photo et vidéo prises sur l’appareil.

Notez que les applications HoloLens ne sont pas en mesure de capturer de photo ou de vidéo MRC en cours d’enregistrement ou de diffusion en continu d’un aperçu instantané à partir de Device Portal.

### Performances du système 

L’outil Performances du système sur HoloLens propose trois mesures supplémentaires qu’il est possible d’enregistrer. 

Voici les mesures disponibles :
- **Alimentation SOC** : utilisation de l’alimentation système sur puce instantanée, moyenne par minute
- **Alimentation système** : utilisation de l’alimentation système instantanée, moyenne par minute
- **Fréquence d’images** : images par seconde, VBlanks manqués par seconde et VBlanks manqués successifs

### Page relative aux vidages sur incident des applications 

Cette page vous permet de recueillir les vidages sur incident de vos applications chargées de manière indépendante. Cochez la case Activation des vidages sur incident pour chaque application pour laquelle vous souhaitez recueillir des vidages sur incident. Revenez à cette page pour recueillir les vidages sur incident. Les fichiers de vidage peuvent être ouverts dans Visual Studio pour le débogage.

### Mode plein écran 

Active le mode plein écran, qui limite la capacité de l’utilisateur à lancer de nouvelles applications ou à modifier l’application en cours d’exécution. Lorsque le mode plein écran est activé, le mouvement correspondant à une paume de main qui s’ouvre et Cortana sont désactivés et les applications placées ne sont pas affichées dans l’environnement de l’utilisateur.

Cochez la case Activer le mode plein écran pour placer le casque HoloLens en mode plein écran. Sélectionnez l’application dans la liste déroulante Application de démarrage. Cliquez ou appuyez sur Enregistrer pour valider les paramètres.

Notez que l’application s’exécute au démarrage même si le mode plein écran n’est pas activé. Sélectionnez Aucune pour qu’aucune application ne s’exécute au démarrage.

### Simulation 

Vous permet d’enregistrer et de lire des données d’entrée pour le test.
- **Capturer la salle** : permet de télécharger un fichier de simulation de pièce contenant le maillage de mappage spatial de l’environnement de l’utilisateur. Nommez la pièce, puis cliquez sur Capturer pour enregistrer les données sous forme de fichier .xef sur votre PC. Ce fichier de pièce peut être chargé dans l’émulateur HoloLens.
- **Enregistrement** : cochez les flux à enregistrer, nommez l’enregistrement, puis cliquez ou appuyez sur Enregistrer pour démarrer l’enregistrement. Effectuer des actions avec votre HoloLens, puis cliquez sur Arrêter pour enregistrer les données sous forme de fichier .xef sur votre PC. Ce fichier peut être chargé dans l’émulateur ou l’appareil HoloLens.
- **Lecture** : cliquez ou appuyez sur Charger l’enregistrement pour sélectionner un fichier xef à partir de votre PC et envoyer les données à HoloLens.
- **Mode contrôle** : sélectionnez Par défaut ou Simulation dans la liste déroulante, puis cliquez ou appuyez sur le bouton Définir pour sélectionner le mode sur le casque HoloLens. Choisissez Simulation pour désactiver les capteurs réels de votre casque HoloLens et utiliser les données simulées à la place. Si vous passez à Simulation, votre casque HoloLens ne répondra pas à l’utilisateur réel tant que vous ne serez pas revenu à l’utilisateur Par défaut.


### Entrée virtuelle 

Envoie la saisie au clavier de l’ordinateur distant au casque HoloLens.

Cliquez ou appuyez sur la zone située sous le clavier virtuel pour permettre l’envoi de séquences de touches au casque HoloLens. Saisissez du texte dans la zone de saisie de texte, puis cliquez ou appuyez sur Envoyer pour envoyer les séquences de touches à l’application active.



<!--HONumber=Jun16_HO4-->


