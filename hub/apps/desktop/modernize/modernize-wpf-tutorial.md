---
description: Ce didacticiel montre comment ajouter des interfaces utilisateur UWP XAML, créer des packages MSIX et incorporer d’autres composants modernes dans votre application WPF.
title: 'Tutoriel : Moderniser une application WPF'
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, uwp, WinForms, wpf, xaml (îles)
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 5e7179d4aeb66cad547e31e2456da2e8264ebbcd
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420081"
---
# <a name="tutorial-modernize-a-wpf-app"></a>Tutoriel : Moderniser une application WPF 

Il existe plusieurs façons de [moderniser](index.md) des applications de bureau existantes en intégrant des fonctionnalités de Windows les plus récentes dans existant de source de code au lieu de réécrire les applications à partir de zéro. Dans ce didacticiel, nous allons Explorer plusieurs façons de moderniser une application WPF line of business à l’aide de ces fonctionnalités :

* .NET Core 3
* Contrôles UWP XAML avec XAML (îles)
* Des cartes adaptatives et Windows 10 notifications
* Déploiement de MSIX

Ce didacticiel requiert les compétences de développement suivantes :

* Expérience de développement d’applications de bureau Windows avec WPF.
* Une connaissance élémentaire des C# et XAML.
* Connaissances de base sur UWP.

## <a name="overview"></a>Vue d'ensemble

Ce didacticiel fournit le code pour une application de line-of-business WPF simple nommée dépenses de Contoso. Dans le scénario fictif de ce didacticiel, les frais de Contoso est une application interne utilisée par les gestionnaires de Contoso Corporation pour suivre les dépenses soumis par leurs rapports. Les gestionnaires sont désormais équipés d’appareils tactiles, et il souhaite utiliser l’application de dépenses de Contoso sans la souris ou du clavier. Malheureusement, la version actuelle de l’application n’est pas tactile convivial.

Contoso souhaite moderniser cette application avec les nouvelles fonctionnalités de Windows pour permettre aux employés créer des rapports de dépenses plus efficacement. La plupart des fonctionnalités pourrait être facilement implémentées en créant une nouvelle application UWP. Toutefois, l’application existante est complexe et est le résultat de nombreuses années de développement par différentes équipes. Par conséquent, la réécrire de zéro avec une nouvelle technologie n’est pas une option. L’équipe recherche la meilleure approche pour ajouter de nouvelles fonctionnalités dans la base de code existante.

Au début du didacticiel, les dépenses de Contoso cible le .NET Framework 4.7.2 et utilise les bibliothèques de tiers 3e suivantes :

* MVVM Light, il s’agit d’une implémentation de base pour le modèle MVVM.
* Unity, un conteneur d’injection de dépendance.
* LiteDb, il s’agit d’une solution NoSQL incorporée pour stocker les données.
* Erroné, un outil pour générer des données fictives.

Dans le didacticiel, vous allez améliorer des dépenses de Contoso avec de nouvelles fonctionnalités de Windows :

* Migrer une application WPF existante vers .NET Core 3.0. Vous ouvrirez ainsi les scénarios et les points importants à l’avenir.
* Utilisez les îles de XAML à l’hôte le **InkCanvas** et **MapControl** encapsulé les contrôles fournis par le Kit de ressources de communauté de Windows.
* Utilisez les îles de XAML pour héberger n’importe quel contrôle UWP XAML standard (dans ce cas, un **CalendardView**).
* Intégrez des notifications de cartes adaptatives et Windows 10 dans l’application.
* Package de l’application avec MSIX et configurer un CI/CD pipeline sur Azure DevOps afin que les nouvelles versions de l’application peut fournir automatiquement aux testeurs et aux utilisateurs dès qu’elles sont disponibles.

## <a name="prerequisites"></a>Prérequis

Pour effectuer ce didacticiel, votre ordinateur de développement doit avoir ces composants requis installés :

