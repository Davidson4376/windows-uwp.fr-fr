---
author: QuinnRadich
Description: Design an instructional user interface (UI) that teaches users how to work with your UWP app.
title: Recommandations en matière de conception d’une interface utilisateur d’instructions
label: Instructional UI
template: detail.hbs
ms.author: quradic
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: c87e2f06-339d-4413-b585-172752964f56
ms.localizationpriority: medium
ms.openlocfilehash: 9c97b6b5eca82d309a4b65a914041adeb1e782db
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5559103"
---
# <a name="instructional-ui-guidelines"></a>Recommandations en matière d’interface utilisateur d’instructions



Dans certains cas, il peut être utile de former l’utilisateur aux fonctions les plus subtiles de votre application, telles que des interactions tactiles spécifiques. Vous devez alors fournir des instructions par le biais de l’interface utilisateur pour signaler à l’utilisateur les fonctionnalités dont il risque de ne pas encore avoir eu connaissance.

## <a name="when-to-use-instructional-ui"></a>Quand utiliser l’interface utilisateur d’instructions

L’interface utilisateur d’instructions doit être utilisée avec parcimonie. Si vous en abusez, cette interface risque d’être rapidement ignorée ou d’incommoder l’utilisateur, et donc de se révéler inefficace.

L’interface utilisateur d’instructions doit permettre à l’utilisateur de découvrir les fonctionnalités importantes et subtiles de votre application, comme des mouvements tactiles ou des paramètres qui sont susceptibles de l’intéresser. Il est également possible de recourir à cette interface pour signaler aux utilisateurs les nouveautés ou modifications de votre application qui sont susceptibles de lui avoir échappé.

À moins que votre application ne dépende des mouvements tactiles, l’interface utilisateur d’instructions ne doit pas être utilisée pour enseigner aux utilisateurs les fonctionnalités fondamentales de votre application.

## <a name="principles-of-writing-instructional-ui"></a>Principes de création d’une interface utilisateur d’instructions

Une interface utilisateur d’instructions allie pertinence et pédagogie, et améliore l’expérience utilisateur. Elle doit présenter les caractéristiques suivantes:

-   **Simplicité:** les utilisateurs ne veulent pas que leur expérience soit interrompue par des informations complexes.
-   **Facilité de mémorisation:** les utilisateurs ne veulent pas voir s’afficher les mêmes instructions chaque fois qu’ils tentent d’exécuter une tâche spécifique. Les instructions doivent donc se révéler faciles à mémoriser.
-   **Pertinence immédiate:** si l’interface utilisateur d’instructions ne présente pas une procédure directement liée à la tâche que l’utilisateur souhaite exécuter, celui-ci n’aura pas de raison d’y prêter attention.

N’abusez pas de l’interface utilisateur d’instructions et prenez soin de choisir les rubriques adéquates. N’abordez pas les éléments suivants:

-   **Fonctionnalités fondamentales:** si les utilisateurs ont besoin de disposer d’instructions d’utilisation concernant votre application, envisagez de retravailler la conception de votre application pour la rendre plus intuitive.
-   **Fonctionnalités évidentes:** si un utilisateur peut comprendre le rôle d’une fonctionnalité sans aucune instruction, l’interface utilisateur d’instructions ne fera que le gêner.
-   **Fonctionnalités complexes:** l’interface utilisateur d’instructions doit rester concise, et les utilisateurs intéressés par des fonctionnalités complexes sont généralement disposés à rechercher les instructions correspondantes sans avoir besoin qu’on les leur présente spontanément.

Évitez de déranger l’utilisateur avec votre interface utilisateur d’instructions. Pratiques déconseillées:

-   **Masquer des informations importantes:** l’interface utilisateur d’instructions ne doit jamais occulter d’autres fonctionnalités de votre application.
-   **Exiger la participation des utilisateurs:** les utilisateurs doivent avoir la possibilité d’ignorer l’interface utilisateur d’instructions et de continuer à parcourir l’application.
-   **Afficher des informations répétées:** votre interface utilisateur d’instructions ne doit pas harceler l’utilisateur, même si ce dernier l’a ignorée la première fois. L’ajout d’un paramètre permettant de réafficher l’interface utilisateur d’instructions constitue une solution plus judicieuse.

## <a name="examples-of-instructional-ui"></a>Exemples d’interfaces utilisateur d’instructions

Voici quelques exemples dans lesquels une interface utilisateur d’instructions peut se révéler utile pour vos utilisateurs:

-   **Aidez les utilisateurs à découvrir les interactions tactiles.** L’écran suivant illustre une interface utilisateur d’instruction qui apprend à un joueur à utiliser les interactions tactiles dans le jeu, Cut the Rope.

    ![capture d’écran d’un jeu montrant un message d’interface utilisateur d’instruction, «Effectuer un balayage transversal pour couper la corde»](images/in-game-controls-3.png)

-   **Faites une première impression favorable.** Quand l’application Instants vidéos est lancée pour la première fois, l’interface utilisateur d’instructions invite l’utilisateur à créer des films sans gêner son utilisation.

    ![Écran de démarrage de l’application Instants vidéos](images/instructional-ui-movie.png)

-   **Guidez les utilisateurs pour effectuer une tâche compliquée.** Dans l’application Courrier Windows, un texte en bas de la Boîte de réception les invite à utiliser les **Paramètres** pour accéder à des messages plus anciens.

    ![partie d’une capture d’écran de l’application Courrier Windows qui montre un message d’interface utilisateur d’instruction](images/instructional-ui-mail-inbox.png)

    Quand l’utilisateur clique sur le message, le menu volant **Paramètres** de l’application s’affiche sur le côté droit de l’écran, ce qui permet à l’utilisateur d’effectuer la tâche. Les captures d’écran suivantes montrent l’application Courrier avant et après le clic de l’utilisateur sur le message d’interface utilisateur d’instruction.

    | Avant                                                               | Après                                                                                                        |
    |----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
    | ![Capture d’écran de l’application Courrier Windows](images/instructional-ui-mail.png) | ![capture d’écran de l’application courrier windows avec un menu volant paramètres étendu](images/instructional-ui-mail-flyout.png) |

## <a name="related-articles"></a>Articles connexes

* [Recommandations en matière d’aide de l’application](guidelines-for-app-help.md)
