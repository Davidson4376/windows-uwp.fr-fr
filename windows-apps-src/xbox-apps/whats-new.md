---
title: Nouveautés de la plateforme UWP sur Xbox One
description: Cette rubrique présente les nouvelles fonctionnalités des applications UWP sur Xbox One.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: aff65e5f1b4771cbb33bc8b8219224042b7bf7e2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660684"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Nouveautés de la dernière mise à jour de la plateforme UWP sur Xbox One pour les développeurs

La dernière version de la plateforme Windows universelle (UWP) sur Xbox One contient les nouvelles fonctionnalités, mises à jour de fonctionnalités et résolutions de bogues suivantes.

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>Les jeux et applications x86 ne sont plus pris en charge sur Xbox  
Xbox ne prend plus en charge le développement d’applications x86 ni les soumissions d’applications x86 dans le Store.

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>Les applications prennent désormais en charge la navigation vers l’application précédente 
Les applications UWP sur Xbox One prennent désormais en charge la navigation vers l’application précédente. Pour ce faire, abonnez-vous à l’événement [**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893595) et réglez la propriété **Handled** sur **false** (faux) dans votre gestionnaire d’événements.

> [!NOTE]
> Pour des raisons de compatibilité, cette fonctionnalité n’est disponible que pour les applications créées avec la version la plus récente d’UWP sur Xbox One. 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>Accueil du développeur (Dev Home) constitue dorénavant l’accueil par défaut sur les consoles de développement
Les consoles de développement chargent désormais l’Accueil du développeur (Dev Home) par défaut. Ainsi, vous accédez directement à la zone de travail en évitant une série fastidieuse de clics à partir de l’écran d’accueil de la vente au détail. L’Accueil du développeur intègre maintenant une action rapide permettant de lancer l’écran d’accueil de la vente au détail. En outre, un nouveau paramètre vous permet de charger par défaut l’écran d’accueil de la vente au détail. 

## <a name="new-dev-home-user-interface"></a>Nouvelle interface utilisateur Accueil du développeur
L’interface utilisateur Accueil du développeur comprend désormais les améliorations de productivité suivantes :
 - Les données importantes telles que l’adresse IP et la version de récupération s’affichent dorénavant en haut de l’écran pour une meilleure visibilité. 
 - L’Accueil du développeur dispose maintenant d’une interface utilisateur à onglets regroupant les outils par ensembles logiques, ce qui accélère la navigation.
 - Les boutons d’action rapide du premier onglet de l’Accueil du développeur permettent d’accéder rapidement aux actions les plus courantes. 

## <a name="wdp-for-xbox-enhancements"></a>Améliorations de WDP pour Xbox
Windows Device Portal (WDP) intègre désormais la prise en charge de paramètres supplémentaires de la console. 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>Vous pouvez maintenant commuter le type de votre titre UWP entre « Application » et « Jeu »
Le fait de commuter le type de votre titre UWP entre « Application » et « Jeu » permet de tester les scénarios du jeu sans avoir à le publier dans le Windows Store. Dans l’Accueil du développeur, sélectionnez l’application dans le volet **Jeux et applications**, appuyez sur le bouton Affichage du contrôleur, sélectionnez **Détails de l’application** et choisissez le type « Application » ou « Jeu ».

## <a name="see-also"></a>Voir également
- [Problèmes connus](known-issues.md)
- [UWP sur Xbox One](index.md)
 - Vous pouvez désormais effectuer une capture d’écran de la console. Pour plus d’informations sur la réalisation d’une capture d’écran, voir la rubrique de référence [/ext/screenshot](wdp-media-capture-api.md).
 - L’outil peut déployer une build de fichier isolé de votre application. Pour plus d’informations sur les builds de fichier isolé, consultez la rubrique de référence [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Il est possible d’accéder aux fichiers de développement sur votre console via l’Explorateur de fichiers sur votre ordinateur de développement. Pour plus d’informations sur l’accès aux fichiers via l’Explorateur de fichiers, voir la rubrique de référence [/ext/smb/developerfolder](wdp-smb-api.md).

## <a name="see-also"></a>Voir également
- [Problèmes connus](known-issues.md)
- [UWP sur Xbox One](index.md)
