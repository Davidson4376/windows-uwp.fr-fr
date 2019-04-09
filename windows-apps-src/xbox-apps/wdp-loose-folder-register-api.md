---
title: Référence sur les API d’inscription dans dossier isolé Device Portal
description: Découvrez comment accéder par programme aux API d’inscription dans des dossiers isolés.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: efdf4214-9738-4df6-bf1f-ed7141696ef6
ms.localizationpriority: medium
ms.openlocfilehash: 6e1a6d2e0c408d37195f5a4764f71c2acc932ab5
ms.sourcegitcommit: 681c1e3836d2a51cd3b31d824ece344281932bcd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59240007"
---
# <a name="register-an-app-in-a-loose-folder"></a>Inscrire une application dans un dossier isolé  

**Demande**

Vous pouvez inscrire une application dans un dossier isolé en utilisant le format de requête suivant.

Méthode      | URI de requête
:------     | :------
PUBLIER | /api/app/packagemanager/register

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI      | Description
:------     | :-----
folder (obligatoire) | Le nom du dossier de destination du package à inscrire. Ce dossier doit exister sous d:\developmentfiles\LooseApps sur la console. Ce nom de dossier doit être codé en base 64, dans la mesure où il peut contenir des séparateurs de chemin d’accès si le dossier est un sous-dossier de LooseApps.

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Requête de déploiement acceptée et traitée
4XX | Codes d’erreur
5XX | Codes d’erreur

**Familles d’appareils disponibles**

* Windows Xbox

**Notes**

Il existe au moins trois manières différentes d’obtenir l’application isolée sur la console dans le dossier souhaité. Le plus simple consiste à simplement copier les fichiers via SMB à \\< adresse_IP > \DevelopmentFiles\LooseApps. Cela nécessite un nom d’utilisateur et un mot de passe sur les kits UWA qui peuvent être obtenus via [/ext/smb/developerfolder](wdp-smb-api.md). 

La deuxième méthode consiste à copier les fichiers individuels à l’emplacement adéquat en utilisant une commande POST vers /api/filesystem/apps/file, où knownfolderid est DevelopmentFiles, packagefullname est vide, et où le nom de fichier et le chemin d’accès sont fournis (le chemin d’accès doit commencer par LooseApps).

La troisième méthode consiste à copier un dossier complet en une fois via [/api/app/packagemanager/upload](wdp-folder-upload.md), où destinationFolder est le nom du dossier à placer sous d:\developmentfiles\looseapps et où la charge utile est un corps HTTP à parties multiples conforme du contenu du répertoire.

