---
author: normesta
ms.assetid: BC7E8130-A28A-443C-8D7E-353E7DA33AE3
description: "Entity Framework (EF) est un mappeur objet relationnel qui vous permet de travailler avec les données relationnelles en utilisant des objets propres au domaine."
title: Entity FrameworkCoreavec SQLite pour applicationsC#
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, SQLite, C#, EF, entity framework
ms.openlocfilehash: 9d447b8a197ed7eaea0f991749064912dffb66d7
ms.sourcegitcommit: 378382419f1fda4e4df76ffa9c8cea753d271e6a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2017
---
# <a name="entity-framework-core-with-sqlite-for-c-apps"></a>Entity FrameworkCoreavec SQLite pour applicationsC#

\[ Mise à jour pour les applicationsUWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Entity Framework (EF) est un mappeur relationel objet qui vous permet de travailler avec les données relationnelles en utilisant des objets spécifiques au domaine. Cet article explique comment utiliser Entity Framework Core avec une base de données SQLite dans une application Windows universelle.

Conçu à l’origine pour les développeurs .NET, EntityFrameworkCore peut être utilisé avec SQLite sur une plateforme Windows universelle (UWP) pour stocker et manipuler des données relationnelles à l’aide d’objets spécifiques au domaine. Vous pouvez migrer le code EF d’une application .NET vers une applicationUWP. Cela fonctionne une fois les modifications appropriées effectuées dans la chaîne de connexion.

Pour le moment, EF prend uniquement en charge SQLite sur UWP. Vous trouverez une procédure détaillée pour l’installation d’Entity Framework Core et la création de modèles sur la page [Prise en main de la plateforme Windows universelle (en anglais)](http://go.microsoft.com/fwlink/p/?LinkId=735013). Vous y trouverez les rubriques suivantes:

-   Prerequisites (Prérequis)
-   Create a new project (Créer un projet)
-   Install Entity Framework (Installer Entity Framework)
-   Create your model (Créer votre modèle)
-   Create your database (Créer votre base de données)
-   Use your model (Utiliser votre modèle)
