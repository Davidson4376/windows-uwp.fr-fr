---
author: Mtoepke
title: Activation du Mode développeur XboxOne
description: Procédure d’activation du mode développeur de façon à pouvoir basculer du mode commercial vers le mode développeur et inversement.
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: ade80769-17ae-46e9-9c2f-bf08ae5a51ee
ms.localizationpriority: medium
ms.openlocfilehash: 730c345fe1746bf3284f9c0ce2c9bbeaa7ab0501
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4309059"
---
# <a name="xbox-one-developer-mode-activation"></a>Activation du mode développeur XboxOne

## <a name="how-developer-mode-works"></a>Fonctionnement du Mode développeur
La console Xbox One possède deux modes : le mode *Commercial* (**1**) et le mode *Développeur* (**2**). En Mode commercial, la console permet à n’importe quel utilisateur de jouer et d’exécuter des applications en tant qu’utilisateur. Le Mode développeur, vous permet de développer des logiciels pour la console, mais pas de jouer à des jeux commerciaux ou d’exécuter des applications commerciales.

Ce mode peut être activé sur n’importe quelle console Xbox One commerciale. Une fois le Mode développeur activé, vous pouvez basculer entre le mode commercial (**2a**) et le mode développeur (**2b**).

![Modes XboxOne](images/dev-mode-flow.png)

## <a name="activate-developer-mode-on-your-retail-xbox-one-console"></a>Activer le Mode développeur sur votre console Xbox One commerciale

1.  Démarrez votre console Xbox One.

2.  Recherchez l’application **Dev Mode Activation** dans le Marché Xbox One et installez-la.

    ![Installer l’application Dev Mode Activation](images/devkit-activation-1.png)

3.  Lancez l’application à partir de la page du Store.

    ![Application Dev Mode Activation](images/devkit-activation-2.png)

4.  Notez le code affiché dans l’application Dev Mode Activation.

    ![Activation - Étape5](images/activation-step-5.png)  
    
5.  Accédez à [partner.microsoft.com/xboxactivate](https://partner.microsoft.com/xboxactivate).

6.  Connectez-vous au Centre de développement à l’aide du compte correspondant.

7.  Entrez le code d’activation affiché dans l’application Dev Mode Activation. Vous disposez d’un nombre limité d’activations associées à votre compte. Une fois le Mode développeur activé, le Centre de développement indique que vous avez utilisé une des activations associées à votre compte.

    ![Activation - Étape8](images/activation-step-8-rs2.png)    
    
8.  Cliquez sur **Accepter et activer**. La page sera rechargée et votre appareil apparaîtra dans le tableau. Pour plus d’informations, voir [Programme d’activation du mode développeur Xbox One](http://go.microsoft.com/fwlink/p/?LinkId=760399).

9.  Après avoir saisi votre code d’activation, votre console affiche un écran de progression pour le processus d’activation.  
    
10. Une fois l’activation est terminée, ouvrez l’application Dev Mode Activation, puis cliquez sur **Basculer et redémarrer** pour accéder au Mode développeur. Notez que cette opération peut être plus longue que d’habitude.

    ![Activation - Étape12](images/activation-step-12.png)   

## <a name="switch-between-retail-and-developer-mode"></a>Basculer entre le Mode commercial et développeur
Une fois le Mode développeur activé sur votre console, utilisez **Dev Home** pour basculer entre le Mode commercial et développeur. Pour en savoir plus sur le démarrage et l’utilisation de Dev Home, consultez [Présentation des outils XboxOne](introduction-to-xbox-tools.md).

* Pour basculer en Mode commercial, ouvrez **Accueil du développeur**. Sous **Actions rapides**, sélectionnez **Quitter le mode développeur**. Cette action a pour effet de redémarrer votre console en Mode commercial.    

  ![Activation - Étape13](images/activation-step-13-rs4.png)  
  
* Pour basculer vers le Mode développeur, utilisez l’application Dev Mode Activation. Ouvrez l’application et sélectionnez **Basculer et redémarrer**. Cette action a pour effet de redémarrer votre console en Mode développeur.  

  ![Activation - Étape14](images/activation-step-12.png)  

## <a name="see-also"></a>Voir aussi
- [Désactivation du Mode développeur XboxOne](devkit-deactivation.md)
- [UWP sur XboxOne](index.md)
