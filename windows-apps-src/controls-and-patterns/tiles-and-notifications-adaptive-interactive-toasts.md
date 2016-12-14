---
author: mijacobs
Description: Les notifications toast adaptatives et interactives contextuelles et flexibles (plus de contenu, des images incluses/une interaction utilisateur facultatives).
title: Notifications toast adaptatives et interactives
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Adaptive and interactive toast notifications
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 2ac3a4a1efa85a3422d8964ad4ee62db28bc975f
ms.openlocfilehash: cfbbf110ed6df1b7e81e0505dcf55a63ba8739aa

---
# <a name="adaptive-and-interactive-toast-notifications"></a>Notifications toast adaptatives et interactives

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Les notifications toast adaptatives et interactives vous permettent de créer des notifications contextuelles flexibles présentant davantage de contenu, ainsi que des images incluses et une interaction utilisateur facultatives.

Le modèle de notifications toast adaptatives et interactives comporte les mises à jour ci-après par rapport au catalogue de modèles de notifications toast hérité :

-   possibilité d’inclure des boutons et des entrées sur les notifications ;
-   trois différents types d’activations pour la notification toast principale et pour chaque action ;
-   possibilité de créer une notification pour certains scénarios, comprenant les alarmes, les rappels et les appels entrants.

**Remarque** Pour découvrir les modèles hérités de Windows 8.1 et de Windows Phone 8.1, consultez le [catalogue de modèles de notifications toast hérités](https://msdn.microsoft.com/library/windows/apps/hh761494).


## <a name="getting-started"></a>Prise en main

**Installez la bibliothèque Notifications.** Si vous préférez utiliser C# plutôt que XML pour générer les notifications, installez le package NuGet [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) (recherchez « notifications uwp ». Les exemples de code C# indiqués dans cet article utilisent la version 1.0.0 du package NuGet.

**Installez Notifications Visualizer.** Cette application UWP gratuite vous permet de concevoir des notifications toast adaptatives en affichant un aperçu instantané de votre notification toast lorsque vous la modifiez, comme dans la vue de conception et l’éditeur XAML de Visual Studio. Pour plus d’informations, lisez [ce billet de blog](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/09/22/introducing-notifications-visualizer-for-windows-10.aspx). Vous pouvez également télécharger Notifications Visualizer [ici](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1).


## <a name="toast-notification-structure"></a>Structure de notification toast


Les notifications toast sont construites en XML et comprennent généralement les éléments clés suivants :

-   &lt;visual&gt; indique le contenu visible par les utilisateurs, incluant le texte et les images
-   &lt;actions&gt; contient les boutons/entrées que le développeur souhaite ajouter au sein de la notification
-   &lt;audio&gt; spécifie le son émis lorsque la notification apparaît

Voici un exemple de code :

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Sample</text>
      <text>This is a simple toast notification example</text>
      <image placement="AppLogoOverride" src="oneAlarm.png" />
    </binding>
  </visual>
  <actions>
    <action content="check" arguments="check" imageUri="check.png" />
    <action content="cancel" arguments="cancel" />
  </actions>
  <audio src="ms-winsoundevent:Notification.Reminder"/>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Sample"
                },
 
                new AdaptiveText()
                {
                    Text = "This is a simple toast notification example"
                }
            },
 
            AppLogoOverride = new ToastGenericAppLogo()
            {
                Source = "oneAlarm.png"
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("check", "check")
            {
                ImageUri = "check.png"
            },
 
            new ToastButton("cancel", "cancel")
            {
                ImageUri = "cancel.png"
            }
        }
    },
 
    Audio = new ToastAudio()
    {
        Src = new Uri("ms-winsoundevent:Notification.Reminder")
    }
};
```

Vous pouvez ensuite utiliser ce code pour créer et envoyer la notification toast :

```CSharp
ToastNotification notification = new ToastNotification(content.GetXml());
ToastNotificationManager.CreateToastNotifier().Show(notification);
```

Pour obtenir une application pleinement fonctionnelle qui affiche les notifications toast en action, consultez [Démarrage rapide pour l’envoi d’une notification toast locale](https://github.com/WindowsNotifications/quickstart-sending-local-toast-win10).

Et voici une représentation visuelle de la structure :

![structure de notification toast](images/adaptivetoasts-structure.jpg)

### <a name="visual"></a>Éléments visuels

L’élément « visual » doit contenir très exactement un seul élément de liaison (« binding ») intégrant le contenu visuel de la notification toast.

Les notifications par vignette dans les applications de plateforme Windows universelle (UWP) prennent en charge plusieurs modèles reposant sur différentes tailles de vignette. Toutefois, les notifications toast n’emploient qu’un seul nom de modèle : **ToastGeneric**. L’utilisation d’un seul nom de modèle signifie plusieurs choses :

-   Vous pouvez modifier le contenu des notifications toast, par exemple en ajoutant une autre ligne de texte ou une image incluse ou en modifiant le comportement de l’image miniature pour qu’elle affiche autre chose que l’icône de l’application, sans avoir besoin de modifier la totalité du modèle ni risquer de créer une charge utile incorrecte découlant d’une incohérence entre le nom du modèle et le contenu.
-   Vous pouvez utiliser le même code pour construire la charge utile d’une **notification toast** ciblant différents types d’appareils Microsoft Windows, tels que les téléphones, les tablettes, les PC et les systèmes Xbox One. Chacun de ces appareils acceptera la notification et la présentera à l’utilisateur conformément à ses stratégies d’interface utilisateur avec les affordances visuelles et le modèle d’interaction appropriés.

Pour découvrir tous les attributs pris en charge dans la section « visual » et tous les éléments enfants de cette dernière, voir la section « Schéma » ci-dessous. Pour consulter d’autres exemples, consultez la section « Exemples XML » ci-après.

L’icône de votre application véhicule l’identité de votre application. Toutefois, si vous utilisez appLogoOverride, nous affichons le nom de votre application sous vos lignes de texte.

| Notification toast normale                                                                              | Notification toast avec appLogoOverride                                                          |
| ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| ![notification sans appLogoOverride](images/adaptivetoasts-withoutapplogooverride.jpg) | ![notification avec appLogoOverride](images/adaptivetoasts-withapplogooverride.jpg) |

### <a name="actions"></a>Actions

Dans les applications UWP, vous pouvez ajouter des boutons et d’autres entrées à vos notifications toast, ce qui permet aux utilisateurs d’effectuer d’autres opérations à l’extérieur de l’application. Vous spécifiez ces actions sous l’élément &lt;actions&gt; en utilisant l’un des deux types d’actions possibles :

-   &lt;action&gt; Cet élément apparaît sous la forme d’un bouton sur les appareils de bureau et sur les appareils mobiles. Vous pouvez spécifier jusqu’à cinq actions personnalisées ou système dans une notification toast.
-   &lt;input&gt; Cet élément permet aux utilisateurs d’entrer des données, telles qu’une réponse rapide à un message, ou de sélectionner une option dans un menu déroulant.

Les éléments &lt;action&gt; et &lt;input&gt; sont tous deux adaptatifs dans la famille d’appareils Windows. Par exemple, sur les appareils mobiles ou de bureau, un élément &lt;action&gt; correspond à un bouton sur lequel l’utilisateur doit appuyer ou cliquer. Un élément &lt;input&gt; de type texte définit une zone permettant aux utilisateurs d’entrer du texte à l’aide d’un clavier physique ou visuel. Ces éléments s’adapteront également aux futurs scénarios d’interaction, tels qu’une action annoncée par la voix ou une entrée de texte dictée.

Quand une action est exécutée par l’utilisateur, vous pouvez effectuer l’une des opérations suivantes en spécifiant l’attribut [**ActivationType**](https://msdn.microsoft.com/library/windows/desktop/dn408447) à l’intérieur de l’élément &lt;action&gt; :

-   activation de l’application au premier plan à l’aide d’un argument propre à l’action qui permet d’accéder à une page ou à un contexte spécifiques ;
-   activation de la tâche en arrière-plan de l’application sans affecter l’utilisateur ;
-   activation d’une autre application par le biais d’un lancement par protocole ;
-   spécification d’une action système à exécuter. Les actions système actuellement disponibles sont la répétition et le masquage d’une alarme ou d’un rappel planifiés et sont décrites en détail dans l’une des sections ci-après.

Pour découvrir tous les attributs pris en charge dans la section « visual » et tous les éléments enfants de cette dernière, voir la section « Schéma » ci-dessous. Pour consulter d’autres exemples, voir la section « Exemples XML » ci-après.

### <a name="audio"></a>Élément &lt;audio&gt;

Pour l’instant, les sons personnalisés ne sont pas pris en charge dans les applications UWP qui ciblent la Plate-forme Desktop ; à la place, vous pouvez choisir un son dans la liste ms-winsoundevents pour votre application destinée aux appareils de bureau. Les applications UWP ciblant les plateformes mobiles prennent en charge aussi bien les sons ms-winsoundevents que les sons personnalisés aux formats suivants :

-   ms-appx:///
-   ms-appdata:///

Pour plus d’informations sur les éléments audio dans les notifications toast, voir la [page de schéma audio](https://msdn.microsoft.com/library/windows/apps/br230842) qui inclut la liste complète des sons ms-winsoundevents.

## <a name="alarms-reminders-and-incoming-calls"></a>Alarmes, rappels et appels entrants


Vous pouvez utiliser des notifications toast pour les alarmes, les rappels et les appels entrants. Ces notifications toast spéciales ont une apparence semblable à celle des notifications toast standard, mais présentent certains motifs et éléments d’interface utilisateur personnalisés basés sur un scénario :

-   Une notification toast de rappel reste affichée à l’écran jusqu’à ce que l’utilisateur la masque ou exécute une action. Sur Windows Mobile, les notifications toast de rappel s’affichent également sous leur forme pré-développée.
-   Outre le fait de partager les comportements ci-dessus avec les notifications de rappel, les notifications d’alarme émettent automatiquement le son en boucle.
-   Les notifications d’appel entrant s’affichent en plein écran sur les appareils Windows Mobile. Ce résultat est obtenu par la spécification de l’attribut « scenario » à l’intérieur de l’élément racine d’une notification toast, c’est-à-dire &lt;toast&gt; : &lt;toast scenario=" { default | alarm | reminder | incomingCall } " &gt;

## <a name="xml-examples"></a>Exemples XML


**Remarque** Les captures d’écran de notification toast correspondant à ces exemples ont été effectuées à partir d’une application exécutée sur un appareil de bureau. Sur les appareils mobiles, une notification toast peut s’afficher sous sa forme réduite en présentant une poignée dans sa partie inférieure pour la développer.

 

**Notification avec un contenu visuel enrichi**

Cet exemple illustre comment inclure plusieurs lignes de texte, une petite image facultative pour remplacer le logo de l’application et une miniature de l’image incluse facultative.

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Photo Share</text>
      <text>Andrew sent you a picture</text>
      <text>See it in full size!</text>
      <image src="https://unsplash.it/360/180?image=1043" />
      <image placement="appLogoOverride" src="https://unsplash.it/64?image=883" hint-crop="circle" />
    </binding>
  </visual>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Photo Share"
                },
 
                new AdaptiveText()
                {
                    Text = "Andrew sent you a picture"
                },
 
                new AdaptiveText()
                {
                    Text = "See it in full size!"
                },
 
                new AdaptiveImage()
                {
                    Source = "https://unsplash.it/360/180?image=1043"
                }
            },
 
            AppLogoOverride = new ToastGenericAppLogo()
            {
                Source = "https://unsplash.it/64?image=883",
                HintCrop = ToastGenericAppLogoCrop.Circle
            }
        }
    }
};
```

