---
author: jnHs
Description: Review this list to help avoid issues that frequently prevent apps from getting certified, or that might be identified during a spot check after the app is published.
title: Éviter les échecs de certification courants
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 283856ad163d2e67078c61559f6f8ec667e92b87
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2018
ms.locfileid: "5161277"
---
# <a name="avoid-common-certification-failures"></a>Éviter les échecs de certification courants


Consultez cette liste pour éviter les problèmes qui empêchent souvent les applications d’être certifiées, ou qui peuvent être identifiés au cours d’une vérification ponctuelle après la publication de l’application.

> [!NOTE]
> Veillez à passer en revue les [Politiques du Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies) pour vous assurer que votre application remplit toutes les obligations spécifiées.

-   Ne soumettez votre application que lorsqu’elle est terminée. Nous vous invitons à utiliser la description de votre application pour signaler les fonctionnalités à venir. Toutefois, assurez-vous que votre application ne contient pas de sections incomplètes, de liens vers des pages web en construction, ni toute autre chose qui donnerait au client l'impression que votre application est incomplète.

-   [Procédez au test de votre application à l’aide du Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) avant de la soumettre.

-   Testez votre application dans différentes configurations pour vérifier qu'elle est aussi stable que possible.

-   Assurez-vous que votre application ne se bloque pas sans connectivité réseau. Même si une connexion est requise pour utiliser réellement votre application, cette dernière doit fonctionner correctement en l’absence de connexion.

-   [Fournissez toutes les informations requises](notes-for-certification.md) pour utiliser votre application, telles que le nom d’utilisateur et le mot de passe d’un compte de test si votre application nécessite que les utilisateurs se connectent à un service, ou toutes les étapes requises pour accéder aux fonctionnalités masquées ou verrouillées.

-   Incluez une [politique de confidentialité](create-app-store-listings.md#privacy-policy) si votre application le nécessite, notamment si elle accède d’une quelconque manière à des informations personnelles ou si la législation l’impose. Pour vous aider à déterminer si votre application nécessite une politique de confidentialité, passez en revue le [Contrat développeur d’applications](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) et les [Stratégies du Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies).

-   Vérifiez que la description de votre application indique clairement en quoi consiste cette dernière. Si vous avez besoin d’aide, consultez nos recommandations en matière de [rédaction d’une description d’application attractive](write-a-great-app-description.md).

-   Fournissez des réponses complètes et précises à l’ensemble des questions de la section [Évaluations par âge](age-ratings.md).

-   Ne [déclarez pas votre application comme accessible](app-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines) à moins de l’avoir spécifiquement conçue et testée pour des scénarios d’accessibilité.

-   Si votre application utilise les API de commerce à partir de l’espace de noms [**Windows.ApplicationModel.Store**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store), prenez soin de tester l’application et de vérifier qu’elle gère les exceptions par défaut. Assurez-vous également que votre application utilise la classe [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp), et non la classe [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator), qui est uniquement fournie à des fins de test. (Notez que si votre application cible Windows10, version1607 ou ultérieure, nous vous recommandons d’utiliser les membres de l’espace de noms [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) plutôt que ceux de l’espace de noms Windows.ApplicationModel.Store.)


 

 




