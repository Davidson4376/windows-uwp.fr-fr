---
description: Ce didacticiel montre comment ajouter des interfaces utilisateur en XAML UWP, créer des packages MSIX et intégrer d’autres composants modernes à votre application WPF.
title: 'Tutoriel : Moderniser une application WPF'
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, îlots XAML
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 3e78bd81531750ac419c5ca3628e72a3e8505038
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317084"
---
# <a name="tutorial-modernize-a-wpf-app"></a>Tutoriel : Moderniser une application WPF 

Il existe de nombreuses façons de [moderniser](index.md) les applications de bureau existantes en intégrant les fonctionnalités Windows les plus récentes dans le code source existant au lieu de réécrire les applications à partir de zéro. Dans ce didacticiel, nous allons explorer plusieurs manières de moderniser une application métier WPF existante à l’aide des fonctionnalités suivantes :

* .NET Core 3
* Contrôles XAML UWP avec des îlots XAML
* Cartes adaptatives et notifications Windows 10
* Déploiement de MSIX

Ce didacticiel nécessite les compétences de développement suivantes :

* Expérience dans le développement d’applications de bureau Windows avec WPF.
* Connaissance de base C# de et XAML.
* Connaissances de base de UWP.

## <a name="overview"></a>Vue d'ensemble

Ce didacticiel fournit le code pour une application métier WPF simple nommée dépenses contoso. Dans le scénario fictif du didacticiel, contoso dépenses est une application interne utilisée par les responsables de Contoso Corporation pour suivre les dépenses soumises par leurs rapports. Les responsables sont désormais équipés d’appareils tactiles et ils souhaitent utiliser l’application Contoso depenses sans clavier ni souris. Malheureusement, la version actuelle de l’application n’est pas conviviale.

Contoso souhaite moderniser cette application avec de nouvelles fonctionnalités Windows pour permettre aux employés de créer des rapports de dépenses plus efficacement. La plupart des fonctionnalités peuvent être facilement implémentées en générant une nouvelle application UWP. Toutefois, l’application existante est complexe et est le résultat de nombreuses années de développement par différentes équipes. Ainsi, la réécriture à partir de zéro avec une nouvelle technologie n’est pas une option. L’équipe recherche la meilleure approche pour ajouter de nouvelles fonctionnalités à la base de code existante.

Au début du didacticiel, contoso depenses targets the .NET Framework 4.7.2 et utilise les bibliothèques tierces suivantes :

* MVVM Light, implémentation de base pour le modèle MVVM.
* Unity, un conteneur d’injection de dépendances.
* LiteDb, une solution NoSQL incorporée pour stocker les données.
* Un outil pour générer des données factices.

Dans ce didacticiel, vous allez améliorer les dépenses de contoso avec les nouvelles fonctionnalités de Windows :

* Migrer une application WPF existante vers .NET Core 3,0. Cela entraînera l’ouverture de scénarios nouveaux et importants à l’avenir.
* Utilisez des îlots XAML pour héberger les contrôles inclus **InkCanvas** et **collection MapControl** fournis par le kit de commandes de la communauté Windows.
* Utilisez des îlots XAML pour héberger n’importe quel contrôle XAML UWP standard (dans ce cas, un **CalendardView**).
* Intégrer des cartes adaptatives et des notifications Windows 10 dans l’application.
* Empaquetez l’application avec MSIX et configurez un pipeline CI/CD sur Azure DevOps afin de pouvoir distribuer automatiquement de nouvelles versions de l’application aux testeurs et aux utilisateurs dès qu’elle est disponible.

## <a name="prerequisites"></a>Prérequis

Pour effectuer ce didacticiel, vous devez installer les composants requis suivants sur votre ordinateur de développement :

