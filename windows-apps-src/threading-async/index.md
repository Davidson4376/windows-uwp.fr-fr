---
ms.assetid: beac6333-655a-4bcf-9caf-bba15f715ea5
title: Threads et programmation asynchrone
description: Les threads et la programmation asynchrone permettent à votre application d’accomplir des tâches de manière asynchrone dans des threads parallèles.
ms.date: 05/14/2018
ms.topic: article
keywords: windows10, uwp, asynchrone, threads, de thread
ms.localizationpriority: medium
ms.openlocfilehash: 22c151b90be30b39da7decd9a0ce3109e29b7fb7
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8937378"
---
# <a name="threading-and-async-programming"></a>Threads et programmation asynchrone
Les threads et la programmation asynchrone permettent à votre application d’accomplir des tâches de manière asynchrone dans des threads parallèles.

Votre application peut utiliser le pool de threads pour accomplir des tâches de manière asynchrone dans des threads parallèles. Le pool de threads gère un ensemble de threads et utilise une file d’attente pour attribuer les éléments de travail aux threads lorsqu’ils deviennent disponibles. Le pool de threads est semblable aux modèles de programmation asynchrones disponibles dans Windows Runtime, car il peut être utilisé pour accomplir des tâches étendues sans bloquer l’interface utilisateur, mais il offre un plus grand contrôle que les modèles de programmation asynchrones, et vous pouvez l’utiliser pour effectuer plusieurs éléments de travail en parallèle. Vous pouvez utiliser le thread pour :

-   envoyer des éléments de travail, contrôler leur priorité et en annuler ;

-   planifier des éléments de travail à l’aide de minuteurs et de minuteurs périodiques ;

-   mettre de côté des ressources pour les éléments de travail critiques ;

-   exécuter des éléments de travail en réponse à des événements et sémaphores nommés.

Le pool de threads est plus efficace pour la gestion des threads car il réduit la surcharge liée à la création et à la destruction de threads. Cela signifie qu’il a la possibilité d’optimiser les threads sur plusieurs cœurs de processeurs, et qu’il peut équilibrer les ressources de threads entre les applications et lorsque des tâches en arrière-plan sont en cours d’exécution. L’utilisation du pool de threads intégré est pratique, car vous vous concentrez sur l’écriture du code qui accomplit une tâche plutôt que sur la mécanique de gestion des threads.

| Rubrique                                                                                                          | Description                         |
|----------------------------------------------------------------------------------------------------------------|-------------------------------------|
| [Programmation asynchrone (applications UWP)](asynchronous-programming-universal-windows-platform-apps.md)              | Cette rubrique décrit la programmation asynchrone dans la plateforme Windows universelle (UWP) ainsi que sa représentation dans les langages c#, Microsoft Visual Basic.NET, les extensions de composant Visual c++ (C++ / CX) et JavaScript. |
| [Programmation asynchrone en C++/CX (applications UWP)](asynchronous-programming-in-cpp-universal-windows-platform-apps.md)| Cet article décrit la meilleure façon d’utiliser des méthodes asynchrones en C++/CX à l’aide de la classe <code>task</code> qui est définie dans l’espace de noms <code>concurrency</code> dans ppltasks.h. |
| [Meilleures pratiques pour l’utilisation du pool de threads](best-practices-for-using-the-thread-pool.md)                         | Cette rubrique décrit les meilleures pratiques relatives à l’utilisation du pool de threads. |
| [Appeler des API asynchrones en C# ou Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md)             | La plateforme Windows universelle (UWP) comporte de nombreuses API asynchrones qui permettent à votre application de rester réactive lorsqu’elle exécute des opérations potentiellement longues. Cette rubrique décrit comment utiliser les méthodes asynchrones de l’UWP en C# ou Microsoft Visual Basic. |
| [Créer un élément de travail périodique](create-a-periodic-work-item.md)                                                   | Découvrez comment créer un élément de travail qui se reproduit régulièrement. |
| [Envoyer un élément de travail au pool de threads](submit-a-work-item-to-the-thread-pool.md)                               | Découvrez comment effectuer des tâches dans un thread distinct en envoyant un élément de travail au pool de threads. |
| [Utiliser un minuteur pour envoyer un élément de travail](use-a-timer-to-submit-a-work-item.md)                                       | Découvrez comment créer un élément de travail qui s’exécute une fois le délai du minuteur écoulé. |
