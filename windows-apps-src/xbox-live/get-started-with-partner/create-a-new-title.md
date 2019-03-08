---
title: Créer un nouveau titre
description: Découvrez comment créer un nouveau titre pour Xbox Live à l’aide de partenaires.
ms.assetid: b8bd69e6-887a-4b1f-a42d-8affdbec0234
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1aa2447a2044bec9b2013b30c05e45342b763fc3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656564"
---
# <a name="create-a-new-title-for-xbox-live"></a>Créer un nouveau titre pour Xbox Live

## <a name="introduction"></a>Introduction

Avant d’écrire de code, vous devez configurer un nouveau titre sur votre portail de configuration de service.  Vous trouverez plus d’informations sur la configuration de service dans [Configuration du Service Xbox Live](../xbox-live-service-configuration.md)

Cet article vous guidera dans ce processus avec les hypothèses suivantes

1. Vous développez un titre de la plateforme universelle Windows (UWP).  Les titres UWP s’exécutent sur Xbox One, ordinateurs de bureau Windows 10 et mobile
2. Vous configurez votre titre dans [partenaires](https://partner.microsoft.com/dashboard).
3. Vous utilisez Visual Studio avec un moteur de jeu personnalisé ou Unity.
4. Votre ordinateur de développement est en cours d’exécution Windows 10.

Si la méthode ci-dessus sont remplie, le reste de cet article guidera tous les éléments requis pour obtenir un titre configuré dans l’espace partenaires, un nouveau projet créé et Xbox Live connectez-vous code écrit et testé.

> [!NOTE]
> Si vous faites partie du programme Xbox Live Creators, les hypothèses ci-dessus s’appliquent à vous et vous devez suivre cet article.

## <a name="partner-center-setup"></a>Programme d’installation de partenaires

Vous avez besoin d’un titre Xbox Live activé est créé dans [partenaires](https://partner.microsoft.com/dashboard) préalable à n’importe quel travail fonctionnalités Xbox Live.

### <a name="create-a-microsoft-account"></a>Créer un compte Microsoft
Si vous n’avez pas un Account Microsoft (MSA), vous devez d’abord créer un à [ https://go.microsoft.com/fwlink/p/?LinkID=254486 ](https://go.microsoft.com/fwlink/p/?LinkID=254486).  Si vous avez un compte Office 365, utilisez Outlook.com ou avoir un compte Xbox Live - vous avez probablement déjà un compte de service administré.

### <a name="register-as-an-app-developer"></a>Inscrire en tant que développeur d’applications
Vous devrez inscrire en tant que développeur d’applications avant que vous êtes autorisé à créer un nouveau titre de partenaires.

Pour inscrire, accédez à https://developer.microsoft.com/en-us/store/register et suivez le processus d’inscription.

### <a name="create-a-new-uwp-title"></a>Créer un nouveau titre UWP
Ensuite, vous avez besoin d’un titre UWP défini dans l’espace partenaires.  Pour cela, allez au tableau de bord

![](../images/getting_started/first_xbltitle_dashboard.png)

<p>
</p>
<br>
<p>
</p>

Ensuite, créez un nouveau titre.  Vous devez réserver un nom.

![](../images/getting_started/first_xbltitle_newapp.png)

Vous allez ensuite dirigé vers le *vue d’ensemble de l’application* page de votre application.  La page principale dans laquelle vous allez configurer Xbox Live est sous les Services -> menu Xbox Live indiqué ci-dessous.

![](../images/getting_started/first_xbltitle_leftnav.png)

<div id="createxblaccount"></div>

## <a name="create-an-xbox-live-account"></a>Créer un compte Live Xbox
Vous devez un compte Xbox Live pour se connecter à Xbox Live.  Si vous avez déjà un que vous utilisez pour vous connecter sur votre console Xbox One ou dans l’application Xbox sur Windows 10, puis vous pouvez l’utiliser.

Sinon, vous devez ouvrir l’application Xbox sur votre ordinateur et connectez-vous avec votre Account Microsoft.  Il sera ensuite être activé pour une utilisation avec Xbox Live.

Vous pouvez trouver l’application Xbox en accédant à la *Menu Démarrer* et en tapant « Xbox » comme indiqué ci-dessous.

![](../images/getting_started/first_xbltitle_xboxapp.png)

## <a name="next-steps"></a>Étapes suivantes
Maintenant que vous avez un nouveau titre créé, vous pouvez désormais configurer un titre Xbox Live est activé dans votre moteur de jeu, Visual Studio ou votre environnement de génération de choix.

Consultez [guide étape par étape pour intégrer la Xbox Live](partners-step-by-step-guide.md)
