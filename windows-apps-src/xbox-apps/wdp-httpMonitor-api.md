---
title: Référence API du moniteur HTTP du portail d'appareil
description: Découvrez comment accéder au trafic HTTP depuis l'application focalisée sur une Xbox.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 1e7c07c92c1671cd9051393586e1e8562fa756d0
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244095"
---
# <a name="http-monitor-api-reference"></a>Référence API du moniteur HTTP   
Vous pouvez accéder au trafic HTTP en temps réel pour les applications focalisées à l'aide de cet API si le moniteur HTTP a été activé sur la console Xbox en cochant la case dans l'accueil du développeur.

## <a name="get-if-the-http-monitor-is-enabled"></a>Déterminer si le moniteur HTTP est activé

**Demande**

Vous pouvez découvrir si le moniteur HTTP est activé dans l'accueil du développeur.

Méthode      | URI de requête
:------     | :-----
GET | /ext/httpmonitor/sessions

**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**   
Un objet JSON avec les champs suivants :

* Activé - (Bool) Si le moniteur HTTP est activé sur la console Xbox en cochant la case dans l'accueil du développeur.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Demande réussie
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="get-http-traffic-from-the-focused-app"></a>Obtenir le trafic HTTP à partir de l'application focalisée

**Demande**

Obtenez le trafic HTTP à partir de l'application focalisée tant qu'il ne s'agit pas d'une application système, en temps réel, si le moniteur HTTP a été activé depuis l'accueil du développeur.

Méthode      | URI de requête
:------     | :-----
Websocket | /ext/httpmonitor/sessions

**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**   
Un objet JSON avec les champs suivants :

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


**Familles d’appareils disponibles**

* Windows Xbox