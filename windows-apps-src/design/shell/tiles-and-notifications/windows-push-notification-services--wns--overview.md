---
Description: Les services de notification Push Windows (WNS) permettent aux développeurs tiers d’envoyer des mises à jour de toast, de vignette et de badge, ainsi que des mises à jour brutes à partir de leur propre service cloud. Il en résulte un mécanisme fiable et optimal de remise des nouvelles mises à jour aux utilisateurs.
title: Vue d’ensemble des services de notifications Push Windows (WNS)
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cf538e6b7c66bfc61574295d5b040db82122e78a
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363344"
---
# <a name="windows-push-notification-services-wns-overview"></a>Vue d’ensemble des services de notifications Push Windows (WNS)
 

Les services de notification Push Windows (WNS) permettent aux développeurs tiers d’envoyer des mises à jour de toast, de vignette et de badge, ainsi que des mises à jour brutes à partir de leur propre service cloud. Il en résulte un mécanisme fiable et optimal de remise des nouvelles mises à jour aux utilisateurs.

## <a name="how-it-works"></a>Fonctionnement


Le diagramme ci-après présente l’intégralité du flux de données impliqué dans l’envoi d’une notification Push. Les étapes nécessaires sont les suivantes :

1.  Votre application demande un canal de notification Push auprès de la plateforme Windows universelle.
2.  Windows demande aux services WNS de créer un canal de notification. Ce canal est retourné à l’appareil appelant sous la forme d’un URI (Uniform Resource Identifier).
3.  L’URI du canal de notification est retourné par Windows à votre application.
4.  Votre application envoie l’URI à votre service cloud. Vous stockez ensuite l’URI sur votre service cloud afin de pouvoir accéder à l’URI lors de l’envoi de notifications. L’URI est une interface entre votre application et votre service ; c’est à vous qu’il incombe d’implémenter cette interface avec des normes Web sécurisées.
5.  Quand votre service cloud a une mise à jour à envoyer, il notifie les services WNS par le biais de l’URI de canal. Pour cela, une requête HTTP POST, incluant la charge utile de notification, est émise via SSL (Secure Sockets Layer). Cette étape nécessite une authentification.
6.  Les services WNS reçoivent la requête et acheminent la notification vers l’appareil approprié.

![Diagramme du flux de données WNS relatif aux notifications Push](images/wns-diagram-01.png)

## <a name="registering-your-app-and-receiving-the-credentials-for-your-cloud-service"></a>Inscription de votre application et réception des informations d’identification de votre service cloud


Pour que vous puissiez envoyer des notifications à l’aide des services WNS, votre application doit au préalable être inscrite auprès du Tableau de bord du Store. Cela vous fournit les informations d’identification de votre application que votre service cloud utilisera lors de l’authentification à l’aide des services WNS. Ces informations d’identification se composent d’un identificateur de sécurité du package (SID) et d’une clé secrète. Pour effectuer cette inscription, connectez-vous à [partenaires](https://partner.microsoft.com/dashboard). Après avoir créé votre application, vous pouvez récupérer les informations d’identification en suivant les instructions disponibles sur la page **Gestion des applications - WNS/MPNS**. Si vous souhaitez utiliser la solution Services Live, suivez le lien **Site Services Live** sur cette page.

Chaque application dispose de son propre ensemble d’informations d’identification pour son service cloud. Ces informations d’identification ne peuvent pas être utilisées pour envoyer des notifications à une autre application.

Pour plus d’informations sur l’inscription de votre application, voir [Comment s’authentifier auprès des services de notifications Push Windows (WNS)](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10)).

## <a name="requesting-a-notification-channel"></a>Demande d’un canal de notification


