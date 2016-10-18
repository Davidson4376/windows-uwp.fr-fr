---
author: payzer
title: "Référence sur les API de sandbox Xbox Live Device Portal"
description: "Découvrez comment accéder par programme au sandbox Xbox Live."
translationtype: Human Translation
ms.sourcegitcommit: a857ba338a971e651653193ff2149f08b1665a36
ms.openlocfilehash: 2a0bfa2eecffb2b0f5ed0bc691cb90bcd7191321

---

# Informations de référence sur les API de sandbox Xbox Live   
Vous pouvez obtenir et définir vos sandbox Xbox Live à l’aide de cette API REST.

## Obtenir le sandbox Xbox Live

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

## Définir le sandbox Xbox Live
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




<!--HONumber=Aug16_HO3-->


