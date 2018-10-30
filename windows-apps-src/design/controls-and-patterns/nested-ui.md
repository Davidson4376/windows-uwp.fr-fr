---
author: muhsinking
Description: Use nested UI to enable multiple actions on a list item
title: Interface utilisateur imbriquée dans des éléments de liste
label: Nested UI in list items
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: 60a29717-56f2-4388-a9ff-0098e34d5896
pm-contact: chigy
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f9bb6daeb01e264cf9cdb0fa9ee9c66738fec972
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5758117"
---
# <a name="nested-ui-in-list-items"></a>Interface utilisateur imbriquée dans des éléments de liste

 

L’interface utilisateur imbriquée est une interface utilisateur (UI) qui expose des contrôles exploitables imbriqués intégrés à l’intérieur d’un conteneur qui peut également faire l’objet d’un focus distinct.

Vous pouvez utiliser l’interface utilisateur imbriquée pour présenter des options supplémentaires permettant d’accélérer la réalisation d’actions importantes. Toutefois, plus le nombre d’actions exposées augmente, plus votre interface utilisateur devient compliquée. Vous devez être particulièrement vigilant lorsque vous choisissez d’utiliser ce modèle d’interface utilisateur. Cet article fournit des recommandations qui vont vous permettre de déterminer le meilleur mode de fonctionnement de votre interface utilisateur spécifique.

> **API importantes**: [classe ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), [classe GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)

Dans cet article, nous abordons la création d’une interface utilisateur imbriquée dans les éléments [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) et [GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx). Bien que les autres cas d’interface utilisateur imbriquée ne soient pas abordés dans cette section, ces concepts sont transférables. Avant de commencer, vous devez être familiarisé avec les instructions générales concernant l’utilisation des contrôles ListView ou GridView dans votre interface utilisateur, qui se trouvent dans les articles [Listes](lists.md) et [Affichage Liste et affichage Grille](listview-and-gridview.md).

Dans cet article, nous utilisons les termes *liste*, *élément de liste*, et *interface utilisateur imbriquée*, comme suit:
- *Liste* fait référence à une collection d’éléments contenus dans un affichage Liste ou Grille.
- *Élément de liste* fait référence à un élément sur lequel un utilisateur peut entreprendre une action dans une liste.
- *Interface utilisateur imbriquée* fait référence aux éléments d’interface utilisateur au sein d’un élément de liste sur lesquels un utilisateur peut entreprendre une action indépendamment d’agir sur l’élément de liste lui-même.

![Parties de l’interface utilisateur imbriquée](images/nested-ui-example-1.png)

> REMARQUE&nbsp;&nbsp;Les contrôles ListView et GridView proviennent de la classe [ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx); ils possèdent donc les mêmes fonctionnalités mais affichent les données différemment. Dans cet article, lorsque nous parlons de listes, les informations s’appliquent aux contrôles ListView et GridView.

## <a name="primary-and-secondary-actions"></a>Actions principales et secondaires

Lorsque vous créez une interface utilisateur avec une liste, envisagez les actions que l’utilisateur peut entreprendre à partir de ces éléments de liste.  

- Un utilisateur peut-il cliquer sur l’élément pour effectuer une action?
    - En règle générale, le fait de cliquer sur un élément de liste génère une action, mais pas nécessairement.
- L’utilisateur peut-il effectuer plusieurs actions?
    - Par exemple, le fait d’appuyer sur un e-mail de la liste a pour effet d’ouvrir ce dernier. Toutefois, l’utilisateur peut vouloir effectuer d’autres actions, telles que la suppression, avant d’ouvrir un e-mail. Le  fait d’accéder à cette action directement dans la liste pourrait intéresser l’utilisateur.
- Comment les actions doivent être exposées à l’utilisateur?
    - Prenez en considération tous les types de saisie. Certaines formes d’interface utilisateur imbriquée fonctionnent parfaitement avec une méthode de saisie, mais peuvent ne pas fonctionner avec d’autres méthodes.  

L’*action principale* est ce à quoi l’utilisateur s’attend qu’il se produise quand il appuie sur l’élément de liste.

