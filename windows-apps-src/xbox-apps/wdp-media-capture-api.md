---
author: WilliamsJason
title: Informations de référence sur les API de capture multimédia
description: Découvrez comment accéder à l’API de capture multimédia par programmation.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: f58fa4c3a9a1abd407f635f27de3a545c3aafc6c
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7556552"
---
# <a name="media-capture-api-reference"></a>Informations de référence sur les API de capture multimédia #

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

###<a name="response"></a>Réponse n°

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP   | Description     | 
| ------------------ |-----------------|
| 200                | Demande de capture d’écran réussie et capture renvoyée |
| 5XX                | Codes d’erreur des échecs inattendus |
<br>

**Familles d’appareils disponibles**

* Windows Xbox

