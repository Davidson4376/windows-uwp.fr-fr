---
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: Accueil du développeur sur la Console (Accueil du développeur)
description: Fournit des informations sur l’application Accueil du développeur pour Xbox One.
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 4113df37446d93883cf395e7c1e86b1de6c1b328
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620844"
---
# <a name="developer-home-on-the-console-dev-home"></a>Accueil du développeur sur la Console (Accueil du développeur)
   
  
L’outil Accueil du développeur permet de faire l’expérience d’outils sur le kit de développement Xbox One, conçu pour améliorer la productivité des développeurs. L’outil Accueil du développeur offre des fonctionnalités pour gérer et configurer votre kit de développement, gérer les utilisateurs, lancer des titres installés et réaliser des captures et des suivis. Lors de prochaines publications, nous continuerons à étendre la fonctionnalité pour activer des fonctionnalités supplémentaires basées sur vos commentaires, ainsi que pour activer l’extensibilité et l’ajout de vos propres outils.   
   
  
Nous nous intéressons de près à vos commentaires sur l’outil Accueil du développeur et sur les scénarios pour lesquels vous souhaitez obtenir de l’aide. Veuillez envoyer vos commentaires par le biais des méthodes décrites sous **Envoyer des commentaires**, dans le menu principal de l’application, ou par le biais du Gestionnaire de compte de développeur.   
   
  
Pour lancer l’outil Accueil du développeur sur la récupération de novembre 2015 ou sur une récupération ultérieure :  
 
   1. Ouvrez le guide en déplaçant votre curseur vers la gauche sur le bouton Accueil, ou en double-cliquant sur le bouton Nexus  
   1. Faites défiler la page jusqu’à **Paramètres** (l’icône d’engrenage)   
   1. Sélectionnez **Tous les paramètres**  
   1. À partir de la page **Développeur** par défaut, sélectionnez **Accueil du développeur** (l’icône d’accueil)   

 ![](images/dev_home_icons.png)   
  
Sur les récupérations antérieures, sélectionnez la vignette Accueil du développeur située sur le côté droit de l’écran d’accueil, dans **contenu spécifique**, ou affichez la liste des applications dans le Gestionnaire Xbox One et lancez **Accueil du développeur**.   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>Interface utilisateur  
   
  
L’en-tête de l’interface utilisateur de l’outil Accueil du développeur contient les informations suivantes relatives à la console de développement, disponibles en un coup d’œil :   
 
   *  **Adresse IP de la console :** L’adresse IP actuelle de la console.   
   *  **Nom de la console :** Le nom d’hôte actuel de la console.  
   *  **Bac à sable :** Le nom de bac à sable figurant dans la console.  
   *  **Version du système d’exploitation :** La version actuelle de récupération qui s’exécute sur la console.
   *  Heure actuelle du système.   

   
  
