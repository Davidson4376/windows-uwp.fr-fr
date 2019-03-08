---
Description: Exécutez votre application empaquetée et examinez-la sans la signer. Ensuite, définissez des points d’arrêt et parcourez le code. Lorsque vous êtes prêt à tester votre application dans un environnement de production, signez votre application, puis installez-la.
Search.Product: eADQiWindows 10XVcnh
title: Exécuter, déboguer et tester une application de bureau mise en package (Pont du bureau)
ms.date: 08/31/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.localizationpriority: medium
ms.openlocfilehash: 8b2350c8164548121baec231335e747166f1c082
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589754"
---
# <a name="run-debug-and-test-a-packaged-desktop-application"></a>Exécuter, déboguer et tester une application de bureau empaquetée

Exécutez votre application empaquetée et vérifier son fonctionnement sans avoir à signer. Ensuite, définissez des points d’arrêt et parcourez le code. Lorsque vous êtes prêt à tester votre application dans un environnement de production, signer votre application et puis installez-le. Cette rubrique vous montre comment effectuer chacune de ces opérations.

<a id="run-app" />

## <a name="run-your-application"></a>Exécutez votre application

Vous pouvez exécuter votre application pour le tester localement sans avoir à obtenir un certificat et signer. Mode d’exécution de l’application dépend de ce que l’outil vous permet de créer le package.

### <a name="you-created-the-package-by-using-visual-studio"></a>Vous avez créé le package à l'aide de Visual Studio

Définissez le projet de mise en package comme projet de démarrage, puis appuyez sur CTRL+F5 pour démarrer votre application.

### <a name="you-created-the-package-manually-or-by-using-the-desktop-app-converter"></a>Vous avez créé le package manuellement ou en utilisant le Desktop App Converter

Ouvrez l'invite de commande Windows PowerShell et à partir du sous-dossier **PackageFiles** de votre dossier de sortie, exécutez cette applet de commande :

```
Add-AppxPackage –Register AppxManifest.xml
```
Pour démarrer votre application, sélectionnez-la dans le menu Démarrer de Windows.

![Application empaquetée dans le menu Démarrer](images/desktop-to-uwp/converted-app-installed.png)

> [!NOTE]
> Une application empaquetée toujours s’exécute comme un utilisateur interactif, et n’importe quel lecteur que vous installez votre application empaquetée à doit être formaté au format NTFS.

## <a name="debug-your-app"></a>Déboguer votre application

Comment déboguer l’application dépend de ce que l’outil vous permet de créer le package.

