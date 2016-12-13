---
author: awkoren
Description: "Exécutez Desktop App Converter pour convertir une application de bureau Windows (Win32, WPF, Windows Forms) en une application de plateforme Windows universelle (UWP)."
Search.Product: eADQiWindows 10XVcnh
title: Desktop App Converter
translationtype: Human Translation
ms.sourcegitcommit: a5ac8acdbb7480bb776cef6d1dffa303dab5a9e1
ms.openlocfilehash: 4095cfbe96f239afb5f3173e0a7f84e01d63452c

---

# <a name="desktop-app-converter"></a>Desktop App Converter

[Obtenir Desktop App Converter](https://aka.ms/converter)

Desktop App Converter (DAC) est un outil qui vous permet de déployer vos applications de bureau existantes écrites pour .NET&nbsp;4.6.1 ou Win32 sur la plateforme Windows universelle (UWP). Vous pouvez exécuter vos programmes d’installation de bureau à l’aide du convertisseur en mode sans assistance et obtenir un package AppX que vous pouvez installer en utilisant l’applet de commande PowerShell Add-AppxPackage sur votre ordinateur de développement.

Desktop App Converter est désormais disponible dans le [Windows Store](https://aka.ms/converter).

Le convertisseur exécute le programme d’installation de bureau dans un environnement Windows isolé à l’aide d’une nouvelle image de base, fournie dans le cadre du téléchargement du convertisseur. Il capture toutes les E/S du Registre et du système de fichier effectuées par le programme d’installation du bureau et les met sous forme de package comme partie intégrante de la sortie. Le convertisseur génère un AppX avec une identité de package et la possibilité d’appeler une gamme étendue d’API WinRT.

## <a name="whats-new"></a>Nouveautés

Cette section présente les modifications entre les versions de Desktop App Converter. 

### <a name="1122016-v101"></a>2/11/2016 (v1.0.1)

* Amélioration de la validation du schéma du manifeste. 
* Amélioration des messages d’erreur. 
* Ajout de la validation des versions de Windows prises en charge. 
* Résolution de bogues pour les tests de filtre du registre.

### <a name="9142016-v10"></a>14/9/2016 (v1.0)

* Desktop App Converter est désormais disponible en téléchargement dans le [Windows Store](https://aka.ms/converter). 
* Obtenez les dernières images de base Windows&nbsp;10 (.wim) dans le [Centre de téléchargement](https://aka.ms/converterimages) pour les utiliser avec DAC.
* Avec l’application du Windows&nbsp;Store, vous pouvez maintenant utiliser le nouveau point d’entrée *DesktopAppConverter.exe <arguments>* pour exécuter le convertisseur à partir de n’importe quel endroit dans une invite de commandes avec élévation de privilèges ou une fenêtre PowerShell.  


### <a name="922016-v0125"></a>2/9/2016 (v0.1.25)

* Intégration du dernier package NuGet dotnet-computervirtualization.
* Ajout de dépendances sur common.dll récemment introduites.
* Plusieurs résolutions de bogues.

### <a name="842016-v0124"></a>4/8/2016 (v0.1.24)

* Ajout de la prise en charge de la signature automatique des applications converties produites par DAC à des fins de test. Consultez l’indicateur ```–Sign``` pour essayer vous-même. 
* Ajout d’avertissements si l’une des inscriptions COM dans la ruche du Registre virtuel n’est pas prise en charge dans l’AppX empaqueté.  
* Ajout de la prise en charge de la détection automatique de dépendances d’application sur les bibliothèques&nbsp;VC++ puis de leur conversion en dépendances de manifeste AppX. Notez que pour charger de manière indépendante des applications et les tester à l’aide du runtime&nbsp;VC++, vous devez télécharger les packages d’infrastructure VCLib comme indiqué dans le billet de blog [Using Visual&nbsp;C++ Runtime in a Centennial project](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project) (en anglais). Recherchez les packages sous le dossier ```Program Files (x86)\Microsoft SDKs\Windows Kits\10\ExtensionSDKs\Microsoft.VCLibs.Desktop``` sur votre ordinateur, accédez à la version dont vous dépendez (par exemple, 11.0, 12,0, 14,0), puis double-cliquez sur le package d’architecture approprié (x64, x86) pour l’installer.
* Mise à jour du schéma de manifeste pour qu’il corresponde à la mise à jour anniversaire Windows&nbsp;10 (10.0.14393.0). 
* Plusieurs résolutions de bogues et amélioration de la disposition de la sortie. 

### <a name="772016-v0122"></a>07/07/2016 (v0.1.22)

* Ajout de la prise en charge de la détection automatique des extensions d’environnement de votre application de bureau et de leur déclaration automatique dans le fichier AppXManifest de votre package UWP. Pour en savoir plus sur les extensions de bureau, voir [**Extensions d’applications de bureau converties**](desktop-to-uwp-extensions.md). 
* Amélioration de la détection de la propriété AppExecutable pour un vaste ensemble d’applications. 

### <a name="6162016-v0120"></a>16/06/2016 (v0.1.20)

* Résolution de problèmes provoquant l’échec des conversions sur les dernières builds de Windows&nbsp;10&nbsp;Insider&nbsp;Preview. 
* Remplacement de ```–CreateX86Package``` par ```–PackageArch```, ce qui vous permet de préciser l’architecture du package généré. 

### <a name="682016"></a>6/8/2016

* Ajout d’une prise en charge pour la génération de packages appx x86 sur des machines hôtes AMD64 exécutant le convertisseur.
* Utilisation réduite de l’espace disque par la suppression d’images de base développées précédemment.
* Ajout d’une prise en charge supplémentaire pour le nettoyage de fichiers temporaires et d’images de base inutiles.
* Prise en charge étoffée pour la détection d’associations de types de fichier et de protocoles.
* Amélioration d’une logique permettant de détecter la propriété AppExecutable pour un vaste ensemble d’applications.
* Ajout d’une prise en charge pour la fourniture d’arguments –InstallerArguments supplémentaires pour les programmes d’installation basés sur MSI.
* Résolutions de bogues pour l’ensemble des erreurs PathTooLongException survenant au cours du processus de conversion.

### <a name="5122016"></a>5/12/2016

- Restauration de la prise en charge de l’édition professionnelle de Windows. 
- L’indicateur ```-Setup``` du Convertisseur active désormais la fonctionnalité Conteneurs Windows et gère l’extension de l’image de base. Pour effectuer une installation unique, à partir d’une invite de commandes avec élévation de privilèges PowerShell, exécutez les éléments suivants&nbsp;: ```PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage BaseImage-12345.wim -Verbose```
- Ajout de la détection automatique du chemin d’installation de l’application et déplacement de la racine de l’application en dehors de VFS pour limiter les redirections inutiles éventuelles du système de fichiers lors de l’exécution.
- Ajout de la détection automatique de l’image de base étendue dans le cadre du processus de conversion.
- Ajout de la détection automatique pour les associations et protocoles de types de fichiers.
- Amélioration de la logique de détection du raccourci du Menu Démarrer.
- Amélioration du filtrage du système de fichiers pour conserver les fichiers MUI installés de l’application.
- Mise à jour de la version de bureau minimale prise en charge (10.0.14342.0) dans le manifeste.

## <a name="system-requirements"></a>Configuration système requise

### <a name="operating-system"></a>Système d’exploitation

+ Version d’évaluation de la mise à jour anniversaire de Windows&nbsp;10 (10.0.14393.0 et versions ultérieures) édition Professionnel ou Entreprise.

### <a name="hardware-configuration"></a>Configuration matérielle

+ Processeur 64&nbsp;bits (x64)
+ Assistance matérielle à la virtualisation
+ Traduction d’adresse de second niveau (SLAT, Second Level Address Translation)

### <a name="required-resources"></a>Ressources nécessaires

+ [Kit de développement logiciel Windows (Kit SDK Windows) pour Windows&nbsp;10](https://go.microsoft.com/fwlink/?linkid=821375)

## <a name="set-up-the-desktop-app-converter"></a>Configurer Desktop App Converter

Desktop App Converter repose sur les dernières fonctionnalités de Windows&nbsp;10. Assurez-vous que vous utilisez la mise à jour anniversaire de Windows&nbsp;10 (14393.0 ou versions ultérieures).

### <a name="store-download"></a>Téléchargement à partir du Windows&nbsp;Store

1.  Téléchargez [DesktopAppConverter à partir du Windows Store](https://aka.ms/converter) et le [fichier .wim de l’image de base correspondant à votre build](https://aka.ms/converterimages).  
2.  Exécutez DesktopAppConverter en tant qu’administrateur. Vous pouvez le faire à partir du menu Démarrer en cliquant avec le bouton droit sur la vignette et en sélectionnant *Exécuter en tant qu’administrateur* sous *Plus*, ou à partir de la barre des tâches en cliquant avec le bouton droit sur la vignette, puis en cliquant à nouveau avec le bouton droit sur le nom de l’application qui s’affiche et en sélectionnant *Exécuter en tant qu’administrateur.*
3.  À partir de la fenêtre de la console de l’application, exécutez ```CMD PS C:\> Set-ExecutionPolicy bypass```.
4.  Configurez le convertisseur en exécutant ```CMD PS C:\> DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose``` à partir de la fenêtre de la console de l’application.
5.  Si l’exécution de la commande précédente vous invite à redémarrer, faites-le.

### <a name="zip-file"></a>Fichier zip 

DAC reste disponible sous la forme d’un fichier zip dans le [centre de téléchargement](https://aka.ms/converterimages) pour faciliter les scénarios hors connexion. Toutefois, toutes les mises à jour futures seront publiées uniquement dans la version disponible sur Windows store.

1.  Téléchargez le fichier zip de DAC et le [fichier .wim de l’image de base correspondant à votre build](https://aka.ms/converterimages).  
2. Extrayez DesktopAppConverter.zip dans un dossier local.
3. À partir d’une fenêtre administrateur PowerShell, exécutez ```CMD PS C:\> Set-ExecutionPolicy bypass```.
4. Configurez le convertisseur en exécutant ```CMD PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose``` à partir de la fenêtre administrateur PowerShell.
5. Si l’exécution de la commande précédente vous invite à redémarrer, faites-le.

## <a name="run-the-desktop-app-converter"></a>Exécuter Desktop App Converter

+ **Téléchargement à partir du Windows Store**&nbsp;: utilisez ```DesktopAppConverter.exe``` pour exécuter le convertisseur.
+ **Fichier zip**&nbsp;: utilisez ```DesktopAppConverter.ps1``` pour exécuter le convertisseur. 

### <a name="usage"></a>Utilisation

```CMD
DesktopAppConverter.exe
-Installer <String> [-InstallerArguments <String>] [-InstallerValidExitCodes <Int32>]
-Destination <String>
-PackageName <String>
-Publisher <String>
-Version <Version>
[-ExpandedBaseImage <String>]
[-AppExecutable <String>]
[-AppFileTypes <String>]
[-AppId <String>]
[-AppDisplayName <String>]
[-AppDescription <String>]
[-PackageDisplayName <String>]
[-PackagePublisherDisplayName <String>]
[-MakeAppx]
[-LogFile <String>]
[<CommonParameters>]  
```

### <a name="example"></a>Exemple

L’exemple suivant montre comment convertir une application de bureau nommée *MyApp* par *&lt;publisher_name&gt;* en un package UWP (AppX).

```CMD
DesktopAppConverter.exe -Installer C:\Installer\MyApp.exe 
-InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" 
-Publisher "CN=<publisher_name>" -Version 0.0.0.1 -MakeAppx -Verbose
```

## <a name="deploy-your-converted-appx"></a>Déployer votre AppX converti

Utilisez l’applet de commande [Add-AppxPackage](https://technet.microsoft.com/library/hh856048.aspx) dans PowerShell pour déployer un package d’application (.appx) signé sur un compte d’utilisateur. 

Vous pouvez utiliser l’indicateur ```-Sign``` dans Desktop App Converter (v0.1.24) pour signer automatiquement votre application convertie. Vous pouvez également vous reporter à [Signer votre application de bureau convertie](desktop-to-uwp-signing.md) pour découvrir comment auto-signer des packages AppX.

Vous pouvez également utiliser le paramètre ```-Register``` de l’applet de commande PowerShell Add-AppXPackage pour effectuer l’installation à partir d’un dossier de fichiers non empaquetés au cours du processus de développement. 

Pour plus d’informations sur le déploiement et le débogage de votre application convertie, voir [Déployer et déboguer votre application UWP convertie](desktop-to-uwp-deploy-and-debug.md). 

## <a name="sign-your-appx-package"></a>Signer votre package&nbsp;.Appx

L’applet de commande Add-AppxPackage nécessite que le package d’application (.appx) déployé soit signé. Utilisez l’indicateur ```-Sign``` dans le cadre de la ligne de commande du convertisseur ou SignTool.exe, fourni dans le SDK Microsoft Windows&nbsp;10, pour signer le package&nbsp;.appx.

Pour obtenir des détails supplémentaires sur la façon de signer votre package&nbsp;.appx, voir [Signer votre application de bureau convertie](desktop-to-uwp-signing.md). 

## <a name="caveats"></a>Avertissements

1. La build de Windows&nbsp;10 sur l’ordinateur hôte doit correspondre à l’image de base que vous avez obtenue lors du téléchargement du Convertisseur d’applications de bureau.  
2. Assurez-vous que le programme d’installation de bureau est dans un répertoire indépendant, car le convertisseur copie tout le contenu du répertoire sur l’environnement Windows isolé.  
3. Actuellement, le Convertisseur d’applications de bureau prend en charge l’exécution du processus de conversion sur système d’exploitation 64&nbsp;bits uniquement. Vous pouvez déployer les packages .appx convertis sur système d’exploitation 64&nbsp;bits (x64) uniquement.  
4. Le Convertisseur d’applications de bureau exige que le programme d’installation de bureau soit exécuté en mode sans assistance. Assurez-vous de transmettre l’indicateur de mode silencieux de votre programme d’installation au convertisseur à l’aide du paramètre *-InstallerArguments*.
5. La publication d’assemblys Fusion côte à côte publics ne fonctionne pas. Pendant l’installation, une application peut publier des assemblys Fusion côte à côte publics, accessibles à tout autre processus. Lors de la création de contexte d’activation des processus, ces assemblys sont récupérés par un processus système nommé CSRSS.exe. Dans le cas d’un processus converti, la création de contexte d’activation et le chargement de modules de ces assemblys échouent. Les assemblys de boîte de réception, comme ComCtl, sont livrés avec le système d’exploitation. Ainsi, la prise d’une dépendance sur eux est sûre. Les assemblys Fusion côte à côte sont enregistrés dans les emplacements suivants&nbsp;:
  + Registre&nbsp;: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + Système de fichiers&nbsp;: %windir%\\SideBySide

## <a name="known-issues"></a>Problèmes connus

+ Si vous obtenez une version d’évaluation Windows&nbsp;Insider sur un ordinateur de développement sur lequel était installé Desktop App Converter, l’erreur `New-ContainerNetwork: The object already exists` peut s’afficher lorsque vous configurez la nouvelle image de base. Pour contourner ce problème, exécutez la commande `Netsh int ipv4 reset` à partir d’une invite de commandes avec élévation des privilèges, puis redémarrez votre ordinateur. 
+ Une application .NET compilée avec l’option de build «&nbsp;AnyCPU&nbsp;» ne peut être installée si l’exécutable principal ou l’une des dépendances ont été placés sous «&nbsp;Program&nbsp;Files&nbsp;» ou «&nbsp;Windows\System32&nbsp;». Pour contourner ce problème, utilisez un programme d’installation spécifique à votre architecture (32&nbsp;bits ou 64&nbsp;bits) pour générer correctement un package AppX.

## <a name="telemetry-from-desktop-app-converter"></a>Télémétrie d’un Convertisseur d’applications de bureau  
Le Convertisseur d’applications de bureau peut collecter des informations sur vous et votre utilisation du logiciel et les envoyer à Microsoft. Vous pouvez en savoir plus sur la collecte et l’utilisation de données de Microsoft dans la documentation des produits et dans la [Déclaration de confidentialité Microsoft](http://go.microsoft.com/fwlink/?LinkId=521839). Vous acceptez de respecter toutes les dispositions applicables de la Déclaration de confidentialité de Microsoft.

Par défaut, la télémétrie est activée pour le Convertisseur d’applications de bureau. Ajoutez la clé de Registre suivante pour configurer la télémétrie selon un paramètre de votre choix&nbsp;:  
```CMD
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ Ajoutez ou modifiez la valeur *DisableTelemetry* à l’aide d’une valeur DWORD définie sur&nbsp;1.
+ Pour activer la télémétrie, supprimez la clé ou définissez la valeur sur 0.

## <a name="desktop-app-converter-usage"></a>Utilisation du Convertisseur d’applications de bureau

Voici une liste de paramètres pour le Convertisseur d’applications de bureau. Vous pouvez également afficher cette liste en exécutant&nbsp;:   

```CMD
Get-Help DesktopAppConverter.exe -detailed
```

### <a name="setup-parameters"></a>Paramètres d’installation  

|Paramètre|Description|
|---------|-----------|
|```-Setup [<SwitchParameter>]``` | Exécute DesktopAppConverter en mode installation. Le mode d’installation prend en charge le développement d’une image de base fournie.|
|```-BaseImage <String>``` | Chemin d’accès complet vers une image de base non développée. Ce paramètre est obligatoire si -Setup est spécifié.|
|```-LogFile <String>``` [facultatif] | Spécifie un fichier journal. S’il est omis, un emplacement temporaire du fichier journal est créé.|
|```-NatSubnetPrefix <String>``` [facultatif] | La valeur de préfixe à utiliser pour l’instance NAT. En règle générale, vous souhaiterez changer cela uniquement si votre ordinateur hôte est attaché à la même plage de sous-réseau que le NetNat du convertisseur. Vous pouvez interroger la configuration actuelle du convertisseur NetNat à l’aide de l’applet de commande **Get-NetNat**. |
|```-NoRestart [<SwitchParameter>]``` | N’exécutez pas de commande de redémarrage lors de l’exécution du programme d’installation (le redémarrage est nécessaire pour activer la fonctionnalité de conteneur). |

### <a name="conversion-parameters"></a>Paramètres de conversion  

|Paramètre|Description|
|---------|-----------|
|```-AppInstallPath <String> [optional]``` | Chemin d’accès complet au dossier racine de votre application pour les fichiers installés en cas d’installation (par exemple, «&nbsp;C:\Program Files (x86)\MyApp&nbsp;»).| 
|```-Destination <String>``` | La destination souhaitée pour la sortie d’appx du convertisseur&nbsp;: DesktopAppConverter peut créer cet emplacement s’il n’existe pas déjà.|
|```-Installer <String>``` | Le chemin d’accès du programme d’installation de votre application doit être en mesure de s’exécuter sans assistance/silencieusement|
|```-InstallerArguments <String>``` [facultatif] | Une liste séparée par des virgules ou une chaîne d’arguments pour forcer votre programme d’installation à s’exécuter sans assistance/silencieusement. Ce paramètre est facultatif si votre programme d’installation est un fichier msi. Pour obtenir un fichier journal à partir de votre programme d’installation, indiquez ici l’argument de la journalisation pour le programme d’installation, et utilisez le chemin d’accès ```<log_folder>```, qui est un jeton que le convertisseur remplace par le chemin d’accès approprié. <br><br>**REMARQUE&nbsp;: les indicateurs de mode sans assistance/silencieux et les arguments de journalisation varient selon les technologies d’installation.** <br><br>Exemple d’utilisation de ce paramètre : ```-InstallerArguments "/silent /log <log_folder>\install.log"``` Un autre exemple qui ne crée pas de fichier journal peut ressembler à ceci : ```-InstallerArguments "/quiet", "/norestart"``` là encore, vous devez littéralement diriger tous les journaux sur le chemin d’accès du jeton ```<log_folder>``` si vous voulez que le convertisseur les capture et les place dans un dossier des journaux finaux.|
|```-InstallerValidExitCodes <Int32>``` [facultatif] | Une liste séparée par des virgules des codes de sortie qui indiquent que votre programme d’installation a été exécuté correctement (par exemple&nbsp;: 0, 1234, 5678).  Par défaut, le code est 0 pour les éléments non msi et 0, 1641, 3010 pour les éléments msi.|

### <a name="appx-identity-parameters"></a>Paramètres d’identité Appx  

|Paramètre|Description|
|---------|-----------|
|```-PackageName <String>``` | Le nom de votre package d’application Windows universelle
|```-Publisher <String>``` | L’éditeur de votre package d’application Windows universelle
|```-Version <Version>``` | Le numéro de version de votre package d’application Windows universelle

### <a name="optional-appx-manifest-parameters"></a>Paramètres facultatifs de manifeste Appx  

|Paramètre|Description|
|---------|-----------|
|```-AppExecutable <String> [optional]``` [facultatif] | Nom de l’exécutable principal de votre application (par exemple, «&nbsp;MyApp.exe&nbsp;»). |
|```-AppFileTypes <String>``` [facultatif] | Une liste séparée par des virgules des types de fichiers auxquels l’application sera associée (p. ex., «&nbsp;.txt, .doc&nbsp;», sans les guillemets).|
|```-AppId <String>``` [facultatif] | Spécifie une valeur sur laquelle définir l’ID d’application dans le manifeste appx. Si elle n’est pas spécifiée, elle sera définie à la valeur transmise pour *PackageName*.|
|```-AppDisplayName <String>``` [facultatif] | Spécifie une valeur sur laquelle définir le nom complet de l’application dans le manifeste appx. Si elle n’est pas spécifiée, elle sera définie à la valeur transmise pour *PackageName*. |
|```-AppDescription <String>``` [facultatif] | Spécifie une valeur sur laquelle définir la description de l’application dans le manifeste appx. Si elle n’est pas spécifiée, elle sera définie à la valeur transmise pour *PackageName*.|
|```-PackageDisplayName <String>``` [facultatif] | Spécifie une valeur sur laquelle définir le nom complet du package dans le manifeste appx. Si elle n’est pas spécifiée, elle sera définie à la valeur transmise pour *PackageName*. |
|```-PackagePublisherDisplayName <String>``` [facultatif] | Spécifie une valeur sur laquelle définir le nom complet de l’éditeur du package dans le manifeste appx. Si elle n’est pas spécifiée, elle sera définie à la valeur transmise pour *Publisher*. |

### <a name="other-conversion-parameters"></a>Autres paramètres de conversion  

|Paramètre|Description|
|---------|-----------|
|```-ExpandedBaseImage <String>``` [facultatif] | Chemin d’accès complet vers une image de base déjà développée.|
|```-MakeAppx [<SwitchParameter>]``` [facultatif] | Un commutateur qui, lorsqu’il est présent, indique à ce script d’appeler MakeAppx sur la sortie. |
|```-LogFile <String>``` [facultatif] | Spécifie un fichier journal. S’il est omis, un emplacement temporaire du fichier journal est créé. |
| ```Sign [<SwitchParameter>] [optional]``` | Indique à ce script de signer l’Appx de sortie. Ce commutateur doit être présent en même temps que le commutateur ```-MakeAppx```. 
|```<Common parameters>``` | Cette applet de commande prend en charge les paramètres courants&nbsp;: *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable* et *OutVariable*. Pour plus d’informations, consultez [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216). |

### <a name="cleanup-parameters"></a>Paramètres de nettoyage

|Paramètre|Description|
|---------|-----------|
|```Cleanup [<Option>]``` | Exécute le nettoyage pour les artefacts DesktopAppConverter. Il existe trois options valides pour le mode de nettoyage. |
|```Cleanup All``` | Supprime toutes les images de base développées, supprime les fichiers de conversion temporaires, supprime le réseau de conteneurs et désactive la fonctionnalité Windows facultative, Conteneurs. |
|```Cleanup WorkDirectory``` | Supprime tous les fichiers de conversion temporaires. |
|```Cleanup ExpandedImage``` | Supprime toutes les images de base développées installées sur votre ordinateur hôte. |

### <a name="package-architecture"></a>Architecture de package

Desktop App Converter prend désormais en charge la création de packages d’applications&nbsp;x64 et&nbsp;x86 que vous pouvez installer et exécuter sur des ordinateurs amd64 et&nbsp;x86. Notez que Desktop App Converter doit toujours s’exécuter sur un ordinateur AMD64 pour effectuer une conversion réussie.

|Paramètre|Description|
|---------|-----------|
|```-PackageArch <String>``` | Génère un package selon l’architecture spécifiée. Les options valides sont «&nbsp;x86&nbsp;» ou «&nbsp;x64&nbsp;»&nbsp;; par exemple, -PackageArch x86. Ce paramètre est facultatif. Si ce paramètre n’est pas spécifié, le Convertisseur d’applications de bureau essaie de détecter automatiquement l’architecture du package. Si la détection automatique échoue, le convertisseur choisit par défaut l’architecture&nbsp;x64. 

### <a name="running-the-peheadercertfixtool"></a>Exécution de PEHeaderCertFixTool

Au cours du processus de conversion, DesktopAppConverter exécute automatiquement l’outil PEHeaderCertFixTool pour corriger tous les en-têtes&nbsp;PE endommagés. Toutefois, vous pouvez également exécuter PEHeaderCertFixTool sur un appx UWP, des fichiers libres ou un fichier binaire spécifique. 

PEHeaderCertFixTool est fourni avec le DesktopAppConverter.zip. Exemple d’utilisation&nbsp;: 

```CMD
PEHeaderCertFixTool.exe <binary file>|<.appx package>|<folder> [/c] [/v]
 /c   -- check for corrupted certificate but do not fix (optional)
 /v   -- verbose (optional)
example1: PEHeaderCertFixTool app.exe
example2: PEHeaderCertFixTool c:\package.appx /c
example3: PEHeaderCertFixTool c:\myapp /c /v
```

## <a name="language-support"></a>Prise en charge des langues

Desktop App Converter ne prend pas en charge Unicode. Par conséquent, aucun caractère chinois ou caractères non&nbsp;ASCII ne peut être utilisés avec l’outil.

## <a name="see-also"></a>Voir aussi

+ [Porter les applications de bureau vers UWP à l’aide de Desktop App Converter](https://channel9.msdn.com/events/Build/2016/P504)
+ [Projet Centennial&nbsp;: Porter des applications de bureau existantes vers la plateforme Windows universelle](https://channel9.msdn.com/events/Build/2016/B829)  


<!--HONumber=Dec16_HO1-->


