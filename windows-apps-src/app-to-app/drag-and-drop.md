---
description: Cet article vous explique comment ajouter le glisser-déplacer dans votre application UWP.
title: Glisser-déplacer
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
---
# Glisser-déplacer

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Cet article vous explique comment ajouter le glisser-déplacer dans votre application UWP. Glisser-déplacer est une méthode naturelle classique d’interaction avec le contenu comme les images et les fichiers. Une fois implémenté, le glisser-déplacer fonctionne parfaitement dans toutes les directions, notamment d’application à application, d’application à bureau et de bureau à application.

## Définir des zones valides

Utilisez les propriétés [**AllowDrop**][AllowDrop] et [**CanDrag**][CanDrag] pour désigner les zones de votre application qui sont valides pour le glisser-déplacer.

Le balisage ci-dessous montre comment définir une zone spécifique de l’application valide pour l’opération Déplacer à l’aide de l’élément [**AllowDrop**][AllowDrop] dans XAML. Si un utilisateur tente de déplacer vers un autre emplacement, le système l’en empêche. Si vous souhaitez que les utilisateurs puissent déplacer des éléments n’importe où dans votre application, définissez l’ensemble de l’arrière-plan en tant que cible de l’opération Déplacer.

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDropArea)]

Pour le glissement, vous devrez généralement être spécifique sur le contenu pouvant être glissé. Les utilisateurs souhaitent faire glisser certains éléments, comme les images, et non la totalité du contenu de l’application. Voici comment configurer l’élément [**CanDrag**][CanDrag] à l’aide de XAML.

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDragArea)]

Vous n’avez besoin d’effectuer aucune autre action pour autoriser le glissement, sauf si vous souhaitez personnaliser l’interface utilisateur (le sujet est abordé plus loin dans cet article). L’opération Déplacer nécessite quelques étapes supplémentaires.

## Gérer l’événement DragOver

L’événement [**DragOver**][DragOver] se déclenche lorsqu’un utilisateur a fait glisser un élément sur votre application, mais qu’il ne l’a pas encore déplacé. Dans ce gestionnaire, vous devez spécifier le type d’opérations que votre application prend en charge à l’aide de la propriété [**DragEventArgs.AcceptedOperation**][AcceptedOperation]. L’opération Copier est la plus courante.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOver)]

## Traiter l’événement Déplacer

L’événement [**Drop**][Drop] se produit lorsque l’utilisateur relâche des éléments dans une zone de dépôt valide. Traitez-les à l’aide de la propriété [**DragEventArgs.DataView**][DataView].

Pour simplifier l’exemple ci-dessous, supposons que l’utilisateur a déplacé une seule photo et un accès. En réalité, les utilisateurs peuvent déplacer plusieurs éléments de formats divers simultanément. Votre application doit gérer cette possibilité en vérifiant les types de fichiers qui ont été déplacés, en les traitant en conséquence, et en notifiant l’utilisateur s’il essaie d’effectuer une action non prise en charge.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_Drop)]

## Personnaliser l’interface utilisateur

Le système fournit une interface utilisateur par défaut pour le glisser-déposer. Toutefois, vous pouvez également choisir de personnaliser les différentes parties de l’interface utilisateur en définissant des légendes et des glyphes personnalisés, ou en choisissant de ne pas afficher d’interface utilisateur du tout. Pour personnaliser l’interface utilisateur, utilisez la propriété [**DragUIOverride**][DragUiOverride] dans le gestionnaire d’événements [**DragOver**][DragOver].

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOverCustom)]

 <!-- LINKS -->
[AllowDrop]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx
[CanDrag]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx
[DragOver]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx
[AcceptedOperation]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.drageventargs.acceptedoperation.aspx
[DataView]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.drageventargs.dataview.aspx
[DragUiOverride]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.drageventargs.draguioverride.aspx
[Drop]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx 

<!--HONumber=Mar16_HO1-->
