---
ms.assetid: 41ac0142-4d86-4bb3-b580-36d0d6956091
title: Informations de référence sur les API Device Portal pour HoloLens
description: Découvrez les API REST Windows Device Portal pour HoloLens que vous pouvez utiliser pour accéder aux données et contrôler votre appareil par programme.
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, uwp, le portail de l’appareil
ms.localizationpriority: medium
ms.openlocfilehash: 3aeb068908adf6d6c40a50cee3aececba1861ee8
ms.sourcegitcommit: 81511fddf1393dffcfc069c769bb149da99529b1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59013336"
---
# <a name="device-portal-api-reference-for-hololens"></a>Informations de référence sur les API Device Portal pour HoloLens

Dans Windows Device Portal, tout repose sur les API REST que vous pouvez utiliser pour accéder aux données et contrôler votre appareil par programme.

## <a name="holographic-os"></a>Système d’exploitation holographique

### <a name="get-https-requirements-for-the-device-portal"></a>Obtenir la spécification HTTPS pour Device Portal

**Demande**

Vous pouvez obtenir la spécification HTTPS pour Device Portal en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/os/webmanagement/settings/https |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="get-the-stored-interpupillary-distance-ipd"></a>Obtenir l’écart pupillaire stocké (IPD)

**Demande**

Vous pouvez obtenir la valeur de l’écart pupillaire stocké en utilisant le format de requête suivant. La valeur renvoyée est exprimée en millimètres.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/os/settings/ipd |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="get-a-list-of-hololens-specific-etw-providers"></a>Obtenir une liste des fournisseurs ETW spécifiques HoloLens

**Demande**

Vous pouvez obtenir une liste des fournisseurs ETW spécifiques HoloLens qui ne sont pas enregistrés avec le système en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/os/etw/customproviders |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.


### <a name="return-the-state-for-all-active-services"></a>Renvoie l’état de tous les services actifs

**Demande**

Vous pouvez obtenir l’état de tous les services en cours d’exécution en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/os/services |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.


### <a name="set-the-https-requirement-for-the-device-portal"></a>Obtenir la spécification HTTPS pour Device Portal

**Demande**

Vous pouvez obtenir la spécification HTTPS pour Device Portal en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/holographic/management/settings/https |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| obligatoire   | (**requis**) Détermine si HTTPS est requis pour Device Portal. Les valeurs possibles sont **yes**, **no**, et **default**. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.


### <a name="set-the-interpupillary-distance-ipd"></a>Définir l’écart pupillaire (IPD)

**Demande**

Vous pouvez définir l’écart pupillaire stocké en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/holographic/os/settings/ipd |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| ipd   | (**requis**) Nouvelle valeur IPD à stocker. Cette valeur doit être exprimée en millimètres. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.


## <a name="holographic-perception"></a>Perception holographique

### <a name="accept-websocket-upgrades-and-run-a-mirage-client-that-sends-updates"></a>Accepter les mises à niveau websocket et exécuter un client mirage qui envoie des mises à jour

**Demande**

Vous pouvez accepter les mises à niveau websocket et exécuter un client mirage qui envoie des mises à jour à 30 fps en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET/WebSocket | /api/holographic/perception/client |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| clientmode   | (**requis**) Détermine le mode de suivi. La valeur **active** force le passage en mode de suivi visuel lorsqu’il est impossible de l’établir de manière passive. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.


## <a name="holographic-thermal"></a>Thermique holographique

### <a name="get-the-thermal-stage-of-the-device"></a>Obtenir la phase thermique de l’appareil

**Demande**

Vous pouvez obtenir la phase thermique de l’appareil en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/ |

**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

Les valeurs possibles sont indiquées par le tableau suivant.

| Value | Description |
| --- | --- |
| 1 | Normal |
| 2 | Chargé |
| 3 | Critique |

**Code d’état**

- Codes d’état standard.

## <a name="hsimulation-control"></a>Contrôle HSimulation
### <a name="create-a-control-stream-or-post-data-to-a-created-stream"></a>Créer un flux de contrôle ou publier des données dans un flux créé

