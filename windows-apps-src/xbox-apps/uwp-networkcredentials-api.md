---
title: Référence sur les API les informations d’identification réseau Device Portal
description: Découvrez comment ajouter, supprimer ou mettre à jour les informations d’identification réseau par programmation.
ms.localizationpriority: medium
ms.openlocfilehash: 2da8dae554a0dcbb84d3d3fc3873e2fb035175dc
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8213704"
---
# <a name="network-credentials-api-reference"></a>Référence de l’API d’informations d’identification réseau
Vous pouvez ajouter, supprimer ou mettre à jour les informations d’identification réseau stockées sur votre Kit de développement à l’aide de cette API REST.

## <a name="get-existing-credentials"></a>Obtenir des informations d’identification existantes

**Requête**

Vous pouvez obtenir une liste des actions stockées, ainsi que le nom d’utilisateur de l’utilisateur qui possède des informations d’identification pour ce partage réseau.

Méthode      | URI de la requête
:------     | :-----
GET | /ext/networkcredential
<br />
**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**   

- Aucun

**Réponse**   

- Tableau JSON au format suivant:
* Informations d’identification
  * NetworkPath - le chemin d’accès au partage réseau.
  * Nom d’utilisateur - le nom d’utilisateur qui a des informations d’identification stockées.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Réussite
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="add-or-update-stored-credentials-for-a-user"></a>Ajouter ou mettre à jour les informations d’identification stockées pour un utilisateur

**Requête**

Méthode      | URI de la requête
:------     | :-----
POST | /ext/networkcredential
<br />
**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête:

| Paramètre d’URI      | Description     | 
| ------------------ |-----------------|
| NetworkPath        | Le chemin d’accès réseau pour le partage de vous ajoutez les informations d’identification pour accéder à. |
<br>

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Les éléments JSON suivants:
* NetworkPath - le chemin d’accès au partage réseau.
* Nom d’utilisateur - le nom d’utilisateur pour stocker les informations d’identification sous.
* -Le mot de passe nouvelle ou mise à jour pour cet utilisateur.

**Réponse**   

- Aucun  

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
204 | Réussite
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="remove-stored-credentials-for-a-share"></a>Supprimer les informations d’identification stockées pour un partage.

**Requête**

Méthode      | URI de la requête
:------     | :-----
DELETE | /ext/networkcredential
<br />
**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête:

| Paramètre d’URI      | Description     | 
| ------------------ |-----------------|
| NetworkPath        | Le chemin d’accès réseau pour le partage à partir de laquelle vous supprimez les informations d’identification stockées. |
<br>

**En-têtes de requête**

- Aucun

**Corps de la requête**   

- Aucun

**Réponse**   

- Aucun 

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
204 | La demande les informations d’identification a réussi.
4XX | Codes d’erreur
5XX | Codes d’erreur

<br />
**Familles d’appareils disponibles**

* Windows Xbox


