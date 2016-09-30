---
author: WilliamsJason
title: "Informations de référence sur les API Fiddler Device Portal"
description: "Apprenez à activer/désactiver le suivi de Fiddler par programmation."
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: eeb3bc5c4843fe86c54930315d4e112166664e45
ms.openlocfilehash: 435a00eaf9c1f0d8e0c0043229c2adc80638ace3

---

# Informations de référence sur les API des paramètres Fiddler   
Vous pouvez activer et désactiver le suivi réseau de Fiddler sur votre kit de développement à l’aide de cette API REST.

## Activer le suivi de Fiddler

**Requête**

Vous pouvez activer le suivi de Fiddler pour le kit de développement à l’aide de la demande suivante.  Notez que l’appareil doit être redémarré avant que cela ne prenne effet.

Méthode      | URI de la requête
:------     | :-----
POST | /ext/fiddler
<br />
**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête:

| Paramètre d’URI      | Description     | 
| ------------------ |-----------------|
| proxyAddress       | L’adresse IP ou le nom d’hôte de l’appareil exécutant Fiddler |
| proxyPort          | Le port que Fiddler utilise pour la surveillance du trafic. Par défaut: 8888 |
| updateCert (facultatif)| Une valeur booléenne indiquant si le certificat Fiddler racine est fourni. Cette valeur doit être true si Fiddler n’a jamais été configuré sur ce kit de développement ou a été configuré pour un autre hôte.  |
<br>

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun si updateCert est false ou n’est pas fourni. Corps HTTP à parties multiples conforme contenant le fichier FiddlerRoot.cer dans le cas contraire.

**Réponse**   

- Aucun  

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
204 | La demande d’activation de Fiddler a été acceptée. Fiddler va être activé au prochain redémarrage de l’appareil.
4XX | Codes d’erreur
5XX | Codes d’erreur

## Désactiver le suivi de Fiddler sur le kit de développement

**Requête**

Vous pouvez désactiver le suivi de Fiddler sur l’appareil à l’aide de la demande suivante. Notez que l’appareil doit être redémarré avant que cela ne prenne effet.

Méthode      | URI de requête
:------     | :-----
DELETE | /ext/fiddler
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
204 | La demande de désactivation du suivi de Fiddler a réussi. Le suivi va être désactivé au prochain redémarrage de l’appareil.
4XX | Codes d’erreur
5XX | Codes d’erreur

<br />
**Familles d’appareils disponibles**

* Windows Xbox

## Voir également
- [Configuration de Fiddler pour UWP sur Xbox](uwp-fiddler.md)




<!--HONumber=Jul16_HO1-->


