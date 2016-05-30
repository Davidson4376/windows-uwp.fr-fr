---
author: awkoren
Description: Déployez et déboguez une app UWP convertie à partir d’une application de bureau Windows (Win32, WPF, Windows Forms) avec les extensions de conversion de bureau.
Search.Product: eADQiWindows 10XVcnh
title: Déployer et déboguer une app UWP convertie depuis une app de bureau Windows
---

# Déployer et déboguer votre application UWP convertie (Projet Centennial)

\[Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.\]

Cette rubrique contient des informations pour vous aider à réussir le déploiement et le débogage de votre application après sa conversion. Si vous êtes curieux de connaître certains éléments internes des extensions de conversion de bureau, alors cette rubrique est faite pour vous.

## Déboguer votre application UWP convertie

Deux options principales s’offrent à vous pour le débogage de votre application convertie à l’aide de Visual Studio.

### Attacher au processus

Lorsque Microsoft Visual Studio est exécuté « en tant qu’administrateur », les commandes __Démarrer le débogage__ et __Exécuter sans débogage__ fonctionnent pour un projet d’application convertie, mais l’application lancée s’exécute avec [un niveau d’intégrité moyen](https://msdn.microsoft.com/library/bb625963). Autrement dit, elle ne disposera _pas_ de privilèges élevés. Pour conférer des privilèges d’administrateur sur l’application lancée, vous devez d’abord la lancer « en tant qu’administrateur » via un raccourci ou une vignette. Une fois que l’application est exécutée, à partir d’une instance de Microsoft Visual Studio exécutée « en tant qu’administrateur », appelez __Attacher au processus__, puis sélectionnez le processus de votre application dans la boîte de dialogue.

### Débogage F5

Désormais, Visual Studio prend en charge un nouveau projet de création de packages qui vous permet de copier automatiquement toutes les mises à jour que vous effectuez lorsque vous concevez votre application dans le package AppX créé lorsque vous avez exécuté le convertisseur sur le programme d’installation de votre application. Une fois que vous configurez le projet de création de packages, vous pouvez également utiliser F5 pour déboguer directement dans le package AppX. 

Voici comment débuter : 

1. Tout d’abord, vérifiez que vous êtes prêt pour utiliser Centennial. Pour obtenir des instructions, consultez [Aperçu du Convertisseur d’applications de bureau (projet Centennial)](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter). 

2. Exécutez le convertisseur, puis le programme d’installation pour votre application Win32. Le convertisseur capture la disposition et toute modification apportée au Registre et génère un Appx avec manifeste et registery.dat pour virtualiser le Registre :

![alt](images/desktop-to-uwp/debug-1.png)

3. Installez et lancez [Visual Studio 2015 Preview 2](https://www.visualstudio.com/downloads/visual-studio-next-downloads-vs.aspx). 

4. À partir de la [Galerie Visual Studio](http://go.microsoft.com/fwlink/?LinkId=797871), installez le projet VSIX de création de packages du bureau vers UWP. 

5. Ouvrez la solution Win32 correspondante qui a été convertie dans Visual Studio.
 
6. Ajoutez le nouveau projet de création de packages à votre solution en cliquant avec le bouton droit sur la solution et en choisissant « Ajouter un nouveau projet ». Ensuite, sous Configuration et déploiement, choisissez le projet de création de packages du bureau vers UWP :

    ![alt](images/desktop-to-uwp/debug-2.png)

    Le projet qui en résulte sera ajouté à votre solution :

    ![alt](images/desktop-to-uwp/debug-3.png)

    Dans le projet de création de packages, AppXFileList fournit un mappage des fichiers dans la disposition AppX. Les références démarrent vides, mais doivent être définies manuellement sur le projet .exe pour le classement des build. 

7. Le projet DesktopToUWPPackaging possède une page de propriété qui vous permet de configurer la racine du package AppX et la vignette à exécuter :

    ![alt](images/desktop-to-uwp/debug-4.png)

    Définissez PackageLayout sur l’emplacement racine de l’AppX qui a été créé par le convertisseur (ci-dessus). Ensuite, choisissez la vignette à exécuter.

8.  Ouvrez et modifiez AppXFileList.xml. Ce fichier définit la façon de copier la sortie de la build de débogage Win32 dans la disposition AppX conçue par le convertisseur. Par défaut, un espace réservé figure dans le fichier avec un exemple de balise et de commentaire :

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

    Le fichier est défini comme suit : 

    Tout d’abord, nous définissons *MyProjectOutputPath* de manière à désigner l’emplacement de création du projet Win32 :

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

11. Si vous mettez à jour votre application, vous devrez utiliser MakeAppX pour remettre en package votre application. Pour plus d’informations, consultez [Outil de création de packages (MakeAppx.exe)](https://msdn.microsoft.com/en-us/library/windows/desktop/hh446767(v=vs.85).aspx). 

Si vous disposez de plusieurs configurations de build (par exemple, pour la sortie et le débogage), vous pouvez ajouter ce qui suit au fichier AppXFileList.xml pour copier la version Win32 depuis différents emplacements :

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

2.  Vous pouvez utiliser Configuration Manager pour ajouter la nouvelle configuration de build :

    ![alt](images/desktop-to-uwp/debug-6.png)

    ![alt](images/desktop-to-uwp/debug-7.png)

3.  Ensuite, sous les propriétés du projet, ajoutez la prise en charge des symboles de compilation conditionnelle :

    ![alt](images/desktop-to-uwp/debug-8.png)

4.  Vous pouvez maintenant basculer la cible de build vers DesktopUWP si vous souhaitez créer pour cibler l’API UWP que vous avez ajoutée.

## Déployer votre application UWP convertie

Pour déployer votre application au cours du développement, exécutez l’applet de commande PowerShell suivante : 

```Add-AppxPackage –Register AppxManifest.xml```

Pour mettre à jour les fichiers .exe ou .dll de votre application, remplacez simplement les fichiers existants dans votre package par les nouveaux, augmentez le nombre de versions dans AppxManifest.xml, puis exécutez à nouveau la commande ci-dessus.

Points à prendre en considération : 

Tout lecteur sur lequel vous installez votre application convertie doit être formaté au format NTFS.

Une application convertie s’exécute toujours en tant qu’utilisateur interactif. Cela revêt une importance particulière pour une application .NET dont le manifeste spécifie un niveau d’exécution __requireAdministrator__. Si l’utilisateur interactif bénéficie de privilèges d’administrateur, une invite de contrôle de compte d’utilisateur apparaît _chaque fois que l’application est lancée_. Pour les utilisateurs standard, le démarrage de l’application échoue.

Si vous essayez d’exécuter l’applet de commande Add-AppxPackage sur un ordinateur où vous n’avez pas importé le certificat que vous avez créé, vous obtiendrez une erreur.

Voici comment vous importez un certificat que vous avez créé précédemment. Vous pouvez l’installer directement, ou à partir d’un appx que vous avez signé, comme le client le fera.
1.  Dans l’Explorateur de fichiers, cliquez avec le bouton droit sur un appx que vous avez signé avec un certificat de test, et choisissez **Propriétés** dans le menu contextuel.
2.  Cliquez ou appuyez sur l’onglet **Signatures numériques**.
3.  Cliquez ou appuyez sur le certificat et choisissez **Détails**.
4.  Cliquez ou appuyez sur **Afficher le certificat**.
5.  Cliquez ou appuyez sur **Installer le certificat**.
6.  Dans le groupe **Emplacement de stockage**, sélectionnez **Ordinateur local**.
7.  Cliquez ou appuyez sur **Suivant** et sur **OK** pour confirmer la boîte de dialogue UAC.
8.  Dans l’écran suivant de l’Assistant Importation du certificat, remplacez l’option sélectionnée par **Placer tous les certificats dans le magasin suivant**.
9.  Cliquez ou appuyez sur **Parcourir**. Dans la fenêtre Sélectionner un magasin de certificats, faites défiler et sélectionnez **Personnes autorisées**, puis cliquez ou appuyez sur **OK**.
10. Cliquez ou appuyez sur **Suivant**. Un nouvel écran s’affiche. Cliquez ou appuyez sur **Terminer**.
11. Une boîte de dialogue de confirmation doit s’afficher. Si tel est le cas, cliquez sur **OK**. Si une boîte de dialogue indique que le certificat pose problème, vous devrez peut-être résoudre des problèmes de certificat.

### Informations supplémentaires

Pour que Windows approuve le certificat, ce dernier doit se trouver dans le nœud **Certificats (Ordinateur local) &gt; Autorités de certification racines de confiance &gt; Certificats** ou dans le nœud **Certificats (Ordinateur local) &gt; Personnes autorisées &gt; Certificats**. Seuls les certificats figurant à ces deux emplacements peuvent valider les certificats de confiance dans le contexte de l’ordinateur local. Autrement, un message d’erreur ressemblant à la chaîne suivante apparaît :
```CMD
"Add-AppxPackage : Deployment failed with HRESULT: 0x800B0109, A certificate chain processed,
but terminated in a rootcertificate which is not trusted by the trust provider.
(Exception from HRESULT: 0x800B0109) error 0x800B0109: The root certificate of the signature
in the app package must be trusted."
```

### En arrière-plan

Lorsque vous exécutez votre application convertie, votre package d’application UWP est lancé à partir de \Program Files\WindowsApps\\&lt;_nom du package_&gt;\\&lt;_nom de l’application_&gt;.exe. Si vous cherchez à cet emplacement, vous verrez que votre application possède un manifeste du package de l’application (nommé AppxManifest.xml), qui fait référence à un espace de noms xml spécial, utilisé pour les applications converties. À l’intérieur de ce fichier manifeste, vous trouverez un élément __&lt;EntryPoint&gt;__, qui fait référence à une application de confiance totale. Lorsque cette application est lancée, elle ne s’exécute pas à l’intérieur d’un conteneur d’applications, mais comme un utilisateur le ferait normalement.

Toutefois, l’application s’exécute dans un environnement spécial dans lequel tous les accès de l’application au système de fichiers et au Registre sont redirigés. Le fichier nommé Registry.dat est utilisé pour la redirection du Registre. Il s’agit en fait d’une ruche du Registre, vous pouvez donc l’afficher dans l’Éditeur du Registre Windows (Regedit). Notez que ce mécanisme signifie que vous ne pouvez pas utiliser le Registre pour la communication entre processus. Le Registre n’a pas été conçu et n’est pas adapté pour cette pratique dans tous les cas. En ce qui concerne le système de fichiers, le seul élément redirigé est le dossier AppData ; il est redirigé vers le même emplacement que les données d’application stockées pour toutes les applications UWP. Cet emplacement est connu comme le magasin de données de l’application locale, et vous y accédez à l’aide de la propriété [ApplicationData.LocalFolder](https://msdn.microsoft.com/library/windows/apps/br241621). De cette façon, votre code est déjà porté pour lire et écrire des données d’application à l’emplacement approprié sans action de votre part. Vous pouvez également écrire directement à cet emplacement. L’un des avantages de la redirection du système de fichiers est une expérience de désinstallation plus fluide.

À l’intérieur d’un dossier nommé VFS, vous verrez des dossiers qui contiennent les fichiers DLL sur lesquels votre application a des dépendances. Ces DLL sont installés dans les dossiers système pour la version de bureau classique de votre application. Toutefois, pour l’application UWP, les fichiers DLL sont installés en local sur votre application. Ainsi, il n’existe aucun problème de contrôle lors de l’installation et de la désinstallation des applications UWP.


<!--HONumber=May16_HO2-->