Si vous avez créé votre package à l’aide d’un [nouveau projet de mise en package](desktop-to-uwp-packaging-dot-net.md#new-packaging-project) disponible dans la mise à jour 15.4 de Visual Studio 2017, définissez simplement le projet de mise en package en tant que projet de démarrage, puis appuyez sur la touche F5 pour déboguer votre application.

Si vous avez créé votre package à l'autre d'un autre outil, appliquez les étapes suivantes.

1. Assurez-vous que vous démarrez votre application empaquetée au moins une fois qu’elle est installée sur votre ordinateur local.

   Consultez la section [Exécuter votre application](#run-app) ci-dessus.

2. Démarrez Visual Studio.

   Si vous souhaitez déboguer votre application avec des autorisations élevées, démarrez Visual Studio à l’aide de la **exécuter en tant qu’administrateur** option.

3. Dans Visual Studio, choisissez **Déboguer**->**Autres cibles de débogage**->**Déboguer le package d'application installé**.

4. Dans la liste **Packages d’application installés**, sélectionnez votre package d’application, puis choisissez le bouton **Lier**.

#### <a name="modify-your-application-in-between-debug-sessions"></a>Modifier votre application entre les sessions de débogage

Si vous apportez vos modifications à votre application pour corriger les bogues, vous recréez le package à l’aide de l’outil MakeAppx. Voir [Exécuter l’outil MakeAppX](desktop-to-uwp-manual-conversion.md#make-appx).

### <a name="debug-the-entire-application-lifecycle"></a>Déboguer le cycle de vie d’application dans son intégralité

Dans certains cas, vous souhaiterez contrôle plus fin sur le processus de débogage, y compris la possibilité de déboguer votre application avant son démarrage.

Vous pouvez utiliser [PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) pour obtenir un contrôle total sur le cycle de vie des applications, y compris la suspension, reprise et arrêt.

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) est inclus dans le SDK Windows.

## <a name="test-your-app"></a>Tester votre application

Pour tester votre application dans un paramètre réaliste comme vous préparer pour distribution, il est préférable signer votre application, puis installez-le.

### <a name="test-an-application-that-you-packaged-by-using-visual-studio"></a>Tester une application que vous avez compressé à l’aide de Visual Studio

Visual Studio connecte à votre application à l’aide d’un certificat de test. Vous trouverez ce certificat dans le dossier de sortie généré par l'assistant **Créer des packages d’application**. Le fichier de certificat a la *.cer* extension et vous devrez installer ce certificat dans le **Trusted Root Certification Authorities** stocker sur le PC que vous souhaitez tester votre application sur. Voir [Chargez de manière indépendante votre package d’application](../packaging/packaging-uwp-apps.md#sideload-your-app-package).

### <a name="test-an-application-that-you-packaged-by-using-the-desktop-app-converter-dac"></a>Tester une application que vous avez compressé à l’aide de la Desktop App Converter (DAC)

Si vous empaquetez votre application à l’aide de la de Desktop App Converter, vous pouvez utiliser le ``sign`` paramètre à se connecter automatiquement à l’aide d’un certificat généré votre application. Vous devrez installer ce certificat, puis installer l’application. Voir [Exécuter l’application empaquetée](desktop-to-uwp-run-desktop-app-converter.md#run-app).   


### <a name="manually-sign-apps-optional"></a>Signer les applications manuellement (facultatif)

Vous pouvez également signer votre application manuellement. Voici comment procéder :

1. Créez un certificat. Voir [Créer un certificat](../packaging/create-certificate-package-signing.md).

2. Installez ce certificat dans le magasin de certificats **racine de confiance** ou **personnes autorisées** sur votre système.

3. Signer votre application à l’aide de ce certificat, consultez [signer un package d’application à l’aide de SignTool](../packaging/sign-app-package-using-signtool.md).

  > [!IMPORTANT]
  > Assurez-vous que le nom de l’éditeur mentionné sur votre certificat correspond à celui de l’éditeur de votre application.

**Exemple**

[SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-application-for-windows-10-s"></a>Tester votre application pour Windows 10 S

Avant de publier votre application, assurez-vous qu’ils fonctionnent correctement sur les appareils qui exécutent Windows 10 S. En fait, si vous envisagez de publier votre application sur le Microsoft Store, vous devez le faire, car il s’agit d’une exigence de magasin. Les applications qui ne fonctionnent pas correctement sur les appareils exécutant Windows 10 S ne seront pas certifiées.

Consultez [tester votre application Windows pour Windows 10 S](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-test-windows-s).

### <a name="run-another-process-inside-the-full-trust-container"></a>Exécuter un autre processus à l’intérieur du conteneur d’un niveau de confiance totale

Vous pouvez appeler des processus personnalisés à l’intérieur du conteneur d’un package d’application spécifié. Cela peut être utile pour les scénarios de tests (par exemple, si vous avez un atelier de test personnalisé et que vous voulez tester la sortie de l’application). Pour ce faire, utilisez l’applet de commande PowerShell ```Invoke-CommandInDesktopPackage``` :

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>Étapes suivantes

**Trouvez des réponses à vos questions**

Des questions ? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Donner votre avis ou faire des suggestions de fonctionnalité**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
