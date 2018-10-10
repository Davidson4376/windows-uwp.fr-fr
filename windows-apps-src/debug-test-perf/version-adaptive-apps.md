---
author: jwmsft
title: Applications adaptatives de version
description: Découvrez comment tirer parti des nouvellesAPI tout en conservant la compatibilité avec les versions précédentes
ms.author: jimwalk
ms.date: 09/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f2485eab4b192fe4a65c68d957de1ec9192f8c20
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4504178"
---
# <a name="version-adaptive-apps-use-new-apis-while-maintaining-compatibility-with-previous-versions"></a>Applications adaptatives de version: Utiliser de nouvellesAPI tout en conservant la compatibilité avec les versions précédentes

Chaque version du Kit de développement logicielWindows10 apporte d’incroyables fonctionnalités qui vous raviront. Toutefois, vos clients ne mettent pas tous en même temps leurs appareils à jour vers la dernière version de Windows10, et vous voulez vous assurer que votre application fonctionne sur le plus large éventail possible d’appareils. Nous allons vous expliquer comment concevoir votre application afin qu’elle s’exécute sur les versions antérieures de Windows10, mais tire également parti des nouvelles fonctionnalités lorsque votre application s’exécute sur un appareil disposant de la dernière mise à jour.

Vous devez suivre 3étapes pour vous assurer que votre application prend en charge la plus large gamme d’appareils Windows10.

- Tout d’abord, configurez votre projet VisualStudio afin de cibler les dernièresAPI. Cela influe sur les résultats de la compilation de votre application.
- Ensuite, effectuez des vérifications à l’exécution pour vérifier que vous appelez uniquement les API présentes sur l’appareil sur lequel votre application s’exécute.
- Enfin, testez votre application sur la version Minimum et la version Cible de Windows10.

## <a name="configure-your-visual-studio-project"></a>Configurer votre projet VisualStudio

La première étape de la prise en charge de plusieurs versions de Windows10 consiste à spécifier les versions de système d’exploitation et de Kit de développement logiciel *Cible* et *Minimum* prises en charge dans votre projet VisualStudio.

- *Cible*: version du Kit de développement logiciel pour laquelle Visual Studio compile le code de votre application et exécute tous les outils. Toutes les API et les ressources de ce Kit de développement logiciel sont disponibles dans le code de votre application au moment de la compilation.
- *Minimum*: version du Kit de développement logiciel qui prend en charge la version la plus ancienne du système d’exploitation sur laquelle votre application peut s’exécuter (et qui sera déployée par le WindowsStore) et la version pour laquelle VisualStudio compile le code de balisage de votre application. 

Pendant son exécution, votre application s’exécute par rapport à la version du système d’exploitation déployée: votre application lève donc des exceptions si vous utilisez des ressources ou si vous appelez des API qui ne sont pas disponibles dans cette version. Nous allons vous montrer comment effectuer des vérifications à l’exécution pour appeler les API appropriées dans la suite de cet article.

Les paramètres Cible et Minimum spécifient les limites de plage des versions de système d’exploitation et de Kit de développement logiciel. Toutefois, si vous testez votre application sur la version Minimum, vous pouvez être sûr qu’elle s’exécutera sur toutes les versions comprises entre la version Minimum et la version Cible.

> [!TIP]
> VisualStudio ne donne aucune information sur la compatibilité des API. Il vous incombe de tester et de vous assurer que votre application fonctionne comme prévu sur toutes les versions de système d’exploitation, de la version Minimum à la version Cible incluses.

Lorsque vous créez un projet dans VisualStudio2015, Update2 ou version ultérieure, vous êtes invité à définir les versions Cible et Minimum prises en charge par votre application. Par défaut, la version Cible est la version du Kit de développement logiciel installée la plus élevée, tandis que la version Minimum est la version du Kit de développement logiciel installée la moins élevée. Vous pouvez choisir les versions Cible et Minimum uniquement parmi les versions du Kit de développement logiciel installées sur votre ordinateur. 

![Définir le Kit de développement logiciel cible dans VisualStudio](images/vs-target-sdk-1.png)

Nous vous recommandons généralement de conserver les valeurs par défaut. Toutefois, si vous avez installé une version d’évaluation du Kit de développement et si vous écrivez du code de production, vous devez modifier la version Cible en passant du Kit de développement logiciel d’évaluation à la dernière version officielle du Kit de développement logiciel. 

Pour modifier la version Minimum et la version Cible d’un projet déjà créé dans VisualStudio, accédez à Projet -&gt; Propriétés -&gt; onglet Application -&gt; Ciblage.

![Modifier le Kit de développement logiciel cible dans VisualStudio](images/vs-target-sdk-2.png)

À titre de référence, les tableaux suivants indiquent les numéros de version pour chaque Kit de développement logiciel:

| Nom convivial | Version | Build OS/SDK |
| ---- | ---- | ---- |
| RTM | 1507 | 10240 |
| Mise à jour de novembre | 1511 | 10586 |
| Mise à jour anniversaire | 1607 | 14393 |
| CreatorsUpdate | 1703 | 15063 |
| FallCreatorsUpdate | 1709 | 16299 |
| Mise à jour d’avril2018 | 1803 | 17134 |
| Mise à jour de l’octobre 2018 | 1809 | _Insider Preview_ |

