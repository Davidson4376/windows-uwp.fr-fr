---
title: Résolution des problèmes relatifs aux applications UWP ARM32
author: msatranjr
description: Problèmes courants avec les applications ARM32 lors de l'exécution sur ARM, et comment les résoudre.
ms.author: misatran
ms.date: 05/09/2018
ms.topic: article
keywords: windows10s, toujours connecté, applications ARM32 sur ARM, windows10 sur ARM, résolution des problèmes
ms.localizationpriority: medium
ms.openlocfilehash: 4eaeccb8b4003bb835ee4700d1df57cd8cefcda0
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6155838"
---
# <a name="troubleshooting-arm32-uwp-apps"></a>Résolution des problèmes relatifs aux applications UWP ARM32
>[!IMPORTANT]
> Le SDK ARM64 est désormais disponible dans le cadre de Visual Studio15.8 Aperçu1. Nous vous recommandons de recompiler votre application pour ARM64 afin que votre application s’exécute à la vitesse native complète. Pour plus d’informations, voir le billet de blog [Version préliminaire de la prise en charge de Visual Studio du développement de Windows10 sur ARM](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/).

Si votre application UWP ARM32 ne fonctionne par correctement sur ARM, ces quelques recommandations sont susceptibles de vous aider. 

## <a name="common-issues"></a>Problèmes courants
Voici quelques problèmes courants à garder à l'esprit lors de la résolution des problèmes liés aux applications ARM32.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>Utilisation des API uniquement Windows10Mobile sur les processeurs basés sur ARM 
Les applications ARM32 peuvent rencontrer des problèmes lors de l'utilisation d'API uniquement mobiles (par exemple, **HardwareButtons**). Pour atténuer ce risque, vous pouvez déterminer dynamiquement si votre application est exécutée sur Windows10Mobile avant d'appeler ces API. Suivez les recommandations du billet de blog [Détection dynamique des fonctionnalités avec les contrats API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/).

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>Inclusion de dépendances non prises en charge par les applications UWP
Les applications de plateforme Windows universelle (UWP) ne sont pas exactement intégrées dans VisualStudio. De ce fait, le kit SDK UWP peut comporter des dépendances sur les composants du système d'exploitation qui sont indisponibles aux applications ARM32 exécutées sur un système ARM64. Voici quelques exemples de ces dépendances:

- Attente de la mise à dispositions de pièces .NETFramework.
- Référencement de composants .NET tiers incompatibles avec UWP.

Ces problèmes peuvent être résolus comme suit: supprimez les dépendances indisponibles et regénérez l'application à l'aide des dernières versions de MicrosoftVisualStudio et du kit SDK UWP. Si cela ne fonctionne pas, vous pouvez supprimer l'application ARM32 du MicrosoftStore afin que la version x86 de l'application (le cas échéant) soit téléchargée sur les PC des utilisateurs. 

Pour plus d’informations sur les API .NET disponibles pour les applications UWP, consultez [.NET pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/mt185501.aspx)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>Compilation d'une application avec une version antérieure de VisualStudio et du kit SDK
Si vous rencontrez des problèmes, veillez à utiliser les dernières versions de MicrosoftVisualStudio et du kit SDKWindows pour compiler votre application. Applications compilées avec une version antérieure de VisualStudio et du kit SDK peuvent rencontrer des problèmes qui ont été résolus dans les versions ultérieures.

## <a name="debugging"></a>Débogage
Vous pouvez utiliser des outils existants pour le développement d’applications ARM32 pour la plateforme ARM. Voici quelques ressources utiles.

- Visual Studio 15.5 L'aperçu 1 et ultérieur de VisualStudio15.5 prennent en charge les applications ARM32 en cours d’exécution à l’aide du mode d’authentification universel. Ainsi, les outils nécessaires de débogage à distance sont automatiquement amorcés.
- Pour en savoir plus sur les outils et stratégies de débogage sur ARM, consultez [Débogage sur ARM64](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64).