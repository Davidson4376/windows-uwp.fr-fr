---
title: Informations de référence sur les API de contrôleurs de Device Portal
description: Découvrez comment obtenir le nombre de contrôleurs physiques attachés et les désactiver par programmation.
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8b5061f9193d78d4ff23f5fa707b0bea67a10f98
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8929834"
---
# <a name="controller-api-reference"></a>Référence API du contrôleur   
Vous pouvez obtenir le nombre de contrôleurs physiques attachés et les désactiver à l'aide de cette API REST.

## <a name="determine-the-number-of-attached-physical-controllers"></a>Déterminer le nombre de contrôleurs physiques attachés

**Requête**

Vous pouvez vérifier le nombre de contrôleurs physiques attachés à l’appareil à l’aide de la requête suivante.

Méthode      | URI de la requête
:------     | :-----
GET | /ext/remoteinput/controllers
<br />
**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**   

- Aucun

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

**Requête**

Vous pouvez déconnecter tous les contrôleurs physiques sur l’appareil à l’aide de la demande suivante.

Méthode      | URI de la requête
:------     | :-----
DELETE | /ext/remoteinput/controllers
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
204 | La demande de déconnexion des contrôleurs a réussi.
4XX | Codes d’erreur
5XX | Codes d’erreur

<br />
**Familles d’appareils disponibles**

* Windows Xbox
