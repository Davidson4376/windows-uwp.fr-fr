---
author: TerryWarwick
title: Utilisation des symbologies des scanneurs de codes-barres
description: Cet article contient des informations sur les symbologies de scanneur de code-barres.
ms.author: jken
ms.date: 08/29/2018
ms.topic: article
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 91e09fd881ea5ce2b5ae6482148cdbb730c7ef17
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5762740"
---
# <a name="working-with-symbologies"></a>Utilisation des symbologies
Une [Symbologie de code-barres](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) correspond au mappage des données dans un format spécifique de code-barres. Certaines symbologies courantes sont UPC, Code 128, Code QR et ainsi de suite.  Le scanneur de codes-barres de plateforme Windows universelle API permettent à une application contrôler la façon dont le scanneur traite ces symbologies sans configurer manuellement le scanneur. 

## <a name="determine-which-symbologies-are-supported"></a>Détermination des symbologies prises en charge 
Dans la mesure où votre application peut être utilisée avec des modèles de scanneur de codes-barres provenant de différents fabricants, vous pouvez souhaiter interroger le scanneur pour déterminer la liste des symbologies qu'il prend en charge.  Cela peut être utile si votre application requiert une symbologie particulière qui n'est pas forcément prise en charge par tous les scanneurs, ou si vous devez activer des symbologies qui ont été désactivées manuellement ou par programmation sur le scanneur.

Une fois que vous avez un objet [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) obtenu à l’aide de [BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync), appelez [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) pour obtenir une liste des symbologies prises en charge par l’appareil.

L’exemple suivant obtient la liste des symbologies prises en charge du scanneur de codes-barres et les affiche dans un bloc de texte:

```cs
private void DisplaySupportedSymbologies(BarcodeScanner barcodeScanner, TextBlock textBlock) 
{
    var supportedSymbologies = await barcodeScanner.GetSupportedSymbologiesAsync();

    foreach (uint item in supportedSymbologies)
    {
        string symbology = BarcodeSymbologies.GetName(item);
        textBlock.Text += (symbology + "\n");
    }
}
```

## <a name="determine-if-a-specific-symbology-is-supported"></a>Déterminer si une symbologie particulière est prise en charge
Pour déterminer si le scanneur prend en charge une symbologie particulière, vous pouvez appeler [IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_).

L’exemple suivant vérifie si le scanneur de codes-barres prend en charge la SYMBOLOGIE **Code32** :

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>Modifier les symbologies reconnues
Dans certains cas, vous voudrez peut-être utiliser un sous-ensemble des symbologies prises en charge par le scanneur de codes-barres.  Cela est particulièrement utile pour bloquer les symbologies que vous ne souhaitez pas utiliser dans votre application. Par exemple, pour garantir qu'un utilisateur scanne le code-barres approprié, vous pouvez restreindre la lecture aux symbologies UPC ou EAN lors de la lecture de références d'article, ou à Code128 lors de la lecture de numéros de série.

Une fois que vous connaissez les symbologies que votre scanneur prend en charge, vous pouvez définir les symbologies que vous souhaitez qu'il reconnaisse.  Cela est possible après avoir créé un objet [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) à l’aide de [ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync). Vous pouvez appeler [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) pour activer un ensemble spécifique de symbologies, tandis que celles qui sont omises dans votre liste sont désactivées.

L’exemple suivant définit les symbologies actives d’un scanneur de code-barres revendiqué [Code39](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39) et [Code39Ex](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex):

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>Attributs de SYMBOLOGIE de code-barres
Symbologies de codes-barres différentes peuvent avoir des attributs différents, tels que prise en charge plusieurs décoder longueurs, transmettre le chiffre de contrôle à l’hôte en tant que partie des données brutes et vérifiez la validation des chiffres. Avec la classe [BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes) , vous pouvez obtenir et définir ces attributs pour une symbologie donnée [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) et codes-barres.

Vous pouvez obtenir les attributs d’une symbologie donnée avec [GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_). L’extrait de code suivant obtient les attributs de la SYMBOLOGIE Upca pour un **ClaimedBarcodeScanner**.

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

Lorsque vous avez terminé de modifier les attributs et que vous êtes prêt à définir les, vous pouvez appeler [SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync). Cette méthode retourne une **valeur booléenne**, qui est **true** si les attributs ont été définies correctement.

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>Restreindre l’analyse de données par la longueur
Certaines symbologies sont de longueur variable, telles que Code 39 ou Code 128.  Les codes-barres de ces symbologies peuvent être situés près eux contenant des données différentes, souvent de longueur spécifique. Définir la longueur spécifique des données dont vous avez besoin peut empêcher des lectures non valides.

Avant de définir la longueur de décodage, vérifiez si la SYMBOLOGIE de code-barres prend en charge plusieurs longueurs avec [IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported). Une fois que vous savez qu’il est pris en charge, vous pouvez définir le [DecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind), qui est de type [BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind). Cette propriété peut être une des valeurs suivantes:

* **AnyLength**: longueurs de n’importe quel nombre de décodage.
* **Discret**: décoder les longueurs de caractères codés sur [DecodeLength1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1) ou [DecodeLength2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2) .
* **Plage**: décoder longueurs entre les caractères codés sur **DecodeLength1** et **DecodeLength2** . L’ordre des **DecodeLength1** et **DecodeLength2** pas seulement (soit peut être supérieure ou inférieure à l’autre).

Enfin, vous pouvez définir les valeurs de **DecodeLength1** et **DecodeLength2** pour contrôler la longueur des données que vous avez besoin.

L’extrait de code suivant illustre la définition de la longueur de décodage:

```cs
private async Task<bool> SetDecodeLength(
    ClaimedBarcodeScanner scanner,
    uint symbology, 
    BarcodeSymbologyDecodeLengthKind kind, 
    uint decodeLength1, 
    uint decodeLength2)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsDecodeLengthSupported)
    {
        attributes.DecodeLengthKind = kind;
        attributes.DecodeLength1 = decodeLength1;
        attributes.DecodeLength2 = decodeLength2;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-transmission"></a>Vérifier la transmission de chiffres

Un autre attribut, que vous pouvez définir sur un symbolisme est indique si le chiffre de contrôle sera transmis à l’hôte en tant que partie des données brutes. Avant cela, assurez-vous que la SYMBOLOGIE prend en charge la vérification de la transmission de chiffres avec [IsCheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported). Ensuite, définissez si transmission de chiffres à cocher est activée avec [IsCheckDigitTransmissionEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled).

L’extrait de code suivant illustre la transmission de chiffres à cocher de paramètre:

```cs
private async Task<bool> SetCheckDigitTransmission(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitTransmissionSupported)
    {
        attributes.IsCheckDigitTransmissionEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-validation"></a>Validation des chiffres à cocher

Vous pouvez également définir si le chiffre de contrôle de codes-barres est validé. Avant cela, assurez-vous que la SYMBOLOGIE prend en charge la vérification de la validation de chiffres avec [IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported). Ensuite, déterminez si la validation de chiffres à cocher est activée avec [IsCheckDigitValidationEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled).

L’extrait de code suivant illustre la validation de chiffres à cocher de paramètre:

```cs
private async Task<bool> SetCheckDigitValidation(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitValidationSupported)
    {
        attributes.IsCheckDigitValidationEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Voir aussi

* [Scanneur de code-barres](pos-barcodescanner.md)
* [Classe BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
* [Classe BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [Classe ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)
* [Classe BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)
* [Énumération BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)