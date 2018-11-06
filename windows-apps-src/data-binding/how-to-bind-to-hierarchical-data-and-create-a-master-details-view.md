---
author: mcleblanc
ms.assetid: 0C69521B-47E0-421F-857B-851B0E9605F2
title: Lier des données hiérarchiques et créer un affichage maître/détails
description: Vous pouvez effectuer un affichage maître/détails (également appelé affichage liste/détails) de données hiérarchiques sur plusieurs niveaux en liant les contrôles d’éléments aux instances CollectionViewSource qui sont liées dans une chaîne.
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 60d283f41c495f9612311e4b9b9da3df1a44d498
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2018
ms.locfileid: "6051450"
---
# <a name="bind-hierarchical-data-and-create-a-masterdetails-view"></a>Lier des données hiérarchiques et créer un affichage maître/détails



> **Remarque**consultez également l' [exemple maître/détails](http://go.microsoft.com/fwlink/p/?linkid=619991).

Vous pouvez effectuer un affichage maître/détails (également appelé affichage liste/détails) de données hiérarchiques sur plusieurs niveaux en liant les contrôles d’éléments aux instances [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) qui sont liées dans une chaîne. Dans cette rubrique, nous utilisons l’[extension de balisage {x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) lorsque cela est possible et l’[extension de balisage {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782), plus souple (mais moins performante), si nécessaire.

Une structure commune aux applications de plateforme Windows universelle (UWP) est la navigation vers différentes pages de détails quand un utilisateur effectue une sélection dans une liste principale. Cette technique est utile quand vous souhaitez fournir une représentation visuelle complète de chaque élément à tous les niveaux d’une hiérarchie. Une autre option consiste à afficher plusieurs niveaux de données sur une seule page. Cette technique est utile si vous voulez afficher un petit nombre de listes simples, que l’utilisateur peut parcourir rapidement pour trouver l’élément qui l’intéresse. Cette rubrique explique comment implémenter cette interaction. Les instances [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) assurent le suivi de la sélection actuelle à chaque niveau hiérarchique.

Nous allons créer un affichage d’une hiérarchie d’équipes sportives organisé en plusieurs listes correspondant aux ligues, aux divisions et aux équipes, et qui inclut un affichage Détails pour chaque équipe. Lorsque vous sélectionnez un élément dans une de ces listes, les affichages suivants sont automatiquement actualisés.

![Affichage maître/détails d’une hiérarchie sportive](images/xaml-masterdetails.png)

## <a name="prerequisites"></a>Connaissances requises

Dans cette rubrique, nous partons du principe que vous savez créer une application UWP de base. Pour obtenir des instructions pour la création de votre première application UWP, consultez [Créer votre première application UWP en C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/Hh974581).

## <a name="create-the-project"></a>Créer le projet

Commencez par créer un projet **Application vide (Windows universel)**. Nommez-le «MasterDetailsBinding».

## <a name="create-the-data-model"></a>Créer le modèle de données

Ajoutez une nouvelle classe à votre projet, nommez-la ViewModel.cs et ajoutez-lui ce code. Il s’agira de votre classe de source de liaison.

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace MasterDetailsBinding
{
    public class Team
    {
        public string Name { get; set; }
        public int Wins { get; set; }
        public int Losses { get; set; }
    }

    public class Division
    {
        public string Name { get; set; }
        public IEnumerable<Team> Teams { get; set; }
    }

    public class League
    {
        public string Name { get; set; }
        public IEnumerable<Division> Divisions { get; set; }
    }

    public class LeagueList : List<League>
    {
        public LeagueList()
        {
            this.AddRange(GetLeague().ToList());
        }

        public IEnumerable<League> GetLeague()
        {
            return from x in Enumerable.Range(1, 2)
                   select new League
                   {
                       Name = "League " + x,
                       Divisions = GetDivisions(x).ToList()
                   };
        }

        public IEnumerable<Division> GetDivisions(int x)
        {
            return from y in Enumerable.Range(1, 3)
                   select new Division
                   {
                       Name = String.Format("Division {0}-{1}", x, y),
                       Teams = GetTeams(x, y).ToList()
                   };
        }

        public IEnumerable<Team> GetTeams(int x, int y)
        {
            return from z in Enumerable.Range(1, 4)
                   select new Team
                   {
                       Name = String.Format("Team {0}-{1}-{2}", x, y, z),
                       Wins = 25 - (x * y * z),
                       Losses = x * y * z
                   };
        }
    }
}
```

## <a name="create-the-view"></a>Créer l’affichage

Ensuite, exposez la classe de source de liaison à partir de la classe qui représente votre page de balisage. Pour ce faire, nous ajoutons une propriété de type **LeagueList** à **MainPage**.

```cs
namespace MasterDetailsBinding
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new LeagueList();
        }
        public LeagueList ViewModel { get; set; }
    }
}
```

Enfin, remplacez le contenu du fichier MainPage.xaml avec le balisage suivant, qui déclare trois instances [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) et les lie dans une chaîne. Les contrôles suivants peuvent être liés à la classe **CollectionViewSource** appropriée, selon son niveau dans la hiérarchie.

```xml
<Page
    x:Class="MasterDetailsBinding.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MasterDetailsBinding"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Page.Resources>
        <CollectionViewSource x:Name="Leagues"
            Source="{x:Bind ViewModel}"/>
        <CollectionViewSource x:Name="Divisions"
            Source="{Binding Divisions, Source={StaticResource Leagues}}"/>
        <CollectionViewSource x:Name="Teams"
            Source="{Binding Teams, Source={StaticResource Divisions}}"/>

        <Style TargetType="TextBlock">
            <Setter Property="FontSize" Value="15"/>
            <Setter Property="FontWeight" Value="Bold"/>
        </Style>

        <Style TargetType="ListBox">
            <Setter Property="FontSize" Value="15"/>
        </Style>

        <Style TargetType="ContentControl">
            <Setter Property="FontSize" Value="15"/>
        </Style>

    </Page.Resources>

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        <StackPanel Orientation="Horizontal">

            <!-- All Leagues view -->

            <StackPanel Margin="5">
                <TextBlock Text="All Leagues"/>
                <ListBox ItemsSource="{Binding Source={StaticResource Leagues}}" 
                    DisplayMemberPath="Name"/>
            </StackPanel>

            <!-- League/Divisions view -->

            <StackPanel Margin="5">
                <TextBlock Text="{Binding Name, Source={StaticResource Leagues}}"/>
                <ListBox ItemsSource="{Binding Source={StaticResource Divisions}}" 
                    DisplayMemberPath="Name"/>
            </StackPanel>

            <!-- Division/Teams view -->

            <StackPanel Margin="5">
                <TextBlock Text="{Binding Name, Source={StaticResource Divisions}}"/>
                <ListBox ItemsSource="{Binding Source={StaticResource Teams}}" 
                    DisplayMemberPath="Name"/>
            </StackPanel>

            <!-- Team view -->

            <ContentControl Content="{Binding Source={StaticResource Teams}}">
                <ContentControl.ContentTemplate>
                    <DataTemplate>
                        <StackPanel Margin="5">
                            <TextBlock Text="{Binding Name}" 
                                FontSize="15" FontWeight="Bold"/>
                            <StackPanel Orientation="Horizontal" Margin="10,10">
                                <TextBlock Text="Wins:" Margin="0,0,5,0"/>
                                <TextBlock Text="{Binding Wins}"/>
                            </StackPanel>
                            <StackPanel Orientation="Horizontal" Margin="10,0">
                                <TextBlock Text="Losses:" Margin="0,0,5,0"/>
                                <TextBlock Text="{Binding Losses}"/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ContentControl.ContentTemplate>
            </ContentControl>

        </StackPanel>

    </Grid>
</Page>
```

Notez qu’en effectuant une liaison directe à la classe [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833), vous signifiez que vous souhaitez créer une liaison à l’élément actuel dans les liaisons où le chemin est introuvable au sein même de la collection. Il est inutile de spécifier la propriété **CurrentItem** en tant que chemin de la liaison, bien que vous puissiez le faire s’il existe la moindre ambiguïté. Par exemple, la classe [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365) de l’affichage de l’équipe a sa propriété [**Content**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content) liée à la classe `Teams`**CollectionViewSource**. Cependant, les contrôles de la classe [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) sont liés aux propriétés de la classe `Team`, car la classe **CollectionViewSource** fournit automatiquement l’équipe actuellement sélectionnée à partir de la liste des équipes dès que nécessaire.

 

 

