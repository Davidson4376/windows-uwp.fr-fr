---
author: jnHs
Description: "Consultez cette liste pour éviter les problèmes qui empêchent souvent les applications d’être certifiées, ou qui peuvent être identifiés au cours d’une vérification ponctuelle après la publication de l’application."
title: "Éviter les échecs de certification courants"
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 6af2842eeb5eeebfc9ffc5a7a3ec98fdcebddb74

---

# Éviter les échecs de certification courants


Consultez cette liste pour éviter les problèmes qui empêchent souvent les applications d’être certifiées, ou qui peuvent être identifiés au cours d’une vérification ponctuelle après la publication de l’application.

> **Remarque** Reportez-vous également aux [Politiques du Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944) pour vous assurer que votre application remplit toutes les obligations spécifiées.

 

-   Ne soumettez votre application que lorsqu’elle est terminée. Nous vous invitons à utiliser la description de votre application pour signaler les fonctionnalités à venir. Toutefois, assurez-vous que votre application ne contient pas de sections incomplètes, de liens vers des pages web en construction, ni toute autre chose qui donnerait au client l'impression que votre application est incomplète.

-   [Procédez au test de votre application à l’aide du Kit de certification des applications Windows](https://msdn.microsoft.com/library/windows/apps/mt186449) avant de la soumettre.

-   Testez votre application dans différentes configurations pour vérifier qu'elle est aussi stable que possible.

-   Assurez-vous que votre application ne se bloque pas sans connectivité réseau. Même si une connexion est requise pour utiliser réellement votre application, cette dernière doit fonctionner correctement en l’absence de connexion.
-   Vérifiez que la description de votre application indique clairement ce que fait votre application. Si vous avez besoin d'aide, consultez nos recommandations en matière de [rédaction d'une description d'application attractive](write-a-great-app-description.md).

-   Veillez à fournir des réponses complètes et précises à l'ensemble des questions dans la section [Classification par âge](age-ratings.md).

-   Si votre application utilise les API de commerce à partir de l’espace de noms [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197), prenez soin de tester l’application et de vérifier qu’elle gère les exceptions par défaut. Assurez-vous également que votre application utilise la classe [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) (et non la classe [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766), qui est uniquement fournie à des fins de test).

-   Ne [déclarez pas votre application comme accessible](app-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines) à moins de l’avoir spécifiquement conçue et testée pour des scénarios d’accessibilité.

-   Veillez à [fournir toutes les informations nécessaires](notes-for-certification.md) pour utiliser votre application, telles que le nom d’utilisateur et le mot de passe d’un compte de test si votre application requiert que les utilisateurs se connectent à un service, ou toutes les étapes requises pour accéder aux fonctionnalités masquées ou verrouillées.

 

 







<!--HONumber=Jun16_HO4-->


