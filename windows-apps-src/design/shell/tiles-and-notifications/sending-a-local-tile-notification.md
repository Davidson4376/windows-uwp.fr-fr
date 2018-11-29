---
Description: This article describes how to send a local tile notification to a primary tile and a secondary tile using adaptive tile templates.
title: Envoyer une notification par vignette locale
ms.assetid: D34B0514-AEC6-4C41-B318-F0985B51AF8A
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5752a7bf18d785121258ea3fe75afe8383be2aff
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8196918"
---
# <a name="send-a-local-tile-notification"></a>Envoyer une notification par vignette locale
 

Vignettes d’application principale de Windows 10 sont définies dans le manifeste de votre application, tandis que les vignettes secondaires sont par programme créés et définis par le code de votre application. Cet article décrit comment envoyer une notification par vignette locale à une vignette principale et une vignette secondaire à l’aide de modèles de vignette adaptative. (Une notification locale est une notification envoyée à partir du code d’application, par opposition à une notification qui fait l’objet d’une transmission de type push ou pull à partir d’un serveur web).

![Vignette par défaut et vignette avec notification](images/sending-local-tile-01.png)

> [!NOTE] 
>Pour plus d’informations, voir [Créer des vignettes adaptatives](create-adaptive-tiles.md) et [Schéma du contenu de la vignette](../tiles-and-notifications/tile-schema.md).

 

## <a name="install-the-nuget-package"></a>Installation du package NuGet


Nous vous recommandons d’installer le [package NuGet de la bibliothèque Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) qui simplifie les choses en générant les charges utiles de vignettes à l’aide d’objets au lieu de code XML brut.

Les exemples de code inclus dans cet article utilisent C# et la bibliothèque Notifications. (Si vous préférez créer votre propre code XML, vous trouverez des exemples de code sans la bibliothèque Notifications vers la fin de l’article.)

## <a name="add-namespace-declarations"></a>Ajouter des déclarations d’espace de noms