Lorsqu’une application capable de recevoir des notifications Push est en cours d’exécution, elle doit d’abord demander un canal de notification par le biais de [**CreatePushNotificationChannelForApplicationAsync**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager#Windows_Networking_PushNotifications_PushNotificationChannelManager_CreatePushNotificationChannelForApplicationAsync_System_String_). Pour obtenir une description complète et un exemple de code, voir [Comment demander, créer et enregistrer un canal de notification](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10)). Cette API renvoie un URI de canal qui est lié de manière unique à l’application appelante et sa vignette, par l’intermédiaire duquel tous les types de notifications peuvent être envoyés.

Une fois que l’application a créé un URI de canal, elle l’envoie à son service cloud, ainsi que toutes les métadonnées spécifiques à l’application qui doivent être associées à cet URI.

### <a name="important-notes"></a>Remarques importantes

-   Nous ne garantissons pas que l’URI de canal de notification d’une application restera toujours le même. Nous vous conseillons de faire en sorte que l’application demande un nouveau canal chaque fois qu’elle s’exécute et qu’elle mette à jour son service lorsque l’URI change. Le développeur ne doit jamais modifier l’URI de canal et doit le considérer comme une chaîne de boîte noire. Actuellement, les URI de canal expirent au bout de 30 jours. Si votre application Windows 10 sera renouvelé régulièrement son canal en arrière-plan, vous pouvez télécharger le [Push et exemple de notifications périodiques](https://go.microsoft.com/fwlink/p/?linkid=231476) pour Windows 8.1 et de réutiliser son code source et/ou le modèle il montre.
-   C’est vous, le développeur, qui implémentez l’interface entre le service cloud et l’application cliente. Nous recommandons que l’application passe par un processus d’authentification auprès de son propre service et qu’elle transmette les données par le biais d’un protocole sécurisé tel que HTTPS.
-   Il est important que le service cloud offre toujours la garantie que l’URI de canal utilise le domaine « notify.windows.com ». Le service ne doit jamais effectuer une transmission de type push de notifications vers un canal se trouvant sur un autre domaine. Si jamais le rappel pour votre application était compromis, une personne malveillante pourrait soumettre un URI de canal pour tromper WNS. Sans une inspection du domaine, votre service cloud pourrait divulguer des informations à cette personne malveillante sans le savoir.
-   Si votre service cloud tente d'envoyer une notification à un canal ayant expiré, le service WNS renvoie le [code de réponse 410](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10)). En réponse à ce code, votre service ne doit plus chercher à envoyer des notifications à cet URI.

## <a name="authenticating-your-cloud-service"></a>Authentification de votre service cloud


Pour envoyer une notification, le service cloud doit être authentifié via WNS. La première étape de ce processus a lieu lorsque vous inscrivez votre application auprès du Tableau de bord du Microsoft Store. Pendant le processus d’inscription, un ID de sécurité (SID) de package et une clé secrète sont attribués à votre application. Ces informations sont utilisées par votre service cloud pour s’authentifier auprès de WNS.

Le schéma d’authentification WNS est implémenté à l’aide du profil d’informations d’authentification du client à partir du protocole [OAuth 2.0](https://go.microsoft.com/fwlink/p/?linkid=226787). Le service cloud s’authentifie auprès de WNS en fournissant ses informations d’identification (SID de package et clé secrète). En retour, il reçoit un jeton d’accès. Ce jeton d’accès permet à un service cloud d’envoyer une notification. Le jeton est requis avec chaque demande de notification envoyée aux services WNS.

Au niveau le plus élevé, la chaîne d’informations se présente comme suit :

1.  Le service cloud envoie ses informations d’identification à WNS via HTTPS en suivant le protocole OAuth 2.0. Cette opération authentifie le service auprès de WNS.
2.  WNS retourne un jeton d’accès si l’authentification a réussi. Ce jeton d’accès est utilisé dans toutes les demandes de notification ultérieures jusqu’à son arrivée à expiration.

![Diagramme WNS concernant l’authentification du service cloud](images/wns-diagram-02.png)

Dans le cadre de l’authentification auprès de WNS, le service cloud soumet une requête HTTP sur SSL (Secure Sockets Layer). Les paramètres sont fournis au format « application/x-www-for-urlencoded ». Fournir le SID de votre Package dans le « client\_id « champ et votre clé secrète de clé dans le « client\_secret » champ. Pour obtenir des détails sur la syntaxe, voir la référence sur la [demande de jeton d’accès](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10)).

**Remarque**  c’est juste un exemple, le code pas couper-coller que vous pouvez utiliser avec succès dans votre propre code.

 

``` http
 POST /accesstoken.srf HTTP/1.1
 Content-Type: application/x-www-form-urlencoded
 Host: https://login.live.com
 Content-Length: 211
 
 grant_type=client_credentials&client_id=ms-app%3a%2f%2fS-1-15-2-2972962901-2322836549-3722629029-1345238579-3987825745-2155616079-650196962&client_secret=Vex8L9WOFZuj95euaLrvSH7XyoDhLJc7&scope=notify.windows.com
```

Les services WNS authentifient le service cloud et, en cas de succès, envoient une réponse « 200 OK ». Ce jeton d’accès est renvoyé dans les paramètres inclus dans le corps de la réponse HTTP, à l’aide du type de média « application/json ». Une fois que votre service a reçu le jeton d’accès, vous êtes prêt à envoyer des notifications.

L’exemple suivant présente une réponse d’authentification réussie, incluant le jeton d’accès. Pour obtenir des détails sur la syntaxe, voir [En-têtes des demandes et des réponses du service de notifications Push](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10)).

``` http
 HTTP/1.1 200 OK   
 Cache-Control: no-store
 Content-Length: 422
 Content-Type: application/json
 
 {
     "access_token":"EgAcAQMAAAAALYAAY/c+Huwi3Fv4Ck10UrKNmtxRO6Njk2MgA=", 
     "token_type":"bearer"
 }
```

### <a name="important-notes"></a>Remarques importantes

-   Le protocole OAuth 2.0 pris en charge dans cette procédure suit la version brouillon V16.
-   Le document RFC (Request For Comments) relatif à OAuth utilise le terme « client » pour faire référence au service cloud.
-   Cette procédure pourra faire l’objet de modifications lorsque l’ébauche sur le protocole OAuth sera finalisée.
-   Le jeton d’accès peut être réutilisé pour plusieurs demandes de notification. Cela permet au service cloud d’effectuer une seule fois l’authentification pour envoyer plusieurs notifications. Toutefois, lorsque le jeton d’accès expire, le service cloud doit à nouveau s’authentifier pour recevoir un nouveau jeton d’accès.

## <a name="sending-a-notification"></a>Envoi d’une notification


À l’aide de l’URI de canal, le service cloud peut envoyer une notification chaque fois qu’il dispose d’une mise à jour pour l’utilisateur.

Le jeton d’accès décrit ci-dessus peut être réutilisé pour plusieurs demandes de notification ; le serveur cloud n’a pas besoin de demander un nouveau jeton d’accès pour chaque notification. Si le jeton d’accès est arrivé à expiration, la demande de notification renvoie une erreur. Il est recommandé de ne pas essayer de renvoyer votre notification plus d’une fois si le jeton d’accès est rejeté. Si vous rencontrez cette erreur, vous devez demander un nouveau jeton d’accès et renvoyer la notification. Pour connaître le code d’erreur exact, voir la rubrique relative aux [Codes de réponse de notification Push](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10)).

