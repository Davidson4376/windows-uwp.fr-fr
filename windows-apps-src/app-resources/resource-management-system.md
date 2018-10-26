---
author: stevewhims
Description: At build time, the Resource Management System creates an index of all the different variants of the resources that are packaged up with your app. At run-time, the system detects the user and machine settings that are in effect and loads the resources that are the best match for those settings.
title: Système de gestion des ressources
template: detail.hbs
ms.author: stwhi
ms.date: 10/20/2017
ms.topic: article
keywords: windows10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: b80eda57ff700d732ba2402582ed6402acca4fc6
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5605058"
---
# <a name="resource-management-system"></a>Système de gestion des ressources
Le système de gestion des ressources inclut des fonctionnalités pour la création et l’exécution. Lors de la génération, le système crée un index de toutes les variantes de ressources qui sont incluses dans le package avec votre application. Cet index est l’index de ressource de package, ou PRI (Package Resource Index) et il est également inclus dans le package de votre application. Lors de l’exécution, le système détecte les paramètres en vigueur pour l’utilisateur et l’ordinateur, consulte les informations dans le fichier PRI, puis charge automatiquement les ressources les plus appropriées pour ces paramètres.

## <a name="package-resource-index-pri-file"></a>Fichier d’index de ressource de package (PRI)
Chaque package d’application doit contenir un index binaire des ressources de l’application. Cet index est créé lors de la génération et il est contenu dans un ou plusieurs fichiers d’index de ressource de package (PRI).

- Un fichier PRI contient des ressources de chaîne réelles et un ensemble indexé de chemins d’accès de fichiers référençant divers fichiers du package.
- Un package contient généralement un fichier PRI unique par langue, nommé resources.pri.
- Le fichier resources.pri situé à la racine de chaque package est automatiquement chargé lorsque le [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) est instancié.
- Les fichiers PRI peuvent être créés et vidés à l’aide de l’outil [MakePRI.exe](compile-resources-manually-with-makepri.md).
- Dans le cadre du développement d’application standard, vous n’aurez pas besoin de MakePRI.exe, car il est déjà intégré dans le flux de compilation de VisualStudio. En outre, VisualStudio prend en charge la modification des fichiers PRI dans une interface utilisateur dédiée. Toutefois, vos traducteurs et les outils qu’ils utilisent peuvent avoir recours à MakePRI.exe.
- Chaque fichier PRI contient une collection nommée de ressources, appelée un mappage de ressources. Lorsqu’un fichier PRI est chargé à partir d’un package, le nom du mappage de ressources est vérifié pour être sûr qu’il correspond au nom de l’identité du package.
- Les fichiers PRI contiennent uniquement des données et ils n’utilisent donc pas le format exécutable portable (PE). Ils sont spécifiquement conçus pour contenir uniquement des données, comme le format de ressources pour Windows. Ils remplacent les ressources contenues dans les DLL dans le modèle d’application Win32.
- La limite de taille d’un fichier PRI est 64kilo-octets.

## <a name="uwp-api-access-to-app-resources"></a>Accès des API UWP aux ressources d’application

### <a name="basic-functionality-resourceloader"></a>Fonctionnalité de base (ResourceLoader)
Le moyen le plus simple pour accéder aux ressources de votre application par programmation consiste à utiliser l’espace de noms [**Windows.ApplicationModel.Resources**](/uwp/api/windows.applicationmodel.resources?branch=live) et la classe [**ResourceLoader**](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live). **ResourceLoader** vous fournit l’accès de base aux ressources de chaîne dans l’ensemble des fichiers de ressources, les bibliothèques référencées ou d’autres packages.

### <a name="advanced-functionality-resourcemanager"></a>Fonctionnalité avancée (ResourceManager)
La classe [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) (dans l’espace de noms [**Windows.ApplicationModel.Resources.Core**](/uwp/api/windows.applicationmodel.resources.core?branch=live)) fournit des informations supplémentaires sur les ressources, notamment pour l’énumération et le contrôle. Cela va au-delà de ce qui est fourni par la classe **ResourceLoader**.

Un objet [**NamedResource**](/uwp/api/windows.applicationmodel.resources.core.namedresource?branch=live) représente une ressource logique individuelle avec plusieurs langues ou d’autres variantes qualifiées. Il décrit la vue logique de l’actif ou de la ressource, avec un identificateur de ressource de chaîne comme `Header1`, ou un nom de fichier de ressources comme `logo.jpg`.