**Demande**

Vous pouvez créer un flux de contrôle ou publier des données dans un flux créé en utilisant le format de requête suivant. Les données publiées doivent être de type **application/octet-stream**.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/holographic/simulation/control/stream |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| priority   | (**requis en cas de création d’un flux de contrôle**) Indique la priorité du flux. |
| streamid   | (**requis en cas de publication dans un flux créé**) Identifiant du flux dans lequel publier. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="delete-a-control-stream"></a>Supprimer un flux de contrôle

**Demande**

Vous pouvez supprimer un flux de contrôle en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| Suppression | /api/holographic/simulation/control/stream |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="get-a-control-stream"></a>Obtenir un flux de contrôle

**Demande**

Vous pouvez ouvrir une connexion web socket pour un flux de contrôle en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET/WebSocket | /api/holographic/simulation/control/stream |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="get-the-simulation-mode"></a>Obtenir le mode de simulation

**Demande**

Vous pouvez obtenir le mode de simulation en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/simulation/control/mode |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="set-the-simulation-mode"></a>Définir le mode de simulation

**Demande**

Vous pouvez définir le mode de simulation en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/holographic/simluation/control/mode |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| mode   | (**requis**) Indique le mode de simulation. Les valeurs possibles sont **default**, **simulation**, **remote**, et **legacy**. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

## <a name="hsimulation-playback"></a>Lecture HSimulation

### <a name="delete-a-recording"></a>Supprimer un enregistrement

**Demande**

Vous pouvez supprimer un enregistrement en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| Suppression | /api/holographic/simulation/playback/file |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| recording   | (**requis**) Nom de l’enregistrement à supprimer. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="get-all-recordings"></a>Obtenir tous les enregistrements

**Demande**

Vous pouvez obtenir tous les enregistrements disponibles en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/simulation/playback/files |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="get-the-types-of-data-in-a-loaded-recording"></a>Obtenir les types de données dans un enregistrement chargé

**Demande**

Vous pouvez obtenir les types de données dans un enregistrement chargé en utilisant le format suivant de la demande.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/simulation/playback/session/types |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| recording   | (**requis**) Nom de l’enregistrement qui vous intéresse. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="get-all-the-loaded-recordings"></a>Obtenir tous les enregistrements chargés

**Demande**

Vous pouvez obtenir tous les enregistrements chargés en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/simulation/playback/session/files |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="get-the-current-playback-state-of-a-recording"></a>Obtenir l’état actuel de lecture d’un enregistrement 

**Demande**

Vous pouvez obtenir l’état actuel de lecture d’un enregistrement en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/simulation/playback/session |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| recording   | (**requis**) Nom de l’enregistrement qui vous intéresse. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="load-a-recording"></a>Charger un enregistrement

**Demande**

Vous pouvez charger un enregistrement en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/holographic/simulation/playback/session/file |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| recording   | (**requis**) Nom de l’enregistrement à charger. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="pause-a-recording"></a>Suspendre un enregistrement

**Demande**

Vous pouvez suspendre un enregistrement en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/holographic/simulation/playback/session/pause |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| recording   | (**requis**) Nom de l’enregistrement à suspendre. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="play-a-recording"></a>Lire un enregistrement

**Demande**

Vous pouvez lire un enregistrement en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/holographic/simulation/playback/session/play |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| recording   | (**requis**) Nom de l’enregistrement à lire. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="stop-a-recording"></a>Arrêter un enregistrement

**Demande**

Vous pouvez arrêter un enregistrement en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/holographic/simulation/playback/session/stop |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| recording   | (**requis**) Nom de l’enregistrement à arrêter. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="unload-a-recording"></a>Décharger un enregistrement

**Demande**

Vous pouvez décharger un enregistrement en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| Suppression | /api/holographic/simulation/playback/session/file |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| recording   | (**requis**) Nom de l’enregistrement à décharger. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="upload-a-recording"></a>Charger un enregistrement

**Demande**

Vous pouvez décharger un enregistrement en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/holographic/simulation/playback/file |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

