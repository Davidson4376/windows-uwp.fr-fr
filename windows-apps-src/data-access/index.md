---
author: mcleblanc
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: "Accès aux données"
description: "Cette section présente le stockage des données sur l’appareil dans une base de données privée et l’utilisation du mappage relationnel objet dans les applications de plateforme Windows universelle (UWP)."
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, données, base de données relationnelle, tables, sqlite"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 0f7aff1c9e7354fcf92d24ed8acb88b41a23d377
ms.lasthandoff: 02/07/2017

---
# <a name="data-access"></a>Accès aux données

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Cette section présente le stockage des données sur l’appareil dans une base de données privée et l’utilisation du mappage relationnel objet dans les applications de plateforme Windows universelle (UWP).

SQLite est inclus dans le SDK UWP. Entity Framework 7 fonctionne avec SQLite dans les applications UWP. Utilisez ces technologies pour développer des scénarios de connectivité intermittente/hors connexion, et pour conserver des données entre des sessions d’application.

| Rubrique | Description|
|-------|------------|
| [Entity Framework 7 avec des applications SQLite pour C#](entity-framework-7-with-sqlite-for-csharp-apps.md) | Entity Framework (EF) est un mappeur relationel objet qui vous permet de travailler avec les données relationnelles en utilisant des objets spécifiques au domaine. Cet article explique comment utiliser Entity Framework 7 avec une base de données SQLite dans une application Windows universelle. |
| [Bases de données SQLite](sqlite-databases.md) | SQLite est un moteur de base de données intégré, sans serveur. Cet article explique comment utiliser la bibliothèque SQLite incluse dans le Kit de développement logiciel (SDK), créer un package pour votre propre bibliothèque SQLite dans une application Windows universelle ou la générer à partir de la source. |

