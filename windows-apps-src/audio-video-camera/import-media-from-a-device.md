---
ms.assetid: dd2a1e01-c284-4d62-963e-f59f58dca61a
description: Cet article vous décrit la procédure d’importation de fichiers multimédias à partir d’un appareil, notamment la recherche de sources de médias disponibles, l’importation de photos et de fichiers sidecar et la suppression des fichiers importés de l’appareil source.
title: Importer des fichiers multimédias
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c08612e48eec7989f3b56fba41a17e1c149b2058
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8748636"
---
# <a name="import-media-from-a-device"></a>Importer des fichiers multimédias à partir d’un appareil

Cet article vous décrit la procédure d’importation de fichiers multimédias à partir d’un appareil, notamment la recherche de sources de médias disponibles, l’importation de photos et de fichiers sidecar et la suppression des fichiers importés de l’appareil source.

> [!NOTE] 
> Le code figurant dans cet article a été adapté de l’[**exemple d’application MediaImport UWP**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport). Vous pouvez dupliquer ou télécharger cet exemple à partir du [**référentiel Git d’exemples d’applications Windows universelles**](https://github.com/Microsoft/Windows-universal-samples) afin d’afficher le code en contexte ou de l’utiliser en tant que point de départ pour votre propre application.

## <a name="create-a-simple-media-import-ui"></a>Créer une interface simple d’importation de médias
L’exemple de cet article utilise une interface utilisateur épurée prenant en charge les scénarios principaux d’importation de médias. Pour savoir comment créer une interface utilisateur plus robuste adaptée à une application d’importation de médias, consultez l’[**exemple MediaImport**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport). Le code XAML suivant crée un panneau d’empilement avec les contrôles suivants:
* Un objet [**Button**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Button) permettant de lancer la recherche des sources à partir desquelles les médias peuvent être importés.
* Un objet [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox) permettant de répertorier et d’effectuer des sélections à partir des sources d’importation de fichiers multimédias identifiées.
* Un objet [**ListView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ListView) permettant d’afficher et de sélectionner les éléments multimédias à partir de la source d’importation sélectionnée.
* Un élément **Button** permettant de lancer l’importation des éléments multimédias à partir de la source sélectionnée.
* Un élément **Button** permettant de lancer la suppression des éléments importés de la source sélectionnée.
* Un élément **Button** permettant d’annuler une opération d’importation asynchrone d’éléments multimédias.

[!code-xml[ImportXAML](./code/PhotoImport_Win10/cs/MainPage.xaml#SnippetImportXAML)]

## <a name="set-up-your-code-behind-file"></a>Configurer votre fichier code-behind
Ajoutez des directives *using* afin d’inclure les espaces de noms utilisés par cet exemple qui ne sont pas encore inclus dans le modèle de projet par défaut.

[!code-cs[Using](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="set-up-task-cancellation-for-media-import-operations"></a>Configurer l’annulation des tâches associées aux opérations d’importation des fichiers multimédias

Étant donné que les opérations d’importation multimédia peuvent prendre un certain temps, elles sont exécutées de manière asynchrone à l’aide de [**IAsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206594.aspx). Déclarez une variable de membre de classe de type [**CancellationTokenSource**](https://msdn.microsoft.com/library/system.threading.cancellationtokensource) à utiliser pour annuler une opération en cours de progression si l’utilisateur clique sur le bouton d’annulation.

[!code-cs[DeclareCts](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareCts)]

Implémentez un gestionnaire pour le bouton d’annulation. L’exemple figurant plus loin dans cet article est dédié à l’initialisation de l’instance **CancellationTokenSource** au démarrage d’une opération. La valeur de cette dernière est définie sur null à l’issue. Dans le gestionnaire du bouton d’annulation, vérifiez si le jeton est défini sur null et si ce n’est pas le cas, appelez [**Cancel**](https://msdn.microsoft.com/library/dd321955) afin d’annuler l’opération.

[!code-cs[OnCancel](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetOnCancel)]

## <a name="data-binding-helper-classes"></a>Classes d’assistance de liaison de données

Dans un scénario d’importation standard d’éléments multimédias, vous présentez à l’utilisateur une liste des éléments multimédias disponibles à l’importation. Le nombre de fichiers pouvant s’avérer conséquent, vous pouvez avoir intérêt à présenter une miniature de chaque élément multimédia. Pour cette raison, cet exemple utilise troisclasses d’assistance permettant de charger de manière incrémentielle des entrées dans le contrôle ListView, à mesure que l’utilisateur fait défiler la liste.

* Classe **IncrementalLoadingBase**: implémente les objets [**IList**](https://msdn.microsoft.com/library/system.collections.ilist), [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading) et [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/system.collections.specialized.inotifycollectionchanged(v=vs.105).aspx) afin de communiquer le comportement de base de chargement incrémentiel.
* Classe **GeneratorIncrementalLoadingClass**: fournit une implémentation de la classe de base de chargement incrémentiel.
* Classe **ImportableItemWrapper**: un wrapper fin autour de la classe [**PhotoImportItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem) afin d’ajouter une propriété [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.BitmapImage) pouvant être liée pour chacune des images miniatures associées aux éléments importés.

Ces classes sont fournies dans l’[**exemple MediaImport**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport); ils peuvent être ajoutées à votre projet sans modifications. Une fois les classes d’assistance ajoutées à votre projet, déclarez une variable de membre de classe de type **GeneratorIncrementalLoadingClass**, qui sera utilisé plus loin dans cet exemple.

[!code-cs[GeneratorIncrementalLoadingClass](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetGeneratorIncrementalLoadingClass)]


## <a name="find-available-sources-from-which-media-can-be-imported"></a>Rechercher les sources disponibles à partir desquelles importer les fichiers multimédias

Dans le gestionnaire de clics du bouton de recherche des sources, appelez la méthode statique [**PhotoImportManager.FindAllSourcesAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportManager.FindAllSourcesAsync) afin de démarrer sur le système la recherche des appareils à partir desquels importer des fichiers multimédias. Après avoir attendu l’exécution de l’opération, parcourez chaque objet [**PhotoImportSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource) de la liste renvoyée et ajoutez une entrée à l’instance **ComboBox**, en définissant la propriété **Tag** sur l’objet source, afin qu’il puisse être facilement récupéré lorsque l’utilisateur effectue une sélection.

[!code-cs[FindSourcesClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindSourcesClick)]

Déclarez une variable de membre de classe dédiée au stockage de la source d’importation sélectionnée de l’utilisateur.

[!code-cs[DeclareImportSource](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportSource)]

Dans le gestionnaire [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged) de la source d’importation **ComboBox**, définissez la variable de membre de classe sur la source sélectionnée, puis appelez la méthode d’assistance **FindItems**, décrite plus loin dans cet article. 

[!code-cs[SourcesSelectionChanged](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetSourcesSelectionChanged)]

## <a name="find-items-to-import"></a>Rechercher des éléments à importer

Ajoutez des variables de membre de classe de type [**PhotoImportSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSession) et [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult) à utiliser dans les étapes suivantes.

[!code-cs[DeclareImport](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImport)]

Dans la méthode **FindItems**, initialisez la variable **CancellationTokenSource** afin de pouvoir l’utiliser pour annuler l’opération de recherche, si nécessaire. Dans un bloc **try**, créez une nouvelle session d’importation en appelant [**CreateImportSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource.CreateImportSession) sur l’objet [**PhotoImportSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource) sélectionné par l’utilisateur. Créez un nouvel objet [**Progress**](https://msdn.microsoft.com/library/hh193692.aspx) afin de fournir un rappel prenant en charge l’affichage de l’avancement de l’opération de recherche. Ensuite, appelez **[FindItemsAsync](https://docs.microsoft.com/uwp/api/windows.media.import.photoimportsession.finditemsasync)** pour démarrer l’opération de recherche. Fournissez une valeur [**PhotoImportContentTypeFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportContentTypeFilter) afin de spécifier si des photos ou des vidéos (ou les deux) sont à renvoyer. Fournissez une valeur [**PhotoImportItemSelectionMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItemSelectionMode) afin de spécifier si l’ensemble des éléments multimédias, aucun d’entre eux ou uniquement les nouvelles instances sont renvoyés avec la propriété [**IsSelected**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem.IsSelected) définie sur true. Cette propriété est liée à une case à cocher pour chaque élément multimédia de notre modèle d’élément ListBox.

**FindItemsAsync** renvoie une valeur [**IAsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206594.aspx). La méthode d’extension [**AsTask**](https://msdn.microsoft.com/library/hh779750.aspx) est utilisée pour créer une tâche pouvant être attendue ou pouvant être annulée avec le jeton d’annulation. Elle fait état de l’avancement à l’aide de l’objet **Progress** fourni.

En regard de la classe d’assistance de liaison des données, la classe **GeneratorIncrementalLoadingClass** est initialisée. **FindItemsAsync**, lorsqu’il revient d’avoir été attendu, renvoie un objet [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult). Cet objet comporte les informations de statut sur l’opération de recherche, notamment sur sa réussite et le nombre des différents types d’éléments multimédias identifiés. La propriété [**FoundItems**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.FoundItems) contient une liste des objets [**PhotoImportItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem) représentant les éléments multimédias identifiés. Le constructeur **GeneratorIncrementalLoadingClass** prend en tant qu’arguments le nombre total d’éléments à charger de manière incrémentielle et une fonction qui génère de nouveaux éléments à charger au besoin. Dans ce cas, l’expression lambda fournie génère une nouvelle instance de l’objet **ImportableItemWrapper**, qui enveloppe **PhotoImportItem** et inclut une miniature pour chaque élément. Une fois que la classe de chargement incrémentielle a été initialisée, définissez-la sur la propriété [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ItemsControl.ItemsSource) du contrôle **ListView** de l’interface utilisateur. À présent, les éléments multimédias identifiés seront chargés de manière incrémentielle et chargés dans la liste.

Ensuite, les informations de statut de l’opération de recherche sont affichées. Une application standard affiche ces informations à l’utilisateur dans l’interface, mais cet exemple transmet simplement les informations à la console de débogage. Enfin, définissez le jeton d’annulation sur null, dans la mesure où l’opération est terminée.

[!code-cs[FindItems](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindItems)]

## <a name="import-media-items"></a>Importer des éléments multimédias

Avant d’implémenter l’opération d’importation, déclarez un objet [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult) dédié au stockage des résultats de l’opération d’importation. Il sera utilisé plus tard pour supprimer les éléments multimédias importés à partir de la source.

[!code-cs[DeclareImportResult](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportResult)]

Avant de démarrer l’opération d’importation d’éléments multimédias, initialisez la variable **CancellationTokenSource** en définissant la valeur du contrôle [**ProgressBar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ProgressBar) sur 0.

Si le contrôle **ListView** ne présente aucun élément sélectionné, l’importation ne peut pas avoir lieu. Sinon, initialisez un objet [**Progress**](https://msdn.microsoft.com/library/hh193692.aspx) afin de fournir un rappel d’avancement qui met à jour la valeur du contrôle de la barre d’avancement. Enregistrez un gestionnaire pour l’événement [**ItemImported**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ItemImported) de la classe  [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult) renvoyée par l’opération de recherche. Cet événement est déclenché chaque fois qu’un élément est importé et, dans cet exemple, génère le nom de chaque fichier importé sur la console de débogage.

Appelez [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync) afin de démarrer l’opération d’importation. Comme c’est le cas avec l’opération de recherche, la méthode d’extension [**AsTask**](https://msdn.microsoft.com/library/hh779750.aspx) est utilisée pour convertir l’opération renvoyée en tâche qui peut être attendue, annulée et qui consigne l’avancement.

À l’issue de l’opération d’importation, le statut d’opération peut être obtenu à partir de l’objet [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult) renvoyé par [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync). Cet exemple affiche les informations de statut sur la console de débogage puis, en définitive, définit le jeton d’annulation sur null.

[!code-cs[ImportClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetImportClick)]

## <a name="delete-imported-items"></a>Supprimer des éléments importés
Pour supprimer les éléments importés de la source à partir de laquelle ils ont été importés, initialisez le jeton d’annulation, de manière à ce que l’opération de suppression puisse être annulée et définissez la valeur de la barre d’avancement sur 0. Assurez-vous que l’instance [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult) renvoyée de [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync) n’est pas définie sur null. Si ce n’est pas le cas, créez de nouveau un objet [**Progress**](https://msdn.microsoft.com/library/hh193692.aspx) afin de fournir un rappel de progression associé à l’opération de suppression. Appelez la méthode [**DeleteImportedItemsFromSourceAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult.DeleteImportedItemsFromSourceAsync) afin de démarrer la suppression des éléments importés. Utilisez **AsTask** pour convertir le résultat en une tâche pouvant être attendue à l’aide de fonctionnalités de progression et d’annulation. Après l’attente, l’objet [**PhotoImportDeleteImportedItemsFromSourceResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportDeleteImportedItemsFromSourceResult) renvoyé peut être utilisé pour récupérer et afficher les informations de statut sur l’opération de suppression.

[!code-cs[DeleteClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeleteClick)]








 


