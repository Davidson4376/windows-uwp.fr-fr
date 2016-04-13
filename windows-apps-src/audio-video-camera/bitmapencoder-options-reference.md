---
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
Cet article répertorie les options de codage qui peuvent être utilisées avec BitmapEncoder.
Référence des options BitmapEncoder
---

# Référence des options BitmapEncoder

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Cet article répertorie les options de codage qui peuvent être utilisées avec [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206). Une option de codage se définit par son nom (à savoir une chaîne) et par une valeur dans un type de données particulier ([**Windows.Foundation.PropertyType**](https://msdn.microsoft.com/library/windows/apps/br225871)). Pour plus d’informations sur l’utilisation des images, voir [Imagerie](imaging.md).

| Nom                    | Type de propriété | Notes d’utilisation                                                                                        | Formats valides |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | unique       | Valeurs valides de 0 à 1.0. Des valeurs plus élevées indiquent une plus haute qualité.                                 | JPEG, JPEG-XR |
| CompressionQuality      | unique       | Valeurs valides de 0 à 1.0. Des valeurs plus élevées indiquent un schéma de compression plus efficace et plus lent. | TIFF          |
| Lossless                | booléen      | Si cette propriété a la valeur True, l’option ImageQuality est ignorée.                                        | JPEG-XR       |
| InterlaceOption         | booléen      | Indique la nécessité ou non d’entrelacer l’image.                                                                    | PNG           |
| FilterOption            | uint8        | Utilisez l’énumération [**PngFilterMode**](https://msdn.microsoft.com/library/windows/apps/br226389).                                | PNG           |
| TiffCompressionMethod   | uint8        | Utilisez l’énumération [**TiffCompressionMode**](https://msdn.microsoft.com/library/windows/apps/br226399).                    | TIFF          |
| Luminance               | uint32Array  | Tableau de 64 éléments comprenant des constantes de quantification de la luminance.                               | JPEG          |
| Chrominance             | uint32Array  | Tableau de 64 éléments comprenant des constantes de quantification de la chrominance.                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | Utilisez l’énumération [**JpegSubsamplingMode**](https://msdn.microsoft.com/library/windows/apps/br226386).                    | JPEG          |
| SuppressApp0            | booléen      | Indique la nécessité ou non de supprimer la création d’un bloc de métadonnées App0.                                        | JPEG          |
| EnableV5Header32bppBGRA | booléen      | Indique la nécessité ou non de coder dans une version 5 BMP avec prise en charge alpha.                                         | BMP           |

 

## Rubriques connexes

* [Imagerie](imaging.md)
 

 






<!--HONumber=Mar16_HO1-->


