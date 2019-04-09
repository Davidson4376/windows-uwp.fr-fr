---
title: Référence sur les API de sandbox Xbox Live Device Portal
description: Découvrez comment accéder par programme au sandbox Xbox Live.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 72c7459c-420a-4da9-8afa-191a846185a5
ms.localizationpriority: medium
ms.openlocfilehash: 8f04514962cf0684daa99ee75d4c4da73c785735
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244085"
---
# <a name="xbox-live-sandbox-api-reference"></a>Informations de référence sur les API de sandbox Xbox Live   
Vous pouvez obtenir et définir vos sandbox Xbox Live à l’aide de cette API REST.

## <a name="get-the-xbox-live-sandbox"></a>Obtenir le sandbox Xbox Live

**Demande**

Vous pouvez lire la valeur actuelle du sandbox Xbox Live de l’appareil en utilisant la requête suivante :

Méthode      | URI de requête
:------     | :-----
GET | /ext/xboxlive/sandbox

**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**

- Aucune

**Réponse**   
Sandbox : (chaîne) le sandbox actuel dans lequel se trouve l’appareil.   

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | La requête pour accéder aux informations d’identification pour le partage de fichiers a été accordée.
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="set-the-xbox-live-sandbox"></a>Définir le sandbox Xbox Live
Vous pouvez modifier le sandbox Xbox Live pour l’appareil en utilisant la requête suivante. Veuillez noter que sur Xbox One, vous devez redémarrer l’appareil pour que le paramètre prenne effet.

**Demande**

Vous pouvez définir la valeur actuelle du sandbox Xbox Live de l’appareil en utilisant la requête suivante :

Méthode      | URI de requête
:------     | :-----
PUT | /ext/xboxlive/sandbox

**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la requête**   
Le corps de la requête est un objet JSON contenant le champ suivant :   
Sandbox : (chaîne) la nouvelle valeur sur laquelle le sandbox de l’appareil doit être défini.

**Réponse**   
Sandbox : (chaîne) le sandbox actuel dans lequel se trouve l’appareil.   

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | La requête pour accéder aux informations d’identification pour le partage de fichiers a été accordée.
4XX | Codes d’erreur
5XX | Codes d’erreur

**Familles d’appareils disponibles**

* Windows Xbox

