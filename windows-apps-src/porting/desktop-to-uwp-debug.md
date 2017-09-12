---
author: normesta
Description: "Exécutez votre application empaquetée et examinez-la sans la signer. Ensuite, définissez des points d’arrêt et parcourez le code. Lorsque vous êtes prêt à tester votre application dans un environnement de production, signez votre application, puis installez-la."
Search.Product: eADQiWindows 10XVcnh
title: "Exécuter, déboguer et tester une application de bureau empaquetée (Pont du bureau)"
ms.author: normesta
ms.date: 06/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.openlocfilehash: c160fecc530a6366de48f4f2ecc24df2463c0469
ms.sourcegitcommit: 77bbd060f9253f2b03f0b9d74954c187bceb4a30
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/11/2017
---
# <a name="run-debug-and-test-a-packaged-desktop-app-desktop-bridge"></a>Exécuter, déboguer et tester une application de bureau empaquetée (Pont du bureau)

Exécutez votre application empaquetée et examinez-la sans la signer. Ensuite, définissez des points d’arrêt et parcourez le code. Lorsque vous êtes prêt à tester votre application dans un environnement de production, signez votre application, puis installez-la. Cette rubrique vous montre comment effectuer chacune de ces opérations.

<span id="run-app" />
## <a name="run-your-app"></a>Exécuter votre application

Vous pouvez exécuter votre application pour la tester localement sans certificat et la signer.

Si vous avez créé votre package à l’aide d’un projet UWP dans Visual Studio, définissez simplement le projet de création de package en tant que projet de démarrage, puis appuyez sur Ctrl + F5 pour démarrer votre application.

Si vous avez utilisé Desktop App Converter ou si vous créez le package de votre application manuellement, ouvrez une invite de commande Windows PowerShell et à partir du sous-dossier **PacakgeFiles** de votre dossier de sortie, exécutez la commande suivante:

```
Add-AppxPackage –Register AppxManifest.xml
```
Pour démarrer votre application, sélectionnez-la dans le menu Démarrer de Windows.

![Application empaquetée dans le menu Démarrer](images/desktop-to-uwp/converted-app-installed.png)

> [!NOTE]
> Une application empaquetée s’exécute toujours en tant qu’utilisateur interactif, et le lecteur sur lequel vous installez votre application empaquetée (quel qu’il soit) doit être formaté en NTFS.

## <a name="debug-your-app"></a>Déboguer votre application

Sélectionnez votre package dans une boîte de dialogue chaque fois que vous déboguez votre application ou installez une extension et déboguez votre application sans avoir à sélectionner votre package chaque fois que vous démarrez la session.

### <a name="debug-your-app-by-selecting-the-package"></a>Déboguer votre application en sélectionnant le package

Cette option a un faible temps d’installation, mais vous oblige à effectuer une étape supplémentaire à chaque démarrage de session de débogage.


