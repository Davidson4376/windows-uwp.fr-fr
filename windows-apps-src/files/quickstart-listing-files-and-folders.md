---
author: laurenhughes
ms.assetid: 4C59D5AC-58F7-4863-A884-E9E54228A5AD
title: "Énumérer et interroger des fichiers et dossiers"
description: "Accédez aux fichiers et dossiers dans un dossier, une bibliothèque, un appareil ou un emplacement réseau. Vous pouvez également interroger les fichiers et dossiers situés dans un emplacement en créant des requêtes de fichiers et de dossiers."
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 7da7002a95c107e64278e8bc67064542ed183d8b
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="enumerate-and-query-files-and-folders"></a>Énumérer et interroger des fichiers et dossiers


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Accédez aux fichiers et dossiers dans un dossier, une bibliothèque, un appareil ou un emplacement réseau. Vous pouvez également interroger les fichiers et dossiers d’un emplacement en créant des requêtes de fichiers et de dossiers.

Pour obtenir des instructions détaillées sur le stockage des données de votre application de plateforme Windows universelle, consultez la classe [ApplicationData](https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.aspx).

**Remarque** Consultez également l’[exemple d’énumération de dossier](http://go.microsoft.com/fwlink/p/?linkid=619993).

 
## <a name="prerequisites"></a>Prérequis

-   **Comprendre la programmation asynchrone pour les applications pour la plateforme Windows universelle (UWP)**

    Pour apprendre à écrire des applications asynchrones en C# ou Visual Basic, voir [Appeler des API asynchrones en C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Pour apprendre à écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Autorisations d’accès à l’emplacement**

    Par exemple, le code de ces exemples nécessite la fonctionnalité **picturesLibrary**, mais votre emplacement peut avoir besoin d’une autre fonctionnalité, voire d’aucune. Pour en savoir plus, voir [Autorisations d’accès aux fichiers](file-access-permissions.md).

## <a name="enumerate-files-and-folders-in-a-location"></a>Énumérer les fichiers et dossiers dans un emplacement

> **Remarque** N’oubliez pas de déclarer la fonctionnalité **picturesLibrary**.

Dans cet exemple, nous utilisons d’abord la méthode [**StorageFolder.GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br227276) pour obtenir tous les fichiers figurant dans le dossier racine de la propriété [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) (pas dans les sous-dossiers) et obtenir le nom de chaque fichier. Ensuite, nous utilisons la méthode [**GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br227280) pour obtenir tous les sous-dossiers dans la propriété **PicturesLibrary** et obtenir le nom de chaque sous-dossier.

<!--BUGBUG: IAsyncOperation<IVectorView<StorageFolder^>^>^  causes build to flake out-->
> [!div class="tabbedCodeSnippets"]
> ```cpp
> //#include <ppltasks.h>
> //#include <string>
> //#include <memory>
> using namespace Windows::Storage;
> using namespace Platform::Collections;
> using namespace concurrency;
> using namespace std;
>
> // Be sure to specify the Pictures Folder capability in the appxmanifext file.
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
>
> // Use a shared_ptr so that the string stays in memory
> // until the last task is complete.
> auto outputString = make_shared<wstring>();
> *outputString += L"Files:\n";
>
> // Get a read-only vector of the file objects
> // and pass it to the continuation.
> create_task(picturesFolder->GetFilesAsync())        
>    // outputString is captured by value, which creates a copy
>    // of the shared_ptr and increments its reference count.
>    .then ([outputString] (IVectorView\<StorageFile^>^ files)
>    {        
>        for ( unsigned int i = 0 ; i < files->Size; i++)
>        {
>            *outputString += files->GetAt(i)->Name->Data();
>            *outputString += L"\n";
>       }
>    })
>    // We need to explicitly state the return type
>    // here: -> IAsyncOperation<...>
>    .then([picturesFolder]() -> IAsyncOperation\<IVectorView\<StorageFolder^>^>^
>    {
>        return picturesFolder->GetFoldersAsync();
>    })
>    // Capture "this" to access m_OutputTextBlock from within the lambda.
>    .then([this, outputString](IVectorView\<StorageFolder^>^ folders)
>    {        
>        *outputString += L"Folders:\n";
>
>        for ( unsigned int i = 0; i < folders->Size; i++)
>        {
>           *outputString += folders->GetAt(i)->Name->Data();
>           *outputString += L"\n";
>        }
>
>        // Assume m_OutputTextBlock is a TextBlock defined in the XAML.
>        m_OutputTextBlock->Text = ref new String((*outputString).c_str());
>     });
> ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> StringBuilder outputText = new StringBuilder();
>
> IReadOnlyList<StorageFile> fileList =
>     await picturesFolder.GetFilesAsync();
>
> outputText.AppendLine("Files:");
> foreach (StorageFile file in fileList)
> {
>     outputText.Append(file.Name + "\n");
> }
>
> IReadOnlyList<StorageFolder> folderList =
>     await picturesFolder.GetFoldersAsync();
>            
> outputText.AppendLine("Folders:");
> foreach (StorageFolder folder in folderList)
> {
>     outputText.Append(folder.DisplayName + "\n");
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
>
> Dim fileList As IReadOnlyList(Of StorageFile) =
>     Await picturesFolder.GetFilesAsync()
>
> outputText.AppendLine("Files:")
> For Each file As StorageFile In fileList
>
>     outputText.Append(file.Name & vbLf)
>
> Next file
>
> Dim folderList As IReadOnlyList(Of StorageFolder) =
>     Await picturesFolder.GetFoldersAsync()
>
> outputText.AppendLine("Folders:")
> For Each folder As StorageFolder In folderList
>
>     outputText.Append(folder.DisplayName & vbLf)
>
> Next folder
> ```


> **Remarque** EnC# ou VisualBasic, n’oubliez pas de placer le mot-clé **async** dans la déclaration de toutes les méthodes dans lesquelles vous utilisez l’opérateur **await**.
 

Vous pouvez aussi utiliser la méthode [**GetItemsAsync**](https://msdn.microsoft.com/library/windows/apps/br227286) pour obtenir tous les éléments (fichiers et sous-dossiers) figurant dans un emplacement particulier. L’exemple suivant utilise la méthode **GetItemsAsync** pour obtenir tous les fichiers et sous-dossiers figurant dans le dossier racine de la propriété [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) (pas dans les sous-dossiers). L’exemple affiche ensuite le nom de chaque fichier et sous-dossier. Si l’élément est un sous-dossier, l’exemple ajoute `"folder"` au nom.

> [!div class="tabbedCodeSnippets"]
> ```cpp
> // See previous example for comments, namespace and #include info.
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
> auto outputString = make_shared<wstring>();
>
> create_task(picturesFolder->GetItemsAsync())        
>     .then ([this, outputString] (IVectorView<IStorageItem^>^ items)
> {        
>     for ( unsigned int i = 0 ; i < items->Size; i++)
>     {
>         *outputString += items->GetAt(i)->Name->Data();
>         if(items->GetAt(i)->IsOfType(StorageItemTypes::Folder))
>         {
>             *outputString += L"  folder\n";
>         }
>         else
>         {
>             *outputString += L"\n";
>         }
>         m_OutputTextBlock->Text = ref new String((*outputString).c_str());
>     }
> });
> ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> StringBuilder outputText = new StringBuilder();
>
> IReadOnlyList<IStorageItem> itemsList =
>     await picturesFolder.GetItemsAsync();
>
> foreach (var item in itemsList)
> {
>     if (item is StorageFolder)
>     {
>         outputText.Append(item.Name + " folder\n");
>
>     }
>     else
>     {
>         outputText.Append(item.Name + "\n");
>
>     }
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
>
> Dim itemsList As IReadOnlyList(Of IStorageItem) =
>     Await picturesFolder.GetItemsAsync()
>
> For Each item In itemsList
>
>     If TypeOf item Is StorageFolder Then
>
>         outputText.Append(item.Name & " folder" & vbLf)
>
>     Else
>
>         outputText.Append(item.Name & vbLf)
>
>     End If
>
> Next item
> ```

## <a name="query-files-in-a-location-and-enumerate-matching-files"></a>Interroger les fichiers figurant dans un emplacement et énumérer les fichiers correspondants

Dans cet exemple, nous interrogeons tous les fichiers figurant dans la propriété [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156), regroupés en fonction du mois, et cette fois, l’exemple recherche dans les sous-dossiers. Tout d’abord, nous appelons la méthode [**StorageFolder.CreateFolderQuery**](https://msdn.microsoft.com/library/windows/apps/br227262) et transmettons la valeur [**CommonFolderQuery.GroupByMonth**](https://msdn.microsoft.com/library/windows/apps/br207957) à la méthode. Nous obtenons ainsi un objet [**StorageFolderQueryResult**](https://msdn.microsoft.com/library/windows/apps/br208066).

Ensuite, nous appelons la méthode [**StorageFolderQueryResult.GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br208074) qui retourne des objets [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) représentant des dossiers virtuels. Dans ce cas, nous regroupons par mois, de sorte que chaque dossier virtuel représente un groupe de fichiers du même mois.

> [!div class="tabbedCodeSnippets"]
> ```cpp
> //#include <ppltasks.h>
> //#include <string>
> //#include <memory>
> using namespace Windows::Storage;
> using namespace Windows::Storage::Search;
> using namespace concurrency;
> using namespace Platform::Collections;
> using namespace Windows::Foundation::Collections;
> using namespace std;
>
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
>
> StorageFolderQueryResult^ queryResult =
>     picturesFolder->CreateFolderQuery(CommonFolderQuery::GroupByMonth);
>
> // Use shared_ptr so that outputString remains in memory
> // until the task completes, which is after the function goes out of scope.
> auto outputString = std::make_shared<wstring>();
>
> create_task( queryResult->GetFoldersAsync()).then([this, outputString] (IVectorView<StorageFolder^>^ view)
> {        
>     for ( unsigned int i = 0; i < view->Size; i++)
>     {
>         create_task(view->GetAt(i)->GetFilesAsync()).then([this, i, view, outputString](IVectorView<StorageFile^>^ files)
>         {
>             *outputString += view->GetAt(i)->Name->Data();
>             *outputString += L"(";
>             *outputString += to_wstring(files->Size);
>             *outputString += L")\r\n";
>             for (unsigned int j = 0; j < files->Size; j++)
>             {
>                 *outputString += L"     ";
>                 *outputString += files->GetAt(j)->Name->Data();
>                 *outputString += L"\r\n";
>             }
>         }).then([this, outputString]()
>         {
>             m_OutputTextBlock->Text = ref new String((*outputString).c_str());
>         });
>     }    
> });
> ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
>
> StorageFolderQueryResult queryResult =
>     picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth);
>         
> IReadOnlyList<StorageFolder> folderList =
>     await queryResult.GetFoldersAsync();
>
> StringBuilder outputText = new StringBuilder();
>
> foreach (StorageFolder folder in folderList)
> {
>     IReadOnlyList<StorageFile> fileList = await folder.GetFilesAsync();
>
>     // Print the month and number of files in this group.
>     outputText.AppendLine(folder.Name + " (" + fileList.Count + ")");
>
>     foreach (StorageFile file in fileList)
>     {
>         // Print the name of the file.
>         outputText.AppendLine("   " + file.Name);
>     }
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
>
> Dim queryResult As StorageFolderQueryResult =
>     picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth)
>
> Dim folderList As IReadOnlyList(Of StorageFolder) =
>     Await queryResult.GetFoldersAsync()
>
> For Each folder As StorageFolder In folderList
>
>     Dim fileList As IReadOnlyList(Of StorageFile) =
>         Await folder.GetFilesAsync()
>
>     ' Print the month and number of files in this group.
>     outputText.AppendLine(folder.Name & " (" & fileList.Count & ")")
>
>     For Each file As StorageFile In fileList
>
>         ' Print the name of the file.
>         outputText.AppendLine("   " & file.Name)
>
>     Next file
>
> Next folder
> ```

La sortie de l’exemple ressemble à ce qui suit.

``` syntax
July ‎2015 (2)
   MyImage3.png
   MyImage4.png
‎December ‎2014 (2)
   MyImage1.png
   MyImage2.png
```
