---
ms.assetid: BF877F23-1238-4586-9C16-246F3F25AE35
Cet article décrit comment ajouter la diffusion en continu adaptative de contenu multimédia avec la protection de contenu Microsoft PlayReady à une application UWP.
Diffusion en continu adaptative avec PlayReady
---

# Diffusion en continu adaptative avec PlayReady

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

\[Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.\]

Cet article décrit comment ajouter la diffusion en continu adaptative de contenu multimédia avec la protection de contenu Microsoft PlayReady à une application UWP. Cette fonctionnalité prend actuellement en charge la lecture de contenu vidéo en flux continu HTTP (HLS) et de contenu à diffusion en continu dynamique sur HTTP (DASH).

Cet article traite uniquement des aspects de la diffusion en continu adaptative propre à PlayReady. Pour des informations plus générales sur l’implémentation de la diffusion en continu adaptative, voir [Diffusion en continu adaptative](adaptive-streaming.md).

Vous aurez besoin des instructions using suivantes :

```csharp
using LicenseRequest;
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Runtime.InteropServices;
using System.Threading.Tasks;
using Windows.Foundation.Collections;
using Windows.Media.Protection;
using Windows.Media.Protection.PlayReady;
using Windows.Media.Streaming.Adaptive;
using Windows.UI.Xaml.Controls;
```

L’espace de noms **LicenseRequest** provient de **CommonLicenseRequest.cs**, un fichier PlayReady fourni par Microsoft aux détenteurs de licences.

Vous devez déclarer plusieurs variables globales :

```csharp
private AdaptiveMediaSource ams = null;
private MediaProtectionManager protectionManager = null;
private string playReadyLicenseUrl = "";
private string playReadyChallengeCustomData = "";
```

Vous devrez également déclarer la constante suivante :

```csharp
private const int MSPR_E_CONTENT_ENABLING_ACTION_REQUIRED = -2147174251;
```

## Configuration du MediaProtectionManager

