---
author: WilliamsJason
title: "Infos de référence sur les API de chargement du dossier Device Portal"
description: "Découvrez comment accéder par programme aux API de chargement des dossiers."
translationtype: Human Translation
ms.sourcegitcommit: fdc25fa4bd7bd5bfa598b993f23cd0ae9783dd0e
ms.openlocfilehash: 6c3eeccdbb2bca315dc84293a36d0923f46e746d

---

# Télécharger un dossier dans le répertoire de développement

**Requête**

Vous pouvez charger un dossier complet en une fois vers l’ID de dossier connu pour les fichiers de développement (ou dans un sous-dossier de ce dossier).

Méthode      | URI de la requête
:------     | :------
POST | /api/app/packagemanager/upload 
<br />
**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête:

Paramètre d’URI      | Description
:------     | :-----
destinationFolder (obligatoire) | Le nom du dossier de destination du dossier à charger. Ce dossier sera placé sous d:\developmentfiles\LooseApps sur la console. Ce nom de dossier doit être codé en base64, dans la mesure où il peut contenir des séparateurs de chemin d’accès si le dossier est un sous-dossier de LooseApps.
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




<!--HONumber=Aug16_HO3-->


