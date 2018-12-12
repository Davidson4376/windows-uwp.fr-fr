---
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: Accueil du développeur sur la Console (Dev Home)
description: Fournit des informations sur l’application Dev Home pour Xbox One.
ms.date: 08/09/2017
ms.topic: article
keywords: windows10, uwp
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 4113df37446d93883cf395e7c1e86b1de6c1b328
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8936430"
---
# <a name="developer-home-on-the-console-dev-home"></a>Accueil du développeur sur la Console (Dev Home)
   
  
Accueil du développeur est d’expérimenter des outils dans le kit de développement Xbox One sont destiné à améliorer la productivité des développeurs. Accueil du développeur offre des fonctionnalités pour gérer et configurer votre kit de développement, gérer les utilisateurs, lancer titres installés et effectuer des captures et effectue le suivi. Dans les futures versions que nous continuerons à développer les fonctionnalités pour activer des fonctionnalités supplémentaires en fonction de vos commentaires, ainsi que pour activer l’ajout de vos propres outils et d’extensibilité.   
   
  
Nous sommes très intéressés par vos commentaires sur l’accueil du développeur et les scénarios que vous vous intéressez plus voir pour prendre en charge. Fournissez vos commentaires par le biais des méthodes décrites sous **Envoyer des commentaires** dans le menu principal de l’application ou par le biais de votre gestionnaire de compte de développeur (mère).   
   
  
Pour lancer l’accueil du développeur sur le novembre 2015 ou restauration ultérieure:  
 
   1. Ouvrez le guide en déplaçant gauche sur Accueil ou double-clic sur le bouton Nexus  
   1. Déplacer vers le bas les **paramètres** (l’icône d’engrenage)   
   1. Sélectionnez **tous les paramètres**  
   1. Dans la page de **développeur** par défaut, sélectionnez **Accueil du développeur** (l’icône d’accueil)   

 ![](images/dev_home_icons.png)   
  
Sur les récupérations antérieures sélectionner la vignette accueil du développeur sur le côté droit de l’écran d’accueil de **contenu du numéro** ou afficher la liste des applications dans le gestionnaire une Xbox et lancer **Accueil du développeur**.   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>Interface utilisateur  
   
  
L’en-tête de l’interface utilisateur de Dev Home contient les éléments suivants importants «en un coup de œil» plus d’informations sur la console de développement:   
 
   *  **IP de la console:** L’adresse IP actuelle de la console.   
   *  **Nom de la console:** Le nom d’hôte en cours de la console.  
   *  **Bac à sable:** Nom du bac à sable figurant dans la console.  
   *  **Version du système d’exploitation:** La version actuelle de récupération qui s’exécute sur la console.
   *  Heure système actuelle.   

   
  