Un objet [**ResourceCandidate**](/uwp/api/windows.applicationmodel.resources.core.resourcecandidate?branch=live) représente une valeur de ressource concrète et ses qualificateurs, telle que la chaîne «HelloWorld» pour l’anglais ou «logo.scale-100.jpg» en tant que ressource d’image qualifiée, spécifique à la résolution **scale-100**.

Les ressources disponibles pour une application sont stockées dans des collections hiérarchiques auxquelles vous pouvez accéder à l’aide d’un objet [**ResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live). La classe **ResourceManager** permet d’accéder aux différentes instances de **ResourceMap** de niveau supérieur utilisées par l’application, correspondant aux différents packages pour l’application. La valeur [**MainResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager.MainResourceMap) correspond au mappage de ressources pour le package d’application en cours, et elle exclut tous les packages d’infrastructure référencés. Chaque objet **ResourceMap** est nommé pour le nom de package qui est spécifié dans le manifeste du package. Un **ResourceMap** comporte des sous-arborescences (voir [**ResourceMap.GetSubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live)) qui contiennent d’autres objets **NamedResource**. En général, les sous-arborescences correspondent aux fichiers de ressources qui contiennent la ressource. Pour plus d’informations, voir [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](localize-strings-ui-manifest.md) et [Charger des images et des ressources adaptées à l’échelle, au thème, au contraste élevé et à d’autres contextes](images-tailored-for-scale-theme-contrast.md).

En voici un exemple.

```csharp
// using Windows.ApplicationModel.Resources.Core;
ResourceMap resourceMap =  ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
ResourceContext resourceContext = ResourceContext.GetForCurrentView()
var str = resourceMap.GetValue("String1", resourceContext).ValueAsString;
```

**Remarque:** l’identificateur de ressource est considéré comme un fragment d’URI (Uniform Resource Identifier), soumis à la sémantique URI. Par exemple, `GetValue("Caption%20")` est traité comme `GetValue("Caption ")`. N’utilisez pas les caractères «?» ou «#» dans les identificateurs de ressource, car ils mettent fin à l’évaluation du chemin d’accès aux ressources. Par exemple, «MyResource?3» est traité comme «MyResource».

Le **ResourceManager** prend en charge l’accès aux ressources de chaîne d’une application, mais il permet également d’énumérer et d’inspecter les diverses ressources de fichier. Afin d’éviter les collisions entre les fichiers et d’autres ressources provenant d’un fichier, les chemins d’accès de fichiers indexés se trouvent tous dans une sous-arborescence «Files» **ResourceMap** réservée. Par exemple, le fichier `\Images\logo.png` correspond au nom de ressource `Files/images/logo.png`.

Les API [**StorageFile**](/uwp/api/Windows.Storage.StorageFile?branch=live) gèrent de manière transparente les références aux fichiers en tant que ressources et elles conviennent pour les scénarios d’utilisation classiques. Le **ResourceManager** doit uniquement être utilisé pour les scénarios avancés, par exemple lorsque vous souhaitez remplacer le contexte actuel.

### <a name="resourcecontext"></a>ResourceContext
Les ressources potentielles sont choisies en fonction d’un [**ResourceContext**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext?branch=live) particulier. Il s’agit d’une collection de valeurs de qualificateur de ressource (langue, échelle, contraste, etc.). Un contexte par défaut utilise la configuration actuelle de l’application pour chaque valeur de qualificateur, sauf en cas de substitution. Par exemple, les ressources telles que les images peuvent être qualifiées pour l’échelle qui varie d’un moniteur à l’autre et, par conséquent, d’un affichage d’application à l’autre. Pour cette raison, chaque affichage d’application a un contexte par défaut spécifique. Le contexte par défaut pour un affichage donné peut être obtenu à l’aide de [**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView). Chaque fois que vous récupérez une ressource potentielle, vous devez passer une instance **ResourceContext** pour obtenir la valeur la plus appropriée pour un affichage donné.

## <a name="important-apis"></a>API importantes
* [ResourceLoader](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live)
* [ResourceManager](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)

## <a name="related-topics"></a>Rubriques associées
* [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](localize-strings-ui-manifest.md)
* [Charger des images et des ressources adaptées à l’échelle, au thème, au contraste élevé et à d’autres contextes](images-tailored-for-scale-theme-contrast.md)
