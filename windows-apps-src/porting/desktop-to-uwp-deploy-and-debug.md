---
author: awkoren
Description: "Déployez et déboguez une app UWP convertie à partir d’une application de bureau Windows (Win32, WPF, Windows Forms) avec les extensions de conversion de bureau."
Search.Product: eADQiWindows 10XVcnh
title: "Déployer et déboguer une application de plateforme Windows universelle (UWP) convertie à partir d’une application de bureau Windows"
translationtype: Human Translation
ms.sourcegitcommit: 2c1a8ea38081c947f90ea835447a617c388aec08
ms.openlocfilehash: 75e176f17845bdbd618c6ca63fbbb5765bef54fb

---

# Déployer et déboguer votre application UWP convertie

\[Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.\]

Cette rubrique contient des informations pour vous aider à réussir le déploiement et le débogage de votre application après sa conversion. Si vous êtes curieux de connaître certains éléments internes des extensions de conversion de bureau, alors cette rubrique est faite pour vous.

## Déboguer votre application UWP convertie

Plusieurs options s’offrent à vous pour le débogage de votre application convertie.

### Attacher au processus

Lorsque Microsoft Visual Studio est exécuté «en tant qu’administrateur», les commandes __Démarrer le débogage__ et __Exécuter sans débogage__ fonctionnent pour un projet d’application convertie, mais l’application lancée s’exécute avec [un niveau d’intégrité moyen](https://msdn.microsoft.com/library/bb625963). Autrement dit, elle ne disposera _pas_ de privilèges élevés. Pour conférer des privilèges d’administrateur sur l’application lancée, vous devez d’abord la lancer «en tant qu’administrateur» via un raccourci ou une vignette. Une fois que l’application est exécutée, à partir d’une instance de Microsoft Visual Studio exécutée «en tant qu’administrateur», appelez __Attacher au processus__, puis sélectionnez le processus de votre application dans la boîte de dialogue.

### Débogage F5

Désormais, Visual Studio prend en charge un nouveau projet de création de packages qui vous permet de copier automatiquement toutes les mises à jour que vous effectuez lorsque vous concevez votre application dans le package AppX créé lorsque vous avez exécuté le convertisseur sur le programme d’installation de votre application. Une fois que vous configurez le projet de création de packages, vous pouvez également utiliser F5 pour déboguer directement dans le package AppX. 

Voici comment débuter: 

1. Tout d’abord, vérifiez que vous êtes prêt pour utiliser Desktop App Converter. Pour obtenir des instructions, voir [Desktop App Converter Preview (Aperçu de Desktop App Converter)](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter). 

2. Exécutez le convertisseur, puis le programme d’installation pour votre application Win32. Le convertisseur capture la disposition et toute modification apportée au Registre et génère un Appx avec manifeste et registery.dat pour virtualiser le Registre:

![alt](images/desktop-to-uwp/debug-1.png)

3. Installez et lancez [Visual Studio 2015 Preview 2](https://www.visualstudio.com/downloads/visual-studio-next-downloads-vs.aspx). 

4. À partir de la [Galerie Visual Studio](http://go.microsoft.com/fwlink/?LinkId=797871), installez le projet VSIX de création de packages du bureau vers UWP. 

5. Ouvrez la solution Win32 correspondante qui a été convertie dans Visual Studio.
 
6. Ajoutez le nouveau projet de création de packages à votre solution en cliquant avec le bouton droit sur la solution et en choisissant «Ajouter un nouveau projet». Ensuite, sous Configuration et déploiement, choisissez le projet de création de packages du bureau vers UWP:

    ![alt](images/desktop-to-uwp/debug-2.png)

    Le projet qui en résulte sera ajouté à votre solution:

    ![alt](images/desktop-to-uwp/debug-3.png)

    Dans le projet de création de packages, AppXFileList fournit un mappage des fichiers dans la disposition AppX. Les références démarrent vides, mais doivent être définies manuellement sur le projet .exe pour le classement des build. 

7. Le projet DesktopToUWPPackaging possède une page de propriété qui vous permet de configurer la racine du package AppX et la vignette à exécuter:

    ![alt](images/desktop-to-uwp/debug-4.png)

    Définissez PackageLayout sur l’emplacement racine de l’AppX qui a été créé par le convertisseur (ci-dessus). Ensuite, choisissez la vignette à exécuter.

8.  Ouvrez et modifiez AppXFileList.xml. Ce fichier définit la façon de copier la sortie de la build de débogage Win32 dans la disposition AppX conçue par le convertisseur. Par défaut, un espace réservé figure dans le fichier avec un exemple de balise et de commentaire:

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
      <ItemGroup>
    <!— Use the following syntax to copy debug output to the AppX layout
       <AppxPackagedFile Include="$(outdir)\App.exe">
          <PackagePath>App.exe</PackagePath>
        </AppxPackagedFile> 
        See http://etc...
    -->
      </ItemGroup>
    </Project>
    ```

    Voici un exemple de création de mappage. Dans ce cas, nous copions les fichiers .exe et .dll depuis l’emplacement de la build Win32 à l’emplacement de la disposition du package. 

    ```XML
    <?xml version="1.0" encoding=utf-8"?>
    <Project ToolsVersion=14.0" xmlns="http://scehmas.microsoft.com/developer/msbuild/2003">
        <PropertyGroup>
            <MyProjectOutputPath>C:\{path}</MyProjectOutputPath>
        </PropertyGroup>
        <ItemGroup>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.exe">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.exe</PackagePath>
            </LayoutFile>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.Models.dll">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.Models.dll</PackagePath>
            </LayoutFile>
        </ItemGroup>
    </Project>
    ```

    Le fichier est défini comme suit: 

    Tout d’abord, nous définissons *MyProjectOutputPath* de manière à désigner l’emplacement de création du projet Win32:

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="14.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        <PropertyGroup>
            <MyProjectOutputPath>C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\DesktopUWP</MyProjectOutputPath>
        </PropertyGroup>
    ```

    Ensuite, chaque *LayoutFile* spécifie un fichier à copier depuis l’emplacement de build Win32 vers la disposition de package Appx. Dans ce cas, un .exe et un .dll sont copiés. 

    ```XML
        <ItemGroup>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.exe">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.exe</PackagePath>
            </LayoutFile>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.Models.dll">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.Models.dll</PackagePath>
            </LayoutFile>
        </ItemGroup>
    </Project>
    ```

9. Définissez le projet de démarrage de création de packages. Cela copiera les fichiers Win32 dans l’AppX, puis lancera le débogueur quand le projet sera créé et exécuté.  

    ![alt](images/desktop-to-uwp/debug-5.png)

10. Enfin, vous pouvez maintenant définir un point d’arrêt dans le code Win32 et appuyer sur F5 pour lancer le débogueur. Cela copiera toutes les mises à jour que vous avez apportées à votre application Win32 sur le package AppX, et vous permettra de déboguer directement depuis Visual Studio.

11. Si vous mettez à jour votre application, vous devrez utiliser MakeAppX pour remettre en package votre application. Pour plus d’informations, consultez [Outil de création de packages (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx). 

Si vous disposez de plusieurs configurations de build (par exemple, pour la sortie et le débogage), vous pouvez ajouter ce qui suit au fichier AppXFileList.xml pour copier la version Win32 depuis différents emplacements:

```XML
<PropertyGroup>
    <MyProjectOutputPath Condition="$(Configuration) == 'DesktopUWP'">C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\DesktopUWP>
    </MyProjectOutputPath>
    <MyProjectOutputPath Condition="$(Configuration) == 'ReleaseDesktopUWP'"> C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\ReleaseDesktopUWP</MyProjectOutputPath>
</PropertyGroup>
```

Vous pouvez également utiliser la compilation conditionnelle pour activer des chemins de code particuliers si vous mettez à jour votre application vers UWP mais que vous souhaitez également toujours la générer pour Win32. 

1.  Dans l’exemple ci-dessous, le code sera compilé uniquement pour DesktopUWP et affichera une vignette à l’aide de l’API WinRT. 

    ```C#
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

2.  Vous pouvez utiliser Configuration Manager pour ajouter la nouvelle configuration de build:

    ![alt](images/desktop-to-uwp/debug-6.png)

    ![alt](images/desktop-to-uwp/debug-7.png)

3.  Ensuite, sous les propriétés du projet, ajoutez la prise en charge des symboles de compilation conditionnelle:

    ![alt](images/desktop-to-uwp/debug-8.png)

4.  Vous pouvez maintenant basculer la cible de build vers DesktopUWP si vous souhaitez créer pour cibler l’API UWP que vous avez ajoutée.

### PLMDebug 

Les options Visual Studio F5 et Attacher au processus sont utiles pour le débogage de votre application pendant qu’elle s’exécute. Dans certains cas, toutefois, vous pouvez souhaiter un contrôle plus fin sur le processus de débogage, notamment la possibilité de déboguer votre application avant son démarrage. Dans ces scénarios plus avancés, utilisez [**PLMDebug**](https://msdn.microsoft.com/library/windows/hardware/jj680085%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396). Cet outil vous permet de déboguer votre application convertie à l’aide du débogueur Windows et propose un contrôle total sur le cycle de vie de l’application, ce qui inclut la suspension, la reprise et l’arrêt. 

PLMDebug.exe est inclus dans le SDK Windows. Pour plus d’informations, voir [**PLMDebug**](https://msdn.microsoft.com/library/windows/hardware/jj680085%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396). 

### Exécution d’un autre processus à l’intérieur du conteneur d’un niveau de confiance totale 

Vous pouvez appeler des processus personnalisés à l’intérieur du conteneur d’un package d’application spécifié. Cela peut être utile pour les scénarios de tests (par exemple, si vous avez un atelier de test personnalisé et que vous voulez tester la sortie de l’application). Pour ce faire, utilisez l’applet de commande PowerShell ```Invoke-CommandInDesktopPackage```: 

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## Déployer votre application UWP convertie

Il existe 2façons de déployer votre application convertie: l’inscription de fichiers libres et le déploiement du packageappx. 

L’inscription de fichiers libres est utile à des fins de débogage quand les fichiers sont placés sur un disque dans un emplacement auquel vous pouvez facilement accéder et que vous pouvez facilement mettre à jour. De plus, elle ne nécessite pas de signature ou de certificat.  

Le déploiement du packageAppx permet de facilement déployer et charger de manière indépendante l’application entre plusieurs ordinateurs, mais nécessite que le package soit signé et que le certificat soit approuvé sur l’ordinateur.

### Inscription de fichier libres

Pour déployer votre application pendant le développement, exécutez l’applet de commande PowerShell suivante: 

```Add-AppxPackage –Register AppxManifest.xml```

Pour mettre à jour les fichiers .exe ou .dll de votre application, remplacez simplement les fichiers existants dans votre package par les nouveaux, augmentez le nombre de versions dans AppxManifest.xml, puis exécutez à nouveau la commande ci-dessus.

Points à prendre en considération: 

* Tout lecteur sur lequel vous installez votre application convertie doit être formaté au format NTFS.

* Une application convertie s’exécute toujours en tant qu’utilisateur interactif.

### Déploiement du package AppX 

Avant de déployer votre application, vous devez la signer avec un certificat. Pour plus d’informations sur la création d’un certificat, voir [Signature de votre package .Appx](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter#deploy-your-converted-appx). 

Voici comment vous importez un certificat que vous avez créé précédemment. Vous pouvez importer le certificat directement à l’aide de CERTUTIL, ou l’installer à partir d’un Appx que vous avez signé, comme le fera le client. 

Pour installer le certificat à l’aide de CERTUTIL, exécutez la commande suivante à partir d’une invite de commandes administrateur:

```cmd
Certutil -addStore TrustedPeople <testcert.cer>
```

Pour importer le certificat à partir de l’Appx, comme le ferait un client:

1.  Dans l’Explorateur de fichiers, cliquez avec le bouton droit sur un Appx que vous avez signé avec un certificat de test, puis choisissez **Propriétés** dans le menu contextuel.
2.  Cliquez ou appuyez sur l’onglet **Signatures numériques**.
3.  Cliquez ou appuyez sur le certificat et choisissez **Détails**.
4.  Cliquez ou appuyez sur **Afficher le certificat**.
5.  Cliquez ou appuyez sur **Installer le certificat**.
6.  Dans le groupe **Emplacement de stockage**, sélectionnez **Ordinateur local**.
7.  Cliquez ou appuyez sur **Suivant** et sur **OK** pour confirmer la boîte de dialogue UAC.
8.  Dans l’écran suivant de l’Assistant Importation du certificat, remplacez l’option sélectionnée par **Placer tous les certificats dans le magasin suivant**.
9.  Cliquez ou appuyez sur **Parcourir**. Dans la fenêtre Sélectionner un magasin de certificats, faites défiler et sélectionnez **Personnes autorisées**, puis cliquez ou appuyez sur **OK**.
10. Cliquez ou appuyez sur **Suivant**. Un nouvel écran s’affiche. Cliquez ou appuyez sur **Terminer**.
11. Une boîte de dialogue de confirmation doit s’afficher. Si tel est le cas, cliquez sur **OK**. Si une autre boîte de dialogue indique que le certificat pose problème, vous devrez peut-être résoudre les problèmes liés au certificat.

Remarque: Pour que Windows approuve le certificat, ce dernier doit se trouver dans le nœud **Certificats (Ordinateur local)&gt; Autorités de certification racines de confiance&gt; Certificats** ou dans le nœud **Certificats (Ordinateur local)&gt; Personnes autorisées&gt; Certificats**. Seuls les certificats figurant à ces deux emplacements peuvent valider les certificats de confiance dans le contexte de l’ordinateur local. Autrement, un message d’erreur ressemblant à la chaîne suivante s’affiche:
```CMD
"Add-AppxPackage : Deployment failed with HRESULT: 0x800B0109, A certificate chain processed,
but terminated in a rootcertificate which is not trusted by the trust provider.
(Exception from HRESULT: 0x800B0109) error 0x800B0109: The root certificate of the signature
in the app package must be trusted."
```

Maintenant que le certificat a été approuvé, il existe 2façons d’installer le package: par le biais de PowerShell ou en double-cliquant simplement sur le fichier de package Appx pour l’installer.  Pour effectuer l’installation par le biais de PowerShell, exécutez l’applet de commande suivante:

```powershell
Add-AppxPackage <MyApp>.appx
```

## En arrière-plan

Lorsque vous exécutez votre application convertie, votre package d’application UWP est lancé à partir de \Program Files\WindowsApps\\&lt;_nom du package_&gt;\\&lt;_nom de l’application_&gt;.exe. Si vous cherchez à cet emplacement, vous verrez que votre application possède un manifeste du package de l’application (nommé AppxManifest.xml), qui fait référence à un espace de noms xml spécial, utilisé pour les applications converties. À l’intérieur de ce fichier manifeste, vous trouverez un élément __&lt;EntryPoint&gt;__, qui fait référence à une application de confiance totale. Lorsque cette application est lancée, elle ne s’exécute pas à l’intérieur d’un conteneur d’applications, mais comme un utilisateur le ferait normalement.

Toutefois, l’application s’exécute dans un environnement spécial dans lequel tous les accès de l’application au système de fichiers et au Registre sont redirigés. Le fichier nommé Registry.dat est utilisé pour la redirection du Registre. Il s’agit en fait d’une ruche du Registre, vous pouvez donc l’afficher dans l’Éditeur du Registre Windows (Regedit). Notez que ce mécanisme signifie que vous ne pouvez pas utiliser le Registre pour la communication entre processus. Le Registre n’a pas été conçu et n’est pas adapté pour cette pratique dans tous les cas. En ce qui concerne le système de fichiers, le seul élément redirigé est le dossier AppData; il est redirigé vers le même emplacement que les données d’application stockées pour toutes les applications UWP. Cet emplacement est connu comme le magasin de données de l’application locale, et vous y accédez à l’aide de la propriété [ApplicationData.LocalFolder](https://msdn.microsoft.com/library/windows/apps/br241621). De cette façon, votre code est déjà porté pour lire et écrire des données d’application à l’emplacement approprié sans action de votre part. Vous pouvez également écrire directement à cet emplacement. L’un des avantages de la redirection du système de fichiers est une expérience de désinstallation plus fluide.

À l’intérieur d’un dossier nommé VFS, vous verrez des dossiers qui contiennent les fichiers DLL sur lesquels votre application a des dépendances. Ces DLL sont installés dans les dossiers système pour la version de bureau classique de votre application. Toutefois, pour l’application UWP, les fichiers DLL sont installés en local sur votre application. Ainsi, il n’existe aucun problème de gestion de version lors de l’installation et de la désinstallation des applications UWP.

### Emplacements de VFS empaquetés

Le tableau suivant montre où les fichiers intégrés à votre package sont superposés sur le système de l’application. Votre application détectera ces fichiers comme se trouvant dans les emplacements système répertoriés, alors qu’en réalité, ils se trouvent dans les emplacements redirigés à l’intérieur de [Racine du package]\VFS\. Les emplacements FOLDERID proviennent des constantes [**KNOWNFOLDERID**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd378457.aspx).

Emplacement système | Emplacement redirigé (sous [Racine du package] \VFS\) | Valide sur les architectures
 :---- | :---- | :---
FOLDERID_SystemX86 | SystemX86 | x86, amd64 
FOLDERID_System | SystemX64 | amd64 
FOLDERID_ProgramFilesX86 | ProgramFilesX86 | x86, amd6 
FOLDERID_ProgramFilesX64 | ProgramFilesX64 | amd64 
FOLDERID_ProgramFilesCommonX86 | ProgramFilesCommonX86 | x86, amd64
FOLDERID_ProgramFilesCommonX64 | ProgramFilesCommonX64 | amd64 
FOLDERID_Windows | Windows | x86, amd64 
FOLDERID_ProgramData | Common AppData | x86, amd64 
FOLDERID_System\catroot | AppVSystem32Catroot | x86, amd64 
FOLDERID_System\catroot2 | AppVSystem32Catroot2 | x86, amd64 
FOLDERID_System\drivers\etc | AppVSystem32DriversEtc | x86, amd64 
FOLDERID_System\driverstore | AppVSystem32Driverstore | x86, amd64 
FOLDERID_System\logfiles | AppVSystem32Logfiles | x86, amd64 
FOLDERID_System\spool | AppVSystem32Spool | x86, amd64 

## Voir aussi
[Convertir votre application de bureau en une application UWP](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-root)

[Desktop App Converter Preview (Aperçu de Desktop App Converter)](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter)

[Convertir manuellement votre application de bureau Windows en une application de plateforme Windows universelle (UWP)](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-manual-conversion)

[Pont d’application de bureau pour les exemples de code UWP dans GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)


<!--HONumber=Sep16_HO2-->


