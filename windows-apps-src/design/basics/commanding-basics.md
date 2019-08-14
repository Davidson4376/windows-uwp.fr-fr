---
Description: Dans une application de plateforme Windows universelle (UWP), les éléments de commande sont les éléments d’interface utilisateur interactifs qui permettent à l’utilisateur d’effectuer des actions telles que l’envoi d’un message électronique, la suppression d’un élément ou l’envoi d’un formulaire.
title: Informations de base relatives à la conception des commandes pour les applications de plateforme Windows universelle (UWP)
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 11/01/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8b33fe420e93c9ce78c625ad365ec8dc10e343ad
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867454"
---
# <a name="command-design-basics-for-uwp-apps"></a>Informations de base relatives à la conception des commandes pour les applications UWP

Dans une application de plateforme Windows universelle (UWP), les *éléments de commande* sont les éléments d’interface utilisateur interactifs qui permettent aux utilisateurs d’effectuer des actions, telles que l’envoi d’un e-mail, la suppression d’un élément ou l’envoi d’un formulaire. Les *interfaces de commande* sont constituées d’éléments de commande courants, des surfaces de commande qui les hébergent, des interactions qu’elles prennent en charge et des expériences proposées.

## <a name="provide-the-best-command-experience"></a>Proposer la meilleure expérience de commande

L’aspect le plus important d’une interface de commande est ce que vous essayez de laisser un utilisateur accomplir. Lorsque vous planifiez la fonctionnalité de votre application, envisagez les étapes requises pour effectuer ces tâches et les expériences utilisateur que vous souhaitez activer. Une fois que vous avez élaboré une première ébauche de ces expériences, vous pouvez prendre des décisions concernant les outils et les interactions pour les implémenter.

Voici quelques expériences de commande courantes :

- Envoi d’informations
- Sélection de paramètres et d’options
- Recherche et filtrage de contenu
- Ouverture, enregistrement et suppression de fichiers
- Modification ou création de contenu

Soyez créatif avec la conception de vos expériences de commande. Choisissez les appareils d’entrée pris en charge par votre application et la façon dont votre application répond à chaque appareil. Par la prise en charge du plus large éventail de fonctionnalités et de préférences, vous rendez votre application aussi utilisable, portable et accessible que possible (consultez [Conception des commandes pour les applications de plateforme Windows universelle (UWP)](../controls-and-patterns/commanding.md) pour plus de détails).



<!--
When designing a command interface, the most important decision is choosing what a user can do. To plan the right type of interactions, focus on your app - consider the user experiences you want to enable, and what steps users will need to take. Once you decide what you want users to accomplish, then you can provide them the tools to do so.
-->

## <a name="choose-the-right-command-elements"></a>Choisir les éléments de commande appropriés

L’utilisation d’éléments adéquats dans une interface de commande peut faire toute la différence entre une application intuitive et facile à utiliser, et une application dont l’utilisation est complexe et déroutante. Un ensemble complet d’éléments de commande est disponible dans la plateforme Windows universelle (UWP). Voici une liste de quelques-uns des éléments de commande UWP les plus courants.

:::row:::
    :::column:::
![image du bouton](images/commanding/thumbnail-button.svg)
    :::column-end:::
    :::column span="2":::
<b>Boutons</b>

Les <a href="../controls-and-patterns/buttons.md" style="text-decoration:none">boutons</a> déclenchent une action immédiate. Il peut s’agir de l’envoi d’un e-mail, de l’envoi des données d’un formulaire ou de la confirmation d’une action dans une boîte de dialogue.
:::row-end:::

:::row:::
    :::column:::
![image de liste](images/commanding/thumbnail-list.svg)
    :::column-end:::
    :::column span="2":::
<b>Listes</b>

Les<a href="../controls-and-patterns/lists.md" style="text-decoration:none">listes</a>présentent les éléments dans une liste ou une grille interactives. Elles sont généralement utilisées pour de nombreuses options ou pour afficher les éléments. Il peut s’agir d’une liste déroulante, d’une zone de liste, d’un affichage de liste et d’un affichage de grille.
:::row-end:::

:::row:::
    :::column:::
![image de contrôle de sélection](images/commanding/thumbnail-selection.svg)
    :::column-end:::
    :::column span="2":::
<b>Contrôles de sélection</b>

Ils permettent aux utilisateurs de choisir entre différentes options, par exemple, pour remplir un questionnaire ou configurer des paramètres d’application. Il peut s’agir d’une <a href="../controls-and-patterns/checkbox.md">case à cocher</a>, d’une <a href="../controls-and-patterns/radio-button.md">case d’option</a> et d’un <a href="../controls-and-patterns/toggles.md">bouton bascule</a>.
:::row-end:::

:::row:::
    :::column:::
