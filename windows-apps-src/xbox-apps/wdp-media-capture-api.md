---
author: WilliamsJason
title: "Informations de référence sur les API de capture multimédia"
description: "Découvrez comment accéder à l’API de capture multimédia par programmation."
translationtype: Human Translation
ms.sourcegitcommit: 4356bd2cfc7697905ed91d60b5829c06d850e109
ms.openlocfilehash: 2f77c565acf272a1a8f147eb07ac6c7f08bf8ef0

---

# Informations de référence sur les API de capture multimédia #

**Requête**

Vous pouvez capturer une représentation PNG de l’écran actuel en utilisant le format de requête suivant.

| Méthode        | URI de la requête     | 
| ------------- |-----------------|
| GET           | /ext/screenshot |
<br>

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête:


| Paramètre d’URI      | Description     | 
| ------------------ |-----------------|
| download (facultatif)| Valeur booléenne qui indique si les en-têtes de réponse HTTP doivent définir que le navigateur hôte doit télécharger la capture d’écran en tant que pièce jointe, plutôt que de l’afficher dans le navigateur.  |
<br>

**En-têtes de requête**

* Aucun

**Corps de la requête**

* Aucun

###Réponse###

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP   | Description     | 
| ------------------ |-----------------|
| 200                | Demande de capture d’écran réussie et capture renvoyée |
| 5XX                | Codes d’erreur des échecs inattendus |
<br>

**Familles d’appareils disponibles**

* Windows Xbox




<!--HONumber=Aug16_HO3-->


