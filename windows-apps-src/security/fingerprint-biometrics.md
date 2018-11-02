---
title: Empreinte digitale biométrique
description: Cet article explique comment ajouter des empreintes digitales biométriques à votre application de plateforme Windows universelle (UWP).
ms.assetid: 55483729-5F8A-401A-8072-3CD611DDFED2
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, sécurité
ms.localizationpriority: medium
ms.openlocfilehash: 6ccd2efee17c7f1205bcac6f05b016af7e6c6dae
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5929675"
---
# <a name="fingerprint-biometrics"></a>Empreinte digitale biométrique




Cet article explique comment ajouter des empreintes digitales biométriques à votre application de plateforme Windows universelle (UWP). L’inclusion d’une demande d’authentification par empreinte digitale (biométrique) lorsque l’utilisateur doit valider une action particulière renforce la sécurité de votre application. Par exemple, vous pouvez exiger une authentification par empreinte digitale avant d’autoriser un achat in-app ou avant l’accès à des ressources restreintes. L’authentification par empreinte digitale est gérée par l’intermédiaire de la classe [**UserConsentVerifier**](https://msdn.microsoft.com/library/windows/apps/dn279134) dans l’espace de noms [**Windows.Security.Credentials.UI**](https://msdn.microsoft.com/library/windows/apps/hh701356).

## <a name="check-the-device-for-a-fingerprint-reader"></a>Déterminer si l’appareil est doté d’un lecteur d’empreintes digitales


Pour savoir si l’appareil dispose d’un lecteur d’empreintes digitales, appelez la méthode [**UserConsentVerifier.CheckAvailabilityAsync**](https://msdn.microsoft.com/library/windows/apps/dn279138). Même si un appareil prend en charge l’authentification par empreinte digitale, votre application doit fournir aux utilisateurs une option dans Paramètres pour activer ou désactiver l’authentification des empreintes digitales.

```cs
public async System.Threading.Tasks.Task<string> CheckFingerprintAvailability()
{
    string returnMessage = "";

    try
    {
        // Check the availability of fingerprint authentication.
        var ucvAvailability = await Windows.Security.Credentials.UI.UserConsentVerifier.CheckAvailabilityAsync();

        switch (ucvAvailability)
        {
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.Available:
                returnMessage = "Fingerprint verification is available.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DeviceBusy:
                returnMessage = "Biometric device is busy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DeviceNotPresent:
                returnMessage = "No biometric device found.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DisabledByPolicy:
                returnMessage = "Biometric verification is disabled by policy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.NotConfiguredForUser:
                returnMessage = "The user has no fingerprints registered. Please add a fingerprint to the " +
                                "fingerprint database and try again.";
                break;
            default:
                returnMessage = "Fingerprints verification is currently unavailable.";
                break;
        }
    }
    catch (Exception ex)
    {
        returnMessage = "Fingerprint authentication availability check failed: " + ex.ToString();
    }

    return returnMessage;
}
```

## <a name="request-consent-and-return-results"></a>Demander le consentement et retourner des résultats


Pour demander à l’utilisateur son consentement par le biais d’une analyse de son empreinte digitale, appelez la méthode [**UserConsentVerifier.RequestVerificationAsync**](https://msdn.microsoft.com/library/windows/apps/dn279139). Pour que l’authentification par empreinte digitale fonctionne, l’utilisateur doit avoir au préalable ajouté une « signature par empreinte digitale » à la base de données d’empreintes digitales.

Lorsque vous appelez la méthode [**UserConsentVerifier.RequestVerificationAsync**](https://msdn.microsoft.com/library/windows/apps/dn279139), l’utilisateur se voit présenter une boîte de dialogue modale lui demandant une analyse de son empreinte digitale. Vous pouvez inclure un message dans la méthode **UserConsentVerifier.RequestVerificationAsync** qui est présenté à l’utilisateur dans la boîte de dialogue modale, comme vous pouvez le voir sur l’image suivante.

```cs
private async System.Threading.Tasks.Task<string> RequestConsent(string userMessage)
{
    string returnMessage;

    if (String.IsNullOrEmpty(userMessage))
    {
        userMessage = "Please provide fingerprint verification.";
    }

    try
    {
        // Request the logged on user's consent via fingerprint swipe.
        var consentResult = await Windows.Security.Credentials.UI.UserConsentVerifier.RequestVerificationAsync(userMessage);

        switch (consentResult)
        {
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.Verified:
                returnMessage = "Fingerprint verified.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DeviceBusy:
                returnMessage = "Biometric device is busy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DeviceNotPresent:
                returnMessage = "No biometric device found.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DisabledByPolicy:
                returnMessage = "Biometric verification is disabled by policy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.NotConfiguredForUser:
                returnMessage = "The user has no fingerprints registered. Please add a fingerprint to the " +
                                "fingerprint database and try again.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.RetriesExhausted:
                returnMessage = "There have been too many failed attempts. Fingerprint authentication canceled.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.Canceled:
                returnMessage = "Fingerprint authentication canceled.";
                break;
            default:
                returnMessage = "Fingerprint authentication is currently unavailable.";
                break;
        }
    }
    catch (Exception ex)
    {
        returnMessage = "Fingerprint authentication failed: " + ex.ToString();
    }

    return returnMessage;
}
```