Le reste de l’interface utilisateur de l’outil Accueil du développeur est divisé en pages, comme suit. Pour plus d’informations concernant les outils présents sur ces pages, consultez leurs rubriques individuelles.   
 
   *  [Accueil](devhome-home.md)  
   *  [Xbox Live](devhome-live.md)  
   *  [Paramètres](devhome-settings.md)  
   *  [Capture de média](devhome-capture.md)  
   *  [Mise en réseau](devhome-networking.md)  
   *  [Performances](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>Menu principal  
   
  
En appuyant sur la touche **Menu** de votre manette, vous pouvez accéder au menu principal qui permet de configurer l’espace de travail de l’application, de gérer à la fois les informations d’identification pour accéder aux emplacements réseau et les informations relatives à l’envoi de commentaires sur l’application.   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>Mode ancrage UX  
   
  
Plusieurs outils existants et à venir dans l’outil Accueil du développeur, tels que Mise en réseau et Multijoueur, sont conçus pour être utilisés ancrés sur le côté de l’écran lors de l’exécution de votre titre. Cela vous permet d’accéder facilement aux outils pendant le test.   
   
  
Pour accéder au mode Ancrer, mettez en surbrillance le titre de l’outil correspondant, appuyez sur la touche **Affichage** de votre manette puis, à partir du menu contextuel, sélectionnez **Ancrer**.  
 ![](images/dev_home_4.png)   
  
L’outil Accueil du développeur s’ancrera à droite de l’écran. Vous pouvez changer de contexte en appuyant toujours deux fois sur le bouton Nexus.  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>Personnalisation de l’outil Accueil du développeur  
   
  
L’outil Accueil du développeur a été conçu pour être modulable et personnalisable. Vous pouvez configurer l’application afin de l’adapter à votre flux de travail, puis l’enregistrer en tant qu’espace de travail. Cet espace de travail peut être exporté et importé, vous donnant la possibilité de copier la structure sur d’autres consoles selon vos besoins. Vous pouvez retrouver ces options dans le menu principal sous **espace de travail**. Le fichier exporté se trouvera sur le lecteur de travail du système, dans le répertoire `Dev Home\Workspaces`.   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>Nouvelle dimension et réorganisation des outils  
   
  
Pour modifier la taille ou la position d’un outil, utilisez le bouton du menu contextuel (touche Affichage de votre manette) pendant que le titre est ciblé. À partir du menu contextuel, sélectionnez **Déplacer** ou **Redimensionner**.   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>Modification de la couleur de thème et de l’image d’arrière-plan  
   
  
À partir du menu principal, vous pouvez sélectionner **Espace de travail**, puis **Modifier la couleur du thème**. Pour mettre à jour la couleur de thème utilisée pour la mise en surbrillance du focus, sélectionnez une nouvelle couleur, puis cliquez sur **Enregistrer**.   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>Configurer l’application par défaut pour un package  
   
  
Si un package contient plusieurs applications, l’outil Accueil du développeur vous permettra de définir l’application par défaut qui sera lancée. Mettez en surbrillance le package dans le lanceur et appuyez sur la touche **A** afin d’ouvrir la liste des applications disponibles. Mettez en surbrillance l’application que vous souhaitez définir comme celle par défaut et appuyez sur la touche **Affichage**, puis choisissez l’option **Définir par défaut** dans le menu contextuel.   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>Utilisation de l’outil Accueil du développeur pour enregistrer et lancer des titres à partir d’un partage réseau  
   
  
À partir du lanceur, à la fin de la liste des applications et des jeux installés, vous pouvez sélectionner l’option **Enregistrer un jeu à partir d'un partage réseau** pour exécuter à distance une version libre de fichier d’un titre.   
 ![](images/dev_home_8.png)   
  
Par la suite, vous pouvez saisir le chemin d’accès réseau vers le fichier appxmanifest.xml pour le titre que vous souhaitez enregistrer. L’outil Accueil du développeur tentera d’enregistrer le titre à l’aide de n’importe quels identifiants existants pour ce partage. Si besoin, il pourra demander de nouvelles informations d’identification réseau. Si vous avez besoin d’accéder aux partages supplémentaires (pour accéder symboliquement aux ressources liées sur un serveur distinct, par exemple), vous devrez dans ce cas les ajouter via l’option ci-dessous.   
   
  
Vous pouvez gérer ces informations d’identification stockées (et en ajouter de nouvelles) sur la console par le biais de l’option **Gérer les informations d’identification du réseau** sur le menu principal.   
 ![](images/dev_home_9.png)   
  
Vous pouvez afficher les informations d’identification actuellement sur la console, modifier les informations d’identification en sélectionnant le chemin d’accès de l’information d’identification et en cliquant sur la touche **A**, ainsi que supprimer une information d’identification en sélectionnant le lien Supprimer et en cliquant sur la touche **A**.   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>Dans cette section  
  
[Page d’accueil (accueil Dev)](devhome-home.md)  


&nbsp;&nbsp;Fournit un accès rapide aux tâches qui sont effectuées régulièrement sur une console de développement. 
  
  
[Xbox Live Page (développement accueil)](devhome-live.md)  


&nbsp;&nbsp;Capture des informations multijoueurs et affiche l’état actuel du service Xbox Live. 
  
  
[Page de paramètres (développement accueil)](devhome-settings.md)  


&nbsp;&nbsp;Fournit l’accès à divers paramètres pour la console de développement. 
  
  
[Page (développement accueil) de Capture de média](devhome-capture.md)  


&nbsp;&nbsp;Le **de capture de média** page d’accueil de développement capture vidéo du titre en cours d’exécution sur la console. 
  
  
[Page de mise en réseau (Dev accueil)](devhome-networking.md)  


&nbsp;&nbsp;Simule différentes conditions de mise en réseau pour résoudre des problèmes. 
  
  
[Page de performances (Dev accueil)](devhome-performance.md)  


&nbsp;&nbsp;Simule différentes activité du disque et les conditions d’utilisation du processeur pour des fins de dépannage. 
 