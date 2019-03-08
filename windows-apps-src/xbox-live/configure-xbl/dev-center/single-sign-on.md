---
title: Configurer l’authentification unique dans l’espace partenaires
description: Décrit comment configurer l’authentification unique dans l’espace partenaires pour permettre un titre connecter un utilisateur dans vos services à l’aide de leur ID de Xbox Live.
ms.assetid: ''
ms.date: 02/21/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, udc, centre de développement universelle, l’authentification unique
ms.openlocfilehash: 32f06edd407d8c1fa74795d0a230c7d56ba8e838
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632874"
---
# <a name="configure-single-sign-on-in-partner-center"></a>Configurer l’authentification unique dans l’espace partenaires

L’authentification unique permet à un lecteur à l’aide de votre titre pour se connecter à vos services à l’aide de leurs Xbox Live connectez-vous. Cela permet à un lecteur qui est connecté à Xbox Live à exécuter une application ou un jeu pour votre service sans avoir à vous connecter une deuxième fois à l’aide de compte différentes informations d’identification spécifiques à votre service.

> [!NOTE]
> Cette rubrique ne s’applique pas aux titres de programme Xbox Live Creators.

Par exemple, votre titre peut être une application qui permet à votre service de diffuser du contenu (vidéos, musique, etc.) sur leur appareil, tant qu’ils disposent d’un compte valid avec votre service. Si l’utilisateur est connecté à son compte Xbox Live, ils doivent pouvoir diffuser du contenu sans avoir à se connecter à votre service chaque fois.

En outre, si votre application envoie des données de Kinect pour un service externe, vous pouvez configurer ici.

Si votre service nécessite que l’utilisateur crée un compte distinct de Xbox Live, vous devez fournir un moyen pour l’utilisateur à lier leur compte Xbox Live avec leur compte sur votre service en tant qu’une fois.

Lorsque vous configurez l’authentification unique, vous pouvez spécifier des URL et leur confiance. Chaque fois que votre application appelle une des URL spécifiées, Xbox Live joint automatiquement un jeton Xbox Secure Token Service (XSTS). Le service qui reçoit la clé est appelé un *confiance*, et vous devez configurer [parties de confiance](https://developer.microsoft.com/en-US/xboxconfig/relyingparties/index) avant de pouvoir configurer l’authentification unique. Chaque configuration de tiers de confiance spécifie quelles informations sont contenues dans le jeton XSTS, ainsi que d’une clé de chiffrement unique que la partie de confiance peut utiliser pour décoder le jeton XSTS.

Ajoutez la configuration en procédant comme suit :

1. Après avoir sélectionné votre titre dans [partenaires](https://partner.microsoft.com/dashboard), accédez à **Services** > **Xbox Live**.

2. Cliquez sur le lien vers **Xbox Live l’authentification unique sur**.

3. Cliquez sur le **ajouter une URL** bouton pour créer une nouvelle entrée de mise sous l’authentification unique. Cela ajoutera une nouvelle ligne au bas de la liste des configurations.

4. Dans la zone URL, entrez l’URL de votre service à l’aide d’un nom de domaine qualifié complet. Vous pouvez remplacer le sous-domaine de niveau le plus bas par un caractère générique ('\*'). Il correspond à n’importe quelle URL qui a les mêmes domaines de niveau supérieur. Par exemple, « *. example.com&quot; correspond à « bar.example.com » ou « foo.bar.example.com ».

5. Dans la zone de tiers de partie de confiance, sélectionnez la partie de confiance configuration du tiers qui spécifie la façon dont le jeton XSTS est encodé.

6. Cliquez sur le **enregistrer** bouton pour enregistrer vos modifications.

![Capture d’écran de la page de configuration de l’authentification unique](../../images/dev-center/single-signon.png)
