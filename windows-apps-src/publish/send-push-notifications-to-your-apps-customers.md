---
author: JnHs
Description: Learn how to send notifications from Windows Dev Center to your app to encourage groups of customers to take an action, such as rating an app or buying an add-on.
title: Envoyer des notifications Push ciblées aux clients de votre application
ms.author: wdg-dev-content
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, notifications ciblées, notifications push, toast, vignette
ms.assetid: 16386c81-702d-47cd-9f91-67659f5dca73
ms.localizationpriority: medium
ms.openlocfilehash: 9d62f46ad1b55fbad3ab7c21a593625a2538b68f
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/30/2018
ms.locfileid: "3115328"
---
# <a name="send-notifications-to-your-apps-customers"></a>Envoyer des notifications aux clients de votre application

Impliquer vos clients au bon moment et en utilisant le message approprié est la clé de votre réussite en tant que développeur d’applications. Les notifications permettent d’inciter vos clients à exécuter une action, par exemple évaluer une application, acheter une extension, essayer une nouvelle fonctionnalité ou télécharger une autre application (éventuellement gratuitement avec un [code promotionnel](generate-promotional-codes.md) que vous fournissez).

Le Centre de développement Windows offre une plateforme d’implication client pilotée par les données que vous pouvez utiliser pour envoyer des notifications à tous les clients de votre application, ou uniquement à un sous-ensemble ciblé de clients Windows10 de votre application, qui remplissent les critères que vous avez définis dans un [segment de clients](create-customer-segments.md). <!-- You can also send a single notification to all of the customers for multiple apps. -->

> [!IMPORTANT]
> Ces notifications sont uniquement utilisables avec les applications UWP.

