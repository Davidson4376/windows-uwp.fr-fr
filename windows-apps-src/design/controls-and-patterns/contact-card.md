---
author: mijacobs
Description: A button gives the user a way to trigger an immediate action.
title: Carte de visite
ms.author: mijacobs
ms.date: 03/07/2018
ms.topic: article
keywords: windows10, uwp
pm-contact: kele
design-contact: tbd
dev-contact: tbd
doc-status: not-published
ms.localizationpriority: medium
ms.openlocfilehash: 7b20ac0692aef0e4809bb879747551e049a26df7
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6467390"
---
# <a name="contact-card"></a>Carte de visite

La carte de visite affiche des coordonnées, telles que le nom, le numéro de téléphone et l’adresse, pour un [Contact](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) (mécanisme utilisé par UWP pour représenter les personnes et les entreprises).  La carte de visite permet également à l’utilisateur de modifier des coordonnées. Vous pouvez choisir d’afficher une carte de visite compacte ou une carte de visite complète contenant des informations supplémentaires.

> **API importantes**: [méthode ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowFullContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_Foundation_Rect_), [méthode ShowFullContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_ApplicationModel_Contacts_FullContactCardOptions_), [méthode IsShowContactCardSupported](/uwp/api/windows.applicationmodel.contacts.contactmanager.IsShowContactCardSupported), [classe Contact](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact)  

Vous pouvez afficher la carte de visite de deux façons:  
* sous forme de carte de visite standard s’affichant dans un menu volant à fermeture de type abandon interactif: la carte de visite disparaît lorsque l’utilisateur clique en dehors de la carte; 
* sous forme de carte de visite complète occupant davantage d’espace et à fermeture classique: l’utilisateur doit cliquer sur **fermer** pour la faire disparaître. 


<figure>
    <img src="images/contact-card/contact-card-standard.png" alt="The full contact card">
    <figcaption>Carte de visite standard</figcaption>
</figure>

<figure>
    <img src="images/contact-card/contact-card-full.png" alt="The full contact card">
    <figcaption>Carte de visite complète</figcaption>
</figure>


## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Utilisez la carte de visite si vous souhaitez afficher les coordonnées d’un contact. Si vous souhaitez uniquement afficher le nom du contact et son image, utilisez le [contrôle de photo de la personne](person-picture.md). 


<!-- TODO: Add examples back when the contact card has been added. -->

<!-- ## Examples

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>If you have the <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> app installed, click here to <a href="xamlcontrolsgallery:/item/Button">open the app and see the Button in action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Get the source code (GitHub)</a></li>
    </ul>
</td>
</tr>
</table> -->

## <a name="show-a-standard-contact-card"></a>Afficher une carte de visite standard

1. En règle générale, vous affichez une carte de visite parce que l’utilisateur a cliqué sur un élément: un bouton ou peut-être le [contrôle de photo de la personne](person-picture.md). Nous ne voulons pas que l’élément soit masqué. Pour éviter qu'il soit masqué, nous devons créer un objet [Rect](/uwp/api/windows.foundation.rect) qui décrit l’emplacement et la taille de l’élément. 

    Nous allons créer une fonction utilitaire qui va s'en charger: nous l'utiliserons ultérieurement.
    ```csharp
    // Gets the rectangle of the element 
    public static Rect GetElementRectHelper(FrameworkElement element) 
    { 
        // Passing "null" means set to root element. 
        GeneralTransform elementTransform = element.TransformToVisual(null); 
        Rect rect = elementTransform.TransformBounds(new Rect(0, 0, element.ActualWidth, element.ActualHeight)); 
        return rect; 
    } 

    ```

