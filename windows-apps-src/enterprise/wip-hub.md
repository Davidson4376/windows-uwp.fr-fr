---
Description: This is a hub topic covering the full developer picture of how Windows Information Protection (WIP) relates to files, buffers, clipboard, networking, background tasks, and data protection under lock.
MS-HAID: dev\_enterprise.edp\_hub
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Protection des informations Windows (WIP)
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, uwp, Protection des informations Windows, données d’entreprise, protection des données d’entreprise, PDE, applications compatibles
ms.assetid: 08f0cfad-f15d-46f7-ae7c-824a8b1c44ea
ms.localizationpriority: medium
ms.openlocfilehash: b65da20c8931f74800f817ecba0139b14d0447ad
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8788515"
---
# <a name="windows-information-protection-wip"></a>Protection des informations Windows (WIP)

__Remarque__ La stratégie de Protection des informations Windows peut être appliquée à Windows10, version1607.

Cette stratégie protège les données qui appartiennent à une organisation en appliquant des stratégies qui sont définies par l’organisation. Si votre application est incluse dans ces stratégies, toutes les données générées par votre application sont soumises aux restrictions de stratégie. Cette rubrique vous aide à créer des applications qui appliquent ces stratégies plus en douceur sans avoir d’impact sur les données personnelles de l’utilisateur.
<iframe src="https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Securing-Enterprise-Data-with-Windows-Information-Protection/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="first-what-is-wip"></a>Tout d’abord, qu’est-ce que la Protection des informations Windows?

WIP est un ensemble de fonctionnalités prenant en charge la gestion des appareils mobiles (GPM) et la gestion des applications mobiles (GAM) de l’organisation sur les postes de travail, les ordinateurs de bureau, les tablettes et les téléphones.

Avec la gestion des appareils mobiles, WIP permet à l’organisation de mieux contrôler l’utilisation des données d’entreprise sur les appareils gérés par l’organisation. Il arrive parfois que les utilisateurs apportent leurs propres appareils à leur travail et ne les inscrivent pas dans le système de gestion des appareils mobiles de l’organisation.  Les organisations peuvent alors utiliser la gestion des applications mobiles pour mieux contrôler l’utilisation de leurs données dans des applications métier que les utilisateurs ont installées sur leur appareil.

À l’aide de la gestion des appareils mobiles et de la gestion des applications mobiles, les administrateurs peuvent identifier les applications autorisées à accéder aux fichiers appartenant à l’organisation et indiquer si les utilisateurs peuvent copier des données à partir de ces fichiers pour les coller ensuite dans des documents personnels.

Voici le principe: Les utilisateurs inscrivent leurs appareils dans le système de gestion des appareils mobiles (GPM) de l’organisation. Un administrateur de l’organisation de gestion utilise Microsoft Intune ou System Center Configuration Manager (SCCM) pour définir, puis déployer une stratégie sur les appareils inscrits.

Si les utilisateurs ne sont pas obligés d’inscrire leurs appareils, les administrateurs définissent et déploient une stratégie pour des applications spécifiques dans le système de gestion des applications mobiles. Quand les utilisateurs installent l’une de ces applications, la stratégie associée est implémentée.

Cette stratégie identifie les applications qui peuvent accéder aux données d’entreprise (appelée *liste des applications autorisées* de la stratégie). Ces applications peuvent accéder aux fichiers d’entreprise protégés, aux réseaux privés virtuels (VPN) et aux données d’entreprise sur le Presse-papiers ou via un contrat de partage. La stratégie définit également les règles qui régissent les données. Par exemple, si les données peuvent être copiées à partir de fichiers appartenant à l’entreprise, puis collées dans des fichiers n’appartenant pas à l’entreprise.

Si les utilisateurs désinscrivent leur appareil à partir du système de gestion des appareils mobiles de l’organisation, ou s’ils désinstallent des applications identifiées par le système de gestion des applications mobiles, les administrateurs peuvent effacer à distance les données d’entreprise de l’appareil.

![Cycle de vie de la Protection des informations Windows](images/wip-lifecycle.png)

