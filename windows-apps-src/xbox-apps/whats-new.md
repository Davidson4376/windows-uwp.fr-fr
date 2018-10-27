---
author: v-angraf
title: Nouveautés de la plateformeUWP sur Xbox One
description: Cette rubrique présente les nouvelles fonctionnalités des applications UWP sur XboxOne.
ms.author: v-angraf
ms.date: 03/29/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: cc2168014e714de0b43b6ffffe84126764f0a4a3
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5697815"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Nouveautés de la dernière mise à jour de la plateforme UWP sur Xbox One pour les développeurs

La dernière mise à jour de plateforme Windows universelle (UWP) sur Xbox One contient les nouvelles fonctionnalités suivantes, mises à jour des fonctionnalités existantes et des résolutions de bogues.

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>x86 jeux et applications ne sont plus pris en charge sur Xbox  
Xbox ne prend plus en charge le développement d’applications x86 ni les soumissions d’applications x86 dans le Store.

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>Les applications peuvent prennent désormais en charge la navigation à l’application précédente 
UWP sur Xbox One applications permettre prennent désormais en charge la navigation à l’application précédente. Pour ce faire, vous abonner à l’événement [**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893595) et définissez la propriété **Handled** sur **false** dans votre gestionnaire d’événements.

> [!NOTE]
> Pour des raisons de compatibilité, cette fonctionnalité est disponible uniquement pour les applications qui sont créées avec la version la plus récente de UWP sur Xbox One. 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>Accueil du développeur est désormais l’expérience domestique par défaut sur les consoles de développement
Consoles de développement maintenant lancement accueil du développeur en tant que l’expérience domestique par défaut. Cela vous permet de réaliser correctement fonctionner sans avoir besoin de cliquer sur par le biais de l’écran d’accueil de vente au détail. Accueil du développeur inclut désormais une action rapide pour lancer l’écran d’accueil de vente au détail. En outre, un nouveau paramètre vous permet de faire de vente au détail à domicile l’expérience par défaut. 

## <a name="new-dev-home-user-interface"></a>Nouvelle interface utilisateur d’accueil du développeur
L’interface utilisateur de Dev Home inclut désormais des améliorations de productivité suivantes:
 - Données importantes telles que l’adresse IP et récupération version est désormais affichée en haut de l’écran de visibilité. 
 - Accueil du développeur dispose maintenant d’une interface utilisateur à onglets qui regroupe des outils dans des jeux de logique, ce qui permet une navigation rapide.
 - Boutons d’action rapide sur le premier onglet d’accueil du développeur autoriser un accès rapide aux actions plus couramment utilisées. 

## <a name="wdp-for-xbox-enhancements"></a>WDP des améliorations apportées à Xbox
Windows Device Portal (WDP) inclut désormais une prise en charge supplémentaire pour les paramètres de la console. 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>Vous pouvez maintenant basculer le type de votre titre UWP entre «Application» et «Game»
Commutation le type de votre titre UWP entre «Application» et «Jeu» vous permet de tester des scénarios de jeu sans le publier dans le Windows store. Dans accueil du développeur, sélectionnez l’application dans le volet de **jeux et applications** appuyez sur le bouton d’affichage sur le contrôleur, **Détails de l’application** et sélectionnez puis modifier le type «Application» ou «Jeu».

## <a name="see-also"></a>Voir aussi
- [Problèmes connus](known-issues.md)
- [UWP sur XboxOne](index.md)
 - Vous pouvez désormais effectuer une capture d’écran de la console. Pour plus d’informations sur la réalisation d’une capture d’écran, voir la rubrique de référence [/ext/screenshot](wdp-media-capture-api.md).
 - L’outil peut déployer une build de fichier isolé de votre application. Pour plus d’informations sur les builds de fichier isolé, consultez la rubrique de référence [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Il est possible d’accéder aux fichiers de développement sur votre console via l’Explorateur de fichiers sur votre ordinateur de développement. Pour plus d’informations sur l’accès aux fichiers via l’Explorateur de fichiers, voir la rubrique de référence [/ext/smb/developerfolder](wdp-smb-api.md).

## <a name="see-also"></a>Voir également
- [Problèmes connus](known-issues.md)
- [UWP sur XboxOne](index.md)
