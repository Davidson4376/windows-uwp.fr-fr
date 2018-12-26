---
Description: In a Universal Windows Platform (UWP) app, command elements are the interactive UI elements that enable the user to perform actions, such as sending an email, deleting an item, or submitting a form.
title: Informations de base relatives à la conception des commandes pour les applications de plateforme Windows universelle (UWP)
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.date: 10/01/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0f78ffb01bf29076733af40b365e91592c71e9c8
ms.sourcegitcommit: 17896441726714fa66b5ca4f9df2cdb2259f360e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/26/2018
ms.locfileid: "8988248"
---
# <a name="command-design-basics-for-uwp-apps"></a>Informations de base relatives à la conception des commandes pour les applications UWP

Dans une application de plateforme Windows universelle (UWP), les *éléments de commande* sont des éléments d’interface utilisateur interactifs qui permettent aux utilisateurs d’effectuer des actions telles que l’envoi d’un message électronique, la suppression d’un élément ou l’envoi d’un formulaire. *Interfaces de commande* sont composées d’éléments de commande courants, les surfaces de commande qui hébergent les, les interactions qu’ils prennent en charge et les expériences qu’ils fournissent.

## <a name="provide-the-best-command-experience"></a>Fournir la meilleure expérience de commande

Le plus important d’une interface de commande est ce que vous essayez permettre à un utilisateur à accomplir. Lorsque vous planifiez les fonctionnalités de votre application, envisagez les étapes requises pour effectuer ces tâches et les expériences utilisateur que vous souhaitez activer. Une fois que vous avez effectué une version préliminaire de ces expériences, puis vous pouvez prendre des décisions sur les outils et les interactions pour les implémenter.

Voici certaines expériences d’application courants:

- Envoi d’informations
- Sélection de paramètres et d’options
- Recherche et filtrage de contenu
- Ouverture, enregistrement et suppression de fichiers
- Modification ou création de contenu

Faites preuve de créativité avec la conception de votre expérience de commande. Choisissez quels périphériques d’entrée votre application prend en charge, et comment votre application répond à chaque appareil. En prenant en charge la gamme de fonctionnalités et de préférences vous rendez votre application comme accessible que possible, portable et utilisable.



<!--
When designing a command interface, the most important decision is choosing what a user can do. To plan the right type of interactions, focus on your app - consider the user experiences you want to enable, and what steps users will need to take. Once you decide what you want users to accomplish, then you can provide them the tools to do so.
-->

## <a name="choose-the-right-command-elements"></a>Choisissez les éléments de commande approprié

Utiliser les éléments adéquats dans une interface de commande peut faire la différence entre une application intuitive et facile à utiliser et d’une application est complexe et déroutante. Un ensemble complet d’éléments de commande sont disponibles dans la plateforme Windows universelle (UWP). Voici une liste de quelques-unes des éléments de commande plus courants de UWP.

:::row:::
    :::column:::
        ![button image](images/commanding/thumbnail-button.svg)
    :::column-end:::
    :::column span="2":::
        <b>Buttons</b>

        <a href="../controls-and-patterns/buttons.md" style="text-decoration:none">Buttons</a> trigger an immediate action. Examples include sending an email, submitting form data, or confirming an action in a dialog.
:::row-end:::

:::row:::
    :::column:::
        ![list image](images/commanding/thumbnail-list.svg)
    :::column-end:::
    :::column span="2":::
        <b>Lists</b>

        <a href="../controls-and-patterns/lists.md" style="text-decoration:none">Lists</a> present items in a interactive list or a grid. Usually used for many options or display items. Examples include drop-down list, list box, list view and grid view.
:::row-end:::

:::row:::
    :::column:::
        ![selection control image](images/commanding/thumbnail-selection.svg)
    :::column-end:::
    :::column span="2":::
        <b>Selection controls</b>

        Lets users choose from a few options, such as when completing a survey or configuring app settings. Examples include <a href="../controls-and-patterns/checkbox.md">check box</a>, <a href="../controls-and-patterns/radio-button.md">radio button</a>, and <a href="../controls-and-patterns/toggles.md">toggle switch</a>.
