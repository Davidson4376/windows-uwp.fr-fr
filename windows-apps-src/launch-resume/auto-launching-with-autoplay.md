---
title: Démarrage automatique avec lecture automatique
description: Vous pouvez utiliser la lecture automatique pour proposer votre application en tant qu’option lorsque l’utilisateur connecte un périphérique à son PC. Cela inclut les périphériques autres que les périphériques de volume, tels qu’un appareil photo ou un lecteur multimédia, ou les périphériques de volume tels qu’une clé USB, une carte mémoire SD ou un DVD.
ms.assetid: AD4439EA-00B0-4543-887F-2C1D47408EA7
---

# <span id="dev_launch_resume.auto-launching_with_autoplay"> </span>Démarrage automatique avec lecture automatique


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Vous pouvez utiliser la **lecture automatique** pour proposer votre application en tant qu’option lorsque l’utilisateur connecte un périphérique à son PC. Cela inclut les périphériques autres que les périphériques de volume, tels qu’un appareil photo ou un lecteur multimédia, ou les périphériques de volume tels qu’une clé USB, une carte mémoire SD ou un DVD. Vous pouvez également utiliser la **lecture automatique** pour proposer votre application en tant qu’option quand des utilisateurs partagent des fichiers entre deux PC à l’aide de la fonction de proximité (appui).

