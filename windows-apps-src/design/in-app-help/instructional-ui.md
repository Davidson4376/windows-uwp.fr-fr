---
Description: Concevoir une interface de démonstration utilisateur (IU) qui explique aux utilisateurs comment travailler avec votre application UWP.
title: Recommandations en matière de conception d’une interface utilisateur d’instructions
label: Instructional UI
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: c87e2f06-339d-4413-b585-172752964f56
ms.localizationpriority: medium
ms.openlocfilehash: b39507fb1333fdb642601c6b4828c3d160c6ceb5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656434"
---
# <a name="instructional-ui-guidelines"></a>Recommandations en matière d’interface utilisateur d’instructions



Dans certains cas, il peut être utile de former l’utilisateur aux fonctions les plus subtiles de votre application, telles que des interactions tactiles spécifiques. Vous devez alors fournir des instructions par le biais de l’interface utilisateur pour signaler à l’utilisateur les fonctionnalités dont il risque de ne pas encore avoir eu connaissance.

## <a name="when-to-use-instructional-ui"></a>Quand utiliser l’interface utilisateur d’instructions

L’interface utilisateur d’instructions doit être utilisée avec parcimonie. Si vous en abusez, cette interface risque d’être rapidement ignorée ou d’incommoder l’utilisateur, et donc de se révéler inefficace.

L’interface utilisateur d’instructions doit permettre à l’utilisateur de découvrir les fonctionnalités importantes et subtiles de votre application, comme des mouvements tactiles ou des paramètres qui sont susceptibles de l’intéresser. Il est également possible de recourir à cette interface pour signaler aux utilisateurs les nouveautés ou modifications de votre application qui sont susceptibles de lui avoir échappé.

À moins que votre application ne dépende des mouvements tactiles, l’interface utilisateur d’instructions ne doit pas être utilisée pour enseigner aux utilisateurs les fonctionnalités fondamentales de votre application.

## <a name="principles-of-writing-instructional-ui"></a>Principes de création d’une interface utilisateur d’instructions

Une interface utilisateur d’instructions allie pertinence et pédagogie, et améliore l’expérience utilisateur. Elle doit présenter les caractéristiques suivantes :

-   **Simple :** Les utilisateurs ne veulent leur expérience d’être interrompue avec des informations complexes
-   **Facile à retenir :** Les utilisateurs ne veulent pas voir les mêmes instructions que chaque fois qu’ils tentent une tâche, des instructions doivent donc être quelque chose qu’ils en souvenir.
-   **Immédiatement pertinents :** Si l’interface utilisateur pédagogique n’animer un utilisateur sur quelque chose qu’ils souhaitent immédiatement effectuer, qu’elles n’auront une raison prêter attention à ce dernier.

N’abusez pas de l’interface utilisateur d’instructions et prenez soin de choisir les rubriques adéquates. N’abordez pas les éléments suivants :

-   **Fonctionnalités fondamentales :** Si un utilisateur a besoin d’instructions pour utiliser votre application, envisagez la conception d’application plus intuitive.
-   **Fonctionnalités évident :** Si un utilisateur peut déterminer une fonctionnalité sur leurs propres sans l’instruction, l’interface utilisateur pédagogique obtiendra simplement de la façon.
-   **Fonctionnalités complexes :** Instruction interface utilisateur doit être concis, et les utilisateurs intéressés par des fonctionnalités complexes sont généralement disposés à rechercher des instructions et n’avez pas besoin de les recevoir.

Évitez de déranger l’utilisateur avec votre interface utilisateur d’instructions. Pratiques déconseillées :

-   **Informations importantes obscures :** Autres fonctionnalités de votre application doit jamais interférer avec l’interface utilisateur pédagogique.
-   **Forcer les utilisateurs de participer :** Les utilisateurs doivent pouvoir ignorer l’interface utilisateur pédagogique et toujours la progression via l’application.
-   **Affichage des informations sur la fréquence :** Ne pas de harceler l’utilisateur avec l’interface utilisateur pédagogique, même si elles l’ignorer la première fois. L’ajout d’un paramètre permettant de réafficher l’interface utilisateur d’instructions constitue une solution plus judicieuse.

## <a name="examples-of-instructional-ui"></a>Exemples d’interfaces utilisateur d’instructions

Voici quelques exemples dans lesquels une interface utilisateur d’instructions peut se révéler utile pour vos utilisateurs :

-   **Aider les utilisateurs à découvrir les interactions tactiles.** L’écran suivant illustre une interface utilisateur d’instruction qui apprend à un joueur à utiliser les interactions tactiles dans le jeu, Cut the Rope.

    ![capture d’écran d’un jeu montrant un message d’interface utilisateur d’instruction, « Effectuer un balayage transversal pour couper la corde »](images/in-game-controls-3.png)

-   **Effectue une bonne première impression.** Quand l’application Instants vidéos est lancée pour la première fois, l’interface utilisateur d’instructions invite l’utilisateur à créer des films sans gêner son utilisation.

    ![Écran de démarrage de l’application Instants vidéos](images/instructional-ui-movie.png)

-   **Guider les utilisateurs à effectuer l’étape suivante dans une tâche compliquée.** Dans l’application Courrier Windows, un texte en bas de la Boîte de réception les invite à utiliser les **Paramètres** pour accéder à des messages plus anciens.

    ![partie d’une capture d’écran de l’application Courrier Windows qui montre un message d’interface utilisateur d’instruction](images/instructional-ui-mail-inbox.png)

    Quand l’utilisateur clique sur le message, le menu volant **Paramètres** de l’application s’affiche sur le côté droit de l’écran, ce qui permet à l’utilisateur d’effectuer la tâche. Les captures d’écran suivantes montrent l’application Courrier avant et après le clic de l’utilisateur sur le message d’interface utilisateur d’instruction.

    | Avant                                                               | Après                                                                                                        |
    |----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
    | ![Capture d’écran de l’application Courrier Windows](images/instructional-ui-mail.png) | ![capture d’écran de l’application courrier windows avec un menu volant paramètres étendu](images/instructional-ui-mail-flyout.png) |

## <a name="related-articles"></a>Articles connexes

* [Instructions de l’aide de l’application](guidelines-for-app-help.md)
