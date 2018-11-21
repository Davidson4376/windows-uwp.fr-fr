---
author: stevewhims
Description: With the package resource indexing (PRI) APIs, you can develop a custom build system for your UWP app's resources. The build system will be able to create, version, and dump PRI files to whatever level of complexity your UWP app needs.
title: API d’indexation de ressources de package (IRP) et systèmes de génération personnalisés
template: detail.hbs
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
keywords: windows10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 9b85f40fc391df764515d21ba3b334bfe068725c
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7431353"
---
# <a name="package-resource-indexing-pri-apis-and-custom-build-systems"></a>API d’indexation de ressources de package (IRP) et systèmes de génération personnalisés
Grâce aux [API d’indexation de ressource de package (IRP)](https://msdn.microsoft.com/library/windows/desktop/mt845690), vous pouvez développer un système de génération personnalisé pour les ressources de votre application UWP. Le système de génération pourra créer, versionner, et vider (en tant que XML) les fichiers d’index de ressource de package (IRP) au niveau de complexité dont votre application UWP a besoin. Si vous avez un système de génération personnalisé qui utilise l’outil de ligne de commande MakePri.exe (consultez [Compiler des ressources manuellement avec MakePri.exe](makepri-exe-command-options.md)), pour améliorer les performances et le contrôle, nous vous recommandons d’appeler les API IRP au lieu d’appeler MakePri.exe.

Les API IRP ont été introduites dans le SDK Windows pour Windows10, version1803. Les API prennent la forme d’API Win32Windows, ce qui signifie que vous disposez de plusieurs options pour les appeler. Vous pouvez les appeler directement à partir d’une application Win32, ou via [appel de code non managé](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live) à partir d’une application .NET ou même à partir d’une application UWP.

Les scénarios de cette rubrique montrent l’appel d’API IRP à partir d’un projet d’application console WindowsWin32Visual C++. Pour plus d’informations contextuelles, consultez [Système de gestion des ressources](resource-management-system.md).

La limite de taille d’un fichier PRI est 64kilo-octets.

> [!NOTE]
> Cet inconvénient est peu susceptible d’être un problème, dans la mesure où vous ne souhaiterez probablement pas soumettre votre application de système de génération personnalisée à MicrosoftStore. Cependant, si vous choisissez l’option de développement de votre système de génération personnalisé sous la forme d’une application UWP, il s’agira d’une application UWP inhabituelle, dans la mesure où vous ne pourrez pas la soumettre dans le MicrosoftStore. La raison en est qu’une application UWP qui utilise des appels de code non managé échoue à la certification de MicrosoftStore. Notez que, dans ce cas, des appels de code non managé *existent uniquement dans votre système de génération personnalisé*; *pas* dans votre application UWP d’expédition (celle pour laquelle vous créez des fichiers IRP).

## <a name="scenario-walkthroughs"></a>Procédures pas à pas des scénarios
|Rubrique|Description|
|-|-|
|[Scénario 1: Générer un fichier IRP à partir de ressources de chaîne et de fichiers de ressources](pri-apis-scenario-1.md)|Dans ce scénario, nous allons créer une application pour représenter notre système de génération personnalisé. Nous allons créer un indexeur de ressources et lui ajouter des chaînes et autres types de ressources. Nous allons ensuite générer et un fichier IRP de vidage.|

## <a name="important-apis"></a>API importantes
* [Référence d’indexation de ressource de package (IRP)](https://msdn.microsoft.com/library/windows/desktop/mt845690)

## <a name="related-topics"></a>Rubriquesassociées
* [Compiler des ressources manuellement avec MakePri.exe](makepri-exe-command-options.md)
* [Utilisation de fonctions DLL non gérées](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)
* [Système de gestion des ressources](resource-management-system.md)
