---
title: Utiliser une base de données SQLite dans une application UWP
description: Utilisez une base de données SQLite dans une application UWP.
ms.date: 11/30/2018
ms.topic: article
keywords: windows 10, uwp, SQLite, base de données
ms.localizationpriority: medium
ms.openlocfilehash: 1cc8cfe696d35872469d97dba24f5388ff6833b5
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339789"
---
# <a name="use-a-sqlite-database-in-a-uwp-app"></a>Utiliser une base de données SQLite dans une application UWP
Vous pouvez utiliser SQLite pour stocker et récupérer des données dans une base de données légère sur l’appareil des utilisateurs. Ce guide vous explique comment procéder.

## <a name="some-benefits-of-using-sqlite-for-local-storage"></a>Avantages liés à l’utilisation de SQLite pour le stockage local

:heavy_check_mark: SQLite est non activable et autonome. Il s’agit d’une bibliothèque de codes indépendante. Tout est déjà configuré.

:heavy_check_mark: Il n’existe aucun serveur de base de données. Le client et le serveur s’exécutent dans le même processus.

:heavy_check_mark: SQLite étant dans le domaine public, son utilisation et sa distribution avec votre application sont libres.

:heavy_check_mark: SQLite fonctionne sur différentes plateformes et architectures.

Vous trouverez davantage d’informations sur SQLite [ici](https://sqlite.org/about.html).

## <a name="choose-an-abstraction-layer"></a>Choisir une couche d’abstraction

Nous vous recommandons d’utiliser Entity Framework Core ou la [bibliothèque SQLite](https://github.com/aspnet/Microsoft.Data.Sqlite/) open source générée par Microsoft.

### <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) est un mappeur relationnel objet qui vous permet de travailler avec des données relationnelles en utilisant des objets propres au domaine. Si vous avez déjà utilisé cette infrastructure pour travailler avec des données dans d’autres applications .NET, vous pouvez migrer ce code vers une application UWP. Il fonctionnera avec les changements appropriés apportés à la chaîne de connexion.

Pour essayer, consultez [Bien démarrer avec EF Core sur la plateforme Windows universelle (UWP) avec une nouvelle base de données](https://docs.microsoft.com/ef/core/get-started/uwp/getting-started).

### <a name="sqlite-library"></a>Bibliothèque SQLite

La bibliothèque [Microsoft.Data.Sqlite](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite?view=msdata-sqlite-2.0.0) implémente les interfaces dans l’espace de noms [System.Data.Common](https://docs.microsoft.com/dotnet/api/system.data.common). Microsoft gère activement ces implémentations, qui fournissent un wrapper intuitif autour de l’API SQLite native de bas niveau.

Le reste de ce guide vous aide à utiliser cette bibliothèque.

## <a name="set-up-your-solution-to-use-the-microsoftdatasqlite-library"></a>Configurer votre solution pour utiliser la bibliothèque Microsoft.Data.SQlite

Nous allons commencer par un projet UWP de base, ajouter une bibliothèque de classes, puis installer les packages NuGet appropriés.

Le type de bibliothèque de classes que vous ajoutez à votre solution et les packages spécifiques que vous installez dépendent de la version minimale du SDK Windows ciblée par votre application. Vous trouverez ces informations dans la page de propriétés de votre projet UWP.

![Version minimale du SDK Windows](images/min-version.png)

Utilisez l’une des sections suivantes en fonction de la version minimale du SDK Windows ciblée par votre projet UWP.

### <a name="the-minimum-version-of-your-project-does-not-target-the-fall-creators-update"></a>La version minimale de votre projet ne cible pas Fall Creators Update

Si vous utilisez Visual Studio 2015, cliquez sur **Aide**->**À propos de Microsoft Visual Studio**. Ensuite, dans la liste des programmes installés, vérifiez que vous disposez du gestionnaire de packages NuGet version **3.5** ou version ultérieure. Si le numéro de version est inférieur à celui-ci, installez une version ultérieure de NuGet [ici](https://www.nuget.org/downloads). Sur cette page, vous trouverez toutes les versions de NuGet listées sous le titre **Visual Studio 2015**.

Ajoutez ensuite une bibliothèque de classes à votre solution. Vous n’êtes pas obligé d’utiliser une bibliothèque de classes pour stocker votre code d’accès aux données, mais nous allons en utiliser une dans notre exemple. Nous allons nommer la bibliothèque **DataAccessLibrary** et nommer la classe de la bibliothèque **DataAccess**.

![Bibliothèque de classes](images/class-library.png)

Cliquez avec le bouton droit sur la solution, puis cliquez sur **Gérer les packages NuGet pour la solution**.

![Gérer des packages NuGet](images/manage-nuget.png)

Si vous utilisez Visual Studio 2015, choisissez l’onglet **Installé**, puis vérifiez que le package **Microsoft.NETCore.UniversalWindowsPlatform** est bien de version **5.2.2** ou de version ultérieure.

![Version de .NETCore](images/package-version.png)

Si ce n’est pas le cas, installez une version plus récente du package.

Choisissez l’onglet **Parcourir**, puis recherchez le package **Microsoft.Data.SQLite**. Installez la version **1.1.1** (ou une version antérieure) de ce package.

![Package SQLite](images/sqlite-package.png)

Accédez à la section [Ajouter et récupérer des données dans une base de données SQLite](#use-data) de ce guide.

### <a name="the-minimum-version-of-your-project-targets-the-fall-creators-update"></a>La version minimale de votre projet cible Fall Creators Update

La mise à jour de la version minimale de votre projet UWP vers Fall Creators Update présente plusieurs avantages.

Tout d’abord, vous pouvez utiliser des bibliothèques .NET Standard 2.0 au lieu de bibliothèques de classes normales. Cela signifie que vous pouvez partager votre code d’accès aux données avec n’importe quelle autre application .NET, par exemple une application WPF, Windows Forms, Android, iOS ou ASP.NET.

Ensuite, votre application n’a pas besoin de créer un package des bibliothèques SQLite. Au lieu de cela, elle peut utiliser la version de SQLite qui est installée avec Windows. Cette solution est particulièrement intéressante.

:heavy_check_mark: Réduit la taille de votre application, car vous n’êtes pas obligé de télécharger le fichier binaire SQLite puis de l’empaqueter dans votre application.

:heavy_check_mark: Vous évite d’avoir à envoyer (par push) une nouvelle version de votre application aux utilisateurs dans le cas où SQLite publie des correctifs critiques pour les bogues et failles de sécurité dans SQLite. La version Windows de SQLite est gérée par Microsoft en coordination avec SQLite.org.

:heavy_check_mark: Le temps de chargement des applications est potentiellement plus rapide, car le plus souvent, la version du SDK de SQLite est déjà chargée en mémoire.

Commençons par ajouter une bibliothèque de classes .NET Standard 2.0 à votre solution. Vous n’êtes pas obligé d’utiliser une bibliothèque de classes pour stocker votre code d’accès aux données, mais nous allons en utiliser une dans notre exemple. Nous allons nommer la bibliothèque **DataAccessLibrary** et nommer la classe de la bibliothèque **DataAccess**.

![Bibliothèque de classes](images/dot-net-standard.png)

Cliquez avec le bouton droit sur la solution, puis cliquez sur **Gérer les packages NuGet pour la solution**.

![Gérer des packages NuGet](images/manage-nuget-2.png)

À ce stade, vous avez plusieurs possibilités. Vous pouvez utiliser la version de SQLite fournie avec Windows. Si vous avez besoin d’utiliser une version spécifique de SQLite, vous pouvez inclure la bibliothèque SQLite dans votre package.

Commençons par étudier l’utilisation de la version de SQLite fournie avec Windows.

#### <a name="to-use-the-version-of-sqlite-that-is-installed-with-windows"></a>Pour utiliser la version de SQLite installée avec Windows

Choisissez l’onglet **Parcourir**, recherchez le package **Microsoft.Data.SQLite.core**, puis installez-le.

![Package SQLite Core](images/sqlite-core-package.png)

Recherchez le package **SQLitePCLRaw.bundle_winsqlite3**, puis installez-le uniquement dans le projet UWP de votre solution.

![Package SQLite PCL Raw](images/sqlite-raw-package.png)

#### <a name="to-include-sqlite-with-your-app"></a>Pour inclure SQLite dans votre application

Cette opération n’est pas nécessaire. Toutefois, si vous avez besoin d’inclure une version spécifique de SQLite à votre application, choisissez l’onglet **Parcourir**, puis recherchez le package **Microsoft.Data.SQLite**. Installez la version **2.0** (ou une version antérieure) de ce package.

![Package SQLite](images/sqlite-package-v2.png)

<a id="use-data" />

## <a name="add-and-retrieve-data-in-a-sqlite-database"></a>Ajouter et récupérer des données dans une base de données SQLite

Nous allons effectuer les opérations suivantes :

:one: Préparer la classe d’accès aux données.

:two: Initialiser la base de données SQLite.

:three: Insérer des données dans la base de données SQLite.

:four: Récupérer des données dans la base de données SQLite.

:five: Ajouter une interface utilisateur de base.

### <a name="prepare-the-data-access-class"></a>Préparer la classe d’accès aux données

Dans votre projet UWP, ajoutez une référence au projet **DataAccessLibrary** dans votre solution.

![Bibliothèque de classes d’accès aux données](images/ref-class-library.png)

Dans votre projet UWP, ajoutez l’instruction ``using`` suivante aux fichiers **App.xaml.cs** et **MainPage.xaml.cs**.

```csharp
using DataAccessLibrary;
```

Ouvrez la classe **DataAccess** dans votre solution **DataAccessLibrary**, puis rendez cette classe statique.

>[!NOTE]
>Notre exemple va placer le code d’accès aux données dans une classe statique. Il ne s’agit là que d’un choix de conception parfaitement facultatif.

```csharp
namespace DataAccessLibrary
{
    public static class DataAccess
    {

    }
}

```

Ajoutez les instructions using suivantes en haut de ce fichier.

```csharp
using Microsoft.Data.Sqlite;
using System.Collections.Generic;
```

<a id="initialize" />

### <a name="initialize-the-sqlite-database"></a>Initialiser la base de données SQLite

Ajoutez à la classe **DataAccess** une méthode qui initialise la base de données SQLite.

```csharp
public static void InitializeDatabase()
{
    using (SqliteConnection db =
        new SqliteConnection("Filename=sqliteSample.db"))
    {
        db.Open();

        String tableCommand = "CREATE TABLE IF NOT " +
            "EXISTS MyTable (Primary_Key INTEGER PRIMARY KEY, " +
            "Text_Entry NVARCHAR(2048) NULL)";

        SqliteCommand createTable = new SqliteCommand(tableCommand, db);

        createTable.ExecuteReader();
    }
}
```

Ce code crée la base de données SQLite et la stocke dans le magasin de données local de l’application.

Dans cet exemple, nous appelons la base de données ``sqlliteSample.db``, mais vous pouvez choisir le nom que vous souhaitez tant que vous l’utilisez dans tous les objets [SqliteConnection](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqliteconnection?view=msdata-sqlite-2.0.0) que vous instanciez.

Dans le constructeur du fichier **App.xaml.cs** de votre projet UWP, appelez la méthode ``InitializeDatabase`` de la classe **DataAccess**.

```csharp
public App()
{
    this.InitializeComponent();
    this.Suspending += OnSuspending;

    DataAccess.InitializeDatabase();

}
```

<a id="insert" />

### <a name="insert-data-into-the-sqlite-database"></a>Insérer des données dans la base de données SQLite

Ajoutez à la classe **DataAccess** une méthode qui insère des données dans la base de données SQLite. Ce code utilise les paramètres de la requête afin d’empêcher les attaques par injection de code SQL.

```csharp
public static void AddData(string inputText)
{
    using (SqliteConnection db =
        new SqliteConnection("Filename=sqliteSample.db"))
    {
        db.Open();

        SqliteCommand insertCommand = new SqliteCommand();
        insertCommand.Connection = db;

        // Use parameterized query to prevent SQL injection attacks
        insertCommand.CommandText = "INSERT INTO MyTable VALUES (NULL, @Entry);";
        insertCommand.Parameters.AddWithValue("@Entry", inputText);

        insertCommand.ExecuteReader();

        db.Close();
    }

}
```

<a id="retrieve" />

### <a name="retrieve-data-from-the-sqlite-database"></a>Récupérer des données dans la base de données SQLite

Ajoutez une méthode qui obtient des lignes de données d’ une base de données SQLite.

```csharp
public static List<String> GetData()
{
    List<String> entries = new List<string>();

    using (SqliteConnection db =
        new SqliteConnection("Filename=sqliteSample.db"))
    {
        db.Open();

        SqliteCommand selectCommand = new SqliteCommand
            ("SELECT Text_Entry from MyTable", db);

        SqliteDataReader query = selectCommand.ExecuteReader();

        while (query.Read())
        {
            entries.Add(query.GetString(0));
        }

        db.Close();
    }

    return entries;
}
```

La méthode [Read](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.read?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_Read) avance dans les lignes de données retournées. Elle retourne **true** s’il reste des lignes ; sinon, elle retourne **false**.

La méthode [GetString](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getstring?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_GetString_System_Int32_) retourne la valeur de la colonne spécifiée sous forme de chaîne. Elle accepte une valeur entière qui représente l’ordinal de colonne de base zéro des données que vous voulez. Vous pouvez utiliser des méthodes similaires comme [GetDataTime](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getdatetime?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_GetDateTime_System_Int32_) et [GetBoolean](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getboolean?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_GetBoolean_System_Int32_). Choisissez une méthode basée sur le type de données contenues dans la colonne.

Le paramètre ordinal n’est pas aussi important dans cet exemple, car nous allons sélectionner toutes les entrées d’une même colonne. Toutefois, si plusieurs colonnes font partie de votre requête, utilisez la valeur ordinale pour obtenir la colonne de laquelle vous souhaitez extraire des données.

## <a name="add-a-basic-user-interface"></a>Ajouter une interface utilisateur de base

Dans le fichier **MainPage.xaml** du projet UWP, ajoutez le code XAML suivant.

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel>
        <TextBox Name="Input_Box"></TextBox>
        <Button Click="AddData">Add</Button>
        <ListView Name="Output">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextBlock Text="{Binding}"/>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackPanel>
</Grid>
```

Cette interface utilisateur de base fournit à l’utilisateur un ``TextBox`` qu’il peut utiliser pour entrer une chaîne que nous ajouterons à la base de données SQLite. Nous allons connecter le ``Button`` de cette interface utilisateur à un gestionnaire d’événements qui va récupérer des données dans la base de données SQLite, puis les afficher dans le ``ListView``.

Dans le fichier **MainPage.xaml.cs**, ajoutez le gestionnaire suivant. Il s’agit de la méthode que nous avons associée à l’événement ``Click`` de ``Button`` dans l’interface utilisateur.

```csharp
private void AddData(object sender, RoutedEventArgs e)
{
    DataAccess.AddData(Input_Box.Text);

    Output.ItemsSource = DataAccess.GetData();
}
```

C’est terminé. Explorez [Microsoft.Data.Sqlite](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite?view=msdata-sqlite-2.0.0) pour découvrir ce que vous pouvez faire d’autre avec votre base de données SQLite. Consultez les liens ci-dessous pour en savoir plus sur les autres façons d’utiliser des données dans votre application UWP.

## <a name="next-steps"></a>Étapes suivantes

**Connecter votre application directement à une base de données SQL Server**

Consultez [Utiliser une base de données SQL Server dans une application UWP](sql-server-databases.md).

**Partager du code entre différentes applications sur différentes plateformes**

Consultez [Partager du code entre une application de bureau et une application UWP](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate).

**Ajouter des pages maître/détail avec les back-ends Azure SQL**

Consultez [Exemple de base de données de commandes des clients](https://github.com/Microsoft/Windows-appsample-customers-orders-database).
