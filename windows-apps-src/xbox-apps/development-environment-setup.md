---
title: Configurer votre plateforme UWP sur l’environnement de développement Xbox
description: Procédure de configuration et de test de votre plateforme Windows universelle sur l’environnement de développement Xbox.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 8801c0d9-94a5-41a2-bec3-14f523d230df
ms.localizationpriority: medium
ms.openlocfilehash: 02c33e0dbe1209f3c31937df800ceecb354475f5
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67322135"
---
# <a name="set-up-your-uwp-on-xbox-development-environment"></a>Configurer votre plateforme UWP sur l’environnement de développement Xbox

La plateforme Windows universelle (UWP) sur l’environnement de développement Xbox se compose d’un ordinateur dédié au développement connecté à une console Xbox One via un réseau local.
L’ordinateur de développement requiert Windows 10, Visual Studio 2017 ou Visual Studio 2015 Update 3, la version 14393 du kit de développement logiciel (SDK) Windows 10 et divers outils de prise en charge.


Cet article couvre les étapes relatives à la configuration et au test de votre environnement de développement.

## <a name="visual-studio-setup"></a>Installation de Visual Studio

1. Installer Visual Studio 2017, Visual Studio 2015 Update 3 ou la dernière version de Visual Studio. Pour en savoir plus et pour l’installation, voir [Téléchargements et outils pour Windows 10](https://developer.microsoft.com/windows/downloads). Nous vous recommandons d’utiliser la dernière version de Visual Studio afin que vous pouvez recevoir les dernières mises à jour pour les développeurs et de sécurité.

2. Si vous installez Visual Studio 2017, veillez à choisir la charge de travail **Développement de plateforme Windows universelle**. Si vous êtes un développeur en C++, assurez-vous de sélectionner également la case **outils de plateforme Windows universelle C++** dans le volet de droite **Résumé**, sous **Développement de plateforme Windows universelle**. Elle ne fait pas partie de l'installation par défaut.

    ![Installer Visual Studio 2017](images/development-environment-setup-1.png)

    Si vous installez Visual Studio 2015 Update 3, assurez-vous que la case **Outils de développement d’applications Windows universelles** soit cochée.

    ![Installer Visual Studio 2015 Update 2 ou version ultérieure.](images/vs_install_tools.png)

## <a name="windows-10-sdk-setup"></a>Installation du Kit de développement logiciel (SDK) Windows 10

Installer la dernière version du kit SDK Windows 10. Ce kit est fourni avec votre installation de Visual Studio. Mais si vous souhaitez le télécharger séparément, consultez [Kit SDK Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk).


## <a name="enabling-developer-mode"></a>Activation du mode développeur

Avant de pouvoir développer les applications à partir de votre PC de développement, vous devez activer le mode Développeur. Dans les **Paramètres**, naviguez vers **Mise à jour et sécurité** / **Pour les développeurs**, et sous **Utiliser les fonctionnalités de développeur**, sélectionnez **Mode Développeur**.

## <a name="setting-up-your-xbox-one"></a>Configuration de votre Xbox One

Avant de pouvoir déployer une application sur votre Xbox One, un utilisateur doit être connecté à la console. Vous pouvez utiliser votre compte Xbox Live existant ou créer un compte pour votre console en mode Développeur. 

## <a name="create-your-first-app"></a>Créer votre première application

1. Assurez-vous que l’ordinateur de développement se trouve sur le même réseau local que la console Xbox One cible. En règle générale, cela signifie qu’ils doivent utiliser le même routeur et être sur le même sous-réseau. Une connexion réseau câblée est recommandée.

2. Assurez-vous que votre console Xbox One se trouve en Mode développeur.  Pour plus d’informations, consultez [Activation du mode Développeur Xbox One](devkit-activation.md).

3. Déterminez le langage de programmation que vous voulez utiliser pour votre application UWP.

4. Sur votre PC de développement, dans Visual Studio, sélectionnez **Nouveau / projet**.

5. Dans la fenêtre **Nouveau projet**, sélectionnez **Application Windows universelle / vide (Windows universelle)** .

### <a name="starting-a-c-project"></a>Démarrage d’un projet C#

  ![Boîte de dialogue Nouveau projet](images/development-environment-setup-2.png)

1. Dans la boîte de dialogue **Nouveau projet Windows universel**, sélectionnez la build 14393 ou ultérieure dans la liste déroulante **Version minimum**. Dans la liste déroulante **Version cible**, sélectionnez le dernier kit SDK. Si la boîte de dialogue **Mode Développeur** s’affiche, cliquez sur **OK**. Une nouvelle application vide est créée.

2. Configurez votre environnement de développement pour le débogage à distance :

    a. Cliquez avec le bouton droit sur le projet dans **Explorateur de solutions**, puis sélectionnez **Propriétés**.

    b. Dans l’onglet **Déboguer**, redéfinissez **Plateforme** sur **x64**. (La plateforme x86 n’est plus prise en charge sur Xbox.)

    c. Sous **Options de démarrage**, définissez **Périphérique cible** sur **Ordinateur distant**.

    d. Dans **Ordinateur distant**, entrez l’adresse IP du système ou le nom d’hôte de la console Xbox One. Pour plus d’informations sur l’obtention de l’adresse IP ou du nom d’hôte, consultez [Présentation des outils Xbox One](introduction-to-xbox-tools.md).

    e. Dans la liste déroulante **Mode d’authentification**, sélectionnez **Universel (protocole non chiffré)** .

    ![Pages de propriétés BlankApp C#](images/vs_remote.jpg)

### <a name="starting-a-c-project"></a>Démarrage d’un projet C++

  ![Projet C++](images/development-environment-setup-3.png)

1. Dans la boîte de dialogue **Nouveau projet Windows universel**, sélectionnez la build 14393 ou ultérieure dans la liste déroulante **Version minimum**. Dans la liste déroulante **Version cible**, sélectionnez le dernier kit SDK. Si la boîte de dialogue **Mode Développeur** s’affiche, cliquez sur **OK**. Une nouvelle application vide est créée.

2. Configurez votre environnement de développement pour le débogage à distance :

   a. Cliquez avec le bouton droit sur le projet dans **Explorateur de solutions**, puis sélectionnez **Propriétés**.

   b. Dans l’onglet **Débogage**, définissez **Débogueur au lancement** sur **Ordinateur distant**.

   c. Dans **Nom de l’ordinateur**, entrez l’adresse IP du système ou le nom d’hôte de la console Xbox One. Pour plus d’informations sur l’obtention de l’adresse IP ou du nom d’hôte, consultez [Présentation des outils Xbox One](introduction-to-xbox-tools.md).

   d. Dans la liste déroulante **Type d’authentification**, sélectionnez **Universel (protocole non chiffré)** .

   e. Dans le menu déroulant **Plateforme**, sélectionnez **x64**.

    ![Pages de propriétés BlankApp C++](images/development-environment-setup-4.png)

### <a name="pin-pair-your-device-with-visual-studio"></a>Jumeler le code confidentiel de votre appareil avec Visual Studio

1. Enregistrez vos paramètres et assurez-vous que votre console Xbox One est en Mode développeur.

2. Une fois votre projet ouvert dans Visual Studio, appuyez sur la touche F5.

3. S’il s’agit de votre premier déploiement, vous obtiendrez une boîte de dialogue issue de Visual Studio vous demandant d’épingler par paire votre appareil.

    a. Pour obtenir un code confidentiel, ouvrez **Accueil Dev** à partir de l’écran d’accueil de votre console Xbox One.

    b. Dans l'onglet **Accueil**, sous **Actions rapides**, sélectionnez **Afficher le code confidentiel Visual Studio**.
  
    ![Boîte de dialogue Jumeler avec Visual Studio](images/development-environment-setup-5.png)

    c. Entrez votre code confidentiel dans la boîte de dialogue **Jumeler avec Visual Studio**. Le code confidentiel suivant est juste un exemple. Le vôtre sera différent.

    ![Boîte de dialogue du code confidentiel Jumeler avec Visual Studio](images/devhome_pin.png)

    d. Le cas échéant, des erreurs de déploiement s’afficheront dans la fenêtre **Sortie**.

Félicitations, vous avez correctement créé et déployé votre première application UWP sur Xbox !

## <a name="see-also"></a>Voir aussi
- [Activation du mode développeur Xbox One](devkit-activation.md)  
- [Téléchargements et outils pour Windows 10](https://developer.microsoft.com/windows/downloads)  
- [Programme Windows Insider](https://go.microsoft.com/fwlink/?LinkId=780552)  
- [Introduction aux outils de Xbox One](introduction-to-xbox-tools.md) 
- [UWP sur Xbox One](index.md)