> **Remarque** Si vous êtes fabricant de périphériques et si vous voulez associer votre [application pour périphériques du Windows Store](http://go.microsoft.com/fwlink/p/?LinkID=301381) en tant que gestionnaire de **lecture automatique** de votre périphérique, vous pouvez identifier l’application dans les métadonnées du périphérique. Pour plus d’informations, voir [Lecture automatique pour les applications pour périphériques du Windows Store](http://go.microsoft.com/fwlink/p/?LinkId=306684).

## S’inscrire pour le contenu de lecture automatique

Vous pouvez inscrire des applications en tant qu’options pour les événements de contenu de **lecture automatique**. Les événements de contenu de **lecture automatique** se déclenchent lorsqu’un périphérique de volume, tel que la carte mémoire d’un appareil photo, une clé USB ou un DVD, est inséré dans le PC. Voici comment identifier votre application en tant qu’option de **lecture automatique** lorsqu’un périphérique de volume d’un appareil photo est inséré.

Dans ce didacticiel, vous avez créé une application qui affiche des fichiers image ou les copie dans les images. Vous avez inscrit l’application pour l’événement de contenu de lecture automatique **ShowPicturesOnArrival**.

La lecture automatique déclenche également des événements de contenu pour du contenu partagé entre des PC en utilisant la fonction de proximité (action d’appuyer). Vous pouvez utiliser les étapes et le code de cette section pour gérer des fichiers qui sont partagés entre des PC utilisant la fonction de proximité. Le tableau suivant répertorie les événements de contenu de lecture automatique disponibles pour le partage de contenu via la fonction de proximité.

| Action         | Événement de contenu de lecture automatique  |
|----------------|-------------------------|
| Partage de musique  | PlayMusicFilesOnArrival |
| Partage de vidéos | PlayVideoFilesOnArrival |

 
Lorsque les fichiers sont partagés à l’aide de la fonction de proximité, la propriété **Files** de l’objet **FileActivatedEventArgs** contient une référence à un dossier racine reprenant l’intégralité des fichiers partagés.

### Étape 1 : Créer un projet et ajouter les déclarations de lecture automatique

1.  Ouvrez Microsoft Visual Studio et sélectionnez **Nouveau projet** dans le menu **Fichier**. Dans la section **Visual C#**, sous **Windows**, sélectionnez **Application vide (Windows universelle)**. Attribuez à l’application le nom **AutoPlayDisplayOrCopyImages**, puis cliquez sur **OK**.
2.  Ouvrez le fichier Package.appxmanifest, puis sélectionnez l’onglet **Capacités**. Sélectionnez les fonctionnalités **Stockage amovible** et **Bibliothèque d’images**. Cela permet à l’application d’accéder à des périphériques de stockage amovibles pour la mémoire de l’appareil photo, et d’accéder aux images locales.
3.  Dans le fichier manifeste, sélectionnez l’onglet **Déclarations**. Dans la liste déroulante **Déclarations disponibles**, sélectionnez **Contenu de lecture automatique**, puis cliquez sur **Ajouter**. Sélectionnez le nouvel élément **Contenu de lecture automatique** ajouté à la liste **Déclarations prises en charge**.
4.  Une déclaration **Contenu de lecture automatique** identifie votre application en tant qu’option lorsque la lecture automatique déclenche un événement de contenu. L’événement est basé sur le contenu d’un périphérique de volume tel qu’un DVD ou une clé USB. La lecture automatique examine le contenu du périphérique de volume et détermine l’événement de contenu à déclencher. Si la racine du volume contient un dossier DCIM, AVCHD ou PRIVATE\ACHD ou si un utilisateur a activé **Choisir l’action pour chaque type de média** dans l’applet Lecture automatique du Panneau de configuration et que des images sont présentes à la racine du volume, la lecture automatique déclenche alors l’événement **ShowPicturesOnArrival**. Dans la section **Actions de lancement**, entrez les valeurs suivantes pour la première action de lancement.
5.  Dans la section **Actions de lancement** pour l’élément **Contenu de lecture automatique**, cliquez sur **Ajouter nouveau** pour ajouter une deuxième action de lancement. Entrez les valeurs suivantes pour la deuxième action de lancement.
6.  Dans la liste déroulante **Déclarations disponibles**, sélectionnez **Associations de types de fichiers**, puis cliquez sur **Ajouter**. Dans les propriétés de la nouvelle déclaration **Associations de types de fichiers**, attribuez au champ **Nom complet** la valeur **AutoPlay Copy or Show Images** et au champ **Nom** la valeur **image_association1**. Dans la section **Types de fichiers pris en charge**, cliquez sur **Ajouter nouveau**. Attribuez au champ **Type de fichier** la valeur **.jpg**. Dans la section **Types de fichiers pris en charge**, attribuez au champ **Type de fichier** de la nouvelle association de fichier la valeur **.png**. Pour les événements de contenu, la lecture automatique filtre les types de fichiers qui ne sont pas explicitement associés à votre application.
7.  Enregistrez et fermez le fichier manifeste.


**Tableau 1**

| Paramètre             | Valeur                 |
|---------------------|-----------------------|
| Verbe                | show                  |
| Nom complet de l’action | Show Pictures         |
| Événement de contenu       | ShowPicturesOnArrival |

Le paramètre **Nom complet de l’action** identifie la chaîne que la lecture automatique affiche pour votre application. Le paramètre **Verbe** identifie une valeur qui est transmise à votre application pour l’option sélectionnée. Vous pouvez spécifier plusieurs options de lancement pour un événement de lecture automatique et utiliser le paramètre **Verbe** pour déterminer quelle option l’utilisateur a sélectionnée pour votre application. Vous pouvez vérifier quelle option a été sélectionnée par l’utilisateur par le biais de la propriété **verb** des arguments d’événement de démarrage transmis à votre application. Vous pouvez attribuer n’importe quelle valeur au paramètre **Verbe**, sauf la valeur **open** qui est réservée.

**Tableau 2**  

| Paramètre             | Valeur                      |
|---------------------|----------------------------|
| Verbe                | copy                       |
| Nom complet de l’action | Copy Pictures Into Library |
| Événement de contenu       | ShowPicturesOnArrival      |

### Étape 2 : Ajouter une interface utilisateur XAML

Ouvrez le fichier MainPage.xaml et ajoutez le code XAML suivant à la section &lt;Grid&gt; par défaut.

```xaml
<TextBlock FontSize="18">File List</TextBlock>
<TextBlock x:Name="FilesBlock" HorizontalAlignment="Left" TextWrapping="Wrap" 
           VerticalAlignment="Top" Margin="0,20,0,0" Height="280" Width="240" />
<Canvas x:Name="FilesCanvas" HorizontalAlignment="Left" VerticalAlignment="Top" 
        Margin="260,20,0,0" Height="280" Width="100"/>
```

### Étape 3 : Ajouter du code d’initialisation

Le code de cette étape vérifie la valeur du verbe dans la propriété **Verb**, qui est l’un des arguments de démarrage transmis à l’application pendant l’événement **OnFileActivated**. Le code appelle alors une méthode associée à l’option que l’utilisateur a sélectionnée. Pour un événement de mémoire d’appareil photo, la lecture automatique passe le dossier racine du stockage de l’appareil photo à l’application. Vous pouvez récupérer ce dossier à partir du premier élément de la propriété **Files**.

Ouvrez le fichier App.xaml.cs et ajoutez le code suivant à la classe **App**.

```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
    if (args.Verb == "show")
    {
        Frame rootFrame = (Frame)Window.Current.Content;
        MainPage page = (MainPage)rootFrame.Content;

        // Call DisplayImages with root folder from camera storage.
        page.DisplayImages((Windows.Storage.StorageFolder)args.Files[0]);
    }

    if (args.Verb == "copy")
    {
        Frame rootFrame = (Frame)Window.Current.Content;
        MainPage page = (MainPage)rootFrame.Content;

        // Call CopyImages with root folder from camera storage.
        page.CopyImages((Windows.Storage.StorageFolder)args.Files[0]);
    }

    base.OnFileActivated(args);
}
```

> **Remarque** Les méthodes `DisplayImages` et `CopyImages` sont ajoutées dans les étapes suivantes.

### Étape 4 : Ajouter du code pour afficher des images

Dans le fichier MainPage.xaml.cs, ajoutez le code suivant à la classe **MainPage**.

```cs
async internal void DisplayImages(Windows.Storage.StorageFolder rootFolder)
{
    // Display images from first folder in root\DCIM.
    var dcimFolder = await rootFolder.GetFolderAsync("DCIM");
    var folderList = await dcimFolder.GetFoldersAsync();
    var cameraFolder = folderList[0];
    var fileList = await cameraFolder.GetFilesAsync();
    for (int i = 0; i < fileList.Count; i++)
    {
        var file = (Windows.Storage.StorageFile)fileList[i];
        WriteMessageText(file.Name + "\n");
        DisplayImage(file, i);
    }
}

async private void DisplayImage(Windows.Storage.IStorageItem file, int index)
{
    try
    {
        var sFile = (Windows.Storage.StorageFile)file;
        Windows.Storage.Streams.IRandomAccessStream imageStream =
            await sFile.OpenAsync(Windows.Storage.FileAccessMode.Read);
        Windows.UI.Xaml.Media.Imaging.BitmapImage imageBitmap =
            new Windows.UI.Xaml.Media.Imaging.BitmapImage();
        imageBitmap.SetSource(imageStream);
        var element = new Image();
        element.Source = imageBitmap;
        element.Height = 100;
        Thickness margin = new Thickness();
        margin.Top = index * 100;
        element.Margin = margin;
        FilesCanvas.Children.Add(element);
    }
    catch (Exception e)
    {
       WriteMessageText(e.Message + "\n");
    }
}

// Write a message to MessageBlock on the UI thread.
private Windows.UI.Core.CoreDispatcher messageDispatcher = Window.Current.CoreWindow.Dispatcher;

private async void WriteMessageText(string message, bool overwrite = false)
{
    await messageDispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () =>
        {
            if (overwrite)
                FilesBlock.Text = message;
            else
                FilesBlock.Text += message;
        });
}
```

### Étape 5 : Ajouter du code pour copier des images

Dans le fichier MainPage.xaml.cs, ajoutez le code suivant à la classe **MainPage**.

```cs
async internal void CopyImages(Windows.Storage.StorageFolder rootFolder)
{
    // Copy images from first folder in root\DCIM.
    var dcimFolder = await rootFolder.GetFolderAsync("DCIM");
    var folderList = await dcimFolder.GetFoldersAsync();
    var cameraFolder = folderList[0];
    var fileList = await cameraFolder.GetFilesAsync();

    try
    {
        var folderName = "Images " + DateTime.Now.ToString("yyyy-MM-dd HHmmss");
        Windows.Storage.StorageFolder imageFolder = await
            Windows.Storage.KnownFolders.PicturesLibrary.CreateFolderAsync(folderName);

        foreach (Windows.Storage.IStorageItem file in fileList)
        {
            CopyImage(file, imageFolder);
        }
    }
    catch (Exception e)
    {
        WriteMessageText("Failed to copy images.\n" + e.Message + "\n");
    }
}

async internal void CopyImage(Windows.Storage.IStorageItem file,
                              Windows.Storage.StorageFolder imageFolder)
{
    try
    {
        Windows.Storage.StorageFile sFile = (Windows.Storage.StorageFile)file;
        await sFile.CopyAsync(imageFolder, sFile.Name);
        WriteMessageText(sFile.Name + " copied.\n");
    }
    catch (Exception e)
    {
        WriteMessageText("Failed to copy file.\n" + e.Message + "\n");
    }
}
```

### Étape 6 : Générer et exécuter l’application

1.  Appuyez sur F5 pour créer et déployer l’application (en mode débogage).
2.  Pour exécuter votre application, insérez une carte mémoire d’appareil photo ou un autre périphérique de stockage d’un appareil photo dans votre PC. Sélectionnez ensuite l’une des options d’événement de contenu que vous avez spécifiées dans votre fichier package.appxmanifest à partir de la liste d’options de la lecture automatique. Cet exemple de code affiche ou copie uniquement les images du dossier DCIM de la carte mémoire d’un appareil photo. Si la carte mémoire de votre appareil photo stocke des images dans un dossier AVCHD ou PRIVATE\\ACHD, vous devez mettre à jour le code en conséquence.
    **Remarque** Si vous n’avez pas de carte mémoire d’appareil photo, vous pouvez utiliser un disque mémoire flash à condition qu’il ait un dossier nommé **DCIM** à la racine, et que le dossier DCIM dispose d’un sous-dossier contenant des images.

## S’inscrire pour un appareil de lecture automatique


Vous pouvez inscrire des applications en tant qu’options pour les événements de périphérique de **lecture automatique**. Les événements de périphériques de **lecture automatique** sont déclenchés lorsqu’un périphérique est connecté à un PC.

Voici comment identifier votre application en tant qu’option de **lecture automatique** lorsqu’un appareil photo est connecté à un PC. L’application s’inscrit en tant que gestionnaire de l’événement **WPD\\ImageSourceAutoPlay**. Il s’agit d’un événement courant que le système WPD (Windows Portable Device) déclenche lorsque des appareils photo et d’autres périphériques d’acquisition d’images l’avertissent qu’ils sont une source d’image (ImageSource) utilisant MTP. Pour plus d’informations, voir [Appareils mobiles Windows](https://msdn.microsoft.com/library/windows/hardware/ff597729).

**Important** Les API [**Windows.Devices.Portable.StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654) font partie de la [famille d’appareils de bureau](https://msdn.microsoft.com/library/windows/apps/dn894631). Les applications peuvent utiliser ces API uniquement sur les appareils Windows 10 de la famille d’appareils de bureau, tels que les PC.

 

### Étape 1 : Créer un projet et ajouter les déclarations de lecture automatique

1.  Ouvrez Visual Studio et sélectionnez **Nouveau projet** dans le menu **Fichier**. Dans la section **Visual C#**, sous **Windows**, sélectionnez **Application vide (Windows universelle)**. Attribuez à l’application le nom **AutoPlayDevice\_Camera**, puis cliquez sur **OK.**
2.  Ouvrez le fichier Package.appxmanifest, puis sélectionnez l’onglet **Capacités**. Sélectionnez la fonctionnalité **Stockage amovible**. Cela permet à l’application d’accéder aux données situées sur l’appareil photo en tant que périphérique de volume de stockage amovible.
3.  Dans le fichier manifeste, sélectionnez l’onglet **Déclarations**. Dans la liste déroulante **Déclarations disponibles**, sélectionnez **Périphérique de lecture automatique**, puis cliquez sur **Ajouter**. Sélectionnez le nouvel élément **Périphérique de lecture automatique** ajouté à la liste **Déclarations prises en charge**.
4.  Une déclaration **Périphérique de lecture automatique** identifie votre application en tant qu’option lorsque la lecture automatique déclenche un événement de périphérique pour des événements connus. Dans la section **Actions de lancement**, entrez les valeurs suivantes pour la première action de lancement.
5.  Dans la liste déroulante **Déclarations disponibles**, sélectionnez **Associations de types de fichiers**, puis cliquez sur **Ajouter**. Dans les propriétés de la nouvelle déclaration **Associations de types de fichiers**, attribuez au champ **Nom complet** la valeur **Show Images from Camera** et au champ **Nom** la valeur **camera_association1**. Dans la section **Types de fichiers pris en charge**, cliquez sur **Ajouter nouveau** (si nécessaire). Attribuez au champ **Type de fichier** la valeur **.jpg**. Dans la section **Types de fichiers pris en charge**, cliquez à nouveau sur **Ajouter nouveau**. Attribuez au champ **Type de fichier** de la nouvelle association de fichier la valeur **.png**. Pour les événements de contenu, la lecture automatique filtre les types de fichiers qui ne sont pas explicitement associés à votre application.
6.  Enregistrez et fermez le fichier manifeste.

| Paramètre             | Valeur            |
|---------------------|------------------|
| Verbe                | show             |
| Nom complet de l’action | Show Pictures    |
| Événement de contenu       | WPD\\ImageSource |

Le paramètre **Nom complet de l’action** identifie la chaîne que la lecture automatique affiche pour votre application. Le paramètre **Verbe** identifie une valeur qui est transmise à votre application pour l’option sélectionnée. Vous pouvez spécifier plusieurs options de lancement pour un événement de lecture automatique et utiliser le paramètre **Verbe** pour déterminer quelle option l’utilisateur a sélectionnée pour votre application. Vous pouvez vérifier quelle option a été sélectionnée par l’utilisateur par le biais de la propriété **verb** des arguments d’événement de démarrage transmis à votre application. Vous pouvez attribuer n’importe quelle valeur au paramètre **Verbe**, sauf la valeur **open** qui est réservée. Pour obtenir un exemple d’utilisation de plusieurs verbes dans une même application, voir [S’inscrire pour du contenu de lecture automatique](#autoplaycontent).

### Étape 2 : Ajouter la référence d’assembly pour les extensions de bureau

Les API nécessaires pour accéder au stockage d’un appareil portable Windows, [**Windows.Devices.Portable.StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654), font partie de la [famille d’appareils de bureau](https://msdn.microsoft.com/library/windows/apps/dn894631). Cela signifie qu’un assembly spécial est requis pour utiliser les API et ces appels fonctionneront uniquement sur un appareil de la famille des appareils de bureau (par exemple un PC).

1.  Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis cliquez sur **Ajouter une référence...**.
2.  Développez **Windows universel** et cliquez sur **Extensions**.
3.  Sélectionnez ensuite **Extensions de bureau Windows pour UWP** et cliquez sur **OK**.

### Étape 3 : Ajouter une interface utilisateur XAML

Ouvrez le fichier MainPage.xaml et ajoutez le code XAML suivant à la section &lt;Grid&gt; par défaut.

```xaml
<StackPanel Orientation="Vertical" Margin="10,0,-10,0">
    <TextBlock FontSize="24">Device Information</TextBlock>
    <StackPanel Orientation="Horizontal">
        <TextBlock x:Name="DeviceInfoTextBlock" FontSize="18" Height="400" Width="400" VerticalAlignment="Top" />
        <ListView x:Name="ImagesList" HorizontalAlignment="Left" Height="400" VerticalAlignment="Top" Width="400">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Vertical">
                        <Image Source="{Binding Path=Source}" />
                        <TextBlock Text="{Binding Path=Name}" />
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
            <ListView.ItemsPanel>
                <ItemsPanelTemplate>
                    <WrapGrid Orientation="Horizontal" ItemHeight="100" ItemWidth="120"></WrapGrid>
                </ItemsPanelTemplate>
            </ListView.ItemsPanel>
        </ListView>
    </StackPanel>
</StackPanel>
```

### Étape 4 : Ajouter du code d’activation

Le code de cette étape fait référence à l’appareil photo en tant que [**StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654) en transmettant l’ID des informations de périphérique de l’appareil photo à la méthode [**FromId**](https://msdn.microsoft.com/library/windows/apps/br225655). L’ID des informations de périphérique de l’appareil photo est obtenu en effectuant tout d’abord un cast des arguments de l’événement en tant que [**DeviceActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224710), puis en obtenant la valeur de la propriété [**DeviceInformationId**](https://msdn.microsoft.com/library/windows/apps/br224711).

Ouvrez le fichier App.xaml.cs et ajoutez le code suivant à la classe **App**.

```cs
protected override void OnActivated(IActivatedEventArgs args)
{
   if (args.Kind == ActivationKind.Device)
   {
      Frame rootFrame = null;
      // Ensure that the current page exists and is activated
      if (Window.Current.Content == null)
      {
         rootFrame = new Frame();
         rootFrame.Navigate(typeof(MainPage));
         Window.Current.Content = rootFrame;
      }
      else
      {
         rootFrame = Window.Current.Content as Frame;
      }
      Window.Current.Activate();

      // Make sure the necessary APIs are present on the device
      bool storageDeviceAPIPresent =
      Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.Devices.Portable.StorageDevice");

      if (storageDeviceAPIPresent)
      {
         // Reference the current page as type MainPage
         var mPage = rootFrame.Content as MainPage;

         // Cast the activated event args as DeviceActivatedEventArgs and show images
         var deviceArgs = args as DeviceActivatedEventArgs;
         if (deviceArgs != null)
         {
            mPage.ShowImages(Windows.Devices.Portable.StorageDevice.FromId(deviceArgs.DeviceInformationId));
         }
      }
      else
      {
         // Handle case where APIs are not present (when the device is not part of the desktop device family)
      }

   }

   base.OnActivated(args);
}
```

> **Remarque** La méthode `ShowImages` est ajoutée à l’étape suivante.

### Étape 5 : Ajouter du code pour afficher des informations de périphérique

Vous pouvez obtenir des informations sur l’appareil photo à partir des propriétés de la classe [**StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654). Le code de cette étape affiche le nom du périphérique et d’autres informations pour l’utilisateur au moment de l’exécution de l’application. Le code appelle alors les méthodes GetImageList et GetThumbnail, que vous ajouterez à l’étape suivante, pour afficher des miniatures des images stockées dans l’appareil photo.

Dans le fichier MainPage.xaml.cs, ajoutez le code suivant à la classe **MainPage**.

```cs
private Windows.Storage.StorageFolder rootFolder;

internal async void ShowImages(Windows.Storage.StorageFolder folder)
{
    DeviceInfoTextBlock.Text = "Display Name = " + folder.DisplayName + "\n";
    DeviceInfoTextBlock.Text += "Display Type =  " + folder.DisplayType + "\n";
    DeviceInfoTextBlock.Text += "FolderRelativeId = " + folder.FolderRelativeId + "\n";

    // Reference first folder of the device as the root
    rootFolder = (await folder.GetFoldersAsync())[0];
    var imageList = await GetImageList(rootFolder);

    foreach (Windows.Storage.StorageFile img in imageList)
    {
        ImagesList.Items.Add(await GetThumbnail(img));
    }
}
```

> **Remarque** Les méthodes `GetImageList` et `GetThumbnail` sont ajoutées à l’étape suivante.

 

### Étape 6 : Ajouter du code pour afficher des images

Le code de cette étape affiche des miniatures des images stockées dans l’appareil photo. Le code effectue des appels asynchrones à l’appareil photo pour obtenir l’image miniature. Toutefois, l’appel asynchrone suivant ne se produit qu’une fois l’appel asynchrone précédent terminé. Cela garantit qu’une seule demande à la fois est effectuée auprès de l’appareil photo.

Dans le fichier MainPage.xaml.cs, ajoutez le code suivant à la classe **MainPage**.

```cs
async private System.Threading.Tasks.Task<List<Windows.Storage.StorageFile>> GetImageList(Windows.Storage.StorageFolder folder) 
{
    var result = await folder.GetFilesAsync();
    var subFolders = await folder.GetFoldersAsync();
    foreach (Windows.Storage.StorageFolder f in subFolders)
        result = result.Union(await GetImageList(f)).ToList();

    return (from f in result orderby f.Name select f).ToList();
}

async private System.Threading.Tasks.Task<Image> GetThumbnail(Windows.Storage.StorageFile img) 
{
    // Get the thumbnail to display
    var thumbnail = await img.GetThumbnailAsync(Windows.Storage.FileProperties.ThumbnailMode.SingleItem,
                                                100,
                                                Windows.Storage.FileProperties.ThumbnailOptions.UseCurrentScale);

    // Create a XAML Image object bind to on the display page
    var result = new Image();
    result.Height = thumbnail.OriginalHeight;
    result.Width = thumbnail.OriginalWidth;
    result.Name = img.Name;
    var imageBitmap = new Windows.UI.Xaml.Media.Imaging.BitmapImage();
    imageBitmap.SetSource(thumbnail);
    result.Source = imageBitmap;

    return result;
}
```

### Étape 7 : Générer et exécuter l’application

1.  Appuyez sur F5 pour créer et déployer l’application (en mode débogage).
2.  Pour exécuter votre application, connectez un appareil photo à votre ordinateur. Sélectionnez ensuite l’application dans la liste d’options de la lecture automatique.
    **Remarque** Tous les appareils photo n’effectuent pas de publication pour l’événement de périphérique de lecture automatique **WPD\\ImageSource**.

     

## Configurer le stockage amovible


Vous pouvez identifier un périphérique de volume, tel qu’une carte mémoire ou une clé USB, comme périphérique de **lecture automatique** lorsque le périphérique de volume est connecté à un PC. Cette fonctionnalité est particulièrement utile lorsque vous voulez associer une application spécifique pour la **lecture automatique** à présenter à l’utilisateur pour votre périphérique de volume.

Voici comment identifier votre périphérique de volume comme périphérique de **lecture automatique**.

Pour identifier votre périphérique de volume comme périphérique de **lecture automatique**, ajoutez un fichier autorun.inf au lecteur racine de votre périphérique. Dans le fichier autorun.inf, ajoutez une clé **CustomEvent** à la section **AutoRun**. Lorsque votre périphérique de volume se connecte à un PC, la **lecture automatique** trouve le fichier autorun.inf et traite votre volume comme un périphérique. La **lecture automatique** crée un événement de **lecture automatique** à l’aide du nom que vous avez fourni dans la clé **CustomEvent**. Vous pouvez alors créer une application et l’inscrire en tant que gestionnaire de cet événement de **lecture automatique**. Lorsque le périphérique est connecté au PC, la **lecture automatique** affiche votre application comme gestionnaire de votre périphérique de volume. Pour plus d’informations sur les fichiers autorun.inf, voir [Entrées du fichier Autorun.inf](https://msdn.microsoft.com/library/windows/desktop/cc144200).

### Étape 1 : Créer un fichier autorun.inf

Dans le lecteur racine de votre périphérique de volume, ajoutez un fichier nommé autorun.inf. Ouvrez le fichier autorun.inf et ajoutez le texte suivant.

``` syntax
[AutoRun]
CustomEvent=AutoPlayCustomEventQuickstart
```

### Étape 2 : Créer un projet et ajouter les déclarations de lecture automatique

1.  Ouvrez Visual Studio et sélectionnez **Nouveau projet** dans le menu **Fichier**. Dans la section **Visual C#**, sous **Windows**, sélectionnez **Application vide (Windows universelle)**. Nommez l’application **AutoPlayCustomEvent** et cliquez sur **OK.**
2.  Ouvrez le fichier Package.appxmanifest, puis sélectionnez l’onglet **Capacités**. Sélectionnez la fonctionnalité **Stockage amovible**. Cela permet à l’application d’accéder aux fichiers et dossiers situés sur les périphériques de stockage amovibles.
3.  Dans le fichier manifeste, sélectionnez l’onglet **Déclarations**. Dans la liste déroulante **Déclarations disponibles**, sélectionnez **Contenu de lecture automatique**, puis cliquez sur **Ajouter**. Sélectionnez le nouvel élément **Contenu de lecture automatique** ajouté à la liste **Déclarations prises en charge**.

    **Remarque** Vous pouvez également choisir d’ajouter une déclaration **Périphérique de lecture automatique** pour votre événement de lecture automatique personnalisé.
    
4.  Dans la section **Actions de lancement** de votre déclaration d’événement **Contenu de lecture automatique**, entrez les valeurs suivantes pour la première action de lancement.
5.  Dans la liste déroulante **Déclarations disponibles**, sélectionnez **Associations de types de fichiers**, puis cliquez sur **Ajouter**. Dans les propriétés de la nouvelle déclaration **Associations de types de fichiers**, attribuez au champ **Nom complet** la valeur **Show .ms Files** et au champ **Nom** la valeur **ms_association**. Dans la section **Types de fichiers pris en charge**, cliquez sur **Ajouter nouveau**. Attribuez au champ **Type de fichier** la valeur **.ms**. Pour les événements de contenu, la lecture automatique filtre les types de fichiers qui ne sont pas explicitement associés à votre application.
6.  Enregistrez et fermez le fichier manifeste.

| Paramètre             | Valeur                         |
|---------------------|-------------------------------|
| Verbe                | show                          |
| Nom complet de l’action | Afficher les fichiers                    |
| Événement de contenu       | AutoPlayCustomEventQuickstart |

La valeur **Événement de contenu** correspond au texte que vous avez fourni pour la clé **CustomEvent** dans votre fichier autorun.inf. Le paramètre **Nom complet de l’action** identifie la chaîne que la lecture automatique affiche pour votre application. Le paramètre **Verbe** identifie une valeur qui est transmise à votre application pour l’option sélectionnée. Vous pouvez spécifier plusieurs options de lancement pour un événement de lecture automatique et utiliser le paramètre **Verbe** pour déterminer quelle option l’utilisateur a sélectionnée pour votre application. Vous pouvez vérifier quelle option a été sélectionnée par l’utilisateur par le biais de la propriété **verb** des arguments d’événement de démarrage transmis à votre application. Vous pouvez attribuer n’importe quelle valeur au paramètre **Verbe**, sauf la valeur **open** qui est réservée.

### Étape 3 : Ajouter une interface utilisateur XAML

Ouvrez le fichier MainPage.xaml et ajoutez le code XAML suivant à la section &lt;Grid&gt; par défaut.

```xaml
<StackPanel Orientation="Vertical">
    <TextBlock FontSize="28" Margin="10,0,800,0">Files</TextBlock>
    <TextBlock x:Name="FilesBlock" FontSize="22" Height="600" Margin="10,0,800,0" />
</StackPanel>
```

### Étape 4 : Ajouter du code d’activation

Dans les étapes suivantes, le code appelle une méthode destinée à afficher les dossiers présents dans le lecteur racine de votre périphérique de volume. Pour les événements de contenu de lecture automatique, la lecture automatique transmet le dossier racine du périphérique de stockage aux arguments de démarrage qui sont transmis à l’application pendant l’événement **OnFileActivated**. Vous pouvez récupérer ce dossier à partir du premier élément de la propriété **Files**.

Ouvrez le fichier App.xaml.cs et ajoutez le code suivant à la classe **App**.

```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
    var rootFrame = Window.Current.Content as Frame;
    var page = rootFrame.Content as MainPage;

    // Call ShowFolders with root folder from device storage.
    page.DisplayFiles(args.Files[0] as Windows.Storage.StorageFolder);

    base.OnFileActivated(args);
}
```

> **Remarque** La méthode `DisplayFiles` est ajoutée à l’étape suivante.

 

### Étape 5 : Ajouter du code pour afficher des dossiers

Dans le fichier MainPage.xaml.cs, ajoutez le code suivant à la classe **MainPage**.

```cs
internal async void DisplayFiles(Windows.Storage.StorageFolder folder)
{
    foreach (Windows.Storage.StorageFile f in await ReadFiles(folder, ".ms"))
    {
        FilesBlock.Text += "  " + f.Name + "\n";
    }
}

internal async System.Threading.Tasks.Task<IReadOnlyList<Windows.Storage.StorageFile>> 
    ReadFiles(Windows.Storage.StorageFolder folder, string fileExtension)
{
    var options = new Windows.Storage.Search.QueryOptions();
    options.FileTypeFilter.Add(fileExtension);
    var query = folder.CreateFileQueryWithOptions(options);
    var files = await query.GetFilesAsync();

    return files;
}
```

### Étape 6 : Générer et exécuter l’application

1.  Appuyez sur F5 pour créer et déployer l’application (en mode débogage).
2.  Pour exécuter votre application, insérez une carte mémoire ou un autre périphérique de stockage dans votre PC. Sélectionnez ensuite votre application dans la liste des options de gestionnaire de lecture automatique.

## Référence des événements de lecture automatique


Le système de **lecture automatique** permet aux applications de s’inscrire pour de nombreux événements d’arrivée de périphérique et de volume (disque). Pour pouvoir vous inscrire à des événements de contenu de **lecture automatique**, vous devez activer la fonctionnalité **Stockage amovible** dans votre manifeste de package. Ce tableau affiche les événements auxquels vous pouvez vous inscrire au moment où ils sont déclenchés.

| Scénario                                                           | Événement   | Description   |
|--------------------------------------------------------------------|---------|---------------|
| Utilisation de photos sur un appareil photo                                           | **WPD\ImageSource**                | Événement déclenché pour des appareils photo identifiés comme des appareils mobiles Windows et dotés de la fonctionnalité ImageSource.                                                                                                                                                                                                                                                                  |
| Utilisation de musique sur un lecteur audio                                     | **WPD\AudioSource**                | Événement déclenché pour des lecteurs multimédias identifiés comme des appareils mobiles Windows et dotés de la fonctionnalité AudioSource.                                                                                                                                                                                                                                                            |
| Utilisation de vidéos dans une caméra vidéo                                     | **WPD\VideoSource**                | Événement déclenché pour des caméras vidéo identifiées comme des appareils mobiles Windows et dotées de la fonctionnalité VideoSource.                                                                                                                                                                                                                                                            |
| Accéder à un disque mémoire flash ou un disque dur externe              | **StorageOnArrival**               | Événement déclenché lorsqu’un lecteur ou un volume est connecté au PC.   Si le lecteur ou le volume contient un dossier DCIM, AVCHD ou PRIVATE\ACHD à la racine du disque, l’événement **ShowPicturesOnArrival** est déclenché à la place.                                                                                                                                                             |
| Utilisation de photos situées sur un dispositif de stockage de masse (hérité)                            | **ShowPicturesOnArrival**          | Événement déclenché lorsqu’un lecteur ou un volume contient un dossier DCIM, AVCHD ou PRIVATE\ACHD à la racine du disque. Si un utilisateur a activé l’option **Choisir l’action pour chaque type de média** dans le Panneau de configuration Lecture automatique, ce dernier examine un volume connecté à l’ordinateur pour déterminer le type de contenu présent sur le disque. Lorsque le système détecte des images, un événement **ShowPicturesOnArrival** est déclenché. |
| Réception de photos via le partage de proximité (appui et envoi)             | **ShowPicturesOnArrival**          | Lorsque des utilisateurs transmettent du contenu à l’aide de la fonction de proximité (appui et envoi), la lecture automatique examine les fichiers partagés pour déterminer le type de contenu. Si des images sont détectées, un événement **ShowPicturesOnArrival** est déclenché.                                                                                                                                                                         |
| Utilisation de musique située sur un dispositif de stockage de masse (hérité)                             | **PlayMusicFilesOnArrival**        | Si un utilisateur a activé l’option **Choisir l’action pour chaque type de média** dans le Panneau de configuration Lecture automatique, ce dernier examine un volume connecté au PC pour déterminer le type de contenu présent sur le disque.  Lorsque le système détecte des fichiers de musique, un événement **PlayMusicFilesOnArrival** est déclenché.                                                                                                   |
| Réception de musique via le partage de proximité (appui et envoi)              | **PlayMusicFilesOnArrival**        | Lorsque des utilisateurs transmettent du contenu à l’aide de la fonction de proximité (appui et envoi), la lecture automatique examine les fichiers partagés pour déterminer le type de contenu. Si des fichiers de musique sont détectés, un événement **PlayMusicFilesOnArrival** est déclenché.                                                                                                                                                                    |
| Utilisation de vidéos situées sur un dispositif de stockage de masse (hérité)                            | **PlayVideoFilesOnArrival**        | Si un utilisateur a activé l’option **Choisir l’action pour chaque type de média** dans le Panneau de configuration Lecture automatique, ce dernier examine un volume connecté au PC pour déterminer le type de contenu présent sur le disque. Lorsque le système détecte des fichiers vidéo, un événement **PlayVideoFilesOnArrival** est déclenché.                                                                                                   |
| Réception de vidéos via le partage de proximité (appui et envoi)             | **PlayVideoFilesOnArrival**        | Lorsque des utilisateurs transmettent du contenu à l’aide de la fonction de proximité (appui et envoi), la lecture automatique examine les fichiers partagés pour déterminer le type de contenu. Si des fichiers vidéo sont détectés, un événement **PlayVideoFilesOnArrival** est déclenché.                                                                                                                                                                    |
| Gestion d’ensembles mixtes de fichiers depuis un périphérique connecté               | **MixedContentOnArrival**          | Si un utilisateur a activé l’option **Choisir l’action pour chaque type de média** dans le Panneau de configuration Lecture automatique, ce dernier examine un volume connecté au PC pour déterminer le type de contenu présent sur le disque. Si aucun type de contenu spécifique n’est trouvé (par exemple, des images), un événement **MixedContentOnArrival** est déclenché.                                                                    |
| Gestion d’ensembles mixtes de fichiers via le partage de proximité (appui et envoi) | **MixedContentOnArrival**          | Lorsque des utilisateurs transmettent du contenu à l’aide de la fonction de proximité (appui et envoi), la lecture automatique examine les fichiers partagés pour déterminer le type de contenu. Si aucun type de contenu spécifique n’est trouvé (par exemple, des images), un événement **MixedContentOnArrival** est déclenché.                                                                                                                                  |
| Gérer de la vidéo depuis un média optique                                    | **PlayDVDMovieOnArrival**          |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    |                                    |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    | **PlayBluRayOnArrival**            |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    |                                    |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    | **PlayVideoCDMovieOnArrival**      |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    |                                    |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    | **PlaySuperVideoCDMovieOnArrival** |                                                                                                                                                                                                                                                                                                                                                                           |
| Gérer de la musique depuis un média optique                                    | **PlayCDAudioOnArrival**           |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    |                                    |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    | **PlayDVDAudioOnArrival**          |                                                                                                                                                                                                                                                                                                                                                                           |
| Lire des disques optimisés                                                | **PlayEnhancedCDOnArrival**        |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    |                                    |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    | **PlayEnhancedDVDOnArrival**       |                                                                                                                                                                                                                                                                                                                                                                           |
| Gérer des disques optiques inscriptibles                                     | **HandleCDBurningOnArrival**       |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    |                                    |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    | **HandleDVDBurningOnArrival**      |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    |                                    |                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                    | **HandleBDBurningOnArrival**       |                                                                                                                                                                                                                                                                                                                                                                           |
| Gérer un autre périphérique ou une connexion de volume                       | **UnknownContentOnArrival**        | Événement déclenché pour tous les événements si du contenu ne correspondant à aucun événement de contenu de lecture automatique est trouvé. L’utilisation de cet événement n’est pas recommandée. Vous devez seulement inscrire votre application aux événements de lecture automatique spécifiques qu’elle est capable de gérer.                                                                                                                               |

Vous pouvez spécifier que la lecture automatique déclenche l’événement de contenu de lecture automatique à l’aide de l’entrée **CustomEvent** dans le fichier autorun.inf file pour un volume. Pour plus d’informations, voir [Entrées du fichier Autorun.inf](https://msdn.microsoft.com/library/windows/desktop/cc144200).

Vous pouvez inscrire votre application en tant que gestionnaire d’événements Contenu de lecture automatique ou Périphérique de lecture automatique en ajoutant une extension au fichier package.appxmanifest de votre application. Si vous utilisez Visual Studio, vous pouvez ajouter une déclaration **Contenu de lecture automatique** ou **Périphérique de lecture automatique** dans l’onglet **Déclarations**. Si vous modifiez directement le fichier package.appxmanifest de votre application, ajoutez un élément [**Extension**](https://msdn.microsoft.com/library/windows/apps/br211400) au manifeste de votre package qui spécifie **windows.autoPlayContent** ou **windows.autoPlayDevice** en tant que **Category**. Par exemple, l’entrée suivante dans le manifeste du package ajoute une extension **Contenu de lecture automatique** pour inscrire l’application en tant que gestionnaire de l’événement **ShowPicturesOnArrival**.

```xml
  <Applications>
    <Application Id="AutoPlayHandlerSample.App">
      <Extensions>
        <Extension Category="windows.autoPlayContent">
          <AutoPlayContent>
            <LaunchAction Verb="show" ActionDisplayName="Show Pictures" 
                          ContentEvent="ShowPicturesOnArrival" />
          </AutoPlayContent>
        </Extension>
      </Extensions>
    </Application>
  </Applications>
```

 

 





<!--HONumber=Mar16_HO1-->


