---
L’espace de noms Windows.ApplicationModel.Contacts fournit plusieurs options de sélection des contacts.
Sélectionner des contacts
ms.assetid: 35FEDEE6-2B0E-4391-84BA-5E9191D4E442
Mots clés : contacts, sélection
Mots clés : sélection d’un seul contact
Mots clés : sélection de plusieurs contacts
Mots clés : contacts, sélection multiple
Mots clés : sélection de données de contact spécifiques
Mots clés : contact, sélection de données spécifiques
Mots clés : contact, sélection de champs spécifiques
---

# Sélectionner des contacts

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


L’espace de noms [**Windows.ApplicationModel.Contacts**](https://msdn.microsoft.com/library/windows/apps/BR225002) fournit plusieurs options de sélection des contacts. Ici, nous allons vous montrer comment sélectionner un ou plusieurs contacts, et comment configurer le sélecteur de contacts pour récupérer uniquement les informations de contact dont votre application a besoin.

## Configurer le sélecteur de contacts

Créez une instance de [**Windows.ApplicationModel.Contacts.ContactPicker**](https://msdn.microsoft.com/library/windows/apps/BR224913) et attribuez-lui une variable.

```cs
var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
```

## Définir le mode de sélection (facultatif)

Par défaut, le sélecteur de contacts récupère toutes les données disponibles des contacts que l’utilisateur sélectionne. La propriété [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/BR224913-selectionmode) vous permet de configurer le sélecteur de contacts pour récupérer uniquement les champs de données dont votre application a besoin. Il s’agit d’un moyen plus efficace pour utiliser le sélecteur de contacts, si vous avez seulement besoin d’un sous-ensemble des données de contact disponibles.

Affectez d’abord à la propriété [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/BR224913-selectionmode) la valeur **Fields** :

```cs
contactPicker.SelectionMode = Windows.ApplicationModel.Contacts.ContactSelectionMode.Fields;
```

Utilisez ensuite la propriété [**desiredFieldsWithContactFieldType**](https://msdn.microsoft.com/library/windows/apps/BR224913-desiredfieldswithcontactfieldtype) pour indiquer les champs que le sélecteur de contacts doit récupérer. Cet exemple configure le sélecteur de contacts pour récupérer les adresses de messagerie :

``` cs
contactPicker.DesiredFieldsWithContactFieldType.Add(Windows.ApplicationModel.Contacts.ContactFieldType.Email);
```

## Lancer le sélecteur

```cs
Contact contact = await contactPicker.PickContactAsync();
```

Utilisez [**pickContactsAsync**](https://msdn.microsoft.com/library/windows/apps/BR224913-pickcontactsasync) si vous voulez que l’utilisateur sélectionne un ou plusieurs contacts.

```cs
public IList&lt;Contact&gt; contacts;
contacts = await contactPicker.PickContactsAsync();
```

## Traiter les contacts

Quand le sélecteur retourne sa valeur, vérifiez si l’utilisateur a sélectionné des contacts. Si c’est le cas, traitez les informations de contact.

Cet exemple montre comment traiter un contact unique. Ici, nous récupérons le nom du contact afin de le copier dans un contrôle [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) appelé *OutputName*.

```cs
if (contact != null)
{
    OutputName.Text = contact.DisplayName;
}
else
{
    rootPage.NotifyUser(&quot;No contact was selected.&quot;, NotifyType.ErrorMessage);
}
```

Cet exemple montre comment traiter plusieurs contacts.

```cs
if (contacts != null &amp;&amp; contacts.Count &gt; 0)
{
    foreach (Contact contact in contacts)
    {
        // Do something with the contact information.
    }
}
```

## Exemple complet (contact unique)

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

private void AppendContactFieldValues&lt;T&gt;(TextBlock content, IList&lt;T&gt; fields)
{
    if (fields.Count &gt; 0)
    {
        StringBuilder output = new StringBuilder();

        if (fields[0].GetType() == typeof(ContactEmail))
        {
            foreach (ContactEmail email in fields as IList&lt;ContactEmail&gt;)
            {
                output.AppendFormat(&quot;Email: {0} ({1})\n&quot;, email.Address, email.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactPhone))
        {
            foreach (ContactPhone phone in fields as IList&lt;ContactPhone&gt;)
            {
                output.AppendFormat(&quot;Phone: {0} ({1})\n&quot;, phone.Number, phone.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactAddress))
        {
            List&lt;String&gt; addressParts = null;
            string unstructuredAddress = &quot;&quot;;

            foreach (ContactAddress address in fields as IList&lt;ContactAddress&gt;)
            {
                addressParts = (new List&lt;string&gt; { address.StreetAddress, address.Locality, address.Region, address.PostalCode });
                unstructuredAddress = string.Join(&quot;, &quot;, addressParts.FindAll(s =&gt; !string.IsNullOrEmpty(s)));
                output.AppendFormat(&quot;Address: {0} ({1})\n&quot;, unstructuredAddress, address.Kind);
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

## Exemple complet (contacts multiples)

Cet exemple utilise le sélecteur de contacts pour récupérer plusieurs contacts, puis les ajoute à un contrôle [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) appelé `OutputContacts`.

```cs
MainPage rootPage = MainPage.Current;
public IList&lt;Contact&gt; contacts;

private async void PickContactsButton-Click(object sender, RoutedEventArgs e)
{
    var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
    contactPicker.CommitButtonText = &quot;Select&quot;;
    contacts = await contactPicker.PickContactsAsync();

    // Clear the ListView.
    OutputContacts.Items.Clear();

    if (contacts.Count &gt; 0)
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
        if (contact.Emails.Count &gt; 0)
        {
            SecondaryText = contact.Emails[0].Address;
        }
        else if (contact.Phones.Count &gt; 0)
        {
            SecondaryText = contact.Phones[0].Number;
        }
        else if (contact.Addresses.Count &gt; 0)
        {
            List&lt;string&gt; addressParts = (new List&lt;string&gt; { contact.Addresses[0].StreetAddress, 
              contact.Addresses[0].Locality, contact.Addresses[0].Region, contact.Addresses[0].PostalCode });
              string unstructuredAddress = string.Join(&quot;, &quot;, addressParts.FindAll(s =&gt; !string.IsNullOrEmpty(s)));
            SecondaryText = unstructuredAddress;
        }
    }
}
```

## Récapitulatif et étapes suivantes

À présent, vous savez utiliser de manière basique le sélecteur de contacts pour récupérer des informations de contact. Téléchargez les [Exemples d’applications Windows universelles](http://go.microsoft.com/fwlink/p/?linkid=619979) dans GitHub pour voir d’autres exemples illustrant comment utiliser les contacts et le sélecteur de contacts.



<!--HONumber=Mar16_HO1-->


