---
author: jnHs
Description: "Si votre compte de développeur a reçu les autorisations appropriées, vous pouvez générer et télécharger des packages de préinstallation permettant à un fabricant d’ordinateurs OEM d’inclure votre application dans son image."
title: "Générer des packages de préinstallation pour les fabricants d’ordinateurs OEM"
ms.assetid: AC3A45E8-7BBD-44E9-B2D3-B74B7C9B2BC9
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: e040f0ea1f2106da2d7da76464d4c818f39d6735
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="generate-preinstall-packages-for-oems"></a>Générer des packages de préinstallation pour les fabricants d’ordinateurs OEM


Si votre compte de développeur a reçu les autorisations appropriées, vous pouvez générer et télécharger des packages de préinstallation permettant à un fabricant d'ordinateurs OEM d'inclure votre application dans son image. Les autorisations de préinstallation sont uniquement activées sur les comptes de développeur qui sont sponsorisés par des OEM.

## <a name="important-preinstall-policy--limitations"></a>Politique et limitations importantes en matière de préinstallation


Les applications de préinstallation doivent être certifiées par le biais du Centre de développement Windows comme disposant de la dernière licence du Windows Store pour être en mesure de se connecter au Windows Store et de recevoir les mises à jour d'application.

Toute application déjà préinstallée doit être et rester gratuite dans tous les marchés.

## <a name="generating-preinstall-packages"></a>Génération de packages de préinstallation


Une fois qu'un compte a été activé avec des autorisations de préinstallation, suivez la procédure ci-après :

1.  Dans votre tableau de bord, accédez à l'application à préinstaller.
2.  Dans le menu de navigation à gauche, développez l'option **Gestion des applications**, puis cliquez sur **Packages actuels**.
3.  Dans la section **Demander des packages pour la préinstallation du système d’exploitation**, cliquez sur **Activer les packages téléchargeables**.
4.  Cette opération affiche une boîte de dialogue de confirmation signalant que les applications préinstallées sur un système d’exploitation antérieur à Windows 10 doivent être gratuites. Sélectionnez **Activer**.
5.  Recherchez le package à télécharger, puis cliquez sur le lien **Générer le package** approprié.
    > **Remarque**  Le temps nécessaire à la génération des packages de préinstallation varie en fonction de la taille du package que vous avez sélectionné. Vous pouvez quitter cette page et y revenir par la suite, ou laisser la page ouverte.
6.  Une fois le package généré, un lien **Télécharger le package** s'affiche. Cliquez sur ce lien pour télécharger le fichier .zip.

Vous pouvez alors fournir ce fichier .zip à l'OEM pour qu'il puisse l'inclure dans son image de système d'exploitation.

## <a name="support"></a>Support technique


Si vous avez des questions sur la génération de packages de pré-installation, envoyez un e-mail à l’adresse <partnerops@microsoft.com>.

 

 




