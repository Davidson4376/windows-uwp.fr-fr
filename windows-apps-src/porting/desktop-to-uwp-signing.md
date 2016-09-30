# Signer votre application de bureau convertie

Cet article explique comment signer une application de bureau que vous avez convertie à la plateforme Windows universelle (UWP). Vous devez signer votre package .appx avec un certificat avant de pouvoir le déployer.

## Signer votre package .appx

Tout d’abord, créez un certificat à l’aide de MakeCert.exe. Si vous êtes invité à entrer un mot de passe, sélectionnez «Aucun». 

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

Pour plus d’informations, voir [Comment signer un package d’application à l’aide de SignTool](https://msdn.microsoft.com/en-us/library/windows/desktop/jj835835(v=vs.85).aspx). 

Les trois outils ci-dessus sont inclus dans le Kit de développement logiciel (SDK) de MicrosoftWindows10. Pour les appeler directement, appelez le script ```C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\Tools\VsDevCmd.bat``` à l’aide d’une invite de commandes.

## Erreurs courantes

### Signatures Authenticode endommagées ou mal formées

Cette section illustre comment identifier des problèmes dans les fichiers au format exécutable portable (PE) de votre package AppX, qui peuvent contenir des signatures endommagées ou mal formées. Toute signature Authenticode non valide de vos fichiers PE, qui peuvent être au n’importe quel format binaire (par exemple, .exe, .dll, .chm, etc.), empêchera la signature correcte de votre package. Ce dernier ne peut donc pas être déployé à partir d’un package AppX. 

L’emplacement de la signature Authenticode d’un fichier PE est spécifié par l’entrée de la table de certificats, qui se trouve dans les répertoires de données d’en-tête facultatifs, ainsi que par la table des certificats d’attribut associée. Lors de la vérification des signatures, les informations spécifiées dans ces structures sont utilisées pour localiser la signature d’un fichier PE. Si ces valeurs sont endommagées, il est possible que la signature d’un fichier paraisse non valide. 

Pour que la signature Authenticode soit correcte, les conditions suivantes doivent être réunies:

- Le début de l’entrée **WIN_CERTIFICATE** dans le fichier PE ne peut pas dépasser la fin de l’exécutable
- L’entrée **WIN_CERTIFICATE** doit se trouver à la fin de l’image
- La taille de l’entrée **WIN_CERTIFICATE** doit être positive
- L’entrée **WIN_CERTIFICATE** doit commencer après la structure **IMAGE_NT_HEADERS32** pour les exécutables 32bits et après la structure IMAGE_NT_HEADERS64 pour les exécutables 64bits

Pour plus d’informations, reportez-vous à la [spécification relative à l’exécutable portable Authenticode](http://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/Authenticode_PE.docx) et à la [spécification relative au format de fichier PE](https://msdn.microsoft.com/en-us/windows/hardware/gg463119.aspx). 

Notez que SignTool.exe peut créer une liste des fichiers binaires endommagés ou mal formés lorsque vous tentez de signer un package AppX. Pour ce faire, activez la journalisation détaillée en définissant la variable d’environnement APPXSIP_LOG sur 1 (par exemple, ```set APPXSIP_LOG=1```), puis réexécutez SignTool.exe.

Pour réparer ces fichiers binaires endommagés, assurez-vous qu’ils respectent les exigences ci-dessus.

## Voir également

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx)
- [SignTool.exe (Sign Tool)](https://msdn.microsoft.com/library/8s9b9yaz(v=vs.110).aspx)
- [Comment signer un package d’application à l’aide de SignTool](https://msdn.microsoft.com/en-us/library/windows/desktop/jj835835(v=vs.85).aspx)

<!--HONumber=Jun16_HO5-->


