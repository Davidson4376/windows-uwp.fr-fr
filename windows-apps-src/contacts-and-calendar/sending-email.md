---
author: Xansky
description: "Montre comment lancer la boîte de dialogue de rédaction d’un message électronique pour permettre à l’utilisateur d’envoyer un message électronique. Vous pouvez préremplir les champs de l’e-mail avec des données avant d’afficher la boîte de dialogue. Le message ne sera pas envoyé tant que l’utilisateur n’aura pas appuyé sur le bouton d’envoi."
title: "Envoyer un courrier électronique"
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: contacts, email, send
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: b4b5b029c321256028993e283395a91bd0ed3d7c

---

# Envoyer un courrier électronique

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Montre comment lancer la boîte de dialogue de rédaction d’un message électronique pour permettre à l’utilisateur d’envoyer un message électronique. Vous pouvez préremplir les champs de l’e-mail avec des données avant d’afficher la boîte de dialogue. The message will not be sent until the user taps the send button.

**In this article**

-   [Lancer la boîte de dialogue de rédaction d’un message électronique](#launch-the-compose-email-dialog)
-   [Récapitulatif et étapes suivantes](#summary-and-next-steps)
-   [Rubriques connexes](#related-topics)

## Lancer la boîte de dialogue de rédaction d’un message électronique

Créez un objet [**EmailMessage**](https://msdn.microsoft.com/library/windows/apps/Dn631270) et définissez les données à préremplir dans la boîte de dialogue de rédaction d’un message électronique. Appelez [**ShowComposeNewEmailAsync**](https://msdn.microsoft.com/library/windows/apps/Dn631269) pour afficher la boîte de dialogue.

``` cs
private async void ComposeEmail(Windows.ApplicationModel.Contacts.Contact recipient, 
    string messageBody, 
    StorageFile attachmentFile)
{
    var emailMessage = new Windows.ApplicationModel.Email.EmailMessage();
    emailMessage.Body = messageBody;

    if (attachmentFile != null)
    {
        var stream = Windows.Storage.Streams.RandomAccessStreamReference.CreateFromFile(attachmentFile);

        var attachment = new Windows.ApplicationModel.Email.EmailAttachment(
            attachmentFile.Name,
            stream);

        emailMessage.Attachments.Add(attachment);
    }

    var email = recipient.Emails.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactEmail>();
    if (email != null)
    {
        var emailRecipient = new Windows.ApplicationModel.Email.EmailRecipient(email.Address);
        emailMessage.To.Add(emailRecipient);
    }

    await Windows.ApplicationModel.Email.EmailManager.ShowComposeNewEmailAsync(emailMessage);
        
}
```

## Récapitulatif et étapes suivantes

Cette rubrique vous a montré comment lancer la boîte de dialogue de rédaction d’un message électronique. Pour plus d’informations sur la sélection de contacts en tant que destinataires d’un message électronique, voir [Sélectionner des contacts](selecting-contacts.md). Voir [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275) pour sélectionner un fichier à utiliser en pièce jointe d’un message électronique.

## Rubriques connexes

* [Sélection de contacts](selecting-contacts.md)
* [How to continue your Windows Phone app after calling a file picker](https://msdn.microsoft.com/library/windows/apps/xaml/Dn614994)
 

 







<!--HONumber=Jun16_HO3-->


