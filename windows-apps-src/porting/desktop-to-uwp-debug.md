---
author: normesta
Description: Run your packaged app and see how it looks without having to sign it. Then, set breakpoints and step through code. When you're ready to test your app in a production environment, sign your app and then install it.
Search.Product: eADQiWindows 10XVcnh
title: Exécuter, déboguer et tester une application de bureau mise en package (Pont du bureau)
ms.author: normesta
ms.date: 08/31/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.localizationpriority: medium
ms.openlocfilehash: b5110eebde087593f07704e89c2e4708b2fcbb8b
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4315165"
---
# <a name="run-debug-and-test-a-packaged-desktop-application"></a>Exécuter, déboguer et tester une application de bureau empaquetée

Exécutez votre application empaquetée et examinez-la sans avoir à vous connecter. Ensuite, définissez des points d’arrêt et parcourez le code. Lorsque vous êtes prêt à tester votre application dans un environnement de production, signez votre application et puis installez-le. Cette rubrique vous montre comment effectuer chacune de ces opérations.

<a id="run-app" />

## <a name="run-your-application"></a>Exécutez votre application

Vous pouvez exécuter votre application pour le tester localement sans avoir à obtenir un certificat et la signer. Votre manière d’exécuter l’application dépend de l’outil utilisé pour créer le package.

### <a name="you-created-the-package-by-using-visual-studio"></a>Vous avez créé le package à l'aide de VisualStudio

Définissez le projet de mise en package comme projet de démarrage, puis appuyez sur CTRL+F5 pour démarrer votre application.

### <a name="you-created-the-package-manually-or-by-using-the-desktop-app-converter"></a>Vous avez créé le package manuellement ou en utilisant le DesktopAppConverter

Ouvrez l'invite de commande WindowsPowerShell et à partir du sous-dossier **PackageFiles** de votre dossier de sortie, exécutez cette applet de commande:

```
Add-AppxPackage –Register AppxManifest.xml
```
Pour démarrer votre application, sélectionnez-la dans le menu Démarrer de Windows.

![Application empaquetée dans le menu Démarrer](images/desktop-to-uwp/converted-app-installed.png)

> [!NOTE]
> Une application empaquetée toujours s’exécute en tant qu’utilisateur interactif, et tout lecteur sur lequel vous installez votre application empaquetée à doit être formaté au format NTFS.

## <a name="debug-your-app"></a>Déboguer votre application

Comment déboguer l’application dépend de l’outil utilisé pour créer le package.

