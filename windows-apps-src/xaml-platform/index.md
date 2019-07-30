---
ms.assetid: b632a6cc-3503-4ab8-bfd1-dde731bd89ab
description: Cette section inclut des rubriques qui décrivent le framework XAML des applications UWP.
title: Plateforme XAML
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 84427f0cec6aa0921f903fff9b5e74b4da5a58a9
ms.sourcegitcommit: f8c354def02d5c82d195e4f629e6470110268223
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68623386"
---
# <a name="xaml-platform"></a>Plateforme XAML

Cette section comprend des rubriques qui décrivent les concepts de programmation généralement utilisables dans les applications que vous écrivez en utilisant C#, Visual Basic ou des extensions de composant Visual C++ (C++/CX) et XAML pour la définition de votre interface utilisateur. Les concepts de programmation comprennent comment utiliser des propriétés et des événements, et comment les appliquer à la programmation d’applications de plateforme Windows universelle (UWP). La plateforme Windows universelle étend les concepts des propriétés C#, Visual Basic et C++/CX et leurs valeurs en ajoutant le système de propriétés de dépendance. Les rubriques de cette section documentent également le langage XAML tel qu’utilisé par UWP, ainsi que des scénarios simples et avancés sur l’utilisation du code XAML pour définir l’interface utilisateur de votre application UWP.

| Rubrique | Description |
|-------|-------------|
| [Vue d’ensemble du langage XAML](xaml-overview.md) | Introduit le langage XAML et ses concepts aux développeurs d’applications Windows Runtime, puis décrit les différentes manières de déclarer des objets et de définir des attributs en langage XAML tel qu’il est utilisé pour créer une application Windows Runtime. |
| [Vue d’ensemble des propriétés de dépendance](dependency-properties-overview.md) | Explique le système de propriétés de dépendance disponible quand vous écrivez une application Windows Runtime en C++, C# ou Visual Basic avec des définitions XAML pour l’interface utilisateur. |
| [Propriétés de dépendance personnalisées](custom-dependency-properties.md) | Explique comment définir et implémenter des propriétés de dépendance personnalisées pour une application Windows Runtime en C++, C# ou Visual Basic. |
| [Vue d’ensemble des propriétés jointes](attached-properties-overview.md) | Explique le concept d’une propriété jointe en XAML et fournit quelques exemples. |
| [Propriétés jointes personnalisées](custom-attached-properties.md) | Explique comment implémenter une propriété jointe XAML en tant que propriété de dépendance et comment définir la convention d’accesseur nécessaire pour que votre propriété jointe soit utilisable en XAML. |
| [Vue d’ensemble des événements et des événements routés](events-and-routed-events-overview.md) | Décrit le concept de programmation des événements dans une application Windows Runtime, quand vous utilisez C#, Visual Basic ou les extensions de composants Visual C++ (C++/CX) comme langage de programmation et le langage XAML pour la définition de votre interface utilisateur. Vous pouvez assigner des gestionnaires pour les événements dans le cadre des déclarations des éléments d’interface utilisateur en XAML, ou vous pouvez ajouter les gestionnaires dans le code. Windows Runtime prend en charge les *événements routés* : certains événements d’entrée et événements de données peuvent être gérés par des objets autres que l’objet ayant déclenché l’événement. Les événements routés s’avèrent utiles quand vous définissez des modèles de contrôles ou utilisez des pages ou conteneurs de disposition. |
|[Contrôles UWP dans les applications de bureau (XAML Islands)](/windows/apps/desktop/modernize/xaml-islands)| Explique comment utiliser les contrôles XAML UWP pour améliorer l’interface utilisateur d’une application de bureau Windows Forms, WPF ou Win32.|
