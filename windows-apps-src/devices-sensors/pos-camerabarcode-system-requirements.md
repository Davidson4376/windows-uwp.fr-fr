---
title: Configuration système requise du scanneur de codes-barres à caméra
description: Cet article répertorie les exigences d'utilisation d'un scanneur de codes-barres à caméra à partir d’une application UWP.
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 4eed59e302bc34e93d21adef794de02427f2933e
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243272"
---
# <a name="camera-barcode-scanner-system-requirements"></a>Configuration système requise du scanneur de codes-barres à caméra
À partir de Windows 10, version 1803, vous pouvez lire des codes-barres via un objectif de caméra standard à partir d’une application Windows universelle.

## <a name="supported-windows-editions"></a>Versions de Windows prises en charge
- Windows Mobile 10 Professionnel en mode S
- Windows 10 Professionnel
- Windows 10 Entreprise
- Windows 10 IoT Core


## <a name="webcam-requirements"></a>Configuration système de la webcam
| Category      | Recommandation           | Commentaires |
| ------------- | ------------------------ | -------- |
| Focus         | Mise au point automatique               | La mise au point fixe n’est pas recommandée |
| Résolution :    | 1920 x 1440, ou supérieure    | Nous avons eu une expérience optimale avec des caméras capables d'une résolution de 1 920 x 1 440 ou plus.  Certaines caméras à résolution plus faible peuvent lire les codes-barres standards s'ils sont imprimés suffisamment grands. Les codes-barres comportant des éléments plus fins peuvent nécessiter des caméras d'une résolution supérieure. |
|

## <a name="see-also"></a>Voir aussi

### <a name="samples"></a>Exemples

- [Exemple de scanneur de codes-barres](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
