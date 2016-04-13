---
ms.assetid: 374D1983-60E0-4E18-ABBB-04775BAA0F0D
Numériser à partir de votre application
Découvrez ici comment numériser du contenu à partir de votre application à l’aide d’un scanneur à plat, à chargeur ou configuré automatiquement.
---
# Numériser à partir de votre application

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

** API importantes **

-   [**Windows.Devices.Scanners**](https://msdn.microsoft.com/library/windows/apps/Dn264250)
-   [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)
-   [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381)

Découvrez ici comment numériser du contenu à partir de votre application à l’aide d’un scanneur à plat, à chargeur ou configuré automatiquement.

**Important** Les API [**Windows.Devices.Scanners**](https://msdn.microsoft.com/library/windows/apps/Dn264250) font partie de la [famille d’appareils](https://msdn.microsoft.com/library/windows/apps/Dn894631) bureautiques. Les applications peuvent utiliser ces API uniquement sur la version bureau de Windows 10.

Pour numériser à partir de votre application, vous devez d’abord répertorier les scanneurs disponibles en déclarant un nouvel objet [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) et en récupérant le type [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381). Seuls les scanneurs installés localement avec des pilotes WIA sont répertoriés et disponibles pour votre application.

Une fois que votre application a répertorié les scanneurs disponibles, elle peut utiliser les paramètres de numérisation configurés automatiquement en fonction du type de scanneur, ou numériser en utilisant simplement le scanneur à plat ou à chargeur disponible. Pour utiliser les paramètres configurés automatiquement, le scanneur doit être activé pour la configuration automatique et ne doit pas posséder à la fois un dispositif de numérisation à plat et un dispositif de numérisation à chargeur. Pour plus d’informations, consultez [Numérisation configurée automatiquement](https://msdn.microsoft.com/library/windows/hardware/Ff539393).

## Énumérer les scanneurs disponibles

Windows ne détecte pas les scanneurs automatiquement. Vous devez effectuer cette étape pour que votre application communique avec le scanneur. Dans cet exemple, l’énumération des périphériques de numérisation est effectuée à l’aide de l’espace de noms [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459).

1.  Dans un premier temps, ajoutez les instructions « using » ci-après à votre fichier de définition de classe.

``` csharp
    using Windows.Devices.Enumeration;
    using Windows.Devices.Scanners;
```

2.  Ensuite, implémentez un observateur d’appareil pour démarrer l’énumération des scanneurs. Pour plus d’informations, voir [Énumérer les appareils](enumerate-devices.md).

```csharp
    void InitDeviceWatcher()
    {
       // Create a Device Watcher class for type Image Scanner for enumerating scanners
       scannerWatcher = DeviceInformation.CreateWatcher(DeviceClass.ImageScanner);

       scannerWatcher.Added += OnScannerAdded;
       scannerWatcher.Removed += OnScannerRemoved;
       scannerWatcher.EnumerationCompleted += OnScannerEnumerationComplete;
    }
```

3.  Créez un gestionnaire d’événements pour l’ajout d’un scanneur.

```csharp
    private async void OnScannerAdded(DeviceWatcher sender,  DeviceInformation deviceInfo)
    {
       await
       MainPage.Current.Dispatcher.RunAsync(
             Windows.UI.Core.CoreDispatcherPriority.Normal,
             () =&gt;
             {
                MainPage.Current.NotifyUser(String.Format(&quot;Scanner with device id {0} has been added&quot;, deviceInfo.Id), NotifyType.StatusMessage);

                // search the device list for a device with a matching device id
                ScannerDataItem match = FindInList(deviceInfo.Id);

                // If we found a match then mark it as verified and return
                if (match != null)
                {
                   match.Matched = true;
                   return;
                }

                // Add the new element to the end of the list of devices
                AppendToList(deviceInfo);
             }
       );
    }
```

## Numériser

1.  **Obtenir un objet ImageScanner**

Pour chaque type d’énumération [**ImageScannerScanSource**](https://msdn.microsoft.com/library/windows/apps/Dn264238), que ce soit **Default**, **AutoConfigured**, **Flatbed** ou **Feeder**, vous devez d’abord créer un objet [**ImageScanner**](https://msdn.microsoft.com/library/windows/apps/Dn263806) en appelant la méthode [**ImageScanner.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.scanners.imagescanner.fromidasync), comme suit.

 ```csharp
    ImageScanner myScanner = await ImageScanner.FromIdAsync(deviceId);
 ```

2.  **Effectuer une numérisation simple**

Pour une numérisation avec les paramètres par défaut, votre application s’appuie sur l’espace de noms [**Windows.Devices.Scanners**](https://msdn.microsoft.com/library/windows/apps/Dn264250) pour sélectionner un scanneur et effectuer la numérisation à partir de cette source. Aucun paramètre de numérisation n’est modifié. Les scanneurs possibles sont les scanneurs configurés automatiquement, à plat ou à chargeur. En règle générale, ce type de numérisation fonctionne correctement, même si la numération est effectuée à partir d’une source incorrecte, par exemple un dispositif à plat au lieu d’un chargeur.

**Remarque** Si l’utilisateur place le document à numériser dans le chargeur, le scanneur effectue la numérisation à partir du dispositif à plat. S’il essaie de numériser à partir d’un chargeur vide, la numérisation ne génère aucun fichier numérisé.
 
```csharp
    var result = await myScanner.ScanFilesToFolderAsync(ImageScannerScanSource.Default, 
        folder).AsTask(cancellationToken.Token, progress);
```

3.  **Numériser à partir d’une source configurée automatiquement, à plat ou à chargeur**

Votre application peut utiliser la [numérisation configurée automatiquement](https://msdn.microsoft.com/library/windows/hardware/Ff539393) du périphérique pour bénéficier des paramètres de numérisation les plus performants. Grâce à cette option, le périphérique peut lui-même déterminer les meilleurs paramètres de numérisation, comme le mode couleur et la résolution de la numérisation, en fonction du contenu numérisé. L’appareil sélectionne les paramètres de numérisation au moment de l’exécution pour chaque nouveau travail de numérisation.

**Remarque** Comme tous les scanneurs ne prennent pas en charge cette fonctionnalité, l’application doit vérifier si le scanneur est compatible avec ce paramétrage avant de lancer l’opération.

Dans cet exemple, l’application vérifie si le scanneur prend en charge la configuration automatique, puis effectue la numérisation. Pour spécifier un scanneur à plat ou à chargeur, remplacez simplement **AutoConfigured** par **Flatbed** ou **Feeder**.

```csharp
    if (myScanner.IsScanSourceSupported(ImageScannerScanSource.AutoConfigured))
    {
        ...
        // Scan API call to start scanning with Auto-Configured settings. 
        var result = await myScanner.ScanFilesToFolderAsync(
            ImageScannerScanSource.AutoConfigured, folder).AsTask(cancellationToken.Token, progress);
        ...
    }
```

## Obtenir un aperçu de la numérisation

Vous pouvez ajouter du code pour obtenir un aperçu de la numérisation avant de l’enregistrer dans un dossier. Dans l’exemple ci-après, l’application vérifie si le scanneur **Flatbed** prend en charge la fonctionnalité d’aperçu, puis génère un aperçu de la numérisation.

```csharp
if (myScanner.IsPreviewSupported(ImageScannerScanSource.Flatbed))
{
    rootPage.NotifyUser(&quot;Scanning&quot;, NotifyType.StatusMessage);
                // Scan API call to get preview from the flatbed.
                var result = await myScanner.ScanPreviewToStreamAsync(
                    ImageScannerScanSource.Flatbed, stream);
```

## Annuler la numérisation

Vous pouvez permettre aux utilisateurs d’annuler un travail de numérisation en cours comme ceci.

```csharp
void CancelScanning()
{
    if (ModelDataContext.ScenarioRunning)
    {
        if (cancellationToken != null)
        {
            cancellationToken.Cancel();
        }                
        DisplayImage.Source = null;
        ModelDataContext.ScenarioRunning = false;
        ModelDataContext.ClearFileList();
    }
}
```

## Numériser avec indication de la progression

1.  Créez un objet **System.Threading.CancellationTokenSource**.

```csharp
cancellationToken = new CancellationTokenSource();
```

2.  Configurez le gestionnaire d’événements de progression et récupérez la progression de la numérisation.

```csharp
    rootPage.NotifyUser(&quot;Scanning&quot;, NotifyType.StatusMessage);
    var progress = new Progress&lt;UInt32&gt;(ScanProgress);
```

## Numériser vers la bibliothèque d’images

Grâce à la classe [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/BR207881), les utilisateurs peuvent numériser dynamiquement vers n’importe quel dossier, mais vous devez déclarer la fonctionnalité *Bibliothèque d’images* dans le manifeste pour qu’ils puissent numériser dans ce dossier. Pour plus d’informations sur les fonctionnalités d’application, voir [Déclarations des fonctionnalités d’application](https://msdn.microsoft.com/library/windows/apps/Mt270968).



<!--HONumber=Mar16_HO1-->


