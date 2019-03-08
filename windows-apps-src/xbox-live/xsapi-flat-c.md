---
title: Xbox Live C API
description: En savoir plus sur le modèle d’API C plat que vous pouvez utiliser pour interagir avec le service Xbox Live.
ms.date: 06/05/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox un, c, xsapi
ms.localizationpriority: medium
ms.openlocfilehash: a1c73661b561d586f9e28957c7caa6a1b1f9cb03
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597524"
---
# <a name="introduction-to-the-xbox-live-c-apis"></a>Introduction aux API Xbox Live C

En juin 2018, une nouvelle couche API C plate a été ajoutée à XSAPI. Cette nouvelle couche API résout certains problèmes qui se sont produits avec les couches de C++ et les API WinRT.

L’API C ne couvre pas encore toutes les fonctionnalités XSAPI, mais des fonctionnalités supplémentaires sont en cours de traitement. Toutes les 3 API couches, C, C++ et WinRT continueront à être pris en charge et ont des fonctionnalités supplémentaires sont ajoutées au fil du temps.

> [!NOTE]
> Les API C actuellement fonctionnent uniquement avec les titres qui utilisent le Kit de développement Xbox (XDK). Ils ne gèrent pas les jeux UWP pour l’instant.

## <a name="features-covered-by-the-c-apis"></a>Fonctionnalités couvertes par les API C

L’API C prend actuellement en charge les fonctionnalités et les services suivants :

- Succès
- Présence
- Profil
- Social
- Gestionnaire sociale

## <a name="benefits-of-the-c-api-for-xsapi"></a>Avantages de l’API C pour XSAPI

- Permet de titres contrôler les allocations de mémoire lors de l’appel XSAPI.
- Permet de titres d’assumer le contrôle complet de gestion lors de l’appel XSAPI de thread.
- Utilise une nouvelle bibliothèque HTTP, libHttpClient, conçue pour les développeurs de jeux.

Vous pouvez utiliser les API C en même temps que C++ XSAPI, mais vous ne bénéficierez pas les avantages mentionnés ci-dessus avec les APIs C++.

### <a name="managing-memory-allocations"></a>La gestion des allocations de mémoire

Avec la nouvelle API C, vous pouvez maintenant spécifier une fonction de rappel XSAPI appellera chaque fois qu’il tente d’allouer de la mémoire. Si vous ne spécifiez pas de rappels de fonction, XSAPI utilise les routines d’allocation de mémoire.

Pour spécifier manuellement vos routines de la mémoire, vous pouvez procédez comme suit :

- Au début du jeu :
  - Appeler `XblMemSetFunctions(memAllocFunc, memFreeFunc)` pour spécifier les rappels d’allocation de l’attribution et la libération de mémoire.
  - Appelez `XblInitialize()` pour initialiser l’instance de la bibliothèque.  
- Pendant l’exécution du jeu :
  - Appeler une des nouvelles API C de XSAPI qui allouer ou libérer de la mémoire entraîne XSAPI appeler des rappels de gestion de la mémoire spécifiée.  
- Lorsque le jeu se ferme :
  - Appelez `XblCleanup()` pour récupérer toutes les ressources associées à la bibliothèque XSAPI.
  - Nettoyer le Gestionnaire de mémoire personnalisé de votre jeu.

### <a name="managing-asynchronous-threads"></a>La gestion des threads asynchrones

L’API C introduit un nouveau thread asynchrone appelant le modèle qui permet aux développeurs un contrôle total sur le modèle de thread. Pour plus d’informations, consultez [modèle d’appel pour XSAPI plat C couche async appelle](flatc-async-patterns.md).

## <a name="migrating-code-to-use-c-xsapi"></a>Migration du code à utiliser C XSAPI

Les XSAPI C APIs peut servir à côté de l’API C++ XSAPI dans un projet, nous vous recommandons de migrer une fonctionnalité à la fois.

L’API C et C++ APIs sont des wrappers légers simplement autour un tronc commun, juste avec différents points d’entrée, la fonctionnalité devrait donc être inchangée. Toutefois, que les API C peuvent tirer parti de la mémoire personnalisé et d’un thread fonctionnalités de gestion.

> [!IMPORTANT]
> Vous ne pouvez pas mélanger XSAPI WinRT APIs avec les API C.

## <a name="where-to-view-the-c-apis"></a>Où consulter les API C

- [Fichiers d’en-tête API C](https://github.com/Microsoft/xbox-live-api/tree/master/Include/xsapi-c)
- [Exemple de code utilisant les nouvelles API C](https://github.com/Microsoft/xbox-live-api/tree/master/InProgressSamples/Social/Xbox/C)
- [libHttpClient](https://github.com/Microsoft/libHttpClient)
