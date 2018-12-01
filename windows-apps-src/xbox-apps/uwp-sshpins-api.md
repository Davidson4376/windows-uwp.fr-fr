---
title: Informations de référence sur les API de code confidentiel SSH Device Portal
description: Découvrez comment supprimer par programmation tous les codes confidentiels SSH approuvés.
ms.localizationpriority: medium
ms.openlocfilehash: 1ddf15d3cdb4089a8ef010a4ae46d247a06a10d7
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8335520"
---
# <a name="ssh-pins-api-reference"></a>Informations de référence sur les codes confidentiels SSH API
Vous pouvez supprimer tous les codes confidentiels SSH approuvés sur votre Kit de développement à l’aide de cette API REST.

## <a name="remove-trusted-ssh-pins"></a>Supprimer les codes confidentiels SSH approuvés

**Requête**

Méthode      | URI de la requête
:------     | :-----
DELETE | /ext/app/sshpins
<br />
**Paramètres d’URI**

- Aucun

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
204 | La demande d'effacement des codes confidentiels a réussi.
4XX | Codes d’erreur
5XX | Codes d’erreur

<br />
**Familles d’appareils disponibles**

* Windows Xbox

