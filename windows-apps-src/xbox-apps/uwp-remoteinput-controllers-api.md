---
title: Informations de référence sur les API de contrôleurs de Device Portal
description: Découvrez comment obtenir le nombre de contrôleurs physiques attachés et les désactiver par programmation.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8b5061f9193d78d4ff23f5fa707b0bea67a10f98
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657004"
---
# <a name="controller-api-reference"></a>Référence API du contrôleur   
Vous pouvez obtenir le nombre de contrôleurs physiques attachés et les désactiver à l'aide de cette API REST.

## <a name="determine-the-number-of-attached-physical-controllers"></a>Déterminer le nombre de contrôleurs physiques attachés

**Demande**

Vous pouvez vérifier le nombre de contrôleurs physiques attachés à l’appareil à l’aide de la requête suivante.

Méthode      | URI de requête
:------     | :-----
GET | /ext/remoteinput/controllers
<br />
**Paramètres d’URI**

- Aucune

**En-têtes de demande**

- Aucune

**Corps de la demande**   

- Aucune

**Réponse**   

- Propriété de nombre JSON ConnectedControllerCount qui spécifie le nombre de contrôleurs physiques attachés.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Réussite
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="disconnect-all-physical-controllers-on-the-devkit"></a>Déconnectez tous les contrôleurs physiques sur le Kit de développement

**Demande**

Vous pouvez déconnecter tous les contrôleurs physiques sur l’appareil à l’aide de la demande suivante.

Méthode      | URI de requête
:------     | :-----
DELETE | /ext/remoteinput/controllers
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
204 | La demande de déconnexion des contrôleurs a réussi.
4XX | Codes d’erreur
5XX | Codes d’erreur

<br />
**Familles de périphériques disponibles**

* Windows Xbox