![Notification avec un contenu visuel enrichi](images/adaptivetoasts-xmlsample01.jpg)

 

**Notification avec des actions, exemple 1**

Cet exemple illustre...

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Microsoft Company Store</text>
      <text>New Halo game is back in stock!</text>
    </binding>
  </visual>
  <actions>
    <action activationType="foreground" content="See more details" arguments="details"/>
    <action activationType="background" content="Remind me later" arguments="later"/>
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Microsoft Company Store"
                },
 
                new AdaptiveText()
                {
                    Text = "New Halo game is back in stock!"
                }
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("See more details", "details"),
 
            new ToastButton("Remind me later", "later")
            {
                ActivationType = ToastActivationType.Background
            }
        }
    }
};
```

![Notification avec des actions, exemple 1](images/adaptivetoasts-xmlsample02.jpg)

 

**Notification avec des actions, exemple 2**

Cet exemple illustre...

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Restaurant suggestion...</text>
      <text>We noticed that you are near Wasaki. Thomas left a 5 star rating after his last visit, do you want to try it?</text>
    </binding>
  </visual>
  <actions>
    <action activationType="foreground" content="Reviews" arguments="reviews" />
    <action activationType="protocol" content="Show map" arguments="bingmaps:?q=sushi" />
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Restaurant suggestion..."
                },
 
                new AdaptiveText()
                {
                    Text = "We noticed that you are near Wasaki. Thomas left a 5 star rating after his last visit, do you want to try it?"
                }
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("Reviews", "reviews"),
 
            new ToastButton("Show map", "bingmaps:?q=sushi")
            {
                ActivationType = ToastActivationType.Protocol
            }
        }
    }
};
```

