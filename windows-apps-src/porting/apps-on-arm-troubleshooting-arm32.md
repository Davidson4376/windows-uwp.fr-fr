---
title: Résolution des problèmes relatifs aux applications UWP ARM32
author: msatranjr
description: Problèmes courants avec les applications ARM32 lors de l'exécution sur ARM, et comment les résoudre.
ms.author: misatran
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10s, toujours connecté, applications ARM32 sur ARM, windows10 sur ARM, résolution des problèmes
ms.localizationpriority: medium
ms.openlocfilehash: 71d92ec26311514e0eebdfa4a1dab39e86ce72fc
ms.sourcegitcommit: 11edca90aaf7856c762e68903483079d30ad3877
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2018
ms.locfileid: "1595124"
---
# <a name="troubleshooting-arm32-uwp-apps"></a>Résolution des problèmes relatifs aux applications UWP ARM32
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