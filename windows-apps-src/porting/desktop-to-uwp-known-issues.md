---
author: normesta
Description: This article contains known issues with the Desktop Bridge.
Search.Product: eADQiWindows 10XVcnh
title: Problèmes connus (Pont du bureau)
ms.author: normesta
ms.date: 06/20/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
ms.localizationpriority: medium
ms.openlocfilehash: 50a455dc43007a433bfabd995af7968e93fe1900
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4471195"
---
# <a name="known-issues-with-packaged-desktop-applications"></a>Problèmes connus avec les applications de bureau empaquetées

Cet article présente les problèmes pouvant se produire lorsque vous créez un package d’application Windows pour votre application de bureau.

<a id="app-converter" />

## <a name="known-issues-with-the-desktop-app-converter"></a>Problèmes connus avec DesktopAppConverter

### <a name="ecreatingisolatedenvfailed-an-estartingisolatedenvfailed-errors"></a>Erreurs E_CREATING_ISOLATED_ENV_FAILED et E_STARTING_ISOLATED_ENV_FAILED    

Si vous êtes confronté à l’une de ces erreurs, assurez-vous que vous utilisez une image de base valide à partir du [Centre de téléchargement](https://aka.ms/converterimages).
Si vous utilisez une image de base valide, essayez d’utiliser ``-Cleanup All`` dans votre commande.
Si cela ne fonctionne pas, envoyez-nous vos journaux à l’adresse converter@microsoft.com pour nous aider à examiner le problème.

### <a name="new-containernetwork-the-object-already-exists-error"></a>New-ContainerNetwork: erreur, l’objet existe déjà

Vous pouvez être confronté à cette erreur lorsque vous configurez une nouvelle image de base. Cela peut se produire si vous utilisez une version d’évaluation de Windows Insider sur une machine de développement où était auparavant installé Desktop App Converter.

Pour résoudre ce problème, essayez d’exécuter la commande `Netsh int ipv4 reset` depuis une invite de commandes aux privilèges élevés, puis redémarrez votre machine.

### <a name="your-net-application-is-compiled-with-the-anycpu-build-option-and-fails-to-install"></a>Votre application .NET est compilée avec l’option de build «AnyCPU» et ne parvient pas à installer

Cela peut se produire si le fichier exécutable principal ou l’une de ses dépendances ont été placés n’importe où dans la hiérarchie de dossiers **Programmes** ou **Windows\System32**.

Pour résoudre ce problème, essayez d’utiliser un programme d’installation spécifique à votre architecture (32bits ou 64bits) pour générer un package d’application Windows.

### <a name="publishing-public-side-by-side-fusion-assemblies-wont-work"></a>La publication d’assemblys Fusion côte à côte publics ne fonctionne pas

 Pendant l’installation, une application peut publier des assemblys Fusion côte à côte publics, accessibles à tout autre processus. Lors de la création de contexte d’activation des processus, ces assemblys sont récupérés par un processus système nommé CSRSS.exe. Dans le cas d’un processus converti, la création de contexte d’activation et le chargement de modules de ces assemblys échouent. Les assemblys Fusion côte à côte sont enregistrés dans les emplacements suivants:
  + Registre: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + Système de fichiers: %windir%\\SideBySide

Il s’agit d’une limitation connue et il n’existe actuellement aucune solution de contournement. Ceci dit, les assemblys Inbox, comme ComCtl, sont livrés avec le système d’exploitation. Ainsi, la prise de dépendance sur eux est sûre.

### <a name="error-found-in-xml-the-executable-attribute-is-invalid---the-value-myappexe-is-invalid-according-to-its-datatype"></a>Erreur détectée dans XML. L’attribut «Exécutable» n’est pas valide, la valeur «MyApp.EXE» n’est pas valide en fonction du type de ses données

Cela peut se produire si les fichiers exécutables de votre application ont une extension **. EXE** en majuscules. Bien que la casse de cette extension n’affecte l’exécution de votre application, cela peut amener le DAC à générer cette erreur.

Pour résoudre ce problème, essayez de spécifier l’indicateur **-AppExecutable** lors de la création de package et utilisez les minuscules «.exe» comme extension de votre fichier exécutable principal (par exemple: MONAPP.exe).    Par ailleurs, vous pouvez modifier la casse de tous les fichiers exécutables dans votre application à partir de minuscules en majuscules (par exemple: à partir de. EXE vers .exe).

### <a name="corrupted-or-malformed-authenticode-signatures"></a>Signatures Authenticode endommagées ou mal formées

Cette section illustre comment identifier des problèmes dans les fichiers au format exécutable portable (PE) de votre package d’application Windows, qui peuvent contenir des signatures Authenticode endommagées ou mal formées. Toute signature Authenticode non valide de vos fichiers PE, qui peuvent être sous tout format binaire (par exemple, .exe, .dll, .chm, etc.), empêchera la signature correcte de votre package. Ce dernier ne peut donc pas être déployé à partir d’un package d’application Windows.

L’emplacement de la signature Authenticode d’un fichier PE est spécifié par l’entrée de la table de certificats, qui se trouve dans les répertoires de données d’en-tête facultatifs, ainsi que par la table des certificats d’attribut associée. Lors de la vérification des signatures, les informations spécifiées dans ces structures sont utilisées pour localiser la signature d’un fichier PE. Si ces valeurs sont endommagées, il est possible que la signature d’un fichier paraisse non valide.

Pour que la signature Authenticode soit correcte, les conditions suivantes doivent être réunies:

- Le début de l’entrée **WIN_CERTIFICATE** dans le fichier PE ne peut pas dépasser la fin de l’exécutable
- L’entrée **WIN_CERTIFICATE** doit se trouver à la fin de l’image
- La taille de l’entrée **WIN_CERTIFICATE** doit être positive
- L’entrée **WIN_CERTIFICATE** doit commencer après la structure **IMAGE_NT_HEADERS32** pour les exécutables 32bits et après la structure IMAGE_NT_HEADERS64 pour les exécutables 64bits

Pour plus d’informations, reportez-vous à la [spécification relative à l’exécutable portable Authenticode](http://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/Authenticode_PE.docx) et à la [spécification relative au format de fichier PE](https://msdn.microsoft.com/windows/hardware/gg463119.aspx).

Notez que SignTool.exe peut créer une liste des fichiers binaires endommagés ou mal formés lorsque vous tentez de signer un package d’application Windows. Pour ce faire, activez la journalisation détaillée en définissant la variable d’environnement APPXSIP_LOG sur 1 (par exemple, ```set APPXSIP_LOG=1```), puis réexécutez SignTool.exe.

Pour réparer ces fichiers binaires endommagés, assurez-vous qu’ils respectent les exigences ci-dessus.

## <a name="you-receive-the-error----msb4018-the-generateresource-task-failed-unexpectedly"></a>Vous recevez l’erreur MSB4018: Échec inattendu de la tâche «GenerateResource»

Cela peut se produire lorsque vous essayez de convertir des assemblys satellites en fichiers d’index de ressource de package (IRP).

Nous avons connaissance de ce problème et nous efforçons de trouver une solution à plus long terme. Comme solution de contournement temporaire, vous pouvez désactiver le générateur de ressources en ajoutant cette ligne de code XML au premier élément PropertyGroup du fichier de projet d’hébergement:

``<AppxGeneratePrisForPortableLibrariesEnabled>false</AppxGeneratePrisForPortableLibrariesEnabled>``

## <a name="blue-screen-with-error-code-0x139-kernelsecuritycheckfailure"></a>Écran bleu avec code d’erreur 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

Après l’installation ou le lancement de certaines applications du Microsoft Store, votre ordinateur peut redémarrer de façon inattendue et afficher l’erreur: **0x139 (KERNEL\_SECURITY\_CHECK\_FAILURE)**.

Les applications concernées connues sont Kodi, JT2Go, Ear Trumpet, Teslagrad et d’autres.

La [mise à jour de Windows (version 14393.351 - KB3197954)](https://support.microsoft.com/kb/3197954) publiée le 27/10/2016 inclut des correctifs importants qui résolvent ce problème. Si vous rencontrez ce problème, mettez à jour votre ordinateur. Si vous n’êtes pas en mesure de mettre à jour votre ordinateur car il redémarre avant que vous puissiez vous connecter, vous devez restaurer le système à un point de récupération antérieur à l’installation de l’une des applications concernées. Pour plus d’informations sur l’utilisation de la restauration du système, consultez [Options de récupération dans Windows10](https://support.microsoft.com/help/12415/windows-10-recovery-options).

Si la mise à jour ne résout pas le problème ou si vous ne savez pas vraiment comment récupérer votre ordinateur, contactez le [Support Microsoft](https://support.microsoft.com/contactus/).

Si vous êtes développeur, vous ne voulez peut-être pas que votre application empaquetée soit installée sur des versions de Windows n’incluant pas cette mise à jour. Notez que dans ce cas, votre application ne sera pas disponible pour les utilisateurs qui n’ont pas encore installé la mise à jour. Pour limiter la disponibilité de votre application pour les utilisateurs qui ont installé cette mise à jour, modifiez le fichier AppxManifest.xml comme suit:

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

Vous trouverez les informations relatives à la mise à jour de Windows aux adresses suivantes:
* https://support.microsoft.com/kb/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history

## <a name="common-errors-that-can-appear-when-you-sign-your-app"></a>Erreurs courantes susceptibles de s’afficher lorsque vous signez votre application

### <a name="publisher-and-cert-mismatch-causes-signtool-error-error-signersign-failed--21470248850x8007000b"></a>La non-correspondance de l’éditeur et du certificat provoque une erreur Signtool «Erreur: SignerSign() Failed» (-2147024885/0x8007000b)

L’entrée Publisher dans le manifeste du package d’application Windows doit correspondre à l’objet du certificat avec lequel vous effectuez la signature.  Vous pouvez utiliser l’une des méthodes suivantes pour afficher l’objet du certificat.

**Option1: Powershell**

Exécutez la commande PowerShell suivante. Il est possible d’utiliser le format.cer ou.pfx comme fichier de certificat, car ils ont les mêmes informations d’éditeur.

```ps
(Get-PfxCertificate <cert_file>).Subject
```

**Option2: Explorateur de fichiers**

Double-cliquez sur le certificat dans l’Explorateur de fichiers. Sélectionnez ensuite l’onglet *Détails*, puis le champ *Objet* dans la liste. Vous pouvez alors copier le contenu.

**Option3: CertUtil**

Exécutez **certutil** à partir de la ligne de commande sur le fichierPFX, puis copier le champ *Sujet* à partir de la sortie.

```cmd
certutil -dump <cert_file.pfx>
```

<a id="bad-pe-cert" />

### <a name="bad-pe-certificate-0x800700c1"></a>Certificat PE incorrect (0x800700C1)

Cela peut se produire lorsque votre package contient un fichier binaire qui dispose d’un certificat endommagé. Voici quelques raisons pourquoi cela peut se produire:

* Le début du certificat n’est pas à la fin d’une image.  

* La taille du certificat n’est pas positive.

* Le début du certificat n’est pas après le `IMAGE_NT_HEADERS32` structure pour un exécutable 32 bits ou après le `IMAGE_NT_HEADERS64` structure pour un exécutable 64 bits.

* Le pointeur de certificat n’est pas correctement aligné pour une structure WIN_CERTIFICATE.

Pour rechercher des fichiers qui contiennent un certificat PE incorrect, ouvrez une **invite de commandes**et définissez la variable d’environnement nommée `APPXSIP_LOG` sur une valeur égale à 1.

```
set APPXSIP_LOG=1
```

Ensuite, à partir de l' **invite de commandes**, connectez-vous à votre application à nouveau. Exemple :

```
signtool.exe sign /a /v /fd SHA256 /f APPX_TEST_0.pfx C:\Users\Contoso\Desktop\pe\VLC.appx
```

Informations sur les fichiers qui contiennent un certificat PE incorrect seront affiche dans la **Fenêtre de Console**. Exemple :

```
...

ERROR: [AppxSipCustomLoggerCallback] File has malformed certificate: uninstall.exe

...   
```
## <a name="next-steps"></a>Étapes suivantes

**Trouvez des réponses à vos questions**

Des questions? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Transmettre des commentaires ou suggérer des fonctionnalités**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