![Notification avec des actions, exemple 2](images/adaptivetoasts-xmlsample03.jpg)

 

**Notification avec une entrée de texte et des actions, exemple 1**

Cet exemple illustre...

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Andrew B.</text>
      <text>Shall we meet up at 8?</text>
      <image placement="appLogoOverride" src="https://unsplash.it/64?image=883" hint-crop="circle" />
    </binding>
  </visual>
  <actions>
    <input id="message" type="text" placeHolderContent="Type a reply" />
    <action activationType="background" content="Reply" arguments="reply" />
    <action activationType="foreground" content="Video call" arguments="video" />
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Andrew B."
                },
 
                new AdaptiveText()
                {
                    Text = "Shall we meet up at 8?"
                }
            },
 
            AppLogoOverride = new ToastGenericAppLogo()
            {
                Source = "https://unsplash.it/64?image=883",
                HintCrop = ToastGenericAppLogoCrop.Circle
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("message")
            {
                PlaceholderContent = "Type a reply"
            }
        },
 
        Buttons =
        {
            new ToastButton("Reply", "reply")
            {
                ActivationType = ToastActivationType.Background
            },
 
            new ToastButton("Video call", "video")
            {
                ActivationType = ToastActivationType.Foreground
            }
        }
    }
};
```

![Notification avec une entrée de texte et des actions d’entrée](images/adaptivetoasts-xmlsample04.jpg)

 

**Notification avec une entrée de texte et des actions, exemple 2**

Cet exemple illustre...

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Andrew B.</text>
      <text>Shall we meet up at 8?</text>
      <image placement="appLogoOverride" src="https://unsplash.it/64?image=883" hint-crop="circle" />
    </binding>
  </visual>
  <actions>
    <input id="message" type="text" placeHolderContent="Type a reply" />
    <action activationType="background" content="Reply" arguments="reply" hint-inputId="message" imageUri="Assets/Icons/send.png"/>
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Andrew B."
                },
 
                new AdaptiveText()
                {
                    Text = "Shall we meet up at 8?"
                }
            },
 
            AppLogoOverride = new ToastGenericAppLogo()
            {
                Source = "https://unsplash.it/64?image=883",
                HintCrop = ToastGenericAppLogoCrop.Circle
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("message")
            {
                PlaceholderContent = "Type a reply"
            }
        },
 
        Buttons =
        {
            new ToastButton("Reply", "reply")
            {
                TextBoxId = "message",
                ImageUri = "Assets/Icons/send.png",
                ActivationType = ToastActivationType.Background
            }
        }
    }
};
```

