---
description: Cette rubrique vous montre comment lancer la boîte de dialogue de rédaction d’un message SMS pour permettre à l’utilisateur d’envoyer un message SMS. Vous pouvez préremplir les champs du message SMS avec des données avant d’afficher la boîte de dialogue. Le message ne sera pas envoyé tant que l’utilisateur n’aura pas appuyé sur le bouton d’envoi.
title: Envoyer un message SMS
ms.assetid: 4D7B509B-1CF0-4852-9691-E96D8352A4D6
keywords: contacts, SMS, envoi
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: abc9ca7d6c3d6e7120cfc5ede4f10a4dfd5a7c1f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597654"
---
# <a name="send-an-sms-message"></a>Envoyer un message SMS

Cette rubrique vous montre comment lancer la boîte de dialogue de rédaction d’un message SMS pour permettre à l’utilisateur d’envoyer un message SMS. Vous pouvez préremplir les champs du message SMS avec des données avant d’afficher la boîte de dialogue. Le message ne sera pas envoyé tant que l’utilisateur n’aura pas appuyé sur le bouton d’envoi.

Pour appeler ce code, déclarez le **chat**, **smsSend**, et **chatSystem** fonctionnalités dans votre manifeste du package. Il s’agit de [des capacités restreintes](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#special-and-restricted-capabilities) mais vous pouvez les utiliser dans votre application. Vous avez besoin d’approbation uniquement si vous avez l’intention de publier votre application sur le Store. Consultez [types de compte, emplacements et frais](https://docs.microsoft.com/windows/uwp/publish/account-types-locations-and-fees).

## <a name="launch-the-compose-sms-dialog"></a>Lancer la boîte de dialogue de rédaction d’un message SMS

Créez un objet [**ChatMessage**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.chat.chatmessage) et définissez les données à préremplir dans la boîte de dialogue de rédaction d’un message SMS. Appelez [**ShowComposeSmsMessageAsync**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.chat.chatmessagemanager.showcomposesmsmessageasync) pour afficher la boîte de dialogue.

```cs
private async void ComposeSms(Windows.ApplicationModel.Contacts.Contact recipient,
    string messageBody,
    StorageFile attachmentFile,
    string mimeType)
{
    var chatMessage = new Windows.ApplicationModel.Chat.ChatMessage();
    chatMessage.Body = messageBody;

    if (attachmentFile != null)
    {
        var stream = Windows.Storage.Streams.RandomAccessStreamReference.CreateFromFile(attachmentFile);

        var attachment = new Windows.ApplicationModel.Chat.ChatMessageAttachment(
            mimeType,
            stream);

        chatMessage.Attachments.Add(attachment);
    }

    var phone = recipient.Phones.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactPhone>();
    if (phone != null)
    {
        chatMessage.Recipients.Add(phone.Number);
    }
    await Windows.ApplicationModel.Chat.ChatMessageManager.ShowComposeSmsMessageAsync(chatMessage);
}
```

Vous pouvez utiliser le code suivant pour déterminer si l’appareil qui exécute votre application est en mesure d’envoyer des messages SMS.

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.ApplicationModel.Chat"))
{
   // Call code here.
}
```

## <a name="summary-and-next-steps"></a>Récapitulatif et étapes suivantes

Cette rubrique vous a montré comment lancer la boîte de dialogue de rédaction d’un message SMS. Pour plus d’informations sur la sélection de contacts en tant que destinataires d’un message SMS, voir la section [Sélectionner des contacts](selecting-contacts.md). Téléchargez les [Exemples d’applications Windows universelles](https://go.microsoft.com/fwlink/p/?linkid=619979) dans GitHub pour voir d’autres exemples illustrant comment envoyer et recevoir des messages SMS à l’aide d’une tâche en arrière-plan.

## <a name="related-topics"></a>Rubriques connexes

* [Sélectionnez des contacts](selecting-contacts.md)
