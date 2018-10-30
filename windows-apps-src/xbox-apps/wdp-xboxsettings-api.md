---
author: m-stahl
title: Référence des API des paramètres de développement Xbox Device Portal
description: Découvrez comment accéder aux paramètres de développement Xbox.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: 6ab12b99-2944-49c9-92d9-f995efc4f6ce
ms.localizationpriority: medium
ms.openlocfilehash: 8f3d0c09b242f8d60b06ee0dc510ad9a756466c5
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5753382"
---
# <a name="developer-settings-api-reference"></a>Informations de référence sur les API des paramètres de développement   
Cette API vous permet d’accéder aux paramètres Xbox One utiles pour le développement.

## <a name="get-all-developer-settings-at-once"></a>Obtenir tous les paramètres de développement à la fois

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

* Name: (chaîne) le nom du paramètre.
* Value: (chaîne) la valeur du paramètre.
* RequiresReboot: («Oui» | «Non») ce champ indique si un redémarrage est nécessaire pour que le paramètre prenne effet.
* Désactivé: («Oui» | «Non») ce champ indique si le paramètre est désactivé et ne peut pas être modifié.
* Category: (chaîne) catégorie du paramètre.
* Type: («Texte» | «Nombre» | «Booléen» | «Sélection») ce champ indique le type d’un paramètre: texte saisi, valeur booléenne («vrai» ou «faux»), un nombre avec valeurs max et min ou une sélection dans une liste de valeurs spécifiques.

Si le paramètre est un nombre:
* Min - (nombre) ce champ indique la valeur numérique minimale du paramètre.
* Max - (nombre) ce champ indique la valeur numérique maximale du paramètre.

Si le paramètre est sélection:
* OptionsVariable - («Oui» | «Non») ce champ indique si les définition des options sont variables, si les options valides peuvent changer sans redémarrage.
* Options - tableau JSON contenant les options de sélection valides sous forme de chaînes.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Demande réussie
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="get-settings-one-at-a-time"></a>Obtenir les paramètres un par un
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

* Name: (chaîne) le nom du paramètre.
* Value: (chaîne) la valeur du paramètre.
* RequiresReboot: («Oui» | «Non») ce champ indique si un redémarrage est nécessaire pour que le paramètre prenne effet.
* Désactivé: («Oui» | «Non») ce champ indique si le paramètre est désactivé et ne peut pas être modifié.
* Category: (chaîne) catégorie du paramètre.
* Type: («Texte» | «Nombre» | «Booléen» | «Sélection») ce champ indique le type d’un paramètre: texte saisi, valeur booléenne («vrai» ou «faux»), un nombre avec valeurs max et min ou une sélection dans une liste de valeurs spécifiques.

Si le paramètre est un nombre:
* Min - (nombre) ce champ indique la valeur numérique minimale du paramètre.
* Max - (nombre) ce champ indique la valeur numérique maximale du paramètre.

Si le paramètre est sélection:
* OptionsVariable - («Oui» | «Non») ce champ indique si les définition des options sont variables, si les options valides peuvent changer sans redémarrage.
* Options - tableau JSON contenant les options de sélection valides sous forme de chaînes.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Demande réussie
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="set-the-value-of-a-setting"></a>Définir la valeur d’un paramètre
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