![Notification avec une entrée de texte et des actions](images/adaptivetoasts-xmlsample05.jpg)

 

**Notification avec une entrée de sélection et des actions**

Cet exemple illustre...

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Spicy Heaven</text>
      <text>When do you plan to come in tomorrow?</text>
    </binding>
  </visual>
  <actions>
    <input id="time" type="selection" defaultInput="2" >
      <selection id="1" content="Breakfast" />
      <selection id="2" content="Lunch" />
      <selection id="3" content="Dinner" />
    </input>
    <action activationType="background" content="Reserve" arguments="reserve" />
    <action activationType="foreground" content="Call Restaurant" arguments="call" />
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Spicy Heaven"
                },
 
                new AdaptiveText()
                {
                    Text = "When do you plan to come in tomorrow?"
                }
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("time")
            {
                DefaultSelectionBoxItemId = "2",
                Items =
                {
                    new ToastSelectionBoxItem("1", "Breakfast"),
                    new ToastSelectionBoxItem("2", "Lunch"),
                    new ToastSelectionBoxItem("3", "Dinner")
                }
            }
        },
 
        Buttons =
        {
            new ToastButton("Reserve", "reserve")
            {
                ActivationType = ToastActivationType.Background
            },
 
            new ToastButton("Call Restaurant", "call")
            {
                ActivationType = ToastActivationType.Foreground
            }
        }
    }
};
```

![Notification avec une entrée de sélection et des actions](images/adaptivetoasts-xmlsample06.jpg)

 

**Notification de rappel**

Cet exemple illustre...

```XML
<toast scenario="reminder" launch="action=viewEvent&amp;eventId=1983">
   
  <visual>
    <binding template="ToastGeneric">
      <text>Adaptive Tiles Meeting</text>
      <text>Conf Room 2001 / Building 135</text>
      <text>10:00 AM - 10:30 AM</text>
    </binding>
  </visual>
 
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

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "action=viewEvent&eventId=1983",
    Scenario = ToastScenario.Reminder,
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Adaptive Tiles Meeting"
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
    },
 
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

