---
ms.assetid: 4b0c86d3-f05b-450b-bf9c-6ab4d3f07d31
description: Cette feuille de route propose une vue d’ensemble des principales fonctionnalités d’entreprise pour les applications UWP (plateforme Windows universelle) et Windows 10.
title: Entreprise
ms.date: 08/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fae4d5b57ac5cfb5c47fca1a2f3476cd16a56534
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57582632"
---
# <a name="enterprise"></a>Entreprise

Cet article propose une vue d’ensemble des principales fonctionnalités d’entreprise fournies par les applications UWP (plateforme Windows universelle) pour Windows 10. Pour regarder une vidéo présentant en détail certaines de ces fonctionnalités, consultez [Rapidly Construct LOB Applications with UWP and Visual Studio](https://channel9.msdn.com/Events/Build/2018/BRK3502).

<a id="template-studio" />

### <a name="windows-template-studio"></a>Windows Template Studio

Windows Template Studio est une extension Visual Studio 2017 qui accélère la création d’applications UWP (plateforme Windows universelle) à l’aide d’un Assistant. Le projet UWP résultant est un code bien formé et lisible, qui intègre les dernières fonctionnalités de Windows 10 tout en implémentant les bonnes pratiques et les modèles ayant fait leurs preuves.

![Windows Template Studio](images/windows-template-studio.png)

Consultez [Windows Template Studio](https://marketplace.visualstudio.com/items?itemName=WASTeamAccount.WindowsTemplateStudio)

<a id="desktop-style-UI" />

### <a name="controls-to-create-desktop-style-uis"></a>Contrôles permettant de créer des IU d’applications de bureau

Nous avons publié de nouveaux contrôles UWP XAML qui comblent le fossé entre une IU d’Application de bureau classique et une IU UWP.

Par exemple, les nouveaux contrôles [MenuBar](/windows/uwp/design/controls-and-patterns/menus), [DropDownButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button), [SplitButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button) et [CommandBarFlyout](/windows/uwp/design/controls-and-patterns/command-bar-flyout) vous offrent des moyens plus souples d’exposer les commandes. De plus, [EditableComboBox](/windows/uwp/design/controls-and-patterns/combo-box#make-a-combo-box-editable) permet à l’utilisateur d’entrer des valeurs qui ne figurent pas dans une liste prédéfinie d’options.

![MenuBar](images/menu-bar.png)

<a id="enterprise" />

### <a name="controls-to-support-enterprise-scenarios"></a>Contrôles permettant de prendre en charge des scénarios d’entreprise

[DataGridView](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/datagrid) offre un moyen flexible d’afficher une collection de données en lignes et en colonnes.

[TreeView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view) active une liste hiérarchique comportant des nœuds que vous pouvez développer et réduire, et qui contiennent des éléments imbriqués. Vous pouvez l’utiliser pour illustrer une structure de dossiers ou des relations imbriquées dans votre IU.

![Contrôle DataGrid](images/DataGrid.gif)


### <a name="windows-ui-library"></a>Bibliothèque d’IU Windows

La bibliothèque d’IU Windows est un ensemble de packages NuGet qui fournissent des contrôles et autres éléments d’interface utilisateur pour les applications UWP. Elle permet également une compatibilité de bas niveau avec les versions antérieures de Windows 10, pour que votre application fonctionne même si les utilisateurs n’ont pas le dernier système d’exploitation.

![Bibliothèque d’IU Windows](images/win-ui.png)

Consultez [Bibliothèque d’IU Windows (préversion)](https://docs.microsoft.com/uwp/toolkits/winui/).

<a id="xaml-islands" />

### <a name="uwp-controls-in-desktop-applications"></a>Contrôles UWP dans les Applications de bureau

Windows 10 vous permet désormais d’utiliser les contrôles UWP dans les Applications de bureau WPF, Windows Forms et C++ Win32. Cela signifie que vous pouvez améliorer l’apparence et les fonctionnalités de vos Applications de bureau existantes à l’aide des dernières fonctionnalités de l’IU Windows 10. Celles-ci sont disponibles uniquement via les contrôles UWP, tels que Windows Ink, et les contrôles prenant en charge Fluent Design System. Cette fonctionnalité est désignée sous le nom d’îles XAML.

Consultez [Contrôles UWP dans les applications de bureau](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls).

<a id="standard" />

### <a name="net-standard-20"></a>.NET Standard 2.0

.NET Standard comprend plus de 20 000 API supplémentaires par rapport à .NET Standard 1.x. Cela rend tellement plus facile la migration des bibliothèques .NET Framework existantes, puis leur utilisation dans différentes applications .NET, notamment votre application UWP.

![net-standard](images/dot-net-standard-project-template.png)

Consultez [Partager du code entre une application de bureau et une application UWP](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate).

<a id="sql-server" />

### <a name="sql-server-connectivity"></a>Connectivité SQL Server

Votre application peut se connecter directement à une base de données SQL Server, puis stocker et récupérer des données à l’aide de classes de l’espace de noms [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient?redirectedfrom=MSDN&view=netframework-4.7.2).

Consultez [Utiliser une base de données SQL Server dans une application UWP](https://docs.microsoft.com/en-us/windows/uwp/data-access/sql-server-databases).

<a id="MSIX" />

### <a name="msix-deployment"></a>Déploiement de MSIX

MSIX est le format de package d’applications Windows qui offre une expérience de packaging moderne pour toutes les applications Windows. Le format de package MSIX conserve les fonctionnalités des packages d’applications et des fichiers d’installation existants. De plus, il permet l’activation de fonctionnalités de packaging et de déploiement à la fois nouvelles et modernes pour les applications Win32, WPF et Windows Forms.

MSIX est un format de packaging conçu pour être sûr, sécurisé et fiable. Il est basé sur une combinaison des technologies d’installation .msi, .appx, App-V et ClickOnce.

![Icône MSIX](images/MSIX-App-Package.ico)

Consultez la [documentation de MSIX](https://docs.microsoft.com/windows/msix/).

<a id="distribution" />

## <a name="security"></a>Sécurité

Windows 10 fournit une suite de fonctionnalités de sécurité qui permet aux développeurs d’applications de protéger l’identité de leurs utilisateurs, la sécurité des réseaux d’entreprise ainsi que toutes les données professionnelles stockées sur des appareils. Nouveauté de Windows 10, Microsoft Passport constitue une alternative à l’utilisation de mots de passe. Il s’agit d’une solution d’authentification à deux facteurs facile à déployer et accessible à l’aide d’un code PIN ou de Windows Hello, qui fournit une sécurité de qualité professionnelle et prend en charge la reconnaissance des empreintes digitales et de l’iris ainsi que la reconnaissance faciale.

| Rubrique | Description |
|-------|-------------|
| [Présentation du développement d’applications Windows sécurisées](https://msdn.microsoft.com/library/windows/apps/mt622741) | Cet article introductif décrit les différentes fonctionnalités d’entreprise Windows lors des phases d’authentification, de données en transit et de données au repos. Il explique également comment vous pouvez intégrer ces phases dans vos applications. Il couvre une large gamme de rubriques et vise essentiellement à aider les architectes d’application à mieux comprendre les fonctionnalités Windows qui facilitent et accélèrent la création d’applications de plateforme Windows universelle. |
| [Authentification et identité des utilisateurs](https://msdn.microsoft.com/library/windows/apps/mt270184) | Les applications UWP disposent de plusieurs options pour l’authentification des utilisateurs qui sont décrites dans cet article. Pour l’entreprise, l’utilisation de la nouvelle fonctionnalité Microsoft Passport est fortement recommandée. Microsoft Passport remplace les mots de passe par la méthode d’authentification à 2 facteurs (2FA) forte en vérifiant les informations d’identification existantes et en créant des informations d’identification spécifiques à l’appareil, protégées par un mouvement de l’utilisateur reposant sur l’entrée de son code PIN ou par la biométrie. |
| [Chiffrement](https://msdn.microsoft.com/library/windows/apps/mt270191) | La section relative au chiffrement fournit une vue d’ensemble des fonctionnalités de chiffrement disponibles pour les applications UWP. Les articles comprennent des procédures pas à pas introductives pour chiffrer facilement les données professionnelles sensibles et couvrent également des sujets avancés tels que la manipulation des clés de chiffrement et l’utilisation des codes d’authentification de message (MAC), codes de hachage et signatures. |
| [Protection des informations Windows (WIP)](wip-hub.md) | Il s’agit d’une rubrique de hub destinée aux développeurs abordant de manière exhaustive la relation de la Protection des informations Windows avec les fichiers, les mémoires tampons, le Presse-papiers, la mise en réseau, les tâches en arrière-plan et la protection des données verrouillées. |

## <a name="data-binding-and-databases"></a>Liaison de données et bases de données

La liaison de données est un moyen dont dispose l’interface utilisateur de votre application pour afficher des données provenant d’une source externe (par exemple, une base de données) et éventuellement rester synchronisée avec ces données. La liaison de données vous permet de séparer les problématiques liées aux données de celles liées à l’interface utilisateur, ce qui se traduit par un modèle conceptuel plus simple et l’amélioration de la lisibilité, de la testabilité et de la gestion de la maintenance de votre application.

| Rubrique | Description |
|-------|-------------|
| [Vue d’ensemble de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt269383) | Cette rubrique vous montre comment lier un contrôle (ou tout autre élément d’IU) à un seul élément, ou comment lier un contrôle d’éléments à une collection d’éléments dans une application UWP (plateforme Windows universelle). Elle explique également comment contrôler le rendu des éléments, implémenter un affichage des détails en fonction d’une sélection et convertir des données pour l’affichage. |
| [Entity Framework 7 pour UWP](https://msdn.microsoft.com/library/windows/apps/mt592863) | Entity Framework 7, qui prend en charge UWP, vous permet d’exécuter facilement des requêtes complexes dans de grands ensembles de données. Dans cette procédure pas à pas, vous allez générer une application UWP qui propose un accès de base aux données d’une base de données SQLite locale à l’aide d’Entity Framework. |
| [Base de données SQLite locale](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/10) | Cette vidéo est un guide du développeur complet sur l’utilisation de SQLite, la solution recommandée pour les bases de données d’applications locales. Visitez [SQLite](https://www.sqlite.org/download.html) pour télécharger la dernière version pour UWP, ou utilisez la version fournie avec le Kit de développement logiciel (SDK) Windows 10. |

## <a name="networking-and-data-serialization"></a>Réseau et sérialisation des données

Les applications métier ont souvent besoin de communiquer avec des données ou de les stocker dans un grand nombre d’autres systèmes. Cette opération est généralement effectuée en vous connectant à un service réseau (à l’aide de protocoles tels que REST ou SOAP), puis en sérialisant ou désérialisant les données dans un format commun. L’utilisation des services réseau et de la sérialisation des données dans les applications UWP est similaire à celle dans les applications WPF, WinForms et ASP.NET. Voir les articles suivants pour plus d’informations.

| Rubrique | Description |
|-------|-------------|
| [Notions de base relatives aux réseaux](https://msdn.microsoft.com/library/windows/apps/mt280233) | Cette procédure pas à pas explique les concepts de mise en réseau de base pertinents pour toutes les applications UWP, quels que soient les protocoles de communication utilisés.  |
| [Quelle technologie réseau ?](https://msdn.microsoft.com/library/windows/apps/mt280235) | Une vue d’ensemble des technologies de réseau disponibles pour les applications UWP, avec des conseils qui vous aideront à choisir les technologies appropriées pour votre application. |
| [Sérialisation XML et SOAP](https://msdn.microsoft.com/library/90c86ass.aspx) | La sérialisation XML convertit les objets en un flux XML conforme à un langage XSD (définition de schéma XML) spécifique. Pour effectuer une conversion entre XML et une classe fortement typée, vous pouvez utiliser la classe [XDocument](https://msdn.microsoft.com/library/system.xml.linq.xdocument.aspx) native ou une bibliothèque externe. |
| [Sérialisation JSON](https://msdn.microsoft.com/library/windows/apps/br240639) | La sérialisation JSON (JavaScript Object Notation) est un format populaire utilisé pour la communication avec les API REST. Le [Newtonsoft Json.NET](https://www.newtonsoft.com/json), qui est entièrement pris en charge par les applications UWP. |

## <a name="devices"></a>Appareils

Afin d’interagir avec des outils métier comme des imprimantes, des scanneurs de codes-barres ou des lecteurs de cartes à puce, vous jugerez peut-être nécessaire d’intégrer des appareils ou des capteurs externes à votre application. Voici quelques exemples des fonctionnalités que vous pouvez ajouter à votre application à l’aide de la technologie décrite dans cette section.

| Rubrique  | Description |
|--------|-------------|
| [Énumérer les appareils](https://msdn.microsoft.com/library/windows/apps/mt187355) | Cet article décrit comment utiliser l’espace de noms [Windows.Devices.Enumeration](https://msdn.microsoft.com/library/windows/apps/br225459) pour rechercher des appareils connectés au système, en interne, en externe ou détectables sur les protocoles sans fil ou réseau. Commencez ici si vous créez une application qui fonctionne avec des appareils. |
| [Impression et numérisation](https://msdn.microsoft.com/library/windows/apps/mt204544) | Explique comment imprimer et numériser à partir de votre application, et notamment comment se connecter aux appareils métier (par exemple, les systèmes de point de vente (PDV), les imprimantes de reçus ainsi que les scanneurs à chargeur à grande capacité) et comment les utiliser. |
| [Bluetooth](https://msdn.microsoft.com/library/windows/apps/mt270288) | Outre l’utilisation des connexions Bluetooth traditionnelles pour envoyer et recevoir des données ou contrôler les appareils, Windows 10 permet d’utiliser la technologie Bluetooth Low Energy (BTLE) pour envoyer ou recevoir des balises en arrière-plan. Utilisez-la pour afficher des notifications ou activer des fonctionnalités quand un utilisateur s’approche d’un emplacement particulier ou le quitte. |
| [Stockage partagé d’entreprise](enterprise-shared-storage.md) | Dans les scénarios où l’appareil est verrouillé, découvrez comment partager les données au sein de la même application, entre les instances d’une application ou entre les applications. |

## <a name="device-targeting"></a>Ciblage des appareils

Aujourd’hui, de nombreux utilisateurs travaillent avec leur propre téléphone ou tablette, des appareils qui présentent des facteurs de forme et des tailles d’écran différents. Avec la plateforme Windows universelle (UWP), vous pouvez écrire une application métier unique qui s’exécute en toute transparence sur tous les types d’appareils, y compris les ordinateurs de bureau et quel que soit le nombre de PPP de l’écran, ce qui vous permet d’optimiser la portée de votre application ainsi que l’efficacité de votre code.

| Rubrique | Description |
|-------|-------------|
| [Guide des applications UWP](https://msdn.microsoft.com/library/windows/apps/dn894631) | Dans ce guide introductif, vous allez vous familiariser avec la plateforme UWP Windows 10. Vous découvrirez, entre autres, ce qu’est une famille d’appareils et comment déterminer celle à cibler, quels sont les nouveaux volets et contrôles d’interface utilisateur permettant d’adapter votre interface utilisateur à différents facteurs de forme d’appareil, et comment utiliser et contrôler la surface d’API disponible dans votre application. |
| [Exemple de code d’IU XAML adaptative](https://go.microsoft.com/fwlink/p/?LinkId=619992) | Cet exemple de code montre toutes les options de disposition et tous les contrôles possibles pour votre application, quel que soit le type d’appareil. Il vous permet d’interagir avec les panneaux pour découvrir comment obtenir les dispositions que vous recherchez. En plus de vous présenter la façon dont chaque contrôle répond à différents facteurs de forme, l’application réagit et indique les différentes méthodes permettant d’obtenir une interface utilisateur adaptative. |
| [Rubrique Xamarin](/xamarin/) | Xamarin pour le ciblage du téléphone |

## <a name="deployment"></a>Déploiement

Vous disposez d’options pour la distribution des applications aux utilisateurs de votre organisation. Vous pouvez utiliser le Microsoft Store pour Entreprises ou la gestion existante des appareils mobiles, ou vous pouvez effectuer un chargement indépendant des applications sur les appareils. Vous pouvez également mettre vos applications à la disposition du grand public en les publiant sur le Microsoft Store.

| Rubrique | Description |
|-------|-------------|
| [Distribuer des applications métier aux entreprises](https://msdn.microsoft.com/library/windows/apps/mt608995) | Vous pouvez publier des applications métier directement à l’attention des entreprises pour une acquisition en volume via le Microsoft Store pour Entreprises, sans mettre vos applications à la disposition du grand public. |
| [Chargement de la version test des applications](https://technet.microsoft.com/library/mt269549) | Lorsque vous chargez de manière indépendante une application, vous déployez un package d’application signée sur un appareil. Vous conservez la signature, l’hébergement et le déploiement de ces applications. Le processus de chargement indépendant d’applications est simplifié pour Windows 10.             |
| [Publier des applications sur le Microsoft Store](https://dev.windows.com/publish) | Le Microsoft Store unifié vous permet de publier et de gérer toutes vos applications pour l’ensemble des appareils Windows. Personnalisez la disponibilité de votre application à l’aide d’une tarification par marché, de contrôles de distribution et de visibilité et d’autres options. |

## <a name="enterprise-uwp-samples"></a>Exemples UWP pour entreprises

| Rubrique |  Description |
|------ |--------------|
| [Exemple de l’inventaire VanArsdel](https://github.com/Microsoft/InventorySample) | Exemple d’application UWP qui présente des scénarios métier. L’exemple est basé sur la création et la gestion de clients, de commandes et de produits pour la société fictive VanArsdel. |
| [Exemple de base de données de commandes de clients](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | Exemple d’application UWP qui présente des fonctionnalités utiles aux développeurs d’entreprise, par exemple l’authentification AAD (Azure Active Directory), les contrôles d’IU (notamment une grille de données), l’intégration de bases de données SQL Azure et Sqlite, Entity Framework ainsi que les services d’API cloud. L’exemple est basé sur la création et la gestion de comptes clients, de commandes et de produits pour la société fictive Contoso. |

## <a name="patterns-and-practices"></a>Modèles et pratiques

Les bases de code pour les applications d’entreprise à grande échelle peuvent être difficiles à gérer. Prism est un framework permettant de générer des applications XAML pouvant être testées, faciles à gérer et faiblement couplées dans WPF, Windows 10 UWP et Xamarin Forms. Prism fournit une implémentation d’une collection de modèles de conception utiles pour écrire des applications XAML bien structurées et faciles à gérer, notamment des modèles MVVM, d’injection de dépendance, de commandes, EventAggregator, etc.

Pour plus d’informations sur Prism, voir le [référentiel GitHub](https://github.com/PrismLibrary/Prism).

 

 
