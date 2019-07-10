---
title: Utiliser une base de données MongoDB dans une application UWP
description: Utilisez une base de données MongoDB dans une application UWP.
ms.date: 3/28/2019
ms.topic: article
keywords: windows 10, uwp, MongoDB, base de données
ms.localizationpriority: medium
ms.openlocfilehash: a104993d77828a6383eed39a4ab8c12852533894
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63785251"
---
# <a name="use-a-mongodb-database"></a>Utiliser une base de données MongoDB
Cet article contient les étapes nécessaires pour permettre de travailler avec une base de données MongoDB à partir d’une application UWP. Il contient également un petit extrait de code montrant comment vous pouvez interagir avec la base de données dans du code.

## <a name="set-up-your-solution"></a>Configurer votre solution

Pour connecter votre application directement à une base de données MongoDB, vérifiez que la version minimale de votre projet cible la mise à jour de Fall Creators (Build 16299).  Vous trouverez ces informations dans la page de propriétés de votre projet UWP.

![Image du volet de propriétés Ciblage de Visual Studio montrant la version cible et la version minimale définies sur Fall Creators Update](images/min-version-fall-creators.png)

Ouvrez la **Console du Gestionnaire de package** (Affichage -> Autres fenêtres -> Console du Gestionnaire de package). Utilisez la commande **Install-Package MongoDB.Driver** pour installer le pilote pour MongoDB. Cela vous permet d’accéder par programmation aux bases de données MongoDB.

## <a name="test-your-connection-using-sample-code"></a>Tester votre connexion à l’aide d’un exemple de code
L’exemple de code suivant obtient une collection d’un client MongoDB distant, puis ajoute un nouveau document à cette collection. Il utilise ensuite des API MongoDB pour récupérer la nouvelle taille de la collection ainsi que le document inséré, puis il les imprime. Notez que l’adresse IP et les noms de bases de données doivent être personnalisés.

```csharp
var client = new MongoClient("mongodb://10.xxx.xx.xxx:xxx");
IMongoDatabase database = client.GetDatabase("foo");
IMongoCollection<BsonDocument> collection = database.GetCollection<BsonDocument>("bar");
BsonDocument document = new BsonDocument
{
     { "name","MongoDB"},
     { "type","Database"},
     { "count",1},
     { "info",new BsonDocument { { "x", 203 }, { "y", 102 } }}
};
collection.InsertOne(document);
long count = collection.CountDocuments(document);
Debug.WriteLine(count);
IFindFluent<BsonDocument, BsonDocument> document1 = collection.Find(document);
Debug.WriteLine(document1.ToString());
```