![notification de rappel](images/adaptivetoasts-xmlsample07.jpg)

 

## <a name="handling-activation-foreground-and-background"></a>Gestion de l’activation (au premier plan et en arrière-plan)

Pour savoir comment gérer les activations toast (l’utilisateur clique sur votre notification toast ou les boutons de celle-ci), consultez [Démarrage rapide : envoi d’une notification toast locale et gestion de l’activation](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10/).


## <a name="schemas-ltvisualgt-and-ltaudiogt"></a>Schémas : &lt;visuel&gt; et &lt;audio&gt;


Dans les schémas XML suivants, le suffixe « ? » signifie qu’un attribut est facultatif.

```
<toast launch? duration? activationType? scenario? >
  <visual lang? baseUri? addImageQuery? >
    <binding template? lang? baseUri? addImageQuery? >
      <text lang? hint-maxLines? >content</text>
      <image src placement? alt? addImageQuery? hint-crop? />
      <group>
        <subgroup hint-weight? hint-textStacking? >
          <text />
          <image />
        </subgroup>
      </group>
    </binding>
  </visual>
  <audio src? loop? silent? />
</toast>
```

```
ToastContent content = new ToastContent()
{
    Launch = ?,
    Duration = ?,
    ActivationType = ?,
    Scenario = ?,
 
    Visual = new ToastVisual()
    {
        Language = ?,
        BaseUri = ?,
        AddImageQuery = ?,
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = ?,
                    Language = ?,
                    HintMaxLines = ?
                },
 
                new AdaptiveGroup()
                {
                    Children =
                    {
                        new AdaptiveSubgroup()
                        {
                            HintWeight = ?,
                            HintTextStacking = ?,
                            Children =
                            {
                                new AdaptiveText(),
                                new AdaptiveImage()
                            }
                        }
                    }
                },
 
                new AdaptiveImage()
                {
                    Source = ?,
                    AddImageQuery = ?,
                    AlternateText = ?,
                    HintCrop = ?
                }
            }
        }
    },
 
    Audio = new ToastAudio()
    {
        Src = ?,
        Loop = ?,
        Silent = ?
    }
};
```

