---
title: Notifications de mes contacts
description: "Explique comment créer et utiliser les notifications de mes contacts, qui sont un nouveau type de notification toast."
author: mukin
ms.author: mukin
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: b1fbba8b8cea3edd51dc9b60cae1ea3853f39dd1
ms.sourcegitcommit: ec18e10f750f3f59fbca2f6a41bf1892072c3692
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/14/2017
---
# <a name="my-people-notifications"></a>Notifications de mes contacts

> [!IMPORTANT]
> **VERSION PRÉLIMINAIRE | Nécessite FallCreatorsUpdate**: vous devez cibler le [Kit de développement logiciel (SDK) Insider16225](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK) et exécuter [Insider build16226](https://blogs.windows.com/windowsexperience/2017/06/21/announcing-windows-10-insider-preview-build-16226-pc/) ou version ultérieure pour utiliser les API Mes Contacts.

Les notifications de mes contacts sont un nouveau type de mouvement mettant en avant les personnes. Ils offrent une nouvelle façon pour les utilisateurs d’entrer en relation avec les personnes qui les intéressent par le biais de mouvements expressifs rapides et légers.

![notification par emoji représentant un cœur](images/heart-emoji-notification-small.gif)

## <a name="requirements"></a>Configuration requise

+ Windows10 et Microsoft Visual Studio2017. Pour en savoir plus sur l’installation, voir [Prendre en main VisualStudio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up).
+ Connaissances de base de C# ou d’un langage de programmation orienté objet similaire. Pour vous familiariser avec C#, voir [Créer une application «Hello, world»](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).

## <a name="how-it-works"></a>Fonctionnement

Comme alternative aux notifications toast génériques, les développeurs d’applications peuvent désormais envoyer des notifications par le biais de la fonctionnalité Mes Contacts pour proposer une expérience plus personnelle aux utilisateurs. Il s’agit d’un nouveau type de toast, envoyé à partir d’un contact épinglé à la barre des tâches de l’utilisateur. À réception de la notification, la photo du contact de l’expéditeur s’anime dans la barre des tâches et un son est émis pour signaler qu’une notification de mes contacts démarre. Ensuite, l’animation ou l’image définie dans la charge utile s’affiche pendant 5secondes (si la charge utile est une animation d’une durée inférieure à 5secondes, elle s’exécute en boucle pendant 5secondes).

## <a name="supported-image-types"></a>Types d’images pris en charge

+ GIF
+ Image statique (JPEG, PNG)
+ Spritesheet (vertical uniquement)

> [!NOTE]
> Un Spritesheet est une animation dérivée d’une image statique (JPEG ou PNG). Les images sont disposées verticalement, de sorte que la première image soit placée au-dessus (vous pouvez spécifier une autre image de démarrage dans la charge utile de la notification toast). Chaque image doit avoir la même hauteur. Le programme les parcourt pour créer une séquence animée (comme un livret dans lequel toutes les pages sont disposées verticalement). Voici un exemple de Spritesheet:

![Spritesheet arc-en-ciel](images/shoulder-tap-rainbow-spritesheet.png)

## <a name="notification-parameters"></a>Paramètres de notification
Les notifications de mes contacts utilisent l’infrastructure de la [notification toast](../controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts.md) dans Windows10 et nécessitent un nœud de liaison supplémentaire dans la charge utile de la notification toast. Cela signifie que les notifications de mes contacts doivent avoir deux liaisons au lieu d’une. La deuxième liaison doit inclure le paramètre suivant:

```xml
experienceType=”shoulderTap”
```

Cela indique que la notification toast doit être traitée comme une notification de mes contacts.

Le nœud de l’image à l’intérieur de la liaison doit inclure les paramètres suivants:

+ **src**
    + L’URI de la ressource. Il peut s’agir d’un URI web HTTP/HTTPS, d’un URI msappx ou d’un chemin d’accès à un fichier local.
+ **spritesheet-src**
    + L’URI de la ressource. Il peut s’agir d’un URI web HTTP/HTTPS, d’un URI msappx ou d’un chemin d’accès à un fichier local. Requis uniquement pour les animations Spritesheet.
+ **spritesheet-height**
    + La hauteur de l’image (en pixels). Requis uniquement pour les animations Spritesheet.
+ **spritesheet-fps**
    + Images par seconde. Requis uniquement pour les animations Spritesheet.
+ **spritesheet-startingFrame**
    + Numéro de l’image pour commencer l’animation. Uniquement utilisé pour les animations Spritesheet et a pour valeur par défaut 0 si le numéro n’est pas indiqué.
+ **alt**
    + Chaîne de texte utilisée pour la narration du lecteur d’écran.

> [!NOTE]
> Même si vous utilisez un Spritesheet, vous devez toujours spécifier une image statique dans le paramètre «src» en secours au cas où l’animation ne parviendrait pas à afficher.

En outre, le nœud toast de niveau supérieur doit inclure le paramètre **hint-people** pour indiquer le contact d’envoi. Ce paramètre peut prendre les valeurs suivantes:

+ **Adresse électronique** 
    + Par exemple, mailto:johndoe@mydomain.com
+ **Numéro de téléphone** 
    + Par exemple, tél: 888-888-8888
+ **ID distant** 
    + Par exemple, remoteid:1234

> [!NOTE]
> Si votre application utilise les [API ContactStore](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactstore) et la propriété [StoredContact.RemoteId](https://docs.microsoft.com/en-us/uwp/api/Windows.Phone.PersonalInformation.StoredContact#Windows_Phone_PersonalInformation_StoredContact_RemoteId) pour lier des contacts stockés sur le PC avec des contacts stockés à distance, il est essentiel que la valeur de la propriété RemoteID soit stable et unique. Cela signifie que l’ID distant doit identifier de manière cohérente un compte d’utilisateur unique et doit contenir une balise unique afin de garantir qu’il n’est pas en conflit avec les ID distants des autres contacts sur le PC, y compris les contacts qui appartiennent à d’autres applications.
> S’il n’est pas garanti que les ID à distance utilisés par votre application soient stables et uniques, vous pouvez utiliser la classe RemoteIdHelper indiquée plus loin dans cette rubrique afin d’ajouter une balise unique à l’ensemble de vos ID distants avant de les ajouter au système. Vous pouvez également choisir de ne pas utiliser la propriété RemoteID et de créer à la place une propriété personnalisée étendue dans laquelle stocker les ID distants de vos contacts.

En plus de la deuxième liaison et de sa charge utile, vous DEVEZ inclure une autre charge utile dans la première liaison pour le toast de secours (que la notification utilise si elle est forcée de revenir à une notification toast régulière). Cela est expliqué plus en détail dans la section finale.

## <a name="creating-the-notification"></a>Création de la notification
Vous pouvez créer un modèle de notification de mes contacts comme vous créeriez une [notification toast](../controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts.md).

Voici un exemple montrant comment créer une notification de mes contacts à l’aide d’une image statique:

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-static-payload.png"/>
        </binding>
    </visual>
</toast>
```

Si vous démarrez la notification, celle-ci doit ressembler à ceci:

![notification d’image statique](images/static-image-notification-small.gif)

Ensuite, nous allons montrer comment créer une notification à l’aide d’un Spritesheet animé. Ce Spritesheet a une hauteur d’image de 80pixels, que nous allons animer à la vitesse de 25images par seconde. Nous avons défini l’image de départ sur 15 pour et nous l’avons associée à une image de secours statique dans le paramètre «src». L’image de secours est utilisée si l’animation de Spritesheet ne parvient pas à s’afficher.

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-static.png"
                spritesheet-src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-spritesheet.png"
                spritesheet-height='80' spritesheet-fps='25' spritesheet-startingFrame='15'/>
        </binding>
    </visual>
</toast>
```

Si vous démarrez la notification, celle-ci doit ressembler à ceci:

![Notification par Spritesheet](images/pizza-notification-small.gif)

## <a name="starting-the-notification"></a>Démarrage de la notification
Pour démarrer une notification de mes contacts, nous devons convertir le modèle de notification toast en un objet [XmlDocument](https://msdn.microsoft.com/en-us/library/windows/apps/windows.data.xml.dom.xmldocument.aspx). En supposant que vous avez défini la notification toast dans un fichier XML (ici nommé «content.xml»), vous pouvez utiliser ce code C# pour le démarrer:

```CSharp
string xmlText = File.ReadAllText("content.xml");
XmlDocument xmlContent = new XmlDocument();
xmlContent.LoadXml(xmlText);
```

Vous pouvez ensuite utiliser ce code pour créer et envoyer la notification toast:

```CSharp
ToastNotification notification = new ToastNotification(xmlContent);
ToastNotificationManager.CreateToastNotifier().Show(notification);
```

## <a name="falling-back-to-toast"></a>Revenir à la notification toast
Il existe certains cas dans lesquels une notification codée sous forme de notification de mes contacts s’affiche en tant que notification régulière. Une notification de mes contacts remplace une notification toast dans les conditions suivantes:

+ La notification ne parvient pas à s’afficher
+ Les notifications de mes contacts ne sont pas activées par le destinataire
+ Le contact de l’expéditeur n’est pas épinglé sur la barre des tâches du destinataire

Si une notification de mes contacts remplace une notification toast, la deuxième liaison spécifique de mes contacts est ignorée, et seule la première liaison est utilisée pour afficher la notification toast. Cela signifie que, comme mentionné précédemment, une charge utile de secours doit être indiquée dans la première liaison toast.

## <a name="see-also"></a>Articles associés
+ [Ajout de la prise en charge de Mes Contacts](my-people-support.md)
+ [Notifications toast adaptatives](../controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts.md)
+ [Classe ToastNotification](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification)