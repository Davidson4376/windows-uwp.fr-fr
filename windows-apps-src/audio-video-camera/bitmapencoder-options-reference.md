---
author: drewbatgit
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: "Cet article répertorie les options de codage qui peuvent être utilisées avec BitmapEncoder."
title: "Référence des options BitmapEncoder"
translationtype: Human Translation
ms.sourcegitcommit: de54d389488d8298ea1341b0a6f27a476d38584e
ms.openlocfilehash: 0ccaf55215cb82633313e145a126db92aa6d16e6

---

# Référence des options BitmapEncoder

\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Cet article répertorie les options de codage qui peuvent être utilisées avec [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206). Une option de codage se définit par son nom (à savoir une chaîne) et par une valeur dans un type de données particulier ([**Windows.Foundation.PropertyType**](https://msdn.microsoft.com/library/windows/apps/br225871)). Pour plus d’informations sur l’utilisation des images, voir [Créer, modifier et enregistrer des images bitmap](imaging.md).

| Nom                    | Type de propriété | Notes d’utilisation                                                                                        | Formats valides |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | unique       | Valeurs valides de0 à1.0. Des valeurs plus élevées indiquent une meilleure qualité                                 | JPEG, JPEG-XR |
| CompressionQuality      | unique       | Valeurs valides de0 à1.0. Des valeurs plus élevées indiquent un schéma de compression plus efficace et plus lent. | TIFF          |
| Lossless                | valeur booléenne      | Si cette propriété a la valeur true, l’option ImageQuality est ignorée                                        | JPEG-XR       |
| InterlaceOption         | valeur booléenne      | Indique s’il faut ou non entrelacer l’image.                                                                    | PNG           |
| FilterOption            | uint8        | Utiliser l’énumération [**PngFilterMode**](https://msdn.microsoft.com/library/windows/apps/br226389)                                | PNG           |
| TiffCompressionMethod   | uint8        | Utiliser l’énumération [**TiffCompressionMode**](https://msdn.microsoft.com/library/windows/apps/br226399)                    | TIFF          |
| Luminance               | uint32Array  | Tableau de 64éléments comprenant des constantes de quantification de la luminance                               | JPEG          |
| Chrominance             | uint32Array  | Tableau de 64éléments comprenant des constantes de quantification de la chrominance                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | Utiliser l’énumération [**JpegSubsamplingMode**](https://msdn.microsoft.com/library/windows/apps/br226386)                    | JPEG          |
| SuppressApp0            | valeur booléenne      | Indique s’il faut ou non supprimer la création d’un bloc de métadonnées App0.                                        | JPEG          |
| EnableV5Header32bppBGRA | valeur booléenne      | Indique s’il faut ou non coder dans une version 5BMP avec prise en charge alpha.                                         | BMP           |

 

## Rubriques connexes

* [Créer, modifier et enregistrer des images bitmap](imaging.md)
 

 







<!--HONumber=Aug16_HO3-->


