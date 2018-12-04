---
title: Référence API du moniteur HTTP du portail d'appareil
description: Découvrez comment accéder au trafic HTTP depuis l'application focalisée sur une Xbox.
ms.localizationpriority: medium
ms.openlocfilehash: 81de2a2a3194384e9c5de1c5c45a827e4d965c91
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8476160"
---
# <a name="http-monitor-api-reference"></a>Référence API du moniteur HTTP   
Vous pouvez accéder au trafic HTTP en temps réel pour les applications focalisées à l'aide de cet API si le moniteur HTTP a été activé sur la console Xbox en cochant la case dans l'accueil du développeur.

## <a name="get-if-the-http-monitor-is-enabled"></a>Déterminer si le moniteur HTTP est activé

**Requête**

Vous pouvez découvrir si le moniteur HTTP est activé dans l'accueil du développeur.

Méthode      | URI de la requête
:------     | :-----
GET | /ext/httpmonitor/sessions
<br />
**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**   
Un objet JSON avec les champs suivants:

* Activé - (Bool) Si le moniteur HTTP est activé sur la console Xbox en cochant la case dans l'accueil du développeur.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Demande réussie
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="get-http-traffic-from-the-focused-app"></a>Obtenir le trafic HTTP à partir de l'application focalisée
**Requête**

Obtenez le trafic HTTP à partir de l'application focalisée tant qu'il ne s'agit pas d'une application système, en temps réel, si le moniteur HTTP a été activé depuis l'accueil du développeur.

Méthode      | URI de requête
:------     | :-----
Websocket | /ext/httpmonitor/sessions
<br />
**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**   
Un objet JSON avec les champs suivants:

* Sessions
    * RequestHeaders - (objet JSON) Les en-têtes de requête depuis la requête HTTP.
    * RequestContentHeaders - (objet JSON) Le contenu de requête depuis la requête HTTP.
    * RequestURL - (Chaîne) L'URL de demande.
    * RequestMethod - (Chaîne) La méthode de requête.
    * RequestMessage - (Chaîne) Le message de la requête, actuellement prenant en charge uniquement le contenu JSON et textuel.
    * ResponseHeaders - (objet JSON) Les en-têtes de réponse depuis la réponse HTTP.
    * ResponseContentHeaders - (objet JSON) Les en-têtes du contenu de réponse depuis la réponse HTTP.
    * StatusCode - (Numéro) Le code d’état de la réponse.
    * ReasponsePhrase - (Chaîne) La phrase de raison de la réponse.
    * ResponseMessage - (Chaîne) Le message de la réponse, actuellement prenant en charge uniquement le contenu JSON et textuel.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Demande réussie
4XX | Codes d’erreur
403 | Moniteur HTTP désactivé, doit être activé dans l'accueil du développeur
5XX | Codes d’erreur

<br />
**Familles d’appareils disponibles**

* Windows Xbox