---
author: M-Stahl
title: Référence des API portail Xbox informations sur l’appareil
description: Découvrez comment accéder aux informations d’appareil Xbox.
ms.author: mstahl
ms.date: 11/7/2017
ms.topic: article
keywords: Windows 10, uwp, xbox, le portail d’appareil
ms.localizationpriority: medium
ms.openlocfilehash: 4b0e2bab0ce7d5525e8032809954ff656a74a61c
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7557229"
---
# <a name="xbox-info-api-reference"></a>Référence de l’API d’informations Xbox   
Vous pouvez accéder à Xbox One informations sur l’appareil à l’aide de cette API.

## <a name="get-xbox-one-device-information"></a>Obtenir des informations sur l’appareil Xbox One

**Requête**

Vous pouvez obtenir des informations sur l’appareil sur votre console Xbox One.

Méthode      | URI de la requête
:------     | :-----
GET | /ext/xbox/info
<br />
**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**   
Un objet JSON avec les champs suivants:

* OsVersion - (chaîne) la version du système d’exploitation.
* OsEdition - (chaîne) l’édition du système d’exploitation, par exemple, «Mars 2017» ou «mars 2017 correctif 1».
* ConsoleId - ID. de (chaîne) la console
* ID d’appareil - Xbox Live Device de (chaîne) la console ID.
* Numéro de série - numéro de série de (chaîne) la console.
* DevMode - Active le mode développeur de (chaîne) la console, par exemple, «None» ou «Commercial».
* ConsoleType - type de (chaîne) la console, par exemple, «Xbox One» ou «Xbox One S».
* DevkitCertificateExpirationTime - (nombre) de l’heure UTC en quelques secondes de la date d’expiration du certificat de kit de développement de la console.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Demande réussie
4XX | Codes d’erreur
5XX | Codes d’erreur

<br />
**Familles d’appareils disponibles**

* Windows Xbox