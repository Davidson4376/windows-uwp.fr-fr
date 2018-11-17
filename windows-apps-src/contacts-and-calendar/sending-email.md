---
author: normesta
description: Montre comment lancer la boîte de dialogue de rédaction d’un message électronique pour permettre à l’utilisateur d’envoyer un message électronique. Vous pouvez préremplir les champs de l’e-mail avec des données avant d’afficher la boîte de dialogue. Le message ne sera pas envoyé tant que l’utilisateur n’aura pas appuyé sur le bouton d’envoi.
title: Envoyer un e-mail
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: contacts, e-mail, envoi
ms.author: normesta
ms.date: 10/11/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0a28809210f71bf523e3cc5f9c8da1db9fbcc90c
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7148115"
---
# <a name="send-email"></a>Envoyer un courrier électronique

Montre comment lancer la boîte de dialogue de rédaction d’un courrier électronique pour permettre à l’utilisateur d’envoyer un courrier électronique. Vous pouvez préremplir les champs de l’e-mail avec des données avant d’afficher la boîte de dialogue. The message will not be sent until the user taps the send button.

**In this article**

-   [Lancer la boîte de dialogue de rédaction d’un message électronique](#launch-the-compose-email-dialog)
-   [Récapitulatif et étapes suivantes](#summary-and-next-steps)
-   [Rubriques connexes](#related-topics)

## <a name="launch-the-compose-email-dialog"></a>Lancer la boîte de dialogue de rédaction d’un message électronique

Créez un objet [**EmailMessage**](https://msdn.microsoft.com/library/windows/apps/Dn631270) et définissez les données à préremplir dans la boîte de dialogue de rédaction d’un message électronique. Appelez [**ShowComposeNewEmailAsync**](https://msdn.microsoft.com/library/windows/apps/Dn631269) pour afficher la boîte de dialogue.

``` cs
private async Task ComposeEmail(Windows.ApplicationModel.Contacts.Contact recipient,
    string subject, string messageBody)
{
    var emailMessage = new Windows.ApplicationModel.Email.EmailMessage();
    emailMessage.Body = messageBody;

    var email = recipient.Emails.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactEmail>();
    if (email != null)
    {
        var emailRecipient = new Windows.ApplicationModel.Email.EmailRecipient(email.Address);
        emailMessage.To.Add(emailRecipient);
        emailMessage.Subject = subject;
    }

    await Windows.ApplicationModel.Email.EmailManager.ShowComposeNewEmailAsync(emailMessage);
}
```

>[!NOTE]
> Les pièces jointes que vous ajoutez à un message électronique à l’aide de la classe [EmailAttachment](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email.emailattachment) seront affichent uniquement dans l’application de messagerie. Si les utilisateurs disposent d’autres programmes de messagerie configuré en tant que leur programme de messagerie par défaut, la fenêtre de rédaction d’un message s’affiche sans la pièce jointe. Il s’agit d’un problème connu.

## <a name="summary-and-next-steps"></a>Récapitulatif et étapes suivantes

Cette rubrique vous a montré comment lancer la boîte de dialogue de rédaction d’un message électronique. Pour plus d’informations sur la sélection de contacts en tant que destinataires d’un message électronique, voir [Sélectionner des contacts](selecting-contacts.md). Voir [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275) pour sélectionner un fichier à utiliser en pièce jointe d’un message électronique.

## <a name="related-topics"></a>Rubriques connexes

* [Sélection de contacts](selecting-contacts.md)
* [How to continue your Windows Phone app after calling a file picker](https://msdn.microsoft.com/library/windows/apps/xaml/Dn614994)
 

 