1.  Le service cloud effectue une requête HTTP POST à l’URI de canal. Cette requête doit être effectuée via SSL, et contient les en-têtes nécessaires et la charge utile de notification. L’en-tête d’autorisation doit inclure le jeton d’accès acquis pour l’autorisation.

    Voici un exemple de requête : Pour obtenir des détails sur la syntaxe, voir [Codes de réponse de notification Push](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10)).

    Pour plus d’informations sur la composition de la charge utile de notification, consultez [Guide de démarrage rapide : Envoyer une notification push](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10)). La charge utile d’une notification Push par vignette, toast ou badge est fournie sous la forme d’un contenu XML qui respecte le [schéma de vignettes adaptatives](adaptive-tiles-schema.md) ou le [schéma de vignettes héritées](https://docs.microsoft.com/uwp/schemas/tiles/tiles-xml-schema-portal) définis. La charge utile d’une notification brute ne dispose d’aucune structure spécifique. Elle est strictement définie pour l’application.

    ``` http
     POST https://cloud.notify.windows.com/?token=AQE%bU%2fSjZOCvRjjpILow%3d%3d HTTP/1.1
     Content-Type: text/xml
     X-WNS-Type: wns/tile
     Authorization: Bearer EgAcAQMAAAAALYAAY/c+Huwi3Fv4Ck10UrKNmtxRO6Njk2MgA=
     Host: cloud.notify.windows.com
     Content-Length: 24

     <body>
     ....
    ```

2.  Les services WNS répondent pour indiquer que la notification a été reçue et qu’elle sera remise à la prochaine occasion. Toutefois, les services WNS ne fournissent pas de confirmation de bout en bout indiquant que votre notification a été reçue par l’appareil ou l’application.

Le diagramme ci-après illustre le flux de données :

![Diagramme WNS concernant l’envoi d’une notification](images/wns-diagram-03.png)

### <a name="important-notes"></a>Remarques importantes

-   Les services WNS ne garantissent pas la fiabilité ou la latence d’une notification.
-   Les notifications ne doivent jamais inclure des données confidentielles ou sensibles.
-   Pour envoyer une notification, le service cloud doit d’abord s’authentifier auprès de WNS et recevoir un jeton d’accès.
-   Un jeton d’accès permet uniquement à un service cloud d’envoyer des notifications à l’application pour laquelle le jeton a été créé. Un jeton d’accès ne peut pas être utilisé pour envoyer des notifications vers plusieurs applications. Par conséquent, si votre service cloud prend en charge plusieurs applications, il doit fournir le jeton d’accès approprié pour l’application lorsqu’il exécute un push de notification à chaque URI de canal.
-   Lorsque le périphérique est hors connexion, les services WNS stockent par défaut jusqu’à cinq notifications par vignette (si la mise en file d’attente est activée ; sinon, une notification par vignette) et une notification de badge pour chaque URI de canal et pas de notification brute. Ce comportement de mise en cache par défaut peut être modifié par l'intermédiaire de l'[en-tête X-WNS-Cache-Policy](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10)). Notez que les notifications toast ne sont jamais stockées lorsque l’appareil est hors connexion.
-   Dans les scénarios où le contenu de notification est personnalisé pour l’utilisateur, les services WNS recommandent que le service cloud envoie immédiatement ces mises à jour lorsqu’elles sont reçues. C’est le cas, par exemple, des mises à jour de flux de médias sociaux, des invitations à des communications instantanées, des notifications de nouveau message ou des alertes. D’autres scénarios peuvent impliquer l’envoi fréquent d’une même mise à jour générique à un vaste sous-ensemble de vos utilisateurs ; par exemple, des mises à jour relatives à la météo, à la bourse et aux actualités. Selon les recommandations WNS, la fréquence de ces mises à jour doit être au maximum d’une fois toutes les 30 minutes. L’utilisateur final ou les services WNS peuvent déterminer comme abusives des mises à jour de routine plus fréquentes.