Les *actions secondaires* sont généralement des accélérateurs associés à des éléments de liste. Ces accélérateurs peuvent être destinés à la gestion des listes ou aux actions associées à l’élément de liste.

## <a name="options-for-secondary-actions"></a>Options pour les actions secondaires

Quand vous créez l’interface utilisateur de la liste, vous devez d’abord veiller à prendre en compte toutes les méthodes de saisie prises en charge par la plateforme Windows universelle. Pour plus d’informations sur les différents types d’entrée, voir [Notions fondamentales sur la saisie](../input/index.md).

Après s’être assuré que votre application gère toutes les saisies prises en charge par la plateforme UWP, vous devez déterminer si les actions secondaires de votre application sont suffisamment importantes pour figurer comme accélérateurs dans la liste principale. N’oubliez pas que plus le nombre d’actions exposées augmente, plus votre interface utilisateur devient compliquée. Avez-vous vraiment besoin de faire figurer les actions secondaires dans la liste principale de l’interface utilisateur ou pouvez-vous les placer ailleurs?

Vous pouvez envisager de faire figurer des actions supplémentaires dans l’interface utilisateur de la liste principale lorsque ces actions doivent être accessibles à toute saisie en permanence.

Si vous décidez qu’il n’est pas nécessaire de faire figurer des actions secondaires dans l’interface utilisateur de la liste principale, il existe plusieurs autres façons de les exposer à l’utilisateur. Voici quelques options que vous pouvez prendre en considération pour décider de l’endroit où placer des actions secondaires.

### <a name="put-secondary-actions-on-the-detail-page"></a>Placer les actions secondaires sur la page des détails

Placez les actions secondaires sur la page à laquelle l’élément de liste accède lorsque l’on appuie dessus. Lorsque vous utilisez le modèle maître/détails, la page des détails est souvent l’endroit approprié pour placer les actions secondaires.

Pour plus d’informations, voir le [modèle Maître/Détails](master-details.md).

### <a name="put-secondary-actions-in-a-context-menu"></a>Placer les actions secondaires dans un menu contextuel

Placez les actions secondaires dans un menu contextuel auquel l’utilisateur peut accéder par le biais d’un clic droit ou d’un appui prolongé. Cela offre l’avantage de permettre à l’utilisateur d’effectuer une action, telle que la suppression d’un e-mail, sans avoir à charger la page des détails. Il est également recommandé de rendre ces options disponibles sur la page des détails, dans la mesure où les menus contextuels sont destinés à être accélérateurs plutôt qu’une interface utilisateur principale.

Nous recommandons d’utiliser un menu contextuel pour faire figurer des actions secondaires lorsque la saisie s’effectue à partir d’un boîtier de commande ou d’une télécommande.

Pour plus d’informations, voir [Menus contextuels et menus volants](menus.md).

### <a name="put-secondary-actions-in-hover-ui-to-optimize-for-pointer-input"></a>Placez les actions secondaires dans l’interface utilisateur de pointage pour optimiser les entrées du pointeur

Si vous pensez que le mode d’utilisation le plus fréquent de votre application sera la souris et le stylet et si vous souhaitez mettre des actions secondaires à disposition uniquement pour ce mode de saisie, ces dernières seront accessibles uniquement au pointage. Cet accélérateur est visible uniquement lorsqu’une saisie par pointeur est utilisée. Par conséquent, veillez à utiliser les autres options pour prendre en charge d’autres types de saisie.

![Interface utilisateur imbriquée affichée par pointage](images/nested-ui-hover.png)


Pour plus d’informations, voir [Interactions avec la souris](../input/mouse-interactions.md).

## <a name="ui-placement-for-primary-and-secondary-actions"></a>Positionnement de l’interface utilisateur pour les actions principales et secondaires

Si vous décidez que les actions secondaires doivent figurer dans l’interface utilisateur de la liste principale, nous vous recommandons de vous conformer aux instructions suivantes.

Lorsque vous créez un élément de liste comportant des actions principales et secondaires, placez l’action principale à gauche et les actions secondaires à droite. Dans le cas d’une lecture de gauche à droite, les utilisateurs associent les actions situées sur le côté gauche de l’élément de liste comme l’action principale.

