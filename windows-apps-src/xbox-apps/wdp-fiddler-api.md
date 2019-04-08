---
title: Informations de référence sur les API Fiddler Device Portal
description: Apprenez à activer/désactiver le suivi de Fiddler par programmation.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7d4225e-ac2c-41dc-aca7-9b1a95ec590b
ms.localizationpriority: medium
ms.openlocfilehash: f60f3fc8678208f694a9ffabde06fa60de759a45
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603334"
---
# <a name="fiddler-settings-api-reference"></a>Informations de référence sur les API des paramètres Fiddler   
Vous pouvez activer et désactiver le suivi réseau de Fiddler sur votre kit de développement à l’aide de cette API REST.

## <a name="determine-if-fiddler-tracing-is-enabled"></a>Déterminez si le suivi de Fiddler est activé

**Demande**

La requête suivante vous permet de vérifier si le suivi de Fiddler est activé sur l’appareil.

Méthode      | URI de requête
:------     | :-----
GET | /ext/fiddler
<br />
**Paramètres d’URI**

- Aucune

**En-têtes de demande**

- Aucune

**Corps de la demande**   

- Aucune

**Réponse**   

- La propriété booléenne JSON IsProxyEnabled spécifie si le proxy est activé ou non.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Réussite
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="enable-fiddler-tracing"></a>Activer le suivi de Fiddler

**Demande**

Vous pouvez activer le suivi de Fiddler pour le kit de développement à l’aide de la demande suivante.  Notez que l’appareil doit être redémarré avant que cela ne prenne effet.

Méthode      | URI de requête
:------     | :-----
POST | /ext/fiddler
<br />
**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI      | Description     | 
| ------------------ |-----------------|
| proxyAddress       | L’adresse IP ou le nom d’hôte de l’appareil exécutant Fiddler |
| proxyPort          | Le port que Fiddler utilise pour la surveillance du trafic. Par défaut : 8888 |
| updateCert (facultatif)| Une valeur booléenne indiquant si le certificat Fiddler racine est fourni. Cette valeur doit être true si Fiddler n’a jamais été configuré sur ce kit de développement ou a été configuré pour un autre hôte.  |
<br>

**En-têtes de demande**

- Aucune

**Corps de la demande**

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
DELETE | /ext/fiddler
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
204 | La demande de désactivation du suivi de Fiddler a réussi. Le suivi va être désactivé au prochain redémarrage de l’appareil.
4XX | Codes d’erreur
5XX | Codes d’erreur

<br />
**Familles de périphériques disponibles**

* Windows Xbox

## <a name="see-also"></a>Voir également
- [Configuration de Fiddler pour UWP sur Xbox](uwp-fiddler.md)

