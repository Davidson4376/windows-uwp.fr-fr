---
author: jwmsft
ms.assetid: b632a6cc-3503-4ab8-bfd1-dde731bd89ab
description: Cette section inclut des rubriques qui expliquent l’infrastructure XAML pour les applications de plateforme Windows universelle (UWP).
title: Plateforme XAML
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fe9bad5afabca3ef0b9c782446e581aea61fe4dc
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6031211"
---
# <a name="xaml-platform"></a>Plateforme XAML


Cette section inclut des rubriques qui décrivent des concepts de programmation qui sont généralement applicables aux applications que vous écrivez, si vous utilisez des extensions de composant c#, Microsoft Visual Basic ou Visual c++ (C++ / CX) comme langage de programmation et XAML pour votre interface utilisateur définition. Il s’agit notamment de concepts de programmation de base tels que l’utilisation des propriétés et des événements, et la façon dont ces derniers s’appliquent à la programmation d’applications de plateforme Windows universelle (UWP). La plateforme Windows universelle (UWP) étend les concepts des propriétés C#, Visual Basic et C++/CX et leurs valeurs en ajoutant le système de propriétés de dépendance. Les rubriques de cette section documentent également le langage XAML tel qu’utilisé par UWP, et couvrent des cas de base et des sujets avancés sur l’utilisation du code XAML pour définir l’interface utilisateur de votre application UWP.

| Rubrique | Description |
|-------|-------------|
| [Vue d’ensemble du langage XAML](xaml-overview.md) | Nous présentons le langage XAML et les concepts XAML aux développeurs d’applications Windows Runtime, puis nous décrivons les différentes manières de déclarer des objets et de définir des attributs en langage XAML tel qu’il est utilisé pour créer une application du Windows Store. |
| [Vue d’ensemble des propriétés de dépendance](dependency-properties-overview.md) | Cette rubrique décrit le système de propriétés de dépendance disponible quand vous écrivez une application Windows Runtime en C++, C# ou Visual Basic avec des définitions XAML pour l’interface utilisateur. |
| [Propriétés de dépendance personnalisées](custom-dependency-properties.md) | Explique comment définir et implémenter des propriétés de dépendance personnalisées pour une application Windows Runtime en C++, C# ou Visual Basic. |
| [Vue d’ensemble des propriétés jointes](attached-properties-overview.md) | Explique le concept de propriété jointe en XAML et fournit quelques exemples. |
| [Propriétés jointes personnalisées](custom-attached-properties.md) | Explique comment implémenter une propriété jointe XAML en tant que propriété de dépendance et comment définir la convention d’accesseur nécessaire pour que votre propriété jointe soit utilisable en XAML. |
| [Vue d’ensemble des événements et des événements routés](events-and-routed-events-overview.md) | Nous décrivons le concept de programmation des événements dans une application Windows Runtime, quand vous utilisez C#, Visual Basic ou les extensions de composants Visual C++ (C++/CX) comme langage de programmation et le langage XAML pour la définition de votre interface utilisateur. Vous pouvez assigner des gestionnaires pour les événements dans le cadre des déclarations des éléments d’interface utilisateur en XAML, ou vous pouvez ajouter les gestionnaires dans le code. Windows Runtime prend en charge les **événements routés** : certains événements d’entrée et événements de données peuvent être gérés par des objets autres que l’objet ayant déclenché l’événement. Les événements routés s’avèrent utiles quand vous définissez des modèles de contrôles ou utilisez des pages ou conteneurs de disposition. |
|[Hébergez des contrôles UWP dans les applications WPF et Windows Forms](xaml-host-controls.md)| Explique comment utiliser les contrôles XAML UWP pour améliorer l’interface utilisateur d’une application Windows Forms ou d'une application de bureau WPF.|