![Image de calendrier](images/commanding/thumbnail-calendar.svg)
    :::column-end:::
    :::column span="2":::
<b>Sélecteurs de calendriers, de date et d’heure</b>

Les <a href="../controls-and-patterns/date-and-time.md">sélecteurs de calendriers, de date et d’heure</a> permettent aux utilisateurs d’afficher et de modifier les informations de date et d’heure, notamment pour créer un événement ou configurer d’une alarme. Il peut s’agir d’un sélecteur de date du calendrier, d’un affichage de calendrier, d’un sélecteur de date et d’un sélecteur d’heure.
:::row-end:::

:::row:::
    :::column:::
![Image de saisie de texte prédictive](images/commanding/thumbnail-autosuggest.svg)
    :::column-end:::
    :::column span="2":::
<b>Saisie de texte prédictive</b>

Elle fournit des suggestions au fur et à mesure que les utilisateurs entrent des données ou émettent des requêtes. Il peut s’agir d’une <a href="../controls-and-patterns/auto-suggest-box.md">zone de suggestion automatique</a>.<br>
:::row-end:::

Pour obtenir la liste complète de ces éléments, voir [Contrôles et éléments d’interface utilisateur](../controls-and-patterns/index.md).

## <a name="place-commands-on-the-right-surface"></a>Placer les commandes sur la surface appropriée

Vous pouvez placer les éléments de commande sur un certain nombre de surfaces dans votre application, notamment le canevas d’application ou des conteneurs de commande spécifiques, comme une barre de commandes, un menu volant de barre de commandes, une barre de menus ou une boîte de dialogue.

Essayez toujours de laisser les utilisateurs manipuler du contenu directement plutôt que par le biais des commandes qui agissent sur le contenu, par exemple le glisser-déplacer pour réorganiser des éléments de liste plutôt que des boutons de commande vers le haut et vers le bas. 

Toutefois, cela peut ne pas être possible avec certains appareils d’entrée ou lors de la prise en compte de préférences et de capacités spécifiques à l’utilisateur. Dans ce cas, fournissez autant d’affordances de commandes que possible et placez ces éléments de commande sur une surface de commande dans votre application.

Voici une liste de quelques-unes des surfaces de commande les plus courantes.

:::row:::
    :::column:::
![image de zones de dessin d’application](images/commanding/thumbnail-canvas.svg)
    :::column-end:::
    :::column span="2":::
<b>Zones de dessin d’application (zone de contenu)</b>

Si une commande est nécessaire en tout temps pour permettre aux utilisateurs d’effectuer des scénarios de base, placez-la sur la zone de dessin. Comme vous pouvez placer les commandes près des objets qu’elles affectent (ou sur ceux-ci), le fait de les placer sur le Canvas rend leur utilisation facile et évidente. Toutefois, choisissez les commandes que vous placez sur le Canvas avec soin. S’il y a trop de commandes sur la zone de dessin de l’application, vous perdrez de l’espace précieux sur l’écran et risquez de submerger l’utilisateur. Si la commande n’est pas fréquemment utilisée, pensez à la placer sur une autre surface de commande.
:::row-end:::

:::row:::
    :::column:::
![image CommandBar](images/commanding/thumbnail-commandbar.svg)
    :::column-end:::
    :::column span="2":::
<b>Barres de commandes et barres de menus</b>

Les <a href="../controls-and-patterns/app-bars.md">barres de commandes</a> simplifient l’organisation des commandes et en facilitent l’accès. Les barres de commandes peuvent être placées en haut de l’écran, en bas de l’écran, ou les deux (une <a href="../controls-and-patterns/menus.md#create-a-menu-bar">barre de menus</a> peut également être utilisée lorsque la fonctionnalité de votre application est trop complexe pour une barre de commandes).
:::row-end:::

:::row:::
    :::column:::
![image de menu contextuel](images/commanding/thumbnail-contextmenu.svg)
    :::column-end:::
    :::column span="2":::
<b>Menus et menus contextuels</b>

<p>Les menus et les menus contextuels permettent de gagner de l’espace en organisant les commandes et en les masquant jusqu’à ce que l’utilisateur en ait besoin. Les utilisateurs accèdent généralement à un menu ou à un menu contextuel en cliquant sur un bouton ou en cliquant avec le bouton droit sur un contrôle.</p> 

<p>Le <a href="../controls-and-patterns/command-bar-flyout.md">menu volant de barre de commandes </a> est un type de menu contextuel qui regroupe les avantages d’une barre de commandes et d’un menu contextuel en un seul contrôle. Il peut fournir des raccourcis pour les actions fréquemment effectuées et un accès aux commandes secondaires qui ne sont pertinentes que dans certains contextes, comme le presse-papier ou les commandes personnalisées.</p>

