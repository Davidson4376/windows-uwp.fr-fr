---
title: Informations de référence sur les API de gestion des utilisateurs test Xbox Live
description: Découvrez comment accéder par programme aux API de gestion des utilisateurs.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 70876ab6-8222-4940-b4fb-65b581a77d6a
ms.openlocfilehash: c934a88dd1825fb0111083d71eb25e477956d79c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627364"
---
#<a name="xbox-live-user-management"></a>Gestion de Xbox Live utilisateur #

**Demande**

Vous pouvez obtenir la liste des utilisateurs sur la console, ou mettre à jour la liste en ajoutant des utilisateurs ou en supprimant, connectant, déconnectant ou modifiant des utilisateurs existants.

| Méthode        | URI de requête     | 
| ------------- |-----------------|
| GET           | /ext/user |
| PUT           | /ext/user |
<br>

**Paramètres d’URI**

* Aucune

**En-têtes de demande**

* Aucune

**Corps de la demande**

Les appels à PUT doivent inclure un tableau JSON ayant la structure suivante :

* Utilisateurs
  * AutoSignIn (facultatif) : valeur booléenne permettant la désactivation ou l’activation de l’ouverture automatique de session pour le compte spécifié dans EmailAddress ou UserId.
  * EmailAddress (facultatif : doit être fourni si le nom d’utilisateur n’est pas fourni, sauf si la connexion utilisateur sponsorisé) : Adresse de messagerie en spécifiant l’utilisateur à modifier/ajouter/supprimer.
  * Mot de passe (facultatif : doit être fournie si l’utilisateur n’est pas actuellement sur la console) : Mot de passe utilisé pour ajouter un nouvel utilisateur à la console.
  * SignedIn (facultatif) : valeur booléenne indiquant si le compte fourni doit être connecté ou déconnecté.
  * ID d’utilisateur (facultatif : doit être fourni si EmailAddress n’est pas fourni, sauf si la connexion utilisateur sponsorisé) : ID d’utilisateur en spécifiant l’utilisateur à modifier/ajouter/supprimer.
  * SponsoredUser (facultatif) : valeur booléenne indiquant s’il convient d’ajouter un utilisateur sponsorisé.
  * Delete (facultatif) : valeur booléenne qui spécifie pour supprimer cet utilisateur à partir de la console

###<a name="response"></a>Réponse ###

**Corps de la réponse**

Les appels à GET renvoient un tableau JSON ayant les propriétés suivantes :

* Utilisateurs
  * AutoSignIn (facultatif)
  * EmailAddress (facultatif)
  * Gamertag
  * SignedIn
  * UserId
  * XboxUserId
  * SponsoredUser (facultatif)
  
**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP   | Description     | 
| ------------------ |-----------------|
| 200                | L’appel à GET a réussi et le tableau JSON des utilisateurs a été renvoyé dans le corps de réponse. |
| 204                | L’appel à PUT a réussi et les utilisateurs sur la console ont été mis à jour. |
| 4XX                | Diverses erreurs de format ou de données de requête non valides |
| 5XX                | Codes d’erreur des échecs inattendus |
<br>


