---
author: mijacobs
Description: Les notifications toast adaptatives et interactives contextuelles et flexibles (plus de contenu, des images incluses/une interaction utilisateur facultatives).
title: Notifications toast adaptatives et interactives
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Adaptive and interactive toast notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: c8e77773b9118c3177dc958ddc7b51d32a452fa5
ms.sourcegitcommit: 9d1ca16a7edcbbcae03fad50a4a10183a319c63a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/09/2017
---
# <a name="adaptive-and-interactive-toast-notifications"></a>Notifications toast adaptatives et interactives

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Les notifications toast adaptatives et interactives vous permettent de créer des notifications flexibles dotées de texte, d’images et de boutons/entrées.

> **API importante**: [package NuGet UWP Community Toolkit Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

> [!NOTE]
> Pour découvrir les modèles hérités de Windows8.1 et de Windows Phone8.1, consultez le [catalogue de modèles de toast hérités](https://msdn.microsoft.com/library/windows/apps/hh761494).


## <a name="getting-started"></a>Prise en main

**Installez la bibliothèque Notifications.** Si vous préférez utiliserC# plutôt queXML pour générer les notifications, installez le packageNuGet [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) (recherchez «notifications uwp». Les exemples de code C# indiqués dans cet article utilisent la version1.0.0 du package NuGet.

**Installez Notifications Visualizer.** Cette application UWP gratuite vous permet de concevoir des notifications toast adaptatives en affichant un aperçu instantané de votre notification toast lorsque vous la modifiez, comme dans la vue de conception et l’éditeur XAML de VisualStudio. Pour plus d’informations, lisez [ce billet de blog](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/09/22/introducing-notifications-visualizer-for-windows-10.aspx). Vous pouvez également télécharger NotificationsVisualizer [ici](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1).


## <a name="sending-a-toast-notification"></a>Envoi d’une notification toast

Pour découvrir comment envoyer une notification, voir [Envoyer une notification toast locale](tiles-and-notifications-send-local-toast.md). Cette documentation ne couvre que la création de contenu de toast.


## <a name="toast-notification-structure"></a>Structure de notification toast

Les notifications toast sont une combinaison de certaines propriétés de données comme Tag/Group (qui vous permettent d’identifier la notification) et du *contenu du toast*.

Principaux composants du contenu du toast:
* **lancement**: définit les arguments qui sont transmis à nouveau à votre application lorsque l’utilisateur clique sur votre toast, vous permettant ainsi de créer un lien ciblé dans le contenu approprié affiché par le toast. Pour plus d’informations, voir [Envoyer une notification toast locale](tiles-and-notifications-send-local-toast.md).
* **élément visuel**: partie visuelle du toast, comprenant la liaison générique qui contient le texte, les images et les logos d’application.
* **actions**: partie interactive du toast, comprenant les entrées et les actions.
* **audio**: contrôle le son qui est lu lorsque le toast est affiché à l’utilisateur.

Le contenu de toast est défini dans le code XML brut, mais vous pouvez utiliser notre [bibliothèque NuGet](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) pour obtenir un modèle d’objet C# (ou C++) pour construire le contenu de toast. Cet article documente tout ce qui se passe dans le contenu de toast.

```csharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric() { ... }
    },
 
    Actions = new ToastActionsCustom() { ... },
 
    Audio = new ToastAudio() { ... }
};
```

```xml
<toast launch="app-defined-string">

  <visual>
    <binding template="ToastGeneric">
      ...
    </binding>
  </visual>

  <actions>
    ...
  </actions>

  <audio src="ms-winsoundevent:Notification.Reminder"/>

</toast>
```

Voici une représentation visuelle du contenu du toast:

![structure de notification toast](images/adaptivetoasts-structure.jpg)


## <a name="visual"></a>Élément visuel

Chaque toast doit spécifier un élément visuel, où vous devez fournir une liaison de toast générique, qui peut contenir un texte, des images, des logos, etc. Ces éléments sont rendus sur différents appareils Windows, notamment des ordinateurs, des téléphones, des tablettes et la Xbox.

Pour découvrir tous les attributs pris en charge dans la section Élément visuel et tous les éléments enfants de cette dernière, [voir la documentation sur le schéma](tiles-and-notifications-toast-schema.md#toastvisual).

L’identité de votre application sur la notification toast est acheminée via l’icône de votre application. Toutefois, si vous utilisez le remplacement de logo d’application, nous affichons le nom de votre application au-dessous de vos lignes de texte.

| Notification toast normale                                                                              | Notification toast avec appLogoOverride                                                          |
| ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| ![notification sans appLogoOverride](images/adaptivetoasts-withoutapplogooverride.jpg) | ![notification avec appLogoOverride](images/adaptivetoasts-withapplogooverride.jpg) |


## <a name="text-elements"></a>Éléments de texte

Chaque toast doit contenir au moins un élément de texte, et peut en comporter deuxautres, tous étant de type [AdaptiveText](tiles-and-notifications-toast-schema.md#adaptivetext).

![Toast pourvu d’un titre et d’une description](images/toast-title-and-description.jpg)

Depuis la Mise à jour anniversaire, vous pouvez contrôler le nombre de lignes de texte affichées à l’aide de la propriété **HintMaxLines** dans le texte. Par défaut, le titre s’affiche sur 2lignes de texte au plus, et la description s’affiche sur 4lignes de texte au plus.

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        new AdaptiveText()
        {
            Text = "Adaptive Tiles Meeting",
            HintMaxLines = 1
        },

        new AdaptiveText()
        {
            Text = "Conf Room 2001 / Building 135"
        },

        new AdaptiveText()
        {
            Text = "10:00 AM - 10:30 AM"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    <text hint-maxLines="1">Adaptive Tiles Meeting</text>
    <text>Conf Room 2001 / Building 135</text>
    <text>10:00 AM - 10:30 AM</text>
</binding>
```


## <a name="app-logo-override"></a>Remplacement du logo d’application

Par défaut, votre toast affiche le logo de votre application. Toutefois, vous pouvez remplacer ce logo par votre propre image [ToastGenericAppLogo](tiles-and-notifications-toast-schema.md#toastgenericapplogo). Par exemple, s’il s’agit d’une notification provenant d’une personne, nous vous recommandons de remplacer le logo de l’application par une image de cette personne.

![Toast avec remplacement du logo d’application](images/toast-applogooverride.jpg)

Vous pouvez utiliser la propriété **HintCrop** pour modifier le rognage de l’image. Par exemple, *circle* entraîne une image rognée en forme de cercle. Sinon, l’image est carrée, et ses dimensions sont de 64x64pixels à une mise à l’échelle à 100%.

```csharp
new ToastBindingGeneric()
{
    ...

    AppLogoOverride = new ToastGenericAppLogo()
    {
        Source = "https://unsplash.it/64?image=883",
        HintCrop = ToastGenericAppLogoCrop.Circle
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="appLogoOverride" hint-crop="circle" src="https://unsplash.it/64?image=883"/>
</binding>
```


## <a name="hero-image"></a>Image Hero

**Nouveauté de la Mise à jour anniversaire**: les toasts peuvent afficher une image Hero, qui est une image [ToastGenericHeroImage](tiles-and-notifications-toast-schema.md#toastgenericheroimage) recommandée, affichée en évidence avec la bannière de toast et dans le centre de notifications. Les dimensions de l’image sont de 360x180pixels à une mise à l’échelle à 100%.

![Toast avec image Hero](images/toast-heroimage.jpg)

```csharp
new ToastBindingGeneric()
{
    ...

    HeroImage = new ToastHeroImage()
    {
        Source = "https://unsplash.it/360/180?image=1043"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="hero" src="https://unsplash.it/360/180?image=1043"/>
</binding>
```


## <a name="inline-image"></a>Image incorporée

Vous pouvez fournir une image incorporée de pleine largeur qui s’affiche lorsque vous développez le toast.

![Toast avec une image supplémentaire](images/toast-additionalimage.jpg)

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveImage()
        {
            Source = "https://unsplash.it/360/180?image=1043"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image src="https://unsplash.it/360/180?image=1043" />
</binding>
```


## <a name="attribution-text"></a>Texte d’attribution

**Nouveauté de la Mise à jour anniversaire**: si vous devez référencer la source de votre contenu, vous pouvez utiliser un texte d’attribution. Ce texte est toujours affiché en bas de votre notification avec l’identité de votre application ou l’horodatage de la notification.

Sur les versions antérieures de Windows qui ne prennent pas en charge le texte d’attribution, le texte est simplement affiché en tant qu’autre élément de texte (en supposant que vous ne disposez pas encore du nombre maximal de troiséléments de texte).

![Toast avec texte d’attribution](images/toast-attributiontext.jpg)

```csharp
new ToastBindingGeneric()
{
    ...

    Attribution = new ToastGenericAttributionText()
    {
        Text = "Via SMS"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <text placement="attribution">Via SMS</text>
</binding>
```


## <a name="custom-timestamp"></a>Horodatage personnalisé

**Nouveauté de CreatorsUpdate**: vous pouvez maintenant remplacer l’horodatage système par votre propre horodatage qui représente avec précision le moment auquel les message/informations/contenu ont été générés. L’horodatage est visible dans le centre de notifications.

![Toast avec horodatage personnalisé](images/toast-customtimestamp.jpg)

Pour en savoir plus sur l’utilisation d’un horodatage personnalisé, [consultez ce billet de blog](https://blogs.msdn.microsoft.com/tiles_and_toasts/2017/01/09/custom-timestamp-on-toast-notifications-windows-10-creators-update/).

```csharp
ToastContent toastContent = new ToastContent()
{
    DisplayTimestamp = new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc),
    ...
};
```

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```


## <a name="adaptive-content"></a>Contenu adaptatif

**Nouveauté de la Mise à jour anniversaire**: outre le contenu spécifié ci-dessus, vous pouvez afficher le contenu adaptatif supplémentaire qui est visible lorsque le toast est développé.

Ce contenu supplémentaire est spécifié à l’aide de l’élément Adaptive, sur lequel vous pouvez en savoir plus en lisant la [documentation sur les vignettes adaptatives](tiles-and-notifications-create-adaptive-tiles.md).

Notez que tout contenu adaptatif doit être inséré dans un élément AdaptiveGroup, sinon il n’est pas rendu de façon adaptative.


### <a name="columns-and-text-elements"></a>Colonnes et éléments de texte

Voici un exemple illustrant l’utilisation de colonnes et de certains éléments de texte adaptatifs avancés. Comme les éléments de texte sont insérés dans un élément AdaptiveGroup, ils prennent en charge toutes les propriétés de style adaptatif enrichi.

![Toast pourvu d’un texte supplémentaire](images/toast-additionaltext.jpg)

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveGroup()
        {
            Children =
            {
                new AdaptiveSubgroup()
                {
                    Children =
                    {
                        new AdaptiveText()
                        {
                            Text = "52 attendees",
                            HintStyle = AdaptiveTextStyle.Base
                        },
                        new AdaptiveText()
                        {
                            Text = "23 minute drive",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle
                        }
                    }
                },
                new AdaptiveSubgroup()
                {
                    Children =
                    {
                        new AdaptiveText()
                        {
                            Text = "1 Microsoft Way",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle,
                            HintAlign = AdaptiveTextAlign.Right
                        },
                        new AdaptiveText()
                        {
                            Text = "Bellevue, WA 98008",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle,
                            HintAlign = AdaptiveTextAlign.Right
                        }
                    }
                }
            }
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <group>
        <subgroup>
            <text hint-style="base">52 attendees</text>
            <text hint-style="captionSubtle">23 minute drive</text>
        </subgroup>
        <subgroup>
            <text hint-style="captionSubtle" hint-align="right">1 Microsoft Way</text>
            <text hint-style="captionSubtle" hint-align="right">Bellevue, WA 98008</text>
        </subgroup>
    </group>
</binding>
```


## <a name="inputs-and-buttons"></a>Entrées et boutons

Les entrées et boutons sont spécifiés dans la région Actions du toast, ce qui signifie qu’ils ne sont visibles que lorsque le toast est développé.


### <a name="quick-reply-text-box"></a>Zone de texte de réponse rapide

Pour activer une zone de texte de réponse rapide, par exemple pour un scénario de messagerie, ajoutez une entrée de texte et un bouton, et référencez l’ID de l’entrée de texte afin que le bouton soit affiché en regard de l’entrée.

![Notification avec entrée de texte et actions](images/adaptivetoasts-xmlsample05.jpg)

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("tbReply")
            {
                PlaceholderContent = "Type a reply"
            }
        },

        Buttons =
        {
            new ToastButton("Reply", "action=reply&convId=9318")
            {
                ActivationType = ToastActivationType.Background,

                // To place the button next to the text box,
                // reference the text box's Id and provide an image
                TextBoxId = "tbReply",
                ImageUri = "Assets/Reply.png"
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeholderContent="Type a reply"/>

        <action
            content="Send",
            arguments="action=reply&amp;convId=9318"
            activationType="background"
            hint-inputId="textBox"
            imageUri="Assets/Reply.png"/>

    </actions>

</toast>
```


### <a name="inputs-with-buttons-bar"></a>Entrées avec la barre de boutons

Vous pouvez également avoir une ou plusieurs entrées pourvues de boutons normaux affichés au-dessous des entrées.

![Notification avec texte et actions d’entrée](images/adaptivetoasts-xmlsample04.jpg)

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("tbReply")
            {
                PlaceholderContent = "Type a reply"
            }
        },

        Buttons =
        {
            new ToastButton("Reply", "action=reply&threadId=9218")
            {
                ActivationType = ToastActivationType.Background
            },

            new ToastButton("Video call", "action=videocall&threadId=9218")
            {
                ActivationType = ToastActivationType.Foreground
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeholderContent="Type a reply"/>

        <action
            content="Reply",
            arguments="action=reply&amp;threadId=9218"
            activationType="background"/>

        <action
            content="Video call",
            arguments="action=videocall&amp;threadId=9218"
            activationType="foreground"/>

    </actions>

</toast>
```


### <a name="selection-input"></a>Entrée de sélection

Outre les zones de texte, vous pouvez utiliser un menu de sélection.

![Notification avec entrée de sélection et actions](images/adaptivetoasts-xmlsample06.jpg)

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("time")
            {
                DefaultSelectionBoxItemId = "lunch",
                Items =
                {
                    new ToastSelectionBoxItem("breakfast", "Breakfast"),
                    new ToastSelectionBoxItem("lunch", "Lunch"),
                    new ToastSelectionBoxItem("dinner", "Dinner")
                }
            }
        },

        Buttons = { ... }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="time" type="selection" defaultInput="lunch">
            <selection id="breakfast" content="Breakfast" />
            <selection id="lunch" content="Lunch" />
            <selection id="dinner" content="Dinner" />
        </input>

        ...

    </actions>

</toast>
```


## <a name="buttons"></a>Boutons

Les boutons rendent votre toast interactif, ce qui permet à l’utilisateur d’effectuer des actions rapides dans votre notification toast sans interrompre son flux de travail en cours. Par exemple, les utilisateurs peuvent répondre à un message directement à partir d’un toast, ou supprimer un message électronique sans même ouvrir l’application de messagerie.

Les boutons peuvent effectuer les différentes actions suivantes...

-   Activation de l’application au premier plan à l’aide d’un argument qui permet d’accéder à une page ou à un contexte spécifiques
-   Activation de la tâche en arrière-plan de l’application pour un scénario de réponse rapide ou similaire
-   Activation d’une autre application par le biais d’un lancement par protocole
-   Exécution d’une action système, comme la répétition ou le masquage de la notification

Notez que vous ne pouvez afficher que 5boutons au plus (y compris les éléments de menu contextuel dont nous parlons ultérieurement).

![Notification avec des actions, exemple1](images/adaptivetoasts-xmlsample02.jpg)

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("See more details", "action=viewdetails&contentId=351")
            {
                ActivationType = ToastActivationType.Foreground
            },

            new ToastButton("Remind me later", "action=remindlater&contentId=351")
            {
                ActivationType = ToastActivationType.Background
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <action
            content="See more details",
            arguments="action=viewdetails&amp;contentId=351"
            activationType="foreground"/>

        <action
            content="Remind me later",
            arguments="action=remindlater&amp;contentId=351"
            activationType="background"/>

    </actions>

</toast>
```


### <a name="snoozedismiss-buttons"></a>Boutons Répéter/Ignorer

En utilisant un menu de sélection et deuxboutons, nous pouvons créer une notification de rappel qui utilise les actions système de répétition et de masquage. Veillez à définir le scénario sur Rappel pour que la notification se comporte comme un rappel.

![Notification de rappel](images/adaptivetoasts-xmlsample07.jpg)

Nous lions le bouton Répéter à l’entrée du menu de sélection à l’aide de la propriété *SelectionBoxId* du bouton de toast.

```csharp
ToastContent content = new ToastContent()
{
    Scenario = ToastScenario.Reminder,

    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("snoozeTime")
            {
                DefaultSelectionBoxItemId = "15",
                Items =
                {
                    new ToastSelectionBoxItem("5", "5 minutes"),
                    new ToastSelectionBoxItem("15", "15 minutes"),
                    new ToastSelectionBoxItem("60", "1 hour"),
                    new ToastSelectionBoxItem("240", "4 hours"),
                    new ToastSelectionBoxItem("1440", "1 day")
                }
            }
        },

        Buttons =
        {
            new ToastButtonSnooze()
            {
                SelectionBoxId = "snoozeTime"
            },
 
            new ToastButtonDismiss()
        }
    }
};
```

```xml
<toast scenario="reminder" launch="action=viewEvent&amp;eventId=1983">
   
  ...
 
  <actions>
     
    <input id="snoozeTime" type="selection" defaultInput="15">
      <selection id="1" content="1 minute"/>
      <selection id="15" content="15 minutes"/>
      <selection id="60" content="1 hour"/>
      <selection id="240" content="4 hours"/>
      <selection id="1440" content="1 day"/>
    </input>
 
    <action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content="" />
 
    <action activationType="system" arguments="dismiss" content=""/>
     
  </actions>
   
</toast>
```

Pour utiliser les actions système de répétition et de masquage, procédez comme suit:

-   Spécifiez un élément ToastButtonSnooze ou ToastButtonDismiss.
-   Spécifiez éventuellement une chaîne de contenu personnalisé:
    -   Si vous n’indiquez pas une chaîne, nous utilisons automatiquement les chaînes localisées pour «Répéter» et «Ignorer».
-   Spécifiez éventuellement la propriété *SelectionBoxId*:
    -   Si vous ne voulez pas que l’utilisateur sélectionne un intervalle de répétition, mais souhaitez simplement que votre notification se répète une seule fois pendant un intervalle de temps défini par le système (et cohérent dans l’ensemble du système d’exploitation), ne construisez aucun élément &lt;input&gt;.
    -   Si vous voulez fournir des sélections d’intervalle de répétition:
        -   Spécifiez la propriété *SelectionBoxId* dans l’action de répétition.
        -   Faites correspondre l’ID de l’entrée à la propriété *SelectionBoxId* de l’action de répétition.
        -   Spécifiez la valeur de l’élément *ToastSelectionBoxItem* sur un entier nonNegativeInteger qui représente l’intervalle de répétition en minutes.



## <a name="audio"></a>Audio

L’option audio personnalisée est toujours prise en charge par les appareils mobiles, ainsi que par Desktop Version 1511 (build 10586) ou une version plus récente. L’option audio personnalisée peut être référencée via les chemins d’accès suivants:

-   ms-appx:///
-   ms-appdata:///

Sinon, vous pouvez choisir dans la [liste des sons ms-winsoundevents](https://msdn.microsoft.com/library/windows/apps/br230842), prise en charge par les deux plateformes.

```csharp
ToastContent content = new ToastContent()
{
    ...

    Audio = new ToastAudio()
    {
        Src = new Uri("ms-appx:///Assets/NewMessage.mp3")
    }
}
```

```xml
<toast launch="app-defined-string">

    ...

    <audio src="ms-appx:///Assets/NewMessage.mp3"/>

</toast>
```

Voir la [page de schéma audio](https://msdn.microsoft.com/library/windows/apps/br230842) pour plus d’informations sur les options audio dans les notifications toast. Pour savoir comment envoyer un toast en utilisant une option audio personnalisée, [voir ce blog](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/06/18/quickstart-sending-a-toast-notification-with-custom-audio/).


## <a name="alarms-reminders-and-incoming-calls"></a>Alarmes, rappels et appels entrants

Pour créer des alarmes, des rappels et des notifications d’appels entrants, vous utilisez simplement une notification toast normale avec une valeur de scénario qui lui est attribuée. Le scénario ajuste quelques comportements pour créer une expérience utilisateur cohérente et unifiée.

* **Rappel**: une notification reste affichée à l’écran jusqu’à ce que l’utilisateur la masque ou exécute une action. Sur WindowsMobile, le toast s’affiche également sous sa forme pré-développée. Un signal sonore de rappel est lu.
* **Alarme**: outre les comportements de rappel, les alarmes exécutent en boucle un son supplémentaire avec un son d’alarme par défaut.
* **IncomingCall**: les notifications d’appels entrants s’affichent en plein écran sur les appareils WindowsMobile. Dans le cas contraire, elles ont les mêmes comportements que les alarmes, sauf qu’elles utilisent une sonnerie.

```csharp
ToastContent content = new ToastContent()
{
    Scenario = ToastScenario.Reminder,

    ...
}
```

```xml
<toast scenario="reminder" launch="app-defined-string">

    ...

</toast>
```


## <a name="handling-activation"></a>Gestion de l’activation
Pour découvrir comment gérer les activations de toast (l’utilisateur cliquant sur votre toast ou sur les boutons du toast), voir [Envoyer une notification toast locale](tiles-and-notifications-send-local-toast.md).


 
## <a name="related-topics"></a>Rubriques connexes

* [Envoyer une notification toast locale](tiles-and-notifications-send-local-toast.md)
* [Bibliothèque Notifications sur GitHub](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)