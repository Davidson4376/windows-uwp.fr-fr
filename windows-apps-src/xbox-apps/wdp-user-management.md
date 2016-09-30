---  
author: WilliamsJason
title: "Informations de référence sur les API de gestion des utilisateurs test Xbox Live"
description: "Découvrez comment accéder par programme aux API de gestion des utilisateurs."
translationtype: Human Translation
ms.sourcegitcommit: 67f158b1d3d5ece14c36483a2513a2db2f478660
ms.openlocfilehash: ad01d4daf089c61fc50c7927cfbf123d7d7ee4df

---  

#Gestion des utilisateurs Xbox Live#

**Requête**

Vous pouvez obtenir la liste des utilisateurs sur la console, ou mettre à jour la liste en ajoutant des utilisateurs ou en supprimant, connectant, déconnectant ou modifiant des utilisateurs existants.

| Méthode        | URI de la requête     | 
| ------------- |-----------------|
| GET           | /ext/user |
| PUT           | /ext/user |
<br>

**Paramètres d’URI**

* Aucun

**En-têtes de requête**

* Aucun

**Corps de la requête**

Les appels à PUT doivent inclure un tableau JSON ayant la structure suivante:

* Utilisateurs
  * AutoSignIn (facultatif): valeur booléenne permettant la désactivation ou l’activation de l’ouverture automatique de session pour le compte spécifié dans EmailAddress ou UserId.
  * EmailAddress (facultatif, doit être fourni si UserId n’est pas fourni, sauf si vous connectez un utilisateur sponsorisé): adresse de messagerie indiquant l’utilisateur à modifier/ajouter/supprimer.
  * Password (facultatif, doit être fourni si l’utilisateur n’est pas actuellement sur la console): mot de passe utilisé pour ajouter un nouvel utilisateur à la console.
  * SignedIn (facultatif): valeur booléenne indiquant si le compte fourni doit être connecté ou déconnecté.
  * UserId (facultatif, doit être fourni si EmailAddress n’est pas fourni, sauf si vous connectez un utilisateur sponsorisé): ID utilisateur indiquant l’utilisateur à modifier/ajouter/supprimer.
  * SponsoredUser (facultatif): valeur booléenne indiquant s’il convient d’ajouter un utilisateur sponsorisé.
  * Delete (facultatif): valeur booléenne indiquant de supprimer cet utilisateur de la console.

###Réponse###

**Corps de la réponse**

Les appels à GET renvoient un tableau JSON ayant les propriétés suivantes:

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





<!--HONumber=Jul16_HO1-->


