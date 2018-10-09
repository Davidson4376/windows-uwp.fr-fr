---
author: eliotcowley
title: Obtenir et comprendre les données de code-barres
description: Découvrez comment obtenir et interpréter les données de code-barres que vous analysez.
ms.author: elcowle
ms.date: 08/29/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 0992ea54092063ba53f23871599905e58f1b456e
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4467888"
---
# <a name="obtain-and-understand-barcode-data"></a>Obtenir et comprendre les données de code-barres

Une fois que vous avez configuré votre scanneur de code-barres, vous devez bien entendu un moyen de comprendre les données que vous analysez. Lorsque vous scannez un code-barres, l’événement [DataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) est déclenché. La [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) doit s’abonner à cet événement. L’événement **DataReceived** transmet un objet [BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs) , que vous pouvez utiliser pour accéder aux données du code-barres.

## <a name="subscribe-to-the-datareceived-event"></a>S’abonner à l’événement DataReceived

Une fois que vous avez un **ClaimedBarcodeScanner**, qu’il s’abonner à l’événement **DataReceived** :

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

Le Gestionnaire d’événements est transmis la **ClaimedBarcodeScanner** et un objet **BarcodeScannerDataReceivedEventArgs** . Vous pouvez accéder aux données code-barres par le biais de [rapport de](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) propriété de cet objet, qui est du type [BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport).

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>Obtenir les données

Une fois que vous avez la **BarcodeScannerReport**, vous pouvez accéder et analyser les données de code-barres. **BarcodeScannerReport** possède trois propriétés:

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata): les données de code-barres complet, brutes.
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel): l’étiquette de code-barres décodée, qui n’inclut pas l’en-tête, somme de contrôle et d’autres informations diverses.
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype): le type d’étiquette de code-barres décodé. Les valeurs possibles sont définies dans la classe [BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) .

Si vous souhaitez accéder à des **ScanDataLabel** ou **ScanDataType**, vous devez tout d’abord définir [IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) sur **true**.

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>Obtenir le type de données d’analyse

Obtenir le type d’étiquette de code-barres décodée est assez simple&mdash;nous appelons simplement [GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) sur **ScanDataType**.

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>Obtenir l’étiquette de données d’analyse

Pour obtenir le libellé de code-barres décodée, il existe quelques points, que vous devez être conscient des. Seuls certains types de données contiennent du texte codé, vous devez donc tout d’abord vérifier si la SYMBOLOGIE peuvent être converties en une chaîne et le reconvertir la mémoire tampon que nous obtenons à partir de **ScanDataLabel** à une chaîne codée en UTF-8.

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

### <a name="get-the-raw-scan-data"></a>Obtenir les données d’analyse brute

Pour obtenir la version complète, les données brutes à partir du code-barres, nous avons simplement convertir la mémoire tampon que nous obtenons à partir de **ScanData** dans une chaîne.

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

Ces données sont, en règle générale, dans le format fournie par l’à partir de l’analyseur. Informations sur l’en-tête et le code du message sont supprimées, toutefois, dans la mesure où ils ne contiennent pas d’informations utiles pour une application et sont susceptibles d’être scanneur spécifique.

Informations d’en-tête communes sont un préfixe (par exemple, un caractère STX). Des informations courantes bande-annonce sont un caractère de terminaison (par exemple, un caractère ETX ou CR) et un caractère de vérification de bloc si un est généré par le scanneur.

Cette propriété doit inclure un caractère SYMBOLOGIE si un est retourné par le scanneur (par exemple, un pour **un** UPC-A). Il doit également inclure les chiffres de contrôle s’ils sont présents dans l’étiquette et renvoyée par le scanneur. (Notez que les caractères SYMBOLOGIE et les chiffres de contrôle peuvent ou ne pas être présents, en fonction de la configuration de l’analyseur. L’analyseur retournera les si présenter, mais ne seront pas générer ou les calculer si elles sont absentes.)

Certains marchandise peut-être être marqué avec un code-barres supplémentaire. Ce code-barres est généralement placés à droite du code-barres principal et se compose d’un caractères deux ou cinq supplémentaires d’informations. Si le scanneur lit marchandise qui contient les codes-barres supplémentaires et principaux, les caractères supplémentaires sont ajoutés dans les personnages principaux, et le résultat est fourni à l’application en tant qu’une seule étiquette. (Notez qu’un scanneur peut prendre en charge une configuration qui active ou désactive la lecture de codes supplémentaires).

Certains marchandise peut-être être marqué avec des étiquettes plusieurs, parfois appelées *étiquettes multisymbol* ou *hiérarchisé*. Ces codes sont généralement disposés verticalement et peuvent être de la SYMBOLOGIE identiques ou différent. Si le scanneur lit marchandise qui contient plusieurs étiquettes, chaque code-barres est fourni à l’application comme une étiquette distincte. Cela est nécessaire en raison du manque de normalisation de ces types de code-barres actuel. Un n’est pas en mesure de déterminer toutes les variantes basées sur les données de code-barres individuels. Par conséquent, l’application devra déterminer quand un code-barre étiquette plusieurs a été lu selon les données renvoyées. (Notez qu’un scanneur peut ou ne peut pas en charge la lecture des étiquettes de plusieurs).

Cette valeur est définie avant un événement **DataReceived** est déclenché à l’application.

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Voir aussi
* [Scanneur de code-barres](pos-barcodescanner.md)
* [Classe ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [Classe BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [Classe BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [Classe BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)