1. Veillez à démarrer votre application empaquetée au moins une fois pour qu’elle soit installée sur votre ordinateur local.

   Consultez la section [Exécuter votre application](#run-app) ci-dessus.

2. Démarrez Visual Studio.

   Si vous souhaitez déboguer votre application avec des autorisations élevées, démarrez Visual Studio en utilisant l’option **Exécuter en tant qu’administrateur**.

3. Dans Visual Studio, choisissez **Déboguer**->**Autres cibles de débogage**->**Déboguer le package d'application installé**.

4. Dans la liste **Packages d’application installés**, sélectionnez votre package d’application, puis choisissez le bouton **Lier**.


### <a name="debug-your-app-without-having-to-select-the-package"></a>Déboguer votre application sans avoir à sélectionner le package

Cette option a la durée d’installation la plus élevée, mais vous n’aurez pas à sélectionner le package installé chaque fois que vous démarrez l’application. Vous devrez installer [Visual Studio2017](https://www.visualstudio.com/vs/whatsnew/) afin d’utiliser cette approche.

1. Tout d’abord, installez le [Projet de débogage Pont du bureau](http://go.microsoft.com/fwlink/?LinkId=797871).

2. Démarrez Visual Studio et ouvrez le projet d’application de bureau.

6. Ajoutez un projet de **Débogage Pont du bureau** à votre solution.

   Vous pouvez trouver le modèle de projet dans le groupe de modèles installés **Autres types de projets**.

    ![alt](images/desktop-to-uwp/debug-2.png)

    Le projet de **Débogage Pont du bureau** apparaîtra dans votre solution.

    ![alt](images/desktop-to-uwp/debug-3.png)

7. Ouvrez les pages de propriétés du projet de **Débogage Pont du bureau**.

8. Définissez le champ **Disposition de package** à l’emplacement de votre fichier manifeste de package (AppxManifest.xml) et choisissez le fichier exécutable de votre application à partir de la liste déroulante **Vignette de démarrage**.

     ![alt](images/desktop-to-uwp/debug-4.png)

8. Ouvrez le fichier AppXPackageFileList.xml dans l’éditeur de code.

9. Supprimez les commentaires sur le bloc de code XML et ajoutez les valeurs de ces éléments:

   **MyProjectOutputPath**: le chemin d’accès relatif au dossier de débogage de votre application de bureau.

   **LayoutFile**: le fichier exécutable qui se trouve dans le dossier de débogage de votre application de bureau.

   **PackagePath**: le nom de fichier complet de l’exécutable de votre application de bureau qui a été copié dans votre dossier de package d’application Windows pendant le processus de conversion.

    Voici un exemple:

    ```XML
  <?xml version="1.0" encoding="utf-8"?>
  <Project ToolsVersion="14.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <PropertyGroup>
     <MyProjectOutputPath>..\MyDesktopApp\bin\Debug</MyProjectOutputPath>
    </PropertyGroup>
    <ItemGroup>
      <LayoutFile Include="$(MyProjectOutputPath)\MyDesktopApp.exe">
        <PackagePath>$(PackageLayout)\MyDesktopApp.exe</PackagePath>
      </LayoutFile>
    </ItemGroup>
  </Project>
    ```

  Si votre application utilise des fichiers .dll qui sont générés à partir d’autres projets dans votre solution, et que vous souhaitez détailler le code qui est contenu dans les DLL, incluez un élément **LayoutFile** pour chacun des fichiers .dll.

  ```XML
  ...
      <LayoutFile Include="$(MyProjectOutputPath)\MyDesktopApp.Models.dll">
      <PackagePath>$(PackageLayout)\MyDesktopApp.Models.dll</PackagePath>
      </LayoutFile>
  ...
  ```

10. Définissez le projet de démarrage de création de package.  

    ![alt](images/desktop-to-uwp/debug-5.png)

11. Définissez des points d’arrêt dans votre code d’application de bureau, puis démarrez le débogueur.

  ![Bouton de débogage](images/desktop-to-uwp/debugger-button.png)

  Visual Studio copie les fichiers exécutables et les fichiers .dll que vous avez spécifiés dans le fichier XML à votre package d’application Windows, puis démarrez le débogueur.

#### <a name="handle-multiple-build-configurations"></a>Gérer plusieurs configurations de build

Si vous avez défini plusieurs configurations de build (par exemple: Release et Debug), vous pouvez modifier votre fichier AppXPackageFileList.xml pour copier uniquement les fichiers qui correspondent à la configuration de build choisie dans Visual Studio lorsque vous démarrez le débogueur.

En voici un exemple:

```XML
<PropertyGroup>
    <MyProjectOutputPath Condition="$(Configuration) == 'Debug'">..\MyDesktopApp\bin\Debug</MyProjectOutputPath>
    <MyProjectOutputPath Condition="$(Configuration) == 'Release'"> ..\MyDesktopApp\bin\Release</MyProjectOutputPath>
</PropertyGroup>
```

#### <a name="debug-uwp-enhancements-to-your-app"></a>Déboguer les améliorations UWP apportées à votre application

Vous voudrez peut-être améliorer votre application avec des expériences modernes telles que les vignettes dynamiques. Si vous le faites, vous pouvez utiliser la compilation conditionnelle pour activer les chemins de code avec des configurations de build spécifiques.

1. Tout d’abord, dans Visual Studio, définissez une configuration de build et donnez-lui un nom semblable à «DesktopUWP».

2. Dans l’onglet **Build** des propriétés de projet de votre projet, ajoutez ce nom dans le champ **Symboles de compilation conditionnelle**.

     ![alt](images/desktop-to-uwp/debug-8.png)

3. Ajouter des blocs de code conditionnels. Ce code est compilé uniquement pour la configuration de build **DesktopUWP**.

    ```csharp
    [Conditional("DesktopUWP")]
    private void showtile()
    {
        XmlDocument tileXml = TileUpdateManager.GetTemplateContent(TileTemplateType.TileSquare150x150Text01);
        XmlNodeList textNodes = tileXml.GetElementsByTagName("text");
        textNodes[0].InnerText = string.Format("Welcome to DesktopUWP!");
        TileNotification tileNotification = new TileNotification(tileXml);
        TileUpdateManager.CreateTileUpdaterForApplication().Update(tileNotification);
    }
    ```

### <a name="debug-the-entire-app-lifecycle"></a>Déboguer l’intégralité du cycle de vie de l’application

Dans certains cas, vous pouvez souhaiter un contrôle plus fin sur le processus de débogage, notamment la possibilité de déboguer votre application avant son démarrage.

Vous pouvez utiliser [PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) pour obtenir un contrôle total sur le cycle de vie de l’application, y compris la suspension, la reprise et la résiliation.

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) est inclus dans le SDK Windows.


### <a name="modify-your-app-in-between-debug-sessions"></a>Modifier votre application entre les sessions de débogage

Si vous apportez vos modifications à votre application pour corriger des bogues, remettez-la en package à l’aide de l’outil MakeAppx. Voir [Exécuter l’outil MakeAppX](desktop-to-uwp-manual-conversion.md#make-appx).

## <a name="test-your-app"></a>Tester votre application

Pour tester votre application dans un paramètre réaliste lorsque vous vous préparez pour la distribution, il est préférable de signer votre application, puis de l’installer.

Si vous avez créé un package de votre application à l’aide de Visual Studio, vous pouvez exécuter un script pour signer votre application, puis l’installer. Voir [Chargez de manière indépendante votre package d’application](../packaging/packaging-uwp-apps.md#sideload-your-app-package).

Si vous créez un package de votre application à l’aide de Desktop App Converter, vous pouvez utiliser le paramètre ``sign`` pour signer automatiquement votre application à l’aide d’un certificat généré. Vous devrez installer ce certificat, puis installer l’application. Voir [Exécuter l’application empaquetée](desktop-to-uwp-run-desktop-app-converter.md#run-app).   

Vous pouvez également signer votre application manuellement. Voici comment procéder:

1. Créez un certificat. Voir [Créer un certificat](../packaging/create-certificate-package-signing.md).

2. Installez ce certificat dans le magasin de certificats **racine de confiance** ou **personnes autorisées** sur votre système.

3. Signez votre application à l’aide d’un certificat, voir [Signer un package d’application à l’aide de SignTool](../packaging/sign-app-package-using-signtool.md).

  > [!IMPORTANT]
  > Assurez-vous que le nom de l’éditeur mentionné sur votre certificat correspond à celui de l’éditeur de votre application.

### <a name="related-sample"></a>Exemple connexe

[SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-app-for-windows-10-s"></a>Tester votre application pour Windows10 S

Avant de publier votre application, assurez-vous qu’elle fonctionne correctement sur les appareils qui exécutent Windows10 S. En réalité, si vous envisagez de publier votre application dans le Windows Store, vous devez le faire, car c'est une condition requise par le Windows store. Les applications qui ne fonctionnent pas correctement sur les appareils exécutant Windows10 S ne seront pas certifiées. 

Voir [Tester votre application pour Windows10 S](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-test-windows-s).

### <a name="run-another-process-inside-the-full-trust-container"></a>Exécuter un autre processus à l’intérieur du conteneur d’un niveau de confiance totale

Vous pouvez appeler des processus personnalisés à l’intérieur du conteneur d’un package d’application spécifié. Cela peut être utile pour les scénarios de tests (par exemple, si vous avez un atelier de test personnalisé et que vous voulez tester la sortie de l’application). Pour ce faire, utilisez l’applet de commande PowerShell ```Invoke-CommandInDesktopPackage```:

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>Étapes suivantes

**Trouver des réponses aux questions spécifiques**

Notre équipe contrôle ces [balises StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Envoyer vos commentaires concernant cet article**

Utilisez la section remarques ci-dessous.
