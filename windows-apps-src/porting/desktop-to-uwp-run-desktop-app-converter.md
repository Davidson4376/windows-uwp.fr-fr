---
author: awkoren
Description: "Exécutez le Convertisseur d’applications de bureau pour convertir une application de bureau Windows (Win32, WPF, Windows Forms) en une application UWP."
Search.Product: eADQiWindows 10XVcnh
title: "Aperçu du Convertisseur d’applications de bureau (projet Centennial)"
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 8a22285467005722ad6ee5bf4f129a7dfdea944c

---

# Aperçu du Convertisseur d’applications de bureau (projet Centennial)

\[Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.\]

[Obtenez le Convertisseur d’applications de bureau.](http://go.microsoft.com/fwlink/?LinkId=785437)

Le Convertisseur d’applications de bureau est un outil de version préliminaire qui vous permet de déployer vos applications de bureau existantes écrites pour .NET4.6.1 ou Win32 sur la plateforme Windows universelle (UWP). Vous pouvez exécuter vos programmes d’installation de bureau via le convertisseur en mode sans assistance (silencieux) et obtenir un package AppX que vous pouvez installer en utilisant l’applet de commande PowerShell Add-AppxPackage sur votre ordinateur de développement.

Le convertisseur exécute le programme d’installation de bureau dans un environnement Windows isolé à l’aide d’une nouvelle image de base, fournie dans le cadre du téléchargement du convertisseur. Il capture toutes les E/S du Registre et du système de fichier effectuées par le programme d’installation du bureau et les met sous forme de package comme partie intégrante de la sortie. Le convertisseur génère un AppX avec une identité de package et la possibilité d’appeler une gamme étendue d’API WinRT.

## Nouveautés

Cette section présente les modifications entre les versions du Convertisseur d’applications de bureau. 

### 07/07/2016 (v0.1.22)

* Ajout de la prise en charge de la détection automatique des extensions d’environnement de votre application de bureau et de leur déclaration automatique dans le fichier AppXManifest de votre package UWP. Pour en savoir plus sur les extensions de bureau, voir [**Extensions d’applications de bureau converties**](desktop-to-uwp-extensions.md). 
* Amélioration de la détection de la propriété AppExecutable pour un vaste ensemble d’applications. 

### 16/06/2016 (v0.1.20)

* Résolution de problèmes provoquant l’échec des conversions sur les dernières builds de Windows10InsiderPreview. 
* Remplacement de ```–CreateX86Package``` par ```–PackageArch```, ce qui vous permet de préciser l’architecture du package généré. 

### 6/8/2016

* Ajout d’une prise en charge pour la génération de packages appx x86 sur des machines hôtes AMD64 exécutant le convertisseur.
* Utilisation réduite de l’espace disque par la suppression d’images de base développées précédemment.
* Ajout d’une prise en charge supplémentaire pour le nettoyage de fichiers temporaires et d’images de base inutiles.
* Prise en charge étoffée pour la détection d’associations de types de fichier et de protocoles.
* Amélioration d’une logique permettant de détecter la propriété AppExecutable pour un vaste ensemble d’applications.
* Ajout d’une prise en charge pour la fourniture d’arguments –InstallerArguments supplémentaires pour les programmes d’installation basés sur MSI.
* Résolutions de bogues pour l’ensemble des erreurs PathTooLongException survenant au cours du processus de conversion.

### 5/12/2016

- Restauration de la prise en charge de l’édition professionnelle de Windows. 
- L’indicateur ```-Setup``` du Convertisseur active désormais la fonctionnalité Conteneurs Windows et gère l’extension de l’image de base. Pour effectuer une installation unique, à partir d’une invite de commandes avec élévation de privilèges PowerShell, exécutez les éléments suivants: ```PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage BaseImage-12345.wim -Verbose```
- Ajout de la détection automatique du chemin d’installation de l’application et déplacement de la racine de l’application en dehors de VFS pour limiter les redirections inutiles éventuelles du système de fichiers lors de l’exécution.
- Ajout de la détection automatique de l’image de base étendue dans le cadre du processus de conversion.
- Ajout de la détection automatique pour les associations et protocoles de types de fichiers.
- Amélioration de la logique de détection du raccourci du Menu Démarrer.
- Amélioration du filtrage du système de fichiers pour conserver les fichiers MUI installés de l’application.
- Mise à jour de la version de bureau minimale prise en charge (10.0.14342.0) pour le projet Centennial dans le manifeste.

## Configuration système

### Système d’exploitation pris en charge
+ Version d’évaluation de la mise à jour anniversaire de Windows10 édition Entreprise (Build 10.0.14342.0 et versions ultérieures)

### Configuration matérielle requise

Votre ordinateur doit disposer des fonctionnalités minimales suivantes:
+ Processeur 64bits (x64)
+ Assistance matérielle à la virtualisation
+ Traduction d’adresse de second niveau (SLAT, Second Level Address Translation)

### Ressources recommandées
+ [Kit de développement logiciel (SDK) Windows pour Windows10](http://go.microsoft.com/fwlink/?LinkId=615097)

## Configurer le Convertisseur d’applications de bureau   
Le Convertisseur d’applications de bureau s’appuie sur les fonctionnalités de Windows 10 fournies comme versions d’évaluation des builds Windows Insider Preview. Assurez-vous que vous exécutez la dernière build pour utiliser le convertisseur.

1. Assurez-vous que vous disposez de la dernière version du système d’exploitation Windows 10 Insider Preview Édition Entreprise ou Pro (http://insider.windows.com). 
2. Téléchargez DesktopAppConverter.zip et le fichier .wim d’image de base qui correspond à votre build d’InsiderPreview (http://aka.ms/converter). 
3. Extrayez DesktopAppConverter.zip dans un dossier local.
4. À partir d’une fenêtre administrateur PowerShell:  
```CMD
PS C:\> Set-ExecutionPolicy bypass
```
5. Exécutez la commande suivante depuis une fenêtre administrateur PowerShell pour installer le convertisseur:
```CMD
PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose
```
6. Si l’exécution de la commande précédente vous invite à redémarrer votre ordinateur, faites-le, puis réexécutez la commande.

## Exécuter le Convertisseur d’applications de bureau
Le Convertisseur d’applications de bureau est doté de deux points d’entrée: PowerShell et Interface de commande. Vous pouvez utiliser l’un de ces points d’entrée pour démarrer le processus de conversion.

### Utilisation
```CMD
DesktopAppConverter.ps1
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

### Exemple
L’exemple suivant montre comment convertir une application de bureau nommée *MyApp* par *&lt;publisher_name&gt;* en un package UWP (AppX).

+ À partir d’une fenêtre administrateur PowerShell, exécutez la commande suivante:
```CMD
PS C:\>.\DesktopAppConverter.ps1 -Installer C:\Installer\MyApp.exe 
-InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" 
-Publisher "CN=<publisher_name>" -Version 0.0.0.1 -MakeAppx -Verbose
```

## Déployer votre AppX converti
Utilisez l’applet de commande [Add-AppxPackage](https://technet.microsoft.com/library/hh856048.aspx) dans PowerShell pour déployer un package de l’application (.appx) signé sur un compte d’utilisateur. Pour signer votre package .appx, consultez la section «Signature de votre package .Appx». En outre, vous pouvez inclure le paramètre *Register* de l’applet de commande pour effectuer l’installation à partir d’un dossier de fichiers non empaquetés au cours du processus de développement. Pour plus d’informations, voir [Déployer et déboguer votre application UWP convertie](desktop-to-uwp-deploy-and-debug.md).

## Signer votre package .Appx

L’applet de commande Add-AppxPackage nécessite que le package d’application (.appx) déployé soit signé. Utilisez SignTool.exe, fourni dans le SDK Microsoft Windows 10 pour signer le package .appx.

### Exemple
```CMD
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
```
**Remarque** Lorsque vous exécutez MakeCert.exe et que vous êtes invité à entrer un mot de passe, sélectionnez **Aucun**.

Pour plus d’informations sur les certificats et la signature, consultez:

+ [Procédure: Créer des certificats temporaires à utiliser pendant le développement](https://msdn.microsoft.com/library/ms733813.aspx)
+ [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)
+ [SignTool.exe (Sign Tool)](https://msdn.microsoft.com/library/8s9b9yaz.aspx)

### Avertissements
1. La build de Windows 10 sur l’ordinateur hôte doit correspondre à l’image de base que vous avez obtenue lors du téléchargement du Convertisseur d’applications de bureau.  
2. Assurez-vous que le programme d’installation de bureau est dans un répertoire indépendant, car le convertisseur copie tout le contenu du répertoire sur l’environnement Windows isolé.  
3. Actuellement, le Convertisseur d’applications de bureau prend en charge l’exécution du processus de conversion sur système d’exploitation 64bits uniquement. Vous pouvez déployer les packages .appx convertis sur système d’exploitation 64bits (x64) uniquement.  
4. Le Convertisseur d’applications de bureau exige que le programme d’installation de bureau soit exécuté en mode sans assistance. Assurez-vous de transmettre l’indicateur de mode silencieux de votre programme d’installation au convertisseur à l’aide du paramètre *-InstallerArguments*.
5. La publication d’assemblys Fusion côte à côte publics ne fonctionne pas. Pendant l’installation, une application peut publier des assemblys Fusion côte à côte publics, accessibles à tout autre processus. Lors de la création de contexte d’activation des processus, ces assemblys sont récupérés par un processus système nommé CSRSS.exe. Dans le cas d’un processus Centennial, la création de contexte d’activation de ces assemblys et le chargement de modules échouent. Les assemblys de boîte de réception, comme ComCtl, sont livrés avec le système d’exploitation. Ainsi, la prise d’une dépendance sur eux à partir des processus Centennial est sûre. Les assemblys Fusion côte à côte sont enregistrés dans les emplacements suivants:
  + Registre: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + Système de fichiers: %windir%\\SideBySide

## Problèmes connus

+ Si vous obtenez une version d’évaluation WindowsInsider sur un ordinateur de développement sur lequel était installé l’aperçu du Convertisseur d’applications de bureau, l’erreur `New-ContainerNetwork: The object already exists` peut s’afficher lorsque vous configurez la nouvelle image de base. Pour contourner ce problème, exécutez la commande `Netsh int ipv4 reset` à partir d’une invite de commandes avec élévation des privilèges, puis redémarrez votre ordinateur. 
+ Une application .NET compilée avec l’option de build «AnyCPU» ne peut être installée si l’exécutable principal ou l’une des dépendances ont été placés sous «ProgramFiles» ou «Windows\System32». Pour contourner ce problème, utilisez un programme d’installation spécifique à votre architecture (32bits ou 64bits) pour générer correctement un package AppX.

## Télémétrie d’un Convertisseur d’applications de bureau  
Le Convertisseur d’applications de bureau peut collecter des informations sur vous et votre utilisation du logiciel et les envoyer à Microsoft. Vous pouvez en savoir plus sur la collecte et l’utilisation de données de Microsoft dans la documentation des produits et dans la [Déclaration de confidentialité Microsoft](http://go.microsoft.com/fwlink/?LinkId=521839). Vous acceptez de respecter toutes les dispositions applicables de la Déclaration de confidentialité de Microsoft.

Par défaut, la télémétrie est activée pour le Convertisseur d’applications de bureau. Ajoutez la clé de Registre suivante pour configurer la télémétrie selon un paramètre de votre choix:  
```CMD
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ Ajoutez ou modifiez la valeur *DisableTelemetry* à l’aide d’une valeur DWORD définie sur1.
+ Pour activer la télémétrie, supprimez la clé ou définissez la valeur sur 0.

## Utilisation du Convertisseur d’applications de bureau
Voici une liste de paramètres pour le Convertisseur d’applications de bureau. Vous pouvez également afficher cette liste dans la fenêtre Windows PowerShell en exécutant la commande suivante:  
```CMD
get-help .\DesktopAppConverter.ps1 -detailed
```

### Paramètres d’installation  
|Paramètre|Description|
|---------|-----------|
|```-Setup [<SwitchParameter>]``` | Exécute DesktopAppConverter en mode installation. Le mode d’installation prend en charge le développement d’une image de base fournie.|
|```-BaseImage <String>``` | Chemin d’accès complet vers une image de base non développée. Ce paramètre est obligatoire si -Setup est spécifié.|
|```-LogFile <String>``` [facultatif] | Spécifie un fichier journal. S’il est omis, un emplacement temporaire du fichier journal est créé.|
|```-NatSubnetPrefix <String>``` [facultatif] | La valeur de préfixe à utiliser pour l’instance NAT. En règle générale, vous souhaiterez changer cela uniquement si votre ordinateur hôte est attaché à la même plage de sous-réseau que le NetNat du convertisseur. Vous pouvez interroger la configuration actuelle du convertisseur NetNat à l’aide de l’applet de commande **Get-NetNat**. |
|```-NoRestart [<SwitchParameter>]``` | N’exécutez pas de commande de redémarrage lors de l’exécution du programme d’installation (le redémarrage est nécessaire pour activer la fonctionnalité de conteneur). |

### Paramètres de conversion  
|Paramètre|Description|
|---------|-----------|
|```-Installer <String>``` | Le chemin d’accès du programme d’installation de votre application doit être en mesure de s’exécuter sans assistance/silencieusement|
|```-InstallerArguments <String>``` [facultatif] | Une liste séparée par des virgules ou une chaîne d’arguments pour forcer votre programme d’installation à s’exécuter sans assistance/silencieusement. Ce paramètre est facultatif si votre programme d’installation est un fichier msi. Pour obtenir un fichier journal à partir de votre programme d’installation, indiquez ici l’argument de la journalisation pour le programme d’installation, et utilisez le chemin d’accès ```<log_folder>```, qui est un jeton que le convertisseur remplace par le chemin d’accès approprié. <br><br>**REMARQUE: les indicateurs de mode sans assistance/silencieux et les arguments de journalisation varient selon les technologies d’installation.** <br><br>Exemple d’utilisation de ce paramètre : ```-InstallerArguments "/silent /log <log_folder>\install.log"``` Un autre exemple qui ne crée pas de fichier journal peut ressembler à ceci : ```-InstallerArguments "/quiet", "/norestart"``` là encore, vous devez littéralement diriger tous les journaux sur le chemin d’accès du jeton ```<log_folder>``` si vous voulez que le convertisseur les capture et les place dans un dossier des journaux finaux.|
|```-InstallerValidExitCodes <Int32>``` [facultatif] | Une liste séparée par des virgules des codes de sortie qui indiquent que votre programme d’installation a été exécuté correctement (par exemple: 0, 1234, 5678).  Par défaut, le code est 0 pour les éléments non msi et 0, 1641, 3010 pour les éléments msi.|
|```-Destination <String>``` | La destination souhaitée pour la sortie d’appx du convertisseur: DesktopAppConverter peut créer cet emplacement s’il n’existe pas déjà.|

### Paramètres d’identité Appx  
|Paramètre|Description|
|---------|-----------|
|```-PackageName <String>``` | Le nom de votre package d’application Windows universelle
|```-Publisher <String>``` | L’éditeur de votre package d’application Windows universelle
|```-Version <Version>``` | Le numéro de version de votre package d’application Windows universelle

### Paramètres facultatifs de manifeste Appx  
|Paramètre|Description|
|---------|-----------|
|```-AppExecutable <String>``` [facultatif] | Le chemin d’accès complet vers le principal fichier exécutable de votre application s’il devait être installé (ce n’est pas le cas), par exemple, «C:\Program Files (x86)\MyApp\MyApp.exe».|
|```-AppFileTypes <String>``` [facultatif] | Une liste séparée par des virgules des types de fichiers auxquels l’application sera associée (p. ex., «.txt, .doc», sans les guillemets).|
|```-AppId <String>``` [facultatif] | Spécifie une valeur sur laquelle définir l’ID d’application dans le manifeste appx. Si elle n’est pas spécifiée, elle sera définie à la valeur transmise pour *PackageName*.|
|```-AppDisplayName <String>``` [facultatif] | Spécifie une valeur sur laquelle définir le nom complet de l’application dans le manifeste appx. Si elle n’est pas spécifiée, elle sera définie à la valeur transmise pour *PackageName*. |
|```-AppDescription <String>``` [facultatif] | Spécifie une valeur sur laquelle définir la description de l’application dans le manifeste appx. Si elle n’est pas spécifiée, elle sera définie à la valeur transmise pour *PackageName*.|
|```-PackageDisplayName <String>``` [facultatif] | Spécifie une valeur sur laquelle définir le nom complet du package dans le manifeste appx. Si elle n’est pas spécifiée, elle sera définie à la valeur transmise pour *PackageName*. |
|```-PackagePublisherDisplayName <String>``` [facultatif] | Spécifie une valeur sur laquelle définir le nom complet de l’éditeur du package dans le manifeste appx. Si elle n’est pas spécifiée, elle sera définie à la valeur transmise pour *Publisher*. |

### Autres paramètres de conversion  
|Paramètre|Description|
|---------|-----------|
|```-ExpandedBaseImage <String>``` [facultatif] | Chemin d’accès complet vers une image de base déjà développée.|
|```-MakeAppx [<SwitchParameter>]``` [facultatif] | Un commutateur qui, lorsqu’il est présent, indique à ce script d’appeler MakeAppx sur la sortie. |
|```-LogFile <String>``` [facultatif] | Spécifie un fichier journal. S’il est omis, un emplacement temporaire du fichier journal est créé. |
|```<Common parameters>``` | Cette applet de commande prend en charge les paramètres courants: *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable* et *OutVariable*. Pour plus d’informations, consultez [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216). |

### Paramètres de nettoyage
|Paramètre|Description|
|---------|-----------|
|```Cleanup [<Option>]``` | Exécute le nettoyage pour les artefacts DesktopAppConverter. Il existe trois options valides pour le mode de nettoyage. |
|```Cleanup All``` | Supprime toutes les images de base développées, supprime les fichiers de conversion temporaires, supprime le réseau de conteneurs et désactive la fonctionnalité Windows facultative, Conteneurs. |
|```Cleanup WorkDirectory``` | Supprime tous les fichiers de conversion temporaires. |
|```Cleanup ExpandedImages``` | Supprime toutes les images de base développées installées sur votre ordinateur hôte. |

### Architecture de package
L’aperçu du Convertisseur d’applications de bureau prend désormais en charge la création de packages d’applications x64 et x86 que vous pouvez installer et exécuter sur des ordinateurs amd64 et x86. Notez que le Convertisseur d’applications de bureau doit toujours s’exécuter sur un ordinateur AMD64 pour effectuer une conversion réussie.

|Paramètre|Description|
|---------|-----------|
|```-PackageArch <String>``` | Génère un package selon l’architecture spécifiée. Les options valides sont «x86» ou «x64»; par exemple, -PackageArch x86. Ce paramètre est facultatif. Si ce paramètre n’est pas spécifié, le Convertisseur d’applications de bureau essaie de détecter automatiquement l’architecture du package. Si la détection automatique échoue, le Convertisseur choisit par défaut l’architecture x64. |

## Voir également
+ [Obtenir le Convertisseur d’applications de bureau](http://go.microsoft.com/fwlink/?LinkId=785437)
+ [Porter votre application de bureau vers la plateforme Windows universelle](https://developer.microsoft.com/windows/bridges/desktop)
+ [Porter les applications de bureau vers UWP à l’aide du Convertisseur d’applications de bureau](https://channel9.msdn.com/events/Build/2016/P504)
+ [Projet Centennial: Porter des applications de bureau existantes vers la plateforme Windows universelle](https://channel9.msdn.com/events/Build/2016/B829)  
+ [UserVoice pour Desktop Bridge (projet Centennial)](http://aka.ms/UserVoiceDesktopToUwp)
+ [Pont d’application de bureau pour les exemples de code UWP dans GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)



<!--HONumber=Jul16_HO2-->