> **En savoir plus sur la Protection des informations Windows** <br>
* [Présentation de la Protection des informations Windows](https://blogs.technet.microsoft.com/windowsitpro/2016/06/29/introducing-windows-information-protection/)
* [Protéger vos données d’entreprise à l’aide de la Protection des informations Windows (WIP)](https://technet.microsoft.com/library/dn985838(v=vs.85).aspx)

Si votre application se trouve sur la liste autorisée, toutes les données générées par votre application sont soumises aux restrictions de stratégie. Cela signifie que si les administrateurs révoquent l’accès de l’utilisateur aux données d’entreprise, ces utilisateurs perdent l’accès à toutes les données produites par votre application.

Cela est approprié si votre application est conçue uniquement à des fins professionnelles Mais si votre application crée des données que les utilisateurs considèrent comme personnelles, vous souhaiterez rendre votre application *compatible* afin qu’elle fasse la distinction de manière intelligente entre les données personnelles et les données d’entreprise. Cette application est considérée comme *compatible avec l’entreprise* dans la mesure où elle peut appliquer la stratégie d’entreprise de manière fluide tout en préservant l’intégrité des données personnelles de l’utilisateur.

## <a name="create-an-enterprise-enlightened-app"></a>Créer une application compatible avec l’entreprise

Utiliser les API WIP pour rendre votre application compatible avec l’entreprise et la déclarer comme tel.

Rendez votre application compatible si elle doit être utilisée à la fois à des fins personnelles et professionnelles.

Rendez votre application compatible si vous voulez gérer de manière fluide l’application des éléments de stratégie.

Par exemple, si la stratégie permet aux utilisateurs de coller des données d’entreprise dans un document personnel, vous pouvez empêcher les utilisateurs d’avoir à répondre à une boîte de dialogue de consentement avant le collage des données. De même, vous pouvez présenter des boîtes de dialogue informatives personnalisées en réponse à ces types d’événements.

Si vous êtes prêt à rendre votre application compatible, reportez-vous à l’un de ces guides:

**Pour les applications de plateforme Windows universelle (UWP) générées à l’aide de c#**

[Guide du développeur sur la Protection des informations Windows](wip-dev-guide.md).

**Pour les applications de bureau que vous créez à l’aide de C++**

[Guide du développeur sur la Protection des informations Windows (C++)](http://go.microsoft.com/fwlink/?LinkId=822192).


## <a name="create-non-enlightened-enterprise-app"></a>Créer une application non compatible qui utilise des données d’entreprise

Si vous créez une application métier destinée à un usage professionnel, vous n’avez pas besoin de la rendre compatible.

### <a name="windows-desktop-apps"></a>Applications de bureau Windows
Vous n’avez pas besoin de rendre compatible une application de bureau Windows, mais vous devez la tester pour vérifier qu’elle fonctionne conformément à la stratégie applicable. Par exemple, démarrez votre application et utilisez-la, puis désinscrivez l’appareil à partir du système de gestion des appareils mobiles. Vérifiez ensuite que vous pouvez redémarrer l’application. Si des fichiers nécessaires au fonctionnement de l’application sont chiffrés, cela risque d’empêcher le démarrage de l’application. Examinez également les fichiers avec lesquels l’application interagit pour vous assurer que l’application ne chiffre pas par erreur des fichiers personnels de l’utilisateur, tels que des fichiers de métadonnées, des images, etc.

Quand vous avez fini de tester votre application, ajoutez cet indicateur au fichier de ressources de votre projet, puis recompilez l’application.

```cpp
MICROSOFTEDPAUTOPROTECTIONALLOWEDAPPINFO EDPAUTOPROTECTIONALLOWEDAPPINFOID
BEGIN
    0x0001
END
```
Cet indicateur est obligatoire pour les stratégies de gestion des applications mobiles, mais pas pour les stratégies de gestion des appareils mobiles.

### <a name="uwp-apps"></a>Applications UWP

Si vous prévoyez d’inclure votre application dans une stratégie de gestion des applications mobiles (GAM), vous devez la rendre compatible. Cela n’est pas obligatoire pour les stratégies déployées sur des appareils inscrits dans un système de gestion des appareils mobiles (GPM). Toutefois, quand vous distribuez votre application à des organisations, vous ne savez généralement pas quel type de système de gestion des stratégies ces organisations utilisent. Pour être sûr que votre application fonctionnera dans les deux types de systèmes de gestion des stratégies (GPM et GAM), il est donc préférable de la rendre compatible.






 
