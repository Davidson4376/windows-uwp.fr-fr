---
title: Obtenir et comprendre les données de bande magnétique
description: Découvrez comment obtenir et interpréter les données à partir d’une bande magnétique.
ms.date: 10/04/2018
ms.topic: article
keywords: Windows 10, uwp, point de service pos, lecteur de bande magnétique
ms.localizationpriority: medium
ms.openlocfilehash: 12b88d942e4b5a9c90880f6bd362ba9e7e011186
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321553"
---
# <a name="obtain-and-understand-magnetic-stripe-data"></a>Obtenir et comprendre les données de bande magnétique

Une fois que vous avez configuré votre lecteur de bande magnétique dans votre application à l’aide de la procédure décrite dans [prise en main de Service du Point de](pos-basics.md), vous êtes prêt à commencer à obtenir des données à partir de celui-ci.

## <a name="subscribe-to-datareceived-events"></a>S’abonner à * DataReceived événements

Chaque fois que le lecteur reconnaît une carte passée, il déclenchera un des trois événements :

* [Événement de AamvaCardDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): Se produit lorsqu’une carte de véhicule à moteur est glissée.
* [Événement de BankCardDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): Se produit lorsqu’une carte bancaire est glissée.
* [Événement de VendorSpecificDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.vendorspecificdatareceived): Se produit lorsqu’une carte spécifique au fournisseur est passée.

Votre application doit uniquement s’abonner aux événements qui sont pris en charge par le lecteur de bande magnétique. Vous pouvez voir quels types de cartes sont pris en charge avec [MagneticStripeReader.SupportedCardTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.supportedcardtypes).

Le code suivant montre l’abonnement aux trois ***DataReceived** événements :

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

Le Gestionnaire d’événements recevront le [ClaimedMagneticStripeReader](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader) et un *args* objet, dont le type varie en fonction de l’événement :

* **AamvaCardDataReceived** événement : [MagneticStripeReaderAamvaCardDataReceivedEventArgs Class](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* **BankCardDataReceived** événement : [MagneticStripeReaderBankCardDataReceivedEventArgs Class](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* **VendorSpecificDataReceived** événement : [Classe de MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)

## <a name="get-the-data"></a>Obtenir les données

Pour le **AamvaCardDataReceived** et **BankCardDataReceived** événements, vous pouvez obtenir des données directement à partir de la *args* objet. L’exemple suivant illustre l’obtention de quelques propriétés et de les assigner à des variables de membre :

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

Toutefois, certaines données, y compris toutes les données à partir de la **VendorSpecificDataReceived** événement, doivent être récupérées par le biais le **rapport** objet, qui est une propriété de la *args* paramètre. Il s’agit du type [MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport).

Vous pouvez utiliser la [CardType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.cardtype) propriété pour déterminer quel type de carte a été passée et utilise ensuite pour indiquer comment vous interprétez les données à partir de [Track1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track1), [pole 2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track2), [ Track3](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track3), et [Track4](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track4).

Les données à partir de chacune des pistes sont représentées en tant que [MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata) objets. À partir de cette classe, vous pouvez obtenir les types de données suivants :

* [Données](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.data): Les données brutes ou décodées.
* [DiscretionaryData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.discretionarydata): Les données facultatives. 
* [EncryptedData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.encrypteddata): Les données chiffrées.

L’extrait de code suivant obtient le rapport et les données de suivi et vérifie ensuite le type de carte :

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
* [MagneticStripeReaderAamvaCardDataReceivedEventArgs Class](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* [MagneticStripeReaderBankCardDataReceivedEventArgs Class](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* [Classe de MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)
* [MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)
* [MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)