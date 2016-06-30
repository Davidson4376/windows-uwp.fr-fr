---
author: IvorB
ms.assetid: E9ADC88F-BD4F-4721-8893-0E19EA94C8BA
title: Couplage hors-bande
description: "Le couplage hors-bande permet aux applications de se connecter à un périphérique de point de service sans qu’une découverte soit requise."
translationtype: Human Translation
ms.sourcegitcommit: 0bf96b70a915d659c754816f4c115f3b3f0a5660
ms.openlocfilehash: d8d37b779a0f9a4bec36d73fcd2d35272c587b11

---
# Couplage hors-bande

Le couplage hors-bande permet aux applications de se connecter à un périphérique de point de service sans qu’une découverte soit requise. Les applications doivent utiliser l’espace de noms [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/windows.devices.pointofservice.aspx) et transmettre une chaîne spécialement formatée (objet blob hors-bande) à la méthode **FromIdAsync** appropriée pour le périphérique souhaité. Quand **FromIdAsync** est exécuté, l’appareil hôte se couple et se connecte au périphérique avant que l’opération ne soit renvoyée à l’appelant.

## Format d’objet blob hors-bande

```json
    "connectionKind":"Network",
    "physicalAddress":"AA:BB:CC:DD:EE:FF",
    "connectionString":"192.168.1.1:9001",
    "peripheralKinds":"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}",
    "providerId":"{02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2}",
    "providerName":"PrinterProtocolProvider.dll"
```

**connectionKind** : type de connexion. Les valeurs valides sont « Network » et « Bluetooth ».

**physicalAddress** : adresse MAC du périphérique. Par exemple, dans le cas d’une imprimante réseau, il s’agirait de l’adresse MAC fournie par la feuille de test de l’imprimante au format AA:BB:CC:DD:EE:FF.

**connectionString** : chaîne de connexion du périphérique. Par exemple, dans le cas d’une imprimante réseau, il s’agirait de l’adresse IP fournie par la feuille de test de l’imprimante au format 192.168.1.1:9001. Ce champ est omis pour tous les périphériques Bluetooth.

**peripheralKinds** : GUID pour le type de périphérique. Valeurs valides :

| Type de périphérique | GUID |
| ---- | ---- |
| *Imprimante POS* | C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33 |
| *Scanneur de code-barres* | C243FFBD-3AFC-45E9-B3D3-2BA18BC7EBC5 |
| *Caisse enregistreuse* | 772E18F2-8925-4229-A5AC-6453CB482FDA |


**providerId** : GUID de la classe de fournisseur de protocole. Valeurs valides :

| Classe de fournisseur de protocole | GUID |
| ---- | ---- |
| *Imprimante réseau ESC/POS générique* | 02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2 |
| *Imprimante BT ESC/POS générique* | CCD5B810-95B9-4320-BA7E-78C223CAF418 |
| *Imprimante BT Epson* | 94917594-544F-4AF8-B53B-EC6D9F8A4464 |
| *Imprimante réseau Epson* | 9F0F8BE3-4E59-4520-BFBA-AF77614A31CE |
| *Imprimante réseau Star* | 1E3A32C2-F411-4B8C-AC91-CC2C5FD21996 |
| *Scanneur BT socket* | 6E7C8178-A006-405E-85C3-084244885AD2 |
| *Caisse réseau APG* | E619E2FE-9489-4C74-BF57-70AED670B9B0 |
| *Caisse BT APG* | 332E6550-2E01-42EB-9401-C6A112D80185 |


**providerName** : nom de la DLL de fournisseur. Fournisseurs par défaut :

| Fournisseur | Nom de la DLL |
| ---- | ---- |
| Imprimante | PrinterProtocolProvider.dll |
| Caisse enregistreuse | CashDrawerProtocolProvider.dll |
| Scanneur | BarcodeScannerProtocolProvider.dll |

## Exemple d’utilisation : imprimante réseau

```csharp
String oobBlobNetworkPrinter =
  "{\"connectionKind\":\"Network\"," +
  "\"physicalAddress\":\"AA:BB:CC:DD:EE:FF\"," +
  "\"connectionString\":\"192.168.1.1:9001\"," +
  "\"peripheralKinds\":\"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}\"," +
  "\"providerId\":\"{02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2}\"," +
  "\"providerName\":\"PrinterProtocolProvider.dll\"}";

printer = await PosPrinter.FromIdAsync(oobBlobNetworkPrinter);
```

## Exemple d’utilisation : imprimante Bluetooth

```csharp
string oobBlobBTPrinter =
    "{\"connectionKind\":\"Bluetooth\"," +
    "\"physicalAddress\":\"AA:BB:CC:DD:EE:FF\"," +
    "\"peripheralKinds\":\"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}\"," +
    "\"providerId\":\"{CCD5B810-95B9-4320-BA7E-78C223CAF418}\"," +
    "\"providerName\":\"PrinterProtocolProvider.dll\"}";

printer = await PosPrinter.FromIdAsync(oobBlobBTPrinter);

```



<!--HONumber=Jun16_HO4-->


