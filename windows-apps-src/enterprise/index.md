---
ms.assetid: 4b0c86d3-f05b-450b-bf9c-6ab4d3f07d31
description: Cette feuille de route propose une vue d’ensemble de fonctionnalités principales d’entreprise pour les applications Windows 10 et la plateforme Windows universelle (UWP).
title: Entreprise
ms.date: 08/30/2018
ms.topic: article
keywords: 'windows10, uwp'
ms.localizationpriority: medium
---
# <a name="enterprise"></a>Entreprise

Cet article fournit une vue d’ensemble de fonctionnalités principales d’entreprise fournis par la plateforme Windows universelle (UWP) pour les applications Windows 10. Pour une vidéo qui illustre certaines de ces fonctionnalités en détail, consultez [Construire rapidement des Applications cœur de métier avec UWP et de Visual Studio](https://channel9.msdn.com/Events/Build/2018/BRK3502).

<a id="template-studio" />

### <a name="windows-template-studio"></a>WindowsTemplateStudio

Windows Template Studio est une Extension Visual Studio 2017 qui accélère la création de nouvelles applications de plateforme Windows universelle (UWP) à l’aide d’une expérience de type Assistant. Le projet UWP qui en résulte est correct, lisible car le code qui incorpore les dernières fonctionnalités de Windows 10 lors de la mise en œuvre des meilleures pratiques et modèles éprouvés.

![WindowsTemplateStudio](images/windows-template-studio.png)

Voir [Windows Template Studio](https://marketplace.visualstudio.com/items?itemName=WASTeamAccount.WindowsTemplateStudio)

<a id="desktop-style-UI" />

### <a name="controls-to-create-desktop-style-uis"></a>Contrôles pour créer des interfaces utilisateur de bureau-style

Nous avons publié nouveaux contrôles UWP XAML qui remplissent l’écart entre une application de bureau classique l’interface utilisateur et un UI UWP.

Par exemple, les nouveaux contrôles de [barre de menus](/windows/uwp/design/controls-and-patterns/menus), [DropDownButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button), [bouton partagé](/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button)et [CommandBarFlyout](/windows/uwp/design/controls-and-patterns/command-bar-flyout) offrent des moyens plus souples pour exposer des commandes et [EditableComboBox](/windows/uwp/design/controls-and-patterns/combo-box#make-a-combo-box-editable) Commençons par l’utilisateur entrer les valeurs qui ne sont pas répertoriées dans une liste prédéfinie d’options.

![Barre de menus](images/menu-bar.png)

<a id="enterprise" />

### <a name="controls-to-support-enterprise-scenarios"></a>Contrôles pour prendre en charge des scénarios d’entreprise

[DataGridView](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/datagrid) offre une grande souplesse pour afficher une collection de données en lignes et colonnes.

Le [contrôle TreeView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view) Active une liste hiérarchique comportant des nœuds qui contiennent des éléments imbriqués développement et. Il peut être utilisé pour illustrer une structure de dossiers ou des relations imbriquées dans votre interface utilisateur.

![Contrôle DataGrid](images/DataGrid.gif)


### <a name="windows-ui-library"></a>Bibliothèque de l’interface utilisateur de Windows

La bibliothèque de l’interface utilisateur de Windows est un ensemble de packages NuGet qui fournissent des contrôles et autres éléments d’interface utilisateur pour les applications UWP. Il permet également la compatibilité de bas niveau avec les versions antérieures de Windows 10, afin que votre application fonctionne même si les utilisateurs n’aient pas le dernier système d’exploitation.

![Bibliothèque de l’interface utilisateur de Windows](images/win-ui.png)

Voir la [Bibliothèque de l’interface utilisateur de Windows (version d’évaluation)](https://docs.microsoft.com/uwp/toolkits/winui/).

<a id="xaml-islands" />

### <a name="uwp-controls-in-desktop-applications"></a>Contrôles UWP dans des applications de bureau

Windows 10 vous permet désormais d’utiliser les contrôles UWP dans les applications de bureau C++ Win32, Windows Forms et WPF. Cela signifie que vous pouvez améliorer l’apparence et les fonctionnalités de vos applications de bureau existantes avec les dernières fonctionnalités de l’interface utilisateur de Windows 10 qui sont uniquement disponibles via les contrôles UWP, telles que Windows Ink et les contrôles qui prennent en charge le système Fluent Design. Cette fonctionnalité est appelée (îles) XAML.

Voir [contrôles UWP dans les applications de bureau](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls).

<a id="standard" />

### <a name="net-standard-20"></a>.NET Standard2.0

.NET Standard inclut plus de 20 000 plus important d’API que .NET Standard 1.x. Cela facilite beaucoup à migrer des bibliothèques .NET Framework existantes, puis les utiliser sur différentes applications .NET, y compris votre application UWP.

![NET standard](images/dot-net-standard-project-template.png)

Voir [partager du code entre une application de bureau et une application UWP](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate).

<a id="sql-server" />

### <a name="sql-server-connectivity"></a>Connectivité SQL Server

Votre application peut se connecter directement à une base de données SQLServer, puis stocker et récupérer des données à l’aide de classes dans l’espace de noms [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient?redirectedfrom=MSDN&view=netframework-4.7.2).

Voir [Utiliser une base de données SQLServer dans une applicationUWP](https://docs.microsoft.com/en-us/windows/uwp/data-access/sql-server-databases).

<a id="MSIX" />

### <a name="msix-deployment"></a>Déploiement MSIX

MSIX est le format de package d’application Windows qui fournit une expérience de mise en package moderne à toutes les applications Windows. Le format du package MSIX conserve les fonctionnalités de packages d’application existant et installer les fichiers en plus de l’activation des fonctionnalités de création de packages et de déploiement modernes, nouvelle pour les applications Win32, WPF et Windows Forms.

MSIX est un format d’empaquetage conçu pour être sûr, sécurisé et fiable, basée sur une combinaison de .msi, .appx, App-V et ClickOnce technologies d’installation.

![Icône MSIX](images/MSIX-App-Package.ico)

Consultez la [documentation MSIX](https://docs.microsoft.com/windows/msix/).

<a id="distribution" />

## <a name="security"></a>Sécurité

Windows 10 fournit un ensemble de fonctionnalités de sécurité pour les développeurs d’applications protéger l’identité de leurs utilisateurs, la sécurité des réseaux d’entreprise et les données professionnelles stockées sur des appareils. Nouveauté de Windows 10 est Microsoft Passport, une solution facile à déployer le mot de passe à 2 facteurs qui est accessible à l’aide d’un code confidentiel ou Windows Hello, qui fournit la sécurité de qualité professionnelle et prend en charge de la reconnaissance, des empreintes digitales et la reconnaissance de l’iris en fonction.

| Rubrique | Description |
|-------|-------------|
| [Présentation du développement d’applications Windows sécurisées](https://msdn.microsoft.com/library/windows/apps/mt622741) | Cet article introductif décrit les différentes fonctionnalités d’entreprise Windows lors des phases d’authentification, de données en transit et de données au repos. Il explique également comment vous pouvez intégrer ces phases dans vos applications. Il couvre une large gamme de rubriques et vise essentiellement à aider les architectes d’application à mieux comprendre les fonctionnalités de Windows qui facilitent la création d’applications de plateforme Windows universelle rapide et simple. |
| [Authentification et identité des utilisateurs](https://msdn.microsoft.com/library/windows/apps/mt270184) | Les applications UWP disposent de plusieurs options pour l’authentification des utilisateurs qui sont décrites dans cet article. Pour l’entreprise, l’utilisation de la nouvelle fonctionnalité Microsoft Passport est fortement recommandée. Microsoft Passport remplace les mots de passe par une authentification à 2 facteurs (2FA) forte en vérifiant les informations d’identification existantes et en créant une information d’identification propre à l’appareil qui protège un mouvement biométrique ou basée sur le code confidentiel utilisateur, ce qui entraîne une pratique à la fois et hautement expérience sécurisée. |
| [Chiffrement](https://msdn.microsoft.com/library/windows/apps/mt270191) | La section relative au chiffrement fournit une vue d’ensemble des fonctionnalités de chiffrement disponibles pour les applications UWP. Les articles comprennent des procédures pas à pas introductives pour chiffrer facilement les données professionnelles sensibles et couvrent également des sujets avancés tels que la manipulation des clés de chiffrement et l’utilisation des codes d’authentification de message (MAC), codes de hachage et signatures. |
| [Protection des informations Windows (WIP)](wip-hub.md) | Il s’agit d’une rubrique de hub destinée aux développeurs abordant de manière exhaustive la relation de la Protection des informations Windows avec les fichiers, les mémoires tampons, le Presse-papiers, la mise en réseau, les tâches en arrière-plan et la protection des données verrouillées. |

## <a name="data-binding-and-databases"></a>Liaison de données et bases de données

La liaison de données est un moyen dont dispose l’interface utilisateur de votre application pour afficher des données provenant d’une source externe (par exemple, une base de données) et éventuellement rester synchronisée avec ces données. La liaison de données vous permet de séparer les problématiques liées aux données de celles liées à l’interface utilisateur, ce qui se traduit par un modèle conceptuel plus simple et l’amélioration de la lisibilité, de la testabilité et de la gestion de la maintenance de votre application.

| Rubrique | Description |
|-------|-------------|
| [Vue d’ensemble de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt269383) | Cette rubrique vous montre comment lier un contrôle (ou un autre élément d’interface utilisateur) à un élément individuel ou lier un contrôle d’éléments à une collection d’éléments dans une application de plateforme Windows universelle (UWP). Elle explique également comment contrôler le rendu des éléments, implémenter un affichage des détails en fonction d’une sélection et convertir des données pour l’affichage. |
| [Entity Framework7 pour UWP](https://msdn.microsoft.com/library/windows/apps/mt592863) | Entity Framework7, qui prend en charge UWP, vous permet d’exécuter facilement des requêtes complexes dans de grands ensembles de données. Dans cette procédure pas à pas, vous allez créer une application UWP qui exécute l’accès aux données de base par rapport à une base de données SQLite locale à l’aide d’Entity Framework. |
| [Base de données SQLite locale.](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/10) | Cette vidéo est un guide du développeur complet sur l’utilisation de SQLite, la solution recommandée pour les bases de données d’applications locales. Visitez [SQLite](https://www.sqlite.org/download.html) pour télécharger la dernière version pour UWP, ou utilisez la version fournie avec le Kit de développement logiciel (SDK) Windows 10. |

## <a name="networking-and-data-serialization"></a>Réseau et sérialisation des données

Les applications métier ont souvent besoin de communiquer avec des données ou de les stocker dans un grand nombre d’autres systèmes. Cette opération est généralement effectuée en vous connectant à un service réseau (à l’aide de protocoles tels que REST ou SOAP), puis en sérialisant ou désérialisant les données dans un format commun. L’utilisation des services réseau et de la sérialisation des données dans les applications UWP est similaire à celle dans les applications WPF, WinForms et ASP.NET. Voir les articles suivants pour plus d’informations.

| Rubrique | Description |
|-------|-------------|
| [Notions de base en matière de réseau](https://msdn.microsoft.com/library/windows/apps/mt280233) | Cette procédure pas à pas explique les concepts de mise en réseau de base pertinents pour toutes les applications UWP, quels que soient les protocoles de communication utilisés.  |
| [Quelle technologie de réseau?](https://msdn.microsoft.com/library/windows/apps/mt280235) | Une vue d’ensemble des technologies de réseau disponibles pour les applications UWP, avec des conseils qui vous aideront à choisir les technologies appropriées pour votre application. |
| [Sérialisation XML et SOAP](https://msdn.microsoft.com/library/90c86ass.aspx) | Sérialisation XML convertit les objets en un flux XML qui est conforme à un langage de définition de schéma XML (XSD) spécifique. Pour effectuer une conversion entre XML et une classe fortement typée, vous pouvez utiliser la classe [XDocument](https://msdn.microsoft.com/library/system.xml.linq.xdocument.aspx) native ou une bibliothèque externe. |
| [Sérialisation JSON](https://msdn.microsoft.com/library/windows/apps/br240639) | Sérialisation JSON (JavaScript objet notation des points) est un format populaire pour communiquer avec les API REST. Le [Newtonsoft Json.NET](https://www.newtonsoft.com/json), qui est entièrement pris en charge par les applications UWP. |

## <a name="devices"></a>Appareils

Afin d’interagir avec des outils métier comme des imprimantes, des scanneurs de codes-barres ou des lecteurs de cartes à puce, vous jugerez peut-être nécessaire d’intégrer des appareils ou des capteurs externes à votre application. Voici quelques exemples des fonctionnalités que vous pouvez ajouter à votre application à l’aide de la technologie décrite dans cette section.

| Rubrique  | Description |
|--------|-------------|
| [Énumérer les appareils](https://msdn.microsoft.com/library/windows/apps/mt187355) | Cet article décrit comment utiliser l’espace de noms [Windows.Devices.Enumeration](https://msdn.microsoft.com/library/windows/apps/br225459) pour rechercher des appareils connectés au système, en interne, en externe ou détectables sur les protocoles sans fil ou réseau. Commencez ici si vous créez une application qui fonctionne avec des appareils. |
| [Impression et numérisation](https://msdn.microsoft.com/library/windows/apps/mt204544) | Décrit comment imprimer et numériser à partir de votre application, y compris la connexion aux et fonctionne avec des appareils d’entreprise comme les systèmes de point de vente (PDV), les imprimantes et scanneurs à chargeur haute capacité. |
| [Bluetooth](https://msdn.microsoft.com/library/windows/apps/mt270288) | Outre l’utilisation des connexions Bluetooth traditionnelles pour envoyer et recevoir des données ou contrôler les appareils, Windows 10 permet d’utiliser la technologie Bluetooth Low Energy (BTLE) pour envoyer ou recevoir des balises en arrière-plan. Utilisez-la pour afficher des notifications ou activer des fonctionnalités quand un utilisateur s’approche d’un emplacement particulier ou le quitte. |
| [Stockage partagé d’entreprise](enterprise-shared-storage.md) | Dans les scénarios où l’appareil est verrouillé, découvrez comment partager les données au sein de la même application, entre les instances d’une application ou entre les applications. |

## <a name="device-targeting"></a>Ciblage des appareils

Aujourd’hui, de nombreux utilisateurs travaillent avec leur propre téléphone ou tablette, des appareils qui présentent des facteurs de forme et des tailles d’écran différents. Avec la plateforme Windows universelle (UWP), vous pouvez écrire une application métier unique qui s’exécute en toute transparence sur tous les types d’appareils, y compris les ordinateurs de bureau et quel que soit le nombre de PPP de l’écran, ce qui vous permet d’optimiser la portée de votre application ainsi que l’efficacité de votre code.

| Rubrique | Description |
|-------|-------------|
| [Guide des applications UWP](https://msdn.microsoft.com/library/windows/apps/dn894631) | Dans ce guide introductif, vous allez vous familiariser avec la plateforme UWP Windows 10. Vous découvrirez, entre autres, ce qu’est une famille d’appareils et comment déterminer celle à cibler, quels sont les nouveaux volets et contrôles d’interface utilisateur permettant d’adapter votre interface utilisateur à différents facteurs de forme d’appareil, et comment utiliser et contrôler la surface d’API disponible dans votre application. |
| [Exemple de code d’interface utilisateur XAML adaptative](https://go.microsoft.com/fwlink/p/?LinkId=619992) | Cet exemple de code montre toutes les options de disposition possibles et les contrôles de votre application, quel que soit le type d’appareil et vous permet d’interagir avec les volets pour découvrir comment réaliser les dispositions que vous recherchez. En plus de vous présenter la façon dont chaque contrôle répond à différents facteurs de forme, l’application réagit et indique les différentes méthodes permettant d’obtenir une interface utilisateur adaptative. |
| [Rubrique Xamarin](/xamarin/) | Xamarin pour le ciblage téléphonique |

## <a name="deployment"></a>Déploiement

Vous disposez d’options pour la distribution des applications aux utilisateurs de votre organisation. Vous pouvez utiliser Microsoft Store pour entreprises, la gestion des périphériques mobiles existant ou vous pouvez charger des applications sur des appareils. Vous pouvez également rendre vos applications disponibles au grand public en les publiant sur le Microsoft Store.

| Rubrique | Description |
|-------|-------------|
| [Distribuer des applications métier aux entreprises](https://msdn.microsoft.com/library/windows/apps/mt608995) | Vous pouvez publier des applications cœur de métier directement aux entreprises pour une acquisition en volume par le biais du Microsoft Store pour entreprises, sans mettre les applications à grande échelle à la disposition du public. |
| [Charger une version test des applications](https://technet.microsoft.com/library/mt269549) | Lorsque vous chargez de manière indépendante une application, vous déployez un package d’application signée sur un appareil. Vous conservez la signature, l’hébergement et le déploiement de ces applications. Le processus de chargement indépendant d’applications est simplifié pour Windows 10.             |
| [Publier des applications dans le Microsoft Store](https://dev.windows.com/publish) | Le Microsoft Store unifié vous permet de publier et de gérer toutes vos applications pour tous les appareils Windows. Personnalisez la disponibilité de votre application à l’aide d’une tarification par marché, de contrôles de distribution et de visibilité et d’autres options. |

## <a name="enterprise-uwp-samples"></a>Exemples d’entreprise UWP

| Rubrique |  Description |
|------ |--------------|
| [Exemple d’inventaire VanArsdel](https://github.com/Microsoft/InventorySample) | Un exemple d’application UWP qui illustre des scénarios de cœur de métier. L’exemple est basé sur la création et la gestion des clients, des commandes et produits pour la société fictive VanArsdel. |
| [Exemple de base de données de commandes de clients](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | Un exemple d’application UWP qui présente des fonctionnalités utiles aux développeurs d’entreprise, telles que l’authentification Azure Active Directory (AAD), l’interface utilisateur (y compris une grille de données) des contrôles, intégration de base de données Sqlite et SQL Azure, Entity Framework et services de cloud API. L’exemple est basé sur la création et gestion des comptes client, les commandes et les produits pour la société fictive Contoso. |

## <a name="patterns-and-practices"></a>Modèles et pratiques

Les bases de code pour les applications d’entreprise à grande échelle peuvent être difficiles à gérer. Prism est une infrastructure permettant de créer des applications XAML faiblement couplées, faciles à gérer et positionnement dans WPF, UWP Windows 10 et Xamarin Forms. Prism fournit une implémentation d’une collection de modèles de conception utiles pour écrire des applications XAML bien structurées et faciles à gérer, notamment des modèles MVVM, d’injection de dépendance, de commandes, EventAggregator, etc.

Pour plus d’informations sur Prism, voir le [référentiel GitHub](https://github.com/PrismLibrary/Prism).

 

 
