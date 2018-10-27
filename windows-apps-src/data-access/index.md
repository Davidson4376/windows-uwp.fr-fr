---
author: normesta
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: Accès aux données
description: Cette section présente le stockage des données sur l’appareil dans une base de données privée et l’utilisation du mappage relationnel objet dans les applications de plateforme Windows universelle (UWP).
ms.author: normesta
ms.date: 11/13/2017
ms.topic: article
keywords: windows10, uwp, données, base de données relationnelle, tables, sqlite
ms.localizationpriority: medium
ms.openlocfilehash: beca20d358430ecd82cd1bc57459a6f6af36be77
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5693439"
---
# <a name="data-access"></a>Accès aux données

Vous pouvez stocker des données sur l’appareil de l’utilisateur à l’aide d’une base de données SQLite. Vous pouvez également connecter votre application directement à une base de données SQL Server sans avoir à utiliser de couche de service quelconque.

| Rubrique | Description|
|-------|------------|
| [Utiliser une base de donnéesSQLite dans une applicationUWP](sqlite-databases.md) | Vous montre comment utiliser SQLite pour stocker et récupérer des données dans une base de données légère sur l’appareil des utilisateurs. SQLite est un moteur de base de données intégré, sans serveur. |
| [Utiliser une base de données SQL server dans une application UWP](sql-server-databases.md) | Vous montre comment se connecter directement à une base de données SQL Server, puis stocker et récupérer des données à l’aide de classes dans l’espace de noms [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient.aspx) . Aucune couche de service n'est requise. |

## <a name="related-topics"></a>Rubriques associées

* [Exemple de base de données de commandes clients](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
