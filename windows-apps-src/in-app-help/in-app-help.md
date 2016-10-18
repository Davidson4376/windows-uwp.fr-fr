---
author: QuinnRadich
Description: "Concevez une aide efficace destinée à s’afficher de manière réactive dans votre application."
title: "Recommandations en matière de conception d’aide dans l’application"
label: In-app help
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 0d13ca6850df1b575eddebdb36b345423392664f
ms.openlocfilehash: 0c606e3b5be2889e83efce77d4c98ca646fef725

---

# Pages d’aide dans l’application

Dans la plupart des cas, il est préférable d’afficher l’aide au sein de l’application et à la demande de l’utilisateur. Tenez compte des recommandations suivantes lors de la création d’une aide dans l’application.

## <span id="when_to_use_in_app_help"></span><span id="WHEN_TO_USE_IN_APP_HELP"></span>Quand utiliser des pages d’aide dans l’application

L’aide dans l’application doit constituer la méthode d’affichage par défaut des informations d’aide destinées à l’utilisateur. Cette méthode est préconisée pour toute aide simple, directe et ne présentant aucun nouveau contenu à l’utilisateur. Les instructions, les conseils et les astuces peuvent tous être affichés sous la forme d’une aide dans l’application.

Les instructions complexes ou didacticiels ne sont pas faciles à référencer rapidement, et ils occupent un espace important. Ces données doivent donc être hébergées en externe et ne pas être incorporées à l’application elle-même.

Les utilisateurs ne doivent pas avoir besoin de rechercher de l’aide concernant des instructions de base ou de nouvelles fonctionnalités. Si vous devez fournir une aide destinée à former les utilisateurs, utilisez une interface utilisateur d’instructions.

## <span id="types_of_in_app_help"></span><span id="TYPES_OF_IN_APP_HELP"></span>Types d’aides dans l’application

L’aide dans l’application peut prendre plusieurs formes, mais ces dernières sont toutes régies par les mêmes principes généraux de conception et de facilité d’utilisation.

### <span id="help_pages"></span><span id="HELP_PAGES"></span>Pages d’aide

La fourniture d’une ou de plusieurs pages d’aide distinctes au sein de votre application constitue un moyen rapide et simple d’afficher des instructions utiles:

-   **Soyez concis:** une bibliothèque de rubriques d’aide volumineuse se révèle peu maniable et inadaptée pour une aide dans l’application.
-   **Restez cohérent:** assurez-vous que les utilisateurs pourront accéder à vos pages d’aide de la même manière qu’aux autres parties de votre application. Ils ne doivent jamais avoir besoin de chercher comment y accéder.
-   **Pensez à attribuer des titres significatifs à vos pages d’aide:** Étant donné que le contenu d’aide recherché par un utilisateur peut figurer sur la même page que d’autres rubriques d’aide, veillez à ce que l’utilisateur puisse facilement déterminer la rubrique qui l’intéresse spécifiquement.


### <span id="popups"></span><span id="POPUPS"></span>Fenêtres contextuelles

Les fenêtres contextuelles permettent de fournir une aide très spécifique en affichant des instructions et des conseils précisément adaptés à la tâche que l’utilisateur tente d’exécuter:

-   **Concentrez-vous sur un seul problème:** L’espace disponible dans une fenêtre contextuelle est encore plus restreint que celui d’une page d’aide. Pour être efficaces, les fenêtres d’aide contextuelles ne doivent donc faire référence qu’à une seule tâche.
-   **Souciez-vous de la visibilité:** les fenêtres d’aide contextuelles n’étant visualisables qu’à partir d’un seul emplacement, faites en sorte qu’elles soient nettement visibles par l’utilisateur sans masquer le reste du contenu. Si l’utilisateur ne distingue pas immédiatement une fenêtre contextuelle, il risque de la faire disparaître en recherchant une page d’aide.
-   **Évitez d’utiliser une quantité de ressources excessive:** l’aide ne doit pas s’afficher avec un temps de décalage ni mettre du temps à se charger. L’utilisation de vidéos, de fichiers audio ou d’images haute résolution dans des fenêtres contextuelles est plus susceptible de frustrer l’utilisateur que de l’aider véritablement.

### <span id="descriptions"></span><span id="DESCRIPTIONS"></span>Descriptions

Il peut parfois être utile de fournir un complément d’informations concernant une fonctionnalité au moment où l’utilisateur accède à cette dernière. Les descriptions sont comparables à une interface utilisateur d’instructions, mais il existe une différence essentielle entre ces deux types d’aides: l’interface utilisateur d’instructions tente d’enseigner et de faire découvrir à l’utilisateur des fonctionnalités qu’il ne connaît pas encore, alors que les descriptions détaillées enrichissent les connaissances d’un utilisateur concernant des fonctionnalités qui l’intéressent déjà:

-   **N’enseignez pas les concepts de base:** supposez que l’utilisateur possède déjà les connaissances de base en matière d’utilisation de l’élément décrit. L’utilisateur a besoin de clarifications ou de compléments d’informations. Il est inutile de lui fournir des informations qu’il connaît déjà.
-   **Décrivez les interactions intéressantes:** l’une des fonctions les plus utiles des descriptions consiste à informer l’utilisateur des interactions possibles de fonctionnalités qu’il connaît déjà. Cette approche complète les connaissances des utilisateurs sur les fonctions qu’ils ont pris l’habitude d’utiliser.
-   **Restez à l’écart:** à l’instar d’une interface utilisateur d’instructions, les descriptions ne doivent pas interférer avec le plaisir que prend l’utilisateur à manipuler l’application.

## <span id="related_topics"></span>Articles connexes

* [Recommandations en matière d’aide de l’application](guidelines-for-app-help.md)



<!--HONumber=Aug16_HO3-->


