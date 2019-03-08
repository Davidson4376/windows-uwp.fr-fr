---
title: Créer et tester un nouveau titre de créateurs
description: Découvrez comment créer un nouveau titre de programme Xbox Live Creators et publier dans l’environnement de test.
ms.assetid: ced4d708-e8c0-4b69-aad0-e953bfdacbbf
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, créateurs, test
ms.localizationpriority: medium
ms.openlocfilehash: 5b39a2ed67d2fe77fa8904408b81a329125d73c7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594594"
---
# <a name="create-a-new-xbox-live-creators-program-title-and-publish-to-the-test-environment"></a>Créer un nouveau titre de programme Xbox Live Creators et publier dans l’environnement de test

## <a name="introduction"></a>Introduction

Avant d’écrire n’importe quel code Xbox Live, vous devez configurer un nouveau titre sur votre portail de configuration de service.  Vous trouverez plus d’informations sur la configuration de service dans [Configuration du Service Xbox Live](../xbox-live-service-configuration.md).

Cet article vous aidera à tous les éléments requis pour obtenir un titre configuré dans [partenaires](https://partner.microsoft.com/dashboard), un nouveau projet créé et la préparation de la Xbox Live pour le test. Cet article suppose que les éléments suivants :

1. Vous utilisez le programme Xbox Live Creators.
2. Vous développez un titre de la plateforme universelle Windows (UWP).  Les titres UWP peuvent s’exécuter sur Xbox One, ordinateurs de bureau Windows 10 et mobile
3. Vous configurez votre titre dans [partenaires](https://partner.microsoft.com/dashboard).
4. Votre ordinateur de développement est en cours d’exécution Windows 10.

> [!NOTE]
> Si vous faites partie du programme Xbox Live Creators, les hypothèses ci-dessus s’appliquent à vous et vous devez suivre cet article

## <a name="partner-center-setup"></a>Programme d’installation de partenaires

Vous avez besoin d’un titre Xbox Live activé est créé dans [partenaires](https://partner.microsoft.com/dashboard) comme condition préalable à l’utilisation des fonctionnalités Xbox Live.

### <a name="create-a-microsoft-account"></a>Créer un compte Microsoft
Si vous n’avez pas un Account Microsoft (MSA), vous devez d’abord créer un à [Account Microsoft - signe dans](https://go.microsoft.com/fwlink/p/?LinkID=254486). Si vous avez un compte Office 365, utilisez Outlook.com ou avoir un compte Xbox Live - vous avez probablement déjà un compte de service administré.

### <a name="register-as-an-app-developer"></a>Inscrire en tant que développeur d’applications
Vous devrez inscrire en tant que développeur d’applications avant de pouvoir créer un nouveau titre de partenaires.

Pour vous inscrire, accédez à [inscrire en tant que développeur d’applications](https://developer.microsoft.com/store/register) et suivez le processus d’inscription.

### <a name="create-a-new-uwp-title"></a>Créer un nouveau titre UWP
Vous aurez besoin d’un titre UWP défini dans l’espace partenaires. Pour ce faire, allez dans [partenaires](https://partner.microsoft.com/dashboard).

Ensuite, créez un nouveau titre. Vous devez réserver un nom.

![](../images/getting_started/first_xbltitle_newapp.png)

La capture d’écran montre la page principale dans laquelle vous allez configurer Xbox Live, situé sous le **Services** > **Xbox Live** menu.

![](../images/creators_udc/creators_udc_xboxlive_page.png)

## <a name="enable-xbox-live-services"></a>Activer les services Xbox Live
Lorsque vous cliquez sur le **Xbox Live** situé sous **Services** pour la première fois pour un produit, vous êtes redirigé vers la page Activer le programme Creators Live Xbox.  Ensuite, cliquez sur le **activer** bouton pour afficher la boîte de dialogue du programme d’installation de Xbox Live.

![](../images/creators_udc/creators_udc_xboxlive_enable.png)

Dans la boîte de dialogue d’installation, sélectionnez les plateformes que vous souhaitez activer les Services Live Xbox pour (Xbox One et PC Windows 10 sont sélectionnées par défaut).  Cliquez sur le **confirmer** bouton pour activer le programme Xbox Live Creators pour votre jeu.

> [!IMPORTANT]
> Xbox Live est uniquement pris en charge pour les jeux. Pour pouvoir publier votre jeu avec Xbox Live, vous devez définir le type de votre produit pour « Game » dans la section Propriétés de l’envoi.

![](../images/creators_udc/creators_udc_xboxlive_enable_dialog.png)

## <a name="test-xbox-live-service-configuration-in-your-game"></a>Tester la configuration du service Xbox Live dans votre jeu
Lorsque vous apportez des modifications à la configuration de Xbox Live à votre jeu, vous devez publier les modifications dans un environnement spécifique avant qu’ils sont récupérés par le reste de la Xbox Live et peuvent être vus par votre jeu.

Lorsque vous travaillez toujours sur votre jeu, vous publiez dans votre propre bac à sable de développement.  Le bac à sable de développement vous permet de travailler sur les modifications apportées à votre jeu dans un environnement isolé.

Quand votre jeu est publié pour le grand public, la configuration de Xbox Live sera automatiquement publiée pour le bac à sable de vente au détail.

Par défaut, les Consoles une Xbox et PC Windows 10 sont dans le bac à sable de vente au détail.

### <a name="publish-xbox-live-configuration-to-the-test-environment"></a>Publier la Configuration de Xbox Live à l’environnement de test

Chaque fois que vous activez les services Xbox Live et apportez des modifications à la configuration du service Xbox Live, vous devez publier ces modifications à votre sandbox de développement pour appliquer les modifications.

Dans la page de configuration Xbox Live, cliquez sur le **Test** bouton Publier la configuration actuelle de Xbox Live à votre sandbox de développement.

![](../images/creators_udc/creators_udc_xboxlive_config_test.png)

### <a name="authorize-devices-and-users-for-the-development-sandbox"></a>Autoriser les utilisateurs pour le bac à sable de développement et les appareils

Seuls les appareils autorisés et les utilisateurs peuvent accéder à la configuration de Xbox Live pour le jeu à votre sandbox de développement.

Par défaut, toutes les consoles de développement Xbox One que vous avez ajouté à votre compte espace partenaires ont accès à votre sandbox de développement.  Pour ajouter une console Xbox One, accédez à [consoles gérer Xbox One](https://partner.microsoft.com/xboxconfig/devices). Si vous êtes déjà dans votre compte espace partenaires, vous pouvez accéder à **comptable** > **comptable** > **Dev appareils**  >  **Consoles de développement Xbox One**.

Vous pouvez également autoriser des comptes normaux Xbox Live à accéder à votre sandbox de développement.  Pour autoriser l’accès de comptes Xbox Live à votre sandbox de développement, accédez à [gérer les comptes](https://developer.microsoft.com/xboxtestaccounts/configurecreators).

## <a name="next-steps"></a>Étapes suivantes
Maintenant que vous avez un nouveau titre créé, vous pouvez désormais configurer un titre Xbox Live est activé dans votre moteur de jeu, Visual Studio ou votre environnement de génération de choix.

Consultez [guide étape par étape pour intégrer la Xbox Live](creators-step-by-step-guide.md)
