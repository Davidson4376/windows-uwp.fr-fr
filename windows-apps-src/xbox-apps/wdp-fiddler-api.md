---
title: Informations de référence sur les API Fiddler Device Portal
description: Apprenez à activer/désactiver le suivi de Fiddler par programmation.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7d4225e-ac2c-41dc-aca7-9b1a95ec590b
ms.localizationpriority: medium
ms.openlocfilehash: 4cbdae1084f96901e90f8237d71bd59bf2d4c592
ms.sourcegitcommit: 681c1e3836d2a51cd3b31d824ece344281932bcd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59240017"
---
# <a name="fiddler-settings-api-reference"></a>Informations de référence sur les API des paramètres Fiddler   
Vous pouvez activer et désactiver le suivi réseau de Fiddler sur votre kit de développement à l’aide de cette API REST.

## <a name="determine-if-fiddler-tracing-is-enabled"></a>Déterminez si le suivi de Fiddler est activé

**Demande**

La requête suivante vous permet de vérifier si le suivi de Fiddler est activé sur l’appareil.

Méthode      | URI de requête
:------     | :-----
GET | /ext/fiddler


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**   

- Aucune

**Réponse**   

- La propriété booléenne JSON IsProxyEnabled spécifie si le proxy est activé ou non.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Opération réussie
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="enable-fiddler-tracing"></a>Activer le suivi de Fiddler

**Demande**

Vous pouvez activer le suivi de Fiddler pour le kit de développement à l’aide de la demande suivante.  Notez que l’appareil doit être redémarré avant que cela ne prenne effet.

Méthode      | URI de requête
:------     | :-----
PUBLIER | /ext/fiddler

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI      | Description     | 
| ------------------ |-----------------|
| proxyAddress       | L’adresse IP ou le nom d’hôte de l’appareil exécutant Fiddler |
| proxyPort          | Le port que Fiddler utilise pour la surveillance du trafic. Par défaut : 8888 |
| updateCert (facultatif)| Une valeur booléenne indiquant si le certificat Fiddler racine est fourni. Cette valeur doit être true si Fiddler n’a jamais été configuré sur ce kit de développement ou a été configuré pour un autre hôte.  |


**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucun si updateCert est false ou n’est pas fourni. Corps HTTP à parties multiples conforme contenant le fichier FiddlerRoot.cer dans le cas contraire.

**Réponse**   

- Aucune  

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
204 | La demande d’activation de Fiddler a été acceptée. Fiddler va être activé au prochain redémarrage de l’appareil.
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="disable-fiddler-tracing-on-the-devkit"></a>Désactiver le suivi de Fiddler sur le kit de développement

**Demande**

Vous pouvez désactiver le suivi de Fiddler sur l’appareil à l’aide de la demande suivante. Notez que l’appareil doit être redémarré avant que cela ne prenne effet.

Méthode      | URI de requête
:------     | :-----
Suppression | /ext/fiddler

**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**   

- Aucune

**Réponse**   

- Aucune 

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
204 | La demande de désactivation du suivi de Fiddler a réussi. Le suivi va être désactivé au prochain redémarrage de l’appareil.
4XX | Codes d’erreur
5XX | Codes d’erreur


**Familles d’appareils disponibles**

* Windows Xbox

## <a name="see-also"></a>Voir aussi
- [Configuration de Fiddler pour UWP sur Xbox](uwp-fiddler.md)

