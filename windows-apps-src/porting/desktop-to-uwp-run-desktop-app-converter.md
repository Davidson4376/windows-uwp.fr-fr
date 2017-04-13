---
author: normesta
Description: "Exécutez Desktop App Converter pour convertir une application de bureau Windows (Win32, WPF, Windows Forms) en une application de plateforme Windows universelle (UWP)."
Search.Product: eADQiWindows 10XVcnh
title: 'Pont du bureau vers UWP: Desktop App Converter'
ms.author: normesta
ms.date: 03/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 74c84eb6-4714-4e12-a658-09cb92b576e3
ms.openlocfilehash: 4daf45a4637ac846c550430cbc7518238ebd5bd8
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="desktop-to-uwp-bridge-desktop-app-converter"></a>Pont du bureau vers UWP: Desktop App Converter

[Obtenir Desktop App Converter](https://aka.ms/converter)

Desktop App Converter (DAC) est un outil qui vous permet de déployer vos applications de bureau existantes écrites pour .NET4.6.1 ou Win32 sur la plateforme Windows universelle (UWP). Vous pouvez exécuter vos programmes d’installation de bureau à l’aide du convertisseur en mode sans assistance et obtenir un package d’application Windows que vous pouvez installer en utilisant l’applet de commande PowerShell Add-AppxPackage sur votre ordinateur de développement.

Desktop App Converter est désormais disponible dans le [Windows Store](https://aka.ms/converter).

Le convertisseur exécute le programme d’installation de bureau dans un environnement Windows isolé à l’aide d’une nouvelle image de base, fournie dans le cadre du téléchargement du convertisseur. Il capture toutes les E/S du Registre et du système de fichiers effectuées par le programme d’installation du bureau et les intègre sous forme de package comme partie intégrante de la sortie. Le convertisseur génère un package d’application Windows avec une identité de package et la possibilité d’appeler une gamme étendue d’API WinRT.

## <a name="whats-new"></a>Nouveautés

La dernière version de DAC est la versionv1.0.9.0. Nouveautés dans cette mise à jour:

* Conversion sans programme d’installation: si votre application est installée via xcopy ou si vous connaissez les modifications que le programme d’installation de votre application apporte au système, vous pouvez exécuter la conversion sans programme d’installation, en définissant le paramètre -Installer sur le répertoire racine de vos fichiers d’application.
* Validation de package d’application: utilisez l’indicateur `-Verify` afin de vérifier que votre package d’application converti satisfait les exigences du Pont du bureau et du WindowsStore.


## <a name="system-requirements"></a>Configuration requise

### <a name="operating-system"></a>Système d’exploitation

+ Version d’évaluation de la mise à jour anniversaire de Windows10 (10.0.14393.0 et versions ultérieures) édition Professionnel ou Entreprise.

### <a name="hardware-configuration"></a>Configuration matérielle

+ Processeur 64bits (x64)
+ Assistance matérielle à la virtualisation
+ Traduction d’adresse de second niveau (SLAT, Second Level Address Translation)

### <a name="required-resources"></a>Ressources nécessaires

+ [Kit de développement logiciel Windows (Kit SDK Windows) pour Windows10](https://go.microsoft.com/fwlink/?linkid=821375)

## <a name="set-up-the-desktop-app-converter"></a>Configurer Desktop App Converter

*(Ces étapes ne sont pas requises lors d’une conversion sans programme d’installation).*

Desktop App Converter repose sur les dernières fonctionnalités de Windows10. Assurez-vous que vous utilisez la mise à jour anniversaire de Windows10 (14393.0 ou versions ultérieures).

1.    Téléchargez [DesktopAppConverter à partir du Windows Store](https://aka.ms/converter) et le [fichier .wim de l’image de base correspondant à votre build](https://aka.ms/converterimages).  
2.    Exécutez DesktopAppConverter en tant qu’administrateur. Vous pouvez le faire à partir du menu Démarrer en cliquant avec le bouton droit sur la vignette et en sélectionnant *Exécuter en tant qu’administrateur* sous *Plus*, ou à partir de la barre des tâches en cliquant avec le bouton droit sur la vignette, puis en cliquant à nouveau avec le bouton droit sur le nom de l’application qui s’affiche et en sélectionnant *Exécuter en tant qu’administrateur.*
3.    À partir de la fenêtre de la console de l’application, exécutez ```Set-ExecutionPolicy bypass```.
4.    Configurez le convertisseur en exécutant ```DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose``` à partir de la fenêtre de la console de l’application.
5.    Si l’exécution de la commande précédente vous invite à redémarrer, faites-le.

## <a name="run-the-desktop-app-converter"></a>Exécuter Desktop App Converter

+ **Téléchargement à partir du Windows Store**: utilisez ```DesktopAppConverter.exe``` pour exécuter le convertisseur.

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

### <a name="examples"></a>Exemples

Les exemples suivants représentent la conversion d’une application de bureau nommée *MyApp*, par *MyPublisher*, en package d’application Windows.

#### <a name="no-installer-conversion"></a>Conversion sans programme d’installation

Lors d’une conversion sans programme d’installation, le paramètre `-Installer` pointe vers le répertoire racine de vos fichiers d’application, et le paramètre `-AppExecutable` est requis.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyApp\ -AppExecutable MyApp.exe -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1 -MakeAppx -Sign -Verbose
```

#### <a name="installer-based-conversion"></a>Conversion avec programme d’installation

Lors d’une conversion avec programme d’installation, `-Installer` pointe vers le programme d’installation de votre application.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1 -MakeAppx -Sign -Verbose
```

## <a name="deploy-your-converted-windows-app-package"></a>Déployer votre package d’application Windows converti

Utilisez l’applet de commande [Add-AppxPackage](https://technet.microsoft.com/library/hh856048.aspx) dans PowerShell pour déployer un package d’application (.appx) signé sur un compte d’utilisateur.

Vous pouvez utiliser l’indicateur ```-Sign``` dans Desktop App Converter (v0.1.24) pour signer automatiquement votre application convertie. Vous pouvez également vous reporter à [Signer votre application de bureau convertie](desktop-to-uwp-signing.md) pour découvrir comment auto-signer des packages AppX.

Vous pouvez également utiliser le paramètre ```-Register``` de l’applet de commande PowerShell Add-AppXPackage pour effectuer l’installation à partir d’un dossier de fichiers non empaquetés au cours du processus de développement.

Pour plus d’informations sur le déploiement et le débogage de votre application convertie, voir [Déployer et déboguer votre application UWP convertie](desktop-to-uwp-deploy-and-debug.md).

## <a name="sign-your-windows-app-package"></a>Signer votre package d’application Windows

L’applet de commande Add-AppxPackage nécessite que le package d’application Windows (.appx) déployé soit signé. Utilisez l’indicateur ```-Sign``` dans le cadre de la ligne de commande du convertisseur ou SignTool.exe, fourni dans le SDK Microsoft Windows10, pour signer le packaged’application Windows.

Pour obtenir des détails supplémentaires sur la façon de signer votre package d’application Windows, voir [Signer votre application de bureau convertie](desktop-to-uwp-signing.md).

Remarque: si vous essayez de signer un package à l’aide du certificat généré automatiquement, vous devez utiliser le mot de passe par défaut «123456».

## <a name="modify-vfs-folder-and-registry-hive-optional"></a>Modifier le dossier VFS et la ruche du Registre (facultatif)

Desktop App Converter utilise une approche très classique pour le filtrage des fichiers et du bruit système dans le conteneur.  Après une conversion, vous pouvez effectuer les étapes facultatives suivantes:

1. Examiner le dossier VFS et supprimer les fichiers dont le programme d’installation n’a pas besoin.
2. Examiner le contenu de Reg.dat et supprimer les clés qui ne sont pas installées/utilisées par l’application.

Si vous apportez des modifications à votre application convertie (comme celles décrites ci-dessus), vous n’avez pas besoin de réexécuter ensuite le convertisseur. Vous pouvez recréer manuellement le package de votre application à l’aide de l’outil MakeAppx et du fichier appxmanifest.xml que Desktop App Converter génère pour l’application. Pour obtenir de l’aide, consultez [Convertir manuellement votre application en application UWP à l’aide de Desktop Bridge](desktop-to-uwp-manual-conversion.md).

## <a name="caveats"></a>Avertissements

1. La build de Windows10 sur la machine hôte doit correspondre à l’image de base que vous avez obtenue lors du téléchargement de Desktop App Converter.  
2. Assurez-vous que le programme d’installation de bureau est dans un répertoire indépendant, car le convertisseur copie tout le contenu du répertoire sur l’environnement Windows isolé.  
3. Actuellement, Desktop App Converter prend en charge l’exécution du processus de conversion sur système d’exploitation 64bits uniquement. Vous pouvez déployer les packages d’application Windows convertis sur système d’exploitation 64bits (x64) uniquement.  
4. Desktop App Converter exige que le programme d’installation de bureau soit exécuté en mode sans assistance. Assurez-vous de transmettre l’indicateur de mode silencieux de votre programme d’installation au convertisseur à l’aide du paramètre *-InstallerArguments*.
5. La publication d’assemblys Fusion côte à côte publics ne fonctionne pas. Pendant l’installation, une application peut publier des assemblys Fusion côte à côte publics, accessibles à tout autre processus. Lors de la création de contexte d’activation des processus, ces assemblys sont récupérés par un processus système nommé CSRSS.exe. Dans le cas d’un processus converti, la création de contexte d’activation et le chargement de modules de ces assemblys échouent. Les assemblys de boîte de réception, comme ComCtl, sont livrés avec le système d’exploitation. Ainsi, la prise d’une dépendance sur eux est sûre. Les assemblys Fusion côte à côte sont enregistrés dans les emplacements suivants:
  + Registre: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + Système de fichiers: %windir%\\SideBySide

## <a name="known-issues"></a>Problèmes connus

* Nous sommes actuellement en train d’examiner les erreurs suivantes qui se produisent avec certaines builds du système d’exploitation:

    * ```E_CREATTING_ISOLATED_ENV_FAILED```
    * ```E_STARTING_ISOLATED_ENV_FAILED```

    Si vous rencontrez l’une de ces erreurs, vérifiez que vous utilisez une image de base valide à partir du [Centre de téléchargement](https://aka.ms/converterimages). Si vous utilisez un fichier .wim valide, veuillez nous envoyer vos fichiers journaux à l’adresse converter@microsoft.com pour nous aider dans nos recherches.

* Si vous obtenez une version d’évaluation WindowsInsider sur un ordinateur de développement sur lequel était installé Desktop App Converter, l’erreur `New-ContainerNetwork: The object already exists` peut s’afficher lorsque vous configurez la nouvelle image de base. Pour contourner ce problème, exécutez la commande `Netsh int ipv4 reset` à partir d’une invite de commandes avec élévation des privilèges, puis redémarrez votre ordinateur.

* Une application .NET compilée avec l’option de build «AnyCPU» ne peut être installée si l’exécutable principal ou l’une des dépendances ont été placés sous «ProgramFiles» ou «Windows\System32». Pour contourner ce problème, utilisez un programme d’installation spécifique à votre architecture (32bits ou 64bits) pour générer correctement un package d’application Windows.

## <a name="telemetry-from-desktop-app-converter"></a>Télémétrie de Desktop App Converter

Le Convertisseur d’applications de bureau peut collecter des informations sur vous et votre utilisation du logiciel et les envoyer à Microsoft. Vous pouvez en savoir plus sur la collecte et l’utilisation de données de Microsoft dans la documentation des produits et dans la [Déclaration de confidentialité Microsoft](http://go.microsoft.com/fwlink/?LinkId=521839). Vous acceptez de respecter toutes les dispositions applicables de la Déclaration de confidentialité de Microsoft.

Par défaut, la télémétrie est activée pour le Convertisseur d’applications de bureau. Ajoutez la clé de Registre suivante pour configurer la télémétrie selon un paramètre de votre choix:  

```cmd
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ Ajoutez ou modifiez la valeur *DisableTelemetry* à l’aide d’une valeur DWORD définie sur1.
+ Pour activer la télémétrie, supprimez la clé ou définissez la valeur sur 0.

## <a name="desktop-app-converter-usage"></a>Utilisation du Convertisseur d’applications de bureau

Voici une liste de paramètres pour le Convertisseur d’applications de bureau. Vous pouvez également afficher cette liste en exécutant:   

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
|```-AppInstallPath <String> [optional]``` | Chemin d’accès complet au dossier racine de votre application pour les fichiers installés en cas d’installation (par exemple, «C:\Program Files (x86)\MyApp»).|
|```-Destination <String>``` | La destination souhaitée pour la sortie d’appx du convertisseur: DesktopAppConverter peut créer cet emplacement s’il n’existe pas déjà.|
|```-Installer <String>``` | Le chemin d’accès du programme d’installation de votre application doit être en mesure de s’exécuter sans assistance/silencieusement. Lors d’une conversion sans programme d’installation, il s’agit du chemin d’accès aux répertoires racine de vos fichiers d’application. |
|```-InstallerArguments <String>``` [facultatif] | Une liste séparée par des virgules ou une chaîne d’arguments pour forcer votre programme d’installation à s’exécuter sans assistance/silencieusement. Ce paramètre est facultatif si votre programme d’installation est un fichier msi. Pour obtenir un fichier journal à partir de votre programme d’installation, indiquez ici l’argument de la journalisation pour le programme d’installation, et utilisez le chemin d’accès ```<log_folder>```, qui est un jeton que le convertisseur remplace par le chemin d’accès approprié. <br><br>**REMARQUE: les indicateurs de mode sans assistance/silencieux et les arguments de journalisation varient selon les technologies d’installation.** <br><br>Exemple d’utilisation de ce paramètre : ```-InstallerArguments "/silent /log <log_folder>\install.log"``` Un autre exemple qui ne crée pas de fichier journal peut ressembler à ceci : ```-InstallerArguments "/quiet", "/norestart"``` là encore, vous devez littéralement diriger tous les journaux sur le chemin d’accès du jeton ```<log_folder>``` si vous voulez que le convertisseur les capture et les place dans un dossier des journaux finaux.|
|```-InstallerValidExitCodes <Int32>``` [facultatif] | Une liste séparée par des virgules des codes de sortie qui indiquent que votre programme d’installation a été exécuté correctement (par exemple: 0, 1234, 5678).  Par défaut, le code est 0 pour les éléments non msi et 0, 1641, 3010 pour les éléments msi.|

### <a name="windows-app-package-identity-parameters"></a>Paramètres d’identité du package d’application Windows  

|Paramètre|Description|
|---------|-----------|
|```-PackageName <String>``` | Le nom de votre package d’application Windows universelle
|```-Publisher <String>``` | L’éditeur de votre package d’application Windows universelle
|```-Version <Version>``` | Le numéro de version de votre package d’application Windows universelle

### <a name="optional-windows-app-package-manifest-parameters"></a>Paramètres facultatifs du manifeste du package d’application Windows  

|Paramètre|Description|
|---------|-----------|
|```-AppExecutable <String> [optional]``` [facultatif] | Nom de l’exécutable principal de votre application (par exemple, «MyApp.exe»). Ce paramètre est requis avec les conversions sans programme d’installation. |
|```-AppFileTypes <String>``` [facultatif] | Une liste séparée par des virgules des types de fichiers auxquels l’application sera associée (p. ex., «.txt, .doc», sans les guillemets).|
|```-AppId <String>``` [facultatif] | Spécifie une valeur sur laquelle définir l’ID d’application dans le manifeste du package d’application Windows. Si elle n’est pas spécifiée, elle sera définie à la valeur transmise pour *PackageName*.|
|```-AppDisplayName <String>``` [facultatif] | Spécifie une valeur sur laquelle définir le nom complet de l’application dans le manifeste du package d’application Windows. Si elle n’est pas spécifiée, elle sera définie à la valeur transmise pour *PackageName*. |
|```-AppDescription <String>``` [facultatif] | Spécifie une valeur sur laquelle définir la description d’application dans le manifeste du package d’application Windows. Si elle n’est pas spécifiée, elle sera définie à la valeur transmise pour *PackageName*.|
|```-PackageDisplayName <String>``` [facultatif] | Spécifie une valeur sur laquelle définir le nom complet du package dans le manifeste du package d’application Windows. Si elle n’est pas spécifiée, elle sera définie à la valeur transmise pour *PackageName*. |
|```-PackagePublisherDisplayName <String>``` [facultatif] | Spécifie une valeur sur laquelle définir le nom complet de l’éditeur du package dans le manifeste du package d’application Windows. Si elle n’est pas spécifiée, elle sera définie à la valeur transmise pour *Publisher*. |

### <a name="other-conversion-parameters"></a>Autres paramètres de conversion  

|Paramètre|Description|
|---------|-----------|
|```-ExpandedBaseImage <String>``` [facultatif] | Chemin d’accès complet vers une image de base déjà développée.|
|```-MakeAppx [<SwitchParameter>]``` [facultatif] | Un commutateur qui, lorsqu’il est présent, indique à ce script d’appeler MakeAppx sur la sortie. |
|```-LogFile <String>``` [facultatif] | Spécifie un fichier journal. S’il est omis, un emplacement temporaire du fichier journal est créé. |
| ```-Sign [<SwitchParameter>] [optional]``` | Indique à ce script de signer le package de sortie de l’application Windows. Ce commutateur doit être présent en même temps que le commutateur ```-MakeAppx```.
|```<Common parameters>``` | Cette applet de commande prend en charge les paramètres courants: *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable* et *OutVariable*. Pour plus d’informations, consultez [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216). |
| ```-Verify [<SwitchParameter>] [optional]``` | Un commutateur qui, lorsqu’il est présent, indique à l’outil DAC de vérifier que le package d’application converti satisfait les exigences du Pont du bureau et du Windows Store. Le résultat obtenu est un rapport de validation «VerifyReport.xml», qui s’affiche de manière plus efficace dans un navigateur. Ce commutateur doit être présent en même temps que le commutateur `-MakeAppx`.

### <a name="cleanup-parameters"></a>Paramètres de nettoyage

|Paramètre|Description|
|---------|-----------|
|```Cleanup [<Option>]``` | Exécute le nettoyage pour les artefacts DesktopAppConverter. Il existe trois options valides pour le mode de nettoyage. |
|```Cleanup All``` | Supprime toutes les images de base développées, supprime les fichiers de conversion temporaires, supprime le réseau de conteneurs et désactive la fonctionnalité Windows facultative, Conteneurs. |
|```Cleanup WorkDirectory``` | Supprime tous les fichiers de conversion temporaires. |
|```Cleanup ExpandedImage``` | Supprime toutes les images de base développées installées sur votre ordinateur hôte. |

### <a name="package-architecture"></a>Architecture de package

Desktop App Converter prend désormais en charge la création de packages d’applicationsx64 etx86 que vous pouvez installer et exécuter sur des ordinateurs amd64 etx86. Notez que Desktop App Converter doit toujours s’exécuter sur un ordinateur AMD64 pour effectuer une conversion réussie.

|Paramètre|Description|
|---------|-----------|
|```-PackageArch <String>``` | Génère un package selon l’architecture spécifiée. Les options valides sont «x86» ou «x64»; par exemple, -PackageArch x86. Ce paramètre est facultatif. Si ce paramètre n’est pas spécifié, le Convertisseur d’applications de bureau essaie de détecter automatiquement l’architecture du package. Si la détection automatique échoue, le convertisseur choisit par défaut l’architecturex64.

### <a name="running-the-peheadercertfixtool"></a>Exécution de PEHeaderCertFixTool

Au cours du processus de conversion, DesktopAppConverter exécute automatiquement l’outil PEHeaderCertFixTool pour corriger tous les en-têtesPE endommagés. Toutefois, vous pouvez également exécuter PEHeaderCertFixTool sur un package d’application Windows UWP, des fichiers libres ou un fichier binaire spécifique. Exemple d’utilisation:

```CMD
PEHeaderCertFixTool.exe <binary file>|<.appx package>|<folder> [/c] [/v]
 /c   -- check for corrupted certificate but do not fix (optional)
 /v   -- verbose (optional)
example1: PEHeaderCertFixTool app.exe
example2: PEHeaderCertFixTool c:\package.appx /c
example3: PEHeaderCertFixTool c:\myapp /c /v
```

## <a name="language-support"></a>Prise en charge linguistique

Desktop App Converter ne prend pas en charge Unicode. Par conséquent, aucun caractère chinois ou caractères nonASCII ne peut être utilisés avec l’outil.

## <a name="see-also"></a>Voir aussi

+ [Porter les applications de bureau vers UWP à l’aide de Desktop App Converter](https://channel9.msdn.com/events/Build/2016/P504)
+ [Projet Centennial: Porter des applications de bureau existantes vers la plateforme Windows universelle](https://channel9.msdn.com/events/Build/2016/B829)  