**Attributs dans &lt;toast&gt;**

launch?

-   launch? = chaîne
-   Il s’agit d’un attribut facultatif.
-   Chaîne transmise à l’application lorsqu’elle est activée par la notification toast.
-   Selon la valeur d’activationType, cette valeur peut être reçue par l’application au premier plan, à l’intérieur de la tâche en arrière-plan ou par une autre application lancée par protocole à partir de l’application d’origine.
-   L’application définit le format et le contenu de cette chaîne pour son propre usage.
-   Lorsque l’utilisateur appuie ou clique sur la notification toast pour lancer l’application qui lui est associée, la chaîne de lancement précise le contexte à l’application pour permettre à cette dernière de présenter à l’utilisateur une vue adaptée au contenu de la notification toast plutôt qu’une vue par défaut.
-   Si l’activation découle d’un clic par l’utilisateur sur une action plutôt que dans le corps de la notification toast, le développeur récupère les « arguments » prédéfinis dans cette balise &lt;action&gt; à la place de la chaîne « launch » prédéfinie dans la balise &lt;toast&gt;.

duration?

-   duration? = "short|long"
-   Il s’agit d’un attribut facultatif. La valeur par défaut est « short ».
-   Cet attribut n’est proposé que pour des scénarios spécifiques et pour la base de données de compatibilité des applications (AppCompat). Vous n’en avez plus besoin pour le scénario d’alarme.
-   Nous vous déconseillons d’utiliser cette propriété.

activationType?

-   activationType? = "foreground | background | protocol | system"
-   Il s’agit d’un attribut facultatif.
-   La valeur par défaut est « foreground ».

scenario?

-   scenario? = "default | alarm | reminder | incomingCall"
-   Il s’agit d’un attribut facultatif, dont la valeur par défaut est « default ».
-   Vous n’en avez pas besoin, sauf si votre scénario consiste à présenter une alarme, un rappel ou un appel entrant.
-   Ne l’utilisez pas dans le seul but d’assurer la persistance de votre notification à l’écran.

**Attributs dans &lt;visual&gt;**

lang?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230847).

baseUri?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230847).

addImageQuery?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230847).

**Attributs dans &lt;binding&gt;**

template?

-   \[Important\] template? = "ToastGeneric"
-   Si vous avez recours à l’une des nouvelles fonctionnalités des notifications adaptatives et interactives, vérifiez que vous commencez par utiliser le modèle « ToastGeneric » plutôt que le modèle hérité.
-   Même si l’utilisation des modèles hérités avec les nouvelles actions fonctionne encore, il ne s’agit pas du cas d’utilisation prévu, et nous ne pouvons pas garantir que cette approche continuera de fonctionner à l’avenir.

lang?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230847).

baseUri?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230847).

addImageQuery?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230847).

**Attributs dans &lt;text&gt;**

lang?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230847).

**Attributs dans &lt;image&gt;**

src

-   Pour plus de détails sur cet attribut obligatoire, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230844).

placement?

-   placement? = "inline" | "appLogoOverride"
-   Cet attribut est facultatif.
-   Il spécifie l’endroit où cette image s’affichera.
-   L’élément « inline » indique de placer l’image dans le corps de la notification toast, sous le texte ; L’élément « appLogoOverride » indique de remplacer l’icône de l’application (qui s’affiche dans le coin supérieur gauche de la notification toast).
-   Vous ne pouvez définir qu’une seule image par attribut « placement ».

alt?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230844).

addImageQuery?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230844).

hint-crop?

-   hint-crop? = "none" | "circle"
-   Cet attribut est facultatif.
-   L’élément « none » est la valeur par défaut qui signifie aucun rognage.
-   L’élément « circle » rogne l’image pour lui donner une forme circulaire. Utilisez cette valeur pour les images de profil d’un contact, les images d’une personne, etc.

**Attributs dans &lt;audio&gt;**

src?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230842).

loop?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230842).

silent?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230842).