:::row-end:::

:::row:::
    :::column:::
        ![Calendar  image](images/commanding/thumbnail-calendar.svg)
    :::column-end:::
    :::column span="2":::
        <b>Calendar, date and time pickers</b>

        <a href="../controls-and-patterns/date-and-time.md">Calendar, date and time pickers</a> enable users to view and modify date and time info, such as when creating an event or setting an alarm. Examples include calendar date picker, calendar view, date picker, time picker.
:::row-end:::

:::row:::
    :::column:::
        ![Predictive text entry image](images/commanding/thumbnail-autosuggest.svg)
    :::column-end:::
    :::column span="2":::
        <b>Predictive text entry</b>

        Provides suggestions as users type, such as when entering data or performing queries. Examples include <a href="../controls-and-patterns/auto-suggest-box.md">auto-suggest box</a>.<br>
:::row-end:::

Pour obtenir la liste complète de ces éléments, voir [Contrôles et éléments d’interface utilisateur](../controls-and-patterns/index.md).

## <a name="place-commands-on-the-right-surface"></a>Placer les commandes sur la surface appropriée

Vous pouvez placer des éléments de commande sur un certain nombre de surfaces dans votre application, notamment le canevas d’application ou les conteneurs de commande spécifiques, par exemple, une barre de commandes, menu volant de barre de commandes, barre de menus ou boîte de dialogue.

Toujours essayer permettre aux utilisateurs de manipuler le contenu directement plutôt que par le biais des commandes qui agissent sur le contenu, par exemple, le glisser -déplacer pour réorganiser des éléments de liste plutôt que des boutons de commande haut et bas. 

Toutefois, cela peut être pas possible avec certains périphériques d’entrée, ou lors de la prise en charge de préférences et fonctionnalités choisies utilisateur spécifique. Dans ces cas, fournissez des affordances de commandes autant que possible et placez ces éléments de commande sur une surface de commande dans votre application.

Voici une liste de quelques-unes des surfaces de commande les plus courantes.

:::row:::
    :::column:::
        ![app canvas image](images/commanding/thumbnail-canvas.svg)
    :::column-end:::
    :::column span="2":::
        <b>App canvas (content area)</b>

        If a command is constantly needed for users to complete core scenarios, put it on the canvas. Because you can put commands near (or on) the objects they affect, putting commands on the canvas makes them easy and obvious to use. However, choose the commands you put on the canvas carefully. Too many commands on the app canvas take up valuable screen space and can overwhelm the user. If the command won't be frequently used, consider putting it in another command surface.
:::row-end:::

:::row:::
    :::column:::
        ![commandbar image](images/commanding/thumbnail-commandbar.svg)
    :::column-end:::
    :::column span="2":::
        <b>Command bars and menu bars</b>

        <a href="../controls-and-patterns/app-bars.md">Command bars</a> help organize commands and make them easy to access. Command bars can be placed at the top of the screen, at the bottom of the screen, or at both the top and bottom of the screen (a <a href="../controls-and-patterns/menus.md#create-a-menu-bar">MenuBar</a> can also be used when the functionality in your app is too complex for a command bar).
:::row-end:::

:::row:::
    :::column:::
        ![context menu image](images/commanding/thumbnail-contextmenu.svg)
    :::column-end:::
    :::column span="2":::
        <b>Menus and context menus</b>

        <p>Menus and context menus save space by organizing commands and hiding them until the user needs them. Users typically access a menu or context menu by clicking a button or right-clicking a control.</p> 

        <p>The <a href="../controls-and-patterns/command-bar-flyout.md">command bar flyout </a> is a type of contextual menu that combines the benefits of a command bar and a context menu into a single control. It can provide shortcuts to commonly-used actions and provide access to secondary commands that are only relevant in certain contexts, such as clipboard or custom commands.</p>

        <p>UWP also provides a set of traditional menus and context menus; for more info, see the <a href="../controls-and-patterns/menus.md">menus and context menus overview</a>.</p>
