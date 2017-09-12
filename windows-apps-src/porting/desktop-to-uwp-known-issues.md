---
author: normesta
Description: "Cet article présente les problèmes liés au Pont du bureau."
Search.Product: eADQiWindows 10XVcnh
title: "Problèmes connus (Pont du bureau)"
ms.author: normesta
ms.date: 07/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
ms.openlocfilehash: bf0e81d4a6ff7bd091f25963b1cf26cdecc93636
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2017
---
# <a name="known-issues-desktop-bridge"></a>Problèmes connus (Pont du bureau)

Cet article présente les problèmes connus liés au Pont du bureau.

## <a name="blue-screen-with-error-code-0x139-kernelsecuritycheckfailure"></a>Écran bleu avec code d’erreur 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

Après l’installation ou le lancement de certaines applications du Windows Store, votre ordinateur peut redémarrer de façon inattendue et afficher l’erreur: **0x139 (KERNEL\_SECURITY\_CHECK\_FAILURE)**.

Les applications concernées connues sont Kodi, JT2Go, Ear Trumpet, Teslagrad et d’autres.

La [mise à jour de Windows (version 14393.351 - KB3197954)](https://support.microsoft.com/kb/3197954) publiée le 27/10/2016 inclut des correctifs importants qui résolvent ce problème. Si vous rencontrez ce problème, mettez à jour votre ordinateur. Si vous n’êtes pas en mesure de mettre à jour votre ordinateur car il redémarre avant que vous puissiez vous connecter, vous devez restaurer le système à un point de récupération antérieur à l’installation de l’une des applications concernées. Pour plus d’informations sur l’utilisation de la restauration du système, consultez [Options de récupération dans Windows10](https://support.microsoft.com/help/12415/windows-10-recovery-options).

Si la mise à jour ne résout pas le problème ou si vous ne savez pas vraiment comment récupérer votre ordinateur, contactez le [Support Microsoft](https://support.microsoft.com/contactus/).

Si vous êtes développeur, vous ne voulez peut-être pas que vos applications Desktop Bridge soient installées sur des versions de Windows n’incluant pas cette mise à jour. Notez que dans ce cas, votre application n’est pas disponible pour les utilisateurs n’ayant pas encore installé la mise à jour. Pour limiter la disponibilité de votre application aux utilisateurs qui ont installé cette mise à jour, modifiez le fichier AppxManifest.xml comme suit:

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

Exécutez **certutil** à partir de la ligne de commande sur le fichierPFX, puis copier le champ *Subject* à partir de la sortie.

```cmd
certutil -dump <cert_file.pfx>
```

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

<span id="known-issues-anchor" />
## <a name="known-issues-with-cvbnet-and-c-uwp-projects"></a>Problèmes connus avec les projets UWP en C#/VB.NET et C++

Si vous préférez utiliser un projetC# pour empaqueter votre application, vous devez prendre note des problèmes connus ci-après.

- La génération de l’application dans la fonctionnalité Débogage entraîne l’erreur suivante: _Microsoft.Net.CoreRuntime.targets(235,5): erreur: Les applications avec des exécutables de point d’entrée personnalisés ne sont pas prises en charge. Vérifiez l’attribut Executable de l’élément Application dans le manifeste du package._

  Pour résoudre ce problème, utilisez plutôt le mode Version finale.

- Les fichiers binaires Win32 stockés dans le dossier racine du projet UWP sont supprimés de la version finale. Le compilateur .NETNative supprimera ces fichiers du package final, ce qui entraînera une erreur de validation du manifeste, puisque le point d’entrée d’exécutable sera introuvable.

  Pour résoudre ce problème, créez un sous-dossier dans votre projet pour stocker les fichiers binaires Win32.


## <a name="you-receive-the-error----msb4018-the-generateresource-task-failed-unexpectedly"></a>Vous recevez l’erreur MSB4018: Échec inattendu de la tâche «GenerateResource»

Cela peut se produire lorsque vous essayez de convertir des assemblys satellites en fichiers d’index de ressource de package (IRP).

Nous avons connaissance de ce problème et nous efforçons de trouver une solution à plus long terme. Comme solution de contournement temporaire, vous pouvez désactiver le générateur de ressources en ajoutant cette ligne de code XML au premier élément PropertyGroup du fichier de projet d’hébergement:

``<AppxGeneratePrisForPortableLibrariesEnabled>false</AppxGeneratePrisForPortableLibrariesEnabled>``