## <a name="schemas-ltactiongt"></a>Schémas : &lt;action&gt;


Dans les schémas XML suivants, le suffixe « ? » signifie qu’un attribut est facultatif.

```
<toast>
  <visual>
  </visual>
  <audio />
  <actions>
    <input id type title? placeHolderContent? defaultInput? >
      <selection id content />
    </input>
    <action content arguments activationType? imageUri? hint-inputId />
  </actions>
</toast>
```

```
ToastContent content = new ToastContent()
{
    Visual = ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("id")
            {
                Title = ?
                DefaultSelectionBoxItemId = ?,
                Items =
                {
                    new ToastSelectionBoxItem("id", "content")
                }
            },
 
            new ToastTextBox("id")
            {
                Title = ?,
                PlaceholderContent = ?,
                DefaultInput = ?
            }
        },
 
        Buttons =
        {
            new ToastButton("content", "args")
            {
                ActivationType = ?,
                ImageUri = ?,
                TextBoxId = ?
            },
 
            new ToastButtonSnooze("content")
            {
                SelectionBoxId = "snoozeTime"
            },
 
            new ToastButtonDismiss("content")
        }
    }
};
```

**Attributs dans &lt;input&gt;**

id

-   id = chaîne
-   Cet attribut est obligatoire.
-   L’attribut « id » est obligatoire et permet aux développeurs de récupérer les entrées utilisateur une fois l’application activée (au premier plan ou en arrière-plan).

type

-   type = "text | selection"
-   Cet attribut est obligatoire.
-   Il permet de spécifier une entrée de texte ou une entrée effectuée à partir d’une liste de sélections prédéfinies.
-   Sur les appareils mobiles et de bureau, cet attribut sert à indiquer si vous souhaitez créer une entrée de zone de texte ou une entrée de zone de liste.

title?

-   title? = chaîne
-   L’attribut « title » est facultatif et permet aux développeurs de spécifier un titre pour l’entrée à afficher par les interpréteurs de commande lorsqu’il existe une affordance.
-   Sur les appareils mobiles et de bureau, ce titre apparaîtra au-dessus de l’entrée.

placeHolderContent?

-   placeHolderContent? = chaîne
-   L’attribut « placeHolderContent » est facultatif et constitue le texte d’information grisé d’une entrée de type texte. Cet attribut est ignoré lorsque l’entrée ne présente pas le type « text ».

defaultInput?

-   defaultInput? = chaîne
-   L’attribut « defaultInput » est facultatif et permet de fournir une valeur d’entrée par défaut.
-   Si l’entrée présente le type « text », cette valeur sera traitée comme une entrée de chaîne.
-   Si l’entrée présente le type « selection », cette valeur doit correspondre à l’identificateur de l’une des sélections disponibles dans les éléments de cette entrée.

**Attributs dans &lt;selection&gt;**

id

-   Cet attribut est obligatoire. Il permet d’identifier les sélections de l’utilisateur. L’identificateur est renvoyé à votre application.

content

-   Cet attribut est obligatoire. Il fournit la chaîne à afficher pour cet élément de sélection.

**Attributs dans &lt;action&gt;**

content

-   content = chaîne
-   L’attribut « content » est obligatoire. Il fournit la chaîne de texte affichée sur le bouton.

arguments

-   arguments = chaîne
-   L’attribut « arguments » est obligatoire. Il décrit les données définies par l’application que cette dernière peut récupérer par la suite une fois activée par l’utilisateur exécutant cette action.

activationType?

-   activationType? = "foreground | background | protocol | system"
-   L’attribut « activationType » est facultatif et présente la valeur par défaut « foreground ».
-   Il décrit le type d’activation généré par cette action : au premier plan, en arrière-plan, lancement d’une autre application par le biais d’un lancement par protocole ou appel d’une action système.

imageUri?

-   imageUri? = chaîne
-   L’attribut « imageUri » est facultatif et permet de fournir une icône d’image pour cette action à afficher dans le bouton avec le contenu de texte.

hint-inputId

