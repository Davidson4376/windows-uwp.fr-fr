---
author: payzer
title: "Référence des API des paramètres de développement Xbox Device Portal"
description: "Découvrez comment accéder aux paramètres de développement Xbox."
translationtype: Human Translation
ms.sourcegitcommit: c51eff41e63d815f6298b4fc46a9b11314bc8bc9
ms.openlocfilehash: 5a983714cda9b5a5f45e555e2cb6f980f082a003

---

# Informations de référence sur les API des paramètres de développement   
Cette API vous permet d’accéder aux paramètres Xbox One utiles pour le développement.

## Obtenir tous les paramètres de développement à la fois

**Requête**

Vous pouvez utiliser la requête suivante pour obtenir tous les paramètres de développement en une seule requête.

Méthode      | URI de la requête
:------     | :-----
GET | /ext/settings
<br />
**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**   
La réponse est un tableau de paramètres JSON contenant tous les paramètres. Chaque paramètre inclut les champs suivants:   

Name: (chaîne) le nom du paramètre.   
Value: (chaîne) la valeur du paramètre.   
RequiresReboot: («Oui» | «Non») ce champ indique si un redémarrage est nécessaire pour que le paramètre prenne effet.
Category: (chaîne) la catégorie du paramètre.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Demande réussie
4XX | Codes d’erreur
5XX | Codes d’erreur

## Obtenir les paramètres un par un
Les paramètres peuvent également être récupérés individuellement.

**Requête**

Vous pouvez utiliser la requête suivante pour obtenir des informations sur un paramètre individuel.

Méthode      | URI de la requête
:------     | :-----
GET | /ext/settings/\&lt;nom paramètre\&gt;
<br />
**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**   
La réponse est un objet JSON avec les champs suivants:   

Name: (chaîne) le nom du paramètre.   
Value: (chaîne) la valeur du paramètre.   
RequiresReboot: («Oui» | «Non») ce champ indique si un redémarrage est nécessaire pour que le paramètre prenne effet.
Category: (chaîne) la catégorie du paramètre.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Demande réussie
4XX | Codes d’erreur
5XX | Codes d’erreur

## Définir la valeur d’un paramètre
Vous pouvez définir la valeur d’un paramètre.

**Requête**

Vous pouvez utiliser la requête suivante pour définir la valeur d’un paramètre.

Méthode      | URI de la requête
:------     | :-----
PUT | /ext/settings/\&lt;nom paramètre\&gt;
<br />
**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**   
Le corps de la requête est un objet JSON contenant le champ suivant:   
Value: (chaîne) la nouvelle valeur du paramètre.

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




<!--HONumber=Aug16_HO3-->


