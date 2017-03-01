---
author: awkoren
Description: Cet article explique comment signer une application de bureau que vous avez convertie vers la plateforme Windows universelle (UWP).
Search.Product: eADQiWindows 10XVcnh
title: Signez une application convertie avec le kit de ressources Pont de plateforme Windows universelle (pont UWP) pour applications de bureau.
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: 232c3012-71ff-4f76-a81e-b1758febb596
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: b5936030f09e52933053f2465659157083b9eec2
ms.lasthandoff: 02/08/2017

---

# <a name="sign-an-app-converted-with-the-desktop-bridge"></a>Signer une application convertie avec Pont du bureau

Cet article explique comment signer une application de bureau que vous avez convertie vers la plateforme Windows universelle (UWP). Vous devez signer votre package .appx avec un certificat avant de pouvoir le déployer.

## <a name="automatically-sign-using-the-desktop-app-converter-dac"></a>Signer automatiquement à l’aide de Desktop App Converter (DAC)

Utilisez l’indicateur ```-Sign``` lors de l’exécution de DAC pour signer automatiquement votre package .appx. Pour plus d’informations, voir [Desktop App Converter Preview (Aperçu de Desktop App Converter)](desktop-to-uwp-run-desktop-app-converter.md).

## <a name="manually-sign-using-signtoolexe"></a>Signer manuellement à l’aide de SignTool.exe

Tout d’abord, créez un certificat à l’aide de MakeCert.exe. Si vous êtes invité à entrer un mot de passe, sélectionnez « Aucun ». 

```cmd
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
```

Ensuite, copiez vos informations de clé publique et privée dans le certificat à l’aide de pvk2pfx.exe. 

```cmd
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
```
Pour finir, utilisez SignTool.exe pour signer votre .appx avec le certificat.

```cmd
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
``` 

Pour plus d’informations, voir [Comment signer un package d’application à l’aide de SignTool](https://msdn.microsoft.com/library/windows/desktop/jj835835.aspx). 

Les trois outils ci-dessus sont inclus dans le Kit de développement logiciel (SDK) de Microsoft Windows 10. Pour les appeler directement, appelez le script ```C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\Tools\VsDevCmd.bat``` à l’aide d’une invite de commandes.

## <a name="common-errors"></a>Erreurs courantes

### <a name="publisher-and-cert-mismatch-causes-signtool-error-error-signersign-failed--21470248850x8007000b"></a>La non-correspondance de l’éditeur et du certificat provoque une erreur Signtool « Erreur : SignerSign() Failed » (-2147024885/0x8007000b)

L’entrée Publisher dans le manifeste appx doit correspondre à l’objet du certificat avec lequel vous effectuer la signature.  Vous pouvez utiliser l’une des méthodes suivantes pour afficher l’objet du certificat. 

**Option 1 : Powershell**

Exécutez la commande PowerShell suivante. Il est possible d’utiliser le format .cer ou .pfx comme fichier de certificat, car ils ont les mêmes informations d’éditeur.

```ps
(Get-PfxCertificate <cert_file>).Subject
```

**Option 2 : Explorateur de fichiers**

Double-cliquez sur le certificat dans l’Explorateur de fichiers. Sélectionnez ensuite l’onglet *Détails*, puis le champ *Objet* dans la liste. Vous pouvez alors copier le contenu. 

**Option 3 : CertUtil**

Exécutez **certutil** à partir de la ligne de commande sur le fichier PFX, puis copier le champ *Subject* à partir de la sortie. 

```cmd
certutil -dump <cert_file.pfx>
```

### <a name="corrupted-or-malformed-authenticode-signatures"></a>Signatures Authenticode endommagées ou mal formées

Cette section illustre comment identifier des problèmes dans les fichiers au format exécutable portable (PE) de votre package AppX, qui peuvent contenir des signatures endommagées ou mal formées. Toute signature Authenticode non valide de vos fichiers PE, qui peuvent être au n’importe quel format binaire (par exemple, .exe, .dll, .chm, etc.), empêchera la signature correcte de votre package. Ce dernier ne peut donc pas être déployé à partir d’un package AppX. 

L’emplacement de la signature Authenticode d’un fichier PE est spécifié par l’entrée de la table de certificats, qui se trouve dans les répertoires de données d’en-tête facultatifs, ainsi que par la table des certificats d’attribut associée. Lors de la vérification des signatures, les informations spécifiées dans ces structures sont utilisées pour localiser la signature d’un fichier PE. Si ces valeurs sont endommagées, il est possible que la signature d’un fichier paraisse non valide. 

Pour que la signature Authenticode soit correcte, les conditions suivantes doivent être réunies :

- Le début de l’entrée **WIN_CERTIFICATE** dans le fichier PE ne peut pas dépasser la fin de l’exécutable
- L’entrée **WIN_CERTIFICATE** doit se trouver à la fin de l’image
- La taille de l’entrée **WIN_CERTIFICATE** doit être positive
- L’entrée **WIN_CERTIFICATE** doit commencer après la structure **IMAGE_NT_HEADERS32** pour les exécutables 32 bits et après la structure IMAGE_NT_HEADERS64 pour les exécutables 64 bits

Pour plus d’informations, reportez-vous à la [spécification relative à l’exécutable portable Authenticode](http://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/Authenticode_PE.docx) et à la [spécification relative au format de fichier PE](https://msdn.microsoft.com/windows/hardware/gg463119.aspx). 

Notez que SignTool.exe peut créer une liste des fichiers binaires endommagés ou mal formés lorsque vous tentez de signer un package AppX. Pour ce faire, activez la journalisation détaillée en définissant la variable d’environnement APPXSIP_LOG sur 1 (par exemple, ```set APPXSIP_LOG=1```), puis réexécutez SignTool.exe.

Pour réparer ces fichiers binaires endommagés, assurez-vous qu’ils respectent les exigences ci-dessus.

## <a name="see-also"></a>Voir également

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)
- [SignTool.exe (Sign Tool)](https://msdn.microsoft.com/library/8s9b9yaz.aspx)
- [Comment signer un package d’application à l’aide de SignTool](https://msdn.microsoft.com/library/windows/desktop/jj835835.aspx)
