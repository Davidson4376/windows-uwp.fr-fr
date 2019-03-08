---
title: Infos de référence sur les API de chargement du dossier Device Portal
description: Découvrez comment accéder par programme aux API de chargement des dossiers.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e1a2c7f0-0040-4ce7-94de-17224736e20b
ms.localizationpriority: medium
ms.openlocfilehash: 0805dbeedcf66bc3596f3d284f51e8f177608396
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617604"
---
# <a name="upload-a-folder-to-the-development-directory"></a>Télécharger un dossier dans le répertoire de développement

**Demande**

Vous pouvez charger un dossier complet en une fois vers l’ID de dossier connu pour les fichiers de développement (ou dans un sous-dossier de ce dossier).

Méthode      | URI de requête
:------     | :------
POST | /api/app/packagemanager/upload 
<br />
**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI      | Description
:------     | :-----
destinationFolder (obligatoire) | Le nom du dossier de destination du dossier à charger. Ce dossier sera placé sous d:\developmentfiles\LooseApps sur la console. Ce nom de dossier doit être codé en base 64, dans la mesure où il peut contenir des séparateurs de chemin d’accès si le dossier est un sous-dossier de LooseApps.
<br />

**En-têtes de demande**

- Aucune

**Corps de la demande**

- Corps HTTP à parties multiples conforme du contenu du répertoire.

**Réponse**

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

