---
author: jnHs
Description: If your developer account has been granted the appropriate permissions, you can generate and download preinstall packages so that an OEM can include your app in their OS image.
title: Générer des packages de préinstallation pour les fabricants d’ordinateurs OEM
ms.assetid: AC3A45E8-7BBD-44E9-B2D3-B74B7C9B2BC9
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 598a73b291d5f8b3c004f1e9adeddf0b92b841ab
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "2857142"
---
# <a name="generate-preinstall-packages-for-oems"></a>Générer des packages de préinstallation pour les fabricants d’ordinateurs OEM

Si votre compte de développeur a reçu les autorisations appropriées, vous pouvez générer et télécharger des packages de préinstallation pour permettre à un fabricant d’ordinateurs OEM d’inclure votre application dans son image de système d’exploitation. Les autorisations de préinstallation sont uniquement activées sur les comptes de développeur qui sont sponsorisés par des OEM.


## <a name="important-preinstall-policy--limitations"></a>Politique et limitations importantes en matière de préinstallation

Les applications de préinstallation doivent être certifiées par le biais du Centre de développement Windows comme disposant de la dernière licence du Windows Store pour être en mesure de se connecter au Windows Store et de recevoir les mises à jour d'application.

Toute application préinstallée doit être et rester gratuite dans tous les marchés.


## <a name="generating-preinstall-packages"></a>Génération de packages de préinstallation

Une fois qu'un compte a été activé avec des autorisations de préinstallation, suivez la procédure ci-après :

1.  Dans votre tableau de bord, accédez à l'application à préinstaller.
2.  Dans le menu de navigation de gauche, développez l’option **Gestion des applications**, puis sélectionnez **Packages actuels**.
3.  Dans la section **Demander des packages pour la préinstallation du système d’exploitation**, sélectionnez **Activer les packages téléchargeables**.
4.  Dans la boîte de dialogue de confirmation, sélectionnez **Activer**.
5.  Recherchez le package à télécharger, puis sélectionnez le lien **Générer le package** approprié.

    > [!NOTE]
    > Le temps nécessaire à la génération des packages de préinstallation varie en fonction de la taille du package que vous avez sélectionné. Vous pouvez quitter cette page et y revenir par la suite, ou vous pouvez laisser la page ouverte pendant la génération de votre package.

6.  Une fois le package généré, un lien **Télécharger le package** s’affiche. Sélectionnez ce lien pour télécharger le fichier.zip.

Vous pouvez alors fournir le fichier.zip à l’OEM pour qu’il puisse l’inclure dans son image de système d’exploitation.


## <a name="support"></a>Support

Si vous avez des questions sur la génération de packages de pré-installation, envoyez un e-mail à l’adresse <partnerops@microsoft.com>.

 

 




