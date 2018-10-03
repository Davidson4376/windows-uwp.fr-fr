---
author: normesta
Description: Share code between a desktop application and a UWP app
Search.Product: eADQiWindows 10XVcnh
title: Partager du code entre une application de bureau et une application UWP
ms.author: normesta
ms.date: 10/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ca5b722ea97202d57f05613bec88ae6bee1db5f2
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/02/2018
ms.locfileid: "4260108"
---
# <a name="share-code-between-a-desktop-application-and-a-uwp-app"></a>Partager du code entre une application de bureau et une application UWP

Vous pouvez déplacer votre code dans des bibliothèques .NET Standard et créer ensuite une application de plateforme Windows universelle (UWP) pour toucher tous les appareils Windows10. Il n’existe aucun outil qui puisse convertir une application de bureau vers une application UWP, mais vous pouvez réutiliser une grande partie de votre code existant, ce qui réduit le coût de création. Ce guide vous explique comment procéder.

## <a name="share-code-in-a-net-standard-20-library"></a>Partager du code dans une bibliothèque .NET Standard2.0

Placez autant de code que vous pouvez dans les bibliothèques de classes .NET Standard2.0.  Tant que votre code utilise des API qui sont définies dans la norme, vous pouvez le réutiliser dans une application UWP. Il est plus facile que jamais de partager du code dans une bibliothèque .NET Standard, car .NET Standard2.0 inclut un nombre beaucoup plus important d’API.

