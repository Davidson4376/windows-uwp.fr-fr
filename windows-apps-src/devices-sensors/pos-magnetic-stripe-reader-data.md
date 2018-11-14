---
author: eliotcowley
title: Obtenir et comprendre les données de bande magnétique
description: Découvrez comment obtenir et interpréter les données à partir d’une bande magnétique.
ms.author: elcowle
ms.date: 10/04/2018
ms.topic: article
keywords: Windows 10, uwp, point de service, PDV, lecteur de bande magnétique
ms.localizationpriority: medium
ms.openlocfilehash: a130243fb5a77277e4e8a326316cd30bab2cd96e
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6183715"
---
# <a name="obtain-and-understand-magnetic-stripe-data"></a>Obtenir et comprendre les données de bande magnétique

Une fois que vous avez configuré votre lecteur de bande magnétique dans votre application en utilisant les étapes décrites dans la [mise en route avec le Point de Service](pos-basics.md), vous êtes prêt à commencer à obtenir des données à partir de celui-ci.

## <a name="subscribe-to-datareceived-events"></a>S’abonner à * DataReceived événements

Chaque fois que le lecteur reconnaît une carte balayée, il déclenche un des trois événements:

* [Événement de AamvaCardDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): se produit lorsqu’une carte automobile est balayée.
* [Événement de BankCardDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): se produit lorsqu’une carte bancaire est balayée.
* [Événement de VendorSpecificDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.vendorspecificdatareceived): se produit lorsqu’une carte spécifique au fournisseur est balayée.

Votre application ne doit s’abonner aux événements qui sont pris en charge par le lecteur de bande magnétique. Vous pouvez voir les types de cartes sont pris en charge avec [MagneticStripeReader.SupportedCardTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.supportedcardtypes
).

Le code suivant illustre l’abonnement à trois ***DataReceived** événements:

```cs
private void SubscribeToEvents(ClaimedMagneticStripeReader claimedReader, MagneticStripeReader reader)
{
    foreach (var type in reader.SupportedCardTypes)
    {
        if (item == MagneticStripeReaderCardTypes.Aamva)
        {
            claimedReader.AamvaCardDataReceived += Reader_AamvaCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.Bank)
        {
            claimedReader.BankCardDataReceived += Reader_BankCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.ExtendedBase)
        {
            claimedReader.VendorSpecificDataReceived += Reader_VendorSpecificDataReceived;
        }
    }
}
```

Le Gestionnaire d’événements recevront le [ClaimedMagneticStripeReader](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader) et un objet *d’arguments* , dont le type varie en fonction de l’événement:

* Événement de **AamvaCardDataReceived** : [MagneticStripeReaderAamvaCardDataReceivedEventArgs classe](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* Événement de **BankCardDataReceived** : [MagneticStripeReaderBankCardDataReceivedEventArgs classe](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* Événement de **VendorSpecificDataReceived** : [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs classe](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)

## <a name="get-the-data"></a>Obtenir les données

Pour les événements **AamvaCardDataReceived** et **BankCardDataReceived** , vous pouvez obtenir des données directement à partir de l’objet *arguments* . L’exemple suivant illustre l’obtention de plusieurs propriétés et de les affecter à des variables de membre:

```cs
private string _accountNumber;
private string _expirationDate;
private string _firstName;

private void Reader_BankCardDataReceived(
    ClaimedMagneticStripeReader sender, 
    MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    _accountNumber = args.AccountNumber;
    _expirationDate = args.ExpirationDate;
    _firstName = args.FirstName;
    // etc...
}
```

Toutefois, certaines données, y compris toutes les données à partir de l’événement **VendorSpecificDataReceived** , doivent être récupérées par le biais de l’objet de **Report** , qui est une propriété du paramètre *args* . Il s’agit du type [MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport).

Vous pouvez utiliser la propriété de [CardType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.cardtype) pour déterminer quel type de carte a été balayé et utilisez-la pour indiquer comment vous interprétez les données à partir de [Track1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track1), [Track2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track2), [Track3](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track3)et [Track4](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track4).

Les données à partir de chacune des pistes sont représentées en tant qu’objets de [MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata) . À partir de cette classe, vous pouvez obtenir les types de données suivants:

* [Données](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.data): les données brutes ou décodées.
* [DiscretionaryData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.discretionarydata): les données facultatives. 
* [EncryptedData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.encrypteddata): les données chiffrées.

L’extrait de code suivant obtient l’état et les données de suivi, puis vérifie le type de carte:

```cs
private void GetTrackData(MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    MagneticStripeReaderReport report = args.Report;
    IBuffer track1 = report.Track1.Data;
    IBuffer track2 = report.Track2.Data;
    IBuffer track3 = report.Track3.Data;
    IBuffer track4 = report.Track4.Data;

    if (report.CardType == MagneticStripeReaderCardTypes.Aamva)
    {
        // Card type is AAMVA
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Bank)
    {
        // Card type is bank card
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.ExtendedBase)
    {
        // Card type is vendor-specific
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Unknown)
    {
        // Card type is unknown
    }
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Voir aussi

* [Lecteur de bande magnétique](pos-magnetic-stripe-reader.md)
* [Classe de ClaimedMagneticStripeReader](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)
* [Classe de MagneticStripeReaderAamvaCardDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* [Classe de MagneticStripeReaderBankCardDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* [Classe de MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)
* [MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)
* [MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)