---
author: laurenhughes
ms.assetid: 12ECEA89-59D2-4BCE-B24C-5A4DD525E0C7
title: "Accès au contenu du Groupement résidentiel"
description: "Accédez au contenu stocké dans le dossier Groupement résidentiel de l’utilisateur, qui contient des images, de la musique et des vidéos."
translationtype: Human Translation
ms.sourcegitcommit: 6822bb63ac99efdcdd0e71c4445883f4df5f471d
ms.openlocfilehash: d55908186e5e0687c7dbd22fee9d2f7b70ba1707

---
# <a name="accessing-homegroup-content"></a>Accès au contenu du Groupement résidentiel

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


** API importantes **

-   [**Classe Windows.Storage.KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151)

Accédez au contenu stocké dans le dossier Groupement résidentiel de l’utilisateur, qui contient des images, de la musique et des vidéos.

## <a name="prerequisites"></a>Conditions préalables

-   **Comprendre la programmation asynchrone pour les applications pour la plateforme Windows universelle (UWP)**

    Pour apprendre à écrire des applications asynchrones en C# ou Visual Basic, voir [Appeler des API asynchrones en C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Pour apprendre à écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Déclarations des fonctionnalités d’application**

    Pour accéder au contenu Groupement résidentiel, l’ordinateur de l’utilisateur doit avoir un Groupement résidentiel configuré et votre application au moins l’une des fonctionnalités suivantes : **picturesLibrary**, **musicLibrary** ou **videosLibrary**. Lorsque votre application accédera au dossier Groupement résidentiel, elle ne verra que les bibliothèques correspondant aux fonctionnalités déclarées dans le manifeste de votre application. Pour en savoir plus, voir [Autorisations d’accès aux fichiers](file-access-permissions.md).

    **Remarque** Le contenu de la bibliothèque Documents d’un Groupement résidentiel n’est pas visible pour votre application quelles que soient les fonctionnalités déclarées dans le manifeste de votre application et quels que soient les paramètres de partage de l’utilisateur.

     

-   **Comprendre comment utiliser les sélecteurs de fichiers**

    Le sélecteur de fichiers sert généralement à accéder aux fichiers et aux dossiers du Groupement résidentiel. Pour savoir comment utiliser le sélecteur de fichiers, voir [Ouvrir des fichiers et dossiers à l’aide d’un sélecteur](quickstart-using-file-and-folder-pickers.md).

-   **Comprendre les requêtes de fichiers et de dossiers**

    Vous pouvez utiliser des requêtes pour énumérer les fichiers et les dossiers du Groupement résidentiel. Pour en savoir plus sur les requêtes de fichiers et de dossiers, voir [Énumération et interrogation de fichiers et de dossiers](quickstart-listing-files-and-folders.md).

## <a name="open-the-file-picker-at-the-homegroup"></a>Ouvrir le sélecteur de fichiers au Groupement résidentiel

Suivez ces étapes pour ouvrir une instance du sélecteur de fichiers qui permet à l’utilisateur de sélectionner des fichiers et des dossiers du Groupement résidentiel :

