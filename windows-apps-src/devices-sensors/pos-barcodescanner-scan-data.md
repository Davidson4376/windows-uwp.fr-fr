---
title: Obtenir et comprendre les données de code-barres
description: Découvrez comment obtenir et interpréter les données de code-barres que vous analysez.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ece246ffd369ee21c089598f07b2566424757f55
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605964"
---
# <a name="obtain-and-understand-barcode-data"></a>Obtenir et comprendre les données de code-barres

Une fois que vous avez configuré votre lecteur de code-barres, vous devez bien sûr un moyen de comprendre les données que vous analysez. Quand vous scannez un code-barres, le [DataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) événement est déclenché. Le [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) doit s’abonner à cet événement. Le **DataReceived** événement passe une [BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs) objet, que vous pouvez utiliser pour accéder aux données de code-barres.

## <a name="subscribe-to-the-datareceived-event"></a>S’abonner à l’événement DataReceived

Une fois que vous avez un **ClaimedBarcodeScanner**, qu’il s’abonner à la **DataReceived** événement :

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

Le Gestionnaire d’événements recevront le **ClaimedBarcodeScanner** et un **BarcodeScannerDataReceivedEventArgs** objet. Vous pouvez accéder aux données code-barres par le biais de cet objet [rapport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) propriété, qui est de type [BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport).

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>Obtenir les données

Une fois que vous avez le **BarcodeScannerReport**, vous pouvez y accéder et analyser les données de code-barres. **BarcodeScannerReport** possède trois propriétés :

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata): Les données de code-barres complet, brutes.
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel): L’étiquette de code-barres décodés, ce qui n’inclut pas l’en-tête, de somme de contrôle et d’autres informations diverses.
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype): Le type d’étiquette de code-barres décodé. Les valeurs possibles sont définies dans le [BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) classe.

Si vous souhaitez accéder soit **ScanDataLabel** ou **ScanDataType**, vous devez d’abord définir [IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) à **true**.

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>Obtenir le type de données d’analyse

Le type d’étiquette de code-barres décodé est relativement trivial&mdash;nous appelons simplement [GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) sur **ScanDataType**.

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>Obtenir l’étiquette de données d’analyse

Pour obtenir l’étiquette de code-barres décodé, il existe quelques éléments que vous devez être conscient. Seuls certains types de données contiennent du texte encodé, vous devez donc tout d’abord vérifier si la SYMBOLOGIE peut être converti en une chaîne, puis convertir la mémoire tampon que nous obtenons à partir **ScanDataLabel** à une chaîne UTF-8 encodée.

```cs
private string GetDataLabel(BarcodeScannerDataReceivedEventArgs args)
{
    uint scanDataType = args.Report.ScanDataType;

    // Only certain data types contain encoded text.
    // To keep this simple, we'll just decode a few of them.
    if (args.Report.ScanDataLabel == null)
    {
        return "No data";
    }

    // This is not an exhaustive list of symbologies that can be converted to a string.
    else if (scanDataType == BarcodeSymbologies.Upca ||
        scanDataType == BarcodeSymbologies.UpcaAdd2 ||
        scanDataType == BarcodeSymbologies.UpcaAdd5 ||
        scanDataType == BarcodeSymbologies.Upce ||
        scanDataType == BarcodeSymbologies.UpceAdd2 ||
        scanDataType == BarcodeSymbologies.UpceAdd5 ||
        scanDataType == BarcodeSymbologies.Ean8 ||
        scanDataType == BarcodeSymbologies.TfStd)
    {
        // The UPC, EAN8, and 2 of 5 families encode the digits 0..9
        // which are then sent to the app in a UTF8 string (like "01234").
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanDataLabel);
    }

    // Some other symbologies (typically 2-D symbologies) contain binary data that
    // should not be converted to text.
    else
    {
        return "Decoded data unavailable.";
    }
}
```

### <a name="get-the-raw-scan-data"></a>Obtenir les données de l’analyse brute

Pour obtenir les données brutes complètes à partir du code-barres, nous avons simplement convertir la mémoire tampon que nous obtenons à partir **ScanData** en une chaîne.

```cs
private string GetRawData(BarcodeScannerDataReceivedEventArgs args)
{
    // Get the full, raw barcode data.
    if (args.Report.ScanData == null)
    {
        return "No data";
    }

    // Just to show that we have the raw data, we'll print the value of the bytes.
    else
    {
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanData);
    }
}
```

Ces données sont en général, dans le format de rendu à partir de l’analyseur. Les informations d’en-tête et de code de fin de message sont supprimés, toutefois, dans la mesure où ils ne contiennent pas d’informations utiles pour une application et sont susceptibles d’être spécifique à l’analyseur.

Informations d’en-tête communes sont un caractère de préfixe (par exemple, un caractère STX). Informations relatives au code de fin Common est un caractère de marque de fin (par exemple, un caractère ETX ou CR) et un caractère de contrôle de bloc si une est générée par le scanneur.

Cette propriété doit inclure un caractère de SYMBOLOGIE si une est retournée par le scanneur (par exemple, un **A** pour UPC-A). Il doit également inclure les chiffres de vérification s’ils sont présents dans l’étiquette et retourné par le scanneur. (Notez que les caractères de SYMBOLOGIE et les chiffres de vérification peuvent ou peut ne pas exister, en fonction de la configuration du scanneur. Le scanneur retournera les si présent, mais ne sera pas générer ou les calculer si elles ne figurent pas.)

Certains produits peuvent être marqués avec un code-barres supplémentaire. Ce code-barres est généralement placé à droite du code-barres principale et se compose d’un supplémentaires deux cinq caractères d’informations. Si le moteur d’analyse lit des marchandises qui contient les codes-barres principales et supplémentaires, les caractères supplémentaires sont ajoutées aux caractères principales et le résultat est remis à l’application comme une étiquette. (Notez qu’un analyseur peut prendre en charge une configuration qui active ou désactive la lecture des codes supplémentaires).

Certains produits peuvent être marqués avec plusieurs légendes, parfois appelés *étiquettes multisymbol* ou *à plusieurs niveaux d’étiquettes*. Ces codes à barres sont généralement organisés verticalement et peuvent être de la SYMBOLOGIE identiques ou différente. Si le moteur d’analyse lit des marchandises qui contient plusieurs étiquettes, chaque code-barres est remis à l’application sous forme d’étiquette distinct. Cela est nécessaire en raison de l’absence de normalisation de ces types de codes-barres. Un n’est pas en mesure de déterminer toutes les variations en fonction des données de code-barres individuels. Par conséquent, l’application devra déterminer quand un code-barres d’étiquette plusieurs ont été lues en fonction des données retournées. (Notez qu’un analyseur peut ou peut prend pas en charge la lecture de plusieurs étiquettes).

Cette valeur est définie avant une **DataReceived** événement déclenché à l’application.

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Voir également
* [Scanneur de codes-barres](pos-barcodescanner.md)
* [Classe de ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [Classe de BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [Classe de BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [Classe de BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
