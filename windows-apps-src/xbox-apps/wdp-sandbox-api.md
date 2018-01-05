---
author: payzer
title: "Référence sur les API de sandbox Xbox Live Device Portal"
description: "Découvrez comment accéder par programme au sandbox Xbox Live."
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 72c7459c-420a-4da9-8afa-191a846185a5
ms.openlocfilehash: d31c943336b36c325218b0b2a8830daf54ed25ca
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: HT
ms.contentlocale: fr-FR
---
# <a name="xbox-live-sandbox-api-reference"></a>Informations de référence sur les API de sandbox Xbox Live   
Vous pouvez obtenir et définir vos sandbox Xbox Live à l’aide de cette API REST.

## <a name="get-the-xbox-live-sandbox"></a>Obtenir le sandbox Xbox Live

**Requête**

Vous pouvez lire la valeur actuelle du sandbox Xbox Live de l’appareil en utilisant la requête suivante:

Méthode      | URI de la requête
:------     | :-----
GET | /ext/xboxlive/sandbox
<br />
**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**

- Aucun

**Réponse**   
Sandbox: (chaîne) le sandbox actuel dans lequel se trouve l’appareil.   

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | La requête pour accéder aux informations d’identification pour le partage de fichiers a été accordée.
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="set-the-xbox-live-sandbox"></a>Définir le sandbox Xbox Live
Vous pouvez modifier le sandbox Xbox Live pour l’appareil en utilisant la requête suivante. Veuillez noter que sur Xbox One, vous devez redémarrer l’appareil pour que le paramètre prenne effet.

**Requête**

Vous pouvez définir la valeur actuelle du sandbox Xbox Live de l’appareil en utilisant la requête suivante:

Méthode      | URI de la requête
:------     | :-----
PUT | /ext/xboxlive/sandbox
<br />
**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la requête**   
Le corps de la requête est un objet JSON contenant le champ suivant:   
Sandbox: (chaîne) la nouvelle valeur sur laquelle le sandbox de l’appareil doit être défini.

**Réponse**   
Sandbox: (chaîne) le sandbox actuel dans lequel se trouve l’appareil.   

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | La requête pour accéder aux informations d’identification pour le partage de fichiers a été accordée.
4XX | Codes d’erreur
5XX | Codes d’erreur

<br />
**Familles d’appareils disponibles**

* Windows Xbox