Voici une excellente vidéo qui vous en apprend davantage à ce sujet.
&nbsp;
> [!VIDEO https://www.youtube.com/embed/YI4MurjfMn8]

### <a name="add-net-standard-libraries"></a>Ajouter des bibliothèques .NET Standard

Tout d’abord, ajoutez une ou plusieurs bibliothèques de classes .NET Standard à votre solution.  

![Ajouter un projet dotnet standard](images/desktop-to-uwp/dot-net-standard-project-template.png)

Le nombre de bibliothèques que vous ajoutez à votre solution dépend de la façon dont vous souhaitez organiser votre code.

Assurez-vous que chaque bibliothèque de classes cible **.NET Standard2.0**.

![Cible .NET Standard2.0](images/desktop-to-uwp/target-standard-20.png)

Vous pouvez trouver ce paramètre dans les pages de propriétés du projet de bibliothèque de classes.

À partir de votre projet d’application de bureau, ajoutez une référence au projet de bibliothèque de classes.

![Référence de la bibliothèque de classes](images/desktop-to-uwp/class-library-reference.png)

Ensuite, utilisez les outils pour déterminer la quantité de votre code qui est conforme à la norme. De cette façon, avant de déplacer du code dans la bibliothèque, vous pouvez choisir les parties que vous pouvez réutiliser, les parties qui nécessitent des modifications minimales et les parties qui resteront spécifiques à l’application.

### <a name="check-library-and-code-compatibility"></a>Vérifier la compatibilité de la bibliothèque et du code

Nous allons commencer par les Packages Nuget et autres fichiers dll que vous avez obtenus d’un tiers.

Si votre application utilise l’un d’eux, déterminez s’ils sont compatibles avec .NET Standard2.0. Pour ce faire, vous pouvez utiliser une extension VisualStudio ou un utilitaire de ligne de commande.

Utilisez ces mêmes outils pour analyser votre code. Téléchargez les outils ici ([dotnet-apiport](https://github.com/Microsoft/dotnet-apiport/releases)), puis regardez cette vidéo pour savoir comment les utiliser.
&nbsp;
> [!VIDEO https://www.youtube.com/embed/rzs_FGPyAlY]

Si votre code n’est pas compatible avec la norme, envisagez d’autres façons d’implémenter ce code. Commencez en ouvrant le [navigateur d’API .NET](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0). Vous pouvez utiliser ce navigateur pour passer en revue les API qui sont disponibles dans .NET Standard2.0. Veillez à cibler la liste sur .NET Standard2.0.

![Option dotnet](images/desktop-to-uwp/dot-net-option.png)

Une partie de votre code sera spécifique à la plateforme et devra rester dans votre projet d’application de bureau.

### <a name="example-migrating-data-access-code-to-a-net-standard-20-library"></a>Exemple: migration du code d’accès aux données vers une bibliothèque .NET Standard2.0

Supposons que nous avons une application Windows Forms très simple qui affiche les clients à partir de notre base de données exemple Northwind.

![Application WindowsForms](images/desktop-to-uwp/win-forms-app.png)

Le projet contient une bibliothèque de classes .NET Standard2.0 avec une classe statique nommée **Northwind**. Si nous déplaçons ce code dans la classe **Northwind**, il ne sera pas compilé car il utilise les classes ``SQLConnection``, ``SqlCommand`` et ``SqlDataReader``, et ces classes ne sont pas disponibles dans .NET Standard2.0.

```csharp
public static ArrayList GetCustomerNames()
{
    ArrayList customers = new ArrayList();

    using (SqlConnection conn = new SqlConnection())
    {
        conn.ConnectionString =
            @"Data Source=" +
            @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        SqlCommand command = new SqlCommand("select ContactName from customers order by ContactName asc", conn);

        using (SqlDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}

```
Vous pouvez utiliser le [navigateur d’API .NET](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0) pour trouver cependant une alternative. Les classes ``DbConnection``, ``DbCommand`` et ``DbDataReader`` sont toutes disponibles dans .NET Standard2.0, donc nous pouvons les utiliser à la place.  

Cette version révisée utilise ces classes pour obtenir une liste de clients. Mais, pour créer une classe ``DbConnection``, nous allons devoir transférer un objet usine créé dans l’application cliente.

```csharp
public static ArrayList GetCustomerNames(DbProviderFactory factory)
{
    ArrayList customers = new ArrayList();

    using (DbConnection conn = factory.CreateConnection())
    {
        conn.ConnectionString = @"Data Source=" +
                        @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        DbCommand command = factory.CreateCommand();
        command.Connection = conn;
        command.CommandText = "select ContactName from customers order by ContactName asc";

        using (DbDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}
```  

Dans la page code-behind de WindowsForms, nous pouvons simplement créer l’instance usine et la transférer dans notre méthode.

```csharp
public partial class Customers : Form
{
    public Customers()
    {
        InitializeComponent();

        dataGridView1.Rows.Clear();

        SqlClientFactory factory = SqlClientFactory.Instance;

        foreach (string customer in Northwind.GetCustomerNames(factory))
        {
            dataGridView1.Rows.Add(customer);
        }
    }
}
```

## <a name="reach-all-windows-devices"></a>Toucher tous les appareils Windows

Vous êtes maintenant prêt à ajouter une application UWP à votre solution.

![image du pont du bureau vers UWP](images/desktop-to-uwp/adaptive-ui.png)

Vous devrez toujours concevoir les pages de l’interface utilisateur en XAML et écrire tout code spécifique à l'appareil ou à la plateforme, mais, une fois que vous aurez terminé, vous serez en mesure d’atteindre l’éventail complet des appareils Windows10 et les pages de votre application auront un aspect moderne qui s’adaptera parfaitement aux différentes tailles d’écran et résolutions.

Votre application répondra aux mécanismes d'entrée autres que via un clavier et une souris, et les fonctionnalités et les paramètres seront intuitifs sur tous les appareils. Cela signifie que les utilisateurs devront apprendre à effectuer les opérations une seule fois, et votre application fonctionnera ensuite de manière très familière, quel que soit l’appareil.

Voici quelques-unes des fonctionnalités fournies avec UWP. Pour en savoir plus, voir [Créez des expériences exceptionnelles avec Windows](https://developer.microsoft.com/windows/why-build-for-uwp).

### <a name="add-a-uwp-project"></a>Ajouter un projet UWP

Tout d'abord, ajoutez un projet UWP à votre solution.

![Projet UWP](images/desktop-to-uwp/new-uwp-app.png)

Ensuite, à partir de votre projet UWP, ajoutez une référence au projet de bibliothèque .NET Standard2.0.

![Référence de bibliothèque de classes](images/desktop-to-uwp/class-library-reference2.png)

### <a name="build-your-pages"></a>Générer vos pages

Ajoutez des pages XAML et appelez le code dans votre bibliothèque .NET Standard2.0.

![Application UWP](images/desktop-to-uwp/uwp-app.png)

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel x:Name="customerStackPanel">
        <ListView x:Name="customerList"/>
    </StackPanel>
</Grid>
```

```csharp
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        SqlClientFactory factory = SqlClientFactory.Instance;

        customerList.ItemsSource = Northwind.GetCustomerNames(factory);
    }
}
```


Pour découvrir la prise en main avec UWP, consultez [Qu'est-ce qu'une application UWP](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

## <a name="reach-ios-and-android-devices"></a>Toucher les appareils iOS et Android

Vous pouvez toucher les appareils iOS et Android en ajoutant des projets Xamarin.  

![Applications Xamarin](images/desktop-to-uwp/xamarin-apps.png)

Ces projets vous permettent d’utiliser C# pour créer des applications iOS et Android avec un accès complet aux API spécifiques à la plateforme et à l'appareil. Ces applications tirent parti de l’accélération matérielle spécifique à la plateforme et sont compilées pour des performances natives.

Elles ont accès à la gamme complète des fonctionnalités exposées par la plate-forme et l’appareil sous-jacents, y compris les fonctionnalités spécifiques à la plateforme comme iBeacons et Android Fragments. Vous utiliserez des contrôles d’interface utilisateur native standard pour créer des interfaces utilisateur dont l'apparence répondra aux attentes des utilisateurs.

Tout comme les applications UWP, le coût d’ajout d’une application Android ou iOS est inférieur, car vous pouvez réutiliser la logique métier dans une bibliothèque de classes .NET Standard2.0. Vous devrez concevoir vos pages d’interface utilisateur en XAML et écrire tout code spécifique à l'appareil ou à la plateforme.

### <a name="add-a-xamarin-project"></a>Ajouter un projet Xamarin

Tout d’abord, ajoutez un **Android**, **iOS** ou **interplateforme** à votre solution.

Vous trouverez ces modèles dans la boîte de dialogue **Ajouter un nouveau projet** sous le groupe **Visual C#**.

![Applications Xamarin](images/desktop-to-uwp/xamarin-projects.png)

>[!NOTE]
>Les projets interplateformes sont parfaits pour les applications avec peu de fonctionnalités spécifiques à la plateforme. Vous pouvez les utiliser pour créer une interface utilisateur XAML native qui s’exécute sur iOS, Android et Windows. Pour en savoir plus, cliquez [ici](https://www.xamarin.com/forms).

Ensuite, à partir de votre projet Android, iOS ou interplateforme, ajoutez une référence au projet de bibliothèque de classes.

![Référence de la bibliothèque de classes](images/desktop-to-uwp/class-library-reference3.png)

### <a name="build-your-pages"></a>Générer vos pages

Notre exemple affiche une liste de clients dans une application Android.

![Application Android](images/desktop-to-uwp/android-app.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp" android:textSize="16sp"
    android:id="@android:id/list">
</TextView>
```

```csharp
[Activity(Label = "MyAndroidApp", MainLauncher = true)]
public class MainActivity : ListActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SqlClientFactory factory = SqlClientFactory.Instance;

        var customers = (string[])Northwind.GetCustomerNames(factory).ToArray(typeof(string));

        ListAdapter = new ArrayAdapter<string>(this, Resource.Layout.list_item, customers);
    }
}
```

Pour vous familiariser avec les projets Android, iOS et interplateforme, voir le [portail des développeurs Xamarin](https://developer.xamarin.com/).

## <a name="next-steps"></a>Étapes suivantes

**Trouvez des réponses à vos questions**

Des questions? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Transmettre des commentaires ou suggérer des fonctionnalités**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
