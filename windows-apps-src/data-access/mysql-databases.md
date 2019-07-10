---
title: Utiliser une base de données MySQL dans une application UWP
description: Utilisez une base de données MySQL dans une application UWP.
ms.date: 3/28/2019
ms.topic: article
keywords: windows 10, uwp, MySQL, base de données
ms.localizationpriority: medium
ms.openlocfilehash: a7708ca082647aef6bbf2261922d2ebd6723923e
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63785488"
---
# <a name="use-a-mysql-database"></a>Utiliser une base de données MySQL
Cet article contient les étapes nécessaires pour permettre de travailler avec une base de données MySQL à partir d’une application UWP. Il contient également un petit extrait de code montrant comment vous pouvez interagir avec la base de données dans du code.

## <a name="set-up-your-solution"></a>Configurer votre solution

Pour connecter votre application directement à une base de données MySQL, vérifiez que la version minimale de votre projet cible la mise à jour de Fall Creators (Build 16299).  Vous trouverez ces informations dans la page de propriétés de votre projet UWP.

![Image du volet de propriétés Ciblage de Visual Studio montrant la version cible et la version minimale définies sur Fall Creators Update](images/min-version-fall-creators.png)

Ouvrez la **Console du Gestionnaire de package** (Affichage -> Autres fenêtres -> Console du Gestionnaire de package). Utilisez la commande **Install-Package MySql.Data** pour installer le pilote pour MySQL. Cela vous permet d’accéder programmatiquement aux bases de données MySQL.

## <a name="test-your-connection-using-sample-code"></a>Tester votre connexion à l’aide d’un exemple de code
Voici un exemple de connexion à une base de données MySQL distante et de lecture à partir d’une telle base de données. Notez que l’adresse IP, les informations d’identification et le nom de la base de données doivent être personnalisés.

```csharp
string M_str_sqlcon = "server=10.xxx.xx.xxx;user id=foo;password=bar;database=baz";
MySqlConnection mysqlcon = new MySqlConnection(M_str_sqlcon);
MySqlCommand mysqlcom = new MySqlCommand("select * from table1", mysqlcon);
mysqlcon.Open();
MySqlDataReader mysqlread = mysqlcom.ExecuteReader(CommandBehavior.CloseConnection);
while (mysqlread.Read())
{
    Debug.WriteLine(mysqlread.GetString(0)+":"+mysqlread.GetString(1));
}
mysqlcon.Close();
```