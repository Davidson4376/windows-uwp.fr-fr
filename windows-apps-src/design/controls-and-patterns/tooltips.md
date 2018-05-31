---
author: Jwmsft
Description: Use a tooltip to reveal more info about a control before asking the user to perform an action.
title: Info-bulles
ms.assetid: A21BB12B-301E-40C9-B84B-C055FD43D307
label: Tooltips
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: b60b06d9dbe8c0eb6216c2c909cc5184855056d5
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2018
ms.locfileid: "1493606"
---
# <a name="tooltips"></a>Info-bulles
 

Une info-bulle est une brève description qui est liée à un autre contrôle ou objet. Les info-bulles aident à comprendre des objets peu familiers qui ne sont pas directement décrits dans l’interface utilisateur. Une info-bulle s’affiche automatiquement lorsque l’utilisateur déplace le focus, appuie de façon prolongée ou pointe avec sa souris sur un contrôle. L’info-bulle disparaît après quelques secondes, ou lorsque l’utilisateur déplace le focus du doigt, du pointeur ou du clavier/boîtier de commande.

![Info-bulle](images/controls/tool-tip.png)

> **API importantes**: [classe ToolTip](https://msdn.microsoft.com/library/windows/apps/br227608), [classe ToolTipService](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.tooltipservice)


## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Utilisez une info-bulle pour fournir plus d’informations sur un contrôle avant d’inviter l’utilisateur à effectuer une action. Les info-bulles doivent être utilisées avec parcimonie, et uniquement quand elles aident l’utilisateur à accomplir une tâche. En règle générale, si les informations sont déjà disponibles autre part, n’ajoutez pas une info-bulle. Une info-bulle utile donne un éclairage sur une action qui n’est pas bien expliquée.

Quand utiliser une info-bulle Pour vous décider, posez-vous les questions suivantes:

-   **Les informations doivent-elles devenir visibles lors du passage du pointeur?**
    Si ce n’est pas le cas, utilisez un autre contrôle. Les info-bulles doivent uniquement être affichées par une action de l’utilisateur; ne les laissez jamais s’afficher d’elles-mêmes.

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

## <a name="example"></a>Exemple

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/ToolTip">ouvrir l’application et voir le contrôle ToolTip en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Une info-bulle dans l’application Bing Cartes.

![Une info-bulle dans l’application Bing Cartes](images/control-examples/tool-tip-maps.png)

## <a name="recommendations"></a>Recommandations

- Utilisez les info-bulles avec parcimonie (ou pas du tout). Les info-bulles sont une interruption. Une info-bulle peut être aussi distrayante qu’une fenêtre contextuelle. Dès lors, utilisez-les seulement si elles apportent un plus.
- Veillez à ce que le texte de l’info-bulle soit concis. Les info-bulles sont adaptées aux phrases courtes et aux fragments de phrases. La lecture de grands blocs de texte peut prendre du temps et l’info-bulle risque d’expirer avant que l’utilisateur ait fini de la lire.
- Veillez à ce que l’information complémentaire soit utile et pertinente. Le texte de l’info-bulle doit être instructif. N’écrivez pas quelque chose qui est évident ou qui apparaît déjà à l’écran. Le texte de l’info-bulle n’étant pas toujours visible, il doit apporter une information complémentaire que l’utilisateur n’est pas obligé de lire. Les informations importantes doivent être communiquées via un étiquetage explicite des contrôles ou via un texte complémentaire placé directement dans l’interface utilisateur.
- Utilisez les images de façon appropriée. Il est parfois préférable d’utiliser une image dans une info-bulle. Par exemple, quand l’utilisateur passe le curseur sur un lien hypertexte, vous pouvez utiliser une info-bulle pour afficher un aperçu de la page liée.
- N’utilisez pas d’info-bulle pour afficher du texte déjà visible dans l’interface utilisateur. Par exemple, ne placez pas sur un bouton une info-bulle dont le texte est identique à celui du bouton.
- Ne placez pas de contrôles interactifs à l’intérieur de l’info-bulle.
- Ne placez pas d’images qui semblent être interactives à l’intérieur de l’info-bulle.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles associés

- [Classe ToolTip](https://msdn.microsoft.com/library/windows/apps/br227608)
