---
title: Informations de référence sur les API de déploiement de Device Portal
description: Découvrez comment accéder aux API d’informations de déploiement par programmation.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 7543b41c6ee1d9c07f4540012f84dccc10bb4d76
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57638004"
---
# <a name="requests-deployment-information-for-one-or-more-installed-packages"></a>Nécessite des informations de déploiement pour un ou plusieurs packages installés.

**Demande**

Méthode      | URI de requête
:------     | :------
POST | /ext/app/deployinfo
<br />
**Paramètres d’URI**

 - Aucune

**En-têtes de demande**

- Aucune

**Corps de la demande**

Tableau JSON au format suivant :

* DeployInfo
  * PackageFullName - nom du package à propos duquel nous demandons des informations.
  * OverlayFolder - chemin d’accès facultatif vers un chemin d’accès du dossier de superposition si nous utilisons cette fonctionnalité.

###<a name="response"></a>Réponse

**Corps de la réponse**

Un tableau JSON au format suivant (certains champs sont facultatifs) :

* DeployInfo
  * PackageFullName - nom du package à propos duquel nous recevons des informations.
  * DeployType - type du déploiement.
  * DeployPathOrSpecifiers - chemin d’accès destiné aux déploiements libres ou aux spécificateurs installés pour les déploiements en packages.
  * DeployDrive - le lecteur sur lequel le package est déployé pour les types de déploiement applicables.
  * DeploySizeInBytes - la taille (en octets) du package pour les types de déploiement applicables.
  * OverlayFolder - le dossier de superposition pour les déploiements qui prennent en charge cette fonctionnalité.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Réussite
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />

**Familles de périphériques disponibles**

* Windows Xbox