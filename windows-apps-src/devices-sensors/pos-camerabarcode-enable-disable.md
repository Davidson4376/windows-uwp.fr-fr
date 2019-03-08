---
title: Configuration des scanneurs de codes-barres à caméra
description: Activer ou désactiver un scanneur de code-barres à caméra
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 3db54caa5be88559aa44f9a99273e9d8d5d6a00d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610304"
---
# <a name="enable-or-disable-the-software-decoder-that-ships-with-windows"></a>Activer ou désactiver le décodeur logiciel qui est fourni avec Windows
Dans Windows 10, version 1803, le décodeur logiciel est installé et activé par défaut.  Vous pouvez désactiver le logiciel de décodage qui est fourni avec Windows si vous ne souhaitez pas utiliser le scanneur de code-barres à caméra ou que vous avez acheté un décoder tiers qui fonctionne avec les API Windows.Devices.PointOfService.BarcodeScanner et ne souhaitez pas utiliser les deux.

## <a name="enable-or-disable-using-the-system-registry"></a>Activer ou désactiver à l’aide du registre système
Le décodeur logiciel qui est fourni avec Windows peut être activé ou désactivé via le registre système en ajoutant la clé de registre *InboxDecoder* sous *HKLM\Software\Microsoft\PointOfService\BarcodeScanner* et en fixant la valeur *Enable* comme décrit ci-dessous.

| Nom de valeur  | Type de valeur | Valeur | État |
| ----------- | --------- | -------|--------|
| Activer      | DWORD     | 1 (par défaut)<br/>0 |  Active le décodeur logiciel qui est fourni avec Windows <br/> Désactive le décodeur logiciel qui est fourni avec Windows |


Voici un exemple de fichier de registre que vous pouvez utiliser pour **désactiver** le décodeur logiciel qui est fourni avec Windows :

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000000


```  
    
Utilisez cet exemple de fichier de registre que vous pouvez utiliser pour **activer** le décodeur logiciel qui est fourni avec Windows :

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000001


```  

> [!Warning] 
> De graves problèmes peuvent se produire si vous modifiez le Registre de façon incorrecte.  Pour vous protéger, sauvegardez le registre avant de le modifier.  Vous pourrez ainsi restaurer le Registre en cas de problème.  Pour plus d'informations sur la sauvegarde et la restauration du registre, cliquez sur le numéro ci-après pour afficher l'article correspondant dans la Base de connaissances Microsoft : <br/><br/> [322756](https://support.microsoft.com/kb/322756) Comment sauvegarder et restaurer le registre dans Windows.

> [!NOTE]
> Le décodeur logiciel intégré à Windows 10 est fourni avec l'autorisation de [**Digimarc Corporation**](https://www.digimarc.com/).
