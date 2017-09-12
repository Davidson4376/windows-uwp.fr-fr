---
author: mijacobs
description: 
title: "Contrôle de la photo de la personne"
template: detail.hbs
label: Parallax View
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: kevjey
design-contact: kimsea
dev-contact: kefodero
doc-status: Published
ms.openlocfilehash: c71fdf3d19c8c8e43666620df53258825c4a8eb1
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/22/2017
---
# <a name="person-picture-control"></a>Contrôle de la photo de la personne

> [!IMPORTANT]
> Cet article décrit les fonctionnalités qui n’ont pas encore été publiées et sont susceptibles d’être considérablement modifiées d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.

Le contrôle de photo de la personne affiche l’image d’avatar d’une personne, si celle-ci est disponible. Dans le cas contraire, il affiche les initiales de la personne ou un glyphe générique. Vous pouvez utiliser ce contrôle pour afficher un [objet Contact](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact), un objet qui gère les coordonnées d’une personne, ou vous pouvez fournir manuellement des coordonnées, par exemple un nom d’affichage et une photo de profil.  

> **API importantes**: [classe PersonPicture](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.personpicture), [classe Contact](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact), [classe ContactManager](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.ContactManager)

![Contrôle de la photo de la personne](images/person-picture/person-picture_hero.png)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Utiliser la photo de la personne lorsque vous souhaitez représenter une personne et ses coordonnées. Voici quelques exemples de cas dans lesquels vous pourrez utiliser ce contrôle:
* Pour afficher l’utilisateur actuel
* Pour afficher les contacts d’un carnet d’adresses
* Pour afficher l’expéditeur d’un message 
* Pour afficher un contact de média social

L’illustration montre le contrôle de photo de la personne dans une liste de contacts: ![Contrôle de photo de la personne](images/person-picture/person-picture-control.png)

## <a name="how-to-use-the-person-picture-control"></a>Comment utiliser le contrôle de photo de la personne

La classe PersonPicture vous permet de créer une photo de la personne. Cet exemple crée un contrôle PersonPicture et fournit manuellement le nom d’affichage de la personne, sa photo de profil et ses initiales:

```xaml
<Page
    x:Class="App2.ExamplePage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App2"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <PersonPicture
            DisplayName="Betsy Sherman"
            ProfilePicture="Assets\BetsyShermanProfile.png"
            Initials="BS" />
    </StackPanel>
</Page>
```

## <a name="using-the-person-picture-control-to-display-a-contact-object"></a>Utilisation du contrôle de photo de la personne pour afficher un objet Contact

Vous pouvez utiliser le contrôle de sélecteur de personnes pour afficher un objet [Contact](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact): 

```xaml
<Page
    x:Class="SampleApp.PersonPictureContactExample"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SampleApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        <PersonPicture
            Contact="{x:Bind CurrentContact, Mode=OneWay}" />
            
        <Button Click="LoadContactButton_Click">Load contact</Button>
    </StackPanel>
</Page>
```

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;
using Windows.ApplicationModel.Contacts;

namespace SampleApp
{
    public sealed partial class PersonPictureContactExample : Page, System.ComponentModel.INotifyPropertyChanged
    {
        public PersonPictureContactExample()
        {
            this.InitializeComponent();
        }

        private Windows.ApplicationModel.Contacts.Contact _currentContact; 
        public Windows.ApplicationModel.Contacts.Contact CurrentContact
        {
            get => _currentContact;
            set
            {
                _currentContact = value;
                PropertyChanged?.Invoke(this,
                    new System.ComponentModel.PropertyChangedEventArgs(nameof(CurrentContact)));
            }

        }
        public event System.ComponentModel.PropertyChangedEventHandler PropertyChanged;

        public static async System.Threading.Tasks.Task<Windows.ApplicationModel.Contacts.Contact> CreateContact()
        {

            var contact = new Windows.ApplicationModel.Contacts.Contact();
            contact.FirstName = "Betsy";
            contact.LastName = "Sherman";

            // Get the app folder where the images are stored.
            var appInstalledFolder = 
                Windows.ApplicationModel.Package.Current.InstalledLocation;
            var assets = await appInstalledFolder.GetFolderAsync("Assets");
            var imageFile = await assets.GetFileAsync("betsy.png");
            contact.SourceDisplayPicture = imageFile;

            return contact;
        }

        private async void LoadContactButton_Click(object sender, RoutedEventArgs e)
        {
            CurrentContact = await CreateContact();
        }
    }
}
```

> [!NOTE]
> Pour conserver un code simple, cet exemple crée un nouvel objet Contact. Dans une application réelle, vous permettriez à l’utilisateur de sélectionner un contact ou vous utiliseriez un [ContactManager](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.ContactManager) pour une liste de contacts. Pour plus d’informations sur la récupération et la gestion des contacts, consultez les [articles Contacts et calendriers](../contacts-and-calendar/index.md). 

## <a name="determining-which-info-to-display"></a>Détermination des informations à afficher

Lorsque vous fournissez un objet [Contact](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact), le contrôle de photo de la personne l’évalue afin de déterminer les informations qu’il peut afficher. 

Si une photo est disponible, le contrôle affiche la première image qu’il trouve, dans cet ordre:

1.    LargeDisplayPicture
2.    SmallDisplayPicture
3.    Thumbnail

Vous pouvez modifier l’image choisie en définissant la propriété PreferSmallImage sur true. Cela offre à SmallDisplayPicture une priorité supérieure à celle de LargeDisplayPicture.

En l’absence d’image, le contrôle affiche le nom du contact ou ses initiales. S’il n’existe aucune donnée de nom, le contrôle affiche les données du contact, telles que son adresse de messagerie ou son numéro de téléphone. 

## <a name="related-articles"></a>Articles connexes

* [Contacts et calendriers](../contacts-and-calendar/index.md)
* [Exemples de cartes de visite](http://go.microsoft.com/fwlink/p/?LinkId=624040)




