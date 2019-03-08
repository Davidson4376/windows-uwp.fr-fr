---
Description: Concevez une aide efficace destinée à s’afficher de manière réactive dans votre application.
title: Recommandations en matière de conception d’aide dans l’application
label: In-app help
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6208b71b-37a7-40f5-91b0-19b665e7458a
ms.localizationpriority: medium
ms.openlocfilehash: 4783d28e4da6c06df0d0676f4a7d28ef3995481a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610054"
---
# <a name="in-app-help-pages"></a>Pages d’aide dans l’application

Dans la plupart des cas, il est généralement préférable d’afficher l’aide au sein de l’application et à la demande de l’utilisateur.

## <a name="when-to-use-in-app-help-pages"></a>Quand utiliser des pages d’aide dans l’application

L’aide dans l’application doit constituer la méthode d’affichage par défaut des informations d’aide destinées à l’utilisateur. Cette méthode est préconisée pour toute aide simple, directe et ne présentant aucun nouveau contenu à l’utilisateur. Les instructions, les conseils et les astuces peuvent tous être affichés sous la forme d’une aide dans l’application.

En revanche, les instructions complexes ou les didacticiels ne sont pas faciles à référencer rapidement et occupent un espace important. Il convient donc de les héberger en externe et de ne pas les intégrer à l’application proprement dite.

Les utilisateurs ne doivent pas avoir besoin de rechercher de l’aide concernant des instructions de base ou de nouvelles fonctionnalités. Si vous devez fournir une aide destinée à former les utilisateurs, utilisez une interface utilisateur d’instructions.

## <a name="types-of-in-app-help"></a>Types d’aides dans l’application

L’aide dans l’application peut prendre plusieurs formes, mais ces dernières sont toutes régies par les mêmes principes généraux de conception et de facilité d’utilisation.

#### <a name="help-pages"></a>Pages d’aide

La fourniture d’une ou de plusieurs pages d’aide distinctes au sein de votre application constitue un moyen rapide et simple d’afficher des instructions utiles.

-   **Être concis :** Une grande bibliothèque des rubriques d’aide est difficile à gérer et exploitable pour l’aide dans l’application.
-   **Soyez cohérent :** Assurez-vous que les utilisateurs peuvent accéder vos pages d’aide de la même façon à partir de n’importe quelle partie de votre application. Ils ne doivent jamais avoir besoin de chercher comment y accéder.
-   **Analyse des utilisateurs, ne pas lire :** Étant donné que l’aide que vous recherchez un utilisateur peut être sur la même page que les autres rubriques d’aide, veillez à ce qu’ils puissent voir facilement à laquelle un dont ils ont besoin pour vous concentrer sur.


#### <a name="popups"></a>Fenêtres contextuelles

Les fenêtres contextuelles permettent de fournir une aide très spécifique en affichant des instructions et des conseils précisément adaptés à la tâche que l’utilisateur tente d’exécuter.

-   **Concentrez-vous sur l’un des problèmes :** Espace est encore plus restreinte dans une fenêtre contextuelle à une page d’aide. Pour être efficaces, les fenêtres d’aide contextuelles ne doivent donc faire référence qu’à une seule tâche.
-   **Visibilité est importante :** Étant donné que les fenêtres contextuelles d’aide ne peuvent être affichés à partir d’un emplacement, assurez-vous qu’ils sont clairement visibles par l’utilisateur sans être obstructive. Si l’utilisateur ne distingue pas immédiatement une fenêtre contextuelle, il risque de la faire disparaître en recherchant une page d’aide.
-   **N’utilisez pas trop de ressources :** Aide ne doit pas de retard ou être lenteur du chargement. L’utilisation de vidéos, de fichiers audio ou d’images haute résolution dans des fenêtres contextuelles est plus susceptible de frustrer l’utilisateur que de l’aider véritablement.

#### <a name="descriptions"></a>Descriptions

Il peut parfois être utile de fournir un complément d’informations concernant une fonctionnalité au moment où l’utilisateur accède à cette dernière. Les descriptions sont comparables à une interface utilisateur d’instructions, mais il existe une différence essentielle entre ces deux types d’aides : l’interface utilisateur d’instructions tente d’enseigner et de faire découvrir à l’utilisateur des fonctionnalités qu’il ne connaît pas encore, alors que les descriptions détaillées enrichissent les connaissances d’un utilisateur concernant des fonctionnalités qui l’intéressent déjà.

-   **Ne pas apprendre les principes de base :** Supposons que l’utilisateur connaît déjà les principes fondamentaux de l’utilisation de l’élément en cours de description. L’utilisateur a besoin de clarifications ou de compléments d’informations. Il est inutile de lui fournir des informations qu’il connaît déjà.
-   **Décrire les interactions intéressantes :** Une des meilleures utilisations pour obtenir une description consiste à informer l’utilisateur sur la façon dont une fonctionnalités qu’ils connaissent déjà sur peuvent interagir. Cette approche complète les connaissances des utilisateurs sur les fonctions qu’ils ont pris l’habitude d’utiliser.
-   **Restez à l’écart :** Comme l’IU d’instruction, des descriptions nécessaire d’éviter toute interférence avec enthousiasme d’un utilisateur de l’application.

## <a name="related-articles"></a>Articles connexes

* [Instructions de l’aide de l’application](guidelines-for-app-help.md)
