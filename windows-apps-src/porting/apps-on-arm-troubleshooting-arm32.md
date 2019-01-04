---
title: Résolution des problèmes relatifs aux applications UWP ARM32
description: Problèmes courants avec les applications ARM32 lors de l'exécution sur ARM, et comment les résoudre.
ms.date: 01/03/2019
ms.topic: article
keywords: windows10s, toujours connecté, applications ARM32 sur ARM, windows10 sur ARM, résolution des problèmes
ms.localizationpriority: medium
ms.openlocfilehash: 75019df4b7d70dad20aea1a256abac05c93a481d
ms.sourcegitcommit: 62bc4936ca8ddf1fea03d43a4ede5d14a5755165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/04/2019
ms.locfileid: "8991635"
---
# <a name="troubleshooting-arm-uwp-apps"></a>Résolution des problèmes de bras les applications UWP

Si votre application ARM32 ou ARM64 UWP ne fonctionne pas correctement sur ARM, voici quelques conseils qui peuvent aider.

>[!NOTE]
> Pour générer votre application UWP en mode natif cible la plateforme ARM64, vous devez disposer de Visual Studio 2017 version 15.9 ou une version ultérieure. Pour plus d’informations, consultez [ce billet de blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).

## <a name="common-issues"></a>Problèmes courants
Voici quelques problèmes courants à prendre en considération lors de la résolution des problèmes d’applications ARM32 et ARM64.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>Utilisation des API uniquement Windows10Mobile sur les processeurs basés sur ARM
Les applications ARM peuvent rencontrer des problèmes lors de l’utilisation des API uniquement mobiles (par exemple, **HardwareButtons**). Pour atténuer ce risque, vous pouvez déterminer dynamiquement si votre application est exécutée sur Windows10Mobile avant d'appeler ces API. Suivez les recommandations du billet de blog [Détection dynamique des fonctionnalités avec les contrats API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/).

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>Inclusion de dépendances non prises en charge par les applications UWP
Les applications de plateforme Windows universelle (UWP) universelle qui ne sont pas exactement intégrées avec Visual Studio et le SDK UWP peuvent comporter des dépendances sur les composants du système d’exploitation qui ne sont pas disponibles pour les applications ARM en cours d’exécution sur un système ARM64. Voici quelques exemples de ces dépendances:

- Attente de la mise à dispositions de pièces .NETFramework.
- Référencement de composants .NET tiers incompatibles avec UWP.

Ces problèmes peuvent être résolus: supprimez les dépendances indisponibles et régénérer l’application en utilisant les dernières versions de Microsoft Visual Studio et SDK UWP; ou en dernier recours, suppression de l’application ARM depuis le Microsoft Store, afin que le x86 version de l’application (le cas échéant) soit téléchargée sur les PC des utilisateurs.

Pour plus d’informations sur les API .NET disponibles pour les applications UWP, consultez [.NET pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/mt185501.aspx)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>Compilation d'une application avec une version antérieure de VisualStudio et du kit SDK
Si vous rencontrez des problèmes, veillez à utiliser les dernières versions de MicrosoftVisualStudio et du kit SDKWindows pour compiler votre application. Applications compilées avec une version antérieure de VisualStudio et du kit SDK peuvent rencontrer des problèmes qui ont été résolus dans les versions ultérieures.

## <a name="debugging"></a>Débogage
Vous pouvez utiliser les outils existants pour le développement d’applications pour la plateforme ARM. Voici quelques ressources utiles.

- Visual Studio 15.5 L'aperçu 1 et ultérieur de VisualStudio15.5 prennent en charge les applications ARM32 en cours d’exécution à l’aide du mode d’authentification universel. Ainsi, les outils nécessaires de débogage à distance sont automatiquement amorcés.
- Pour en savoir plus sur les outils et stratégies de débogage sur ARM, consultez [Débogage sur ARM64](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64).
