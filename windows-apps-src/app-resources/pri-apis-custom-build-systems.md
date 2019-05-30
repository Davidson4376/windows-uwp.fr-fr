---
Description: La ressource de package (PRI) API d’indexation, vous pouvez développer un système de génération personnalisée pour les ressources de votre application UWP. Le système de génération pourra créer, versionner, et vider les fichiers d'index de ressource de package (IRP) au niveau de complexité dont votre application UWP a besoin.
title: API d’indexation de ressources de package (IRP) et systèmes de génération personnalisés
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: b300630ff4a9b6735cb7891444da4333532735c1
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359376"
---
# <a name="package-resource-indexing-pri-apis-and-custom-build-systems"></a>API d’indexation de ressources de package (IRP) et systèmes de génération personnalisés
Grâce aux [API d’indexation de ressource de package (IRP)](https://docs.microsoft.com/windows/desktop/menurc/pri-indexing-reference), vous pouvez développer un système de génération personnalisé pour les ressources de votre application UWP. Le système de génération pourra créer, versionner, et vider (en tant que XML) les fichiers d’index de ressource de package (IRP) au niveau de complexité dont votre application UWP a besoin. Si vous avez un système de génération personnalisé qui utilise l’outil de ligne de commande MakePri.exe (consultez [Compiler des ressources manuellement avec MakePri.exe](makepri-exe-command-options.md)), pour améliorer les performances et le contrôle, nous vous recommandons d’appeler les API IRP au lieu d’appeler MakePri.exe.

Les API IRP ont été introduites dans le SDK Windows pour Windows 10, version 1803. Les API prennent la forme d’API Win32 Windows, ce qui signifie que vous disposez de plusieurs options pour les appeler. Vous pouvez les appeler directement à partir d’une application Win32, ou via [appel de code non managé](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live) à partir d’une application .NET ou même à partir d’une application UWP.

Les scénarios de cette rubrique montrent l’appel d’API IRP à partir d’un projet d’application console Windows Win32 Visual C++. Pour plus d’informations contextuelles, consultez [Système de gestion des ressources](resource-management-system.md).

La limite de taille d’un fichier PRI est 64 kilo-octets.

> [!NOTE]
> Cet inconvénient est peu susceptible d’être un problème, dans la mesure où vous ne souhaiterez probablement pas soumettre votre application de système de génération personnalisée à Microsoft Store. Cependant, si vous choisissez l’option de développement de votre système de génération personnalisé sous la forme d’une application UWP, il s’agira d’une application UWP inhabituelle, dans la mesure où vous ne pourrez pas la soumettre dans le Microsoft Store. La raison en est qu’une application UWP qui utilise des appels de code non managé échoue à la certification de Microsoft Store. Notez que, dans ce cas, des appels de code non managé *existent uniquement dans votre système de génération personnalisé* ; *pas* dans votre application UWP d’expédition (celle pour laquelle vous créez des fichiers IRP).

## <a name="scenario-walkthroughs"></a>Procédures pas à pas des scénarios
|Rubrique|Description|
|-|-|
|[Scénario 1 : Générer un fichier PRI à partir des ressources de chaîne et les fichiers de ressources](pri-apis-scenario-1.md)|Dans ce scénario, nous allons créer une application pour représenter notre système de génération personnalisé. Nous allons créer un indexeur de ressources et lui ajouter des chaînes et autres types de ressources. Nous allons ensuite générer et un fichier IRP de vidage.|

## <a name="important-apis"></a>API importantes
* [Ressource de package que l’indexation de référence de (PRI)](https://docs.microsoft.com/windows/desktop/menurc/pri-indexing-reference)

## <a name="related-topics"></a>Rubriques connexes
* [Compiler des ressources manuellement avec MakePri.exe](makepri-exe-command-options.md)
* [Consommation de fonctions DLL non managées](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)
* [Système de gestion des ressources](resource-management-system.md)