:::row-end:::

## <a name="provide-command-feedback"></a>Fournir des commentaires de commande 

Commentaires de commande communique aux utilisateurs qu’une interaction ou une commande a été détectée et comment elle a été interprétée et gérée, et si elle a réussi ou non. Cela permet aux utilisateurs de comprendre ce qu’ils ont fait et ce qu’ils peuvent faire ensuite. Idéalement, les commentaires doivent être intégrés naturellement à l'interface utilisateur pour que les utilisateurs n’aient pas à être interrompus ou à prendre de mesures supplémentaires, à moins que cela ne soit absolument nécessaire.

> [!NOTE]
> Ne pas fournir des commentaires à moins que ce soit absolument nécessaire et le retour n’est pas disponible ailleurs. Maintenir la version de votre application, l’interface utilisateur propre et aérée, sauf si vous ajoutez de valeur.

Voici quelques solutions pour fournir des commentaires dans votre application.

:::row:::
    :::column:::
        ![commandbar content area image](images/commanding/thumbnail-commandbar2.svg)
    :::column-end:::
    :::column span="2":::
        <b>Command bar</b>

        The content area of the <a href="../controls-and-patterns/app-bars.md">command bar</a> is an intuitive place to communicate status to users if they'd like to see feedback.
:::row-end:::

:::row:::
    :::column:::
        ![Flyout image](images/commanding/thumbnail-flyout.svg)
    :::column-end:::
    :::column span="2":::
        <b>Flyouts</b>

       <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">Menus volants</a> sont des fenêtres contextuelles contextuelles légers qui peuvent être fermées en appuyant ou en cliquant en dehors du menu volant.
:::row-end:::

:::row:::
    :::column:::
        ![Dialog image](images/commanding/thumbnail-dialog.svg)
    :::column-end:::
    :::column span="2":::
        <b>Dialog controls</b>

        <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">Dialog controls</a> are modal UI overlays that provide contextual app information. In most cases, dialogs block interactions with the app window until being explicitly dismissed, and often request some kind of action from the user. Dialogs can be disruptive and should only be used in certain situations. For more info, see the [When to confirm or undo actions](#when-to-confirm-or-undo-actions) section.
    :::column-end:::
:::row-end:::

> [!TIP]
> Prenez garde à ne pas utiliser un trop grand nombre de boîtes de dialogue de confirmation dans votre application: elles peuvent être très utiles en cas d’erreur de l’utilisateur, mais elles représentent un obstacle chaque fois que l’utilisateur tente d’effectuer une action intentionnellement.

### <a name="when-to-confirm-or-undo-actions"></a>Quand confirmer ou annuler des actions

Même si bien l’interface utilisateur de votre application est, tous les utilisateurs effectuent une action, il arrivera. Votre application peut remédier à ce genre en adoptant la confirmation d’une action, ou en fournissant un moyen d’annuler des actions récentes.

:::row:::
    :::column:::
        ![do image](images/do.svg)

        For actions that can't be undone and have major consequences, we recommend using a confirmation dialog. Examples of such actions include:
        -   Overwriting a file
        -   Not saving a file before closing
        -   Confirming permanent deletion of a file or data
        -   Making a purchase (unless the user opts out of requiring a confirmation)
        -   Submitting a form, such as signing up for something
    :::column-end:::
    :::column:::
        ![do image](images/do.svg)

        For actions that can be undone, offering a simple undo command is usually enough. Examples of such actions include:
        -   Deleting a file
        -   Deleting an email (not permanently)
        -   Modifying content or editing text
        -   Renaming a file
:::row-end:::

##  <a name="optimize-for-specific-input-types"></a>Optimiser les types d’entrée spécifiques

Voir les [Notions fondamentales sur les interactions](../input/index.md) pour plus d’informations sur l’optimisation des expériences utilisateur avec un type ou un périphérique d’entrée spécifique.

