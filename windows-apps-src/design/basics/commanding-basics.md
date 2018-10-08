---
author: mijacobs
Description: In a Universal Windows Platform (UWP) app, command elements are the interactive UI elements that enable the user to perform actions, such as sending an email, deleting an item, or submitting a form.
title: Informations de base relatives à la conception des commandes pour les applications de plateforme Windows universelle (UWP)
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 10/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 104788b98377b55564fcc204ecc161521d071c6b
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4415252"
---
#  <a name="command-design-basics-for-uwp-apps"></a>Informations de base relatives à la conception des commandes pour les applications UWP

Dans une application de plateforme Windows universelle(UWP), les *éléments de commande* sont les éléments d’interface utilisateur interactifs qui permettent aux utilisateurs d’effectuer des actions, telles que l’envoi d’un message électronique, la suppression d’un élément ou l’envoi d’un formulaire. Cet article décrit les éléments de commande courants, les interactions qu’ils prennent en charge et les surfaces de commande permettant de les héberger.

## <a name="provide-the-right-type-of-interactions"></a>Fournissez le type approprié d’interactions

Lorsque vous créez une interface de commande, la décision la plus importante consiste à choisir ce que les utilisateurs auront la possibilité de faire. Pour planifier le type d’interactions approprié, concentrez-vous sur votre application: étudiez les expériences utilisateur que vous souhaitez favoriser et les étapes que les utilisateurs devront suivre. Une fois que vous avez défini ce que les utilisateurs doivent faire, vous pouvez leur fournir les outils nécessaires pour y parvenir.

Interactions que vous souhaiterez peut-être proposer pour votre application:

- Envoi d’informations 
- Sélection de paramètres et d’options
- Recherche et filtrage de contenu
- Ouverture, enregistrement et suppression de fichiers
- Modification ou création de contenu

## <a name="use-the-right-command-element-for-the-interaction"></a>Utiliser l’élément de commande approprié pour l’interaction

L'utilisation d'éléments adéquats pour favoriser les interactions de commande peut faire toute la différence entre une application intuitive et facile à utiliser, et une application dont l’utilisation est complexe et déroutante. La plateforme Windows universelle(UWP) fournit un large éventail d’éléments de commande que vous pouvez utiliser dans votre application. Le tableau suivant répertorie quelques-uns des contrôles les plus courants et offre un résumé des interactions qu’ils peuvent permettre.

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

Vous pouvez placer des éléments de commande sur un certain nombre de surfaces dans votre application, y compris le canevas d’application ou les conteneurs de commande spécifiques, par exemple, une barre de commandes, menu volant de barre de commandes, barre de menus et boîte de dialogue.

Notez que, si possible, vous devez permettre aux utilisateurs de manipuler le contenu directement plutôt que d’utiliser les commandes qui agissent sur le contenu. Par exemple, laissez les utilisateurs réorganiser les listes en en faisant glisser les éléments au lieu d’utiliser les boutons de commande vers le haut et vers le bas.

Si les utilisateurs ne peuvent pas manipuler le contenu directement, placez des éléments de commande sur une surface de commande dans votre application. Voici une liste de quelques-unes des surfaces de commande les plus courantes.

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

## <a name="provide-feedback-for-interactions"></a>Fournir des commentaires pour les interactions

Les commentaires communiquent les résultats des commandes et permettent aux utilisateurs de comprendre ce qu’ils ont fait et ce qu’ils peuvent faire ensuite. Idéalement, les commentaires doivent être intégrés naturellement à l'interface utilisateur pour que les utilisateurs n’aient pas à être interrompus ou à prendre de mesures supplémentaires, à moins que cela ne soit absolument nécessaire. 

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

Même si l’interface utilisateur est parfaitement pensée et que l’utilisateur fait preuve de la plus grande précaution, il arrivera inévitablement à celui-ci d’effectuer une action non souhaitée. Votre application peut remédier à ce genre de situation en demandant à l’utilisateur de confirmer une action ou en lui offrant un moyen d’annuler les actions récentes.

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

