---
author: eliotcowley
title: Obtenir et comprendre les données de code à barres
description: Découvrez comment obtenir et interpréter les données de code à barres que vous numérisez.
ms.author: elcowle
ms.date: 08/29/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 0992ea54092063ba53f23871599905e58f1b456e
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/05/2018
ms.locfileid: "3374722"
---
# <a name="obtain-and-understand-barcode-data"></a>Obtenir et comprendre les données de code à barres

Une fois que vous avez configuré votre scanneur de code-barres, vous devez bien sûr une façon de comprendre les données que vous numérisez. Lorsque vous numérisez un code-barres, l’événement [DataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) est déclenché. La [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) doit s’abonner à cet événement. L’événement **DataReceived** transmet un objet [BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs) , qui vous permet d’accéder aux données du code-barres.

## <a name="subscribe-to-the-datareceived-event"></a>S’abonner à l’événement DataReceived

Une fois que vous avez un **ClaimedBarcodeScanner**, qu’il s’abonner à l’événement **DataReceived** :

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

Le Gestionnaire d’événements sera passé la **ClaimedBarcodeScanner** et un objet **BarcodeScannerDataReceivedEventArgs** . Vous pouvez accéder les données code barre par [rapport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) propriété de cet objet, qui est de type [BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport).

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>Obtenir les données

Une fois que vous avez les **BarcodeScannerReport**, vous pouvez accéder et analyser les données de code à barres. **BarcodeScannerReport** a trois propriétés:

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata): les données de code-barres complet, brutes.
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel): l’étiquette de code-barres décodée, qui n’inclut pas l’en-tête, total de contrôle et d’autres informations diverses.
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype): le type d’étiquette de code-barres décodé. Les valeurs possibles sont définies dans la classe [BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) .

Si vous souhaitez accéder à **ScanDataLabel** ou à **ScanDataType**, vous devez d’abord définir [IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) sur **true**.

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>Obtenir le type de données d’analyse

Le type d’étiquette de code-barres décodée est relativement trivial&mdash;nous appelons simplement [GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) sur **ScanDataType**.

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>Obtenir l’étiquette de données d’analyse

Pour obtenir le libellé de code-barres décodé, il existe quelques éléments que vous devez connaître. Seuls certains types de données contiennent du texte codé, donc vous devez d’abord vérifier si le symbolisme peut être converti en une chaîne, puis convertir le tampon que nous obtenons à partir de **ScanDataLabel** à une chaîne codée en UTF-8.

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

Pour obtenir la version complète, des données brutes à partir du code à barres, nous avons simplement convertir le tampon que nous obtenons à partir de **ScanData** dans une chaîne.

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

Ces données sont, en règle générale, dans le format remis à partir de l’analyseur. Les informations d’en-tête et de la remorque de message sont supprimés, cependant, car ils ne contiennent pas d’informations utiles pour une application et qui sont susceptibles d’être spécifique à l’analyseur.

Informations d’en-tête communes sont un caractère de préfixe (par exemple, un caractère STX). Informations communes de remorque sont un caractère de terminaison (par exemple, un caractère ETX ou CR) et un caractère de vérification du bloc si une est générée par le moteur d’analyse.

Cette propriété doit inclure un caractère de symbolisme si un est retourné par le moteur d’analyse (par exemple, un pour **un** UPC-A). Il doit également inclure les chiffres de contrôle s’ils sont présents dans l’étiquette et retourné par le moteur d’analyse. (Notez que les caractères de symbolisme et les chiffres de contrôle peuvent ou peut ne pas exister, en fonction de la configuration de l’analyseur. Le moteur d’analyse les renverra si présente, mais ne sera pas générer ou calculer si elles sont absentes.)

Certaines marchandises peuvent être marqués avec un code à barres supplémentaire. Ce code barre est généralement placé à droite du code barre principale et se compose d’un caractères de deux ou cinq supplémentaires d’informations. Si le moteur d’analyse lit des marchandises qui contient les codes-barres principales et supplémentaires, les caractères supplémentaires sont ajoutés aux caractères principales, et le résultat est remis à l’application sous la forme d’une étiquette. (Notez qu’un analyseur peut prendre en charge une configuration qui active ou désactive la lecture de codes supplémentaires.)

Certaines marchandises peuvent être marqués avec plusieurs étiquettes, parfois appelés *multisymbol étiquettes* ou *monté en cascade*. Ces codes barres sont généralement verticalement et peuvent le symbolisme identiques ou différent. Si le moteur d’analyse lit des marchandises qui contient plusieurs étiquettes, chaque code à barres est remis à l’application sous la forme d’une étiquette distincte. Cela est nécessaire en raison du manque d’en cours de standardisation de ces types de codes à barres. Un n’est pas en mesure de déterminer toutes les variantes basées sur les données de code à barres individuelles. Par conséquent, l’application devra déterminer quand un code-barres étiquette multiples a été lu en fonction des données retournées. (Notez qu’un scanneur peut ou peut ne pas supporter la lecture de plusieurs étiquettes).

Cette valeur est définie avant un événement **DataReceived** est déclenché à l’application.

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Articles associés
* [Scanneur de code-barres](pos-barcodescanner.md)
* [Classe de ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [Classe de BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [Classe de BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [Classe de BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)