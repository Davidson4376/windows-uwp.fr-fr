---
author: Xansky
description: "Montre comment lancer la boîte de dialogue de rédaction d’un message électronique pour permettre à l’utilisateur d’envoyer un message électronique. Vous pouvez préremplir les champs de l’e-mail avec des données avant d’afficher la boîte de dialogue. Le message ne sera pas envoyé tant que l’utilisateur n’aura pas appuyé sur le bouton d’envoi."
title: Envoyer un e-mail
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: contacts, e-mail, envoi
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 67c2f808050547f5a56cbeb4e1087cdf3555727d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="send-email"></a>Envoyer un e-mail

\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Montre comment lancer la boîte de dialogue de rédaction d’un message électronique pour permettre à l’utilisateur d’envoyer un message électronique. Vous pouvez préremplir les champs de l’e-mail avec des données avant d’afficher la boîte de dialogue. The message will not be sent until the user taps the send button.

**In this article**

-   [Lancer la boîte de dialogue de rédaction d’un message électronique](#launch-the-compose-email-dialog)
-   [Récapitulatif et étapes suivantes](#summary-and-next-steps)
-   [Rubriques connexes](#related-topics)

## <a name="launch-the-compose-email-dialog"></a>Lancer la boîte de dialogue de rédaction d’un message électronique

Créez un objet [**EmailMessage**](https://msdn.microsoft.com/library/windows/apps/Dn631270) et définissez les données à préremplir dans la boîte de dialogue de rédaction d’un message électronique. Appelez [**ShowComposeNewEmailAsync**](https://msdn.microsoft.com/library/windows/apps/Dn631269) pour afficher la boîte de dialogue.

``` cs
private async Task ComposeEmail(Windows.ApplicationModel.Contacts.Contact recipient, 
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

## <a name="summary-and-next-steps"></a>Récapitulatif et étapes suivantes

Cette rubrique vous a montré comment lancer la boîte de dialogue de rédaction d’un message électronique. Pour plus d’informations sur la sélection de contacts en tant que destinataires d’un message électronique, voir [Sélectionner des contacts](selecting-contacts.md). Voir [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275) pour sélectionner un fichier à utiliser en pièce jointe d’un message électronique.

## <a name="related-topics"></a>Rubriques connexes

* [Sélection de contacts](selecting-contacts.md)
* [How to continue your Windows Phone app after calling a file picker](https://msdn.microsoft.com/library/windows/apps/xaml/Dn614994)
 

 




