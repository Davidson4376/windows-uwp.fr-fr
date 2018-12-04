---
title: Référence sur les API les informations d’identification réseau Device Portal
description: Apprenez à ajouter, supprimer ou mettre à jour les informations d’identification réseau par programmation.
ms.localizationpriority: medium
ms.openlocfilehash: 2da8dae554a0dcbb84d3d3fc3873e2fb035175dc
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8469065"
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
  * Username: le nom d’utilisateur qui a des informations d’identification stockées.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Réussite
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="add-or-update-stored-credentials-for-a-user"></a>Ajouter ou mettre à jour les informations d’identification stockées d’un utilisateur

**Requête**

Méthode      | URI de la requête
:------     | :-----
POST | /ext/networkcredential
<br />
**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête:

| Paramètre d’URI      | Description     | 
| ------------------ |-----------------|
| NetworkPath        | Le chemin d’accès du partage réseau vous ajoutez les informations d’identification pour accéder à. |
<br>

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Les éléments JSON suivants:
* NetworkPath - le chemin d’accès au partage réseau.
* Username: le nom d’utilisateur pour stocker les informations d’identification sous.
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

## <a name="remove-stored-credentials-for-a-share"></a>Supprimez les informations d’identification stockées pour un partage.

**Requête**

Méthode      | URI de la requête
:------     | :-----
DELETE | /ext/networkcredential
<br />
**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête:

| Paramètre d’URI      | Description     | 
| ------------------ |-----------------|
| NetworkPath        | Le chemin d’accès réseau pour le partage à partir duquel vous supprimez les informations d’identification stockées. |
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


