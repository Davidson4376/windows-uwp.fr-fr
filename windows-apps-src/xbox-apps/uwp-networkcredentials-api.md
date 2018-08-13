---
author: WilliamsJason
title: Référence API les informations d’identification de périphérique portail réseau
description: Apprenez à ajouter, supprimer ou mettre à jour les informations d’identification réseau par programmation.
ms.localizationpriority: medium
ms.openlocfilehash: 7e00169f92ee6f0aa48df64ec4a1186f9682b358
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "410164"
---
# <a name="network-credentials-api-reference"></a>Référence de l’API des informations d’identification réseau
Vous pouvez ajouter, supprimer ou mettre à jour des informations d’identification réseau stockées sur le Kit de développement à l’aide de cette API REST.

## <a name="get-existing-credentials"></a>Obtenir des informations d’identification existantes

**Requête**

Vous pouvez obtenir une liste des actions stockées ainsi que le nom d’utilisateur de l’utilisateur qui dispose des informations d’identification de ce partage réseau.

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

- Tableau JSON dans le format suivant:
* Informations d’identification
  * NetworkPath - le chemin d’accès au partage réseau.
  * Nom d’utilisateur - le nom d’utilisateur qui dispose des informations d’identification stockées.

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
| NetworkPath        | Le chemin d’accès du partage réseau que vous ajoutez les informations d’identification pour accéder à. |
<br>

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Les éléments JSON suivants:
* NetworkPath - le chemin d’accès au partage réseau.
* Nom d’utilisateur - stocker les informations d’identification sous le nom d’utilisateur.
* Mot de passe - le mot de passe nouveaux ou mis à jour pour cet utilisateur.

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
| NetworkPath        | Le chemin d’accès réseau au partage à partir de laquelle vous voulez supprimer les informations d’identification stockées. |
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