Dans ces exemples, il s’agit de l’interface utilisateur de liste dans laquelle l’élément se déroule de manière horizontale (plus en largeur qu’en hauteur). Toutefois, vous pouvez avoir des éléments de forme plus carrée ou plus haut que large. En règle générale, il s’agit d’éléments utilisés dans une grille. Pour ces éléments, si la liste ne défile pas verticalement, vous pouvez placer les actions secondaires au bas de l’élément de liste, et non à droite.

## <a name="consider-all-inputs"></a>Prenez en considération toutes les saisies.

Lorsque vous décidez d’utiliser l’interface utilisateur imbriquée, évaluez également l’expérience utilisateur avec tous les types de saisie. Comme mentionné précédemment, l’interface utilisateur imbriquée fonctionne bien pour certains types de saisie. Toutefois, elle ne fonctionne pas toujours parfaitement pour d’autres. Notamment, l’accès aux éléments de l’interface utilisateur imbriquée peut être difficile via une saisie au clavier, à distance ou par le biais d’une télécommande. Veillez à suivre les instructions ci-dessous pour vous assurer que votre plateforme Windows universelle fonctionne avec tous les types de saisie.

## <a name="nested-ui-handling"></a>Navigation dans l’interface utilisateur imbriquée

Lorsque plusieurs actions sont imbriquées dans l’élément de liste, nous recommandons de suivre ces instructions pour gérer la navigation avec un clavier, un boîtier de commande, une télécommande ou tout autre mode de saisie autre que par pointage.

### <a name="nested-ui-where-list-items-perform-an-action"></a>Interface utilisateur imbriquée dans laquelle les éléments de liste effectuent une action

Si votre interface utilisateur de liste comportant des éléments imbriqués prend en charge des actions telles que l’appel, la sélection (d’un ou de plusieurs éléments), ou des opérations de glisser-déplacer, nous vous recommandons ces techniques de navigation à l’aide des flèches pour vous déplacer dans vos éléments d’interface imbriquée.

![Parties de l’interface utilisateur imbriquée](images/nested-ui-navigation.png)

**Boîtier de commande**

Lorsque la saisie est effectuée à l’aide d’un boîtier de commande, proposez l’expérience utilisateur suivante:

- À partir de **A**, la touche directionnelle de droite met le focus sur l’élément **B**.
- À partir de **B**, la touche directionnelle de droite met le focus sur l’élément **C**.
- À partir de **C**, la touche droite directionnelle de droite n’est pas opérationnelle ou en présence d’un élément d’interface utilisateur sélectionnable à droite de la liste, met le focus sur celui-ci.
- À partir de **C**, la touche directionnelle de gauche met le focus sur l’élément **B**.
- À partir de **B**, la touche directionnelle de gauche met le focus sur l’élément **A**.
- À partir de **A**, la touche droite directionnelle de gauche n’est pas opérationnelle ou en présence d’un élément d’interface utilisateur sélectionnable à droite de la liste, met le focus sur celui-ci.
- À partir de **A**, **B**, ou **C**, la touche directionnelle vers le bas met le focus sur l’élément **D**.
- À partir de l’élément d’interface utilisateur situé à gauche de l’élément de liste, la touche directionnelle de droite met le focus sur l’élément **A**.
- À partir de l’élément d’interface utilisateur situé à droite de l’élément de liste, la touche directionnelle de gauche met le focus sur l’élément **A**.

**Clavier**

Lorsque la saisie s’effectue à partir d’un clavier, voici ce qu’obtient utilisateur:

- À partir de **A**, la touche TABULATION met le focus sur l’élément **B**.
- À partir de **B**, la touche TABULATION met le focus sur l’élément **C**.
- À partir de **C**, la touche TABULATION met le focus sur le prochain élément sélectionnable dans l’ordre de tabulation.
- À partir de **C**, la combinaison de touches Maj+Tab met le focus sur l’élément **B**.
- À partir de **B**, la combinaison de touches Maj+Tab ou flèche vers la gauche met le focus sur l’élément **A**.
- À partir de **A**, la combinaison de touches Maj+Tab met le focus sur le prochain élément d’interface utilisateur sélectionnable dans l’ordre inverse de tabulation.
- À partir de **A**, **B**, ou **C**, la flèche vers le bas met le focus sur l’élément **D**.
- À partir de l’élément d’interface utilisateur situé à gauche de l’élément de liste, la touche TABULATION met le focus sur l’élément **A**.
- À partir de l’élément d’interface utilisateur situé à droite de l’élément de liste, la touche TABULATION met le focus sur l’élément **C**.