Si vous avez créé votre package à l’aide d’un [nouveau projet de mise en package](desktop-to-uwp-packaging-dot-net.md#new-packaging-project) disponible dans la mise à jour 15.4 de VisualStudio2017, définissez simplement le projet de mise en package en tant que projet de démarrage, puis appuyez sur la touche F5 pour déboguer votre application.

Si vous avez créé votre package à l'autre d'un autre outil, appliquez les étapes suivantes.

1. Assurez-vous que vous démarrez votre application empaquetée au moins une fois pour qu’il est installé sur votre ordinateur local.

   Consultez la section [Exécuter votre application](#run-app) ci-dessus.

2. Démarrez Visual Studio.

   Si vous souhaitez déboguer votre application avec des autorisations élevées, démarrez Visual Studio à l’aide de l’option **Exécuter en tant qu’administrateur** .

3. Dans Visual Studio, choisissez **Déboguer**->**Autres cibles de débogage**->**Déboguer le package d'application installé**.

4. Dans la liste **Packages d’application installés**, sélectionnez votre package d’application, puis choisissez le bouton **Lier**.

#### <a name="modify-your-application-in-between-debug-sessions"></a>Modifier votre application entre les sessions de débogage

Si vous apportez vos modifications à votre application pour corriger des bogues, remettez-la à l’aide de l’outil MakeAppx. Consultez [Exécuter l’outil MakeAppX](desktop-to-uwp-manual-conversion.md#make-appx).

### <a name="debug-the-entire-application-lifecycle"></a>Déboguer le cycle de vie de l’ensemble de l’application

Dans certains cas, vous souhaiterez souhaiter un contrôle sur le processus de débogage, notamment la possibilité de déboguer votre application avant son démarrage.

Vous pouvez utiliser [PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) pour obtenir un contrôle total sur le cycle de vie d’application, y compris la suspension, la reprise et l’arrêt.

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) est inclus dans le SDK Windows.

## <a name="test-your-app"></a>Tester votre application

Pour tester votre application dans un paramètre réaliste lorsque vous vous préparez pour la distribution, il est préférable de signer votre application, puis installez-le.

### <a name="test-an-application-that-you-packaged-by-using-visual-studio"></a>Tester une application qui vous un package à l’aide de Visual Studio

Visual Studio signe votre application à l’aide d’un certificat de test. Vous trouverez ce certificat dans le dossier de sortie généré par l'assistant **Créer des packages d’application**. Le fichier de certificat présente l’extension *.cer* et vous devrez installer ce certificat dans le magasin **d’Autorités de Certification racine de confiance** sur le PC auquel vous souhaitez tester votre application sur. Consultez [Chargez de manière indépendante votre package d’application](../packaging/packaging-uwp-apps.md#sideload-your-app-package).

### <a name="test-an-application-that-you-packaged-by-using-the-desktop-app-converter-dac"></a>Tester une application qui vous un package à l’aide de Desktop App Converter (DAC)

Si vous créez un package votre application à l’aide de Desktop App Converter, vous pouvez utiliser le ``sign`` paramètre pour signer automatiquement votre application à l’aide d’un certificat généré. Vous devrez installer ce certificat, puis installer l’application. Consultez [Exécuter l’application mise en package](desktop-to-uwp-run-desktop-app-converter.md#run-app).   


### <a name="manually-sign-apps-optional"></a>Signer les applications manuellement (facultatif)

Vous pouvez également signer votre application manuellement. Voici comment procéder:

1. Créez un certificat. Voir [Créer un certificat](../packaging/create-certificate-package-signing.md).

2. Installez ce certificat dans le magasin de certificats **racine de confiance** ou **personnes autorisées** sur votre système.

3. Signer votre application à l’aide de ce certificat, voir [signer un package d’application à l’aide de SignTool](../packaging/sign-app-package-using-signtool.md).

  > [!IMPORTANT]
  > Assurez-vous que le nom de l’éditeur mentionné sur votre certificat correspond à celui de l’éditeur de votre application.

    **Exemple connexe**

    [SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-application-for-windows-10-s"></a>Tester votre application pour Windows 10 S

Avant de publier votre application, assurez-vous qu’elle fonctionne correctement sur les appareils qui exécutent Windows 10 S. En fait, si vous prévoyez de publier votre application dans le Microsoft Store, vous devez le faire dans la mesure où il s’agit d’une exigence du store. Les applications qui ne fonctionnent pas correctement sur les appareils exécutant Windows10 S ne seront pas certifiées.

Consultez le [Test de votre application Windows pour Windows 10 S](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-test-windows-s).

### <a name="run-another-process-inside-the-full-trust-container"></a>Exécuter un autre processus à l’intérieur du conteneur d’un niveau de confiance totale

Vous pouvez appeler des processus personnalisés à l’intérieur du conteneur d’un package d’application spécifié. Cela peut être utile pour les scénarios de tests (par exemple, si vous avez un atelier de test personnalisé et que vous voulez tester la sortie de l’application). Pour ce faire, utilisez l’applet de commande PowerShell ```Invoke-CommandInDesktopPackage```:

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>Étapes suivantes

**Trouvez des réponses à vos questions**

Des questions? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Transmettre des commentaires ou suggérer des fonctionnalités**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