## <a name="hsimulation-recording"></a>Enregistrement HSimulation

### <a name="get-the-recording-state"></a>Obtenir l’état de l’enregistrement

**Demande**

Vous pouvez obtenir l’état d’enregistrement actuel en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/simulation/recording/status |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="start-a-recording"></a>Démarrer un enregistrement

**Demande**

Vous pouvez démarrer un enregistrement en utilisant le format de requête suivant. Vous ne pouvez effectuer qu’un seul enregistrement actif à la fois. 
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/holographic/simulation/recording/start |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| head   | (**voir ci-dessous**) Définissez cette valeur sur 1 pour indiquer que le système doit enregistrer des données de tête. |
| hands   | (**voir ci-dessous**) Définissez cette valeur sur 1 pour indiquer que le système doit enregistrer des données hands. |
| spatialMapping   | (**voir ci-dessous**) Définissez cette valeur sur 1 pour indiquer que le système doit enregistrer des données de mappage spatial. |
| environment   | (**voir ci-dessous**) Définissez cette valeur sur 1 pour indiquer que le système doit enregistrer des données d’environnement. |
| name   | (**requis**) Nom de l’enregistrement. |
| singleSpatialMappingFrame   | (**facultatif**) Définissez cette valeur à 1 pour indiquer qu’une seule trame de mappage spatial doit être enregistrée. |

Pour ces paramètres, un des paramètres suivants doit être défini sur 1 : *head*, *hands*, *spatialMapping*, ou *environment*.

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="stop-the-current-recording"></a>Arrêter l’enregistrement en cours

**Demande**

Vous pouvez arrêter l’enregistrement en cours en utilisant le format de requête suivant. L’enregistrement est renvoyé sous forme de fichier.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/holographic/simulation/recording/stop |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

## <a name="mixed-reality-capture"></a>MRC (Mixed Reality Capture)

### <a name="delete-a-mixed-reality-capture-mrc-recording-from-the-device"></a>Supprimer un enregistrement MRC à partir de l’appareil

**Demande**

Vous pouvez supprimer un enregistrement MRC en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
Suppression | /api/holographic/mrc/file |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| filename   | (**requis**) Nom du fichier vidéo à supprimer. Ce nom doit être codé en hex64. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="download-a-mixed-reality-capture-mrc-file"></a>Télécharger un fichier MRC

**Demande**

Vous pouvez télécharger un fichier MRC à partir de l’appareil en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/mrc/file |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| filename   | (**requis**) Nom du fichier vidéo que vous souhaitez obtenir. Ce nom doit être codé en hex64. |
| op   | (**facultatif**) Définissez cette valeur sur **stream** si vous voulez télécharger un flux. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="get-the-mixed-reality-capture-mrc-settings"></a>Obtenir les paramètres MRC

**Demande**

Vous pouvez obtenir les paramètres MRC en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/mrc/settings |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="get-the-status-of-the-mixed-reality-capture-mrc-recording"></a>Obtenir l’état de l’enregistrement MRC

**Demande**

Vous pouvez obtenir l’état de l’enregistrement MRC en utilisant le format de requête suivant. Les valeurs possibles sont **running** et **stopped**.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/mrc/status |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="get-the-list-of-mixed-reality-capture-mrc-files"></a>Obtenir la liste des fichiers MRC

**Demande**

Vous pouvez obtenir les fichiers MRC stockés sur l’appareil en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/mrc/files |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="set-the-mixed-reality-capture-mrc-settings"></a>Définir les paramètres MRC

**Demande**

Vous pouvez définir les paramètres MRC en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/holographic/mrc/settings |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="starts-a-mixed-reality-capture-mrc-recording"></a>Démarre un enregistrement MRC

**Demande**

Vous pouvez démarrer un enregistrement MRC en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/holographic/mrc/video/control/start |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="stop-the-current-mixed-reality-capture-mrc-recording"></a>Arrêter l’enregistrement MRC actuel

**Demande**

Vous pouvez arrêter l’enregistrement MRC actuel en utilisant le format de requête suivant.
 