* Windows 10, version 1903 (Build 18362) ou version ultérieure.
* [Visual Studio 2019](https://www.visualstudio.com).
* [SDK .net Core 3](https://dotnet.microsoft.com/download/dotnet-core/3.0) (installez la dernière version).

Veillez à installer les charges de travail suivantes et les fonctionnalités facultatives avec Visual Studio 2019 :

* Développement .NET Desktop
* Développement pour la plateforme Windows universelle
* SDK Windows 10 (10.0.18362.0 ou version ultérieure)

## <a name="get-the-contoso-expenses-sample-app"></a>Télécharger l’exemple d’application de dépenses contoso

Avant de commencer le didacticiel, téléchargez le code source de l’application Contoso depenses et assurez-vous que vous pouvez générer le code dans Visual Studio.

1. Téléchargez le code source de l’application à partir de l’onglet **releases** du [référentiel AppConsult WinAppsModernization Workshop](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop). Le lien direct est [https://aka.ms/wamwc](https://aka.ms/wamwc).
2. Ouvrez le fichier zip et extrayez tout le contenu à la racine de votre lecteur **C :\\**  . Il crée un dossier nommé **C:\WinAppsModernizationWorkshop**.
3. Ouvrez Visual Studio 2019 et double-cliquez sur le fichier **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln** pour ouvrir la solution.
4. Vérifiez que vous pouvez générer, exécuter et déboguer le projet WPF dépenses contoso en appuyant sur le bouton **Démarrer** ou en appuyant sur CTRL + F5.

## <a name="get-started"></a>Prise en main

Une fois que vous disposez du code source de l’exemple d’application dépenses contoso et que vous pouvez le générer dans Visual Studio, vous êtes prêt à commencer le didacticiel :

* [Partie 1 : Migrer l’application Contoso depenses vers .NET Core 3](modernize-wpf-tutorial-1.md)
* [Partie 2 : Ajouter un contrôle de l’InkCanvas UWP à l’aide des îlots XAML](modernize-wpf-tutorial-2.md)
* [Partie 3 : Ajouter un contrôle UWP CalendarView à l’aide des îlots XAML](modernize-wpf-tutorial-3.md)
* [Partie 4 : Ajouter des activités et des notifications utilisateur Windows 10](modernize-wpf-tutorial-4.md)
* [Partie 5 : Empaqueter et déployer avec MSIX](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>Concepts importants

Les sections suivantes fournissent des informations générales sur certains des principaux concepts abordés dans ce didacticiel. Si vous êtes déjà familiarisé avec ces concepts, vous pouvez ignorer cette section.

### <a name="universal-windows-platform-uwp"></a>Plateforme Windows universelle (UWP)

Dans Windows 8, Microsoft a introduit une nouvelle infrastructure appelée Windows Runtime (WinRT). Contrairement à la .NET Framework, WinRT est une couche native d’API qui sont exposées directement aux applications. WinRT a également introduit des projections de langage, qui sont des couches ajoutées par-dessus le runtime pour permettre aux développeurs d’interagir avec C# eux à l’aide de C++langages tels que et JavaScript en plus de. Les projections permettent aux développeurs de créer des applications en plus de WinRT qui tirent parti des mêmes C# connaissances XAML qu’elles ont acquises lors de la création d’applications avec le .NET Framework. 

Dans Windows 10, Microsoft a introduit le [plateforme Windows universelle (UWP)](/windows/uwp/get-started/universal-application-platform-guide), qui s’appuie sur WinRT. La fonctionnalité la plus importante de UWP est qu’elle offre un ensemble commun d’API sur toutes les plateformes d’appareils : peu importe si l’application s’exécute sur un ordinateur de bureau, sur une Xbox ou sur un HoloLens, vous pouvez utiliser les mêmes API.

À l’avenir, la plupart des nouvelles fonctionnalités de Windows 10 sont exposées via des API WinRT, y compris des fonctionnalités telles que Timeline, le projet Rome et Windows Hello.

### <a name="msix-packaging"></a>Empaquetage MSIX

[MSIX](http://aka.ms/msix) (autrefois appelé AppX) est le modèle de Packaging moderne pour les applications Windows. MSIX prend en charge les applications UWP ainsi que les applications de bureau qui utilisent des technologies telles que Win32, WPF, Windows Forms, Java, Electron et bien plus encore. Quand vous empaquetez une application de bureau dans un package MSIX, vous pouvez publier votre application sur le Microsoft Store. Votre application de bureau obtient également l’identité du package lors de son installation, ce qui permet à votre application de bureau d’utiliser un ensemble plus large d’API WinRT.

Pour plus d’informations, consultez les articles suivants:

* [Applications de bureau de package](/windows/uwp/porting/desktop-to-uwp-root)
* [En coulisses de votre application de bureau empaquetée](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>Îlots XAML

À compter de Windows 10, version 1903, vous pouvez héberger des contrôles UWP dans des applications de bureau non UWP à l’aide d’une fonctionnalité appelée *îlots XAML*. Cette fonctionnalité vous permet d’améliorer l’apparence, la convivialité et les fonctionnalités de vos applications de bureau existantes avec les dernières fonctionnalités de l’interface utilisateur de Windows 10 qui sont uniquement disponibles via les contrôles UWP. Cela signifie que vous pouvez utiliser des fonctionnalités UWP telles que Windows Ink et des contrôles qui prennent en charge le système de conception Fluent dans vos applications C++ WPF, Windows Forms et Win32 existantes.

Pour plus d’informations, consultez [contrôles UWP dans les applications de bureau (îlots XAML)](/windows/uwp/xaml-platform/xaml-host-controls). Ce didacticiel vous guide tout au long du processus d’utilisation de deux types différents de contrôles d’îlot XAML :

* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) et [collection MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) dans le kit de pratiques de la communauté Windows. Ces contrôles WPF encapsulent l’interface et les fonctionnalités des contrôles UWP correspondants et peuvent être utilisés comme n’importe quel autre contrôle WPF dans le concepteur Visual Studio.

* Contrôle d' [affichage du calendrier](/windows/uwp/design/controls-and-patterns/calendar-view) UWP. Il s’agit d’un contrôle UWP standard que vous allez héberger à l’aide du contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) de la boîte à outils de la communauté Windows.

### <a name="net-core-3"></a>.NET Core 3

[.Net Core](https://docs.microsoft.com/dotnet/core/) est une infrastructure Open source qui implémente une version multiplateforme, légère et facile à extensible de la .NET Framework complète. Par rapport à la .NET Framework complète, le temps de démarrage de .NET Core est beaucoup plus rapide et la plupart des API ont été optimisées.

Grâce à ses premières versions, le but de .NET Core était de prendre en charge les applications Web ou principales. Avec .NET Core, vous pouvez facilement créer des applications Web évolutives ou des API qui peuvent être hébergées sur Windows, Linux ou dans des architectures de micro-services telles que des conteneurs d’ancrage.

.NET Core 3 est la dernière version de .NET Core. La mise en surbrillance de cette version est la prise en charge des applications de bureau Windows, y compris les Windows Forms et les applications WPF. Vous pouvez exécuter des applications de bureau Windows nouvelles et existantes sur .NET Core 3 et profiter de tous les avantages offerts par .NET Core. De plus, vous pourrez utiliser les contrôles UWP hébergés dans [XAML Islands](xaml-islands.md) dans toutes les applications Windows Forms et WPF qui ciblent .NET Core 3.

> [!NOTE]
> WPF et Windows Forms ne se transforment pas entre plateformes, et vous ne pouvez pas exécuter un WPF ou un Windows Forms sur Linux et MacOS. Les composants de l’interface utilisateur de WPF et Windows Forms ont toujours une dépendance sur le système de rendu Windows.

Pour plus d’informations, consultez [Nouveautés de .net Core 3,0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).