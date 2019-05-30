---
title: Composants Windows Runtime du service Broker pour les applications installées hors UWP
description: Ce document décrit une fonctionnalité enterprise-ciblées pris en charge par Windows 10, ce qui permet aux applications .NET tactile à utiliser le code existant responsable pour les principales opérations critiques.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 81b3930c-6af9-406d-9d1e-8ee6a13ec38a
ms.localizationpriority: medium
ms.openlocfilehash: a9b6c6fc7a7e3ddfab70fe289a41bb4d436e9722
ms.sourcegitcommit: ea15237291ae3ade0bf22e38bd292c3a23947a03
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66377313"
---
# <a name="brokered-windows-runtime-components-for-a-side-loaded-uwp-app"></a>Composants Windows Runtime du service Broker pour les applications installées hors UWP

Cet article décrit une fonctionnalité enterprise-ciblées pris en charge par Windows 10, ce qui permet aux applications .NET tactile à utiliser le code existant responsable pour les principales opérations critiques.

## <a name="introduction"></a>Introduction

>**Remarque**  l’exemple de code qui accompagne cet article peut être téléchargée pour [Visual Studio 2015 et 2017](https://aka.ms/brokeredsample). Le modèle Microsoft Visual Studio pour générer répartie de composants Windows Runtime peut être téléchargé ici : [Modèle de Visual Studio 2015 ciblant les applications Windows universelles pour Windows 10](https://marketplace.visualstudio.com/vsgallery/10be07b3-67ef-4e02-9243-01b78cd27935)

Windows inclut une nouvelle fonctionnalité appelée *répartie de composants Windows Runtime pour les applications chargées*. Nous utilisons le terme IPC (Inter-Process Communication) pour décrire la capacité à exécuter des composants logiciels de bureau existants dans un processus (composant de bureau) lors de l’interaction avec ce code dans une application UWP. Il s’agit d’un modèle bien connu des développeurs d’entreprise car les applications de base de données et les applications qui utilisent les services NT dans Windows partagent une architecture à plusieurs processus similaire.

L’installation hors Windows Store de l’application est un composant essentiel de cette fonctionnalité.
Les applications spécifiques à l’entreprise n’ont pas leur place dans le Microsoft Store et les sociétés ont des exigences spécifiques en matière de sécurité, confidentialité, distribution, installation et maintenance. C’est pourquoi, le modèle d’installation hors Windows Store est un élément requis pour utiliser cette fonctionnalité et également un détail d’implémentation critique.

Les applications centrées sur les données sont une cible clé pour cette architecture d’application. Il est envisagé que les règles d’entreprise existantes utilisées, par exemple, dans SQL Server, fassent partie du composant de bureau. Il ne s’agit pas du seul type de fonctionnalité qui peut être proposé par le composant de bureau, mais cette fonctionnalité est souvent demandée en liaison avec les données existantes et la logique d’entreprise.

Enfin, étant donné la pénétration écrasante du runtime .NET et le C\# language dans le développement d’entreprise, cette fonctionnalité a été développé en mettant l’accent sur l’utilisation de .NET pour l’application UWP et les côtés du composant de bureau. Tandis que les autres langages et les runtimes sont possibles pour l’application UWP, l’exemple qui accompagne cet article illustre uniquement C\#et est réservé exclusivement à l’exécution de .NET.

## <a name="application-components"></a>Composants d’application

>**Remarque**  cette fonctionnalité est exclusivement pour l’utilisation de .NET. L’application cliente et le composant de bureau doivent être créés à l’aide de .NET.

**Modèle d’application**

Cette fonctionnalité est conçue autour de l’architecture d’application générale connue sous le nom de MVVM (Model View View-Model). C’est pourquoi le « modèle » est considéré comme résidant entièrement dans le composant de bureau. Le composant de bureau sera « headless » (sans périphérique de contrôle), c’est-à-dire qu’il ne contiendra pas d’interface utilisateur. L’affichage sera entièrement contenu dans l’application d’entreprise installée hors Windows Store. Bien que la construction « view-model » ne soit pas une exigence de conception de l’application, il est probable que l’utilisation de ce modèle soit la plus courante.

**Composant de bureau**

Le composant de bureau de cette fonctionnalité est un nouveau type d’application proposé dans le cadre de cette fonctionnalité. Ce composant bureau peut uniquement être écrite en C\# et doit cibler .NET 4.6 ou version ultérieure pour Windows 10. Le type de projet est une solution hybride entre le CLR ciblant UWP, dans la mesure où le format de communication entre processus comprend des types et des classes UWP, alors que le composant de bureau peut appeler toutes les parties de la bibliothèque de classes du runtime .NET. L’impact sur le projet Visual Studio sera décrit en détail ultérieurement. Cette configuration hybride permet le marshaling des types UWP dans l’application conçue sur les composants de bureau tout en permettant l’appel de code CLR de bureau au sein de l’implémentation de composant de bureau.

**contrat**

Le contrat entre l’application installée hors Windows Store et le composant du bureau est décrit en fonction du système de type UWP. Cela implique la déclaration d’un ou plusieurs C\# classes qui peuvent représenter une UWP. Consultez la rubrique MSDN [création de composants Windows Runtime en C\# et Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br230301(v=vs.140)) pour une spécification précise de la création de classe d’exécution de Windows à l’aide de C\#.

>**Remarque**  Enums ne sont pas pris en charge dans le contrat de composants Windows Runtime entre le composant de bureau et de chargement indépendant d’application pour l’instant.

**Application de chargement indépendant**

L’application installée hors Store est une application UWP normale à une exception près : elle est installée à partir d’un emplacement autre que le Microsoft Store. La plupart des mécanismes d’installation sont identiques : le manifeste et le package de l’application sont identiques (un ajout au manifeste est décrit plus en détail ultérieurement). Lorsque l’installation hors Windows Store est activée, un script PowerShell simple peut installer les certificats nécessaires et l’application elle-même. La meilleure pratique standard stipule que l’application installée hors Windows Store doit réussir le test de certification WACK inclus dans le menu Projet/Store de Visual Studio.

>**Remarque** chargement indépendant peut être activé dans les paramètres -&gt; mise à jour et sécurité -&gt; pour les développeurs.

Il est à noter que le mécanisme App Broker n’est fourni dans le cadre de la Mise à jour Windows 10 qu’en version 32 bits. Le composant de bureau doit être 32 bits.
Les applications installées hors Windows Store peuvent être 64 bits (si un proxy 64 bits et un proxy 32 bits sont inscrits), mais cela n’est pas la norme. Génération de l’application de chargement indépendant dans C\# à l’aide de la configuration de « neutre » normale et la valeur par défaut « préférer 32 bits » naturellement crée des applications chargées de 32 bits.

**Instanciation du serveur et les domaines d’application**

Toute application installée hors Windows Store reçoit sa propre instance d’un serveur App Broker (« instanciation multiple »). Le code serveur s’exécute dans un unique AppDomain. Cela permet l’exécution de plusieurs versions de bibliothèques dans des instances séparées. Par exemple, l’application A a besoin de V1.1 d’un composant et l’application B a besoin de V2. Ces deux versions sont clairement séparées, les composants V1.1 et V2 se trouvent dans des répertoires serveur distincts et l’application pointe vers le serveur qui prend en charge la version correcte appropriée.

L’implémentation du code serveur peut être partagée entre plusieurs instances de serveur App Broker en faisant pointer plusieurs applications vers le même répertoire serveur. Plusieurs instances du serveur App Broker existent, mais elles exécutent le même code. Tous les composants d’implémentation utilisés dans une application doivent se trouver dans le même chemin.

## <a name="defining-the-contract"></a>Définition du contrat

La première étape pour créer une application en utilisant cette fonctionnalité consiste à créer le contrat entre l’application installée hors Windows Store et le composant de bureau. Pour cela, vous devez utiliser exclusivement des types Windows Runtime.
Heureusement, ceux-ci sont faciles à déclarer à l’aide de C\# classes. Cependant, quand vous définissez ces conversations, des facteurs de performances importants sont à prendre en compte. Ce point est abordé plus en détail plus loin dans cet article.

La séquence pour définir le contrat se présente comme suit :

**Étape 1 :** Créer une nouvelle bibliothèque de classes dans Visual Studio. Veillez à créer le projet à l’aide du modèle Bibliothèque de classes, et non à l’aide du modèle Composant Windows Runtime.

Une implémentation doit suivre, mais cette section ne traite que de la définition du contrat entre processus. L’exemple fourni comprend la classe suivante (EnterpriseServer.cs), qui ressemble à :

```csharp
namespace Fabrikam
{
    public sealed class EnterpriseServer
    {

        public ILis<String> TestMethod(String input)
        {
            throw new NotImplementedException();
        }
        
        public IAsyncOperation<int> FindElementAsync(int input)
        {
            throw new NotImplementedException();
        }
        
        public string[] RetrieveData()
        {
            throw new NotImplementedException();
        }
        
        public event EventHandler<string> PeriodicEvent;
    }
}
```

Ce code définit une classe « EnterpriseServer » pouvant être instanciée à partir de l’application installée hors Windows Store. Cette classe fournit la fonctionnalité promise par la classe Runtime. La classe Runtime peut servir à générer le fichier winmd de référence qui sera inclus dans l’application installée hors Windows Store.

**Étape 2 :** Modifiez le fichier projet manuellement pour modifier le type de sortie de projet pour le composant d’exécution de Windows

Pour ce faire, dans Visual Studio, cliquez avec le bouton droit de la souris sur le projet nouvellement créé et sélectionnez « Décharger le projet ». Cliquez de nouveau avec le bouton droit de la souris, puis sélectionnez « Modifier EnterpriseServer.csproj » pour ouvrir le fichier de projet, un fichier XML, pour l’éditer.

Dans le fichier ouvert, recherchez le \<OutputType\> balise et modifiez sa valeur en « winmdobj ».

**Étape 3 :** Créer une règle de génération qui crée une « référence » Windows le fichier de métadonnées (fichier .winmd). c'est-à-dire sans implémentation.

**Étape 4 :** Créer une règle de génération qui crée un fichier de métadonnées Windows « implémentation », par exemple, a les mêmes informations de métadonnées, mais inclut également l’implémentation.

Cette opération s’effectue via les scripts suivants. Ajoutez les scripts à la ligne de commande de l’événement post-build, dans **Propriétés** > **Événements de build** dans le projet.

> **Remarque** Le script est différent en fonction de la version de Windows que vous visez (Windows 10) et de la version de Visual Studio utilisée.

**Visual Studio 2015**
```cmd
    call "$(DevEnvDir)..\..\vc\vcvarsall.bat" x86 10.0.14393.0

    md "$(TargetDir)"\impl    md "$(TargetDir)"\reference

    erase "$(TargetDir)\impl\*.winmd"
    erase "$(TargetDir)\impl\*.pdb"
    rem erase "$(TargetDir)\reference\*.winmd"

    xcopy /y "$(TargetPath)" "$(TargetDir)impl"
    xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

    winmdidl /nosystemdeclares /metadata_dir:C:\Windows\System32\Winmetadata "$(TargetPath)"

    midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)BrokeredProxyStub\$(TargetName)_i.c" /env win32 /x86 /h   "$(SolutionDir)BrokeredProxyStub\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)BrokeredProxyStub\dlldata.c" /proxy "$(SolutionDir)BrokeredProxyStub\$(TargetName)_p.c"  "$(TargetName).idl"
    mdmerge -n 1 -i "$(ProjectDir)bin\$(ConfigurationName)" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial

    rem erase "$(TargetPath)"

```


**Visual Studio 2017**
```cmd
    call "$(DevEnvDir)..\..\vc\auxiliary\build\vcvarsall.bat" x86 10.0.16299.0

    md "$(TargetDir)"\impl
    md "$(TargetDir)"\reference

    erase "$(TargetDir)\impl\*.winmd"
    erase "$(TargetDir)\impl\*.pdb"
    rem erase "$(TargetDir)\reference\*.winmd"

    xcopy /y "$(TargetPath)" "$(TargetDir)impl"
    xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

    winmdidl /nosystemdeclares /metadata_dir:C:\Windows\System32\Winmetadata "$(TargetPath)"

    midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)BrokeredProxyStub\$(TargetName)_i.c" /env win32 /x86 /h "$(SolutionDir)BrokeredProxyStub\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)BrokeredProxyStub\dlldata.c" /proxy "$(SolutionDir)BrokeredProxyStub\$(TargetName)_p.c"  "$(TargetName).idl"
    mdmerge -n 1 -i "$(ProjectDir)bin\$(ConfigurationName)" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial

    rem erase "$(TargetPath)"
```

Une fois le fichier de référence **winmd** créé (dans le sous-dossier « reference » du dossier Target du projet), il est copié sur chaque projet d’application installée hors Windows Store qui le consomme et il est référencé. La section suivante décrira cette procédure. La structure du projet intégrée dans les règles de génération ci-dessus garantit que l’implémentation et la référence **winmd** se trouvent dans des répertoires séparés dans la hiérarchie de génération pour éviter toute confusion.

## <a name="side-loaded-applications-in-detail"></a>Détails sur les applications installées hors Windows Store
Comme indiqué précédemment, l’application installée hors Windows Store est créée comme n’importe quelle application UWP, à un détail près : la déclaration de la disponibilité de la ou des classes Runtime dans le manifeste de l’application installée hors Windows Store. Cela permet à l’application d’écrire un nouvel accès à la fonctionnalité dans le composant de bureau. Une nouvelle entrée de manifeste dans la section <Extension> décrit la classe Runtime implémentée dans le composant de bureau et les informations sur son emplacement. Le contenu de cette déclaration dans le manifeste de l’application est le même pour les applications qui ciblent Windows 10. Exemple :

```XML
<Extension Category="windows.activatableClass.inProcessServer">
    <InProcessServer>
        <Path>clrhost.dll</Path>
        <ActivatableClass ActivatableClassId="Fabrikam.EnterpriseServer" ThreadingModel="both">
            <ActivatableClassAttribute Name="DesktopApplicationPath" Type="string" Value="c:\test" />
        </ActivatableClass>
    </InProcessServer>
</Extension>
```

La catégorie est inProcessServer, car il existe plusieurs entrées dans la catégorie outOfProcessServer qui ne sont pas applicables à cette configuration d’application. Notez que le <Path> composant doit toujours contenir clrhost.dll (il s’agit toutefois **pas** appliquée et en spécifiant une valeur différente échoue de manière non définie).

La section <ActivatableClass> est identique à une classe Runtime véritablement in-process préférée par un composant Windows Runtime dans le package d’application. <ActivatableClassAttribute> est un nouvel élément et les attributs nom = « DesktopApplicationPath » et le Type = « string » est obligatoires et invariant. L’attribut Value pointe vers l’emplacement où réside le fichier winmd d’implémentation du composant de bureau (décrit en détail dans la section suivante). Chaque classe Runtime préférée par le composant de bureau doit avoir son arborescence d’éléments <ActivatableClass>. ActivatableClassId doit correspondre au nom complet d’espace de noms de la classe Runtime.

Comme indiqué dans la section « Définition du contrat », une référence de projet au winmd de référence du composant de bureau doit être créée. Le système de projet Visual Studio crée une structure de répertoires à deux niveaux portant le même nom. Dans l’exemple, il est EnterpriseIPCApplication\\EnterpriseIPCApplication. La référence **winmd** est copié manuellement à ce second répertoire de niveau, puis la boîte de dialogue est utilisée de références de projet (cliquez sur le **Parcourir...**  bouton) pour rechercher et référencer ce **winmd**. L’espace de noms de premier niveau du composant de bureau (Fabrikam) doit ensuite s’afficher sous la forme d’un nœud de premier niveau dans la partie Références du projet.

>**Remarque** il est très important d’utiliser le **winmd de référence** dans l’application de chargement indépendant. Si vous reportées accidentellement le **implémentation winmd** au répertoire de l’application de chargement indépendant et référence, vous recevrez probablement une erreur liée à « IStringable introuvable ». Il s’agit d’un bien sûr vous connecter qui incorrecte **winmd** a été référencé. Les règles de post-builds dans l’application serveur IPC (détaillée dans la section suivante) soigneusement séparer ces deux **winmd** dans des répertoires distincts.

Variables d’environnement (en particulier % ProgramFiles%) peut être utilisé dans <ActivatableClassAttribute Value="path"> . Comme indiqué précédemment, le répartiteur de l’application prend uniquement en charge 32 bits afin de % ProgramFiles% résoudra vers C:\\Program Files (x86) si l’application est exécutée sur un système d’exploitation 64 bits.

## <a name="desktop-ipc-server-detail"></a>Détails du serveur IPC de bureau

Les deux sections précédentes décrivent la déclaration de la classe et les mécanismes de transport de la référence **winmd** au projet d’application de chargement indépendant. Le travail restant dans le composant de bureau concerne l’implémentation. Comme nous voulons que le composant de bureau puisse appeler le code de bureau (en réutilisant des composants de code existants), le projet doit être configuré d’une certaine façon.
En règle générale, un projet Visual Studio en .NET utilise l’un des deux « profils » existants.
L’un est pour le bureau (« .NetFramework ») et l’autre cible la partie de l’application UWP du CLR (« .NetCore »). Un composant de bureau dans cette fonctionnalité est hybride. Il en résulte que la section Références est construite précisément pour fusionner ces deux profils.

Un projet d’application UWP standard ne contient pas de références de projet explicites, car l’intégralité de la surface d’API Windows Runtime est incluse de façon implicite.
En règle générale, seules des références entre projets sont créées. Cependant, un projet de composant de bureau contient un ensemble de références spécial. Il démarre la vie d’un « bureau classique\\bibliothèque de classes » de projet et n’est donc un projet de bureau. Des références explicites donc à l’API de Runtime Windows (via des références aux **winmd** fichiers) doivent être effectuées. Ajoutez les références adéquates tel qu’indiqué ci-dessous.

```XML
<ItemGroup>
    <!-- These reference are added by VS automatically when you create a Class Library project-->
    <Reference Include="System" />
    <Reference Include="System.Core" />
    <Reference Include="System.Xml.Linq" />
    <Reference Include="System.Data.DataSetExtensions" />
    <Reference Include="Microsoft.CSharp" />
    <Reference Include="System.Data" />
    <Reference Include="System.Net.Http" />
<Reference Include="System.Xml" />
    <!-- These reference should be added manually by editing .csproj file-->

    <Reference Include="System.Runtime.WindowsRuntime, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089, processorArchitecture=MSIL">
      <HintPath>$(MSBuildProgramFiles32)\Microsoft SDKs\NETCoreSDK\System.Runtime.WindowsRuntime\4.0.10\lib\netcore50\System.Runtime.WindowsRuntime.dll</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\UnionMetadata\Facade\Windows.WinMD</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.FoundationContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.FoundationContract\1.0.0.0\Windows.Foundation.FoundationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.UniversalApiContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.UniversalApiContract\1.0.0.0\Windows.Foundation.UniversalApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Connectivity.WwanContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Connectivity.WwanContract\1.0.0.0\Windows.Networking.Connectivity.WwanContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivationCameraSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ContactActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ContactActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ContactActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract\1.0.0.0\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Calls.LockScreenCallContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Calls.LockScreenCallContract\1.0.0.0\Windows.ApplicationModel.Calls.LockScreenCallContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Resources.Management.ResourceIndexerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract\1.0.0.0\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.Core.SearchCoreContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.Core.SearchCoreContract\1.0.0.0\Windows.ApplicationModel.Search.Core.SearchCoreContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.SearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.SearchContract\1.0.0.0\Windows.ApplicationModel.Search.SearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Wallet.WalletContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Wallet.WalletContract\1.0.0.0\Windows.ApplicationModel.Wallet.WalletContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Custom.CustomDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Custom.CustomDeviceContract\1.0.0.0\Windows.Devices.Custom.CustomDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Portable.PortableDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Portable.PortableDeviceContract\1.0.0.0\Windows.Devices.Portable.PortableDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.Extensions.ExtensionsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.Extensions.ExtensionsContract\1.0.0.0\Windows.Devices.Printers.Extensions.ExtensionsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.PrintersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.PrintersContract\1.0.0.0\Windows.Devices.Printers.PrintersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Scanners.ScannerDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Scanners.ScannerDeviceContract\1.0.0.0\Windows.Devices.Scanners.ScannerDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Sms.LegacySmsApiContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Sms.LegacySmsApiContract\1.0.0.0\Windows.Devices.Sms.LegacySmsApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Gaming.Preview.GamesEnumerationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Gaming.Preview.GamesEnumerationContract\1.0.0.0\Windows.Gaming.Preview.GamesEnumerationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract\1.0.0.0\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Graphics.Printing3D.Printing3DContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Graphics.Printing3D.Printing3DContract\1.0.0.0\Windows.Graphics.Printing3D.Printing3DContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Deployment.Preview.DeploymentPreviewContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Deployment.Preview.DeploymentPreviewContract\1.0.0.0\Windows.Management.Deployment.Preview.DeploymentPreviewContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Workplace.WorkplaceSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Workplace.WorkplaceSettingsContract\1.0.0.0\Windows.Management.Workplace.WorkplaceSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.AppCaptureContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.AppCaptureContract\1.0.0.0\Windows.Media.Capture.AppCaptureContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.CameraCaptureUIContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.CameraCaptureUIContract\1.0.0.0\Windows.Media.Capture.CameraCaptureUIContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Devices.CallControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Devices.CallControlContract\1.0.0.0\Windows.Media.Devices.CallControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.MediaControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.MediaControlContract\1.0.0.0\Windows.Media.MediaControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Playlists.PlaylistsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Playlists.PlaylistsContract\1.0.0.0\Windows.Media.Playlists.PlaylistsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Protection.ProtectionRenewalContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Protection.ProtectionRenewalContract\1.0.0.0\Windows.Media.Protection.ProtectionRenewalContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract\1.0.0.0\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Sockets.ControlChannelTriggerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Sockets.ControlChannelTriggerContract\1.0.0.0\Windows.Networking.Sockets.ControlChannelTriggerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.EnterpriseData.EnterpriseDataContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.EnterpriseData.EnterpriseDataContract\1.0.0.0\Windows.Security.EnterpriseData.EnterpriseDataContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.ExchangeActiveSyncProvisioning.EasContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.ExchangeActiveSyncProvisioning.EasContract\1.0.0.0\Windows.Security.ExchangeActiveSyncProvisioning.EasContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.GuidanceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.GuidanceContract\1.0.0.0\Windows.Services.Maps.GuidanceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.LocalSearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.LocalSearchContract\1.0.0.0\Windows.Services.Maps.LocalSearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.SystemManufacturers.SystemManufacturersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract\1.0.0.0\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileHardwareTokenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileHardwareTokenContract\1.0.0.0\Windows.System.Profile.ProfileHardwareTokenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileRetailInfoContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileRetailInfoContract\1.0.0.0\Windows.System.Profile.ProfileRetailInfoContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileContract\1.0.0.0\Windows.System.UserProfile.UserProfileContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileLockScreenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileLockScreenContract\1.0.0.0\Windows.System.UserProfile.UserProfileLockScreenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.ApplicationSettings.ApplicationsSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.ApplicationSettings.ApplicationsSettingsContract\1.0.0.0\Windows.UI.ApplicationSettings.ApplicationsSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.AnimationMetrics.AnimationMetricsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract\1.0.0.0\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.CoreWindowDialogsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.CoreWindowDialogsContract\1.0.0.0\Windows.UI.Core.CoreWindowDialogsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Xaml.Hosting.HostingContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Xaml.Hosting.HostingContract\1.0.0.0\Windows.UI.Xaml.Hosting.HostingContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Web.Http.Diagnostics.HttpDiagnosticsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract\1.0.0.0\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
</ItemGroup>
```

Ces références sont un mélange précis de références qui sont nécessaires au fonctionnement correct de ce serveur hybride. Le protocole consiste à ouvrir le fichier .csproj (tel que décrit dans « Comment modifier le type de sortie du projet ») et à ajouter ces références si nécessaire.

Une fois ces références configurées correctement, la tâche suivante consiste à implémenter la fonctionnalité du serveur. Consultez la rubrique MSDN [meilleures pratiques pour l’interopérabilité avec les composants Windows Runtime (applications UWP à l’aide de C\#/VB/C++ et XAML)](https://docs.microsoft.com/previous-versions/windows/apps/hh750311(v=win.10)).
Cette tâche consiste à créer une DLL de composant Windows Runtime qui est en mesure d’appeler le code de bureau dans le cadre de son implémentation. L’exemple fourni comprend les principaux modèles utilisés dans Windows Runtime :

-   Appels de méthode

-   Sources des événements Windows Runtime par le composant de bureau

-   Opérations asynchrones Windows Runtime

-   Renvoi de tableaux de types de base

**Installer**

Pour installer l’application, copiez l’implémentation **winmd** vers le répertoire correct est spécifié dans le manifeste d’associées chargées de l’application : <ActivatableClassAttribute>de valeur = « path ». Copiez également les fichiers de support associés et les DLL proxy/stub (décrites plus bas). Impossible de copier l’implémentation **winmd** au serveur emplacement du répertoire entraîne le chargement indépendant tous les appels de l’application à nouveau sur le RuntimeClass générera une erreur « classe non enregistrée ». Si vous n’installez pas le proxy/stub (ou ne l’inscrivez pas) tous les appels échoueront sans valeur de retour. Cette erreur ce dernier est fréquemment **pas** associés aux exceptions visibles.
Si des exceptions visibles sont provoquées par cette erreur de configuration, elles font référence à un « cast incorrect ».

**Considérations sur l’implémentation serveur**

Le serveur Windows Runtime de bureau peut être considéré comme étant basé sur un « travail » ou une « tâche ». Chaque appel au serveur se produit sur un thread sans interface utilisateur et tout le code doit être multithread et safe. La partie de l’application installée hors Windows Store qui appelle la fonctionnalité du serveur est très importante. Il ne faut pas appeler du code dont l’exécution dure trop longtemps à partir d’un thread d’interface utilisateur dans l’application installée hors Windows Store. Il existe deux façons de procéder :

1.  Quand vous appelez la fonctionnalité serveur à partir d’un thread d’interface utilisateur, utilisez toujours un modèle asynchrone dans la zone de surface publique et dans l’implémentation du serveur.

2.  Appelez la fonctionnalité du serveur à partir d’un thread en arrière-plan dans l’application installée hors Windows Store.

**Asynchrone Windows Runtime dans le serveur**

Étant donné la nature entre processus du modèle d’application, les appels au serveur durent plus longtemps que le code qui s’exécute uniquement in-process. Il est généralement sûr d’appeler une propriété qui retourne une valeur en mémoire, car elle s’exécutera assez rapidement et le thread de l’interface utilisateur ne sera pas bloqué longtemps. Cependant, les appels qui entraînent des E/S (y compris la gestion de fichiers et les récupérations de base de données) peuvent potentiellement bloquer le thread d’interface utilisateur appelant et provoquer l’arrêt de l’application pour non réponse. De plus, les appels de propriétés sur des objets ne sont pas conseillés dans cette architecture d’application pour des raisons de performances.
La section suivante décrit cela en détail.

Un serveur correctement implémenté implémente directement les appels faits aux threads de l’interface utilisateur via le modèle Windows Runtime asynchrone. Cela peut être implémenté en suivant ce modèle. Tout d’abord, la déclaration (là encore, à partir de l’exemple fourni) :

```csharp
public IAsyncOperation<int> FindElementAsync(int input)
```

Une opération Windows Runtime asynchrone qui retourne un entier est déclarée.
L’implémentation de l’opération asynchrone prend normalement la forme suivante :

```csharp
return Task<int>.Run( () =>
{
    int retval = ...
    // execute some potentially long-running code here 
}).AsAsyncOperation<int>();

```

>**Remarque** Il est possible d’attendre des opérations qui peuvent durer longtemps pendant l’écriture de l’implémentation. Dans ce cas, le **Task.Run** code doit être déclarée :

```csharp
return Task<int>.Run(async () =>
{
    int retval = ...
    // execute some potentially long-running code here 
    await ... // some other WinRT async operation or Task
}).AsAsyncOperation<int>();
```

Les clients de cette méthode asynchrone peuvent attendre cette opération tout comme n’importe quelle opération Windows Runtime asynchrone.

**Appeler des fonctionnalités de serveur à partir d’un thread d’arrière-plan d’application**

Comme le client et le serveur sont généralement écrits par la même organisation, une pratique de programmation peut décider que tous les appels au serveur seront effectués via un thread en arrière-plan dans l’application installée hors Windows Store. Un appel direct qui recueille un ou plusieurs lots de données du serveur peut être effectué à partir d’un thread d’arrière-plan. Quand tous les résultats sont récupérés, les lots de données en mémoire dans le processus d’application peuvent être récupérés directement du thread d’interface utilisateur. C\# objets sont naturellement agiles entre les threads d’arrière-plan et de threads d’interface utilisateur qui sont donc particulièrement utiles pour ce type de modèle d’appel.

## <a name="creating-and-deploying-the-windows-runtime-proxy"></a>Création et déploiement du proxy Windows Runtime

Dans la mesure où l’approche IPC comprend le marshaling des interfaces Windows Runtime entre deux processus, un proxy et un stub Windows Runtime inscrits globalement doivent être utilisés.

**Création du proxy dans Visual Studio**

Le processus de création et l’inscription de proxies et stubs pour une utilisation à l’intérieur d’un package d’application UWP régulière sont décrites dans la rubrique [déclenchement d’événements dans les composants Windows Runtime](https://docs.microsoft.com/previous-versions/windows/apps/dn169426(v=vs.140)).
Les étapes décrites dans cet article sont plus compliquées que la procédure décrite ci-dessous, car elles décrivent l’inscription du proxy/stub dans le package de l’application (plutôt qu’une inscription globale).

**Étape 1 :** À l’aide de la solution pour le projet de composant de bureau, créez un projet de Proxy/Stub dans Visual Studio :

**Solution > Ajouter > projet > Visual C++ > option de Console Win32 sélectionner la DLL.**

Pour connaître les étapes ci-dessous, nous supposons que le composant serveur est appelé **MyWinRTComponent**.

**Étape 3 :** Supprimer tous les fichiers CPP/H du projet.

**Étape 4 :** La section précédente « Définir le contrat » contient une commande POST-Build qui exécute **winmdidl.exe**, **midl.exe**, **mdmerge.exe**, et ainsi de suite. L’une des sorties de l’étape midl de cette commande post-build génère quatre sorties importantes :

a) Dlldata.c

b) Un fichier d’en-tête (MyWinRTComponent.h)

(c) A \* \_i, partie c fichier (par exemple, MyWinRTComponent\_i, partie c)

(d) A \* \_p.c fichier (par exemple, MyWinRTComponent\_p.c)

**Étape 5 :** Ajoutez ces quatre fichiers générés au projet « MyWinRTProxy ».

**Étape 6 :** Ajouter un fichier def au projet de « MyWinRTProxy » **(projet > Ajouter un nouvel élément > Code > fichier de définition de Module**) et mettre à jour le contenu à :

LIBRARY MyWinRTComponent.Proxies.dll

EXPORTS

DllCanUnloadNow PRIVATE

DllGetClassObject PRIVATE

DllRegisterServer PRIVATE

DllUnregisterServer PRIVATE

**Étape 7 :** Ouvrez les propriétés du projet « MyWinRTProxy » :

**Propriétés de configuration de le > Général > nom de la cible :**

MyWinRTComponent.Proxies

**C/C++ > définitions de préprocesseur > Ajouter**

"WIN32;\_WINDOWS;REGISTER\_PROXY\_DLL"

**C/C++ > en-tête précompilé : Sélectionnez « Ne pas à l’aide d’en-tête précompilé »**

**L’éditeur de liens > Général > bibliothèque d’importation ignorée : Sélectionnez « Oui »**

**L’éditeur de liens > entrée > dépendances supplémentaires : Ajouter rpcrt4.lib;runtimeobject.lib**

**L’éditeur de liens > Windows métadonnées > Générer des métadonnées de Windows : Sélectionnez « Non »**

**Étape 8 :** Générez le projet « MyWinRTProxy ».

**Déploiement du proxy**

Le proxy doit être inscrit globalement. Pour ce faire, le processus d’installation doit appeler DllRegisterServer dans la DLL proxy. Dans la mesure où la fonctionnalité ne prend en charge que des serveurs x86 (pas de prise en charge 64 bits), la configuration la plus simple utilise un serveur 32 bits, un proxy 32 bits et une application installée hors Windows Store 32 bits. Le proxy se trouve généralement avec l’implémentation **winmd** pour le composant de bureau.

Une étape de configuration supplémentaire est nécessaire. Pour que le processus d’installation hors Windows Store charge et exécute le proxy, le répertoire doit être marqué « lecture / exécution » pour ALL_APPLICATION_PACKAGES. Utilisez pour cela l’outil en ligne de commande **icacls.exe**. Cette commande doit être exécutée sur le répertoire dans lequel se trouve l’implémentation **winmd** et les DLL proxy/stub :

*icacls. / Grant /T \*S-1-15-2-1:RX*

## <a name="patterns-and-performance"></a>Modèles et performances

Il est très important que les performances du transport interprocessus soient étroitement surveillées. Un appel interprocessus est deux fois plus coûteux qu’un appel in-process. Si vous créez des conversations « bavardes » entre les processus ou effectuez des transferts répétés d’objets de grande taille, comme des images bitmap, les performances de l’application peuvent s’en ressentir.

Voici une liste d’éléments à prendre en compte :

-   Il n’est pas conseillé d’utiliser des appels de méthode synchrones à partir du thread d’interface utilisateur de l’application. Appelez la méthode à partir d’un thread d’arrière-plan dans l’application, puis utilisez CoreWindowDispatcher pour obtenir des résultats dans le thread d’interface utilisateur si nécessaire.

-   L’appel d’opérations async à partir du thread d’interface utilisateur est safe, mais pensez aux problèmes de performances présentées dans ce document.

-   Le transfert en bloc des résultats limite le trafic interprocessus. La construction Windows Runtime Array est utilisée pour cela.

-   Retour *liste<T>*  où *T* est un objet à partir d’une extraction de propriété ou d’opération asynchrone, entraîne un grand nombre d’échanges excessifs entre processus. Par exemple, supposons que vous retournez un*liste&lt;personnes&gt;*  objets. Chaque itération correspondra à un appel interprocessus. Chaque *personnes* objet retourné est représentée par un proxy et chaque appel à une méthode ou propriété sur cet objet individuel entraîne un appel interprocessus. Par conséquent, un « innocents » *liste&lt;personnes&gt;*  objet où *nombre* est volumineux entraîne un grand nombre d’appels lents. Le transfert en bloc de structures de contenu dans un tableau donne de meilleures performances. Exemple :

```csharp
struct PersonStruct
{
    String LastName;
    String FirstName;
    int Age;
   // etc.
}
```

Puis retour * PersonStruct\[\]* à la place de *liste&lt;PersonObject&gt;* .
Toutes les données sont transmises en un saut (hop) interprocessus

Comme pour tous les éléments à prendre en compte pour les performances, les mesures et le test sont critiques. La télémétrie doit être utilisée dans différentes opérations pour connaître la durée de chacune d’elles. Il est important de réaliser ces mesures sur une plage : par exemple, quel est le temps nécessaire pour consommer tous les objets *People* pour une requête spécifique dans l’application installée hors Windows Store ?

Le test de charge variable peut également être utilisé. Pour cela, ajoutez des crochets de test de performance dans l’application qui introduisent des chargements différés variables pendant le traitement du serveur. Cela simule différents types de charge et la réaction de l’application à différentes performances du serveur.
L’exemple montre comment introduire des délais dans le code en utilisant des techniques async appropriées. La durée exacte du délai et la plage de randomisation à ajouter à la charge artificielle dépendent de la conception de l’application et de l’environnement dans lequel l’application s’exécute.

## <a name="development-process"></a>Processus de développement

Lorsque vous souhaitez apporter des modifications au serveur, assurez-vous au préalable qu’aucune instance démarrée précédemment n’est encore en cours d’exécution. Même si le code COM nettoie le processus, le minuteur d’arrêt prend plus de temps et nuit à l’efficacité du développement itératif. La suppression d’instances précédentes en cours d’exécution est donc une étape normale du développement. Cela nécessite que le développeur sache quelle instance dllhost héberge le serveur.

Il est possible de trouver et d’arrêter le processus serveur via le Gestionnaire des tâches ou d’une autre application tierce. L’outil de ligne de commande **TaskList.exe **est également inclus et a une syntaxe flexible, par exemple :

  
 | **Command** | **Action** |
 | ------------| ---------- |
 | tasklist | Affiche une liste de tous les processus en cours d’exécution dans l’ordre approximatif de leur date de création. Les derniers processus créés sont affichés vers le bas de la liste. |
 | tasklist /FI "IMAGENAME eq dllhost.exe" /M | Affiche des informations sur toutes les instances dllhost.exe. Le commutateur /M répertorie les modules ayant été chargés. |
 | tasklist /FI "PID eq 12564" /M | Cette option vous permet de rechercher une instance dllhost.exe en fonction de son PID (si vous le connaissez). |

La liste des modules pour un serveur du service broker doit répertorier *clrhost.dll* dans sa liste des modules chargés.

## <a name="resources"></a>Ressources

-   [Modèles de projet répartie composant WinRT pour Windows 10 et VS 2015](https://marketplace.visualstudio.com/vsgallery/10be07b3-67ef-4e02-9243-01b78cd27935)

-   [Exemple de composant de WinRT répartie NorthwindRT](https://go.microsoft.com/fwlink/p/?LinkID=397349)

-   [Distribution d’applications Microsoft Store dignes de confiance](https://go.microsoft.com/fwlink/p/?LinkID=393644)

-   [Contrats d’application et les extensions (applications du Windows Store)](https://docs.microsoft.com/previous-versions/windows/apps/hh464906(v=win.10))

-   [Comment supprimer des applications sur Windows 10](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)

-   [Déploiement d’applications UWP pour les entreprises](https://go.microsoft.com/fwlink/p/?LinkID=264770)