Pour ajouter la protection de contenu PlayReady à votre application UWP, vous devez configurer un objet [MediaProtectionManager](https://msdn.microsoft.com/library/windows/apps/br207040). Cette opération s’effectue lors de l’initialisation de votre objet [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912).

Le code suivant définit un objet [MediaProtectionManager](https://msdn.microsoft.com/library/windows/apps/br207040) :

```csharp
private void SetUpProtectionManager(ref MediaElement mediaElement)
{
    protectionManager = new MediaProtectionManager();

    protectionManager.ComponentLoadFailed += 
        new ComponentLoadFailedEventHandler(ProtectionManager_ComponentLoadFailed);

    protectionManager.ServiceRequested += 
        new ServiceRequestedEventHandler(ProtectionManager_ServiceRequested);

    PropertySet cpSystems = new PropertySet();

    cpSystems.Add(
        "{F4637010-03C3-42CD-B932-B48ADF3A6A54}", 
        "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput");

    protectionManager.Properties.Add("Windows.Media.Protection.MediaProtectionSystemIdMapping", cpSystems);

    protectionManager.Properties.Add(
        "Windows.Media.Protection.MediaProtectionSystemId", 
        "{F4637010-03C3-42CD-B932-B48ADF3A6A54}");

    protectionManager.Properties.Add(
        "Windows.Media.Protection.MediaProtectionContainerGuid", 
        "{9A04F079-9840-4286-AB92-E65BE0885F95}");

    mediaElement.ProtectionManager = protectionManager;
}
```

Ce code peut simplement être copié sur votre application, dans la mesure où il est obligatoire pour la configuration de la protection de contenu.

L’événement [ComponentLoadFailed](https://msdn.microsoft.com/library/windows/apps/br207041) est déclenché en cas d’échec de chargement des données binaires. Nous devons ajouter un gestionnaire d’événements pour gérer ceci, afin de signaler que le chargement ne s’est pas terminé :

```csharp
private void ProtectionManager_ComponentLoadFailed(
    MediaProtectionManager sender, 
    ComponentLoadFailedEventArgs e)
{
    e.Completion.Complete(false);
}
```

De même, nous devons ajouter un gestionnaire d’événements pour l’événement [ServiceRequested](https://msdn.microsoft.com/library/windows/apps/br207045), qui est déclenché lorsqu’un service est demandé. Ce code vérifie le type de demande et réagit de façon appropriée :

```csharp
private async void ProtectionManager_ServiceRequested(
    MediaProtectionManager sender, 
    ServiceRequestedEventArgs e)
{
    if (e.Request is PlayReadyIndividualizationServiceRequest)
    {
        PlayReadyIndividualizationServiceRequest IndivRequest = 
            e.Request as PlayReadyIndividualizationServiceRequest;

        bool bResultIndiv = await ReactiveIndivRequest(IndivRequest, e.Completion);
    }
    else if (e.Request is PlayReadyLicenseAcquisitionServiceRequest)
    {
        PlayReadyLicenseAcquisitionServiceRequest licenseRequest = 
            e.Request as PlayReadyLicenseAcquisitionServiceRequest;

        LicenseAcquisitionRequest(
            licenseRequest, 
            e.Completion, 
            playReadyLicenseUrl, 
            playReadyChallengeCustomData);
    }
}
```

## Demandes de service d’individualisation

Le code suivant effectue de manière réactive une demande de service d’individualisation PlayReady. Nous transmettons la demande en tant que paramètre à la fonction. Nous insérons l’appel dans un bloc try/catch, et s’il n’y a aucune exception, nous supposons que la demande s’est déroulée correctement :

```csharp
async Task<bool> ReactiveIndivRequest(
    PlayReadyIndividualizationServiceRequest IndivRequest, 
    MediaProtectionServiceCompletion CompletionNotifier)
{
    bool bResult = false;
    Exception exception = null;

    try
    {
        await IndivRequest.BeginServiceRequest();
    }
    catch (Exception ex)
    {
        exception = ex;
    }
    finally
    {
        if (exception == null)
        {
            bResult = true;
        }
        else
        {
            COMException comException = exception as COMException;
            if (comException != null &amp;&amp; comException.HResult == MSPR_E_CONTENT_ENABLING_ACTION_REQUIRED)
            {
                IndivRequest.NextServiceRequest();
            }
        }
    }

    if (CompletionNotifier != null) CompletionNotifier.Complete(bResult);
    return bResult;
}
```

Par ailleurs, nous pouvons effectuer de manière proactive une demande de service d’individualisation, auquel cas nous appelons la fonction ci-dessous à la place du code appelant `ReactiveIndivRequest` dans `ProtectionManager_ServiceRequested` :

```csharp
async void ProActiveIndivRequest()
{
    PlayReadyIndividualizationServiceRequest indivRequest = new PlayReadyIndividualizationServiceRequest();
    bool bResultIndiv = await ReactiveIndivRequest(indivRequest, null);
}
```

## Demandes de service d’acquisition de licence

Si au lieu de cela, il s’agit d’une demande [PlayReadyLicenseAcquisitionServiceRequest](https://msdn.microsoft.com/library/windows/apps/dn986285), nous appelons la fonction ci-dessous pour demander et obtenir la licence PlayReady. Nous demandons à l’objet MediaProtectionServiceCompletion transmis d’indiquer si la demande a réussi ou non, et nous terminons la demande :

```csharp
async void LicenseAcquisitionRequest(
    PlayReadyLicenseAcquisitionServiceRequest licenseRequest, 
    MediaProtectionServiceCompletion CompletionNotifier, 
    string Url, 
    string ChallengeCustomData)
{
    bool bResult = false;
    string ExceptionMessage = string.Empty;

    try
    {
        if (!string.IsNullOrEmpty(Url))
        {
            if (!string.IsNullOrEmpty(ChallengeCustomData))
            {
                System.Text.UTF8Encoding encoding = new System.Text.UTF8Encoding();
                byte[] b = encoding.GetBytes(ChallengeCustomData);
                licenseRequest.ChallengeCustomData = Convert.ToBase64String(b, 0, b.Length);
            }

            PlayReadySoapMessage soapMessage = licenseRequest.GenerateManualEnablingChallenge();

            byte[] messageBytes = soapMessage.GetMessageBody();
            HttpContent httpContent = new ByteArrayContent(messageBytes);

            IPropertySet propertySetHeaders = soapMessage.MessageHeaders;

            foreach (string strHeaderName in propertySetHeaders.Keys)
            {
                string strHeaderValue = propertySetHeaders[strHeaderName].ToString();

                if (strHeaderName.Equals("Content-Type", StringComparison.OrdinalIgnoreCase))
                {
                    httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse(strHeaderValue);
                }
                else
                {
                    httpContent.Headers.Add(strHeaderName.ToString(), strHeaderValue);
                }
            }

            CommonLicenseRequest licenseAcquision = new CommonLicenseRequest();

            HttpContent responseHttpContent = 
                await licenseAcquision.AcquireLicense(new Uri(Url), httpContent);

            if (responseHttpContent != null)
            {
                Exception exResult = licenseRequest.ProcessManualEnablingResponse(
                                         await responseHttpContent.ReadAsByteArrayAsync());

                if (exResult != null)
                {
                    throw exResult;
                }
                bResult = true;
            }
            else
            {
                ExceptionMessage = licenseAcquision.GetLastErrorMessage();
            }
        }
        else
        {
            await licenseRequest.BeginServiceRequest();
            bResult = true;
        }
    }
    catch (Exception e)
    {
        ExceptionMessage = e.Message;
    }

    CompletionNotifier.Complete(bResult);
}
```

## Initialisation d’AdaptiveMediaSource

Enfin, vous aurez besoin d’une fonction pour initialiser [AdaptiveMediaSource](https://msdn.microsoft.com/library/windows/apps/dn946912), créée à partir d’un [URI](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) et d’un [MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926) donnés. L’**URI** doit être le lien vers le fichier multimédia (TLS ou DASH) ; l’élément **MediaElement** doit être défini dans votre code XAML.

```csharp
async private void InitializeAdaptiveMediaSource(System.Uri uri, MediaElement m)
{
    AdaptiveMediaSourceCreationResult result = await AdaptiveMediaSource.CreateFromUriAsync(uri);
    if (result.Status == AdaptiveMediaSourceCreationStatus.Success)
    {
        ams = result.MediaSource;
        SetUpProtectionManager(ref m);
        m.SetMediaStreamSource(ams);
    }
    else
    {
        // Error handling
    }
}
```

Vous pouvez appeler cette fonction dans n’importe quel événement gérant le début de la diffusion en continu adaptative, par exemple, dans un événement click du bouton.

 

 






<!--HONumber=Mar16_HO1-->


