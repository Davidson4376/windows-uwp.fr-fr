---
author: drewbatgit
ms.assetid: 
description: "Cet article vous explique comment lire du contenu multimédia pendant l’exécution de votre application en arrière-plan."
title: "Lire du contenu multimédia en arrière-plan"
translationtype: Human Translation
ms.sourcegitcommit: c8cbc538e0979f48b657197d59cb94a90bc61210
ms.openlocfilehash: a477827553ac1780ac625deeee08d84ab638d4c2

---

# Lire du contenu multimédia en arrière-plan
Cet article vous explique comment configurer votre application de telle sorte que le contenu multimédia continue à être lu quand votre application est déplacée du premier plan vers l’arrière-plan. Cela signifie que même après que l’utilisateur a réduit votre application, est revenu à l’écran d’accueil ou a quitté votre application d’une autre manière, votre application peut continuer à lire le contenu audio. 

Scénarios de lecture audio en arrière-plan:

-   **Playslist de longue durée:** l’utilisateur affiche brièvement une application au premier plan pour sélectionner et lancer une playslist, puis veut que la lecture de la playslist continue en arrière-plan.

-   **Utilisation du Sélecteur de tâches:** l’utilisateur affiche brièvement une application au premier plan pour démarrer la lecture d’un contenu audio, puis passe dans une autre application ouverte à l’aide du Sélecteur de tâches. Il veut que la lecture du contenu audio continue en arrière-plan.

L’implémentation audio en arrière-plan décrite dans cet article permettra à votre application de s’exécuter universellement sur tous les appareils Windows, y compris les appareils mobiles, de bureau et Xbox.

