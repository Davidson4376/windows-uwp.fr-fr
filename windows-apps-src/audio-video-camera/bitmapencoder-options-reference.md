---
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: Cet article répertorie les options de codage qui peuvent être utilisées avec BitmapEncoder.
title: Référence des options BitmapEncoder
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 34e2ac1531b496f076abbe8e23ffa1c0cb318373
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359053"
---
# <a name="bitmapencoder-options-reference"></a>Référence des options BitmapEncoder


Cet article répertorie les options de codage qui peuvent être utilisées avec [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder). Une option de codage se définit par son nom (à savoir une chaîne) et par une valeur dans un type de données particulier ([**Windows.Foundation.PropertyType**](https://docs.microsoft.com/uwp/api/Windows.Foundation.PropertyType)). Pour plus d’informations sur l’utilisation des images, voir [Créer, modifier et enregistrer des images bitmap](imaging.md).

| Nom                    | Type de propriété | Notes d’utilisation                                                                                        | Formats valides |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | unique       | Valeurs valides de 0 à 1.0. Des valeurs plus élevées indiquent une meilleure qualité                                 | JPEG, JPEG-XR |
| CompressionQuality      | unique       | Valeurs valides de 0 à 1.0. Des valeurs plus élevées indiquent un schéma de compression plus efficace et plus lent. | TIFF          |
| Lossless                | booléenne      | Si cette propriété a la valeur true, l’option ImageQuality est ignorée                                        | JPEG-XR       |
| InterlaceOption         | booléenne      | Indique s’il faut ou non entrelacer l’image.                                                                    | PNG           |
| FilterOption            | uint8        | Utiliser l’énumération [**PngFilterMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.PngFilterMode)                                | PNG           |
| TiffCompressionMethod   | uint8        | Utiliser l’énumération [**TiffCompressionMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.TiffCompressionMode)                    | TIFF          |
| Luminance               | uint32Array  | Tableau de 64 éléments comprenant des constantes de quantification de la luminance                               | JPEG          |
| Chrominance             | uint32Array  | Tableau de 64 éléments comprenant des constantes de quantification de la chrominance                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | Utiliser l’énumération [**JpegSubsamplingMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.JpegSubsamplingMode)                    | JPEG          |
| SuppressApp0            | booléenne      | Indique s’il faut ou non supprimer la création d’un bloc de métadonnées App0.                                        | JPEG          |
| EnableV5Header32bppBGRA | booléenne      | Indique s’il faut ou non coder dans une version 5 BMP avec prise en charge alpha.                                         | BMP           |

 

## <a name="related-topics"></a>Rubriques connexes

* [Créer, modifier et enregistrer des images bitmap](imaging.md)
* [Codecs pris en charge](supported-codecs.md)

 




