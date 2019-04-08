---
title: Informations de référence sur les API Device Portal Xbox
description: Découvrez comment accéder aux informations de référence Xbox.
ms.date: 11/072017
ms.topic: article
keywords: Windows 10, uwp, xbox, du portail de l’appareil
ms.localizationpriority: medium
ms.openlocfilehash: 85c2c139aa8064e1f0769064b95eeb531086b8c1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617494"
---
# <a name="xbox-info-api-reference"></a>Informations de référence sur les API Xbox   
Cette API vous permet d’accéder aux informations des périphériques Xbox One.

## <a name="get-xbox-one-device-information"></a>Obtenir les informations des périphériques Xbox One

**Demande**

Vous pouvez obtenir des informations relatives à votre périphérique Xbox One.

Méthode      | URI de requête
:------     | :-----
GET | /ext/xbox/info
<br />
**Paramètres d’URI**

- Aucune

**En-têtes de demande**

- Aucune

**Corps de la demande**

- Aucune

**Réponse**   
Un objet JSON avec les champs suivants :

* OsVersion - (chaîne) version du système d’exploitation.
* OsEdition - (chaîne) édition du système d’exploitation, telle que « mars 2017 » ou « mars 2017 QFE 1 ».
* ConsoleId - (chaîne) ID de la console.
* DeviceId - (chaîne) ID de périphérique Xbox Live de la console.
* Numéro de série - (chaîne) numéro de série de la console.
* DevMode - (chaîne) mode développeur actuel de la console, tels que « None » ou « Commercial ».
* ConsoleType - (chaîne) type de la console, tel que « Xbox One » ou « Xbox One S ».
* DevkitCertificateExpirationTime - (nombre) heure UTC, en secondes, de la date d’expiration du certificat du kit de développement de la console.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Demande réussie
4XX | Codes d’erreur
5XX | Codes d’erreur

<br />
**Familles de périphériques disponibles**

* Windows Xbox