Pour obtenir cette interface utilisateur, définissez [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) sur **true** sur votre liste. [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) peut être n’importe quelle valeur.

Pour obtenir le code permettant d’implémenter ceci, reportez-vous à la section [Exemple](#example) de cet article.

### <a name="nested-ui-where-list-items-do-not-perform-an-action"></a>Interface utilisateur imbriquée dans laquelle les éléments de liste n’effectuent aucune action

Vous pouvez utiliser un affichage Liste permettant la virtualisation et l’optimisation du comportement de défilement sans avoir d’action associée à un élément de liste. Ces interfaces utilisateur utilisent en règle générale l’élément de liste uniquement pour grouper des éléments et s’assurer qu’ils défilent comme prévu.

Ce type d’interface utilisateur a tendance à être beaucoup plus compliqué que les exemples précédents, dans la mesure où il comporte un grand nombre d’éléments imbriqués sur lesquels l’utilisateur peut agir.

![Parties de l’interface utilisateur imbriquée](images/nested-ui-grouping.png)


Pour terminer cette interface utilisateur, définissez les propriétés suivantes sur votre liste:
- [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) sur **Aucun**.
- [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) sur **false**.
- [IsFocusEngagementEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isfocusengagementenabled.aspx) sur **true**.

```xaml
<ListView SelectionMode="None" IsItemClickEnabled="False" >
    <ListView.ItemContainerStyle>
         <Style TargetType="ListViewItem">
             <Setter Property="IsFocusEngagementEnabled" Value="True"/>
         </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

Lorsque les éléments de liste n’effectuent aucune action, nous recommandons de procéder ainsi pour gérer la navigation avec un boîtier de commande ou un clavier.

**Boîtier de commande**

Lorsque la saisie est effectuée à l’aide d’un boîtier de commande, proposez l’expérience utilisateur suivante:

- À partir de l’élément de liste, touche directionnelle vers le bas met le focus sur l’élément de liste suivant.
- À partir de l’élément de liste, la touche gauche/droite n’est pas opérationnelle ou en présence d’un élément d’interface utilisateur sélectionnable à droite de la liste, met le focus sur celui-ci.
- À partir de l’élément de liste, le bouton «A» met le focus sur l’interface utilisateur imbriquée dans la priorité haut/bas gauche/droite.
- Depuis l’interface utilisateur imbriquée, suivez le modèle de navigation en mode focus XY.  Le système de focus permet de naviguer uniquement dans l’interface utilisateur imbriquée contenue au sein de l’élément de liste actuel jusqu’à ce que l’utilisateur appuie sur le bouton «B», ce qui a pour effet de remettre le focus sur l’élément de liste.

**Clavier**

Lorsque la saisie s’effectue à partir d’un clavier, voici ce qu’obtient utilisateur:

- À partir de l’élément de liste, la flèche vers le bas met le focus sur l’élément de liste suivant.
- À partir de l’élément de liste, il est impossible d’appuyer sur la touche gauche/droite.
- À partir de l’élément de liste, la touche TABULATION permet de mettre le focus sur chaque élément sélectionnable dans l’ordre de tabulation.
- À partir d’un des éléments de l’interface utilisateur imbriquée, la touche TABULATION permet de se déplacer dans l’ordre de tabulation.  Après avoir parcouru tous les éléments de l’interface utilisateur imbriquée, la commande suivante dans l’ordre de tabulation après ListView fait l’objet du focus.
- La combinaison de touches Maj + Tab permet un déplacement en sens inverse de l’ordre de tabulation.

## <a name="example"></a>Exemple

Cet exemple montre comment implémenter une [interface utilisateur imbriquée dans laquelle des éléments de liste effectuent une action](#nested-ui-where-list-items-perform-an-action).

```xaml
<ListView SelectionMode="None" IsItemClickEnabled="True"
          ChoosingItemContainer="listview1_ChoosingItemContainer"/>
```

```csharp
private void OnListViewItemKeyDown(object sender, KeyRoutedEventArgs e)
{
    // Code to handle going in/out of nested UI with gamepad and remote only.
    if (e.Handled == true)
    {
        return;
    }

    var focusedElementAsListViewItem = FocusManager.GetFocusedElement() as ListViewItem;
    if (focusedElementAsListViewItem != null)
    {
        // Focus is on the ListViewItem.
        // Go in with Right arrow.
        Control candidate = null;

        switch (e.OriginalKey)
        {
            case Windows.System.VirtualKey.GamepadDPadRight:
            case Windows.System.VirtualKey.GamepadLeftThumbstickRight:
                var rawPixelsPerViewPixel = DisplayInformation.GetForCurrentView().RawPixelsPerViewPixel;
                GeneralTransform generalTransform = focusedElementAsListViewItem.TransformToVisual(null);
                Point startPoint = generalTransform.TransformPoint(new Point(0, 0));
                Rect hintRect = new Rect(startPoint.X * rawPixelsPerViewPixel, startPoint.Y * rawPixelsPerViewPixel, 1, focusedElementAsListViewItem.ActualHeight * rawPixelsPerViewPixel);
                candidate = FocusManager.FindNextFocusableElement(FocusNavigationDirection.Right, hintRect) as Control;
                break;
        }

        if (candidate != null)
        {
            candidate.Focus(FocusState.Keyboard);
            e.Handled = true;
        }
    }
    else
    {
        // Focus is inside the ListViewItem.
        FocusNavigationDirection direction = FocusNavigationDirection.None;
        switch (e.OriginalKey)
        {
            case Windows.System.VirtualKey.GamepadDPadUp:
            case Windows.System.VirtualKey.GamepadLeftThumbstickUp:
                direction = FocusNavigationDirection.Up;
                break;
            case Windows.System.VirtualKey.GamepadDPadDown:
            case Windows.System.VirtualKey.GamepadLeftThumbstickDown:
                direction = FocusNavigationDirection.Down;
                break;
            case Windows.System.VirtualKey.GamepadDPadLeft:
            case Windows.System.VirtualKey.GamepadLeftThumbstickLeft:
                direction = FocusNavigationDirection.Left;
                break;
            case Windows.System.VirtualKey.GamepadDPadRight:
            case Windows.System.VirtualKey.GamepadLeftThumbstickRight:
                direction = FocusNavigationDirection.Right;
                break;
            default:
                break;
        }

        if (direction != FocusNavigationDirection.None)
        {
            Control candidate = FocusManager.FindNextFocusableElement(direction) as Control;
            if (candidate != null)
            {
                ListViewItem listViewItem = sender as ListViewItem;

                // If the next focusable candidate to the left is outside of ListViewItem,
                // put the focus on ListViewItem.
                if (direction == FocusNavigationDirection.Left &&
                    !listViewItem.IsAncestorOf(candidate))
                {
                    listViewItem.Focus(FocusState.Keyboard);
                }
                else
                {
                    candidate.Focus(FocusState.Keyboard);
                }
            }

            e.Handled = true;
        }
    }
}

private void listview1_ChoosingItemContainer(ListViewBase sender, ChoosingItemContainerEventArgs args)
{
    if (args.ItemContainer == null)
    {
        args.ItemContainer = new ListViewItem();
        args.ItemContainer.KeyDown += OnListViewItemKeyDown;
    }
}
```

```csharp
// DependencyObjectExtensions.cs definition.
public static class DependencyObjectExtensions
{
    public static bool IsAncestorOf(this DependencyObject parent, DependencyObject child)
    {
        DependencyObject current = child;
        bool isAncestor = false;

        while (current != null && !isAncestor)
        {
            if (current == parent)
            {
                isAncestor = true;
            }

            current = VisualTreeHelper.GetParent(current);
        }

        return isAncestor;
    }
}
```
