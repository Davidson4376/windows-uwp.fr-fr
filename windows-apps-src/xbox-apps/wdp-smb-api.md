---
author: payzer
title: Informations de référence sur les API SMB Device Portal
description: Découvrez comment accéder par programme aux API SMB.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 1f0eb76e-fe3e-4674-a27e-229beec7e63d
ms.openlocfilehash: 1bc02780808d5b9fca09576165f428eca1cce715
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.locfileid: "230083"
---
# <a name="developer-folder-api-reference"></a>Informations de référence sur les API du dossier de développement   
Vous pouvez accéder aux fichiers de développement sur votre Xbox One à l’aide d’un explorateur de fichiers standard. Cela vous permet d’afficher et de remplacer facilement des fichiers sur la console, à partir de votre PC.

**Requête**

Vous pouvez accéder au dossier de développement en utilisant la requête suivante. La requête renvoie:    
* L’emplacement du partage de fichiers. Cet emplacement peut être saisi dans la barre d’adresses d’un explorateur de fichiers.
* Le nom d’utilisateur pour accéder au partage de fichiers.
* Le mot de passe pour accéder au partage de fichiers.

Méthode      | URI de la requête
:------     | :-----
GET | /ext/smb/developerfolder
<br />
**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**   
Path: le chemin d’accès au partage des fichiers de développement.   
Username: le nom d’utilisateur requis pour accéder au partage des fichiers de développement.   
Password: le mot de passe requis pour accéder au partage des fichiers de développement.   

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | La requête pour accéder aux informations d’identification pour le partage de fichiers a été accordée.
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />
**Familles d’appareils disponibles**

* Windows Xbox