<p>UWP fournit également un ensemble de menus traditionnels et de menus contextuels. Pour plus d’informations, consultez la <a href="../controls-and-patterns/menus.md">vue d’ensemble des menus et des menus contextuels</a>.</p>
:::row-end:::

## <a name="provide-command-feedback"></a>Fournir des commentaires sur la commande 

Les commentaires sur la commande communiquent aux utilisateurs qu’une interaction ou commande a été détectée, comment la commande a été interprétée et gérée, et si la commande a réussi ou non. Cela permet aux utilisateurs de comprendre ce qu’ils ont fait et ce qu’ils peuvent faire ensuite. Idéalement, les commentaires doivent être intégrés naturellement à l’interface utilisateur pour que les utilisateurs n’aient pas à être interrompus ou à prendre de mesures supplémentaires, à moins que cela ne soit absolument nécessaire.

> [!NOTE]
> Fournissez des commentaires uniquement quand cela est nécessaire et quand ils ne sont pas disponibles ailleurs. Maintenez l’interface utilisateur de votre application claire et simple, à moins que cela n’ajoute une valeur.

Voici quelques solutions pour fournir des commentaires dans votre application.

:::row:::
    :::column:::
![image de la zone de contenu CommandBar](images/commanding/thumbnail-commandbar2.svg)
    :::column-end:::
    :::column span="2":::
<b>Barre de commandes</b>

La zone de contenu de la <a href="../controls-and-patterns/app-bars.md">barre de commandes</a> est un espace intuitif pour communiquer le statut aux utilisateurs s’ils veulent voir les commentaires.
:::row-end:::

:::row:::
    :::column:::
![Image du menu volant](images/commanding/thumbnail-flyout.svg)
    :::column-end:::
    :::column span="2":::
<b>Menus volants</b>

       Les <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">menus volants</a> sont des fenêtres contextuelles légères qui peuvent être fermées en appuyant ou en cliquant quelque part à l’extérieur du menu volant.
:::row-end:::

:::row:::
    :::column:::
![Image de la boîte de dialogue](images/commanding/thumbnail-dialog.svg)
    :::column-end:::
    :::column span="2":::
<b>Contrôles de boîte de dialogue</b>

Les <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">contrôles de boîtes de dialogue</a> sont des superpositions d’interface utilisateur modales qui fournissent des informations contextuelles sur l’application. Dans la plupart des cas, les boîtes de dialogue bloquent les interactions avec la fenêtre de l’application jusqu’à ce qu’elles soient masquées explicitement. Elles demandent souvent un type d’action de l’utilisateur. Les boîtes de dialogue peuvent nuire à la fluidité de l’expérience et ne doivent être utilisées que dans certaines situations. Pour plus d’informations, voir la section [Quand confirmer ou annuler des actions](#when-to-confirm-or-undo-actions).
    :::column-end:::
:::row-end:::

> [!TIP]
> Prenez garde à ne pas utiliser un trop grand nombre de boîtes de dialogue de confirmation dans votre application : elles peuvent être très utiles en cas d’erreur de l’utilisateur, mais elles représentent un obstacle chaque fois que l’utilisateur tente d’effectuer une action intentionnellement.

### <a name="when-to-confirm-or-undo-actions"></a>Quand confirmer ou annuler des actions

Même si l’interface utilisateur de votre application a été très bien conçue, il arrivera inévitablement à tous les utilisateurs d’effectuer une action non souhaitée. Votre application peut remédier à ce genre de situation en demandant à l’utilisateur de confirmer une action ou en lui offrant un moyen d’annuler les actions récentes.

:::row:::
    :::column:::
![Image d’action à faire](images/do.svg)

Pour les actions qui ne peuvent pas être annulées et dont les conséquences sont importantes, nous vous recommandons d’utiliser une boîte de dialogue de confirmation. Exemples de telles actions :
-   Remplacement d’un fichier
-   Non-enregistrement d’un fichier avant la fermeture
-   Confirmation de la suppression définitive d’un fichier ou de données
-   Achat (sauf si l’utilisateur refuse de demander une confirmation)
-   Envoi d’un formulaire, par exemple pour s’inscrire à un programme
    :::column-end:::
    :::column:::
![Image d’action à faire](images/do.svg)

Pour les actions qui peuvent être annulées, une simple commande d’annulation est généralement suffisante. Exemples de telles actions :
-   Suppression d’un fichier
-   Suppression d’un e-mail (pas définitivement)
-   Modification du contenu ou d’un texte
-   Changement de nom d’un fichier
:::row-end:::

##  <a name="optimize-for-specific-input-types"></a>Optimiser les types d’entrée spécifiques

Voir les [Notions fondamentales sur les interactions](../input/index.md) pour plus d’informations sur l’optimisation des expériences utilisateur avec un type ou un périphérique d’entrée spécifique.