* Windows 10, version 1903 (build 18362) ou une version ultérieure.
* [Visual Studio 2019](https://www.visualstudio.com).
* [Kit SDK de .NET core 3 Preview](https://dotnet.microsoft.com/download/dotnet-core/3.0) (installer la dernière version disponible en version préliminaire).

Veillez à qu'installer les charges de travail suivantes et les fonctionnalités facultatives avec Visual Studio 2019 :

* Développement .NET desktop
* Développement pour la plateforme Windows universelle
* SDK Windows 10 (10.0.18362.0 ou version ultérieure)

## <a name="get-the-contoso-expenses-sample-app"></a>Obtenir l’exemple d’application Contoso dépenses

Avant de commencer le didacticiel, téléchargez le code source pour l’application de dépenses de Contoso et assurez-vous que vous pouvez générer le code dans Visual Studio.

1. Télécharger le code source d’application à partir de la **versions** onglet de la [référentiel d’atelier AppConsult WinAppsModernization](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop). Le lien direct est [ https://aka.ms/wamwc ](https://aka.ms/wamwc).
2. Ouvrez le fichier zip et extraire tout le contenu à la racine de votre **C:\\**  lecteur. Il crée un dossier nommé **C:\WinAppsModernizationWorkshop**.
3. Ouvrez Visual Studio 2019 et double-cliquez sur le **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln** fichier pour ouvrir la solution.
4. Vérifiez que vous pouvez créer, exécuter et déboguer le projet de Contoso dépenses WPF en appuyant sur la **Démarrer** bouton ou CTRL + F5.

## <a name="get-started"></a>Prise en main

Une fois que vous avez le code source pour l’exemple d’application Contoso dépenses, et vous pouvez confirmer que vous pouvez le créer dans Visual Studio, vous êtes prêt à démarrer le didacticiel :

* [Partie 1 : Migrer le Contoso application de notes de frais vers .NET Core 3](modernize-wpf-tutorial-1.md)
* [Partie 2 : Ajouter un contrôle UWP InkCanvas à l’aide de XAML (îles)](modernize-wpf-tutorial-2.md)
* [Partie 3 : Ajouter un contrôle UWP CalendarView à l’aide de XAML (îles)](modernize-wpf-tutorial-3.md)
* [Partie 4 : Ajouter des notifications et des activités des utilisateurs Windows 10](modernize-wpf-tutorial-4.md)
* [Partie 5 : Empaqueter et déployer avec MSIX](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>Concepts importants

Les sections suivantes fournissent un arrière-plan pour certains des concepts clés abordés dans ce didacticiel. Si vous êtes déjà familiarisé avec ces concepts, vous pouvez ignorer cette section.

### <a name="universal-windows-platform-uwp"></a>Plateforme Windows universelle (UWP)

Dans Windows 8, Microsoft a introduit une nouvelle infrastructure appelée Windows Runtime (WinRT). Contrairement au .NET Framework, WinRT est une couche native d’API qui sont exposés directement aux applications. WinRT a également introduit des projections de langage, qui sont ajoutés au-dessus du runtime pour permettre aux développeurs d’interagir avec lui à l’aide des langages tels que des couches C# et JavaScript en plus de C++. Projections permettent aux développeurs de créer des applications qui tirent parti de la même par-dessus WinRT C# XAML connaissances et leur relation dans la création d’applications avec le .NET Framework. 

Dans Windows 10, Microsoft a introduit le [Universal Windows Platform (UWP)](/windows/uwp/get-started/universal-application-platform-guide), ce qui se trouve au sommet WinRT. La fonctionnalité la plus importante de UWP est qu’elle offre un ensemble commun d’API sur chaque plateforme d’appareil : non que si l’application s’exécute sur un ordinateur de bureau, sur une Xbox One ou sur un HoloLens, vous êtes en mesure d’utiliser les mêmes API.

Fonctionnalités à l’avenir, la plupart des nouveaux Windows 10 sont exposées via APIs WinRT, y compris des fonctionnalités telles que la chronologie, Project Rome et Windows Hello.

### <a name="msix-packaging"></a>Empaquetage de MSIX

[MSIX](http://aka.ms/msix) (anciennement appelé AppX) est le modèle de packaging moderne pour les applications Windows. MSIX prend en charge les applications UWP, ainsi que des applications de bureau génération à l’aide de technologies telles que Win32, WPF, Windows Forms, Java, d’électrons et bien plus encore. Lorsque vous empaquetez une application de bureau dans un package MSIX, vous pouvez publier votre application pour le Microsoft Store. Votre application de bureau obtenir également l’identité du package lorsqu’il est installé, ce qui permet à votre application de bureau utiliser un ensemble plus large de WinRT APIs.

Pour plus d’informations, consultez les articles suivants :

* [Applications de bureau de package](/windows/uwp/porting/desktop-to-uwp-root)
* [Dans les coulisses de votre application de bureau empaquetée](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>XAML (îles)

À compter de Windows 10, version 1903, vous pouvez héberger des contrôles UWP dans les applications de bureau non UWP à l’aide d’une fonctionnalité appelée *XAML îles*. Cette fonctionnalité vous permet d’améliorer l’apparence et les fonctionnalités de vos applications de bureau existantes avec les dernières fonctionnalités de l’interface utilisateur de Windows 10 qui sont uniquement disponibles via les contrôles UWP. Cela signifie que vous pouvez utiliser les fonctionnalités UWP telles que Windows Ink et les contrôles qui prennent en charge le système de conception Fluent dans votre existant WPF, Windows Forms, et C++ applications Win32.

Pour plus d’informations, consultez [dans les applications de bureau (îles XAML), les contrôles UWP](/windows/uwp/xaml-platform/xaml-host-controls). Ce didacticiel vous guide tout au long du processus d’utilisation de deux types différents de contrôles de l’île de XAML :

* Le [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) et [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) dans le Kit de ressources de communauté de Windows. Ces contrôles WPF encapsulent l’interface et les fonctionnalités des contrôles UWP correspondantes et peuvent être utilisés comme tout autre contrôle WPF dans le concepteur Visual Studio.

* La plateforme Windows universelle [affichage Calendrier](/windows/uwp/design/controls-and-patterns/calendar-view) contrôle. Il s’agit d’un contrôle UWP standard que vous hébergerez à l’aide de la [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) contrôle dans la boîte à outils de la Communauté Windows.

### <a name="net-core-3"></a>.NET Core 3

[.NET core](https://docs.microsoft.com/dotnet/core/) est une infrastructure open source qui implémente une version multiplateforme, léger et facilement extensible du .NET Framework. Par rapport à .NET Framework complet, les temps de démarrage de .NET Core sont beaucoup plus rapide et de nombreuses API ont été optimisées.

Via sa première plusieurs versions, le focus de .NET Core a été pour prendre en charge des applications web ou serveur principal. Avec .NET Core, vous pouvez facilement créer des applications web évolutives ou API qui peuvent être hébergés sur Windows, Linux, ou dans des architectures de micro-service comme conteneurs Docker.

.NET Core 3 est la nouvelle version majeure de .NET Core. La grande nouveauté de cette prochaine version est la prise en charge des applications de bureau Windows, notamment les applications Windows Forms et WPF. Vous pouvez exécuter des applications de bureau Windows nouvelles et existantes sur .NET Core 3 et apprécier tous les avantages que .NET Core a à offrir. De plus, vous pourrez utiliser les contrôles UWP hébergés dans [XAML Islands](xaml-islands.md) dans toutes les applications Windows Forms et WPF qui ciblent .NET Core 3.

> [!NOTE]
> WPF et Windows Forms ne sont pas devenir inter-plateformes, et vous ne pouvez pas exécuter un WPF ou les Windows Forms sur Linux et MacOS. Les composants de l’interface utilisateur de WPF et Windows Forms ont toujours une dépendance sur le système d’affichage Windows.

Pour plus d’informations, consultez les articles suivants :

* [Annonce de .NET Core 3.0 Preview 1](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-1-and-open-sourcing-windows-desktop-frameworks/)
* [Annonce de .NET Core 3.0 Preview 2](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-2/)
* [Annonce de .NET Core 3.0 Preview 2](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-3/)
* [Annonce de .NET Core 3.0 Preview 4](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-4/)
* [Nouveautés de .NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).