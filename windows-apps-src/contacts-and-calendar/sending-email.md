---
description: Montre comment lancer la boîte de dialogue de rédaction d’un message électronique pour permettre à l’utilisateur d’envoyer un message électronique. Vous pouvez préremplir les champs de l’e-mail avec des données avant d’afficher la boîte de dialogue. Le message ne sera pas envoyé tant que l’utilisateur n’aura pas appuyé sur le bouton d’envoi.
title: Envoyer un courrier électronique
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: contacts, e-mail, envoi
ms.date: 10/11/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1593ab8b547a464492a35aa7d49d38f667a8210b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626484"
---
# <a name="send-email"></a>Envoyer un courrier électronique

Montre comment lancer la boîte de dialogue de rédaction d’un message électronique pour permettre à l’utilisateur d’envoyer un message électronique. Vous pouvez préremplir les champs de l’e-mail avec des données avant d’afficher la boîte de dialogue. Le message ne sera pas envoyé tant que l’utilisateur n’aura pas appuyé sur le bouton d’envoi.

**Dans cet article**

-   [Lancer la boîte de dialogue Nouveau message électronique](#launch-the-compose-email-dialog)
-   [Résumé et étapes suivantes](#summary-and-next-steps)
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
> Les pièces jointes que vous ajoutez à un message électronique à l’aide de la [EmailAttachment](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email.emailattachment) classe s’affiche uniquement dans l’application de messagerie. Si les utilisateurs disposent de tout autre programme de messagerie configuré en tant que leur programme de messagerie par défaut, la fenêtre Nouveau message s’affiche sans la pièce jointe. Il s'agit d'un problème connu.

## <a name="summary-and-next-steps"></a>Récapitulatif et étapes suivantes

Cette rubrique vous a montré comment lancer la boîte de dialogue de rédaction d’un message électronique. Pour plus d’informations sur la sélection de contacts en tant que destinataires d’un message électronique, voir [Sélectionner des contacts](selecting-contacts.md). Voir [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275) pour sélectionner un fichier à utiliser en pièce jointe d’un message électronique.

## <a name="related-topics"></a>Rubriques connexes

* [Sélection des contacts](selecting-contacts.md)
* [Comment continuer à votre application Windows Phone après l’appel d’un sélecteur de fichiers](https://msdn.microsoft.com/library/windows/apps/xaml/Dn614994)
 

 
