---
title: Informations de référence sur les API de déploiement de Device Portal
description: Découvrez comment accéder aux API d’informations de déploiement par programmation.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 7543b41c6ee1d9c07f4540012f84dccc10bb4d76
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8922779"
---
# <a name="requests-deployment-information-for-one-or-more-installed-packages"></a>Nécessite des informations de déploiement pour un ou plusieurs packages installés.

**Requête**

Méthode      | URI de la requête
:------     | :------
POST | /ext/app/deployinfo
<br />
**Paramètres d’URI**

 - Aucun

**En-têtes de requête**

- Aucun

**Corps de requête**

Tableau JSON au format suivant:

* DeployInfo
  * PackageFullName - nom du package à propos duquel nous demandons des informations.
  * OverlayFolder - chemin d’accès facultatif vers un chemin d’accès du dossier de superposition si nous utilisons cette fonctionnalité.

###<a name="response"></a>Réponse

**Corps de réponse**

Un tableau JSON au format suivant (certains champs sont facultatifs):

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

**Familles d’appareils disponibles**

* Windows Xbox