| Méthode      | URI de requête |
| :------     | :----- |
| PUBLIER | /api/holographic/mrc/video/control/stop |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="take-a-mixed-reality-capture-mrc-photo"></a>Prendre une photo MRC

**Demande**

Vous pouvez prendre une photo MRC en utilisant le format de requête suivant. La photo est renvoyée au format JPEG.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/mrc/photo |


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

## <a name="mixed-reality-streaming"></a>Diffusion en continu de la réalité mixée

### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>Initie un téléchargement mémorisé en bloc d’un mp4 fragmenté

**Demande**

Vous pouvez lancer un téléchargement mémorisé en bloc d’un mp4 fragmenté en utilisant le format de requête suivant. Cette API utilise la qualité par défaut.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/stream/live.mp4 |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| pv   | (**facultatif**) Indique si vous souhaitez capturer la caméra PV. La valeur doit être **true** ou **false**. |
| holo   | (**facultatif**) Indique si vous souhaitez capturer des hologrammes. La valeur doit être **true** ou **false**. |
| mic   | (**facultatif**) Indique si vous souhaitez capturer le microphone. La valeur doit être **true** ou **false**. |
| loopback   | (**facultatif**) Indique si vous souhaitez capturer le son de l’application. La valeur doit être **true** ou **false**. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>Initie un téléchargement mémorisé en bloc d’un mp4 fragmenté

**Demande**

Vous pouvez lancer un téléchargement mémorisé en bloc d’un mp4 fragmenté en utilisant le format de requête suivant. Cette API utilise une qualité de niveau élevé.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/stream/live_high.mp4 |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| pv   | (**facultatif**) Indique si vous souhaitez capturer la caméra PV. La valeur doit être **true** ou **false**. |
| holo   | (**facultatif**) Indique si vous souhaitez capturer des hologrammes. La valeur doit être **true** ou **false**. |
| mic   | (**facultatif**) Indique si vous souhaitez capturer le microphone. La valeur doit être **true** ou **false**. |
| loopback   | (**facultatif**) Indique si vous souhaitez capturer le son de l’application. La valeur doit être **true** ou **false**. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>Initie un téléchargement mémorisé en bloc d’un mp4 fragmenté

**Demande**

Vous pouvez lancer un téléchargement mémorisé en bloc d’un mp4 fragmenté en utilisant le format de requête suivant. Cette API utilise une qualité de niveau faible.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/stream/live_low.mp4 |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| pv   | (**facultatif**) Indique si vous souhaitez capturer la caméra PV. La valeur doit être **true** ou **false**. |
| holo   | (**facultatif**) Indique si vous souhaitez capturer des hologrammes. La valeur doit être **true** ou **false**. |
| mic   | (**facultatif**) Indique si vous souhaitez capturer le microphone. La valeur doit être **true** ou **false**. |
| loopback   | (**facultatif**) Indique si vous souhaitez capturer le son de l’application. La valeur doit être **true** ou **false**. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.

### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>Initie un téléchargement mémorisé en bloc d’un mp4 fragmenté

**Demande**

Vous pouvez lancer un téléchargement mémorisé en bloc d’un mp4 fragmenté en utilisant le format de requête suivant. Cette API utilise une qualité de niveau moyenne.
 
| Méthode      | URI de requête |
| :------     | :----- |
| GET | /api/holographic/stream/live_med.mp4 |


**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI | Description |
| :---          | :--- |
| pv   | (**facultatif**) Indique si vous souhaitez capturer la caméra PV. La valeur doit être **true** ou **false**. |
| holo   | (**facultatif**) Indique si vous souhaitez capturer des hologrammes. La valeur doit être **true** ou **false**. |
| mic   | (**facultatif**) Indique si vous souhaitez capturer le microphone. La valeur doit être **true** ou **false**. |
| loopback   | (**facultatif**) Indique si vous souhaitez capturer le son de l’application. La valeur doit être **true** ou **false**. |

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**

- Aucune

**Code d’état**

- Codes d’état standard.
