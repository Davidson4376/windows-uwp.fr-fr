---
Description: If your developer account has been granted the appropriate permissions, you can generate and download preinstall packages so that an OEM can include your app in their OS image.
title: Générer des packages de préinstallation pour les fabricants d’ordinateurs OEM
ms.assetid: AC3A45E8-7BBD-44E9-B2D3-B74B7C9B2BC9
ms.date: 10/31/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1ab17adc80a643c04ac7793945486c3ff975fde5
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7826435"
---
# <a name="generate-preinstall-packages-for-oems"></a>Générer des packages de préinstallation pour les fabricants d’ordinateurs OEM

Si votre compte de développeur a reçu les autorisations appropriées, vous pouvez générer et télécharger des packages de préinstallation pour permettre à un fabricant d’ordinateurs OEM d’inclure votre application dans son image de système d’exploitation. Les autorisations de préinstallation sont uniquement activées sur les comptes de développeur qui sont sponsorisés par des OEM.


## <a name="important-preinstall-policy--limitations"></a>Politique et limitations importantes en matière de préinstallation

Les applications de préinstallation doivent être certifiées par le biais de [L’espace partenaires](https://partner.microsoft.com/dashboard) pour que la dernière licence du Windows Store afin qu’ils soient en mesure de se connecter au Windows Store et recevoir des mises à jour de l’application.

Toute application préinstallée doit être et rester gratuite dans tous les marchés.


## <a name="generating-preinstall-packages"></a>Génération de packages de préinstallation

Une fois qu'un compte a été activé avec des autorisations de préinstallation, suivez la procédure ci-après :

1.  Dans l’espace partenaires, accédez à l’application à préinstaller.
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

 

 




