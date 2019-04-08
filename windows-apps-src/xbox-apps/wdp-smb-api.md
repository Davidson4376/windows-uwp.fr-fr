---
title: Informations de référence sur les API SMB Device Portal
description: Découvrez comment accéder par programme aux API SMB.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1f0eb76e-fe3e-4674-a27e-229beec7e63d
ms.localizationpriority: medium
ms.openlocfilehash: e248a6ff666efe7dca262daa81a21ab44a4dc5aa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617704"
---
# <a name="developer-folder-api-reference"></a>Informations de référence sur les API du dossier de développement   
Vous pouvez accéder aux fichiers de développement sur votre Xbox One à l’aide d’un explorateur de fichiers standard. Cela vous permet d’afficher et de remplacer facilement des fichiers sur la console, à partir de votre PC.

**Demande**

Vous pouvez accéder au dossier de développement en utilisant la requête suivante. La requête renvoie :    
* L’emplacement du partage de fichiers. Cet emplacement peut être saisi dans la barre d’adresses d’un explorateur de fichiers.
* Le nom d’utilisateur pour accéder au partage de fichiers.
* Le mot de passe pour accéder au partage de fichiers.

Méthode      | URI de requête
:------     | :-----
GET | /ext/smb/developerfolder
<br />
**Paramètres d’URI**

- Aucune

**En-têtes de demande**

- Aucune

**Corps de la demande**

- Aucune

**Réponse**   
Path : le chemin d’accès au partage des fichiers de développement.   
Username : le nom d’utilisateur requis pour accéder au partage des fichiers de développement.   
Password : le mot de passe requis pour accéder au partage des fichiers de développement.   

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | La requête pour accéder aux informations d’identification pour le partage de fichiers a été accordée.
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles de périphériques disponibles**

* Windows Xbox