## <a name="expiration-of-tile-and-badge-notifications"></a>Expiration des notifications par vignette et par badge


Par défaut, les notifications par vignette et par badge arrivent à expiration trois jours après avoir été téléchargées. Quand une notification arrive à expiration, le contenu est supprimé de la vignette ou de la file d’attente et n’est plus présenté à l’utilisateur. Il est préférable de définir une expiration (en utilisant un délai approprié pour votre application) pour toutes les notifications par vignette et par badge, afin de vous assurer que le contenu de votre vignette ne persiste pas plus longtemps que nécessaire. Un délai d’expiration explicite est essentiel pour les contenus dont la durée de vie est limitée. Cette approche assure également la suppression du contenu périmé si votre service cloud arrête d’envoyer des notifications, ou que l’utilisateur se déconnecte du réseau pour une période prolongée.

Votre service cloud peut définir une expiration pour chaque notification en définissant l’en-tête HTTP X-WNS-TTL pour spécifier la durée (en secondes) durant laquelle votre notification restera valide après son envoi. Pour plus d’informations, voir [En-têtes des demandes et des réponses du service de notifications Push](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10)).

Par exemple, au cours d’une journée active d’échanges sur le marché boursier, vous pouvez doubler le délai d’expiration de la mise à jour du cours d’une action par rapport à l’intervalle d’envoi (par exemple une heure après la réception du contenu, si vous envoyez des notifications chaque demi-heure). Autre exemple, dans une application d’infos, le délai d’expiration approprié pour la mise à jour quotidienne des vignettes d’infos est d’une journée.

## <a name="push-notifications-and-battery-saver"></a>Notifications Push et économiseur de batterie


L’économiseur de batterie prolonge l’autonomie en limitant l’activité en arrière-plan sur l’appareil. Windows 10 permet à l’utilisateur de définir économiseur de batterie pour l’activer automatiquement lors de la batterie descend sous un seuil spécifié. Lorsque l’économiseur de batterie est activé, la réception de notifications Push est désactivée afin de réduire la consommation d’énergie. Il y a toutefois quelques exceptions. Les paramètres d’économiseur de batterie Windows 10 suivantes (trouvée dans le **paramètres** application) permettent à votre application recevoir des notifications push même lorsque la batterie de veille est activé.

-   **Autoriser les notifications push à partir de n’importe quelle application en économiseur de batterie**: Ce paramètre permet à toutes les applications de recevoir des notifications push tandis que la batterie de veille est activé. Notez que ce paramètre s’applique uniquement à Windows 10 pour les éditions desktop (accueil, Professionnel, entreprise et éducation).
-   **Toujours autorisé**: Ce paramètre permet à des applications spécifiques d’exécuter en arrière-plan, tandis qu’économiseur de batterie est sur - notamment la réception de notifications push. Cette liste est mise à jour manuellement par l’utilisateur.

