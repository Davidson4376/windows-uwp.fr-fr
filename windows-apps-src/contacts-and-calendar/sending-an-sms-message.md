---
Cette rubrique vous montre comment lancer la boîte de dialogue de rédaction d’un message SMS pour permettre à l’utilisateur d’envoyer un message SMS. Vous pouvez préremplir les champs du message SMS avec des données avant d’afficher la boîte de dialogue. Le message ne sera pas envoyé tant que l’utilisateur n’aura pas appuyé sur le bouton d’envoi.
Envoyer un message SMS
ms.assetid: 4D7B509B-1CF0-4852-9691-E96D8352A4D6
Mots clés : contacts, SMS, envoi
---

# Envoyer un message SMS

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Cette rubrique vous montre comment lancer la boîte de dialogue de rédaction d’un message SMS pour permettre à l’utilisateur d’envoyer un message SMS. Vous pouvez préremplir les champs du message SMS avec des données avant d’afficher la boîte de dialogue. Le message ne sera pas envoyé tant que l’utilisateur n’aura pas appuyé sur le bouton d’envoi.

## Lancer la boîte de dialogue de rédaction d’un message SMS

Créez un objet [**ChatMessage**](https://msdn.microsoft.com/library/windows/apps/Dn642160) et définissez les données à préremplir dans la boîte de dialogue de rédaction d’un message SMS. Appelez [**ShowComposeSmsMessageAsync**](https://msdn.microsoft.com/library/windows/apps/Dn642160manager-showcomposesmsmessageasync) pour afficher la boîte de dialogue.

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

    var phone = recipient.Phones.FirstOrDefault&lt;Windows.ApplicationModel.Contacts.ContactPhone&gt;();
    if (phone != null)
    {
        chatMessage.Recipients.Add(phone.Number);
    }
    await Windows.ApplicationModel.Chat.ChatMessageManager.ShowComposeSmsMessageAsync(chatMessage);
}
```

## Récapitulatif et étapes suivantes

Cette rubrique vous a montré comment lancer la boîte de dialogue de rédaction d’un message SMS. Pour plus d’informations sur la sélection de contacts en tant que destinataires d’un message SMS, voir la section [Sélectionner des contacts](selecting-contacts.md). Téléchargez les [Exemples d’applications Windows universelles](http://go.microsoft.com/fwlink/p/?linkid=619979) dans GitHub pour voir d’autres exemples illustrant comment envoyer et recevoir des messages SMS à l’aide d’une tâche en arrière-plan.

## Rubriques connexes

* [Sélectionner des contacts](selecting-contacts.md)




<!--HONumber=Mar16_HO1-->


