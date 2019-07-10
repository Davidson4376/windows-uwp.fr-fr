---
title: Utiliser une base de données SQL Server dans une application UWP
description: Utilisez une base de données SQL Server dans une application UWP.
ms.date: 3/28/2019
ms.topic: article
keywords: windows 10, uwp, SQL Server, base de données
ms.localizationpriority: medium
ms.openlocfilehash: f8986f14872d4e5de2c45bba264de6619ef07141
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66360149"
---
# <a name="use-a-sql-server-database-in-a-uwp-app"></a>Utiliser une base de données SQL Server dans une application UWP
Votre application peut se connecter directement à une base de données SQL Server, puis stocker et récupérer des données à l’aide de classes de l’espace de noms [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient?redirectedfrom=MSDN).

Dans ce guide, nous vous proposerons une méthode pour y parvenir. Si vous installez la base de données exemple [Northwind](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/linq/downloading-sample-databases) sur votre instance de SQL Server, puis que vous utilisez ces extraits de code, vous obtiendrez une interface utilisateur de base qui montre les produits de la base de données exemple.

![Produits Northwind](images/products-northwind.png)

Les extraits de code qui s’affichent dans ce guide sont basés sur cet [exemple plus complet](https://github.com/StefanWickDev/IgniteDemos/tree/master/NorthwindDemo).

## <a name="first-set-up-your-solution"></a>Tout d’abord, configurez votre solution.

Pour connecter votre application directement à une base de données SQL Server, assurez-vous que la version minimale de votre projet cible la mise à jour de Fall Creators.  Vous trouverez ces informations dans la page de propriétés de votre projet UWP.

![Version minimum du SDK Windows](images/min-version-fall-creators.png)

Dans le concepteur de manifeste, ouvrez le fichier **package.appxmanifest** du projet UWP.

Dans l’onglet **Fonctionnalités**, cochez la case **Authentification d’entreprise** si vous utilisez l’authentification Windows pour authentifier votre serveur SQL Server.

![Fonctionnalité d’authentification en entreprise](images/enterprise-authentication.png)

<a id="use-data" />

## <a name="add-and-retrieve-data-in-a-sql-server-database"></a>Ajouter et récupérer des données dans une base de données SQL Server

Dans cette section, nous allons effectuer les opérations suivantes :

: un : Ajouter une chaîne de connexion.

: deux : Créer une classe pour contenir des données sur les produits.

: trois : Récupérer les produits à partir de la base de données SQL Server.

: quatre : Ajouter une interface utilisateur de base.

: cinq : Remplir l’interface utilisateur avec des produits.

>[!NOTE]
> Cette section illustre une façon d’organiser votre code d’accès aux données. Vous y trouverez un exemple illustrant la façon dont vous pouvez utiliser [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient?redirectedfrom=MSDN) pour stocker et récupérer des données à partir d’une base de données SQL Server. Vous pouvez organiser votre code d’une manière plus appropriée pour la conception de votre application.

### <a name="add-a-connection-string"></a>Ajouter une chaîne de connexion

Dans le fichier **App.xaml.cs**, ajoutez une propriété à la classe ``App`` qui permet aux autres classes de votre solution d’accéder à la chaîne de connexion.

Notre chaîne de connexion pointe vers la base de données Northwind sur une instance de SQL Server Express.

```csharp
sealed partial class App : Application
{
    // Connection string for using Windows Authentication.
    private string connectionString =
        @"Data Source=YourServerName\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

    // This is an example connection string for using SQL Server Authentication.
    // private string connectionString =
    //     @"Data Source=YourServerName\YourInstanceName;Initial Catalog=DatabaseName; User Id=XXXXX; Password=XXXXX";

    public string ConnectionString { get => connectionString; set => connectionString = value; }

    ...
}
```

### <a name="create-a-class-to-hold-product-data"></a>Créer une classe pour contenir des données sur les produits

Nous allons créer une classe qui implémente l’événement [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?redirectedfrom=MSDN) pour pouvoir lier des attributs de notre interface utilisateur XAML aux propriétés de cette classe.

```csharp
public class Product : INotifyPropertyChanged
{
    public int ProductID { get; set; }
    public string ProductCode { get { return ProductID.ToString(); } }
    public string ProductName { get; set; }
    public string QuantityPerUnit { get; set; }
    public decimal UnitPrice { get; set; }
    public string UnitPriceString { get { return UnitPrice.ToString("######.00"); } }
    public int UnitsInStock { get; set; }
    public string UnitsInStockString { get { return UnitsInStock.ToString("#####0"); } }
    public int CategoryId { get; set; }

    public event PropertyChangedEventHandler PropertyChanged;
    private void NotifyPropertyChanged(string propertyName)
    {
        if (PropertyChanged != null)
        {
            PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
        }
    }

}
```

### <a name="retrieve-products-from-the-sql-server-database"></a>Récupérer les produits à partir de la base de données SQL Server

Créez une méthode qui obtient des produits à partir de l’exemple de base de données, Northwind, puis les retourne sous la forme d’une collection [ObservableCollection](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1?redirectedfrom=MSDN) d’instances ``Product``.

```csharp
public ObservableCollection<Product> GetProducts(string connectionString)
{
    const string GetProductsQuery = "select ProductID, ProductName, QuantityPerUnit," +
       " UnitPrice, UnitsInStock, Products.CategoryID " +
       " from Products inner join Categories on Products.CategoryID = Categories.CategoryID " +
       " where Discontinued = 0";

    var products = new ObservableCollection<Product>();
    try
    {
        using (SqlConnection conn = new SqlConnection(connectionString))
        {
            conn.Open();
            if (conn.State == System.Data.ConnectionState.Open)
            {
                using (SqlCommand cmd = conn.CreateCommand())
                {
                    cmd.CommandText = GetProductsQuery;
                    using (SqlDataReader reader = cmd.ExecuteReader())
                    {
                        while (reader.Read())
                        {
                            var product = new Product();
                            product.ProductID = reader.GetInt32(0);
                            product.ProductName = reader.GetString(1);
                            product.QuantityPerUnit = reader.GetString(2);
                            product.UnitPrice = reader.GetDecimal(3);
                            product.UnitsInStock = reader.GetInt16(4);
                            product.CategoryId = reader.GetInt32(5);
                            products.Add(product);
                        }
                    }
                }
            }
        }
        return products;
    }
    catch (Exception eSql)
    {
        Debug.WriteLine("Exception: " + eSql.Message);
    }
    return null;
}
```

### <a name="add-a-basic-user-interface"></a>Ajouter une interface utilisateur de base

 Ajoutez le code XAML suivant au fichier **MainPage.xaml** du projet UWP.

 Ce code XAML crée un [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) pour afficher chaque produit renvoyé dans l’extrait de code précédent et lie les attributs de chaque ligne dans le [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) aux propriétés que nous avons définies dans la classe ``Product``.

```xml
<Grid Background="{ThemeResource SystemControlAcrylicWindowBrush}">
    <RelativePanel>
        <ListView Name="InventoryList"
                  SelectionMode="Single"
                  ScrollViewer.VerticalScrollBarVisibility="Auto"
                  ScrollViewer.IsVerticalRailEnabled="True"
                  ScrollViewer.VerticalScrollMode="Enabled"
                  ScrollViewer.HorizontalScrollMode="Enabled"
                  ScrollViewer.HorizontalScrollBarVisibility="Auto"
                  ScrollViewer.IsHorizontalRailEnabled="True"
                  Margin="20">
            <ListView.HeaderTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Horizontal"  >
                        <TextBlock Text="ID" Margin="8,0" Width="50" Foreground="DarkRed" />
                        <TextBlock Text="Product description" Width="300" Foreground="DarkRed" />
                        <TextBlock Text="Packaging" Width="200" Foreground="DarkRed" />
                        <TextBlock Text="Price" Width="80" Foreground="DarkRed" />
                        <TextBlock Text="In stock" Width="80" Foreground="DarkRed" />
                    </StackPanel>
                </DataTemplate>
            </ListView.HeaderTemplate>
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:Product">
                    <StackPanel Orientation="Horizontal" >
                        <TextBlock Name="ItemId"
                                    Text="{x:Bind ProductCode}"
                                    Width="50" />
                        <TextBlock Name="ItemName"
                                    Text="{x:Bind ProductName}"
                                    Width="300" />
                        <TextBlock Text="{x:Bind QuantityPerUnit}"
                                   Width="200" />
                        <TextBlock Text="{x:Bind UnitPriceString}"
                                   Width="80" />
                        <TextBlock Text="{x:Bind UnitsInStockString}"
                                   Width="80" />
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </RelativePanel>
</Grid>
```

### <a name="show-products-in-the-listview"></a>Afficher les produits dans le contrôle ListView

Ouvrez le fichier **MainPage.xaml.cs** et ajoutez du code au constructeur de la classe ``MainPage`` qui définit la propriété **ItemSource** de l’élément [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) sur [ObservableCollection](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1?redirectedfrom=MSDN) de ``Product`` instances.

```csharp
public MainPage()
{
    this.InitializeComponent();
    InventoryList.ItemsSource = GetProducts((App.Current as App).ConnectionString);
}
```

Démarrez le projet pour afficher les produits de la base de données exemple Northwind dans l’interface utilisateur.

![Produits Northwind](images/products-northwind.png)

Explorez l’espace de noms [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient?redirectedfrom=MSDN) pour voir les autres actions que vous pouvez effectuer avec les données dans votre base de données SQL Server.

## <a name="trouble-connecting-to-your-database"></a>Problèmes de connexion à votre base de données ?

Dans la plupart des cas, certains aspects de la configuration de SQL Server doivent être modifiés. Si vous pouvez vous connecter à votre base de données à partir d’un autre type d’application de bureau, comme une application Windows Forms ou WPF, assurez-vous que vous avez activé TCP/IP pour SQL Server. Vous pouvez le faire dans la console **Gestion de l’ordinateur**.

![Gestion de l'ordinateur](images/computer-management.png)

Ensuite, assurez-vous que votre service SQL Server Browser est en cours d’exécution.

![Service SQL Server Browser](images/sql-browser-service.png)

## <a name="next-steps"></a>Étapes suivantes

**Utiliser une base de données légère pour stocker des données sur le périphérique d’utilisateurs**

Voir [Utiliser une base de données SQLite dans une application UWP](sqlite-databases.md).

**Partager du code entre différentes applications sur différentes plateformes**

Voir [Partager du code entre une application de bureau et une application UWP](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate).

**Ajouter des pages maître/détail avec les back ends Azure SQL**

Voir [Exemple de base de données de commandes de clients](https://github.com/Microsoft/Windows-appsample-customers-orders-database).
