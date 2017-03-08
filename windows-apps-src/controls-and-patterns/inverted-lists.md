---
author: Jwmsft
Description: "Utilisez une liste inversée pour ajouter de nouveaux éléments dans la partie inférieure."
title: "Listes inversées"
label: Inverted lists
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: 52c1d63d-69c1-48d6-a234-6f39296e4bfd
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 0a33aaf71dbf23e991591f790f7327d812b060ef
ms.lasthandoff: 02/08/2017

---
# <a name="inverted-lists"></a>Listes inversées

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Vous pouvez utiliser un affichage de liste pour présenter une conversation dans une expérience de chat avec des éléments visuellement distincts représentant l’expéditeur/le destinataire.  L’utilisation de couleurs différentes et de l’alignement horizontal pour distinguer les messages de l’expéditeur/du destinataire aide l’utilisateur à se repérer facilement dans une conversation.
 
En règle générale, vous définirez la liste selon une configuration ascendante, non descendante.  Lorsqu’un nouveau message arrive et est ajouté en fin de conversation, les messages précédents sont glissés afin de laisser la place et d’attirer l’attention de l’utilisateur sur la dernière entrée.  Toutefois, si un utilisateur a fait défilé le contenu pour afficher des réponses précédentes, l’arrivée d’un nouveau message ne peut pas provoquer de perturbation visuelle susceptible de détourner l’attention.

![application de conversation avec liste inversée](images/listview-inverted.png)

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Classe ListView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)</li>
<li>[**Classe ItemsStackPanel**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx)</li>
<li>[**Propriété ItemsUpdatingScrollMode**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode.aspx)</li>
</ul>
</div>


## <a name="create-an-inverted-list"></a>Créer une liste inversée

Pour créer une liste inversée, utilisez un affichage de liste avec un objet [**ItemsStackPanel**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx) en tant que panneau d’éléments. Sur l’objet ItemsStackPanel, définissez [**ItemsUpdatingScrollMode**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode.aspx) sur [**KeepLastItemInView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsupdatingscrollmode.aspx).

> [!IMPORTANT]
> La valeur d’énumération **KeepLastItemInView** est disponible à partir de Windows 10, version 1607. Vous ne pouvez pas utiliser cette valeur quand votre application s’exécute sur des versions antérieures de Windows 10.

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

