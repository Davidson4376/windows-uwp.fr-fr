---
title: Restrictions des applications et expériences sur ARM
author: msatranjr
description: Étapes de résolution des problèmes de fonctionnement des applications sur ARM.
ms.author: misatran
ms.date: 02/15/2018
ms.topic: article
keywords: Windows10s, toujours connecté, restrictions, windows10 sur ARM
ms.localizationpriority: medium
redirect_url: https://docs.microsoft.com/en-us/windows/uwp/porting/apps-on-arm-troubleshooting-x86
ms.openlocfilehash: 24afc8a876b976f21d0f4ebd5892ceef7c403018
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6459494"
---
# <a name="limitations-of-apps-and-experiences-on-arm"></a>Restrictions des applications et expériences sur ARM
Windows10 sur ARM présente les restrictions nécessaires suivantes:

- **Seuls les pilotes ARM64 sont pris en charge**. Comme dans toutes les architectures, les pilotes en mode noyau, les pilotes [Infrastructure de pilote en mode utilisateur (UMDF)](https://docs.microsoft.com/en-us/windows-hardware/drivers/wdf/overview-of-the-umdf) et les pilotes d'impression doivent être compilés pour correspondre à l'architecture du système d'exploitation. Tandis que les systèmes d'exploitation ARM présentent des fonctionnalités permettant d'émuler les applications x86 en mode utilisateur, les pilotes implémentés pour les autres architectures (notamment x64 et x86) ne sont actuellement pas émulées et, par conséquent, elle ne sont pas prises en charge sur cette plateforme. Toute application fonctionnant avec son propre pilote personnalisé devra être transférée vers ARM64. Dans les scénarios limités, l'application peut s'exécuter en tant qu'application x86 sous émulation, mais la partie pilote de l'application doit être transférée sur ARM64. Pour plus d’informations sur la compilation de votre pilote pour ARM64, consultez [Génération de pilotes ARM64 avec le kit WDK](https://review.docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers?branch=rs4-arm64).

- **Les applications x64 ne sont pas prises en charge**. Windows10 sur ARM ne prend pas en charge l'émulation des applications x64.

- **Certains jeux ne fonctionnent pas**. Les jeux et applications utilisant une version d'OpenGL ultérieure à la version 1.1 ou nécessitant une version d'OpenGL à accélération matérielle ne fonctionnent pas. En outre, les jeux reposant sur des pilotes «anti-tricherie» ne sont pas pris en charge sur cette plateforme.

- **Il est probable que les applications permettant de personnaliser l’expérience Windows ne fonctionnent pas correctement**. Les composants natifs du système d'exploitation ne sont pas en mesure de charger les composants non natifs. Les éditeurs de méthode d'entrée (IME), les technologies d'assistance et les applications de stockage cloud font partie de ces exemples d'applications. Les IME et les technologies d'assistance s'attachent souvent à leur pile d'entrée pour la plupart de leurs fonctionnalités d'application. Les applications de stockage cloud utilisent communément les extensions d'environnement (par exemple, les icônes dans Explorer et les ajouts aux menus à clic droit); leurs extensions d'environnement peuvent échouer. Si l'échec n'est pas traité de manière approprié, l'application en elle-même ne fonctionnera probablement pas.

- **Les applications qui supposent que tous les appareils basés sur ARM exécutent une version mobile de Windows mobile peuvent ne pas fonctionnent correctement**. Les applications intégrant cette supposition peuvent apparaître dans la mauvaise orientation, présenter une disposition ou un rendu d'interface utilisateur inattendu ou complètement échouer à démarrer lorsqu'elles tente d'invoquer les API uniquement mobiles sans tester au préalable la disponibilité du contrat.

- **La plateforme WindowsHypervisor n'est pas prise en charge sur ARM**. L'exécution de toutes machines virtuelles à l'aide d'Hyper-V sur un appareil ARM ne fonctionnera pas.

Le tableau suivant répertorie la liste des problèmes communs et propose des suggestions de résolution.

|Problème|Solution|
|-----|--------|
| Votre application repose sur un pilote qui n'a pas été désigné pour un ARM. | Recompilez votre pilote x86 pour l'ARM64. Consultez [Génération de pilotes ARM64 avec le kit WDK](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers). |
| Votre application est uniquement disponible pour un système x64. | Si vous développez pour le MicrosoftStore, soumettez une version ARM de votre application. Pour plus d’informations, consultez [Architectures des packages d’applications](../packaging/device-architecture.md). Si vous êtes un développeur Win32, distribuez une version x86 de votre application. |
| Votre application utiliser une version de OpenGL ultérieure à la version 1.1 ou nécessite une version d'OpenGL à accélération matérielle. | Les applications x86 utilisant DirectX9, DirectX10, DirectX11 et DirectX12 fonctionneront sur ARM. Pour plus d’informations, consultez [Jeux et graphismes DirectX](https://msdn.microsoft.com/en-us/library/windows/desktop/ee663274(v=vs.85).aspx). |
| Votre application x86 ne fonctionne pas comme prévu. | Essayez d'utiliser l'utilitaire de résolution des problèmes de compatibilité en suivant les recommandations de l'[Utilitaire de résolution des problèmes de compatibilité sur ARM](apps-on-arm-program-compat-troubleshooter.md). Pour certaines étapes de résolution d'autres problèmes, consultez l'article [Résolution des problèmes des applications x86 sur ARM](apps-on-arm-troubleshooting-x86.md). |
| Votre application x86 ne détecte pas sa propre exécution sur ARM. | Utilisez [IsWow64Process2](https://msdn.microsoft.com/en-us/library/windows/desktop/mt804318(v=vs.85).aspx) afin de déterminer si votre application est exécutée sur ARM. |
| Votre application ARM32 UWP ne fonctionne pas comme prévu. | Consultez [Résolution des problèmes d'applications ARM32 sur ARM](apps-on-arm-troubleshooting-arm32.md) pour découvrir comment faire en sorte que votre application fonctionne correctement sur ARM. |
