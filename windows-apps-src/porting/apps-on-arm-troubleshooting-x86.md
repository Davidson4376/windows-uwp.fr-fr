---
title: Résolution des problèmes relatifs aux applications de bureau x86
description: Problèmes courants avec les applications x86 lors de l'exécution sur ARM, et comment les résoudre.
ms.date: 05/09/2018
ms.topic: article
keywords: windows10 s, toujours connecté, émulation x86 sur ARM, résolution des problèmes
ms.localizationpriority: medium
ms.openlocfilehash: 396bb0bf2c5ba5236e0e46e7b474867ffacb8c75
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8209133"
---
# <a name="troubleshooting-x86-desktop-apps"></a>Résolution des problèmes relatifs aux applications de bureau x86
>[!IMPORTANT]
> Le SDK ARM64 est désormais disponible dans le cadre de Visual Studio15.8 Aperçu1. Nous vous recommandons de recompiler votre application pour ARM64 afin que votre application s’exécute à la vitesse native complète. Pour plus d’informations, voir le billet de blog [Version préliminaire de la prise en charge de Visual Studio du développement de Windows10 sur ARM](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/).

Si une application de bureau x86 ne fonctionne pas de la même manière que sur un poste x86, voici quelques recommandations qui vous aideront à résoudre le problème.

|Problème|Solution|
|-----|--------|
| Votre application repose sur un pilote qui n'a pas été désigné pour un ARM. | Recompilez votre pilote x86 pour l'ARM64. Consultez [Génération de pilotes ARM64 avec le kit WDK](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers). |
| Votre application est uniquement disponible pour un système x64. | Si vous développez pour le MicrosoftStore, soumettez une version ARM de votre application. Pour plus d’informations, consultez [Architectures des packages d’applications](../packaging/device-architecture.md). Si vous êtes un développeur Win32, nous vous recommandons de recompiler votre application pour ARM64. Pour plus d’informations, voir [Version préliminaire de la prise en charge de Visual Studio du développement de Windows10 sur ARM](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/). |
| Votre application utilise une version d'OpenGL ultérieure à la version1.1 ou nécessite une version d'OpenGL à accélération matérielle. | S'il est disponible, utilisez le mode DirectX de l’application. Les applications x86 utilisant DirectX9, DirectX10, DirectX11 et DirectX12 fonctionneront sur ARM. Pour plus d’informations, consultez [Jeux et graphismes DirectX](https://msdn.microsoft.com/en-us/library/windows/desktop/ee663274(v=vs.85).aspx). |
| Votre application x86 ne fonctionne pas comme prévu. | Essayez d'utiliser l'utilitaire de résolution des problèmes de compatibilité en suivant les recommandations de l'[Utilitaire de résolution des problèmes de compatibilité sur ARM](apps-on-arm-program-compat-troubleshooter.md). Pour certaines étapes de résolution d'autres problèmes, consultez l'article [Résolution des problèmes des applications x86 sur ARM](apps-on-arm-troubleshooting-x86.md). |

## <a name="best-practices-for-wow"></a>Bonnes pratiques pourWOW
Un problème courant particulier survient lorsqu'une application détermine qu'elle est exécutée sous WOW et suppose qu'il s'agit d'un système x64. Suite à cette supposition, l'application agira probablement de la sorte:

- Elle tente d'installer la version x64, qui n'est pas prise en charge sur ARM.
- Elle cherche un autre logiciel sous l'affichage du registre natif.
- Elle suppose qu'un .NETFramework 64 bits est disponible.

En règle générale, une application ne doit faire aucune hypothèse quant au système hôte lorsqu'il a été déterminé qu'elle doit être exécutée sous WOW. Évitez autan que possible d'interagir avec les composants natifs du système d'exploitation.

Une application est susceptible de disposer les clés de registre sous l'affichage du registre natif, ou d'appliquer des fonctions en fonction de la présence de WOW. Le processus **IsWow64Process** d'origine indique si l'application est exécutée sur un poste x64. Les applications doivent désormais utiliser le processus [IsWow64Process2](https://msdn.microsoft.com/en-us/library/windows/desktop/mt804318(v=vs.85).aspx) pour déterminer si elles sont exécutées sur un système prenant en charge WOW. 

## <a name="drivers"></a>Pilotes 
Tous les pilotes en mode noyau, les pilotes [Infrastructure de pilote en mode utilisateur (UMDF)](https://docs.microsoft.com/windows-hardware/drivers/wdf/overview-of-the-umdf) et les pilotes d'impression doivent être compilés pour correspondre à l'architecture du système d'exploitation. Si une application x86 dispose d’un pilote, ce dernier doit être recompilé pour ARM64. L'application x86 peut très bien s'exécuter sous émulation. Néanmoins, son pilote devra être recompilé pour ARM64, et toute expérience d'application dépendant du pilote sera indisponible. Pour plus d’informations sur la compilation de votre pilote pour ARM64, consultez [Génération de pilotes ARM64 avec le kit WDK](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers).

## <a name="shell-extensions"></a>Extensions d'environnement 
Les applications qui tentent d'attacher des composants Windows ou de charger leurs fichiers DLL dans des processus Windows devront recompiler ces DLL afin de les faire correspondre à l'architecture du système, c'est-à-dire ARM64. En général, ces éléments sont utilisés par les éditeurs de méthodes d'entrée (IME), par les technologies d'assistance et par les applications d'extension d'environnement (par ex., pour afficher les icônes de stockage cloud dans Explorer ou dans le menu local à clic droit). Pour savoir comment recompiler vos applications ou les DLL ARM64, voir le billet de blog [Version préliminaire de la prise en charge de Visual Studio du développement de Windows10 sur ARM](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/). 

## <a name="debugging"></a>Débogage
Pour étudier le comportement de votre application plus en détail, consultez [Débogage sur ARM](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) pour en savoir plus sur les outils et stratégies de débogage sur ARM.

## <a name="virtual-machines"></a>Machines virtuelles
La plateforme WindowsHypervisor n'est pas prise en charge sur la plateforme PC mobile QualcommSnapdragon835. De ce fait, l'exécution de machines virtuelles à l'aide d'Hyper-V ne fonctionnera pas. Nous continuons d'investir dans ces technologies sur les futurs circuits microprogrammés Qualcomm. 