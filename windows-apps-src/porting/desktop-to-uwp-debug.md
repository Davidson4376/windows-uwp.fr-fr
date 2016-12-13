---
author: awkoren
Description: "Déployez et déboguez une application de plateforme Windows universelle (UWP) convertie à partir d’une application de bureau Windows (Win32, WPF, Windows Forms) à l’aide de Desktop to UWP Bridge."
Search.Product: eADQiWindows 10XVcnh
title: "Déboguer des applications converties avec Desktop Bridge"
translationtype: Human Translation
ms.sourcegitcommit: dba00371b29b3179a6dc3bdd96a092437331e61a
ms.openlocfilehash: 537ac8e83d5f54bf83ec0e05b71be354651000f2

---

# <a name="debug-apps-converted-with-the-desktop-bridge"></a>Déboguer des applications converties avec Desktop Bridge

Cette rubrique contient des informations pour vous aider à réussir le débogage de votre application après l’avoir convertie avec Desktop to UWP Bridge. Plusieurs options s’offrent à vous pour le débogage de votre application convertie.

## <a name="attach-to-process"></a>Attacher au processus

Lorsque Microsoft Visual Studio est exécuté «&nbsp;en tant qu’administrateur&nbsp;», les commandes *Démarrer le débogage* et *Exécuter sans débogage* fonctionnent pour un projet d’application convertie, mais l’application lancée s’exécute avec un [niveau d’intégrité moyen](https://msdn.microsoft.com/library/bb625963) (c’est-à-dire, qu’elle ne dispose pas de privilèges élevés). Pour conférer des privilèges d’administrateur sur l’application lancée, vous devez d’abord la lancer «&nbsp;en tant qu’administrateur&nbsp;» via un raccourci ou une vignette. Une fois que l’application est exécutée, à partir d’une instance de Microsoft Visual Studio exécutée «&nbsp;en tant qu’administrateur&nbsp;», appelez __Attacher au processus__, puis sélectionnez le processus de votre application dans la boîte de dialogue.

## <a name="f5-debug"></a>Débogage F5

Visual Studio prend désormais en charge un nouveau projet de création de packages. Le nouveau projet vous permet de copier automatiquement toutes les mises à jour lorsque vous concevez votre application dans le package AppX créé à partir du convertisseur dans le programme d’installation de votre application. Une fois le projet de création de packages configuré , vous pouvez également utiliser F5 pour déboguer directement dans le package AppX. 

>Remarque&nbsp;: vous pouvez également déboguer un package Appx en sélectionnant Déboguer&nbsp;-&gt; Autres cibles à déboguer&nbsp;-&gt; Déboguer le package d’application installé.

Voici comment faire&nbsp;: 

1. Tout d’abord, vérifiez que vous êtes prêt pour utiliser Desktop App Converter. Pour obtenir des instructions, consultez [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md).

2. Exécutez le convertisseur, puis le programme d’installation pour votre application Win32. Le convertisseur capture la disposition et toute modification apportée au Registre et génère un Appx avec manifeste et registery.dat pour virtualiser le Registre&nbsp;:

![alt](images/desktop-to-uwp/debug-1.png)

3. Installez et lancez [Visual Studio 2017 RC](https://www.visualstudio.com/downloads/#visual-studio-community-2017-rc). 

4. Dans la [Galerie Visual Studio](http://go.microsoft.com/fwlink/?LinkId=797871), installez le projet VSIX de création de packages du bureau vers UWP. 

5. Ouvrez la solution Win32 correspondante qui a été convertie dans Visual Studio.
 
6. Ajoutez le nouveau projet de création de packages à votre solution en cliquant avec le bouton droit sur la solution et en choisissant «&nbsp;Ajouter un nouveau projet&nbsp;». Ensuite, sous Configuration et déploiement, choisissez le projet de création de packages du bureau vers UWP&nbsp;:

    ![alt](images/desktop-to-uwp/debug-2.png)

    Le projet qui en résulte sera ajouté à votre solution&nbsp;:

    ![alt](images/desktop-to-uwp/debug-3.png)

    Dans le projet de création de packages, AppXFileList fournit un mappage des fichiers dans la disposition AppX. Les références démarrent vides, mais doivent être définies manuellement sur le projet .exe pour le classement des build. 

7. Le projet DesktopToUWPPackaging possède une page de propriété qui vous permet de configurer la racine du package AppX et la vignette à exécuter&nbsp;:

    ![alt](images/desktop-to-uwp/debug-4.png)

    Définissez PackageLayout sur l’emplacement racine de l’AppX qui a été créé par le convertisseur (ci-dessus). Ensuite, choisissez la vignette à exécuter.

8.  Ouvrez et modifiez AppXFileList.xml. Ce fichier définit la façon de copier la sortie de la build de débogage Win32 dans la disposition AppX conçue par le convertisseur. Par défaut, un espace réservé figure dans le fichier avec un exemple de balise et de commentaire&nbsp;:

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
            <MyProjectOutputPath>{relativepath}</MyProjectOutputPath>
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

    Le fichier est défini comme suit&nbsp;: 

    Tout d’abord, nous définissons *MyProjectOutputPath* de manière à désigner l’emplacement de création du projet Win32&nbsp;:

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="14.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        <PropertyGroup>
            <MyProjectOutputPath>..\ProjectTracker\bin\DesktopUWP</MyProjectOutputPath>
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

Si vous disposez de plusieurs configurations de build (par exemple, pour la sortie et le débogage), vous pouvez ajouter ce qui suit au fichier AppXFileList.xml pour copier la version Win32 depuis différents emplacements&nbsp;:

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

2.  Vous pouvez utiliser Configuration Manager pour ajouter la nouvelle configuration de build&nbsp;:

    ![alt](images/desktop-to-uwp/debug-6.png)

    ![alt](images/desktop-to-uwp/debug-7.png)

3.  Ensuite, sous les propriétés du projet, ajoutez la prise en charge des symboles de compilation conditionnelle&nbsp;:

    ![alt](images/desktop-to-uwp/debug-8.png)

4.  Vous pouvez maintenant basculer la cible de build vers DesktopUWP si vous souhaitez créer pour cibler l’API UWP que vous avez ajoutée.

## <a name="plmdebug"></a>PLMDebug 

Les options Visual Studio F5 et Attacher au processus sont utiles pour le débogage de votre application pendant qu’elle s’exécute. Dans certains cas, toutefois, vous pouvez souhaiter un contrôle plus fin sur le processus de débogage, notamment la possibilité de déboguer votre application avant son démarrage. Dans ces scénarios plus avancés, utilisez [**PLMDebug**](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx). Cet outil vous permet de déboguer votre application convertie à l’aide du débogueur Windows et propose un contrôle total sur le cycle de vie de l’application, ce qui inclut la suspension, la reprise et l’arrêt. 

PLMDebug.exe est inclus dans le SDK Windows. Pour plus d’informations, consultez [**PLMDebug**](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx). 

## <a name="run-another-process-inside-the-full-trust-container"></a>Exécuter un autre processus à l’intérieur du conteneur d’un niveau de confiance totale 

Vous pouvez appeler des processus personnalisés à l’intérieur du conteneur d’un package d’application spécifié. Cela peut être utile pour les scénarios de tests (par exemple, si vous avez un atelier de test personnalisé et que vous voulez tester la sortie de l’application). Pour ce faire, utilisez l’applet de commande PowerShell ```Invoke-CommandInDesktopPackage```&nbsp;: 

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```



<!--HONumber=Dec16_HO1-->


