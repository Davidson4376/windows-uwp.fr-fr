---
author: jnHs
Description: "Consultez cette liste pour éviter les problèmes qui empêchent souvent les applications d’être certifiées, ou qui peuvent être identifiés au cours d’une vérification ponctuelle après la publication de l’application."
title: "Éviter les échecs de certification courants"
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 92e1b35c8eba7f1f3d3a32f0c994d3353a065419
ms.sourcegitcommit: fadde8afee46238443ec1cb71846d36c91db9fb9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2017
---
# <a name="avoid-common-certification-failures"></a>Éviter les échecs de certification courants


Consultez cette liste pour éviter les problèmes qui empêchent souvent les applications d’être certifiées, ou qui peuvent être identifiés au cours d’une vérification ponctuelle après la publication de l’application.

> [!NOTE]
> Veillez à vous reporter aux [Politiques du WindowsStore](https://msdn.microsoft.com/library/windows/apps/dn764944) pour vous assurer que votre application remplit toutes les exigences spécifiées.

-   Ne soumettez votre application que lorsqu’elle est terminée. Nous vous invitons à utiliser la description de votre application pour signaler les fonctionnalités à venir. Toutefois, assurez-vous que votre application ne contient pas de sections incomplètes, de liens vers des pages web en construction, ni toute autre chose qui donnerait au client l'impression que votre application est incomplète.

-   [Procédez au test de votre application à l’aide du Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) avant de la soumettre.

-   Testez votre application dans différentes configurations pour vérifier qu'elle est aussi stable que possible.

-   Assurez-vous que votre application ne se bloque pas sans connectivité réseau. Même si une connexion est requise pour utiliser réellement votre application, cette dernière doit fonctionner correctement en l’absence de connexion.

-   [Fournissez toutes les informations requises](notes-for-certification.md) pour utiliser votre application, telles que le nom d’utilisateur et le mot de passe d’un compte de test si votre application nécessite que les utilisateurs se connectent à un service, ou toutes les étapes requises pour accéder aux fonctionnalités masquées ou verrouillées.

-   Incluez une [politique de confidentialité](create-app-store-listings.md#privacy-policy) si votre application le nécessite, notamment si elle accède d’une quelconque manière à des informations personnelles ou si la législation l’impose. Pour déterminer plus facilement si votre application requiert une politique de confidentialité, examinez le [Contrat du développeur d’application](https://msdn.microsoft.com/library/windows/apps/hh694058) et les [politiques du WindowsStore](https://msdn.microsoft.com/library/windows/apps/dn764944).

-   Vérifiez que la description de votre application indique clairement en quoi consiste cette dernière. Si vous avez besoin d’aide, consultez nos recommandations en matière de [rédaction d’une description d’application attractive](write-a-great-app-description.md).

-   Fournissez des réponses complètes et précises à l’ensemble des questions de la section [Évaluations par âge](age-ratings.md).

-   Ne [déclarez pas votre application comme accessible](app-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines) à moins de l’avoir spécifiquement conçue et testée pour des scénarios d’accessibilité.

-   Si votre application utilise les API de commerce à partir de l’espace de noms [**Windows.ApplicationModel.Store**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store), prenez soin de tester l’application et de vérifier qu’elle gère les exceptions par défaut. Assurez-vous également que votre application utilise la classe [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp), et non la classe [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator), qui est uniquement fournie à des fins de test. (Notez que si votre application cible Windows10, version1607 ou ultérieure, nous vous recommandons d’utiliser les membres de l’espace de noms [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) plutôt que ceux de l’espace de noms Windows.ApplicationModel.Store.)


 

 




