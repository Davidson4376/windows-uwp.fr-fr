---
author: M-Stahl
title: Référence des API de portail Xbox Infos sur l’appareil
description: Découvrez comment accéder aux informations sur le périphérique Xbox.
ms.author: mstahl
ms.date: 11/7/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, xbox, portail de périphérique
ms.localizationpriority: medium
ms.openlocfilehash: db1df2418a2bb60de4a72f51ad01a0bfd547ec20
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "406257"
---
# <a name="xbox-info-api-reference"></a>Référence de l’API d’infos sur Xbox   
Vous pouvez accéder d’informations Xbox un périphérique à l’aide de cette API.

## <a name="get-xbox-one-device-information"></a>Obtenir des informations de périphérique un Xbox

**Requête**

Vous pouvez obtenir des informations sur le périphérique sur votre une Xbox.

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

* OsVersion - (chaîne) de la version du système d’exploitation.
* OsEdition - (String) l’édition du système d’exploitation, tel que «Mars 2017» ou «mars 2017 QFE 1».
* ConsoleId - ID. de (String) la console
* ID de périphérique - Xbox Live périphérique de (String) la console ID.
* Numéro de série - numéro de série la console de () la chaîne.
* DevMode - développeur mode actuel du (String) la console, tel que «None» ou «Retail».
* ConsoleType - type du (String) la console, tel que «Un Xbox» ou «Xbox un S».
* DevkitCertificateExpirationTime - (nombre) le GMT en secondes, la date d’expiration certificat de kit de développement de la console.

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