-   hint-inputId = chaîne
-   L’attribut « hint-inpudId » est obligatoire. Il est spécifiquement destiné au scénario de réponse rapide.
-   Cette valeur doit correspondre à l’identificateur de l’élément d’entrée que vous souhaitez associer.
-   Sur les appareils mobiles et de bureau, cet attribut placera le bouton juste à côté de la zone d’entrée.

## <a name="attributes-for-system-handled-actions"></a>Attributs pour les actions gérées par le système


Le système peut gérer les actions de répétition et de masquage des notifications si vous ne voulez pas que votre application traite la répétition/replanification des notifications sous la forme d’une tâche en arrière-plan. Les actions gérées par le système peuvent être combinées (ou spécifiées individuellement), mais nous vous déconseillons d’implémenter une action de répétition sans une action de masquage.

Combinaison de commandes système : SnoozeAndDismiss

```
<toast>
  <visual>
  </visual>
  <actions hint-systemCommands="SnoozeAndDismiss" />
</toast>
```

```
ToastContent content = new ToastContent()
{
    Visual = ...
 
    Actions = new ToastActionsSnoozeAndDismiss()
};
```

Actions gérées par le système individuelles

```
<toast>
  <visual>
  </visual>
  <actions>
  <input id="snoozeTime" type="selection" defaultInput="10">
    <selection id="5" content="5 minutes" />
    <selection id="10" content="10 minutes" />
    <selection id="20" content="20 minutes" />
    <selection id="30" content="30 minutes" />
    <selection id="60" content="1 hour" />
  </input>
  <action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content=""/>
  <action activationType="system" arguments="dismiss" content=""/>
  </actions>
</toast>
```

```
ToastContent content = new ToastContent()
{
    Visual = ...
 
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
                    new ToastSelectionBoxItem("10", "10 minutes"),
                    new ToastSelectionBoxItem("20", "20 minutes"),
                    new ToastSelectionBoxItem("30", "30 minutes"),
                    new ToastSelectionBoxItem("60", "1 hour")
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

Pour construire des actions de répétition et de masquage individuelles, procédez comme suit :

-   Spécifiez : activationType = "system".
-   Spécifiez : arguments = "snooze" | "dismiss".
-   Spécifiez le contenu :
    -   Si vous souhaitez afficher les chaînes localisées de « snooze » et de « dismiss » sur les actions, spécifiez le contenu comme étant une chaîne vide : &lt;action content = ""/&gt;
    -   Si vous voulez définir une chaîne personnalisée, fournissez simplement sa valeur : &lt;action content="Me le rappeler ultérieurement" /&gt;
-   Spécifiez l’entrée :
    -   Si vous ne voulez pas que l’utilisateur sélectionne un intervalle de répétition, mais souhaitez simplement que votre notification se répète une seule fois pendant un intervalle de temps défini par le système (et cohérent dans l’ensemble du système d’exploitation), ne construisez aucun élément &lt;input&gt;.
    -   Si vous voulez fournir des sélections d’intervalle de répétition :
        -   Spécifiez l’attribut « hint-inputId » dans l’action de répétition.
        -   Faites correspondre l’identificateur de l’entrée avec la valeur de l’attribut « hint-inputId » de l’action de répétition : &lt;input id="snoozeTime"&gt;&lt;/input&gt;&lt;action hint-inputId="snoozeTime"/&gt;
        -   Spécifiez l’identificateur de sélection comme étant un entier non négatif (nonNegativeInteger) qui représente l’intervalle de répétition en minutes : &lt;selection id="240" /&gt; signifie une répétition pendant 4 heures.
        -   Assurez-vous que la valeur de l’attribut « defaultInput » dans &lt;input&gt; correspond à l’un des identificateurs des éléments enfants &lt;selection&gt;.
        -   Fournissez jusqu’à 5 valeurs &lt;selection&gt;.

 

 
## <a name="related-topics"></a>Rubriques connexes

* [Démarrage rapide : Envoyer une notification toast et gérer l’activation](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/07/08/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10.aspx)
* [Bibliothèque Notifications sur GitHub](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)


<!--HONumber=Dec16_HO1-->