Il n’existe aucun moyen de vérifier l’état de ces deux paramètres, mais vous pouvez vérifier l’état de l’économiseur de batterie. Dans Windows 10, utilisez le [ **EnergySaverStatus** ](https://docs.microsoft.com/uwp/api/Windows.System.Power.PowerManager.EnergySaverStatus) propriété pour vérifier l’état de veille de batterie. Votre application peut également utiliser l’événement [**EnergySaverStatusChanged**](https://docs.microsoft.com/uwp/api/Windows.System.Power.PowerManager.EnergySaverStatusChanged) pour détecter les changements d’état de l’économiseur de batterie.

Si votre application s’appuie en grande partie sur les notifications Push, nous vous recommandons d’avertir les utilisateurs qu’ils risquent de ne pas recevoir de notifications lorsque l’économiseur de batterie est activé, et de leur faciliter l’accès aux **paramètres de l’économiseur de batterie**. À l’aide du schéma d’URI de la batterie de veille paramètres dans Windows 10, `ms-settings:batterysaver-settings`, vous pouvez fournir un lien pratique vers l’application paramètres.

**Conseil**    lors de la notification de l’utilisateur sur les paramètres d’économiseur de batterie, nous vous recommandons de fournir un moyen pour supprimer le message à l’avenir. Par exemple, la case à cocher `dontAskMeAgainBox` dans l’exemple suivant conserve la préférence de l’utilisateur dans [**LocalSettings**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData.LocalSettings).

 

Voici un exemple montrant comment vérifier si économiseur de batterie est activé dans Windows 10. Cet exemple avertit l’utilisateur et ouvre l’application Paramètres au niveau des **paramètres de l’économiseur de batterie**. L’élément `dontAskAgainSetting` permet à l’utilisateur de supprimer le message s’il ne souhaite plus être averti.

```cs
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Navigation;
using Windows.System;
using Windows.System.Power;
...
...
async public void CheckForEnergySaving()
{
   //Get reminder preference from LocalSettings
   bool dontAskAgain;
   var localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
   object dontAskSetting = localSettings.Values["dontAskAgainSetting"];
   if (dontAskSetting == null)
   {  // Setting does not exist
      dontAskAgain = false;
   }
   else
   {  // Retrieve setting value
      dontAskAgain = Convert.ToBoolean(dontAskSetting);
   }
   
   // Check if battery saver is on and that it's okay to raise dialog
   if ((PowerManager.EnergySaverStatus == EnergySaverStatus.On)
         && (dontAskAgain == false))
   {
      // Check dialog results
      ContentDialogResult dialogResult = await saveEnergyDialog.ShowAsync();
      if (dialogResult == ContentDialogResult.Primary)
      {
         // Launch battery saver settings (settings are available only when a battery is present)
         await Launcher.LaunchUriAsync(new Uri("ms-settings:batterysaver-settings"));
      }

      // Save reminder preference
      if (dontAskAgainBox.IsChecked == true)
      {  // Don't raise dialog again
         localSettings.Values["dontAskAgainSetting"] = "true";
      }
   }
}
```

Voici le code XAML pour l’élément [**ContentDialog**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) présenté dans cet exemple.

```xaml
<ContentDialog x:Name="saveEnergyDialog"
               PrimaryButtonText="Open battery saver settings"
               SecondaryButtonText="Ignore"
               Title="Battery saver is on."> 
   <StackPanel>
      <TextBlock TextWrapping="WrapWholeWords">
         <LineBreak/><Run>Battery saver is on and you may 
          not receive push notifications.</Run><LineBreak/>
         <LineBreak/><Run>You can choose to allow this app to work normally
         while in battery saver, including receiving push notifications.</Run>
         <LineBreak/>
      </TextBlock>
      <CheckBox x:Name="dontAskAgainBox" Content="OK, got it."/>
   </StackPanel>
</ContentDialog>
```

## <a name="related-topics"></a>Rubriques connexes


* [Envoyer une notification de vignette local](sending-a-local-tile-notification.md)
* [Démarrage rapide : Envoyer une notification push](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10))
* [Comment mettre à jour d’un badge via des notifications push](https://docs.microsoft.com/previous-versions/windows/apps/hh465450(v=win.10))
* [Comment demander, créer et enregistrer un canal de notification](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10))
* [Comment intercepter des notifications pour les applications en cours d’exécution](https://docs.microsoft.com/previous-versions/windows/apps/jj709907(v=win.10))
* [Comment s’authentifier avec Windows Push Notification Service (WNS)](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10))
* [Envoyer des en-têtes de demande et de réponse de service de notification](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))
* [Instructions et liste de vérification pour les notifications push](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
* [Notifications brutes](https://docs.microsoft.com/previous-versions/windows/apps/hh761488(v=win.10))
 

 