Lorsque vous examinez le contenu de vos notifications, tenez compte des points suivants:
- Le contenu de vos notifications doit être conforme avec les [Politiques relatives au contenu](https://docs.microsoft.com/legal/windows/agreements/store-policies#content_policies) du Store.
- Vos notifications ne doivent aucunement comporter d’informations confidentielles ou potentiellement sensibles.
- Si nous faisons de notre mieux pour transmettre les notifications dans les délais prévus, des problèmes de latence peuvent parfois affecter ces remises.
- Veillez à ne pas envoyer trop souvent de notifications. Un intervalle entre les remises inférieur à 30minutes peut sembler intrusif (dans de nombreux scénarios, il est préférable d’observer une fréquence moindre).
- N’oubliez pas que si un client qui utilise votre application (en étant connecté avec son compte Microsoft au moment de la détermination de l’appartenance du segment) prête ultérieurement son appareil à un autre utilisateur, ce dernier a accès à la notification transmise au client d’origine. Pour plus d’informations, voir [Configurer votre application pour recevoir des notifications Push du Centre de développement](../monetize/configure-your-app-to-receive-dev-center-notifications.md#notification-customers).
- Si vous envoyez la même notification aux clients de plusieurs applications, vous ne pouvez pas cibler de segment particulier; la notification est envoyée à tous les clients de l'application sélectionnée.


## <a name="getting-started-with-notifications"></a>Prise en main des notifications

De façon générale, vous devez effectuer trois opérations afin d’utiliser des notifications pour impliquer vos clients.

1. **Inscrivez votre application pour recevoir les notifications Push.** Pour cela, ajoutez une référence au Microsoft Store Services SDK dans votre application, puis quelques lignes de code afin d’inscrire un canal de notification entre le Centre de développement et votre application. Nous utiliserons ce canal pour transmettre vos notifications à vos clients. Pour plus de détails, consultez l’article [Configurer votre application pour les notifications Push ciblées](../monetize/configure-your-app-to-receive-dev-center-notifications.md).
2. **Déterminez quels clients cibler.** Vous pouvez envoyer votre notification à l’ensemble des clients de votre application, ou (pour les notifications créées pour une application unique) à un groupe de clients appelé *segment*, que vous pouvez définir en fonction de critères démographiques ou de revenus. Pour plus d’informations, consultez [Créer des segments de clients](create-customer-segments.md).
3. **Créez le contenu de votre notification et envoyez-la.** Par exemple, vous pouvez créer une notification qui encourage les nouveaux clients à évaluer votre application, ou envoyer une notification pour promouvoir une offre spéciale d’achat d’une extension.


## <a name="to-create-and-send-a-notification"></a>Pour créer et envoyer une notification

Suivez cette procédure pour créer une notification dans le tableau de bord et l’envoyer à un segment de clients spécifique.

> [!NOTE]
> Pour qu’une application puisse recevoir des notifications du Centre de développement, vous devez commencer par appeler la méthode [RegisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) dans votre application afin d’y configurer la réception des notifications. Cette méthode est disponible dans le [Microsoft Store Services SDK](http://aka.ms/store-em-sdk). Pour plus d’informations sur l’appel de cette méthode, notamment pour consulter un exemple de code, consultez l’article [Configurer votre application pour les notifications Push ciblées](../monetize/configure-your-app-to-receive-dev-center-notifications.md).

1. Dans le [tableau de bord du Centre de développement Windows](https://partner.microsoft.com/dashboard/), développez la section **Engager**, puis sélectionnez **Notifications**.
2. Sur la page **Notifications**, sélectionnez **Nouvelle notification**.
3. Dans la section **Sélectionnez un modèle** , choisissez le [type de notification](#notification-template-types) que vous souhaitez envoyer, puis cliquez sur **OK**.
4. À la page suivante, utilisez le menu déroulant pour choisir une **Application unique** ou **Plusieurs applications** pour lesquels vous souhaitez générer une notification. Vous pouvez sélectionner uniquement les applications qui ont été [configurés pour recevoir des notifications à l’aide du Microsoft Store Services SDK](../monetize/configure-your-app-to-receive-dev-center-notifications.md).
5. Dans la section **Paramètres de notification**, choisissez un **nom** pour votre notification et, si applicable, choisissez le **groupe de clients** à qui vous souhaitez envoyer la notification. (Les notifications envoyées à plusieurs applications peuvent uniquement être envoyées à tous les clients de ces applications.) Si vous souhaitez utiliser un segment que vous n'avez pas encore créé, sélectionnez **Créer un nouveau groupe client**. Remarque: Il faut 24heures avant qu’un nouveau segment puisse être utilisé pour les notifications. Pour plus d’informations, consultez [Créer des segments de clients](create-customer-segments.md).
6. Si vous voulez indiquer à quel moment envoyer la notification, décochez la case **Notification immédiate** et choisissez une date et une heure spécifiques (au format UTC pour tous les clients, sauf si vous indiquez d’utiliser le fuseau horaire local de chaque client).
7. Si vous voulez que la notification expire à un moment donné, décochez la case **La notification n’expire jamais** et choisissez une date et une heure d’expiration spécifiques (au format UTC).
8. **Pour les notifications d'une application unique:** Si vous souhaitez filtrer les destinataires afin que votre notification soit envoyée uniquement aux personnes qui utilisent certaines langues ou qui se trouvent dans des fuseaux horaires spécifiques, cochez la case **Utiliser des filtres**. Vous pouvez ensuite spécifier les options de langues et/ou de fuseaux horaires que vous souhaitez utiliser.
8. **Pour les notifications de plusieurs applications:** Indiquez si vous souhaitez envoyer la notification uniquement à la dernière application active sur chaque appareil (par client) ou à toutes les applications sur chaque appareil.
10. Dans la section **Contenu de la notification**, dans le menu **Langue**, choisissez les langues dans lesquelles vous souhaitez que votre notification s’affiche. Pour plus d’informations, voir [Traduire vos notifications](#translate-your-notifications).
11. Dans la section **Options**, entrez du texte et configurez toutes les autres options que vous souhaitez. Si vous avez commencé avec un modèle, certaines de ces options sont fournies par défaut, mais vous pouvez apporter des modifications si vous le souhaitez.

    Les options disponibles varient selon le type de notification que vous utilisez. Voici certaines des options:

    * **Type d’activation** (notification de type toast interactif). Vous pouvez choisir **Premier plan**, **Arrière-plan**, ou **Protocole**.
    * **Lancer** (notification de type toast interactif). Vous pouvez décider que la notification ouvre une application ou un site web.
    * **Suivre la fréquence de lancement d’application** (notification de type toast interactif). Si vous souhaitez mesurer l’implication de vos clients par le biais de chaque notification, activez cette case à cocher. Pour plus d’informations, voir [Mesurer les performances des notifications](#measure-notification-performance).
    * **Durée** (notification de type toast interactif). Vous pouvez choisir **Court** ou **Long**.
    * **Scénario** (notification de type toast interactif). Vous pouvez choisir **Par défaut**, **Alarme**, **Rappel** ou **Appel entrant**.
    * **URI de base** (notification de type toast interactif). Pour plus d’informations, voir [BaseUri](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.baseuri#Windows_UI_Xaml_FrameworkElement_BaseUri).
    * **Ajouter une demande d’image** (notification de type toast interactif). Pour plus d’informations, voir [addImageQuery](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-visual#attributes-and-elements).
    * **Éléments visuels**. Une image, une vidéo ou un son. Pour plus d’informations, voir [visual](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-visual).
    * **Entrée**/**Action**/**Sélection** (notification de type toast interactif). Vous permet de laisser les utilisateurs interagir avec la notification. Pour plus d’informations, voir [Notifications toast adaptatives et interactives](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md).
    * **Liaison** (notification de type vignette interactive). Le modèle de toast. Pour plus d’informations, voir [liaison](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-binding).

    > [!TIP]
    > Essayez d’utiliser l’application [Notifications Visualizer](https://www.microsoft.com/store/apps/9nblggh5xsl1) pour concevoir et tester vos notifications de type vignettes adaptatives et toasts interactifs.

12. Sélectionnez **Enregistrer comme brouillon** pour continuer à travailler sur la notification plus tard, ou sélectionnez **Envoyer** si vous avez terminé.


## <a name="notification-template-types"></a>Types de modèles de notification

Vous pouvez choisir parmi différents modèles de notification.

-   **Vierge (toast).** Commencez avec une notification toast vide que vous pouvez personnaliser. Une notification toast est un élément d’interface utilisateur contextuel qui apparaît sur votre écran pour permettre à l’application de communiquer avec le client quand il se trouve dans une autre application, sur l’écran d’accueil ou sur le Bureau.
-   **Vierge (vignette).** Commencez avec une notification vignette vide que vous pouvez personnaliser. Les vignettes correspondent à la représentation d’une application sur l’écran d’accueil. Les vignettes peuvent être «dynamiques», ce qui signifie que le contenu qu’elles affichent peut changer suite à des notifications.
-   **Demander des évaluations (toast).** Une notification toast qui demande à vos clients d’évaluer votre application. Lorsque le client sélectionne la notification, la page d’évaluation de votre application dans la boutique s’affiche.
-   **Demander des commentaires (toast).** Une notification toast qui demande à vos clients de donner leur avis sur votre application. Lorsque le client sélectionne la notification, la page Hub de commentaires de votre application s’affiche.
    > [!NOTE]
    > Si vous choisissez ce type de modèle, dans la zone **Lancer**, n’oubliez pas de remplacer l’espace réservé {PACKAGE_FAMILY_NAME} par le nom de la famille de packages (PFN) réel de votre application. Vous trouverez le PFN de votre application sur la page [Identité des applications](view-app-identity-details.md) (**Gestion des applications** > **Identité des applications**).

    ![Zone de lancement de la notification toast de commentaire](images/push-notifications-feedback-toast-launch-box.png)

-   **Promouvoir (toast).** Une notification toast visant à promouvoir une autre application de votre choix. Lorsque le client sélectionne la notification, la description de l’application dans le Store s’affiche.
    > [!NOTE]
    > Si vous choisissez ce type de modèle, dans la zone **Lancer**, n’oubliez pas de remplacer l’espace réservé **{ID du produit à promouvoir ici}** par l’ID Store réel de l’élément que vous voulez promouvoir. Vous trouverez l’ID sur la page [Identité des applications](view-app-identity-details.md) (**Gestion des applications** > **Identité des applications**).

    ![Zone de lancement de la notification toast de promotion](images/push-notifications-promote-toast-launch-box.png)

-   **Promouvoir une vente (toast).** Une notification toast que vous pouvez utiliser pour présenter une offre concernant votre application. Lorsque le client sélectionne la notification, la description de votre application dans la boutique s’affiche.
-   **Proposition de mise à jour (toast).** Une notification toast qui encourage les clients utilisant une ancienne version de votre application à installer la dernière version. Lorsque le client sélectionne la notification, l’application Store est lancée et affiche la liste **Téléchargements et mises à jour**. Notez que ce modèle peut uniquement être utilisé avec une seule application et que vous ne pouvez pas cibler un segment de clients particulier ni définir une heure d’envoi. Nous planifierons toujours l’envoi de cette notification dans les 24heures et mettrons tout en œuvre pour cibler tous les utilisateurs qui n’utilisent pas encore la dernière version de votre application.


## <a name="measure-notification-performance"></a>Mesurer les performances des notifications

Vous pouvez mesurer l’implication de vos clients par le biais de chaque notification.


### <a name="to-measure-notification-performance"></a>Pour mesurer les performances des notifications

1.  Lorsque vous créez une notification, dans la section **Contenu de la notification**, cochez la case **Suivre la fréquence de lancement d’application**.
2.  Dans votre application, appelez la méthode [ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) pour informer le Centre de développement que votre application a été lancée en réponse à une notification ciblée. Cette méthode est fournie par le Microsoft Store Services SDK. Pour plus d’informations sur l’appel de cette méthode, voir [Configurer votre application pour recevoir les notifications du Centre de développement](../monetize/configure-your-app-to-receive-dev-center-notifications.md).


### <a name="to-view-notification-performance"></a>Pour afficher les performances des notifications

Une fois que vous avez configuré la notification et votre application afin de mesurer les performances de la notification comme décrit ci-dessus, vous pouvez utiliser le tableau de bord pour afficher les performances de vos notifications.

Pour passer en revue des données détaillées pour chaque notification:

1.  Dans le tableau de bord du Centre de développement Windows, développez la section **Engager**, puis sélectionnez **Notifications**.
2.  Dans la table des notifications existantes, sélectionnez **en cours** ou **terminé**et puis examinez les colonnes de **vitesse de transmission** et **application lancer taux** pour afficher les performances de haut niveau de chaque notification.
3.  Pour afficher des informations plus granulaires sur les performances, sélectionnez le nom d’une notification. Dans la section **Statistiques de livraison**, vous pouvez visualiser le **nombre** et le **pourcentage** pour les types d’**états** de notification suivants:
    * **Échec**: la notification n’a pas été transmise pour une raison quelconque. Cela peut se produire, par exemple, si un problème survient dans le service de notification Windows.
    * **Échec dû à l’expiration du canal**: la notification n’a pas pu être transmise car le canal entre l’application et le Centre de développement a expiré. Cela peut se produire, par exemple, si le client n’a pas ouvert votre application depuis longtemps.
    * **Envoi**: la notification est dans la file d’attente pour être envoyée.
    * **Envoyée**: la notification a été envoyée.
    * **Lancée**: la notification a été envoyée, le client a cliqué dessus et votre application s’est ouverte. Notez que cette option suit uniquement les lancements de l’application. Les notifications qui invitent le client à effectuer d’autres actions (ouvrir la boutique pour laisser une évaluation, par exemple) ne sont pas comptabilisées dans cet état.
    * **Inconnu**: nous n’avons pas pu déterminer l’état de cette notification.

Pour analyser les données de l’activité utilisateur pour toutes vos notifications:

1.  Dans le tableau de bord du Centre de développement Windows, développez la section **Engager**, puis sélectionnez **Notifications**.
2.  Dans la page **Notifications** , cliquez sur l’onglet **analyse** . Cet onglet affiche les données suivantes:
    * Vues des différents états utilisateur action pour vos toasts et les notifications du centre du graphique.
    * Vues de carte monde clic par le biais de taux pour vos toasts et l’action de centre de notifications.
3. Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les données qui vous intéressent. La valeur par défaut est de 30D (30jours), mais vous pouvez choisir d’afficher les données portant sur des périodes de 3, 6 ou 12mois, ou sur une plage de dates personnalisée que vous spécifiez. Vous pouvez également développer des **filtres** pour filtrer toutes les données par application et par marché.

## <a name="translate-your-notifications"></a>Traduire vos notifications

Afin de maximiser l’impact de vos notifications, pensez à les traduire dans les langues préférées de vos clients. Le Centre de développement vous permet de traduire facilement vos notifications de manière automatique en tirant parti de la puissance du service [Microsoft Translator](https://www.microsoft.com/translator/home.aspx).

1.  Une fois votre notification rédigée dans votre langue par défaut, sélectionnez **Ajouter des langues** (sous le menu **Langues** dans la section **Contenu de la notification**).
2.  Dans la fenêtre **Ajouter des langues**, sélectionnez les langues supplémentaires dans lesquelles vous voulez que vos notifications apparaissent, puis sélectionnez **Mettre à jour**.
Votre notification sera automatiquement traduite dans les langues que vous avez choisies dans la fenêtre **Ajouter des langues** et ces langues seront ajoutées au menu **Langue**.
3.  Pour afficher la traduction de votre notification, dans le menu **Langue**, sélectionnez la langue que vous venez d’ajouter.

Éléments à garder à l’esprit concernant la traduction:
 - Vous pouvez remplacer la traduction automatique en saisissant un autre texte dans la zone **Contenu** pour cette langue.
 - Si vous ajoutez une autre zone de texte à la version anglaise de la notification après avoir remplacé une traduction automatique, la nouvelle zone de texte ne sera pas ajoutée à la notification traduite. Dans ce cas, vous devez ajouter manuellement la nouvelle zone de texte à chacune des notifications traduites.
 - Si vous modifiez le texte anglais une fois que la notification a été traduite, nous mettrons automatiquement à jour les notifications traduites pour prendre en compte la modification. Cependant, cette action ne s’effectuera pas si vous avez précédemment remplacé la traduction initiale.

## <a name="related-topics"></a>Rubriquesassociées
- [Vignettes pour les applications UWP](../design/shell/tiles-and-notifications/creating-tiles.md)
- [Vue d’ensemble des services de notifications Push Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)
- [Application Notifications Visualizer](https://www.microsoft.com/store/apps/9nblggh5xsl1)
- [StoreServicesEngagementManager.RegisterNotificationChannelAsync() | méthode registerNotificationChannelAsync()](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync)
