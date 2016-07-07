---
author: payzer
title: "Informations de référence sur les API SMB Device Portal"
description: "Découvrez comment accéder par programme aux API SMB."
ms.sourcegitcommit: 3d76bf181baa9dfd973467d43241230fddf2daf7
ms.openlocfilehash: 5efe2af3524d97e6014c4d6be2a8f1aef22f2e66

---

# Informations de référence sur les API du dossier de développement   
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



<!--HONumber=Jun16_HO4-->


