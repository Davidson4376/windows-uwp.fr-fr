---
author: mijacobs
Description: Dialogs and flyouts display transient UI elements that appear when the user requests them or when something happens that requires notification or approval.
title: Boîtes de dialogue et menus volants
template: detail.hbs
ms.author: mijacobs
ms.date: 07/06/2018
ms.topic: article
keywords: windows10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d4ff66e988634cf1ba48809688ea6535e6e95b03
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5755783"
---
# <a name="dialogs-and-flyouts"></a>Boîtes de dialogue et menus volants



Les boîtes de dialogue et les menus volants sont des éléments temporaires d’interface utilisateur qui s’affichent quand un événement se produit qui nécessite une notification, une approbation ou d’autres informations de l’utilisateur.

> **API importantes**: [classe ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog), [classe Flyout](/uwp/api/Windows.UI.Xaml.Controls.Flyout)


:::row:::
    :::column:::
        **Dialogs**
        
        ![Example of a dialog](../images/dialogs/dialog_RS2_delete_file.png)

        Dialogs are modal UI overlays that provide contextual app information. Dialogs block interactions with the app window until being explicitly dismissed. They often request some kind of action from the user.
    :::column-end:::
    :::column::: 
        **Flyouts**

        ![Example of a flyout](../images/flyout-example2.png)

        A flyout is a lightweight contextual popup that displays UI related to what the user is doing. It includes placement and sizing logic, and can be used to reveal a secondary control or show more detail about an item.

        Unlike a dialog, a flyout can be quickly dismissed by tapping or clicking somewhere outside the flyout, pressing the Escape key or Back button, resizing the app window, or changing the device's orientation.
    :::column-end:::
:::row-end:::


## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Les boîtes de dialogue et les menus volants permettent aux utilisateurs de prendre connaissance d’informations importantes, mais elles perturbent également l’expérience utilisateur. Les boîtes de dialogue étant modales (bloquantes), elles interrompent les utilisateurs et les empêchent de faire autre chose tant qu’ils n’interagissent pas avec la boîte de dialogue. Les menus volants sont moins dérangeants, mais si vous en affichez trop, vous perturbez également l’utilisateur.

Une fois que vous avez déterminé que vous voulez utiliser une boîte de dialogue ou un menu volant, vous devez choisir lequel utiliser.

Étant donné que les boîtes de dialogue bloquent les interactions contrairement aux menus volants, elles doivent être réservées aux situations dans lesquelles vous voulez que l’utilisateur interrompe tout ce qu’il est en train de faire pour se concentrer sur une information particulière ou répondre à une question. Les menus volants, quant à eux, peuvent être utilisés quand vous voulez attirer l’attention de l’utilisateur sur quelque chose, mais qu’il a la possibilité de l’ignorer.

:::row:::
    :::column:::
   <p><b>Utilisez une boîte de dialogue pour...</b> <br/>
<ul>
<li>Afficher des informations importantes que l’utilisateur <b>doit</b> lire et accepter avant de poursuivre. Par exemple:
<ul>
  <li>Utilisez ce type de contrôle pour indiquer à l’utilisateur toute situation d’atteinte possible à la sécurité.</li>
  <li>Utilisez ce type de contrôle pour signaler à l’utilisateur qu’il s’apprête à modifier de manière irrémédiable un élément utile.</li>
  <li>Utilisez ce type de contrôle pour signaler à l’utilisateur qu’il s’apprête à supprimer un élément utile.</li>
  <li>Pour confirmer un achat dans l’application</li>
</ul>

</li>
<li>Les messages d’erreur qui s’appliquent au contexte global de l’application, liés par exemple à une erreur de connectivité.</li>
<li>Utilisez une boîte de dialogue à question pour indiquer que l’application doit poser à l’utilisateur une question bloquante, parce qu’elle ne peut pas choisir telle ou telle option à la place de l’utilisateur, par exemple. Une question bloquante ne peut pas être ignorée ni reportée, et doit offrir à l’utilisateur des options clairement définies.</li>
</ul>
</p>
    :::column-end:::
    :::column:::
   <p><b>Utilisez un menu volant pour...</b> <br/>
<ul>
<li>Collecter des informations supplémentaires nécessaires pour pouvoir effectuer une action.</li>
<li>Afficher des informations qui ne sont pas pertinentes le reste du temps. Par exemple, dans une application de galerie de photos, quand l’utilisateur clique sur une vignette d’image, vous pouvez utiliser un menu volant pour afficher une version agrandie de l’image.</li>
<li>Affichage d’informations supplémentaires, comme des détails ou des descriptions plus longues sur un élément de la page.</li>
</ul></p>
    :::column-end:::
:::row-end:::


## <a name="ways-to-avoid-using-dialogs-and-flyouts"></a>Méthodes pour éviter les boîtes de dialogue et menus volants

Mesurez l’importance des informations à partager: sont-elles suffisamment importantes pour interrompre l’utilisateur? Évaluez également la fréquence à laquelle les informations doivent être affichées. Si vous affichez une boîte de dialogue ou une notification toutes les 5minutes, vous pouvez peut-être plutôt leur allouer un emplacement dans l’interface utilisateur principale. Par exemple, dans un client de chat, au lieu d’afficher un menu volant chaque fois qu’un ami se connecte, vous pouvez afficher la liste des amis en ligne sur le moment et mettre en évidence les amis quand ils se connectent.

Les boîtes de dialogue sont fréquemment utilisées pour confirmer une action (par exemple, la suppression d’un fichier) avant de l’exécuter. Si vous voulez que l’utilisateur effectue souvent une action particulière, fournissez un moyen d’annuler l’action quand il fait une erreur plutôt que de forcer l’utilisateur à confirmer l’action chaque fois.

## <a name="how-to-create-a-dialog"></a>Comment créer une boîte de dialogue

Consultez l' [article des boîtes de dialogue](dialogs.md). 

## <a name="how-to-create-a-flyout"></a>Comment créer un menu volant

Consultez l' [article de menu volant](flyouts.md). 

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour ouvrir l’application et voir l'objet <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> ou <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> en action.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

