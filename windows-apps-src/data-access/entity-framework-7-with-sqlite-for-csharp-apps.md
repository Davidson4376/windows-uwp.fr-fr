---
author: mcleblanc
ms.assetid: BC7E8130-A28A-443C-8D7E-353E7DA33AE3
description: "Entity Framework (EF) est un mappeur objet relationnel qui vous permet de travailler avec les données relationnelles en utilisant des objets propres au domaine."
title: "Entity Framework 7 avec SQLite pour applications C#"
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, SQLite, C#, EF, entity framework"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 2ab2a12f6c2bc2f0f8853b404afaf13bf80635b7
ms.lasthandoff: 02/07/2017

---

# <a name="entity-framework-core-1-with-sqlite-for-c-apps"></a>Entity Framework Core 1 avec SQLite pour applications C#

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Entity Framework (EF) est un mappeur relationel objet qui vous permet de travailler avec les données relationnelles en utilisant des objets spécifiques au domaine. Cet article explique comment utiliser Entity Framework Core 1 avec une base de données SQLite dans une application Windows universelle.

Conçu à l’origine pour les développeurs .NET, Entity Framework Core 1 peut être utilisé avec SQLite sur une plateforme Windows universelle (UWP) pour stocker et manipuler des données relationnelles à l’aide d’objets spécifiques au domaine. Vous pouvez migrer le code EF d’une application .NET vers une application UWP. Cela fonctionne une fois les modifications appropriées effectuées dans la chaîne de connexion.

Pour le moment, EF prend uniquement en charge SQLite sur UWP. Vous trouverez une procédure détaillée pour l’installation d’Entity Framework Core 1 et la création de modèles sur la page [Prise en main de la plateforme Windows universelle (en anglais)](http://go.microsoft.com/fwlink/p/?LinkId=735013). Vous y trouverez les rubriques suivantes :

-   Prerequisites (Prérequis)
-   Create a new project (Créer un projet)
-   Install Entity Framework (Installer Entity Framework)
-   Create your model (Créer votre modèle)
-   Create your database (Créer votre base de données)
-   Use your model (Utiliser votre modèle)

