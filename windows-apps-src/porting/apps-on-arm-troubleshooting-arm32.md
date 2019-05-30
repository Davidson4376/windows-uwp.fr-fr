---
title: Résolution des problèmes relatifs aux applications UWP ARM32
description: Problèmes courants avec les applications ARM32 lors de l'exécution sur ARM, et comment les résoudre.
ms.date: 01/03/2019
ms.topic: article
keywords: windows 10 s, toujours connecté, applications ARM32 sur ARM, windows 10 sur ARM, résolution des problèmes
ms.localizationpriority: medium
ms.openlocfilehash: ddf28627838ebc8cb2df620c398f3803c026cb17
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366833"
---
# <a name="troubleshooting-arm-uwp-apps"></a>Résolution des problèmes ARM des applications UWP

Si votre application ARM32 ou ARM64 UWP ne fonctionne pas correctement sur ARM, voici quelques conseils qui peuvent aider.

>[!NOTE]
> Pour générer votre application UWP en mode natif de cibler la plateforme ARM64, vous devez disposer de Visual Studio 2017 version 15.9 ou ultérieure. Pour plus d’informations, consultez [ce billet de blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).

## <a name="common-issues"></a>Problèmes courants
Voici certains problèmes courants à prendre en compte lors du dépannage d’applications ARM32 et ARM64.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>Utilisation des API uniquement Windows 10 Mobile sur les processeurs basés sur ARM
Les applications ARM risquez de rencontrer des problèmes lors de l’utilisation d’API mobile uniquement (par exemple, **HardwareButtons**). Pour atténuer ce risque, vous pouvez déterminer dynamiquement si votre application est exécutée sur Windows 10 Mobile avant d'appeler ces API. Suivez les recommandations du billet de blog [Détection dynamique des fonctionnalités avec les contrats API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/).

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>Inclusion de dépendances non prises en charge par les applications UWP
Applications universelles Windows Platform (UWP) qui ne sont pas correctement générées avec Visual Studio et le SDK UWP peuvent avoir des dépendances sur les composants du système d’exploitation qui ne sont pas disponibles pour les applications ARM en cours d’exécution sur un système ARM64. Voici quelques exemples de ces dépendances :

- Attente de la mise à dispositions de pièces .NET Framework.
- Référencement de composants .NET tiers incompatibles avec UWP.

Ces problèmes peuvent être résolus par : supprimer les dépendances non disponibles et la reconstruction de l’application en utilisant les dernières versions Microsoft Visual Studio et SDK UWP ; ou, en dernier recours, suppression de l’application ARM à partir du Microsoft Store, afin que la version de l’application (si disponible) est téléchargée sur les ordinateurs des utilisateurs de x86.

Pour plus d’informations sur les API .NET disponibles pour les applications UWP, consultez [.NET pour les applications UWP](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>Compilation d'une application avec une version antérieure de Visual Studio et du kit SDK
Si vous rencontrez des problèmes, veillez à utiliser les dernières versions de Microsoft Visual Studio et du kit SDK Windows pour compiler votre application. Applications compilées avec une version antérieure de Visual Studio et du kit SDK peuvent rencontrer des problèmes qui ont été résolus dans les versions ultérieures.

## <a name="debugging"></a>Débogage
Vous pouvez utiliser les outils existants pour le développement d’applications pour la plateforme ARM. Voici quelques ressources utiles.

- Visual Studio 15.5 L'aperçu 1 et ultérieur de Visual Studio 15.5 prennent en charge les applications ARM32 en cours d’exécution à l’aide du mode d’authentification universel. Ainsi, les outils nécessaires de débogage à distance sont automatiquement amorcés.
- Pour en savoir plus sur les outils et stratégies de débogage sur ARM, consultez [Débogage sur ARM64](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64).