Pour accéder aux API de vignette, incluez l’espace de noms [**Windows.UI.Notifications**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications). Nous vous recommandons également d’inclure l’espace de noms **Microsoft.Toolkit.Uwp.Notifications** pour tirer parti de nos API d’assistance de vignette (vous devez installer le package NuGet de la [bibliothèque Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) pour accéder à ces API).

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```

## <a name="create-the-notification-content"></a>Créer le contenu d’une notification


Dans Windows 10, les charges utiles de vignettes sont définies à l’aide de modèles de vignette adaptative qui vous permettent de créer des dispositions visuelles personnalisées pour vos notifications. (Pour savoir ce qu’il est possible de réaliser avec les vignettes adaptatives, voir [Créer des vignettes adaptatives](create-adaptive-tiles.md).)

Cet exemple de code crée le contenu de vignette adaptative pour les vignettes moyennes et larges.

```csharp
// In a real app, these would be initialized with actual data
string from = "Jennifer Parker";
string subject = "Photos from our trip";
string body = "Check out these awesome photos I took while in New Zealand!";
 
 
// Construct the tile content
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = from
                    },

                    new AdaptiveText()
                    {
                        Text = subject,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },

                    new AdaptiveText()
                    {
                        Text = body,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        },

        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = from,
                        HintStyle = AdaptiveTextStyle.Subtitle
                    },

                    new AdaptiveText()
                    {
                        Text = subject,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },

                    new AdaptiveText()
                    {
                        Text = body,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        }
    }
};
```

Le contenu de la notification se présente comme suit pour une vignette moyenne:

![Contenu de la notification sur une vignette moyenne](images/sending-local-tile-02.png)

## <a name="create-the-notification"></a>Créer la notification


Une fois que vous avez le contenu de votre notification, vous devez créer un élément [**TileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification). Le constructeur **TileNotification** prend un objet Windows Runtime [**XmlDocument**](https://docs.microsoft.com/uwp/api/windows.data.xml.dom.xmldocument) que vous pouvez obtenir de la méthode **TileContent.GetXml** si vous utilisez la [bibliothèque Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/).

Cet exemple de code crée une notification pour une nouvelle vignette.

```csharp
// Create the tile notification
var notification = new TileNotification(content.GetXml());
```

## <a name="set-an-expiration-time-for-the-notification-optional"></a>Définir un délai d’expiration pour la notification (facultatif)


Par défaut, les notifications locales par vignette et de badge n’expirent pas, alors que les notifications Push, périodiques et planifiées expirent après trois jours. Étant donné que le contenu de la vignette ne doit pas être conservé plus longtemps que nécessaire, il est recommandé de définir un délai d’expiration approprié pour votre application, en particulier sur les notifications locales par vignette et de badge.

Cet exemple de code crée une notification qui arrive à expiration et est supprimée de la vignette au bout de 10minutes.

```csharp
tileNotification.ExpirationTime = DateTimeOffset.UtcNow.AddMinutes(10);
```

## <a name="send-the-notification"></a>Envoyer la notification


Bien que l’envoi local d’une notification par vignette soit simple, l’envoi de la notification à une vignette principale ou secondaire est légèrement différent.

**Vignette principale**

Pour envoyer une notification à une vignette principale, utilisez le [**TileUpdateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdateManager) pour créer une mise à jour de vignette pour la vignette principale et envoyez la notification en appelant la méthode «Update». Qu’elle soit visible ou non, la vignette principale de votre application existe toujours ; vous pouvez donc lui envoyer des notifications même si elle n’est pas épinglée. Si l’utilisateur épingle votre vignette principale ultérieurement, les notifications que vous avez envoyées seront alors affichées.

Cet exemple de code envoie une notification à une vignette principale.


```csharp
// Send the notification to the primary tile
TileUpdateManager.CreateTileUpdaterForApplication().Update(notification);
```

**Vignette secondaire**

Pour envoyer une notification à une vignette secondaire, assurez-vous d’abord que la vignette secondaire existe. Si vous essayez de créer une mise à jour pour une vignette secondaire qui n’existe pas (par exemple, si l’utilisateur a désépinglé la vignette secondaire), une exception est levée. Vous pouvez utiliser [**SecondaryTile.Exists**](https://docs.microsoft.com/uwp/api/Windows.UI.StartScreen.SecondaryTile#Windows_UI_StartScreen_SecondaryTile_Exists_System_String_)(tileId) pour détecter si votre vignette secondaire est épinglée, puis créer une mise à jour pour la vignette secondaire et envoyer la notification.

Cet exemple de code envoie une notification à une vignette secondaire.

```csharp
// If the secondary tile is pinned
if (SecondaryTile.Exists("MySecondaryTile"))
{
    // Get its updater
    var updater = TileUpdateManager.CreateTileUpdaterForSecondaryTile("MySecondaryTile");
 
    // And send the notification
    updater.Update(notification);
}
```

![Vignette par défaut et vignette avec notification](images/sending-local-tile-01.png)

## <a name="clear-notifications-on-the-tile-optional"></a>Effacer des notifications sur la vignette (facultatif)


Dans la plupart des cas, vous devez effacer une notification une fois que l’utilisateur a interagi avec ce contenu. Par exemple, lorsque l’utilisateur lance votre application, il peut être judicieux d’effacer toutes les notifications de la vignette. Si vos modifications sont temporaires, nous vous recommandons de définir un délai d’expiration sur la notification au lieu de supprimer celle-ci explicitement.

Cet exemple de code supprime la notification de la vignette principale. Vous pouvez effectuer la même opération pour les vignettes secondaires en créant un outil de mise à jour pour la vignette secondaire.

```csharp
TileUpdateManager.CreateTileUpdaterForApplication().Clear();
```

Si la file d’attente est activée pour une vignette et qu’il y a des notifications en attente, l’appel de la méthode Clear a pour effet de vider la file d’attente. Vous ne pouvez pas, cependant, effacer une notification via un serveur de votre application ; seul le code d’application local peut effacer des notifications.

Les notifications périodiques ou Push peuvent uniquement ajouter de nouvelles notifications ou remplacer les notifications existantes. Un appel local à la méthode Clear effacera la vignette, que les notifications aient été fournies ou non par le biais push, périodique ou local. Les notifications planifiées qui n’ont pas encore été affichées ne sont pas effacées par cette méthode.

![Vignette avec notification et vignette après effacement](images/sending-local-tile-03.png)

## <a name="next-steps"></a>Étapes suivantes


**Utilisation de la file d’attente de notifications**

Maintenant que vous avez réalisé votre première mise à jour de vignette, vous pouvez développer les fonctionnalités de la vignette en activant une [file d’attente de notifications](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234).

**Autres modes de remise des notifications**

Cet article vous montre comment envoyer la mise à jour de vignette sous forme de notification. Pour découvrir d’autres modes de remise de notification, y compris les notifications planifiées, périodiques et Push, voir [Remise de notifications](choosing-a-notification-delivery-method.md).

**Méthode de remise XmlEncode**

Si vous n’utilisez pas la [bibliothèque Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), ce mode de remise de notification représente une autre solution.


```csharp
public string XmlEncode(string text)
{
    StringBuilder builder = new StringBuilder();
    using (var writer = XmlWriter.Create(builder))
    {
        writer.WriteString(text);
    }

    return builder.ToString();
}
```

## <a name="code-examples-without-notifications-library"></a>Exemples de code sans la bibliothèque Notifications


Si vous préférez utiliser du code XML brut à la place du package NuGet de la [bibliothèque Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), utilisez ces autres exemples de code pour les troispremiers exemples fournis dans cet article. Les exemples de code restants peuvent être utilisés avec la [bibliothèque Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) ou du codeXML brut.

Ajouter des déclarations d’espace de noms

```csharp
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
```

Créer le contenu d’une notification

```csharp
// In a real app, these would be initialized with actual data
string from = "Jennifer Parker";
string subject = "Photos from our trip";
string body = "Check out these awesome photos I took while in New Zealand!";
 
 
// TODO - all values need to be XML escaped
 
 
// Construct the tile content as a string
string content = $@"
<tile>
    <visual>
 
        <binding template='TileMedium'>
            <text>{from}</text>
            <text hint-style='captionSubtle'>{subject}</text>
            <text hint-style='captionSubtle'>{body}</text>
        </binding>
 
        <binding template='TileWide'>
            <text hint-style='subtitle'>{from}</text>
            <text hint-style='captionSubtle'>{subject}</text>
            <text hint-style='captionSubtle'>{body}</text>
        </binding>
 
    </visual>
</tile>";
```

Créer la notification

```csharp
// Load the string into an XmlDocument
XmlDocument doc = new XmlDocument();
doc.LoadXml(content);
 
// Then create the tile notification
var notification = new TileNotification(doc);
```

## <a name="related-topics"></a>Rubriques connexes

* [Créer des vignettes adaptatives](create-adaptive-tiles.md)
* [Schéma du contenu de la vignette](../tiles-and-notifications/tile-schema.md)
* [Bibliothèque Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)
* [Exemple de code complet sur GitHub](https://github.com/WindowsNotifications/quickstart-sending-local-tile-win10)
* [**Espace de noms Windows.UI.Notifications**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications)
* [Comment utiliser la file d’attente de notifications (XAML)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234)
* [Remise de notifications](choosing-a-notification-delivery-method.md)
 

 




