---
title: Informations de référence sur les API de capture multimédia
description: Découvrez comment accéder à l’API de capture multimédia par programmation.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: 7dcd4c6c39a983ab11bfacd391bfa78942601258
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244055"
---
# <a name="media-capture-api-reference"></a>Informations de référence sur les API de capture multimédia #

## <a name="request"></a>Demande

Vous pouvez capturer une représentation PNG de l’écran actuel en utilisant le format de requête suivant.

| Méthode        | URI de requête     | 
| ------------- |-----------------|
| GET           | /ext/screenshot |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :


| Paramètre d’URI      | Description     | 
| ------------------ |-----------------|
| download (facultatif)| Valeur booléenne qui indique si les en-têtes de réponse HTTP doivent définir que le navigateur hôte doit télécharger la capture d’écran en tant que pièce jointe, plutôt que de l’afficher dans le navigateur.  |

**En-têtes de requête**

* Aucune

**Corps de la requête**

* Aucune

## <a name="response"></a>Réponse

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d’état HTTP   | Description     | 
| ------------------ |-----------------|
| 200                | Demande de capture d’écran réussie et capture renvoyée |
| 5XX                | Codes d’erreur des échecs inattendus |
<br>

**Familles d’appareils disponibles**

* Windows Xbox

