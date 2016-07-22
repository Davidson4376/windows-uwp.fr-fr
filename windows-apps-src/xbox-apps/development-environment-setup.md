---
author: Mtoepke
title: "Configurer votre plateforme UWP sur l’environnement de développement Xbox"
description: "Étapes relatives à la configuration et au test de votre plateforme Windows universelle sur l’environnement de développement Xbox."
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: bdf7a32d2f0673ab6c176a775b805eff2b7cf437
ms.openlocfilehash: d56206f990e7885af4935401356bd3a2ce2cd292

---

# Configurer votre plateforme UWP sur l’environnement de développement Xbox

La plateforme Windows universelle (UWP) sur l’environnement de développement Xbox se compose d’un ordinateur dédié au développement connecté à une console Xbox One via un réseau local.
L’ordinateur de développement requiert Windows10, Visual Studio2015 Update2, la version d’évaluation14295 du kit de développement logiciel (SDK) Windows10 et divers outils de prise en charge.


Cet article couvre les étapes relatives à la configuration et au test de votre environnement de développement.

## Installation de Visual Studio

1. Installez Visual Studio2015 Update2 ou version ultérieure. Pour en savoir plus et pour l’installation, voir [Téléchargements et outils pour Windows 10](https://dev.windows.com/downloads).

1. Lorsque vous installez Visual Studio 2015 Update 2, assurez-vous que la case **Outils de développement d’applications Windows universelles** est cochée.

  ![Installer Visual Studio2015 Update2 ou version ultérieure.](images/vs_install_tools.png)

## Installation du Kit de développement logiciel (SDK) Windows10

Installez la dernière version d’évaluation du Kit de développement logiciel (SDK) Windows10. Pour obtenir des informations sur l’installation, voir [Télécharger les mises à jour Insider Preview pour les développeurs](http://go.microsoft.com/fwlink/p/?LinkId=780552).

  > **Important** &nbsp;&nbsp;Vous devez installer la dernière version du Kit de développement logiciel (SDK), toutefois, vous n’avez _pas_ à installer la dernière version de Windows Insider Preview du système d’exploitation.

## Configuration de votre XboxOne

Avant de pouvoir déployer une application vers votre XboxOne, un utilisateur doit être connecté à la console. Vous pouvez utiliser votre compte XboxLive existant ou créer un compte pour votre console en Mode développeur. 

## Créer votre première application

1. Assurez-vous que l’ordinateur de développement se trouve sur le même réseau local que la console Xbox One cible. En règle générale, cela signifie qu’ils doivent utiliser le même routeur et être sur le même sous-réseau. Une connexion réseau câblée est recommandée.

1. Assurez-vous que votre console Xbox One se trouve en Mode développeur.  Pour plus d’informations, voir [Activation du Mode développeur sur Xbox One](devkit-activation.md).

1. Déterminez le langage de programmation que vous voulez utiliser pour votre application UWP.

1. Sur votre ordinateur de développement, sélectionnez **Nouveau projet**, puis **Windows / Universel / Application vide**.

### Démarrage d’un projet c#

  ![Boîte de dialogue Nouveau projet](images/vs_universal_blank.jpg)

1. Sélectionnez les options par défaut dans la boîte de dialogue **Nouveau projet Windows universel**. Si la boîte de dialogue **Mode développeur** s’affiche, cliquez sur **OK**. Une nouvelle application vide est créée.

1. Configurez votre environnement de développement pour le débogage à distance:

  1. Cliquez avec le bouton droit sur le projet, puis sélectionnez **Propriétés**.
  1. Sur l’onglet **Déboguer**, définissez **Périphérique cible** sur **Ordinateur distant**.
  1. Dans **Ordinateur distant**, entrez l’adresse IP du système ou le nom d’hôte de la console Xbox One. Pour plus d’informations sur l’obtention de l’adresse IP ou du nom d’hôte, consultez [Présentation des outils Xbox One](introduction-to-xbox-tools.md).
  1. Dans la liste déroulante **Mode d’authentification**, sélectionnez **Universel (protocole non chiffré)**.

    ![Pages de propriétés BlankApp C#](images/vs_remote.jpg)

### Démarrage d’un projet C++

  ![Projet C++](images/vs_universal_cpp_blank.jpg)

1. Sélectionnez les options par défaut dans la boîte de dialogue **Nouveau projet Windows universel**. Si la boîte de dialogue **Mode développeur** s’affiche, cliquez sur **OK**. Une nouvelle application vide est créée.

1. Configurez votre environnement de développement pour le débogage à distance:

   1. Cliquez avec le bouton droit sur le projet, puis sélectionnez **Propriétés**.
   1. Sur l’onglet **Débogage** et définissez **Débogueur à lancer** sur **Ordinateur distant**.
   1. Dans **Nom de l’ordinateur**, entrez l’adresse IP du système ou le nom d’hôte de la console Xbox One. Pour plus d’informations sur l’obtention de l’adresse IP ou du nom d’hôte, consultez [Présentation des outils Xbox One](introduction-to-xbox-tools.md).
   1. Dans la liste déroulante **Type d’authentification**, sélectionnez **Universel (protocole non chiffré)**.

    ![Pages de propriétés BlankApp C++](images/vs_remote_cpp.jpg)

### Jumeler le code confidentiel de votre appareil avec Visual Studio

1. Enregistrez vos paramètres et assurez-vous que votre console Xbox One est en Mode développeur.

1. Appuyez sur F5.

1. S’il s’agit de votre premier déploiement, vous obtiendrez une boîte de dialogue issue de Visual Studio vous demandant d’épingler par paire votre appareil.

  1. Pour obtenir un code confidentiel, ouvrez **Dev Home** à partir de l’écran d’accueil sur votre console Xbox One.
  1. Sélectionnez **Jumeler avec Visual Studio**.

    ![Boîte de dialogue Jumeler avec Visual Studio](images/devhome_visualstudio.png)

  1. Entrez votre code confidentiel dans la boîte de dialogue **Jumeler avec Visual Studio**. Le code confidentiel suivant est juste un exemple. Le vôtre sera différent.

    ![Boîte de dialogue du code confidentiel Jumeler avec Visual Studio](images/devhome_pin.png)

  1. Des erreurs de déploiement, le cas échéant, s’afficheront dans la fenêtre **Sortie**.

Félicitations, vous avez correctement créé et déployé votre première application UWP sur Xbox!



## Voir aussi
- [Activation du Mode développeur sur Xbox One](devkit-activation.md)  
- [Téléchargements et outils pour Windows10](https://dev.windows.com/downloads)  
- [Télécharger les mises à jour Insider Preview pour les développeurs](http://go.microsoft.com/fwlink/?LinkId=780552)  
- [Présentation des outils Xbox One](introduction-to-xbox-tools.md) 
- [UWP sur XboxOne](index.md)

----



<!--HONumber=Jun16_HO5-->


