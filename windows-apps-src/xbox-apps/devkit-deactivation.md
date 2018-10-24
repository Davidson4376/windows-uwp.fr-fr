---
author: Mtoepke
title: Désactivation du Mode développeur XboxOne
description: Procédure de désactivation du mode développeur.
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 244124dd-d80a-4a72-91db-1c9c2fbc7c3c
ms.localizationpriority: medium
ms.openlocfilehash: d1df8d475ec27ff3de7e3d6599e53ec3035cffc6
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/23/2018
ms.locfileid: "5431082"
---
# <a name="xbox-one-developer-mode-deactivation"></a>Désactivation du mode développeur XboxOne

Si vous décidez de ne plus utiliser votre console pour le développement, procédez comme suit pour désactiver le Mode développeur.

## <a name="switch-to-retail-mode"></a>Basculer en Mode commercial

Tout d’abord, basculez de nouveau votre console Xbox One en Mode commercial.

1. Ouvrez **Accueil développeur**.

2. Sélectionnez **Quitter le mode développeur**.  Votre console redémarre en Mode commercial.  

   ![Fermeture du Mode développeur](images/devkit-deactivation-1.png)

Maintenant, désactivez votre console à l’aide de l’une des méthodes suivantes.

## <a name="deactivate-your-console-using-the-dev-mode-activation-app"></a>Désactiver votre console à l’aide de l’application Dev Mode Activation

La méthode recommandée pour désactiver le Mode développeur sur votre console consiste à utiliser l’application **Dev Mode Activation**. 

1. Accédez à **Jeux et applications** > **Applications**.
  
   ![Activation - Étape3](images/devkit-deactivation-5.png)    
   
2.  Ouvrez l’application Dev Mode Activation.

3.  Sélectionnez **Désactiver**.
  
    ![Désactiver la console](images/deactivation-app.png)

Consultez [Activation du mode développeur Xbox One](devkit-activation.md) pour plus d’informations sur l'application **Dev Mode Activation**. 

## <a name="reset-your-console"></a>Réinitialiser votre console

Vous pouvez également désactiver le Mode développeur en réinitialisant votre console.  

> [!NOTE]
> La réinitialisation de votre console entraîne la perte de toutes les données de jeu enregistrées localement.

Pour réinitialiser votre console procédez comme suit:

1.  Accédez à **Mes jeux et applications**.

2.  Sélectionnez **Applications**, puis choisissez **Paramètres**.

3.  Accédez à **Système** dans le volet gauche, puis sélectionnez **Informations de la console** dans le volet de droite.   
   
    ![Informations de la console et mises à jour](images/devkit-deactivation-2.png)  
    
4.  Sélectionnez **Redémarrer la console**.
    
    ![Réinitialiser la console](images/devkit-deactivation-3.png)
    
5.  Ensuite, sélectionnez **Réinitialiser et supprimer tous les éléments**. Cette option rétablit la console dans sa version commerciale d’origine.  L’ensemble de vos applications, jeux et de vos données enregistrées localement seront supprimés. Notez que si vous choisissez l’option **Réinitialiser et conserver mes jeux et applications**, votre console ne sera pas supprimée du programme de développement.  
   
    ![Réinitialiser et supprimer tous les éléments](images/devkit-deactivation-4.png)

## <a name="deactivate-your-console-using-windows-dev-center"></a>Désactiver votre console à l’aide du Centre de développement Windows

Si vous ne parvenez pas à accéder à votre console pour une raison quelconque, vous pouvez également désactiver le Mode développeur sur votre console à l’aide du Centre de développement Windows.

1. Accédez à la page [Gérer les consoles XboxOne](https://partner.microsoft.com/xboxdevices) du centre de développement. Vous êtes invité à vous connecter à votre compte du centre de développement.

2. Recherchez la console que vous voulez désactiver dans la liste des consoles à l’aide du numéro de série ou de l’ID de la console ou de l’appareil.  

3. Cliquez sur **Désactiver**.  
  
![Désactiver à l’aide de DevCenter](images/devkit-deactivation-6.png)

Si vous n’avez pas encore basculé votre console Xbox One en Mode commercial, faites-le maintenant tel que décrit dans [Basculer en mode commercial](#switch-to-retail-mode).

## <a name="see-also"></a>Articles associés
- [Activation du Mode développeur XboxOne](devkit-activation.md)
- [UWP sur XboxOne](index.md)
