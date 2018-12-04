---
description: Cet article vous explique comment ajouter le glisser-déplacer dans votre application UWP.
title: Glisser-déplacer
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e508feb8a530f29b40d5a3839df573cb2ce89896
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8482301"
---
# <a name="drag-and-drop"></a>Glisser-déplacer

L’opération glisser-déplacer constitue un moyen intuitif pour transférer des données dans une application ou entre des applications sur le bureau Windows. Glisser-déplacer permet à l’utilisateur de transférer des données entre des applications ou au sein d’une application à l’aide d’un mouvement standard (appui prolongé et mouvement panoramique avec le doigt ou appui et mouvement panoramique avec une souris ou un stylet).

> **API importantes**: [propriété CanDrag](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag), [propriété AllowDrop](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) 

La source de l’opération glisser, qui est l’application ou la zone où le mouvement de glissement est déclenché, fournit les données qui doivent être transférées en remplissant un objet package de données pouvant contenir des formats de données standard, y compris les formats texte, RTF, HTML, bitmaps, éléments de stockage ou données personnalisées. La source indique également le type d’opérations pris en charge: copier, déplacer ou lier. Lorsque le pointeur est relâché, l’élément est déposé. La cible du dépôt, qui est l’application ou la zone située sous le pointeur, traite le package de données et retourne le type d’opération effectué.

Au cours de l’opération glisser-déplacer, l’interface utilisateur de glissement fournit une indication visuelle du type d’opération glisser-déplacer qui est en cours. Ce retour visuel est initialement fourni par la source, mais peut être modifié par les cibles lorsque le pointeur se déplace sur celles-ci.

L’opération glisser-déplacer moderne est disponible sur tous les appareils qui prennent en charge UWP. Elle permet le transfert de données entre ou dans n’importe quel type d’application, y compris les applications Windows classiques, bien que cet article se concentre sur l’API XAML pour l’opération glisser-déplacer moderne. Une fois implémenté, le glisser-déplacer fonctionne parfaitement dans toutes les directions, notamment d’application à application, d’application à bureau et de bureau à application.

Voici une vue d’ensemble de ce que vous devez faire pour activer l’opération glisser-déplacer dans votre application:

1. Activez l’opération Glisser sur un élément en définissant sa propriété **CanDrag** sur true.  
2. Générez le package de données. Le système gère automatiquement les images et le texte, mais pour d’autres contenus, vous devrez gérer les événements **DragStarted** et **DragCompleted**, et les utiliser pour créer votre propre package de données. 
3. Activez l’opération Déplacer en définissant la propriété **AllowDrop** sur **true** pour tous les éléments qui peuvent recevoir du contenu déplacé. 
4. Gérez l’événement **DragOver** pour informer le système du type d’opérations Glisser que l’élément peut recevoir. 
5. Traitez l’événement **Drop** pour recevoir le contenu déplacé. 



## <a name="enable-dragging"></a>Activer l’opération Glisser

Pour activer l’opération Glisser sur un élément, définissez sa propriété [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag) sur **true**. Cela permet de faire glisser l’élément (et les éléments qu’il contient, dans le cas des collections telles que ListView).

Définissez avec précision les éléments qui peuvent être glissés. Les utilisateurs ne souhaiteront pas faire glisser tous les éléments dans votre application, seulement certains éléments, comme les images ou le texte. 

Voici comment définir [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag).

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDragArea)]

Vous n’avez besoin d’effectuer aucune autre action pour autoriser le glissement, sauf si vous souhaitez personnaliser l’interface utilisateur (le sujet est abordé plus loin dans cet article). L’opération Déplacer nécessite quelques étapes supplémentaires.

## <a name="construct-a-data-package"></a>Créer un package de données 

Dans la plupart des cas, le système va créer un package de données pour vous. Le système gère automatiquement:
* les images,
* le texte. 

Pour tout autre contenu, vous devrez gérer les événements **DragStarted** et **DragCompleted**, et les utiliser pour créer votre propre [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage).

## <a name="enable-dropping"></a>Activer l’opération Déplacer

Le balisage suivant montre comment définir une zone spécifique de l’application comme valide pour l’opération Déplacer à l’aide de l’élément [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) en XAML. Si un utilisateur tente d’effectuer le déplacement vers un autre emplacement, le système l’en empêche. Si vous souhaitez que les utilisateurs puissent déplacer des éléments n’importe où dans votre application, définissez l’ensemble de l’arrière-plan en tant que cible de l’opération Déplacer.

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDropArea)]


## <a name="handle-the-dragover-event"></a>Gérer l’événement DragOver

