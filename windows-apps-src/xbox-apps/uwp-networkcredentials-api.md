---
title: Informations de référence sur les API d’identification réseau de Device Portal
description: Découvrez comment ajouter, supprimer ou mettre à jour les informations d’identification réseau par programmation.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: ac30d8db830c51ee40653feb49b443ed44502617
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659214"
---
# <a name="network-credentials-api-reference"></a>Informations de référence sur les API d’identification réseau
L’API REST vous permet d’ajouter, supprimer ou mettre à jour les informations d’identification réseau stockées dans votre kit de développement.

## <a name="get-existing-credentials"></a>Obtenir des informations d’identification existantes

**Demande**

Vous pouvez obtenir une liste des actions stockées, ainsi que le nom de l’utilisateur disposant d’informations d’identification pour ce partage réseau.

Méthode      | URI de requête
:------     | :-----
GET | /ext/networkcredential
<br />
**Paramètres d’URI**

- Aucune

**En-têtes de demande**

- Aucune

**Corps de la demande**   

- Aucune

**Réponse**   

- Tableau JSON au format suivant :
* Informations d’identification
  * NetworkPath - le chemin d’accès au partage réseau.
  * Username - nom d’utilisateur propriétaire des informations d’identification stockées.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Réussite
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="add-or-update-stored-credentials-for-a-user"></a>Ajouter ou mettre à jour les informations d’identification stockées pour un utilisateur

**Demande**

Méthode      | URI de requête
:------     | :-----
POST | /ext/networkcredential
<br />
**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI      | Description     | 
| ------------------ |-----------------|
| NetworkPath        | Le chemin d’accès réseau du partage pour lequel vous ajoutez des informations d’identification. |
<br>

**En-têtes de demande**

- Aucune

**Corps de la demande**

- Les éléments JSON suivants :
* NetworkPath - le chemin d’accès au partage réseau.
* Username - nom d’utilisateur sous lequel sont stockées les informations d’identification.
* Password - mot de passe nouveau ou mis à jour pour cet utilisateur.

**Réponse**   

- Aucune  

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
204 | Réussite
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="remove-stored-credentials-for-a-share"></a>Supprime les informations d’identification stockées pour un partage.

**Demande**

Méthode      | URI de requête
:------     | :-----
DELETE | /ext/networkcredential
<br />
**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI      | Description     | 
| ------------------ |-----------------|
| NetworkPath        | Le chemin d’accès réseau au partage duquel vous supprimez les informations d’identification stockées. |
<br>

**En-têtes de demande**

- Aucune

**Corps de la demande**   

- Aucune

**Réponse**   

- Aucune 

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
204 | La demande des informations d’identification a réussi.
4XX | Codes d’erreur
5XX | Codes d’erreur

<br />
**Familles de périphériques disponibles**

* Windows Xbox


