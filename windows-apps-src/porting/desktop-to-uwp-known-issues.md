---
author: normesta
Description: "Cet article présente les problèmes connus avec le pont du bureau vers UWP."
Search.Product: eADQiWindows 10XVcnh
title: "Problèmes connus liés au pont du bureau vers UWP"
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
ms.openlocfilehash: 1923d7eb8d2d4956d68a10d3727251eee0398b06
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="desktop-to-uwp-bridge-known-issues"></a>Pont du bureau vers UWP: problèmes connus

Cet article présente les problèmes connus avec le pont du bureau vers UWP.

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
