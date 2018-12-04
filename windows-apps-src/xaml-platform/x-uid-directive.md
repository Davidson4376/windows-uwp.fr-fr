---
description: Fournit un identificateur unique à des éléments de balisage. Pour le code XAML de plateforme Windows universelle (UWP), cet identificateur unique est utilisé par des processus ou outils de localisation XAML, comme l’utilisation des ressources d’un fichier de ressources .resw.
title: Directive xUid
ms.assetid: 9FD6B62E-D345-44C6-B739-17ED1A187D69
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 03cf647fdb243fd18212ca894f7682e913378907
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8478384"
---
# <a name="xuid-directive"></a>Directive x:Uid


Fournit un identificateur unique à des éléments de balisage. Pour le code XAML de plateforme Windows universelle (UWP), cet identificateur unique est utilisé par des processus ou outils de localisation XAML, comme l’utilisation des ressources d’un fichier de ressources .resw.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object x:Uid="stringID".../>
```

## <a name="xaml-values"></a>Valeurs XAML

| Terme | Description |
|------|-------------|
| stringID | Chaîne qui identifie de manière unique un élément XAML dans une application et qui fait partie du chemin d’accès aux ressources dans un fichier de ressources. Voir Notes.| 

## <a name="remarks"></a>Remarques

Utilisez **x:Uid** pour identifier un élément objet dans votre code XAML. Cet élément objet est généralement une instance d’une classe de contrôle ou d’un autre élément affiché dans une interface utilisateur. La relation entre la chaîne utilisée dans **x:Uid** et les chaînes utilisées dans un fichier de ressources est que les chaînes du fichier de ressources sont la directive **x:Uid** suivie d’un point (.),puis du nom d’une propriété spécifique de l’élément en cours de localisation. Examinez cet exemple:

``` syntax
<Button x:Uid="GoButton" Content="Go"/>
```

Pour spécifier le contenu devant remplacer le texte d’affichage **Go**, vous devez spécifier une nouvelle ressource provenant d’un fichier de ressources. Votre fichier de ressources doit contenir une entrée pour la ressource nommée «GoButton.Content». Dans le cas présent, [**Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content) est une propriété spécifique héritée par la classe [**Button**](/uwp/api/windows.ui.xaml.controls.button). Vous pouvez également fournir des valeurs localisées pour d’autres propriétés de ce bouton; par exemple, vous pouvez fournir une valeur basée sur les ressources pour «GoButton.FlowDirection». Pour plus d’informations sur l'utilisation simultanée de **x:Uid** et de fichiers de ressources, voir [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](../app-resources/localize-strings-ui-manifest.md).

La validité des chaînes pouvant servir de valeur **x:Uid** est contrôlée dans un sens pratique par les chaînes légitimes en tant qu’identificateur dans un fichier de ressources et un chemin d’accès aux ressources.

**x:Uid** est discret à partir de **x:Name** à la fois en raison du scénario de localisation XAML indiqué et afin que les identificateurs utilisés pour la localisation n’aient aucune dépendance vis-à-vis des implications du modèle de programmation de **x:Name**. En outre, **x:Name** est régi par le concept de namescope XAML, tandis que l’unicité de **x:Uid** est contrôlée par le système d’index de ressource de package (IRP). Pour plus d’informations, voir [Système de gestion des ressources](../app-resources/resource-management-system.md).

Le langage XAML UWP comporte des règles légèrement différentes pour l’unicité de **x:Uid** par rapport aux technologies XAML précédemment utilisées. En langage XAML UWP, il est légitime que la même valeur d’ID **x:Uid** existe en tant que directive sur plusieurs éléments XAML. Toutefois, chacun de ces éléments doit alors partager la même logique de résolution pour la résolution des ressources dans un fichier de ressources. En outre, tous les fichiers XAML d’un projet partagent une seule étendue de ressources à des fins de résolution de **x:Uid** ; il n’existe pas de concept d’étendues **x:Uid** alignées sur des fichiers XAML individuels.

Dans certains cas, vous utiliserez un chemin d’accès aux ressources plutôt que la fonctionnalité intégrée du système d’index de ressource de package (IRP). Toute chaîne utilisée en tant que valeur **x:Uid** définit un chemin d’accès aux ressources qui commence par ms-resource:///Resources/ et qui inclut la chaîne **x:Uid**. Le chemin d’accès est complété par les noms des propriétés que vous spécifiez dans un fichier de ressources ou que vous ciblez.

Ne placez pas **x:Uid** sur des éléments propriétés ; cette opération n’est pas autorisée dans le langage XAML Windows Runtime.