Le reste de l’interface utilisateur un développement à domicile se divise en les pages suivantes. Pour plus d’informations sur les outils sur ces pages, consultez les rubriques correspondantes.   
 
   *  [Home](devhome-home.md)  
   *  [XboxLive](devhome-live.md)  
   *  [Paramètres](devhome-settings.md)  
   *  [Capture multimédia](devhome-capture.md)  
   *  [Mise en réseau](devhome-networking.md)  
   *  [Analyse des performances](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>Menu principal  
   
  
En appuyant sur le bouton **menu** sur votre contrôleur, vous pouvez accéder au menu principal qui permet la configuration de l’espace de travail d’application, la possibilité de gérer les informations d’identification pour accéder à des emplacements réseau et des informations sur en fournissant des commentaires sur l’application.   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>Mode ancrage UX  
   
  
Plusieurs outils existants et à venir dans accueil du développeur, par exemple, la mise en réseau et le mode multijoueur, sont conçues pour être utilisé ancrée sur le côté lors de l’exécution votre titre, afin que vous pouvez avoir accéder facilement aux outils pendant que vous testez.   
   
  
Pour accéder au mode ancrage, mettre en évidence le titre de l’outil approprié, appuyez sur le bouton **affichage** de votre manette, sélectionnez **Aligner** dans le menu contextuel:  
 ![](images/dev_home_4.png)   
  
L’outil Accueil du développeur s’ancrera à droite de l’écran. Vous pouvez changer de contexte en appuyant comme d’habitude deux fois sur le bouton Nexus.  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>Personnalisation de l’outil Accueil du développeur  
   
  
L’outil Accueil du développeur a été conçu pour être modulable et personnalisable. Vous pouvez configurer l’application pour l’adapter à votre flux de travail et qui enregistrer en tant qu’un espace de travail. Cet espace de travail peut être exportée et importées, ce qui permet de vous permet de copier la disposition pour les autres consoles en tant que nécessaire. Ces options sont disponibles dans le menu principal sous **l’espace de travail**. Le fichier exporté résultants se trouveront sur le lecteur du système scratch dans le `Dev Home\Workspaces` répertoire.   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>Redimensionnement et réorganisation des outils  
   
  
Pour modifier la taille ou la position d’un outil, utilisez le bouton de menu contextuel (bouton Affichage de votre manette) pendant le titre a le focus. Dans le menu contextuel, sélectionnez **déplacer** ou **redimensionner**.   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>Modification de la couleur de thème et de l’image d’arrière-plan  
   
  
Dans le Menu principal, vous pouvez sélectionner un **espace de travail** , puis **changer de couleur de thème**. Sélectionnez une nouvelle couleur et cliquez sur **Enregistrer** pour mettre à jour la couleur de thème utilisée pour mettre en surbrillance du focus.   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>Définition de l’application par défaut d’un package  
   
  
Si un package contient plusieurs applications, Dev Home vous permettent de définir le lancement de l’application par défaut. Sélectionnez le package dans le Lanceur et appuyez sur le bouton **A** pour ouvrir la liste des applications disponibles. Sélectionnez celui que vous souhaitez définir en tant que la valeur par défaut et appuyez sur le bouton **affichage** , puis choisissez **définir par défaut** dans le menu contextuel.   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>L’utilisation de Dev Home pour enregistrer et lancer des titres depuis un partage réseau  
   
  
À partir du Lanceur, en bas de la liste des jeux, les applications installées, vous pouvez sélectionner l’option **inscrire un jeu à partir d’un partage réseau** pour exécuter une version de fichiers isolés d’un titre à distance.   
 ![](images/dev_home_8.png)   
  
Vous pouvez ensuite entrer le chemin d’accès réseau dans le fichier appxmanifest.xml pour le titre que vous voulez inscrire. Accueil du développeur tente d’inscrire le titre à l’aide des informations d’identification existantes pour ce partage et si nécessaire demandera nouvelles informations d’identification réseau. Si vous avez besoin d’accéder aux partages supplémentaires (par exemple pour les ressources d’accès symboliquement lié sur un serveur distinct) vous devrez ajouter celles via l’option ci-dessous.   
   
  
Vous pouvez gérer ces informations d’identification stockées (et ajouter de nouvelles) sur la console via l’option de **Gérer les informations d’identification réseau** du menu principal.   
 ![](images/dev_home_9.png)   
  
Vous pouvez afficher les informations d’identification actuellement sur la console, modifier les informations d’identification en sélectionnant le chemin d’accès de l’information d’identification et en cliquant sur **un** bouton et supprimer des informations d’identification en sélectionnant le lien Supprimer et cliquer sur **un** bouton.   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>Dans cette section  
  
[Page d’accueil (Dev Home)](devhome-home.md)  


&nbsp;&nbsp;Permet d’accéder rapidement aux tâches qui sont effectuées régulièrement sur une console de développement. 
  
  
[Xbox Live Page (Dev Home)](devhome-live.md)  


&nbsp;&nbsp;Capture des informations en mode multijoueur et affiche l’état actuel du service Xbox Live. 
  
  
[Page des paramètres (Dev Home)](devhome-settings.md)  


&nbsp;&nbsp;Fournit l’accès à différents paramètres pour la console de développement. 
  
  
[Page (Dev Home) de Capture multimédia](devhome-capture.md)  


&nbsp;&nbsp;La page d’accueil du développeur **de capture multimédia** capture vidéo du titre qui est en cours d’exécution sur la console. 
  
  
[Page de mise en réseau (Dev Home)](devhome-networking.md)  


&nbsp;&nbsp;Simule les différentes conditions de mise en réseau pour résoudre des problèmes. 
  
  
[Page des performances (Dev Home)](devhome-performance.md)  


&nbsp;&nbsp;Simule différents activité du disque et des conditions d’utilisation du processeur pour résoudre des problèmes. 
 