---
author: TylerMSFT
Description: 'Cette rubrique présente des exemples de tâches de codage nécessaires pour réaliser certains des scénarios EDP relatifs aux flux et mémoires tampons les plus courants.'
MS-HAID: 'dev\_files.use\_edp\_to\_protect\_streams\_and\_buffers'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: 'Utiliser l’EDP pour protéger les flux et les mémoires tampons'
---

# Utiliser l’EDP pour protéger les flux et les mémoires tampons

__Remarque__ La stratégie de protection des données d’entreprise (EDP) ne peut pas être appliquée sur Windows 10, version 1511 (build 10586) ou antérieure.

Cette rubrique présente des exemples de tâches de codage nécessaires pour réaliser certains des scénarios EDP relatifs aux flux et mémoires tampons les plus courants. Pour un aperçu complet du point de vue des développeurs de la manière dont la fonctionnalité EDP est liée aux fichiers, aux flux, au Presse-papiers, à la mise en réseau, aux tâches en arrière-plan et à la protection des données verrouillées, voir [protection des données d’entreprise (EDP)](../enterprise/edp-hub.md).

**Remarque** L’[exemple de protection des données d’entreprise (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409) couvre nombre des scénarios expliqués dans cette rubrique.

## Prérequis


-   **Préparation pour la configuration de la fonctionnalité EDP**

    Voir [Configurer votre ordinateur pour EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP).

-   **Impliquez-vous dans la création d’une application spécifique aux entreprises**

    Une application qui assure de manière autonome que les données d’entreprise restent sous le contrôle des entreprises est véritablement spécifique aux entreprises. Une application spécifique aux entreprises est plus puissante, plus intelligente, plus flexible et plus fiable qu’une application ordinaire. Votre application indique au système qu’elle est spécifique en déclarant la fonctionnalité **enterpriseDataPolicy** restreinte. Mais configurer une fonctionnalité ne suffit pas à établir la spécificité. Pour en savoir plus, voir [Applications spécifiques aux entreprises](../enterprise/edp-hub.md#enterprise-enlightened-apps).

-   **Comprendre la programmation asynchrone pour les applications pour la plateforme Windows universelle (UWP)**

    Pour savoir comment écrire des applications asynchrones en C\# ou Visual Basic, voir [Appeler des API asynchrones en C\# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Pour savoir comment écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

## Protéger un flux de données sous une identité d’entreprise


**Remarque** Chaque fois que vous protégez un flux ou une mémoire tampon, il est vivement recommandé de vous abonner à l’événement [**ProtectionPolicyManager.PolicyChanged**](https://msdn.microsoft.com/library/windows/apps/mt608411) afin que votre application sache lorsque la fonctionnalité EDP est désactivée sur l’appareil. Le cas échéant, vous devez ôter la protection des flux et des mémoires tampons. Tous les flux ou mémoires tampons que vous laissez protégés peuvent être révoqués à condition que l’utilisateur annule l’inscription de l’appareil dans la gestion des périphériques mobiles (GPM). Si la fonctionnalité EDP a été désactivée lors de la création de la ressource, cette révocation est inappropriée. La suppression de la protection des ressources lorsque la fonctionnalité EDP est désactivée empêche cela.



Dans ce scénario, votre application a accès à un flux non protégé qui contient des données d’entreprise. Pour s’assurer que ce flux est protégé lors de son transfert vers et depuis l’appareil, votre application protège les données sous l’identité de l’entreprise à laquelle elle appartient. Cela permet d’effacer les données si l’entreprise le demande. Pour déterminer ultérieurement s’il convient d’appeler une méthode de suppression de protection dans un flux, l’application doit conserver l’état qui indique si le flux a été protégé, raison pour laquelle la fonction de cet exemple de code retourne cet état. Si l’identité transmise n’est pas gérée, ou si l’application n’est pas autorisée pour l’identité, le flux n’est pas protégé, et un état « Non protégé » est renvoyé à partir de l’appel à [**DataProtectionManager.ProtectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706021).

```CSharp
using Windows.Storage.Streams;
using Windows.Security.EnterpriseData;

private async System.Threading.Tasks.Task<bool> ProtectAStream
    (InMemoryRandomAccessStream unprotectedInMemoryRandomAccessStream, string identity,
    InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    IInputStream unprotectedStream = unprotectedInMemoryRandomAccessStream.GetInputStreamAt(0);
    IOutputStream protectedStream = protectedInMemoryRandomAccessStream.GetOutputStreamAt(0);

    // Protect "inputStream".
    DataProtectionInfo info = 
        await DataProtectionManager.ProtectStreamAsync(unprotectedStream, identity, protectedStream);

    // Indicate to the caller whether the stream was protected successfully. It will only be protected if
    // the identity is managed AND this app is allowed for this identity. Similar to buffers, this status
    // must be stored by the app. UnprotectStreamAsync must only be called if the stream was protected
    // successfully earlier.

    return (info.Status == DataProtectionStatus.Protected);
}
```

Pour montrer comment utiliser une méthode comme celle de l’exemple de code ci-dessus, voici une méthode d’assistance qui l’utilise pour convertir une chaîne en un flux non protégé, puis pour protéger le flux.

```CSharp
using Windows.Storage.Streams;

private async System.Threading.Tasks.Task<bool> ProtectStringAsStreamAsync
    (string unprotectedEnterpriseData, string identity, 
    InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    using (var unprotectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream())
    {
        using (var dataWriter = new DataWriter(unprotectedInMemoryRandomAccessStream))
        {
            dataWriter.WriteString(unprotectedEnterpriseData);
            await dataWriter.StoreAsync();
            await dataWriter.FlushAsync();
            return await this.ProtectAStream(unprotectedInMemoryRandomAccessStream,
                identity, protectedInMemoryRandomAccessStream);
        }
    }
}
```

## Récupérer l’état d’un flux


Dans ce scénario, votre application a précédemment protégé un flux auquel vous devez empêcher tout accès non autorisé. Pour récupérer à nouveau le contenu du flux si nécessaire, votre application peut vérifier l’état du flux.

Notez que l’état d’un flux est également retourné à partir de [**DataProtectionManager.UnprotectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706023). Cette API ne retourne jamais l’état « Non protégé », car il nécessite que la ressource d’entrée soit protégée (il n’est pas possible de vérifier de manière fiable qu’une ressource n’est plus protégée). Sachez que si votre code compare le résultat à l’état « Non protégé », cela suggère la présence d’une faille dans la conception. Cela indique que votre code a perdu toute trace indiquant si le flux est protégé.

```CSharp
using Windows.Storage.Streams;
using Windows.Security.EnterpriseData;

private async void CheckProtectedStreamStatus(IInputStream protectedStream)
{
    DataProtectionInfo dataProtectionInfo = 
        await DataProtectionManager.GetStreamProtectionInfoAsync(protectedStream);

    if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
    {
        // Code goes here to handle this situation. Perhaps, show UI
        // saying that the user's data has been revoked.
    }
    else if (dataProtectionInfo.Status != DataProtectionStatus.Protected)
    {
        // Code goes here to handle the unexpected protection status.
    }
}
```

## Ôter la protection d’un flux de données


Dans ce scénario, votre application souhaite ôter la protection d’un flux qui était protégé auparavant. Cet exemple de code prend un flux protégé (notez que le flux doit être protégé pour que ce processus fonctionne) et ôte sa protection en appelant [**DataProtectionManager.UnprotectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706023). Il lit ensuite une chaîne en dehors du flux et la retourne.

```CSharp
using Windows.Storage.Streams;

private async System.Threading.Tasks.Task<string> UnprotectStreamIntoStringAsync
    (InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    using (var unprotectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream())
    {
        DataProtectionInfo dataProtectionInfo = 
            await DataProtectionManager.UnprotectStreamAsync(protectedInMemoryRandomAccessStream, 
            unprotectedInMemoryRandomAccessStream);

        using (var inputStream = unprotectedInMemoryRandomAccessStream.GetInputStreamAt(0))
        {
            using (var dataReader = new DataReader(inputStream))
            {
                await dataReader.LoadAsync((uint)unprotectedInMemoryRandomAccessStream.Size);
                return dataReader.ReadString((uint)unprotectedInMemoryRandomAccessStream.Size);
            }
        }
    }
}
```

Pour montrer comment vous pouvez utiliser les méthodes d’assistance données jusqu’à présent, voici un gestionnaire d’événements qui prend une chaîne à partir d’une zone de texte, écrit la chaîne dans un flux, protège le flux, ôte la protection de ce flux (s’il a été correctement protégé) et lit enfin la chaîne à partir du flux non protégé avant de l’afficher dans l’interface utilisateur.

```CSharp
using Windows.Storage.Streams;

private async void ProtectAndThenUnprotectStream_Click(object sender, RoutedEventArgs e)
{
    var protectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream();
    bool isStreamProtected = await this.ProtectStringAsStreamAsync
        (this.enterpriseDataTextBox.Text, MainPage.IDENTITY, protectedInMemoryRandomAccessStream);

    // Only unprotect the stream if we're sure that the stream actually was protected.
    if (isStreamProtected)
    {
        this.resultDataTextBlock.Text = 
            await this.UnprotectStreamIntoStringAsync(protectedInMemoryRandomAccessStream);
    }
}
```

## Récupérer l’état d’un tampon de données statiques


Dans ce scénario, votre application a précédemment protégé une mémoire tampon à laquelle vous devez empêcher tout accès non autorisé. Pour récupérer à nouveau le contenu de la mémoire tampon si nécessaire, votre application peut vérifier l’état de la mémoire tampon.

Notez que l’état d’une mémoire tampon est également retourné à partir de [**DataProtectionManager.UnprotectAsync**](https://msdn.microsoft.com/library/windows/apps/dn706022). Cette API ne retourne jamais l’état « Non protégé », car elle nécessite la protection de la ressource d’entrée.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

private async void CheckProtectedBufferStatus(IBuffer protectedData)
{
    DataProtectionInfo dataProtectionInfo = 
        await DataProtectionManager.GetProtectionInfoAsync(protectedData);

    if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
    {
        // Code goes here to handle this situation, perhaps show UI
        // saying that the user's data has been revoked.
    }
    else if (dataProtectionInfo.Status != DataProtectionStatus.Protected)
    {
        // Code goes here to handle the unexpected protection status.
    }
}
```

## Protéger les données statiques récupérées à partir d’une ressource d’entreprise


Ce scénario est le même que pour les exemples de code de flux sauf qu’il utilise une mémoire tampon de données. Là encore, vous devez maintenir l’état qui indique si la mémoire tampon a été protégée, comme indiqué. Si l’identité transmise n’est pas gérée, ou si l’application n’est pas autorisée pour l’identité, la mémoire tampon n’est pas protégée, et un état « Non protégé » est renvoyé à partir de l’appel à [**DataProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn706020).

```CSharp
using Windows.Security.Cryptography;
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

...

private IBuffer data = null;
private bool isProtected = false;

void StoreBuffer(IBuffer data, bool isProtected)
{
    this.data = data;
    this.isProtected = isProtected;
}

IBuffer GetStoredBuffer(out bool isProtected)
{
    isProtected = this.isProtected;
    return this.data;
}

private string identity = "contoso.com";

private async void ProtectAndThenUnprotectBuffer_Click(object sender, RoutedEventArgs e)
{
    BinaryStringEncoding encoding = BinaryStringEncoding.Utf8;
    IBuffer inputData = CryptographicBuffer.ConvertStringToBinary
        (this.enterpriseDataTextBox.Text, encoding);
    BufferProtectUnprotectResult result =
        await DataProtectionManager.ProtectAsync(inputData, this.identity);

    // Record whether the buffer was protected successfully. It will only be protected if
    // the identity is managed AND this app is allowed for this identity. This status
    // must be stored by the app. UnprotectAsync must only be called if the buffer was
    // protected successfully earlier.
    bool isBufferProtected = 
        (result.ProtectionInfo.Status == DataProtectionStatus.Protected);
    IBuffer outputData = result.Buffer;

    // Store the data away...
    this.StoreBuffer(outputData, isBufferProtected);

    // ...and then later retrieve it.
    outputData = this.GetStoredBuffer(out isBufferProtected);

    string storedString = string.Empty;

    if (isBufferProtected)
    {
        result = await DataProtectionManager.UnprotectAsync(outputData);

        if (result.ProtectionInfo.Status != DataProtectionStatus.Unprotected)
        {
            // Code goes here to handle the situation where the buffer
            // is no longer accessible.
            return;
        }
        outputData = result.Buffer;
    }
    this.resultDataTextBlock.Text = CryptographicBuffer.ConvertBinaryToString(encoding, outputData);
}
```

## Activer l’application de la stratégie d’interface utilisateur en fonction de l’identité de protection d’un flux ou d’une mémoire tampon


Lorsque votre application est sur le point d’afficher le contenu d’un flux ou d’une mémoire tampon protégés dans son interface utilisateur, elle doit activer l’application de la stratégie d’interface utilisateur en fonction de l’identité de protection de la ressource. Vous devez interroger les informations de protection de la ressource et activer l’application de stratégie d’interface utilisateur du système à partir de l’identité récupérée.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

...

private async void EnableUIPolicyFromProtectedBuffer(IBuffer buffer)
{
    DataProtectionInfo protectionInfo = 
        await DataProtectionManager.GetProtectionInfoAsync(buffer);

    if (protectionInfo.Status != DataProtectionStatus.Protected)
    {
        // In this case, the app has lost access to the buffer
        // (ProtectedToOtherIdentity, Revoked). This must be handled.
        // 'Unprotected' is never returned for GetProtectionInfoAsync().
        return;
    }

    ProtectionPolicyManager.TryApplyProcessUIPolicy(protectionInfo.Identity);
}

```

**Remarque** Cet article s’adresse aux développeurs de Windows 10 qui créent des applications pour la plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Rubriques connexes


[exemple de protection des données d’entreprise (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Espace de noms Windows.Security.EnterpriseData**](https://msdn.microsoft.com/library/windows/apps/dn279153)

 

 





<!--HONumber=May16_HO2-->


