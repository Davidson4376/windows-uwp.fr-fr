---
ms.assetid: B48E21AB-0EA5-444B-8333-393DD8D1B76D
title: Stockage partagé d’entreprise
description: Le stockage partagé d’entreprise définit des emplacements de données locaux pour les applications cœur de métier afin de partager des données.
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 006507d4665f5578310b8d3e31fb8f7fba4117a2
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7696989"
---
# <a name="enterprise-shared-storage"></a>Stockage partagé d’entreprise

Le stockage partagé se compose de deux emplacements où les applications dotées de la fonctionnalité restreinte **enterpriseDeviceLockdown** et d’un certificat d’entreprise disposent d’un accès complet en lecture et écriture. Notez que la fonctionnalité **enterpriseDeviceLockdown** permet aux applications d’utiliser l’API de verrouillage de l’appareil et d’accéder aux dossiers de stockage partagés de l’entreprise. Pour plus d’informations sur l’API, voir l’espace de noms [**Windows.Embedded.DeviceLockdown**](http://go.microsoft.com/fwlink/?LinkId=699331).  

Ces emplacements sont définis sur le disque local:
- \Data\SharedData\Enterprise\Persistent
- \Data\SharedData\Enterprise\Non-Persistent

## <a name="scenarios"></a>Scénarios

Stockage partagé d’entreprise prend en charge les scénarios suivants.

- Vous pouvez partager des données au sein de l’ instance d’une application, entre les instances de la même application, ou encore entre les applications en supposant qu’elles disposent de la fonctionnalité et du certificat appropriés.
- Vous pouvez stocker des données sur le disque dur local dans le dossier \Data\SharedData\Enterprise\Persistent qui seront conservées même après la réinitialisation de l’appareil.
- Vous pouvez manipuler des fichiers (lecture, écriture, suppression) sur un appareil via le service de gestion des périphériques mobiles (GPM). Pour plus d’informations sur l’utilisation du stockage partagé d’entreprise, voir [Fournisseur de services de configuration (CSP) EnterpriseExtFileSystem](http://go.microsoft.com/fwlink/?LinkId=699333).

## <a name="access-enterprise-shared-storage"></a>Accéder au stockage partagé d’entreprise

L’exemple suivant montre comment déclarer la fonctionnalité pour accéder au stockage d’entreprise partagé dans le manifeste du package et comment accéder aux dossiers de stockage partagé à l’aide de la classe Windows.Storage.StorageFolder.

Incluez la fonctionnalité suivante dans le manifeste de package de votre application:

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp rescap">

…

<Capabilities>
    <rescap:Capability Name="enterpriseDeviceLockdown"/>
</Capabilities>
```

Pour accéder à l’emplacement des données partagées, votre application utilise le code suivant.

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using Windows.Storage;

…

// Get the Enterprise Shared Storage folder.
var enterprisePersistentFolderRoot = @"C:\Data\SharedData\Enterprise\Persistent";

StorageFolder folder =
    await StorageFolder.GetFolderFromPathAsync(enterprisePersistentFolderRoot);

// Get the files in the folder.
IReadOnlyList<StorageFile> sortedItems =
    await folder.GetFilesAsync();

// Iterate over the results and print the list of files
// to the Visual Studio Output window.
foreach (StorageFile file in sortedItems)
    Debug.WriteLine(file.Name + ", " + file.DateCreated);
```

