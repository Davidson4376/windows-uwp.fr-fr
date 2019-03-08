---
title: Informations de référence sur les API de code confidentiel SSH Device Portal
description: Découvrez comment supprimer par programmation tous les codes confidentiels SSH approuvés.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 2c7dc6fab021c11c98276ee53af161bea25601a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663354"
---
# <a name="ssh-pins-api-reference"></a>Informations de référence sur les codes confidentiels SSH API
Vous pouvez supprimer tous les codes confidentiels SSH approuvés sur votre Kit de développement à l’aide de cette API REST.

## <a name="remove-trusted-ssh-pins"></a>Supprimer les codes confidentiels SSH approuvés

**Demande**

Méthode      | URI de requête
:------     | :-----
DELETE | /ext/app/sshpins
<br />
**Paramètres d’URI**

- Aucune

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
204 | La demande d'effacement des codes confidentiels a réussi.
4XX | Codes d’erreur
5XX | Codes d’erreur

<br />
**Familles de périphériques disponibles**

* Windows Xbox

