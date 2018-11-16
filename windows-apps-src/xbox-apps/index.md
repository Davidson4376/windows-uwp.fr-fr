---
author: Mtoepke
title: UWP sur XboxOne
description: Procédure de création d’applications pour la plateforme Windows universelle (UWP) sur XboxOne.
ms.author: mstahl
ms.date: 10/25/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: 2d935f53-84db-4108-86dc-cb6a0749782f
ms.localizationpriority: medium
ms.openlocfilehash: e7c14c51c0b5b09b96859a8aba97172485a14c85
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6838207"
---
# <a name="uwp-on-xbox-one"></a>UWP sur XboxOne

Prise en main de la création d’applications pour la plateforme Windows universelle (UWP) sur XboxOne.

UWP sur XboxOne prend en charge le développement d’applications et de jeux. Il n’est pas nécessaire de faire partie d’un programme de développeurs pour essayer, créer et tester des jeux ou des applications sur Xbox. Il vous suffit d’un [compte de développeur](https://developer.microsoft.com/en-us/store/register) dans [L’espace partenaires](https://partner.microsoft.com/dashboard). Lorsque vous êtes prêt à publier et à vendre des jeux sur XboxOne ou à profiter de XboxLive sur Windows10, vous devez intégrer le [Programme Créateurs Xbox Live](https://developer.microsoft.com/games/xbox/xboxlive/creator) ou présenter le statut de développeur [ID@Xbox](http://www.xbox.com/Developers/id). Si vous prévoyez d’être un développeur ID@Xbox, nous vous recommandons d’adhérer dans un premier temps au programme avant de vous inscrire à un compte de développeur. Pour plus d’informations, voir la page [Vue d’ensemble du programme pour les développeurs](../xbox-live/developer-program-overview.md).

Cette section comprend la procédure de configuration, un guide détaillant le processus d’authentification, des informations sur l’installation des versions requises de Visual Studio et des outils Windows 10, et la procédure de création, d’exécution et de débogage de votre première application simple. 

| Rubrique      | Description |
|------------|-------------|
|[Mise en route](getting-started.md)| Guide de prise en main pour le développement d’applications UWP sur XboxOne. |
|[Nouveautés](whats-new.md)| Cette rubrique présente les nouvelles fonctionnalités de la plateformeUWP sur XboxOne. |
|[Activation du mode Développeur XboxOne](devkit-activation.md)| Explique comment activer le Mode développeur sur XboxOne. |
|[Désactivation du Mode développeur sur Xbox One](devkit-deactivation.md)| Explique comment désactiver le Mode développeur sur XboxOne. |
|[Configurer votre plateforme UWP sur l’environnement de développement Xbox](development-environment-setup.md)| Décrit la procédure de configuration et de test de votre environnement de développement Xbox One. |
|[Exemples](samples.md)| Pointeur vers l’emplacement GitHub (TVHelpers) où vous trouverez des exemples utiles de codes XAML et JavaScript pour prendre en main le développement pour Xbox. Les exemples comprennent un modèle complet d’application multimédia XAML ainsi que la navigation automatique par manette, la lecture de contenus multimédias riches et la recherche de technologies basées sur le web. |
|[Problèmes connus](known-issues.md)| Problèmes connus avec UWP sur XboxOne. |
|[FAQ](frequently-asked-questions.md)| FAQ relatif à UWP sur Xbox One. |
|[Outils](introduction-to-xbox-tools.md)| Décrit l’outil XboxOne _Accueil du développeur_, l’utilisation de Windows Device Portal et la configuration de Visual Studio pour le développement. En outre, cette section guide les nouveaux développeurs pour leur première application UWP Xbox, et leur explique comment utiliser l’outil Fiddler pour afficher le trafic réseau. |
| [Événement de développement d’applications sur Xbox](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event) | L'événement de développement d’applications sur Xbox est un excellent point de départ pour les nouveaux développeurs qui veulent créer des applications sur Xbox. Regardez les sessions enregistrées et lisez les billets de blog de l’événement. |
|[Conception pour Xbox et la télévision](../design/devices/designing-for-tv.md)| Décrit les bonnes pratiques de conception d’une application qui sera affichée sur un téléviseur et utilisera une manette pour les entrées. |
|[Meilleures pratiques pour Xbox](tailoring-for-xbox.md)| Comment désactiver le mode souris, dessiner sur les bords de l’écran et désactiver la mise à l’échelle. |
|[Utilisation de la reconnaissance vocale pour appeler les éléments d’interface utilisateur](ves-on-xbox.md)| Décrit les meilleures pratiques pour la prise en charge de Voice Enabled Shell dans les applications UWP sur Xbox. |
|[Ressources système pour les applications UWP et les jeux sur XboxOne](system-resource-allocation.md)| Décrit les ressources disponibles pour votre application quand elle s’exécute sur Xbox One. |
|[Présentation des applications multi-utilisateurs](multi-user-applications.md)| Décrit les applications multi-utilisateurs sur XboxOne. |
| [Automatisation des tâches de développement Xbox One](https://github.com/Microsoft/WindowsDevicePortalWrapper/tree/v0.9.4) | Le projet WindowsDevicePortalWrapper sur GitHub fournit une bibliothèque qui vous permet d’automatiser les tâches de développement courantes telles que le déploiement ou le lancement d’une application. Le projet inclut un exemple, XboxWdpDriver.exe, qui montre comment utiliser les API pour les tâches courantes. |
|[Intégration de jeux existants dansXbox](development-lanes-landing.md)|Selon la technologie sur laquelle est basé votre jeu, nous pouvons vous fournir des instructions détaillées permettant d’accélérer le processus de publication de votre jeu sur Xbox à l’aide des UWP.|
|[Fonctionnalités UWP qui ne sont pas encore prises en charge sur Xbox One](http://go.microsoft.com/fwlink/p/?LinkId=760755)|  Décrit les fonctionnalités UWP qui ne sont pas encore entièrement fonctionnelles sur Xbox One.|

## <a name="videos"></a>Vidéos

Les discussions suivantes sur Channel9 sont une excellente source d’informations pour créer des applications sur Xbox:

* [Building Great Universal Windows Platform (UWP) Apps for Xbox](https://channel9.msdn.com/Events/Build/2016/B883)
* [Adapt Your App for Xbox One and TV](https://channel9.msdn.com/Events/Build/2016/T651-R1)
* [UWP Development 1: Building an Adaptive UI](https://channel9.msdn.com/Events/Build/2016/L724-R1)
* [Web Apps Beyond the Browser: Cross-Platform Meets Cross Device](https://channel9.msdn.com/Events/Build/2016/B888)

## <a name="see-also"></a>Articles associés

- [Automatisation du lancement des applications UWP Windows10](automate-launching-uwp-apps.md)
- [API CPUSets pour le développement de jeux](cpusets-games.md)