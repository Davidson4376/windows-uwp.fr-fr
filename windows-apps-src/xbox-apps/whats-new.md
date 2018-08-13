---
author: v-angraf
title: Nouveautés de la plateformeUWP sur Xbox One
description: Cette rubrique présente les nouvelles fonctionnalités des applications UWP sur XboxOne.
ms.author: v-angraf
ms.date: 03/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: cbabe9d31b5b9762320df8e4a92d19ae4e33497d
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "301459"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Nouveautés de la dernière mise à jour de la plateforme UWP sur Xbox One pour les développeurs

La dernière mise à jour d’universels Windows plateforme (UWP) sur un seul Xbox contient les nouvelles fonctionnalités suivantes, mises à jour des fonctionnalités existantes et des correctifs.

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>x86 jeux et applications n’est plus pris en charge sur Xbox  
Xbox ne prend plus en charge le développement d’applications x86 ni les soumissions d’applications x86 dans le Store.

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>Les applications peuvent désormais gérer à partir de l’application précédente 
UWP sur une Xbox applications peut désormais gérer à partir de l’application précédente. Pour ce faire, vous abonner à l’événement [**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893595) et définissez la propriété **Handled** sur **false** dans le Gestionnaire d’événements.

> [!NOTE]
> Pour des raisons de compatibilité, cette fonctionnalité est disponible uniquement pour les applications qui sont créées avec la version la plus récente de UWP sur une Xbox. 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>Accueil du centre de développement est désormais l’expérience d’accueil par défaut sur les consoles de développement
Consoles de développement lancement maintenant d’accueil du centre de développement en tant que l’expérience d’accueil par défaut. Cela vous permet de mettre immédiatement en sans avoir besoin de clic dans l’écran d’accueil de vente au détail. Accueil du centre de développement inclut désormais une action rapide pour lancer l’écran d’accueil de vente au détail. En outre, un nouveau paramètre vous permet d’accueil au détail l’expérience par défaut. 

## <a name="new-dev-home-user-interface"></a>Nouvelle interface d’accueil pour les développeurs
L’interface utilisateur d’accueil du centre de développement inclut désormais les améliorations de la productivité suivantes:
 - Données importantes comme adresse IP et version de récupération est désormais affichée en haut de l’écran de visibilité. 
 - Accueil du centre de développement a maintenant une interface utilisateur à onglets qui regroupe les outils dans des jeux de logique permettant la navigation rapide.
 - Action rapide sur le premier onglet de développement d’origine permettent d’accéder rapidement aux actions couramment utilisées. 

## <a name="wdp-for-xbox-enhancements"></a>WDP pour Xbox améliorations
Le portail de périphérique Windows (WDP) inclut désormais la prise en charge supplémentaire pour les paramètres de la console. 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>Vous pouvez changer le type de votre titre UWP entre «Application» et «Jeu»
Commutation le type de votre titre UWP entre «Application» et «Jeu» vous permet de tester des scénarios jeu sans le publier dans le magasin. À domicile pour les développeurs, sélectionnez l’application dans le volet **jeux et applications** , appuyez sur le bouton Affichage sur le contrôleur de, sélectionnez les **Détails de l’application** et choisissez le type «Application» ou «Jeu».

## <a name="see-also"></a>Articles associés
- [Problèmes connus](known-issues.md)
- [UWP sur XboxOne](index.md)
 - Vous pouvez désormais effectuer une capture d’écran de la console. Pour plus d’informations sur la réalisation d’une capture d’écran, voir la rubrique de référence [/ext/screenshot](wdp-media-capture-api.md).
 - L’outil peut déployer une build de fichier isolé de votre application. Pour plus d’informations sur les builds de fichier isolé, consultez la rubrique de référence [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Il est possible d’accéder aux fichiers de développement sur votre console via l’Explorateur de fichiers sur votre ordinateur de développement. Pour plus d’informations sur l’accès aux fichiers via l’Explorateur de fichiers, voir la rubrique de référence [/ext/smb/developerfolder](wdp-smb-api.md).

## <a name="see-also"></a>Voir également
- [Problèmes connus](known-issues.md)
- [UWP sur XboxOne](index.md)