> [!NOTE]
> Le code de cet article a été adapté de l’[exemple Contenu audio en arrière-plan](http://go.microsoft.com/fwlink/p/?LinkId=800141) UWP.

## Explication du modèle à processus unique.
Avec Windows10, version 1607, un nouveau modèle à processus unique simplifie considérablement la prise en charge de l’audio d’arrière-plan. Auparavant, votre application devait gérer un processus en arrière-plan en plus de l’application de premier plan. De votre côté, vous deviez communiquer manuellement les modifications d’état de communication entre les deuxprocessus. Sous le nouveau modèle, vous ajoutez simplement la capacité d’audio d’arrière-plan à votre manifeste d’application, de manière à ce que votre application continue à lire le contenu audio lorsqu’elle se déplace vers l’arrière-plan. Deuxévénements de cycle de vie d’application, [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) et [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground), indiquent à votre application les moments d’entrée et de sortie de l’arrière-plan. Quand votre application se déplace au sein des transitions à destination et en provenance de l’arrière-plan, les contraintes de mémoire mises en place par le système peuvent être modifiées, afin que vous puissiez utiliser ces événements pour évaluer votre consommation courante de mémoire et libérer des ressources vous permettant de rester sous la limite.

En éliminant les activités complexes de communication intraprocessus et de gestion de l’état, le nouveau modèle vous permet d’implémenter l’audio d’arrière-plan bien plus rapidement, via une réduction considérable du code. Toutefois, le modèle à deuxprocessus est toujours pris en charge pour la compatibilité descendante dans la version actuelle. Pour plus d’informations, consultez la page [Contenu audio en arrière-plan](background-audio.md).

## Conditions requises pour l’audio d’arrière-plan
Votre application doit satisfaire les exigences suivantes associées à la lecture de contenu audio durant sa mise en arrière-plan.

* Ajoutez la fonctionnalité de **Lecture de médias en arrière-plan** à votre manifeste d’application, tel que décrit plus bas dans cet article.
* Si votre application désactive l’intégration automatique de **MediaPlayer** avec les contrôles de transport de média système, en définissant par exemple la propriété [**CommandManager.IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) sur False, vous devez implémenter l’intégration manuelle avec les contrôles de transport de média système afin de prendre en charge la lecture de médias en arrière-plan. Vous devez également procéder à une intégration manuelle avec les contrôles de transport de média système si vous utilisez une API différente de **MediaPlayer**, telle que [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioGraph), afin de lire du contenu audio de manière ininterrompue durant la mise en arrière-plan de votre application. La configuration minimale requise en matière d’intégration des contrôles de transport de média système est décrite dans la section «Utiliser les contrôles de transport de média système pour le son en arrière-plan» de [Contrôle manuel des contrôles de transport de média système](system-media-transport-controls.md).
* Pendant que votre application est en arrière-plan, vous devez rester sous les limites d’utilisation de la mémoire définies par le système pour les applications en arrière-plan. Les recommandations en matière de gestion de la mémoire pendant la mise en arrière-plan sont fournies plus loin dans cet article.

## Fonctionnalité de manifeste de lecture de médias en arrière-plan
Pour activer l’audio en arrière-plan, vous devez ajouter la fonctionnalité de lecture de médias en arrière-plan au fichier du manifeste d’application, Package.appxmanifest. 

**Pour ajouter des fonctionnalités au manifeste d’application à l’aide du concepteur du manifeste**

1.  Dans Microsoft Visual Studio, dans l’**Explorateur de solutions**, ouvrez le concepteur pour le manifeste de l’application en double-cliquant sur l’élément **package.appxmanifest**.
2.  Sélectionnez l’onglet **Fonctionnalités**.
3.  Sélectionnez la case **Lecture de médias en arrière-plan**.

Pour définir la fonctionnalité en modifiant manuellement le manifeste xml de l’application, assurez-vous que le préfixe d’espace de noms *uap3* est défini dans l’élément **Package**. Si ce n’est pas le cas, ajoutez-le tel qu’illustré ci-dessous.
```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
```

Ensuite, ajoutez la fonctionnalité *backgroundMediaPlayback* à l’élément **Capabilities**:
```xml
<Capabilities>
    <uap3:Capability Name="backgroundMediaPlayback"/>
</Capabilities>
```

##Gérer la transition entre le premier plan et l’arrière-plan
Lorsque votre application passe du premier plan à l’arrière-plan, l’événement [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) est déclenché. Quand votre application revient au premier plan, l’événement [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) est déclenché. Ces événements étant liés au cycle de vie de l’application, vous devez enregistrer des gestionnaires dédiés lors de sa création. Dans le modèle de projet par défaut, vous devrez procéder à leur ajout dans le constructeur de la classe **App**, dans App.xaml.cs. L’exécution en arrière-plan impliquant la réduction des ressources de mémoire que votre application est autorisée à conserver au niveau du système, vous devez également procéder à une inscription pour les événements [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) et [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging), qui seront utilisés pour vérifier l’utilisation courante de la mémoire par l’application et la limite courante. Les gestionnaires de ces événements sont affichés dans les exemples suivants. Pour plus d’informations sur le cycle de vie des applicationsUWP, consultez la page [Cycle de vie de l’application](../\launch-resume\app-lifecycle.md).

[!code-cs[RegisterEvents](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetRegisterEvents)]

Créez une variable affectée à la détection de l’exécution en arrière-plan.

[!code-cs[DeclareBackgroundMode](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetDeclareBackgroundMode)]

Lorsque l’événement [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) est déclenché, définissez la variable de détection afin d’indiquer que l’exécution se déroule actuellement dans l’arrière-plan. Vous n’avez pas intérêt à effectuer de tâches longues dans l’événement **EnteredBackground**, dans la mesure où la transition vers l’arrière-plan pourrait apparaître lente pour l’utilisateur.

[!code-cs[EnteredBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetEnteredBackground)]

Quand votre application effectue sa transition vers l’arrière-plan, la limite de mémoire est réduite par le système, ceci pour garantir que l’application actuelle de premier plan dispose des ressources suffisantes pour procurer une expérience utilisateur réactive. Le gestionnaire d’événements [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging) indique à votre application que sa mémoire allouée a été réduite et fournit la nouvelle limite dans les arguments d’événement qui lui ont été transmis. Comparez la propriété [**MemoryManager.AppMemoryUsage**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsage), qui fait état de l’utilisation courante de votre application, à la propriété [**NewLimit**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLimitChangingEventArgs.NewLimit) des arguments d’événement, qui définit la nouvelle limite. Si votre utilisation de la mémoire dépasse la limite, vous devez tâcher de la réduire. Dans cet exemple, cette opération s’effectue dans la méthode d’assistance **ReduceMemoryUsage**, définie plus loin dans cet article.

[!code-cs[MemoryUsageLimitChanging](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetMemoryUsageLimitChanging)]

> [!NOTE] 
> Certaines configurations d’appareil permettent à une application de continuer à s’exécuter au-delà de la nouvelle limite de mémoire jusqu’à ce que le système subisse la pression des ressources, d’autres ne le permettent pas. Sur Xbox en particulier, les applications sont interrompues ou arrêtées si elles ne réduisent pas la mémoire en deçà de la nouvelle limite dans les 2 &gt; &gt; &gt; secondes. Cela signifie que vous pouvez fournir la meilleure expérience sur un large éventail d’appareils en vous appuyant sur cet événement pour réduire l’utilisation des ressources en deçà de la limite dans les 2secondes suivant le déclenchement de l’événement.


Lors du premier passage de votre application en arrière-plan, il est possible que son utilisation de la mémoire se situe en deçà de la limite appliquée aux applications en arrière-plan. Dans ce cas, son utilisation augmente ultérieurement vers la limite. Le gestionnaire [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) vous permet de vérifier votre utilisation courante lors de son augmentation et si nécessaire, de libérer de la mémoire. Vérifiez si [**AppMemoryUsageLevel**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLevel) présente l’état **High** ou **OverLimit**. Le cas échéant, réduisez votre utilisation de la mémoire. Là encore, dans cet exemple ce processus est traité par la méthode d’assistance, **ReduceMemoryUsage**. Vous pouvez également vous abonner à l’événement [**AppMemoryUsageDecreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageDecreased), vérifier si votre application se situe en deçà de la limite et le cas échéant, allouer des ressources supplémentaires en toute connaissance de cause.

[!code-cs[MemoryUsageIncreased](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetMemoryUsageIncreased)]

**ReduceMemoryUsage** est une méthode d’assistance que vous pouvez implémenter pour libérer de la mémoire quand votre application d’arrière-plan se situe au-delà de la limite d’utilisation. La manière dont vous libérez de la mémoire dépend des spécificités de votre application, mais nous vous recommandons entre autres de vous libérer de votre interface utilisateur et d’autres ressources associées à l’affichage de votre application. Tout d’abord, vérifiez que vous exécutez en mode d’arrière-plan, puis définissez la propriété [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) de la fenêtre de votre application sur Null. Appelez **GC.Collect** pour demander au système de récupérer immédiatement la mémoire libérée.

[!code-cs[UnloadViewContent](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetUnloadViewContent)]

Lorsque le contenu de la fenêtre est collecté, chaque élément Frame démarre son processus de déconnexion. Si l’arborescence des objets visuels sous le contenu de la fenêtre comporte des éléments Pages, ces derniers commenceront à déclencher leur événement [**Unloaded**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Unloaded). Les Pages ne peuvent pas être complètement effacées de la mémoire, sauf si l’ensemble des références associées sont supprimées. Dans le rappel **Unloaded** procédez comme suit afin de garantir la libération rapide de la mémoire:
* Effacez les grandes structures de données de votre Page, et définissez-les sur Null.
* Annulez l’enregistrement de l’ensemble des gestionnaires d’événements présentant des méthodes de rappel dans la Page. Veillez à inscrire ces rappels durant le gestionnaire d’événement Loaded associé à la Page. L’événement Loaded est déclenché quand l’interface utilisateur a été reconstituée et que la Page a été ajoutée à l’arborescence des objets visuels.
* Appelez **GC.Collect** à la fin du rappel Unloaded afin de procéder rapidement à un processus garbage collection des grandes structures de données que vous venez de définir sur Null.

[!code-cs[Unloaded](./code/BackgroundAudio_RS1/cs/MainPage.xaml.cs#SnippetUnloaded)]

Dans le gestionnaire d’événement [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground), vous devez définir la variable de détection afin d’indiquer que votre application ne s’exécute plus en arrière-plan. Ensuite, vérifiez si l’élément [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) de la fenêtre courante est défini sur Null, ce qui sera le cas si vous avez supprimé les vues de l’application afin de nettoyer la mémoire pendant l’exécution en arrière-plan. Si le contenu de la fenêtre est défini sur Null, régénérez la vue de votre application. Dans cet exemple, le contenu de la fenêtre est créé dans la méthode d’assistance **CreateRootFrame**.

[!code-cs[LeavingBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetLeavingBackground)]

La méthode d’assistance **CreateRootFrame** régénère le contenu d’affichage de votre application. Le code de cette méthode est identique au code du gestionnaire [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) fourni dans le modèle du projet par défaut. La seule différence? Le gestionnaire **Launching** détermine l’état d’exécution précédent de la propriété [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs.PreviousExecutionState) de la classe [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) et la méthode **CreateRootFrame** récupère simplement l’état précédent d’exécution transmis en tant qu’argument. Pour réduire le code dédupliqué, vous pouvez refactoriser le code du gestionnaire d’événement **Launching** par défaut afin d’appeler **CreateRootFrame**, si vous le souhaitez.

[!code-cs[CreateRootFrame](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetCreateRootFrame)]

## Disponibilité du réseau pour les applications multimédias en arrière-plan
L’ensemble des sources multimédias reconnaissant le réseau, celles qui ne sont pas créées à partir d’un flux ou d’un fichier, maintiennent l’activité de la connexion réseau pendant la récupération du contenu à distance et abandonnent l’activité dans le cas contraire. [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaStreamSource), particulièrement, s’appuie sur l’application pour signaler correctement à l’application la plage mise en tampon à l’aide de [**SetBufferedRange**](https://msdn.microsoft.com/library/windows/apps/dn282762). Une fois que l’intégralité du contenu est mis en tampon, le réseau n’est plus réservé pour le compte de l’application.

SI vous avez besoin d’effectuer des appels réseau intervenant en arrière-plan quand aucun contenu multimédia n’est en téléchargement, ces opérations doivent être encapsulées dans une tâche appropriée comme [**ApplicationTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.ApplicationTrigger), [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.MaintenanceTrigger) ou [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.TimeTrigger). Pour plus d’informations, voir [Prendre en charge votre application avec des tâches en arrière-plan](https://msdn.microsoft.com/en-us/windows/uwp/launch-resume/support-your-app-with-background-tasks).

## Rubriques connexes
* [Lecture de contenu multimédia](media-playback.md)
* [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Intégrer avec les contrôles de transport de média système](integrate-with-systemmediatransportcontrols.md)
* [Exemple Contenu audio en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)

 

 







<!--HONumber=Aug16_HO3-->


