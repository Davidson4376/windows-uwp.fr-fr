---
author: v-angraf
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: Page d’accueil pour les développeurs sur la Console (pour les développeurs d’accueil)
description: Fournit des informations sur l’application d’accueil du centre de développement pour une Xbox.
ms.author: v-angraf@microsoft.com
ms.date: 08/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 3b802b9b53811e03e11ee3afd78f69db4bfd9986
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1015358"
---
# <a name="developer-home-on-the-console-dev-home"></a>Page d’accueil pour les développeurs sur la Console (pour les développeurs d’accueil)
   
  
Accueil du centre de développement est une expérience d’outils sur le Xbox un kit de développement conçu à l’aide de la productivité des développeurs. Accueil du centre de développement offre des fonctionnalités pour gérer et configurer le kit de développement, gérer les utilisateurs, lancer des titres installés et exécuter capture et trace. Dans les futures versions de que nous continuera à développer les fonctionnalités à activer des fonctionnalités supplémentaires en fonction de vos commentaires, ainsi que pour permettre l’extensibilité et l’ajout de vos propres outils.   
   
  
Nous sommes très intéressés par vos commentaires sur Accueil pour les développeurs et les scénarios qui que vous intéressez le plus voir prise en charge. Indiquez vos commentaires via les méthodes décrites sous **Envoyer des commentaires** dans le menu principal de l’application ou par le biais de votre gestionnaire de compte pour les développeurs (biens).   
   
  
Pour lancer la page d’accueil pour les développeurs sur la 2015 novembre ou récupération ultérieure:  
 
   1. Ouvrez le guide par le déplacement de gauche sur Accueil ou double-clic sur le bouton de connexion  
   1. Déplacer vers le bas pour les **paramètres** (l’icône représentant un engrenage)   
   1. Sélectionnez **tous les paramètres**  
   1. Dans la page par défaut **pour les développeurs** , sélectionnez **Accueil pour les développeurs** (l’icône d’accueil)   

 ![](images/dev_home_icons.png)   
  
Sur les récupérations antérieures sélectionnez la vignette d’accueil du centre de développement sur le côté droit de l’écran d’accueil de **sélection de contenu** ou afficher la liste des applications dans un gestionnaire de Xbox et lancement **D’accueil du centre de développement**.   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>Interface utilisateur  
   
  
L’en-tête de l’interface utilisateur de développement accueil contient les éléments suivants importants «en un coup de œil» des informations sur la console de développement:   
 
   *  **IP de la console:** L’adresse IP actuelle de la console.   
   *  **Nom de la console:** Le nom d’hôte actif de la console.  
   *  **Bac à sable:** Le nom du sandbox figurant dans la console.  
   *  **Version du système d’exploitation:** La version actuelle de récupération qui s’exécute sur la console.
   *  Heure actuelle du système.   

   
  