1.  **Créer et personnaliser le sélecteur de fichiers**

    Utilisez [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) pour créer le sélecteur de fichiers, puis définissez le paramètre [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207854) du sélecteur sur [**PickerLocationId.HomeGroup**](https://msdn.microsoft.com/library/windows/apps/br207890). Ou définissez les autres propriétés qui sont pertinentes pour vos utilisateurs et votre application. Pour obtenir des directives susceptibles de vous aider à choisir comment personnaliser le sélecteur de fichiers, voir [Recommandations et liste de vérification sur les sélecteurs de fichiers](https://msdn.microsoft.com/library/windows/apps/hh465182).

    Cet exemple crée un sélecteur de fichiers qui s’ouvre au Groupement résidentiel, inclut des fichiers de tous types et affiche les fichiers sous forme d’images miniatures :
    ```csharp
    Windows.Storage.Pickers.FileOpenPicker picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.HomeGroup;
    picker.FileTypeFilter.Clear();
    picker.FileTypeFilter.Add("*");
    ```

2.  **Montrer le sélecteur de fichiers et traiter le fichier sélectionné.**

    Une fois que vous avez créé et personnalisé le sélecteur de fichiers, offrez à l’utilisateur la possibilité de choisir un fichier en appelant [**FileOpenPicker.PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275), ou plusieurs fichiers en appelant [**FileOpenPicker.PickMultipleFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br207851).

    Cet exemple affiche le sélecteur de fichiers pour permettre à l’utilisateur de sélectionner un fichier :
    ```csharp
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();

    if (file != null)
    {
        // Do something with the file.
    }
    else
    {
        // No file returned. Handle the error.
    }   
    ```

## <a name="search-the-homegroup-for-files"></a>Chercher des fichiers dans le Groupement résidentiel

Cette sélection illustre comment trouver des éléments du Groupement résidentiel qui correspondent à un terme de requête fourni par l’utilisateur.

1.  **Obtenez le terme de requête de l’utilisateur.**

    Nous obtenons ici un terme de requête que l’utilisateur a entré dans un contrôle [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) appelé `searchQueryTextBox` :
    ```csharp
    string queryTerm = this.searchQueryTextBox.Text;    
    ```

2.  **Définissez les options de requête et le filtre de recherche.**

    Les options de requête déterminent la manière avec laquelle les résultats de la recherche sont triés, tandis que le filtre de recherche détermine quels éléments sont inclus dans les résultats de la recherche.

    Cet exemple définit des options de requête qui trient les résultats de recherche par pertinence, puis par date de modification. Le filtre de recherche est le terme de requête que l’utilisateur a entré à l’étape précédente :
    ```csharp
    Windows.Storage.Search.QueryOptions queryOptions =
            new Windows.Storage.Search.QueryOptions
                (Windows.Storage.Search.CommonFileQuery.OrderBySearchRank, null);
    queryOptions.UserSearchFilter = queryTerm.Text;
    Windows.Storage.Search.StorageFileQueryResult queryResults =
            Windows.Storage.KnownFolders.HomeGroup.CreateFileQueryWithOptions(queryOptions);    
    ```

3.  **Exécutez la requête puis traitez les résultats.**

    L’exemple suivant exécute la requête de recherche dans le Groupement résidentiel et enregistre les noms de tout fichier correspondant sous forme de liste de chaînes.
    ```csharp
    System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFile> files =
        await queryResults.GetFilesAsync();

    if (files.Count > 0)
    {
        outputString += (files.Count == 1) ? "One file found\n" : files.Count.ToString() + " files found\n";
        foreach (Windows.Storage.StorageFile file in files)
        {
            outputString += file.Name + "\n";
        }
    }    
    ```


## <a name="search-the-homegroup-for-a-particular-users-shared-files"></a>Rechercher dans le Groupement résidentiel les fichiers partagés d’un utilisateur particulier

Cette section vous montre comment trouver les fichiers du Groupement résidentiel qui sont partagés par un utilisateur particulier.

1.  **Obtenir un ensemble d’utilisateurs du Groupement résidentiel.**

    Chacun des dossiers de premier niveau du Groupement résidentiel représente un utilisateur individuel. Ainsi, pour obtenir la collection d’utilisateurs du Groupement résidentiel, appelez [**GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br227279) pour récupérer les dossiers du Groupement résidentiel du niveau supérieur.
    ```csharp
    System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFolder> hgFolders =
        await Windows.Storage.KnownFolders.HomeGroup.GetFoldersAsync();    
    ```

2.  **Trouver le dossier de l’utilisateur cible, puis créer une requête de fichier dont la portée correspond au dossier de cet utilisateur.**

    L’exemple suivant répète l’opération sur les dossiers récupérés jusqu’à ce qu’il trouve le dossier de l’utilisateur cible. Ensuite, il définit des options de requête pour trouver tous les fichiers du dossier, d’abord triés par pertinence, puis par date de modification. L’exemple construit une chaîne qui rapporte le nombre de fichiers trouvés, ainsi que les noms des fichiers.
    ```csharp
    bool userFound = false;
    foreach (Windows.Storage.StorageFolder folder in hgFolders)
    {
        if (folder.DisplayName == targetUserName)
        {
            // Found the target user's folder, now find all files in the folder.
            userFound = true;
            Windows.Storage.Search.QueryOptions queryOptions =
                new Windows.Storage.Search.QueryOptions
                    (Windows.Storage.Search.CommonFileQuery.OrderBySearchRank, null);
            queryOptions.UserSearchFilter = "*";
            Windows.Storage.Search.StorageFileQueryResult queryResults =
                folder.CreateFileQueryWithOptions(queryOptions);
            System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFile> files =
                await queryResults.GetFilesAsync();

            if (files.Count > 0)
            {
                string outputString = "Searched for files belonging to " + targetUserName + "'\n";
                outputString += (files.Count == 1) ? "One file found\n" : files.Count.ToString() + " files found\n";
                foreach (Windows.Storage.StorageFile file in files)
                {
                    outputString += file.Name + "\n";
                }
            }
        }
    }    
    ```

## <a name="stream-video-from-the-homegroup"></a>Lecture en continu de la vidéo du Groupement résidentiel

Suivez les étapes suivantes pour lire en continu le contenu vidéo du Groupement résidentiel :

1.  **Inclure un MediaElement dans votre application.**

    Un [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) vous permet de lire des contenus audio et vidéo dans votre application. Pour plus d’informations sur la lecture audio et vidéo, voir [Créer des contrôles de transport personnalisés](https://msdn.microsoft.com/library/windows/apps/mt187271) et [Audio, vidéo et appareil photo](https://msdn.microsoft.com/library/windows/apps/mt203788).
    ```HTML
    <Grid x:Name="Output" HorizontalAlignment="Left" VerticalAlignment="Top" Grid.Row="1">
        <MediaElement x:Name="VideoBox" HorizontalAlignment="Left" VerticalAlignment="Top" Margin="0" Width="400" Height="300"/>
    </Grid>    
    ```

2.  **Ouvrez un sélecteur de fichiers dans le Groupement résidentiel, et appliquez un filtre qui inclut les fichiers vidéo dans les formats pris en charge par votre application.**

    Cet exemple inclut les fichiers .mp4 et .wmv dans le sélecteur d’ouverture de fichier.
    ```csharp
    Windows.Storage.Pickers.FileOpenPicker picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.HomeGroup;
    picker.FileTypeFilter.Clear();
    picker.FileTypeFilter.Add(".mp4");
    picker.FileTypeFilter.Add(".wmv");
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();   
    ```

3.  **Ouvrir la sélection de fichier de l’utilisateur pour un accès en lecture, définir le flux de fichiers comme source pour le** [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), puis lire le fichier.
    ```csharp
    if (file != null)
    {
        var stream = await file.OpenAsync(Windows.Storage.FileAccessMode.Read);
        VideoBox.SetSource(stream, file.ContentType);
        VideoBox.Stop();
        VideoBox.Play();
    }
    else
    {
        // No file selected. Handle the error here.
    }    
    ```

 

 



<!--HONumber=Dec16_HO1-->