2. Déterminez si vous pouvez afficher la carte de visite en appelant la méthode [ContactManager.IsShowContactCardSupported](/uwp/api/windows.applicationmodel.contacts.contactmanager.IsShowContactCardSupported). Si elle n’est pas prise en charge, affichez un message d’erreur. (Cet exemple suppose que vous affichez la carte de visite en réponse à un événement Click.)
    ```csharp
    // Contact and Contact Managers are existing classes 
    private void OnUserClickShowContactCard(object sender, RoutedEventArgs e) 
    { 
        if (ContactManager.IsShowContactCardSupported()) 
        { 

    ```

3. Utilisez la fonction utilitaire que vous avez créée à l’étape1 pour obtenir les limites du contrôle qui a déclenché l’événement (de sorte que la carte de visite ne le recouvre pas).

    ```csharp
            Rect selectionRect = GetElementRect((FrameworkElement)sender); 
    ```

4. Obtenez l'objet [Contact](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) que vous voulez afficher. Cet exemple se contente de créer un contact simple, mais votre code doit récupérer un contact réel. 

    ```csharp
                // Retrieve the contact to display
                var contact = new Contact(); 
                var email = new ContactEmail(); 
                email.Address = "jsmith@contoso.com"; 
                contact.Emails.Add(email); 
    ```
5. Affichez la carte de visite en appelant la méthode [ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowFullContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_Foundation_Rect_). 

    ```csharp
            ContactManager.ShowFullContactCard(
                contact, selectionRect, Placement.Default); 
        } 
    } 
    ```

Voici l'exemple de code complet:

```csharp
// Gets the rectangle of the element 
public static Rect GetElementRect(FrameworkElement element) 
{ 
    // Passing "null" means set to root element. 
    GeneralTransform elementTransform = element.TransformToVisual(null); 
    Rect rect = elementTransform.TransformBounds(new Rect(0, 0, element.ActualWidth, element.ActualHeight)); 
    return rect; 
} 
 
// Display a contact in response to an event
private void OnUserClickShowContactCard(object sender, RoutedEventArgs e) 
{ 
    if (ContactManager.IsShowContactCardSupported()) 
    { 
        Rect selectionRect = GetElementRect((FrameworkElement)sender);

        // Retrieve the contact to display
        var contact = new Contact(); 
        var email = new ContactEmail(); 
        email.Address = "jsmith@contoso.com"; 
        contact.Emails.Add(email); 
    
        ContactManager.ShowContactCard(
            contact, selectionRect, Placement.Default); 
    } 
} 

```

## <a name="show-a-full-contact-card"></a>Afficher une carte de visite complète

Pour afficher la carte de visite complète, appelez la méthode [ShowFullContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_ApplicationModel_Contacts_FullContactCardOptions_) au lieu de [ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowFullContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_Foundation_Rect_).

```csharp
private void onUserClickShowContactCard() 
{ 
   
    Contact contact = new Contact(); 
    ContactEmail email = new ContactEmail(); 
    email.Address = "jsmith@hotmail.com"; 
    contact.Emails.Add(email); 
 
 
    // Setting up contact options.     
    FullContactCardOptions fullContactCardOptions = new FullContactCardOptions(); 
 
    // Display full contact card on mouse click.   
    // Launch the People’s App with full contact card  
    fullContactCardOptions.DesiredRemainingView = ViewSizePreference.UseLess; 
     
 
    // Shows the full contact card by launching the People App. 
    ContactManager.ShowFullContactCard(contact, fullContactCardOptions); 
} 

```

## <a name="retrieving-real-contacts"></a>Récupération de contacts «réels»

Les exemples de cet article créent un contact simple. Dans une application réelle, vous devrez probablement récupérer un contact existant. Pour obtenir des instructions, voir l'[article Contacts et calendriers](/windows/uwp/contacts-and-calendar/).




## <a name="related-articles"></a>Articles associés
- [Contacts et calendriers](/windows/uwp/contacts-and-calendar/)
- [Exemples de cartes de visite](http://go.microsoft.com/fwlink/p/?LinkId=624040)
- [Contrôle de photo de personne](/windows/uwp/controls-and-patterns/person-picture/)
