---
author: normesta
Description: Run the Desktop Converter App to package a Windows desktop application (like Win32, WPF, and Windows Forms).
Search.Product: eADQiWindows 10XVcnh
title: Créer un package d'application à l’aide de Desktop App Converter (Pont du bureau)
ms.author: normesta
ms.date: 08/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 74c84eb6-4714-4e12-a658-09cb92b576e3
ms.localizationpriority: medium
ms.openlocfilehash: 8748b68bf4efbcc79d0bba475db32f3a2d7cc933
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/05/2018
ms.locfileid: "3371541"
---
# <a name="package-an-app-using-the-desktop-app-converter-desktop-bridge"></a>Créer un package d'application à l’aide de Desktop App Converter (Pont du bureau)

[Obtenir Desktop App Converter](https://aka.ms/converter)

Vous pouvez utiliser l’outil Desktop App Converter (DAC) pour mettre vos applications de bureau sur la plateforme Windows universelle (UWP). Cela inclut les applications Win32 et celles que vous avez créées à l’aide de .NET 4.6.1.

![Icône DAC](images/desktop-to-uwp/dac.png)

Bien que le terme «Converter» apparaisse dans son nom, cet outil ne convertit pas réellement votre application. Votre application reste inchangée. Toutefois, cet outil génère un package d’application Windows avec une identité de package et la possibilité d’appeler une gamme étendue d’API WinRT.

Vous pouvez installer ce package sur votre ordinateur de développement à l’aide de l’applet de commande PowerShell Add-AppxPackage.

Le convertisseur exécute le programme d’installation de bureau dans un environnement Windows isolé à l’aide d’une nouvelle image de base, fournie dans le cadre du téléchargement du convertisseur. Il capture toutes les E/S du Registre et du système de fichiers effectuées par le programme d’installation du bureau et les intègre sous forme de package comme partie intégrante de la sortie.

>[!IMPORTANT]
>Le Pont du bureau a été introduit dans Windows10, version1607 et peut être utilisé uniquement dans les projets qui ciblent la Mise à jour anniversaire Windows10 (version10.0; build 14393) ou une version ultérieure dans Visual Studio.

> [!NOTE]
> Obtenez <a href="https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373?l=oZG0B1WhD_8406218965/">cette série</a> de courtes vidéos publiées par MicrosoftVirtual Academy. Ces vidéos vous expliquent certaines méthodes courantes d’utilisation de Desktop App Converter.

## <a name="the-dac-does-more-than-just-generate-a-package-for-you"></a>DAC fait davantage pour vous que simplement générer un package

Voici quelques actions supplémentaires, qu’il peut accomplir pour vous.

**Windows10 Creators Update**

:heavy_check_mark: Enregistrer automatiquement vos gestionnaires d’aperçus, de miniatures et de propriétés, les règles du pare-feu, les indicateurs d’URL.

:heavy_check_mark: Enregistrer automatiquement les mappages des types de fichiers qui permettent aux utilisateurs de grouper des fichiers à l’aide de la colonne **Type** de l’explorateur de fichiers.

:heavy_check_mark: Enregistrer vos serveurs COM publics.

**Mise à jour anniversaire Windows10 ou ultérieure**

:heavy_check_mark: Signer automatiquement votre package pour vous permettre de tester votre application.

:heavy_check_mark: Valider votre application en fonction des exigences du Pont du bureau et du MicrosoftStore.

Vous trouverez une liste complète des options dans la section [Paramètres](#command-reference) de ce guide.

Si vous êtes prêt à créer votre package, mettons-nous au travail.

## <a name="first-prepare-your-application"></a>Tout d'abord, préparez votre application

Passez ce guide en revue avant de commencer à créer un package pour votre application: [Préparer le package d’une application (Pont du bureau)](desktop-to-uwp-prepare.md).

## <a name="make-sure-that-your-system-can-run-the-converter"></a>Assurez-vous que le convertisseur peut fonctionner sur votre système

Assurez-vous que votre système répond aux exigences suivantes:

* Version d’évaluation de la mise à jour anniversaire de Windows10 (10.0.14393.0 et versions ultérieures) édition Professionnel ou Entreprise.
* Processeur 64bits (x64)
* Assistance matérielle à la virtualisation
* Traduction d’adresse de second niveau (SLAT, Second Level Address Translation)
* [Kit de développement logiciel Windows (Kit SDK Windows) pour Windows10](https://go.microsoft.com/fwlink/?linkid=821375).

## <a name="start-the-desktop-app-converter"></a>Démarrez Desktop App Converter

1. Téléchargez et installez [Desktop App Converter](https://aka.ms/converter).

2. Exécutez Desktop App Converter en tant qu’administrateur.

    ![exécutez DAC en tant qu’administrateur](images/desktop-to-uwp/run-converter.png)

   Une fenêtre de console apparaît. Cette fenêtre de console vous permettra d’exécuter des commandes.

## <a name="set-a-few-things-up-apps-with-installers-only"></a>Définir quelques éléments (applications avec programmes d’installation uniquement)

Vous pouvez passer à la section suivante si votre application ne contient pas de programme d’installation.

1. Identifiez le numéro de version de votre système d’exploitation.

   Pour ce faire, tapez **winver** dans la zone de boîte de dialogue **Exécuter**, puis choisissez le bouton **OK**.

   ![winver](images/desktop-to-uwp/winver.png)

   Vous trouverez la version de votre build de Windows dans la boîte de dialogue **À propos de Windows**.

   ![Version de Windows10](images/desktop-to-uwp/win-10-version.png)

2. Téléchargez l’[image de base Desktop app Converter](https://aka.ms/converterimages) appropriée.

   Assurez-vous que le numéro de version qui s’affiche dans le nom du fichier correspond à celui de votre version de Windows.

   >[!IMPORTANT]
   > Si vous utilisez le numéro de build**15063** et que la version mineure de ce build est supérieure ou égale à **.483** (par exemple: **15063.540**), veillez à télécharger le fichier **BaseImage-15063-UPDATE.wim**. Si la version mineure de ce build est inférieure à **.483**, téléchargez le fichier **BaseImage-15063.wim**. Si vous avez déjà installé une version incompatible de ce fichier de base, vous pouvez y remédier. Cela [billet de blog](https://blogs.msdn.microsoft.com/appconsult/2017/08/04/desktop-app-converter-fails-on-windows-10-15063-483-and-later-how-to-solve-it/) explique la procédure à suivre.

3. Placez le fichier téléchargé dans un emplacement de votre ordinateur où vous pourrez le retrouver ultérieurement.

4. Dans la fenêtre de console affichée lors du démarrage de Desktop App Converter, exécutez la commande suivante: ```Set-ExecutionPolicy bypass```.
5. Paramétrez le convertisseur à l’aide de la commande suivante: ```DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose```.
6. Redémarrez votre ordinateur si vous êtes invité à le faire.

   Les messages d’état s’affichent dans la fenêtre de la console au fur et à mesure que le convertisseur développe l’image de base. Si vous ne voyez pas les messages d’état, appuyez sur une touche. Cela peut provoquer l’actualisation du contenu de la fenêtre de console.

   ![messages d’état de la fenêtre de console](images/desktop-to-uwp/bas-image-setup.png)

   Lorsque l’image de base est totalement développée, passez à la section suivante.

## <a name="package-an-app"></a>Créer un package d’application

Pour créer votre package d'application, exécutez la commande ``DesktopAppConverter.exe`` dans la fenêtre de console qui s’est ouverte lorsque vous avez démarré Desktop App Converter.  

Vous allez spécifier le nom de package, l’éditeur et la version de l’application à l’aide des paramètres.

> [!NOTE]
> Si vous avez réservé votre nom d’application dans le Store, vous pouvez obtenir le nom et l’éditeur du package à l’aide du tableau de bord du centre de développement de Windows. Si vous prévoyez de charger votre application de manière indépendante sur d’autres systèmes, vous pouvez fournir vos propres noms tant que le nom d’éditeur que vous choisissez correspond au nom indiqué sur le certificat utilisé pour signer votre application.

### <a name="a-quick-look-at-command-parameters"></a>Aperçu des paramètres de commande

Voici les paramètres requis.

```CMD
DesktopAppConverter.exe
-Installer <String>
-Destination <String>
-PackageName <String>
-Publisher <String>
-Version <Version>
```
Vous pouvez en savoir plus sur chacun d’entre eux [ici](#command-reference).

### <a name="examples"></a>Exemples

Voici quelques méthodes courantes pour créer votre package d'application.

* [Créer un package pour une application munie d’un programme d’installation (.msi)](#installer-conversion)
* [Créer un package pour une application disposant d’un fichier d’installation exécutable](#setup-conversion)
* [Créer un package pour une application sans programme d’installation](#no-installer-conversion)
* [Créer un package d'application, signer et préparer l'application pour la soumission au Store](#optional-parameters)

<a id="installer-conversion" />

#### <a name="package-an-app-that-has-an-installer-msi-file"></a>Créer un package pour une application munie d’un programme d’installation (.msi)

Pointez vers le fichier d’installation à l’aide du paramètre ``Installer``.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.msi -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

> [!IMPORTANT]
> Voici deux éléments important à prendre en considération ici. Assurez-vous en premier lieu que votre programme d’installation se trouve dans un dossier indépendant et que seuls les fichiers associés à ce programme d’installation se trouvent dans le même dossier. Le convertisseur copie l’intégralité du contenu de ce dossier vers l’environnement Windows isolé. <br> Ensuite si le centre de développement attribue une identité commençant par un chiffre à votre package, veillez à également transmettre le paramètre <i>-AppId</i> et à utiliser uniquement le suffixe de la chaîne (après le point de séparation à en tant que valeur pour ce paramètre.  

**Vidéo**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-an-Application-That-Has-an-MSI-Installer-Kh1UU2WhD_7106218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

Si votre programme d'installation comprend des programmes d'installation pour les librairies ou infrastructures indépendantes, vous devrez vous organiser différemment. Consultez [Chaînage de plusieurs programmes d'installation avec le pont du bureau](https://blogs.msdn.microsoft.com/appconsult/2017/09/11/chaining-multiple-installers-with-the-desktop-app-converter/).

<a id="setup-conversion" />

#### <a name="package-an-app-that-has-a-setup-executable-file"></a>Créer un package pour une application disposant d’un fichier d’installation exécutable

Pointez vers l’exécutable d’installation à l’aide du paramètre ``Installer``.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```
>[!IMPORTANT]
>Si le centre de développement attribue une identité commençant par un chiffre à votre package, veillez à également transmettre le paramètre <i>-AppId</i> et à utiliser uniquement le suffixe de la chaîne (après le point de séparation à en tant que valeur pour ce paramètre.

Le paramètre ``InstallerArguments`` est facultatif. Toutefois, étant donné que Desktop App Converter a besoin de votre programme d’installation pour s’exécuter en mode sans assistance, vous devrez peut-être l’utiliser si votre application a besoin d’indicateurs spécifiques pour s’exécuter en mode silencieux. L’indicateur ``/S`` est un indicateur de mode silencieux très courant, mais celui que vous utilisez peut être différent selon les technologies utilisées pour créer le fichier d’installation.

**Vidéo**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-an-Application-That-Has-a-Setup-exe-Installer-amWit2WhD_5306218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

<a id="no-installer-conversion" />

#### <a name="package-an-app-that-doesnt-have-an-installer"></a>Créer un package pour une application sans programme d’installation

Dans cet exemple, utilisez le paramètre ``Installer`` pour pointer vers le dossier racine des fichiers de votre application.

Utilisez le paramètre `AppExecutable` pour pointer vers le fichier exécutable de votre application.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyApp\ -AppExecutable MyApp.exe -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

>[!IMPORTANT]
>Si le centre de développement attribue une identité commençant par un chiffre à votre package, veillez à également transmettre le paramètre <i>-AppId</i> et à utiliser uniquement le suffixe de la chaîne (après le point de séparation à en tant que valeur pour ce paramètre.

**Vidéo**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-a-No-Installer-Application-agAXF2WhD_3506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

<a id="optional-parameters" />

#### <a name="package-an-app-sign-the-app-and-run-validation-checks-on-the-package"></a>Créer un package d'application, signer l’application puis exécuter les vérifications de validation sur le package

Cet exemple est similaire au premier, à ceci près qu’il montre comment signer votre application pour le test local, puis valider votre application en fonction des exigences du Pont du bureau et du MicrosoftStore.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1 -MakeAppx -Sign -Verbose -Verify
```
>[!IMPORTANT]
>Si le centre de développement attribue une identité commençant par un chiffre à votre package, veillez à également transmettre le paramètre <i>-AppId</i> et à utiliser uniquement le suffixe de la chaîne (après le point de séparation à en tant que valeur pour ce paramètre.

Le paramètre ``Sign`` génère un certificat dont il se sert ensuite pour signer votre application. Vous devrez installer ce certificat généré pour exécuter votre application. Pour savoir comment procéder, consultez la section [Exécuter l’application empaquetée](#run-app) de ce guide.

Vous pouvez valider application à l’aide du paramètre ``Verify``.

### <a name="a-quick-look-at-optional-parameters"></a>Aperçu des paramètres facultatifs

Les paramètres ``Sign`` et ``Verify`` sont facultatifs. Il existe bien d’autres paramètres facultatifs.  En voici quelques-uns parmi les plus couramment utilisés.

```CMD
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
Vous trouverez davantage de renseignements sur chacun d’eux dans la section suivante.

<a id="command-reference" />

### <a name="parameter-reference"></a>Références des paramètres

Voici la liste complète des paramètres (organisés par catégorie) que vous pouvez utiliser lorsque vous exécutez Desktop App Converter.

* [Installation](#setup-params)
* [Conversion](#conversion-params)
* [Identité du package](#identity-params)
* [Manifeste du package](#manifest-params)
* [Nettoyage](#cleanup-params)
* [Architecture](#architecture-params)
* [Divers](#other-params)

Vous pouvez également afficher la liste entière en exécutant la commande ``Get-Help`` dans la fenêtre de la console de l’application.   

||||
|-------------|-----------|-------------|
|<a id="setup-params" /> <strong>Paramètres d’installation</strong>  ||
|-Setup [&lt;SwitchParameter&gt;] |Requis |Exécute DesktopAppConverter en mode installation. Le mode d’installation prend en charge le développement d’une image de base fournie.|
|-BaseImage &lt;chaîne&gt; | Requis |Chemin d’accès complet vers une image de base non développée. Ce paramètre est obligatoire si -Setup est spécifié.|
| -LogFile &lt;chaîne&gt; |Facultatif |Spécifie un fichier journal. S’il est omis, un emplacement temporaire du fichier journal est créé.|
|-NatSubnetPrefix &lt;chaîne&gt; |Facultatif |La valeur de préfixe à utiliser pour l’instance NAT. En règle générale, vous souhaiterez changer cela uniquement si votre ordinateur hôte est attaché à la même plage de sous-réseau que le NetNat du convertisseur. Vous pouvez interroger la configuration actuelle du convertisseur NetNat à l’aide de l’applet de commande **Get-NetNat**. |
|-NoRestart [&lt;SwitchParameter&gt;] |Requis |N’exécutez pas de commande de redémarrage lors de l’exécution du programme d’installation (le redémarrage est nécessaire pour activer la fonctionnalité de conteneur). |
|<a id="conversion-params" /> <strong>Paramètres de conversion</strong>|||
|-AppInstallPath &lt;chaîne&gt;  |Facultatif |Chemin d’accès complet au dossier racine de votre application pour les fichiers installés en cas d’installation (par exemple, «C:\Program Files (x86)\MyApp»).|
|-Destination &lt;chaîne&gt; |Requis |La destination souhaitée pour la sortie d’appx du convertisseur: DesktopAppConverter peut créer cet emplacement s’il n’existe pas déjà.|
|-Installer &lt;chaîne&gt; |Requis |Le chemin d’accès du programme d’installation de votre application doit être en mesure de s’exécuter sans assistance/silencieusement. Lors d’une conversion sans programme d’installation, il s’agit du chemin d’accès aux répertoires racine de vos fichiers d’application. |
|-InstallerArguments &lt;chaîne&gt; |Facultatif |Une liste séparée par des virgules ou une chaîne d’arguments pour forcer votre programme d’installation à s’exécuter sans assistance/silencieusement. Ce paramètre est facultatif si votre programme d’installation est un fichier msi. Pour obtenir un fichier journal à partir de votre programme d’installation, indiquez ici l’argument de la journalisation pour le programme d’installation, et utilisez le chemin d’accès &lt;log_folder&gt;, qui est un jeton que le convertisseur remplace par le chemin d’accès approprié. <br><br>**REMARQUE**: les indicateurs de mode sans assistance/silencieux et les arguments de journalisation varient selon les technologies d’installation. <br><br>Exemple d’utilisation de ce paramètre: -InstallerArguments "/silent /log &lt;log_folder&gt;\install.log". Un autre exemple qui ne crée pas de fichier journal peut ressembler à ceci: ```-InstallerArguments "/quiet", "/norestart"``` là encore, vous devez littéralement diriger tous les journaux sur le chemin d’accès du jeton &lt;log_folder&gt; si vous voulez que le convertisseur les capture et les place dans un dossier des journaux finaux.|
|-InstallerValidExitCodes &lt;Int32&gt; |Facultatif |Une liste séparée par des virgules des codes de sortie qui indiquent que votre programme d’installation a été exécuté correctement (par exemple: 0, 1234, 5678).  Par défaut, le code est 0 pour les éléments non msi et 0, 1641, 3010 pour les éléments msi.|
|-MakeAppx [&lt;SwitchParameter&gt;]  |Facultatif |Un commutateur qui, lorsqu’il est présent, indique à ce script d’appeler MakeAppx sur la sortie. |
|-MakeMSIX [&lt;SwitchParameter&gt;]  |Facultatif |Un commutateur qui, lorsqu’il est présent, indique à ce script pour empaqueter la sortie sous la forme d’un Package MSIX. |
|<a id="identity-params" /><strong>Paramètres d’identité de package</strong>||
|-PackageName &lt;chaîne&gt; |Requis |Le nom de votre package d’application Windows universelle. Si le centre de développement attribue une identité commençant par un chiffre à votre package, veillez à également transmettre le paramètre <i>-AppId</i> et à utiliser uniquement le suffixe de la chaîne (après le point de séparation à en tant que valeur pour ce paramètre. |
|-Éditeur &lt;chaîne&gt; |Requis |L’éditeur de votre package d’application Windows universelle |
|-Version &lt;Version&gt; |Requis |Le numéro de version de votre package d’application Windows universelle |
|<a id="manifest-params" /><strong>Paramètres de manifeste de package</strong>||
|-AppExecutable &lt;chaîne&gt; |Facultatif |Nom de l’exécutable principal de votre application (par exemple, «MonApp.exe»). Ce paramètre est requis avec les conversions sans programme d’installation. |
|-AppFileTypes &lt;chaîne&gt;|Facultatif |Une liste séparée par des virgules des types de fichiers auxquels l’application sera associée Exemple d’utilisation: -AppFileTypes "'.md', '.markdown'".|
|-AppId &lt;chaîne&gt; |Facultatif |Spécifie une valeur à partir de laquelle est définie l’ID d’application dans le manifeste du package d’application Windows. Si elle n’est pas spécifiée, elle sera définie en fonction de la valeur transmise pour *PackageName*. Dans de nombreux cas, l'utilisation de *PackageName* convient. Toutefois, si le centre de développement attribue une identité commençant par un chiffre à votre package, veillez à également transmettre le paramètre <i>-AppId</i> et à utiliser uniquement le suffixe de la chaîne (après le point de séparation à en tant que valeur pour ce paramètre. |
|-AppDisplayName &lt;chaîne&gt;  |Facultatif |Spécifie une valeur à partir de laquelle est défini le nom complet de l’application dans le manifeste du package d’application Windows. Si elle n’est pas spécifiée, elle sera définie en fonction de la valeur transmise pour *PackageName*. |
|-AppDescription &lt;chaîne&gt; |Facultatif |Spécifie une valeur à partir de laquelle est définie la description d’application dans le manifeste du package d’application Windows. Si elle n’est pas spécifiée, elle sera définie en fonction de la valeur transmise pour *PackageName*.|
|-PackageDisplayName &lt;chaîne&gt; |Facultatif |Spécifie une valeur à partir de laquelle est défini le nom complet du package dans le manifeste du package d’application Windows. Si elle n’est pas spécifiée, elle sera définie en fonction de la valeur transmise pour *PackageName*. |
|-PackagePublisherDisplayName &lt;chaîne&gt; |Facultatif |Spécifie une valeur à partir de laquelle est défini le nom complet de l’éditeur du package dans le manifeste du package d’application Windows. Si elle n’est pas spécifiée, elle sera définie en fonction de la valeur transmise pour *Publisher*. |
|<a id="cleanup-params" /><strong>Paramètres de nettoyage</strong>|||
|-Cleanup [&lt;Option&gt;] |Requis |Exécute le nettoyage pour les artefacts DesktopAppConverter. Trois options sont valides pour le mode de nettoyage. |
|-Cleanup All (nettoyer tout) | |Supprime toutes les images de base développées, supprime les fichiers de conversion temporaires, supprime le réseau de conteneurs et désactive la fonctionnalité Windows facultative, conteneurs. |
|-Cleanup WorkDirectory (nettoyer le répertoire de travail) |Requis |Supprime tous les fichiers de conversion temporaires. |
|-Cleanup ExpandedImage (nettoyage images développées) |Requis |Supprime toutes les images développées de base installées sur votre ordinateur hôte. |
|<a id="architecture-params" /><strong>Paramètres d’architecture de package</strong>|||
|-PackageArch &lt;chaîne&gt; |Requis |Génère un package selon l’architecture spécifiée. Les options valides sont «x86» ou «x64»; par exemple, -PackageArch x86. Ce paramètre est facultatif. Si ce paramètre n’est pas spécifié, le Convertisseur d’applications de bureau essaie de détecter automatiquement l’architecture du package. Si la détection automatique échoue, le convertisseur choisit par défaut l’architecturex64. |
|<a id="other-params" /><strong>Paramètres divers</strong>|||
|-ExpandedBaseImage &lt;chaîne&gt;  |Facultatif |Chemin d’accès complet vers une image de base déjà développée.|
|-LogFile &lt;chaîne&gt;  |Facultatif |Spécifie un fichier journal. S’il est omis, un emplacement temporaire du fichier journal est créé. |
| -Sign [&lt;SwitchParameter&gt;] |Facultatif |Indique à ce script qu’il doit signer le package d’application Windows produit à l’aide d’un certificat généré à des fins de test. Ce commutateur doit être présent en même temps que le commutateur ```-MakeAppx```. |
|&lt;Paramètres communs&gt; |Requis |Cette applet de commande prend en charge les paramètres courants: *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable* et *OutVariable*. Pour plus d’informations, consultez [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216). |
| -Verify [&lt;SwitchParameter&gt;] |Facultatif |Un commutateur qui, lorsqu’il est présent, indique à l’outil DAC de vérifier que le package d’application satisfait les exigences du Pont du bureau et du MicrosoftStore. Le résultat obtenu est un rapport de validation «VerifyReport.xml», qui s’affiche de manière plus efficace dans un navigateur. Ce commutateur doit être présent en même temps que le commutateur `-MakeAppx`. |
|-PublishComRegistrations| Facultatif| Analyse toutes les inscriptions COM publiques effectuées par votre programme d’installation et publie celles qui sont valides dans votre manifeste. Utilisez cet indicateur seulement si vous souhaitez rendre ces enregistrements accessibles à d’autres applications. Il n’est pas nécessaire d’employer cet indicateur si ces enregistrements ne doivent être utilisés que par votre application. <br><br>Consultez [cet article](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#lDg5gSFxJ2TDlpC6.97) pour vous assurer que vos inscriptions COM fonctionnent comme prévu après la création du package de votre application.

<a id="run-app" />

## <a name="run-the-packaged-app"></a>Exécuter l’application empaquetée

Il existe deux façons d’exécuter votre application.

L’une d’elles consiste à ouvrir une invite de commandes PowerShell pour saisir la commande suivante: ```Add-AppxPackage –Register AppxManifest.xml```. Il s’agit probablement du moyen le plus simple d’exécuter votre application, car vous n’êtes pas obligé de la signer.

L’autre méthode consiste à signer votre application avec un certificat. Si vous utilisez le paramètre ```sign```, Desktop App Converter en génère un pour vous et signe votre application avec ce certificat. Ce fichier est nommé **auto-generated.cer**, et vous pouvez le trouver dans le dossier racine de votre application empaquetée.

Suivez ces étapes pour installer le certificat généré, puis exécutez votre application.

1. Double-cliquez sur le fichier **auto-generated.cer** pour installer le certificat.

   ![fichier de certificat généré](images/desktop-to-uwp/generated-cert-file.png)

   > [!NOTE]
   > Si vous êtes invité à saisir un mot de passe, utilisez le mot de passe par défaut «123456».

2. Dans la boîte de dialogue **Certificat**, choisissez le bouton **Installer un certificat**.
3. Dans l’**Assistant Importation du certificat**, installez ce dernier sur la **Machine locale** et placez-le dans le magasin de certificats **Personnes autorisées**.

   ![Magasin Personnes autorisées](images/desktop-to-uwp/trusted-people-store.png)

5. Dans le dossier racine de votre application empaquetée, double-cliquez sur le fichier de package d’application Windows.

   ![Fichier de package d’application Windows](images/desktop-to-uwp/windows-app-package.png)

6. Installez l’application en sélectionnant le bouton **Installer**.

   ![Bouton Installer](images/desktop-to-uwp/install.png)


## <a name="modify-the-packaged-app"></a>Modifier l’application empaquetée

Vous allez probablement apporter des modifications à votre application empaquetée pour corriger des bogues, ajouter les ressources visuelles ou améliorer votre application avec des expériences modernes telles que les vignettes dynamiques.

Une fois vos modifications accomplies, il n’est pas nécessaire d’effectuer une nouvelle conversion. Dans la plupart des cas, vous pouvez simplement repackager votre application à l’aide de l’outil MakeAppx et du fichier appxmanifest.xml généré par DAC pour votre application. Voir [Générer un package d’application Windows](desktop-to-uwp-manual-conversion.md#make-appx).

* Si vous modifiez l'un des actifs visuels de votre application, générez un nouveau fichier d’Index de ressource de Package, puis exécutez l’outil MakeAppx pour générer un nouveau package. Voir [Générer un fichier d’Index de ressource de package (IRP)](desktop-to-uwp-manual-conversion.md#make-pri).

* Si vous souhaitez ajouter des icônes ou des vignettes qui apparaissent sur la barre des tâches Windows, l'affichage des tâches, LT+TAB, l'alignement automatique et le coin inférieur droit des vignettes de démarrage, consultez [(Facultatif) Ajouter des ressources sans plaque basées sur la cible](desktop-to-uwp-manual-conversion.md#target-based-assets).

> [!NOTE]
> Si vous modifiez les paramètres de registre inscrits par votre programme d’installation, vous devrez exécuter une nouvelle fois Desktop App Converter afin de récupérer ces modifications.

**Vidéos**

|Modifier et générer un nouveau package |Démo: modifier et générer un nouveau package|
|---|---|
|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Video-Modifying-and-Repackaging-Output-from-Desktop-App-Converter-OwpAJ3WhD_6706218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Modify-Output-from-Desktop-App-Converter-gEnsa3WhD_8606218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|

Les deux sections suivantes décrivent quelques corrections facultatives que vous pouvez envisager pour l’application empaquetée.

### <a name="delete-unnecessary-files-and-registry-keys"></a>Supprimez les fichiers et les clés de registre inutiles

Desktop App Converter utilise une approche très classique pour le filtrage des fichiers et du bruit système dans le conteneur.

Si vous le souhaitez, vous pouvez inspecter le dossier VFS et supprimer les fichiers dont votre programme d’installation n’a pas besoin.  Vous pouvez également examiner le contenu de Reg.dat et supprimer les clés qui ne sont pas installées/utilisées par l’application.

### <a name="fix-corrupted-pe-headers"></a>Réparer les en-têtes PE endommagés

Au cours du processus de conversion, DesktopAppConverter exécute automatiquement l’outil PEHeaderCertFixTool pour corriger tous les en-têtes PE endommagés. Toutefois, vous pouvez également exécuter PEHeaderCertFixTool sur un package d’application Windows UWP, des fichiers libres ou un fichier binaire spécifique. En voici un exemple.

```CMD
PEHeaderCertFixTool.exe <binary file>|<.appx package>|<folder> [/c] [/v]
 /c   -- check for corrupted certificate but do not fix (optional)
 /v   -- verbose (optional)
example1: PEHeaderCertFixTool app.exe
example2: PEHeaderCertFixTool c:\package.appx /c
example3: PEHeaderCertFixTool c:\myapp /c /v
```

## <a name="telemetry-from-desktop-app-converter"></a>Télémétrie de Desktop App Converter

Le Convertisseur d’applications de bureau peut collecter des informations sur vous et votre utilisation du logiciel et les envoyer à Microsoft. Vous pouvez en savoir plus sur la collecte et l’utilisation de données de Microsoft dans la documentation des produits et dans la [Déclaration de confidentialité Microsoft](http://go.microsoft.com/fwlink/?LinkId=521839). Vous acceptez de respecter toutes les dispositions applicables de la Déclaration de confidentialité de Microsoft.

Par défaut, la télémétrie est activée pour le Convertisseur d’applications de bureau. Ajoutez la clé de Registre suivante pour configurer la télémétrie selon un paramètre de votre choix:  

```cmd
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ Ajoutez ou modifiez la valeur *DisableTelemetry* à l’aide d’une valeur DWORD définie sur1.
+ Pour activer la télémétrie, supprimez la clé ou définissez la valeur sur 0.

### <a name="language-support"></a>Prise en charge linguistique

Desktop App Converter ne prend pas en charge Unicode. Par conséquent, aucun caractère chinois ou caractère nonASCII ne peut être utilisé avec l’outil.

## <a name="next-steps"></a>Étapes suivantes

**Trouvez des réponses à vos questions**

Des questions? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

Vous pouvez également vous référer à [cette](desktop-to-uwp-known-issues.md#app-converter) liste des problèmes connus.

**Transmettre des commentaires ou suggérer des fonctionnalités**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Exécutez votre application/recherchez et réparez les problèmes**

Voir [Exécuter, déboguer et tester une application de bureau empaquetée (Pont du bureau)](desktop-to-uwp-debug.md)

**Distribuer votre application**

Consultez [Distribuer une application de bureau empaquetée (Pont du bureau)](desktop-to-uwp-distribute.md)
