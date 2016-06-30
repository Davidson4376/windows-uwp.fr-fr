---
author: Jwmsft
Description: "Utilisez une info-bulle pour fournir plus d’informations sur un contrôle avant d’inviter l’utilisateur à effectuer une action."
title: Info-bulles
ms.assetid: A21BB12B-301E-40C9-B84B-C055FD43D307
label: Tooltips
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 0529f212e9fac290bb58925e992518ab0e803bfa


---

# Info-bulles



Une info-bulle est une brève description qui est liée à un autre contrôle ou objet. Les info-bulles aident à comprendre des objets peu familiers qui ne sont pas directement décrits dans l’interface utilisateur. Une info-bulle s’affiche automatiquement lorsque l’utilisateur déplace le focus, appuie de façon prolongée ou pointe avec sa souris sur un contrôle. L’info-bulle disparaît après quelques secondes, ou lorsque l’utilisateur déplace le focus du doigt, du pointeur ou du clavier/boîtier de commande.

![Info-bulle](images/controls/tool-tip.png)

<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [**Classe ToolTip**](https://msdn.microsoft.com/library/windows/apps/br227608)
-   [**Classe ToolTipService**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.tooltipservice)

## Est-ce le contrôle approprié ?

Utilisez une info-bulle pour fournir plus d’informations sur un contrôle avant d’inviter l’utilisateur à effectuer une action. Les info-bulles doivent être utilisées avec parcimonie, et uniquement quand elles aident l’utilisateur à accomplir une tâche. En règle générale, si les informations sont déjà disponibles autre part, n’ajoutez pas une info-bulle. Une info-bulle utile donne un éclairage sur une action qui n’est pas bien expliquée.

Quand utiliser une info-bulle Pour vous décider, posez-vous les questions suivantes :

-   **Les informations doivent-elles devenir visibles lors du passage du pointeur ?**
    Si ce n’est pas le cas, utilisez un autre contrôle. Les info-bulles doivent uniquement être affichées par une action de l’utilisateur ; ne les laissez jamais s’afficher d’elles-mêmes.

-   **Le contrôle a-t-il une étiquette avec un libellé ?**
    Si ce n’est pas le cas, utilisez une info-bulle pour l’indiquer. Attribuer une étiquette à la plus grande partie des contrôles est une bonne habitude de conception de l’expérience utilisateur et pour ceci, vous n’avez pas besoin d’info-bulles. Les contrôles et les commandes des barres d’outils affichant seulement des icônes doivent être accompagnés d’une info-bulle.

-   **Un objet a-t-il besoin d’une description ou d’informations complémentaires ?**
    Si c’est le cas, utilisez une info-bulle. Mais le texte doit être complémentaire, c’est-à-dire non essentiel aux tâches principales. S’il est essentiel, mettez-le directement dans l’interface utilisateur de sorte que les utilisateurs le trouvent sans peine.

-   **L’information complémentaire est-elle une erreur, un avertissement ou un état ?**
    Si c’est le cas, utilisez un autre élément d’interface utilisateur, tel un menu volant.

-   **Les utilisateurs doivent-ils interagir avec l’info-bulle ?**
    Le cas échéant, utilisez un autre contrôle. Il est impossible d’interagir avec une info-bulle parce qu’elle disparaît lorsqu’on déplace la souris.

-   **Les utilisateurs ont-ils besoin d’imprimer l’information complémentaire ?**
    Le cas échéant, utilisez un autre contrôle.

-   **Les utilisateurs pourraient-ils être ennuyés ou gênés par les info-bulles ?**
    Si cela peut être le cas, envisagez une autre solution, notamment de ne rien ajouter du tout. Si les info-bulles sont susceptibles de gêner l’utilisateur, permettez-lui de les désactiver.

## Exemple

Une info-bulle dans l’application Bing Cartes.

![Une info-bulle dans l’application Bing Cartes](images/control-examples/tool-tip-maps.png)

## Recommandations

-   Utilisez les info-bulles avec parcimonie (ou pas du tout). Les info-bulles sont une interruption. Une info-bulle peut être aussi distrayante qu’une fenêtre contextuelle. Dès lors, utilisez-les seulement si elles apportent un plus.
-   Veillez à ce que le texte de l’info-bulle soit concis. Les info-bulles sont adaptées aux phrases courtes et aux fragments de phrases. La lecture de grands blocs de texte peut prendre du temps et l’info-bulle risque d’expirer avant que l’utilisateur ait fini de la lire.
-   Veillez à ce que l’information complémentaire soit utile et pertinente. Le texte de l’info-bulle doit être instructif. N’écrivez pas quelque chose qui est évident ou qui apparaît déjà à l’écran. Le texte de l’info-bulle n’étant pas toujours visible, il doit apporter une information complémentaire que l’utilisateur n’est pas obligé de lire. Les informations importantes doivent être communiquées via un étiquetage explicite des contrôles ou via un texte complémentaire placé directement dans l’interface utilisateur.
-   Utilisez les images de façon appropriée. Il est parfois préférable d’utiliser une image dans une info-bulle. Par exemple, quand l’utilisateur passe le curseur sur un lien hypertexte, vous pouvez utiliser une info-bulle pour afficher un aperçu de la page liée.
-   N’utilisez pas d’info-bulle pour afficher du texte déjà visible dans l’interface utilisateur. Par exemple, ne placez pas sur un bouton une info-bulle dont le texte est identique à celui du bouton.
-   Ne placez pas de contrôles interactifs à l’intérieur de l’info-bulle.
-   Ne placez pas d’images qui semblent être interactives à l’intérieur de l’info-bulle.

<span id="related_topics"></span>Rubriques connexes
-----------------------------------------------

* [**Classe ToolTip**](https://msdn.microsoft.com/library/windows/apps/br227608)



<!--HONumber=Jun16_HO4-->


