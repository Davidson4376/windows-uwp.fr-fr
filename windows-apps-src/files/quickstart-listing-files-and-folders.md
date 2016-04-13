---
ms.assetid: 4C59D5AC-58F7-4863-A884-E9E54228A5AD
title: Énumérer et interroger des fichiers et dossiers
description: Accédez aux fichiers et dossiers dans un dossier, une bibliothèque, un appareil ou un emplacement réseau. Vous pouvez également interroger les fichiers et dossiers situés dans un emplacement en créant des requêtes de fichiers et de dossiers.
---
# Énumérer et interroger des fichiers et dossiers


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Accédez aux fichiers et dossiers dans un dossier, une bibliothèque, un appareil ou un emplacement réseau. Vous pouvez également interroger les fichiers et dossiers situés dans un emplacement en créant des requêtes de fichiers et de dossiers.

**Remarque** Consultez aussi l’[exemple d’énumération de dossier](http://go.microsoft.com/fwlink/p/?linkid=619993).

 
## Prérequis

-   **Comprendre la programmation asynchrone pour les applications pour la plateforme Windows universelle (UWP)**

    Pour apprendre à écrire des applications asynchrones en C# ou Visual Basic, voir [Appeler des API asynchrones en C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Pour apprendre à écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Autorisations d’accès à l’emplacement**

    Par exemple, le code de ces exemples nécessite la fonctionnalité **picturesLibrary**, mais votre emplacement peut avoir besoin d’une autre fonctionnalité, voire d’aucune. Pour en savoir plus, voir [Autorisations d’accès aux fichiers](file-access-permissions.md).

## Énumérer les fichiers et dossiers dans un emplacement

> **Remarque** N’oubliez pas de déclarer la fonctionnalité **picturesLibrary**.

Dans cet exemple, nous utilisons d’abord la méthode [**StorageFolder.GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br227276) pour obtenir tous les fichiers figurant dans le dossier racine de la propriété [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) (pas dans les sous-dossiers) et obtenir le nom de chaque fichier. Ensuite, nous utilisons la méthode [**GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br227280) pour obtenir tous les sous-dossiers dans la propriété **PicturesLibrary** et obtenir le nom de chaque sous-dossier.

<!--BUGBUG: IAsyncOperation<IVectorView<StorageFolder^>^>^  causes build to flake out-->
> [!div class="tabbedCodeSnippets"] > ```cpp
> //#include <ppltasks.h>
> //#include <string>
> //#include <memory>
> using namespace Windows::Storage;
> using namespace Platform::Collections; > using namespace concurrency; > using namespace std; > > // Assurez-vous de spécifier la fonctionnalité Dossier Images dans le fichier appxmanifext.
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary; > > // Utilisez un shared_ptr de sorte que la chaîne reste en mémoire
> // jusqu’à ce que la dernière tâche soit effectuéee.
> auto outputString = make_shared<wstring>(); > *outputString += L"Files:\n"; > > // Obtenez un vecteur en lecture seule des objets de fichier
> // et transmettez-le à la continuationn. 
> create_task(picturesFolder->GetFilesAsync())        
> // outputString est capturée selon la valeur, ce qui crée a copy > // du shared_ptr et incrémente son décomptee coude référence.
> .then ([outputString] (IVectorView\<StorageFile^>^ files) > {        
> for ( unsigned int i = 0 ; i < files->Size; i++) > { > *outputString += files->GetAt(i)->Name->Data(); > *outputString += L"\n"; > } > })
> // Nous devons indiquer explicitement le type de retourpe 
> // ici : -> IAsyncOperation<...>
>     .then([picturesFolder]() -> IAsyncOperation\<IVectorView\<StorageFolder^>^>^ > {
> return picturesFolder->GetFoldersAsync();
> }) > // Capturez « this » pour accéder à m_OutputTextBlock à partir de la fonction lambdaambda.
> .then([this, outputString](IVectorView\<StorageFolder^>^ folders) > {        
> *outputString += L"Folders:\n"; > > for ( unsigned int i = 0; i < folders->Size; i++) > { > *outputString += folders->GetAt(i)->Name->Data(); > *outputString += L"\n"; > } > > // Considérez m_OutputTextBlock comme un TextBlock défini dans le code XAML.
> m_OutputTextBlock->Text = ref new String((*outputString).c_str()); > }); > ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> StringBuilder outputText = new StringBuilder();
> 
> IReadOnlyList<StorageFile> fileList = > await picturesFolder.GetFilesAsync(); > > outputText.AppendLine("Files:");
> foreach (StorageFile file in fileList)
> { > outputText.Append(file.Name + "\n"); > } > > IReadOnlyList<StorageFolder> folderList = > await picturesFolder.GetFoldersAsync(); > > outputText.AppendLine("Folders:");
> foreach (StorageFolder folder in folderList)
> { > outputText.Append(folder.DisplayName + "\n");
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


> **Remarque** En C# ou Visual Basic, n’oubliez pas de placer le mot clé **async** dans la déclaration de méthode de toutes les méthodes dans lesquelles vous utilisez l’opérateur **await**.
 

Vous pouvez aussi utiliser la méthode [**GetItemsAsync**](https://msdn.microsoft.com/library/windows/apps/br227286) pour obtenir tous les éléments (fichiers et sous-dossiers) figurant dans un emplacement particulier. L’exemple suivant utilise la méthode **GetItemsAsync** pour obtenir tous les fichiers et sous-dossiers figurant dans le dossier racine de la propriété [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) (pas dans les sous-dossiers). L’exemple affiche ensuite le nom de chaque fichier et sous-dossier. Si l’élément est un sous-dossier, l’exemple ajoute `"folder"` au nom.

> [!div class="tabbedCodeSnippets"] > ```cpp
> // See previous example for comments, namespace and #include info.
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
> auto outputString = make_shared<wstring>(); > > create_task(picturesFolder->GetItemsAsync())        
> .then ([this, outputString] (IVectorView<IStorageItem^>^ items) > {        
> for ( unsigned int i = 0 ; i < items->Size; i++) > { > *outputString += items->GetAt(i)->Name->Data(); > if(items->GetAt(i)->IsOfType(StorageItemTypes::Folder)) > { > *outputString += L" folder\n"; > } > else > { > *outputString += L"\n"; > } > m_OutputTextBlock->Text = ref new String((*outputString).c_str()); > } > }); > ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> StringBuilder outputText = new StringBuilder();
> 
> IReadOnlyList<IStorageItem> itemsList = > await picturesFolder.GetItemsAsync(); > > foreach (var item in itemsList)
> {
> if (item is StorageFolder) >     {
> outputText.Append(item.Name + " folder\n"); > > } > else > { > outputText.Append(item.Name + "\n"); > > } > } > ``` > ```vb > Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary > Dim outputText As New StringBuilder > > Dim itemsList As IReadOnlyList(Of IStorageItem) = > Await picturesFolder.GetItemsAsync() > > For Each item In itemsList > > If TypeOf item Is StorageFolder Then > > outputText.Append(item.Name & " folder" & vbLf) > > Else > > outputText.Append(item.Name & vbLf) > > End If > > Next item > ``` ## Interroger les fichiers situés dans un emplacement et énumérer les fichiers correspondants Dans cet exemple, nous interrogeons tous les fichiers figurant dans la propriété [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156), regroupés en fonction du mois, et cette fois, l’exemple recherche dans les sous-dossiers. Tout d’abord, nous appelons la méthode [**StorageFolder.CreateFolderQuery**](https://msdn.microsoft.com/library/windows/apps/br227262) et transmettons la valeur [**CommonFolderQuery.GroupByMonth**](https://msdn.microsoft.com/library/windows/apps/br207957) à la méthode. Nous obtenons ainsi un objet [**StorageFolderQueryResult**](https://msdn.microsoft.com/library/windows/apps/br208066).

Ensuite, nous appelons la méthode [**StorageFolderQueryResult.GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br208074) qui retourne des objets [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) représentant des dossiers virtuels. Dans ce cas, nous regroupons par mois, de sorte que chaque dossier virtuel représente un groupe de fichiers du même mois.

> [!div class="tabbedCodeSnippets"] > ```cpp
> //#include <ppltasks.h>
> //#include <string>
> //#include <memory>
> using namespace Windows::Storage;
> using namespace Windows::Storage::Search; > using namespace concurrency; > using namespace Platform::Collections; > using namespace Windows::Foundation::Collections; > using namespace std; > > StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary; > > StorageFolderQueryResult^ queryResult = 
> picturesFolder->CreateFolderQuery(CommonFolderQuery::GroupByMonth); > > // Utilisez shared_ptr de sorte que la chaîne outputString reste en mémoire
> // jusqu’à ce que la tâche se termine, c’est-à-dire lorsque la fonction bascule hors de portéee.
> auto outputString = std::make_shared<wstring>(); > > create_task( queryResult->GetFoldersAsync()).then([this, outputString] (IVectorView<StorageFolder^>^ view) > {        
> for ( unsigned int i = 0; i < view->Size; i++) >     {
>         create_task(view->GetAt(i)->GetFilesAsync()).then([this, i, view, outputString](IVectorView<StorageFile^>^ files) > { > *outputString += view->GetAt(i)->Name->Data(); > *outputString += L"("; > *outputString += to_wstring(files->Size); > *outputString += L")\r\n"; > for (unsigned int j = 0; j < files->Size; j++) > { > *outputString += L" "; > *outputString += files->GetAt(j)->Name->Data(); > *outputString += L"\r\n"; > } > }).then([this, outputString]() > { > m_OutputTextBlock->Text = ref new String((*outputString).c_str()); > }); > } > }); > ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> 
> StorageFolderQueryResult queryResult = 
>     picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth);
>         
> IReadOnlyList<StorageFolder> folderList = > await queryResult.GetFoldersAsync(); > > StringBuilder outputText = new StringBuilder(); > > foreach (StorageFolder folder in folderList)
> {
> IReadOnlyList<StorageFile> fileList = await folder.GetFilesAsync(); > > // Imprimez le mois et le nombre de fichiers de ce groupe.
> outputText.AppendLine(folder.Name + " (" + fileList.Count + ")"); > > foreach (StorageFile file in fileList) > { > // Imprimez le nom du fichier.
> outputText.AppendLine(" " + file.Name); > } > } > ``` > ```vb > Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary > Dim outputText As New StringBuilder > > Dim queryResult As StorageFolderQueryResult = > picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth) > > Dim folderList As IReadOnlyList(Of StorageFolder) = > Await queryResult.GetFoldersAsync() > > For Each folder As StorageFolder In folderList > > Dim fileList As IReadOnlyList(Of StorageFile) = > Await folder.GetFilesAsync() > > ' Imprimez le mois et le nombre de fichiers de ce groupe.
> outputText.AppendLine(folder.Name & " (" & fileList.Count & ")") > > For Each file As StorageFile In fileList > > ' Imprimez le nom du fichier.
> outputText.AppendLine(" " & file.Name) > > Next file > > Next folder > ``` La sortie de l’exemple ressemble à ce qui suit.

``` syntax
July ‎2015 (2)
   MyImage3.png
   MyImage4.png
‎December ‎2014 (2)
   MyImage1.png
   MyImage2.png
```



<!--HONumber=Mar16_HO1-->


