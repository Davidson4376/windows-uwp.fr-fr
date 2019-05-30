---
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: Accès aux données
description: Cette section présente le stockage des données sur l’appareil dans une base de données privée et l’utilisation du mappage relationnel objet dans les applications UWP.
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, données, base de données, relationnelle, tables, sqlite
ms.localizationpriority: medium
ms.openlocfilehash: 8ce2bbb1b72eaa33001e86c9aa81b334e338cb53
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362608"
---
# <a name="data-access"></a>Accès aux données

Vous pouvez stocker des données sur l’appareil de l’utilisateur à l’aide d’une base de données SQLite. Vous pouvez également connecter votre application directement à une base de données SQL Server sans avoir à utiliser une forme quelconque de couche de service.

| Rubrique | Description|
|-------|------------|
| [Utiliser une base de données SQLite dans une application UWP](sqlite-databases.md) | Cette rubrique vous montre comment utiliser SQLite pour stocker et récupérer des données dans une base de données légère sur l’appareil des utilisateurs. SQLite est un moteur de base de données intégré, sans serveur. |
| [Utiliser une base de données SQL Server dans une application UWP](sql-server-databases.md) | Cette rubrique vous montre comment vous connecter directement à une base de données SQL Server, puis stocker et récupérer des données à l’aide de classes dans l’espace de noms [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient?redirectedfrom=MSDN). Aucune couche de service n’est requise. |

## <a name="related-topics"></a>Rubriques connexes

* [Exemple de base de données de commandes de clients](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