L’événement [**DragOver**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DragOver) se déclenche quand un utilisateur a fait glisser un élément sur votre application, mais qu’il ne l’a pas encore déplacé. Dans ce gestionnaire, vous devez spécifier le type d’opérations que votre application prend en charge à l’aide de la propriété [**AcceptedOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.AcceptedOperation). L’opération Copier est la plus courante.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOver)]

## <a name="process-the-drop-event"></a>Traiter l’événement Drop

L’événement [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) se produit quand l’utilisateur relâche des éléments dans une zone de dépôt valide. Traitez-les à l’aide de la propriété [**DataView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DataView).

Pour simplifier, dans l’exemple ci-dessous, nous supposons que l’utilisateur a déplacé une seule photo et y accède directement. En réalité, les utilisateurs peuvent déplacer plusieurs éléments de formats divers simultanément. Votre application doit gérer cette possibilité en vérifiant les types de fichiers déplacés et leur nombre, et traiter chacun en conséquence. Vous devez également envisager d’informer l’utilisateur s’il tente de réaliser une action non prise en charge par votre application.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_Drop)]

## <a name="customize-the-ui"></a>Personnaliser l’interface utilisateur

Le système fournit une interface utilisateur par défaut pour le glisser-déposer. Toutefois, vous pouvez également choisir de personnaliser les différentes parties de l’interface utilisateur en définissant des légendes et des glyphes personnalisés, ou en choisissant de ne pas afficher d’interface utilisateur du tout. Pour personnaliser l’interface utilisateur, utilisez la propriété [**DragEventArgs.DragUIOverride**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DragUIOverride).

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOverCustom)]

## <a name="open-a-context-menu-on-an-item-you-can-drag-with-touch"></a>Ouvrir un menu contextuel sur un élément que vous pouvez faire glisser avec une interface tactile

Quand vous utilisez une interface tactile, pour faire glisser un élément [**UIElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement) et ouvrir son menu contextuel, vous utilisez des mouvements tactiles similaires qui commencent tous deux par un appui prolongé. Voici comment le système lève l’ambiguïté entre les deux actions pour les éléments de votre application qui prennent en charge les deux opérations: 

* Si un utilisateur appuie de façon prolongée sur un élément et commence à le faire glisser dans un intervalle de 500millisecondes, l’élément est déplacé et le menu contextuel n’est pas affiché. 
* Si l’utilisateur appuie de façon prolongée sur l’élément, mais ne le fait pas glisser dans l’intervalle de 500millisecondes, le menu contextuel est ouvert. 
* Une fois que le menu contextuel est ouvert, si l’utilisateur essaie de faire glisser l’élément (sans lever le doigt), le menu contextuel se ferme et le déplacement commence.

## <a name="designate-an-item-in-a-listview-or-gridview-as-a-folder"></a>Désigner un élément dans un contrôle ListView ou GridView en tant que dossier

Vous pouvez spécifier un contrôle [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ListViewItem) ou [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.GridViewItem) en tant que dossier. Ceci est particulièrement utile dans les scénarios TreeView et Explorateur de fichiers. Pour ce faire, définissez explicitement la propriété [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) sur **True** pour l’élément concerné. 

Le système montre automatiquement les animations appropriées pour le déplacement dans un dossier plutôt que dans un élément autre qu’un dossier. Le code de votre application doit continuer à gérer l’événement [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) sur l’élément de dossier (ainsi que sur l’élément autre qu’un dossier) afin de mettre à jour la source de données et d’ajouter l’élément déplacé dans le dossier cible.

## <a name="implementing-custom-drag-and-drop"></a>Implémentation d'’une opération glisser-déplacer personnalisée

La classe [UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) effectue la plus grande partie de l’implémentation de l’opération glisser-déplacer pour vous. Toutefois, si vous le souhaitez, vous pouvez implémenter votre propre version à l’aide de l’API dans l' [espace de noms Windows.ApplicationModel.DataTransfer.DragDrop.Core](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core).

| Fonctionnalités | API WinRT |
| --- | --- |
|  Activer l’opération Glisser | [CoreDragOperation](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
|  Créer un package de données | [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage)  |
| Transmettre l’opération Glisser au shell  | [CoreDragOperation.StartAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
| Recevoir une opération Déplacer depuis le shell  | [CoreDragDropManager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragdropmanager)<br/>[ICoreDropOperationTarget](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.icoredropoperationtarget)    |



## <a name="see-also"></a>Articles associés

* [Communication entre les applications](index.md)
* [AllowDrop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx)
* [CanDrag](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx)
* [DragOver](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx)
* [AcceptedOperation](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.acceptedoperation.aspx)
* [DataView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.dataview.aspx)
* [DragUIOverride](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.draguioverride.aspx)
* [Drop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx)
* [IsDragSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isdragsource.aspx)
