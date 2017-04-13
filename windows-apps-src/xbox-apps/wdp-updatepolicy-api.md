---
author: payzer
title: "Informations de référence sur l’API de stratégie de mise à jour du Kit de développement Device Portal pour Xbox"
description: "Découvrez comment définir par programmation la stratégie de mise à jour de votre console."
ms.openlocfilehash: f9313d3c8b93ba13074c547f1f63c9f3204f0f58
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
REMARQUE: Cette API sera disponible dans la prochaine préversion pour les développeurs.

# <a name="system-update-policy-api-reference"></a>Informations de référence sur l’API de stratégie de mise à jour du système   
Vous pouvez utiliser cette API pour savoir quelle stratégie de mise à jour s’applique à votre console et remplacer la stratégie de mise à jour actuelle par une nouvelle.

IMPORTANT: La plupart des consoles reçoivent une réponse «accès refusé» lors d’une tentative d’appel de cette API. Toutes les consoles de développement n’ont en effet pas la possibilité de modifier leur stratégie de mise à jour.

Cette API affecte la stratégie de mise à jour pour les consoles en mode développeur, pas pour les consoles en mode commercial.

## <a name="get-the-console-update-policy"></a>Obtenir la stratégie de mise à jour de la console

**Requête**

Vous pouvez utiliser la requête suivante pour obtenir la stratégie de mise à jour de votre console.

Méthode      | URI de la requête
:------     | :-----
GET | /ext/update/policy
<br />
**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**   
La réponse est un tableau JSON contenant les groupes de mise à jour du système auxquels la console appartient. Chaque objet comprend les champs suivants:   

ID - (Chaîne) ID du groupe de mise à jour.   
Nom convivial - (Chaîne) Nom d’affichage du groupe de mise à jour.   
Description - («chaîne») Description du groupe de mise à jour.
IsDevKitGroup - (true | false) Indique si le groupe de mise à jour s’adresse aux builds développeurs.
ResourceSetID - (Chaîne) Ignore - utilisé par l’infrastructure de mise à jour du système.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Demande réussie
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="set-a-consoles-system-update-policy"></a>Définir la stratégie de mise à jour du système d’une console
Vous pouvez utiliser cette API pour modifier les groupes de mise à jour du système auxquels la console appartient.

Remarque: Les consoles ne peuvent appartenir qu’à un groupe de mise à jour du système à la fois.

**Requête**

Vous pouvez utiliser la requête suivante pour définir le groupe de mise à jour du système auquel une console appartient.

Méthode      | URI de la requête
:------     | :-----
POST | /ext/update/policy
<br />
**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**   
Le corps de la requête est un objet JSON contenant les champs suivants:   
GroupIdToJoin - (Chaîne) ID du groupe de mise à jour système que la console doit rejoindre.  
GroupIdToLeave - (Chaîne) ID du groupe de mise à jour du système que la console doit quitter.

Tous les champs sont obligatoires.

Les ID de groupe peuvent avoir les valeurs suivantes:   
* Aucune mise à jour - «b2dbed33-2845-44cc-a7a1-4a9afb29d8d9»   
* Dernière récupération de production - «7432ae99-8c09-48dd-99f9-a2697499e240»   
* Dernière récupération de préversion - «a8153054-1a1b-47cc-acc9-9aed90c1f8db»    

**Réponse**   

- Aucun

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Demande réussie
4XX | Codes d’erreur
5XX | Codes d’erreur

<br />
**Familles d’appareils disponibles**

* Windows Xbox

