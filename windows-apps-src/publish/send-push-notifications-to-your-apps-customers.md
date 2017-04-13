---
author: JnHs
Description: "Découvrez comment envoyer des notifications Push ciblées à votre application à partir du Centre de développement Windows pour encourager les clients à effectuer une action, par exemple évaluer une application ou acheter un module complémentaire."
title: "Envoyer des notifications Push ciblées aux clients de votre application"
ms.author: wdg-dev-content
ms.date: 02/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 16386c81-702d-47cd-9f91-67659f5dca73
ms.openlocfilehash: ca57ff45d440ebd68f7fb85b7d6a5da0a9f1995c
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="send-targeted-push-notifications-to-your-apps-customers"></a>Envoyer des notifications Push ciblées aux clients de votre application

Impliquer vos clients au bon moment et en utilisant le message approprié est la clé de votre réussite en tant que développeur d’applications. Le Centre de développement Windows offre une plateforme d’implication client pilotée par les données que vous pouvez utiliser pour envoyer des notifications Push à tous vos clients ou uniquement à un sous-ensemble de vos clients Windows10 qui remplissent les critères que vous avez définis dans un [segment de clients](create-customer-segments.md).

Vous pouvez utiliser des notifications Push ciblées afin d’inciter vos clients à effectuer une action, par exemple évaluer une application, acheter un module complémentaire, essayer une nouvelle fonctionnalité ou télécharger une autre application.

> **Important** Les notifications Push ciblées peuvent uniquement être utilisées avec les applications UWP.