Vous pouvez télécharger les versions finales du SDK à partir des [archives du SDKWindows et de l’émulateur](https://developer.microsoft.com/downloads/sdk-archive). Vous pouvez télécharger la dernière version du Kit de développement logiciel (SDK) WindowsInsiderPreview dans la section du site [WindowsInsider](https://insider.windows.com/Home/BuildWithWindows) dédiée aux développeurs.

 Pour plus d’informations sur les mises à jour Windows 10, consultez [les informations de publication de Windows 10](https://technet.microsoft.com/windows/release-info). Pour des informations importantes sur Windows 10 prend en charge de cycle de vie, consultez la [fiche d’informations de cycle de vie de Windows](https://support.microsoft.com/help/13853/windows-lifecycle-fact-sheet).

## <a name="perform-api-checks"></a>Effectuer des vérifications des API

La clé pour les applications adaptatives de version est la combinaison des contrats API et de la classe [ApiInformation](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation). Cette classe vous permet de détecter si un contrat API, un type ou un membre spécifié est présent, afin de pouvoir effectuer en toute sécurité des appels d’API sur une variété d'appareils et de versions de système d’exploitation.

### <a name="api-contracts"></a>Contrats API

L’ensemble des API au sein d’une famille d’appareils comprend des subdivisions appelées contrats API. La méthode **ApiInformation.IsApiContractPresent** permet de tester la présence d’un contrat API. Cela est utile si vous voulez tester la présence de nombreuses API qui existent toutes dans la même version d’un contrat API.

```csharp
    bool isScannerDeviceContract_1_Present =
        Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent
            ("Windows.Devices.Scanners.ScannerDeviceContract", 1);
```

Qu’est un contrat API? Un contrat API représente essentiellement une fonctionnalité – un ensemble d’API associées qui offrent ensemble une fonctionnalité particulière. Un contrat API hypothétique pourrait représenter un ensemble d’API contenant deuxclasses, cinqinterfaces, unestructure, deuxénumérations, et ainsi de suite.

Les types associés de façon logique sont regroupés dans un contrat API, et à partir de Windows10, chaque API WindowsRuntime est membre d'un contrat API. Avec les contrats API, vous recherchez la disponibilité d'une fonctionnalité ou d'une API spécifique sur l’appareil, en vérifiant efficacement les fonctionnalités d’un appareil plutôt qu'un appareil ou un système d’exploitation spécifique. Une plateforme implémentant toutes les API d'un contrat API est requise pour implémenter chaque API de ce contrat. Cela signifie que vous pouvez tester si le système d’exploitation en cours d’exécution prend en charge un contrat API particulier et, si tel est le cas, appeler n'importe quelle API de ce contrat API sans devoir vérifier chacune d'elle individuellement.

Le contrat API le plus important et le plus utilisé est **Windows.Foundation.UniversalApiContract**. Il contient la majorité des API de la plateforme Windows universelle. La documentation relative aux [Kits de développement logiciels (SDK) d’extension de familles d'appareils et contrats API](https://docs.microsoft.com/uwp/extension-sdks/) décrit la variété de contrats API disponibles. Vous verrez que la plupart d'entre eux représentent un ensemble d’API associées sur un plan fonctionnel.

> [!NOTE]
> Si vous avez installé une version préliminaire du Kit de développement logiciel Windows (KitSDKWindows) qui n’est pas encore documentée, vous trouverez également des informations sur la prise en charge des contrats API dans le fichier Platform.xml situé dans le dossier d’installation du SDK à l‘emplacement suivant: ‘\(Program Files (x86))\Windows Kits\10\Platforms\<platform>\<SDK version>\Platform.xml’.

### <a name="version-adaptive-code-and-conditional-xaml"></a>Code adaptatif de version et XAML conditionnel

Dans toutes les version de Windows10, vous pouvez utiliser la classe ApiInformation dans une condition à l’intérieur de votre code pour tester la présence de l’API que vous voulez appeler. Dans votre code adaptatif, vous pouvez utiliser différentes méthodes de la classe, telles que IsTypePresent, IsEventPresent, IsMethodPresent et IsPropertyPresent, pour tester les API au niveau de granularité dont vous avez besoin.

Pour obtenir plus d’informations et des exemples, voir **[Code adaptatif de version](version-adaptive-code.md)**.

Si la version Minimum de vos applications est la build15063 (CreatorsUpdate) ou version ultérieure, vous pouvez utiliser le *XAML conditionnel* pour définir des propriétés et instancier des objets dans le balisage sans avoir à utiliser le code-behind. Le XAML conditionnel permet d'utiliser la méthode ApiInformation.IsApiContractPresent dans le balisage.

Pour obtenir plus d'informations et des exemples, voir **[XAML conditionnel](conditional-xaml.md)**.

## <a name="test-your-version-adaptive-app"></a>Tester votre application adaptative de version

Lorsque vous utilisez du code adaptatif de version ou du XAML conditionnel pour écrire une application adaptative de version, vous devez le tester sur un appareil exécutant la version Minimum et sur un appareil exécutant la version Cible de Windows10.

Vous ne pouvez pas tester tous les chemins de code conditionnel sur un seul périphérique. Pour vous assurer que tous les chemins de code sont testés, vous devez déployer et tester votre application sur un appareil distant (ou une machine virtuelle) exécutant la version Minimum du système d’exploitation pris en charge.
Pour plus d’informations sur le débogage à distance, voir [Déploiement et débogage des applications UWP](deploying-and-debugging-uwp-apps.md).

## <a name="related-articles"></a>Articles associés

- [Qu’est-ce qu’une application UWP?](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [Détection dynamique des fonctionnalités avec les contratsAPI](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
- [Contrats API](https://channel9.msdn.com/Events/Build/2015/3-733) (Build2015 vidéo)