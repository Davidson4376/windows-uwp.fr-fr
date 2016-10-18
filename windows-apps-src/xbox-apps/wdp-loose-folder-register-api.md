---
author: WilliamsJason
title: "Référence sur les API d’inscription dans dossier isolé Device Portal"
description: "Découvrez comment accéder par programme aux API d’inscription dans des dossiers isolés."
translationtype: Human Translation
ms.sourcegitcommit: ef0f1339b77a8d1f60a677b2ff19a63b68f0d6cd
ms.openlocfilehash: 42dc5e16065f1a2252f63743269970cf85fb0b95

---

# Inscrire une application dans un dossier isolé  

**Requête**

Vous pouvez inscrire une application dans un dossier isolé en utilisant le format de requête suivant.

Méthode      | URI de la requête
:------     | :------
POST | /api/app/packagemanager/register
<br />
**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête:

Paramètre d’URI      | Description
:------     | :-----
folder (obligatoire) | Le nom du dossier de destination du package à inscrire. Ce dossier doit exister sous d:\developmentfiles\LooseApps sur la console. Ce nom de dossier doit être codé en base64, dans la mesure où il peut contenir des séparateurs de chemin d’accès si le dossier est un sous-dossier de LooseApps.
<br />

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Requête de déploiement acceptée et traitée
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Xbox

**Remarques**

Il existe au moins trois manières différentes d’obtenir l’application isolée sur la console dans le dossier souhaité. La méthode la plus simple consiste à copier les fichiers via SMB vers \\&lt;adresse_IP&gt;\DevelopmentFiles\LooseApps. Cela nécessite un nom d’utilisateur et un mot de passe sur les kits UWA qui peuvent être obtenus via [/ext/smb/developerfolder](wdp-smb-api.md). 

La deuxième méthode consiste à copier les fichiers individuels à l’emplacement adéquat en utilisant une commande POST vers /api/filesystem/apps/file, où knownfolderid est DevelopmentFiles, packagefullname est vide, et où le nom de fichier et le chemin d’accès sont fournis (le chemin d’accès doit commencer par LooseApps).

La troisième méthode consiste à copier un dossier complet en une fois via [/api/app/packagemanager/upload](wdp-folder-upload.md), où destinationFolder est le nom du dossier à placer sous d:\developmentfiles\looseapps et où la charge utile est un corps HTTP à parties multiples conforme du contenu du répertoire.




<!--HONumber=Aug16_HO3-->