Lorsque vous examinez le contenu de vos notifications, tenez compte des points suivants:
- Le contenu de vos notifications doit être conforme avec les [Politiques relatives au contenu](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#content_policies) du Windows Store.
- Vos notifications ne doivent aucunement comporter d’informations confidentielles ou potentiellement sensibles.
- Si nous faisons de notre mieux pour transmettre les notifications dans les délais prévus, des problèmes de latence peuvent parfois affecter ces remises.
- Veillez à ne pas envoyer trop souvent de notifications. Un intervalle entre les remises inférieur à 30minutes peut sembler intrusif (dans de nombreux scénarios, il est préférable d’observer une fréquence moindre).
- N’oubliez pas que si un client qui utilise votre application (en étant connecté avec son compte Microsoft au moment de la détermination de l’appartenance du segment) prête ultérieurement son appareil à un autre utilisateur, ce dernier a accès à la notification transmise au client d’origine. Pour plus d’informations, voir [Configurer votre application pour recevoir des notifications Push du Centre de développement](../monetize/configure-your-app-to-receive-dev-center-notifications.md#notification-customers).

## <a name="getting-started-with-push-notifications"></a>Prise en main des notifications Push

De façon générale, vous devez effectuer trois opérations afin d’utiliser des notifications Push pour impliquer vos clients.
1. **Inscrivez votre application pour recevoir les notifications Push.** Pour cela, ajoutez une référence au Microsoft Store Services SDK dans votre application, puis quelques lignes de code afin d’inscrire un canal de notification entre le Centre de développement et votre application. Nous utiliserons ce canal pour transmettre vos notifications Push à vos clients. Pour en savoir plus, voir [Configurer votre application pour recevoir des notifications du Centre de développement](../monetize/configure-your-app-to-receive-dev-center-notifications.md).
2. **Créez un ou plusieurs segments de clients que vous voulez cibler.** Vous pouvez regrouper vos clients en segments en fonction de critères démographiques ou de revenus. Pour plus d’informations, voir [Créer des segments de clients](create-customer-segments.md).
3. **Créez une notification Push et envoyez-la à un segment de clients donné.** Par exemple, envoyez une notification pour encourager les nouveaux clients à évaluer votre application ou envoyez une notification contenant une offre spéciale pour acheter un module complémentaire.

## <a name="to-create-and-send-a-targeted-push-notification"></a>Pour créer et envoyer une notification Push ciblée

Suivez cette procédure pour créer une notification Push dans le tableau de bord et l’envoyer à un segment de clientèle particulier.

> **Remarque** Pour que votre application puisse recevoir des notifications Push ciblées du Centre de développement, vous devez appeler la méthode [RegisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx) dans votre application afin d’y configurer la réception des notifications. Cette méthode est disponible dans le [Microsoft Store Services SDK](http://aka.ms/store-em-sdk). Pour plus d’informations sur l’appel de cette méthode, notamment pour consulter un exemple de code, voir [Configurer votre application pour recevoir des notifications du Centre de développement](../monetize/configure-your-app-to-receive-dev-center-notifications.md).

1.    Dans le [tableau de bord du Centre de développement Windows](https://developer.microsoft.com/dashboard/overview), sélectionnez votre application.
2.    Dans le menu de navigation de gauche, développez **Services**, puis sélectionnez **Notifications Push**.
3.    Sur la page **Notifications Push ciblées**, sélectionnez **Nouvelle notification**.
4.    Dans la section **Sélectionner un modèle**, choisissez le type de notification que vous voulez envoyer. Pour plus d’informations, voir [Types de modèles de notification](#notification-template-types).
  ![Modèles de notification](images/push-notifications-template.png)
5.    Dans la section **Paramètres de notification**, choisissez un **nom** pour votre notification et choisissez le **groupe de clients** à qui vous souhaitez envoyer la notification.
Si vous n’avez pas encore créé de segment, sélectionnez **Créer un nouveau groupe de clients**. Remarque: il faut 24heures pour qu’un nouveau segment puisse être utilisé pour les notifications. Pour plus d’informations, voir [Créer des segments de clients](create-customer-segments.md).
6.    Si vous voulez indiquer à quel moment envoyer la notification, décochez la case **Notification immédiate** et choisissez une date et une heure spécifiques.
7.    Si vous voulez que la notification expire à un moment donné, décochez la case **La notification n’expire jamais** et choisissez une date et une heure d’expiration.
8.    Dans la section **Contenu de la notification**, dans le menu **Langue**, choisissez les langues dans lesquelles vous souhaitez que votre notification s’affiche. Pour plus d’informations, voir [Traduire vos notifications](#translate-your-notifications).
9.    Dans la section **Options**, entrez du texte et configurez toutes les autres options que vous souhaitez. Si vous avez commencé avec un modèle, certaines de ces options sont fournies par défaut, mais vous pouvez apporter des modifications si vous le souhaitez.
   Les options disponibles varient selon le type de notification que vous utilisez. Voici certaines des options:
   - **Type d’activation** (notification de type toast interactif). Vous pouvez choisir **Premier plan**, **Arrière-plan**, ou **Protocole**.
   - **Lancer** (notification de type toast interactif). Vous pouvez décider que la notification ouvre une application ou un site web.
   - **Suivre la fréquence de lancement d’application** (notification de type toast interactif). Si vous souhaitez mesurer l’implication de vos clients par le biais de chaque notification, activez cette case à cocher. Pour plus d’informations, voir [Mesurer les performances des notifications](#measure-notification-performance).
   - **Durée** (notification de type toast interactif). Vous pouvez choisir **Court** ou **Long**.
   - **Scénario** (notification de type toast interactif). Vous pouvez choisir **Par défaut**, **Alarme**, **Rappel** ou **Appel entrant**.
   - **URI de base** (notification de type toast interactif). Pour plus d’informations, voir [BaseUri](https://msdn.microsoft.com/library/windows/apps/br208712).
   - **Ajouter une demande d’image** (notification de type toast interactif). Pour plus d’informations, voir [addImageQuery](https://msdn.microsoft.com/library/windows/apps/br230847).
   - **Éléments visuels**. Une image, une vidéo ou un son. Pour plus d’informations, voir [visual](https://msdn.microsoft.com/library/windows/apps/br230847).
   - **Entrée**/**Action**/**Sélection** (notification de type toast interactif). Vous permet de laisser les utilisateurs interagir avec la notification. Pour plus d’informations, voir [Notifications toast adaptatives et interactives](../controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts.md#actions).
   - **Liaison** (notification de type vignette interactive). Le modèle de toast. Pour plus d’informations, voir [binding](https://msdn.microsoft.com/library/windows/apps/br230843).

   > **Conseil** Essayez d’utiliser l’application [Notifications Visualizer](https://www.microsoft.com/store/apps/9nblggh5xsl1) pour concevoir et tester vos notifications de type vignettes adaptatives et toasts interactifs.

10.    Sélectionnez **Enregistrer comme brouillon** pour continuer à travailler sur la notification plus tard, ou sélectionnez **Envoyer** si vous avez terminé.

## <a name="notification-template-types"></a>Types de modèles de notification

Vous pouvez choisir parmi différents modèles de notification.

-    **Vierge (toast).** Commencez avec une notification toast vide que vous pouvez personnaliser. Une notification toast est un élément d’interface utilisateur contextuel qui apparaît sur votre écran pour permettre à l’application de communiquer avec le client quand il se trouve dans une autre application, sur l’écran d’accueil ou sur le Bureau.
-    **Vierge (vignette).** Commencez avec une notification vignette vide que vous pouvez personnaliser. Les vignettes correspondent à la représentation d’une application sur l’écran d’accueil. Les vignettes peuvent être «dynamiques», ce qui signifie que le contenu qu’elles affichent peut changer suite à des notifications.
-    **Demander des évaluations (toast).** Une notification toast qui demande à vos clients d’évaluer votre application. Lorsque le client sélectionne la notification, la page d’évaluation de votre application dans la boutique s’affiche.
-    **Demander des commentaires (toast).** Une notification toast qui demande à vos clients de donner leur avis sur votre application. Lorsque le client sélectionne la notification, la page Hub de commentaires de votre application s’affiche.
   > **Remarque** Si vous choisissez ce type de modèle, dans la zone **Lancer**, n’oubliez pas de remplacer l’espace réservé {PACKAGE_FAMILY_NAME} par le nom de la famille de packages (PFN) réel de votre application. Vous trouverez le PFN de votre application sur la page [Identité des applications](view-app-identity-details.md) (**Gestion des applications** > **Identité des applications**).

   ![Zone de lancement de la notification toast de commentaire](images/push-notifications-feedback-toast-launch-box.png)
-    **Promouvoir (toast).** Une notification toast visant à promouvoir une autre application de votre choix. Lorsque le client sélectionne la notification, la description de l’application dans la boutique s’affiche.
  > **Remarque** Si vous choisissez ce type de modèle, dans la zone **Lancer**, n’oubliez pas de remplacer l’espace réservé **{ID du produit à promouvoir ici}** par l’ID réel dans la boutique de l’élément que vous voulez promouvoir. Vous trouverez l’ID sur la page [Identité des applications](view-app-identity-details.md) (**Gestion des applications** > **Identité des applications**).

  ![Zone de lancement de la notification toast de promotion](images/push-notifications-promote-toast-launch-box.png)
-    **Promouvoir une vente (toast).** Une notification toast que vous pouvez utiliser pour présenter une offre concernant votre application. Lorsque le client sélectionne la notification, la description de votre application dans la boutique s’affiche.
- **Proposition de mise à jour (toast).** Une notification toast qui encourage les clients utilisant une ancienne version de votre application à installer la dernière version. Lorsque le client sélectionne la notification, la liste **Téléchargements et mises à jour** de l’application s’affiche. Notez que vous n’avez pas besoin de créer un segment de clients pour utiliser ce modèle. Nous allons planifier cette notification sous 24heures et nous mettrons tout en œuvre pour cibler tous les utilisateurs qui ne disposent pas encore de la dernière version de votre application.

## <a name="measure-notification-performance"></a>Mesurer les performances des notifications

Vous pouvez mesurer l’implication de vos clients par le biais de chaque notification.

###<a name="to-measure-notification-performance"></a>Pour mesurer les performances des notifications

1.    Lorsque vous créez une notification, dans la section **Contenu de la notification**, cochez la case **Suivre la fréquence de lancement d’application**.
2.    Dans votre application, appelez la méthode [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx) pour informer le Centre de développement que votre application a été lancée en réponse à une notification ciblée. Cette méthode est fournie par le Kit de développement logiciel de la Boutique Microsoft. Pour plus d’informations sur l’appel de cette méthode, voir [Configurer votre application pour recevoir les notifications du Centre de développement](../monetize/configure-your-app-to-receive-dev-center-notifications.md).

###<a name="to-view-notification-performance"></a>Pour afficher les performances des notifications

Une fois que vous avez configuré la notification et votre application afin de [mesurer les performances de la notification](#to-measure-notification-performance) comme décrit ci-dessus, vous pouvez utiliser le tableau de bord pour afficher les performances de vos notifications.

1.  Dans le tableau de bord, sélectionnez l’une de vos applications.
2.  Développez la section **Services** dans le menu de gauche, puis sélectionnez **Notifications Push** pour afficher les notifications associées à cette application.
3.    Sur la page **Notifications Push ciblées**, sélectionnez **En cours** ou **Terminé**, puis consultez la **vitesse de transmission** et la **fréquence de lancement de l’application** pour voir les performances de haut niveau de chaque notification.
4.  Pour afficher des informations plus granulaires sur les performances, sélectionnez le nom d’une notification. La section **Statistiques de livraison** s’affiche et indique un **nombre** et un **pourcentage** pour les types d’**états** de notification suivants:
 - **Échec**: la notification n’a pas été transmise pour une raison quelconque. Cela peut se produire, par exemple, si un problème survient dans le service de notification Windows.
 - **Échec dû à l’expiration du canal**: la notification n’a pas pu être transmise car le canal entre l’application et le Centre de développement a expiré. Cela peut se produire, par exemple, si le client n’a pas ouvert votre application depuis longtemps.
 - **Envoi**: la notification est dans la file d’attente pour être envoyée.
 - **Envoyée**: la notification a été envoyée.
 - **Lancée**: la notification a été envoyée, le client a cliqué dessus et votre application s’est ouverte. Notez que cette option suit uniquement les lancements de l’application. Les notifications qui invitent le client à effectuer d’autres actions (ouvrir la boutique pour laisser une évaluation, par exemple) ne sont pas comptabilisées dans cet état.
 - **Inconnu**: nous n’avons pas pu déterminer l’état de cette notification.

## <a name="translate-your-notifications"></a>Traduire vos notifications

Afin de maximiser l’impact de vos notifications, pensez à les traduire dans les langues préférées de vos clients. Le Centre de développement vous permet de traduire facilement vos notifications de manière automatique en tirant parti de la puissance du service [Microsoft Translator](https://msdn.microsoft.com/library/dd576287.aspx).

1.    Une fois votre notification rédigée dans votre langue par défaut, sélectionnez **Ajouter des langues** (sous le menu **Langues** dans la section **Contenu de la notification**).
2.    Dans la fenêtre **Ajouter des langues**, sélectionnez les langues supplémentaires dans lesquelles vous voulez que vos notifications apparaissent, puis sélectionnez **Mettre à jour**.
Votre notification sera automatiquement traduite dans les langues que vous avez choisies dans la fenêtre **Ajouter des langues** et ces langues seront ajoutées au menu **Langue**.
3.    Pour afficher la traduction de votre notification, dans le menu **Langue**, sélectionnez la langue que vous venez d’ajouter.

Éléments à garder à l’esprit concernant la traduction:
 - Vous pouvez remplacer la traduction automatique en saisissant un autre texte dans la zone **Contenu** pour cette langue.
 - Si vous ajoutez une autre zone de texte à la version anglaise de la notification après avoir remplacé une traduction automatique, la nouvelle zone de texte ne sera pas ajoutée à la notification traduite. Dans ce cas, vous devez ajouter manuellement la nouvelle zone de texte à chacune des notifications traduites.
 - Si vous modifiez le texte anglais une fois que la notification a été traduite, nous mettrons automatiquement à jour les notifications traduites pour prendre en compte la modification. Cependant, cette action ne s’effectuera pas si vous avez précédemment remplacé la traduction initiale.

## <a name="related-topics"></a>Rubriques connexes
- [Vignettes, badges et notifications pour les applications UWP](../controls-and-patterns/tiles-badges-notifications.md)
- [Vue d’ensemble des services de notifications Push Windows (WNS)](../controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview.md)
- [Vue d’ensemble des services de notifications Push Windows (WNS) (applications Windows Runtime)](https://msdn.microsoft.com/library/windows/apps/hh913756.aspx)
- [Application Notifications Visualizer](https://www.microsoft.com/store/apps/9nblggh5xsl1)
- [StoreServicesEngagementManager.RegisterNotificationChannelAsync() | méthode registerNotificationChannelAsync()](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx)
- [Segmentation des clients et notifications Push: une nouvelle fonctionnalité du programme Insider du Centre de développement Windows (billet de blog)](https://blogs.windows.com/buildingapps/2016/08/17/customer-segmentation-and-push-notifications-a-new-windows-dev-center-insider-program-feature/#XTuCqrG8G5IMgWew.97)
