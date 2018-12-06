---
description: L’espace de noms Windows.ApplicationModel.Contacts fournit plusieurs options de sélection des contacts.
title: Sélectionner des contacts
ms.assetid: 35FEDEE6-2B0E-4391-84BA-5E9191D4E442
keywords: contacts, sélection sélectionner un seul contact sélectionner plusieurs contacts contacts, sélectionner plusieurs sélectionner un contact spécifique contact de données, sélection d’un contact de données spécifique, sélection de champs spécifiques
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 28943c5ff60ebf2326193e4c3f90a02111f4aec2
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8740547"
---
# <a name="select-contacts"></a>Sélectionner des contacts



L’espace de noms [**Windows.ApplicationModel.Contacts**](https://msdn.microsoft.com/library/windows/apps/BR225002) fournit plusieurs options de sélection des contacts. Ici, nous allons vous montrer comment sélectionner un ou plusieurs contacts, et comment configurer le sélecteur de contacts pour récupérer uniquement les informations de contact dont votre application a besoin.

## <a name="set-up-the-contact-picker"></a>Configurer le sélecteur de contacts

Créez une instance de [**Windows.ApplicationModel.Contacts.ContactPicker**](https://msdn.microsoft.com/library/windows/apps/BR224913) et attribuez-lui une variable.

```cs
var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
```

## <a name="set-the-selection-mode-optional"></a>Définir le mode de sélection (facultatif)

Par défaut, le sélecteur de contacts récupère toutes les données disponibles des contacts que l’utilisateur sélectionne. La propriété [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.contacts.contactpicker.selectionmode) vous permet de configurer le sélecteur de contacts pour récupérer uniquement les champs de données dont votre application a besoin. Il s’agit d’un moyen plus efficace pour utiliser le sélecteur de contacts, si vous avez seulement besoin d’un sous-ensemble des données de contact disponibles.

Affectez d’abord à la propriété [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.contacts.contactpicker.selectionmode) la valeur **Fields** :

```cs
contactPicker.SelectionMode = Windows.ApplicationModel.Contacts.ContactSelectionMode.Fields;
```

Utilisez ensuite la propriété [**desiredFieldsWithContactFieldType**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.contacts.contactpicker.desiredfieldswithcontactfieldtype) pour indiquer les champs que le sélecteur de contacts doit récupérer. Cet exemple configure le sélecteur de contacts pour récupérer les adresses de messagerie :

``` cs
contactPicker.DesiredFieldsWithContactFieldType.Add(Windows.ApplicationModel.Contacts.ContactFieldType.Email);
```

## <a name="launch-the-picker"></a>Lancer le sélecteur

```cs
Contact contact = await contactPicker.PickContactAsync();
```

Utilisez [**pickContactsAsync**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.contacts.contactpicker.pickcontactsasync) si vous voulez que l’utilisateur sélectionne un ou plusieurs contacts.

```cs
public IList<Contact> contacts;
contacts = await contactPicker.PickContactsAsync();
```

## <a name="process-the-contacts"></a>Traiter les contacts

Quand le sélecteur retourne sa valeur, vérifiez si l’utilisateur a sélectionné des contacts. Si c’est le cas, traitez les informations de contact.

Cet exemple montre comment traiter un contact unique. Ici, nous récupérons le nom du contact afin de le copier dans un contrôle [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) appelé *OutputName*.

```cs
if (contact != null)
{
    OutputName.Text = contact.DisplayName;
}
else
{
    rootPage.NotifyUser("No contact was selected.", NotifyType.ErrorMessage);
}
```

Cet exemple montre comment traiter plusieurs contacts.

```cs
if (contacts != null && contacts.Count > 0)
{
    foreach (Contact contact in contacts)
    {
        // Do something with the contact information.
    }
}
```

## <a name="complete-example-single-contact"></a>Exemple complet (contact unique)

Cet exemple utilise le sélecteur de contacts pour récupérer le nom, ainsi que l’adresse électronique, la localisation ou le numéro de téléphone d’un contact unique.

```cs
// ...
using Windows.ApplicationModel.Contacts;
// ...

private async void PickAContactButton-Click(object sender, RoutedEventArgs e)
{
    ContactPicker contactPicker = new ContactPicker();

    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.Email);
    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.Address);
    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.PhoneNumber);

    Contact contact = await contactPicker.PickContactAsync();

    if (contact != null)
    {
        OutputFields.Visibility = Visibility.Visible;
        OutputEmpty.Visibility = Visibility.Collapsed;

        OutputName.Text = contact.DisplayName;

        AppendContactFieldValues(OutputEmails, contact.Emails);
        AppendContactFieldValues(OutputPhoneNumbers, contact.Phones);
        AppendContactFieldValues(OutputAddresses, contact.Addresses);
    }
    else
    {
        OutputEmpty.Visibility = Visibility.Visible;
        OutputFields.Visibility = Visibility.Collapsed;
    }
}

private void AppendContactFieldValues<T>(TextBlock content, IList<T> fields)
{
    if (fields.Count > 0)
    {
        StringBuilder output = new StringBuilder();

        if (fields[0].GetType() == typeof(ContactEmail))
        {
            foreach (ContactEmail email in fields as IList<ContactEmail>)
            {
                output.AppendFormat("Email: {0} ({1})\n", email.Address, email.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactPhone))
        {
            foreach (ContactPhone phone in fields as IList<ContactPhone>)
            {
                output.AppendFormat("Phone: {0} ({1})\n", phone.Number, phone.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactAddress))
        {
            List<String> addressParts = null;
            string unstructuredAddress = "";

            foreach (ContactAddress address in fields as IList<ContactAddress>)
            {
                addressParts = (new List<string> { address.StreetAddress, address.Locality, address.Region, address.PostalCode });
                unstructuredAddress = string.Join(", ", addressParts.FindAll(s => !string.IsNullOrEmpty(s)));
                output.AppendFormat("Address: {0} ({1})\n", unstructuredAddress, address.Kind);
            }
        }

        content.Visibility = Visibility.Visible;
        content.Text = output.ToString();
    }
    else
    {
        content.Visibility = Visibility.Collapsed;
    }
}
```

## <a name="complete-example-multiple-contacts"></a>Exemple complet (contacts multiples)

Cet exemple utilise le sélecteur de contacts pour récupérer plusieurs contacts, puis les ajoute à un contrôle [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) appelé `OutputContacts`.

```cs
MainPage rootPage = MainPage.Current;
public IList<Contact> contacts;

private async void PickContactsButton-Click(object sender, RoutedEventArgs e)
{
    var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
    contactPicker.CommitButtonText = "Select";
    contacts = await contactPicker.PickContactsAsync();

    // Clear the ListView.
    OutputContacts.Items.Clear();

    if (contacts != null && contacts.Count > 0)
    {
        OutputContacts.Visibility = Windows.UI.Xaml.Visibility.Visible;
        OutputEmpty.Visibility = Visibility.Collapsed;

        foreach (Contact contact in contacts)
        {
            // Add the contacts to the ListView.
            OutputContacts.Items.Add(new ContactItemAdapter(contact));
        }
    }
    else
    {
        OutputEmpty.Visibility = Visibility.Visible;
    }         
}
```

``` cs
public class ContactItemAdapter
{
    public string Name { get; private set; }
    public string SecondaryText { get; private set; }

    public ContactItemAdapter(Contact contact)
    {
        Name = contact.DisplayName;
        if (contact.Emails.Count > 0)
        {
            SecondaryText = contact.Emails[0].Address;
        }
        else if (contact.Phones.Count > 0)
        {
            SecondaryText = contact.Phones[0].Number;
        }
        else if (contact.Addresses.Count > 0)
        {
            List<string> addressParts = (new List<string> { contact.Addresses[0].StreetAddress,
              contact.Addresses[0].Locality, contact.Addresses[0].Region, contact.Addresses[0].PostalCode });
              string unstructuredAddress = string.Join(", ", addressParts.FindAll(s => !string.IsNullOrEmpty(s)));
            SecondaryText = unstructuredAddress;
        }
    }
}
```

## <a name="summary-and-next-steps"></a>Récapitulatif et étapes suivantes

À présent, vous savez utiliser de manière basique le sélecteur de contacts pour récupérer des informations de contact. Téléchargez les [Exemples d’applications Windows universelles](http://go.microsoft.com/fwlink/p/?linkid=619979) dans GitHub pour voir d’autres exemples illustrant comment utiliser les contacts et le sélecteur de contacts.
