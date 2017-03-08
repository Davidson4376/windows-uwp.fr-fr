---
author: WilliamsJason
title: "Infos de référence sur les API de chargement du dossier Device Portal"
description: "Découvrez comment accéder par programme aux API de chargement des dossiers."
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: e1a2c7f0-0040-4ce7-94de-17224736e20b
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 8c2251c5c78fed10959b89de0d81ff563d0fa3e3
ms.lasthandoff: 02/08/2017

---

# <a name="upload-a-folder-to-the-development-directory"></a>Télécharger un dossier dans le répertoire de développement

**Requête**

Vous pouvez charger un dossier complet en une fois vers l’ID de dossier connu pour les fichiers de développement (ou dans un sous-dossier de ce dossier).

Méthode      | URI de la requête
:------     | :------
POST | /api/app/packagemanager/upload 
<br />
**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI      | Description
:------     | :-----
destinationFolder (obligatoire) | Le nom du dossier de destination du dossier à charger. Ce dossier sera placé sous d:\developmentfiles\LooseApps sur la console. Ce nom de dossier doit être codé en base 64, dans la mesure où il peut contenir des séparateurs de chemin d’accès si le dossier est un sous-dossier de LooseApps.
<br />

**En-têtes de requête**

- Aucun

**Corps de la requête**

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
**Familles d’appareils disponibles**

* Windows Xbox


