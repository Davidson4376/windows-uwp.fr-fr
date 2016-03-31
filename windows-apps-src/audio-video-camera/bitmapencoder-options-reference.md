---
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: This article lists the encoding options that can be used with BitmapEncoder.
title: BitmapEncoder options reference
---

# BitmapEncoder options reference

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

This article lists the encoding options that can be used with [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206). An encoding option is defined by its name, which is a string, and a value in a particular data type ([**Windows.Foundation.PropertyType**](https://msdn.microsoft.com/library/windows/apps/br225871)). For information on working with images, see [Imaging](imaging.md).

| Name                    | PropertyType | Usage notes                                                                                        | Valid formats |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | single       | Valid values from 0 to 1.0. Higher values indicate higher quality.                                 | JPEG, JPEG-XR |
| CompressionQuality      | single       | Valid values from 0 to 1.0. Higher values indicate a more efficient and slower compression scheme. | TIFF          |
| Lossless                | boolean      | If this is set to true, the ImageQuality option is ignored.                                        | JPEG-XR       |
| InterlaceOption         | boolean      | Whether to interlace the image.                                                                    | PNG           |
| FilterOption            | uint8        | Use the [**PngFilterMode**](https://msdn.microsoft.com/library/windows/apps/br226389) enumeration.                                | PNG           |
| TiffCompressionMethod   | uint8        | Use the [**TiffCompressionMode**](https://msdn.microsoft.com/library/windows/apps/br226399) enumeration.                    | TIFF          |
| Luminance               | uint32Array  | An array of 64 elements containing luminance quantization constants.                               | JPEG          |
| Chrominance             | uint32Array  | An array of 64 elements containing chrominance quantization constants.                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | Use the [**JpegSubsamplingMode**](https://msdn.microsoft.com/library/windows/apps/br226386) enumeration.                    | JPEG          |
| SuppressApp0            | boolean      | Whether to suppress the creation of an App0 metadata block.                                        | JPEG          |
| EnableV5Header32bppBGRA | boolean      | Whether to encode to a version 5 BMP which supports alpha.                                         | BMP           |

 

## Related topics

* [Imaging](imaging.md)
 

 




<!--HONumber=Mar16_HO1-->