Le reste de l’interface utilisateur un développement à domicile est divisé en les pages suivantes. Pour plus d’informations sur les outils sur ces pages, consultez les rubriques individuelles.   
 
   *  [Home](devhome-home.md)  
   *  [XboxLive](devhome-live.md)  
   *  [Settings](devhome-settings.md)  
   *  [Capture multimédia](devhome-capture.md)  
   *  [Réseaux](devhome-networking.md)  
   *  [Performances](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>Menu principal  
   
  
En appuyant sur le bouton **menu** sur votre contrôleur, vous pouvez accéder au menu principal qui permet la configuration de l’espace de travail d’application, la possibilité de gérer les informations d’identification pour accéder à des informations à fournir des commentaires sur l’application et les emplacements réseau.   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>L’alignement UX  
   
  
Plusieurs outils existants et à venir à domicile pour les développeurs, telles que la mise en réseau et mode, sont conçus pour être utilisés aligné sur le côté pendant l’exécution de votre titre, afin que vous puissiez avoir un accès facile aux outils pendant que vous testez.   
   
  
Pour accéder au composant logiciel enfichable mode, mettez en surbrillance le titre de l’outil approprié, appuyez sur le bouton **affichage** sur votre contrôleur de, sélectionnez **Aligner** dans le menu contextuel:  
 ![](images/dev_home_4.png)   
  
L’outil Accueil du développeur s’ancrera à droite de l’écran. Vous pouvez changer de contexte en appuyant comme d’habitude deux fois sur le bouton Nexus.  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>Personnalisation de l’outil Accueil du développeur  
   
  
L’outil Accueil du développeur a été conçu pour être modulable et personnalisable. Vous pouvez configurer l’application pour l’adapter à votre flux de travail et qui enregistrent un espace de travail. Cet espace de travail peut être exporté et importés, ce qui permet de vous permet de copier la mise en page pour les autres consoles en tant que nécessaire. Ces options sont trouvent dans le menu principal sous **l’espace de travail**. Le fichier exporté se trouvent sur le lecteur scratch système dans le `Dev Home\Workspaces` directory.   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>Redimensionnement et réorganisation des outils  
   
  
Pour modifier la taille ou la position d’un outil, utilisez le bouton menu de contexte (bouton Affichage sur votre contrôleur) pendant le titre a le focus. Dans le menu contextuel, sélectionnez **déplacer** ou pour **redimensionner**.   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>Modification de la couleur de thème et de l’image d’arrière-plan  
   
  
Dans le Menu principal, vous pouvez sélectionner un **espace de travail** , puis **Modifier la couleur de thème**. Sélectionnez une nouvelle couleur et sélectionnez **Enregistrer** pour mettre à jour la couleur de thème utilisée pour la surbrillance le focus.   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>Définition de l’application par défaut d’un package  
   
  
Si un package contient plusieurs applications, accueil Dev vous autorisera à définir l’application par défaut à utiliser. Sélectionnez le package dans le lancement, appuyez sur **le bouton pour ouvrir la liste des applications disponibles** . Sélectionnez celui que vous souhaitez définir par défaut et appuyez sur le bouton **affichage** , puis cliquez sur **définir par défaut** dans le menu contextuel.   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>À l’aide d’accueil du centre de développement pour inscrire et lancer des titres d’un partage réseau  
   
  
À partir du Lanceur, au bas de la liste des jeux, les applications installées, vous pouvez sélectionner l’option **Enregistrer un jeu à partir d’un partage réseau** pour exécuter une version de fichier libre d’un titre à distance.   
 ![](images/dev_home_8.png)   
  
Vous pouvez ensuite entrer le chemin d’accès réseau au fichier appxmanifest.xml pour le titre que vous souhaitez enregistrer. Accueil du centre de développement tentera d’enregistrer le titre à l’aide des informations d’identification existantes pour ce partage et si nécessaire vous demandera de nouvelles informations d’identification du réseau. Si vous avez besoin d’accéder aux partages supplémentaires (par exemple pour les ressources liées symboliquement l’accès sur un serveur distinct) vous devez ajouter les via l’option ci-dessous.   
   
  
Vous pouvez gérer ces informations d’identification stockées (et ajouter de nouvelles) sur la console de l’option **Gérer les informations d’identification réseau** du menu principal.   
 ![](images/dev_home_9.png)   
  
Vous pouvez afficher les informations d’identification actuellement sur la console, modifier les informations d’identification en sélectionnant le chemin d’accès de l’information d’identification en cliquant sur **un** bouton et supprimer des informations d’identification en cliquant sur le lien Supprimer en cliquant sur **un** bouton.   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>Dans cette section  
  
[Page d’accueil (pour les développeurs d’accueil)](devhome-home.md)  


&nbsp;&nbsp;Fournit un accès rapide aux tâches fréquemment effectuées sur une console de développement. 
  
  
[Xbox Live Page (pour les développeurs d’accueil)](devhome-live.md)  


&nbsp;&nbsp;Capture des informations multijoueur et affiche l’état actuel du service Xbox Live. 
  
  
[Page Paramètres (développement d’accueil)](devhome-settings.md)  


&nbsp;&nbsp;Fournit l’accès à différents paramètres de la console de développement. 
  
  
[Page (pour les développeurs d’accueil) de Capture de média](devhome-capture.md)  


&nbsp;&nbsp;La page d’accueil du centre de développement **de capture de média** capture vidéo du titre qui est en cours d’exécution sur la console. 
  
  
[Page de mise en réseau (pour les développeurs d’accueil)](devhome-networking.md)  


&nbsp;&nbsp;Simule les différentes conditions réseau aux fins de dépannage. 
  
  
[Page de performance (pour les développeurs d’accueil)](devhome-performance.md)  


&nbsp;&nbsp;Simule différents des conditions d’utilisation du processeur pour le dépannage et de l’activité de disque. 
 