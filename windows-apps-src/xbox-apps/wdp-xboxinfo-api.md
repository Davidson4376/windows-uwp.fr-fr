---
title: Informations sur l’appareil Xbox portail référence d’API
description: Découvrez comment accéder aux informations sur l’appareil Xbox.
ms.date: 11/7/2017
ms.topic: article
keywords: Windows 10, uwp, xbox, le portail d’appareil
ms.localizationpriority: medium
ms.openlocfilehash: d7901890e1cc8fab24742e8785562d13d2fe182a
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8749944"
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
* DevkitCertificateExpirationTime - (nombre) de l’heure UTC en secondes, la date d’expiration certificat de kit de développement de la console.

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