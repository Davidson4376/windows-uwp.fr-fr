---
Description: Use an inverted list to add new items at the bottom.
title: Listes inversées
label: Inverted lists
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: 52c1d63d-69c1-48d6-a234-6f39296e4bfd
pm-contact: predavid
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 12f86c0d4f8980cea375b9a0a8a6876510c795b0
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8881752"
---
# <a name="inverted-lists"></a>Listes inversées

 

Vous pouvez utiliser un affichage de liste pour présenter une conversation dans une expérience de chat avec des éléments visuellement distincts représentant l’expéditeur/le destinataire.  L’utilisation de couleurs différentes et de l’alignement horizontal pour distinguer les messages de l’expéditeur/du destinataire aide l’utilisateur à se repérer facilement dans une conversation.

> **API importantes**: [classe ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), [classe ItemsStackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx), [propriété ItemsUpdatingScrollMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode.aspx)
 
En règle générale, vous définirez la liste selon une configuration ascendante, non descendante.  Lorsqu’un nouveau message arrive et est ajouté en fin de conversation, les messages précédents sont glissés afin de laisser la place et d’attirer l’attention de l’utilisateur sur la dernière entrée.  Toutefois, si un utilisateur a fait défilé le contenu pour afficher des réponses précédentes, l’arrivée d’un nouveau message ne peut pas provoquer de perturbation visuelle susceptible de détourner l’attention.

![Application de conversation avec liste inversée](images/listview-inverted.png)

## <a name="create-an-inverted-list"></a>Créer une liste inversée

Pour créer une liste inversée, utilisez un affichage de liste avec un objet [ItemsStackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx) en tant que panneau d’éléments. Sur l’objet ItemsStackPanel, définissez [ItemsUpdatingScrollMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode.aspx) sur [KeepLastItemInView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsupdatingscrollmode.aspx).

> [!IMPORTANT]
> La valeur d’énumération **KeepLastItemInView** est disponible à partir de Windows10, version1607. Vous ne pouvez pas utiliser cette valeur quand votre application s’exécute sur des versions antérieures de Windows10.

Cet exemple montre comment aligner les éléments de l’affichage de liste sur la partie inférieure et indiquer que le dernier élément doit rester affiché en cas de modification des éléments.
 
 **XAML**
 ```xaml
<ListView>
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsStackPanel VerticalAlignment="Bottom"
                             ItemsUpdatingScrollMode="KeepLastItemInView"/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
</ListView>
```

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

- Alignez les messages de l’expéditeur/du destinataire sur des côtés opposés afin de clarifier le flux de conversation pour les utilisateurs.
- Animez les messages existants à l’écart afin d’afficher le dernier message si l’utilisateur est déjà positionné en fin de conversation en attente du message suivant.
- S’il n’est pas positionné en fin de conversation, ne perturbez pas l’utilisateur en déplaçant des éléments.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de liste XAML ascendante](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBottomUpList)