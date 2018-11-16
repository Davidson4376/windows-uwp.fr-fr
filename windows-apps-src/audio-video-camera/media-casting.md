---
author: drewbatgit
ms.assetid: 40B97E0C-EB1B-40C2-A022-1AB95DFB085E
description: Cet article vous montre comment procéder à une diffusion multimédia sur des appareils distants à partir d’une application Windows universelle.
title: Diffusion multimédia
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: da0bb4d25166dd62372d5902ff89221d20189c22
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6860754"
---
# <a name="media-casting"></a>Diffusion multimédia



Cet article vous montre comment procéder à une diffusion multimédia sur des appareils distants à partir d’une application Windows universelle.

## <a name="built-in-media-casting-with-mediaplayerelement"></a>Diffusion multimédia intégrée avec MediaPlayerElement

Le moyen le plus simple d’effectuer une diffusion multimédia à partir d’une application Windows universelle consiste à utiliser la fonctionnalité intégrée de diffusion du contrôle [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement).

Pour permettre à l’utilisateur d’ouvrir un fichier vidéo à lire dans le contrôle **MediaPlayerElement**, ajoutez les espaces de noms suivants à votre projet.

[!code-cs[BuiltInCastingUsing](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetBuiltInCastingUsing)]

Dans le fichier XAML de votre application, ajoutez un élément **MediaPlayerElement** et définissez [**AreTransportControlsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn298977) sur true.

[!code-xml[MediaElement](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetMediaElement)]

Ajoutez un bouton pour permettre à l’utilisateur de lancer la sélection d’un fichier.

[!code-xml[OpenButton](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetOpenButton)]

Dans le gestionnaire d’événements [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) du bouton, créez une instance de la classe [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847), ajoutez des types de fichiers vidéo à la collection [**FileTypeFilter**](https://msdn.microsoft.com/library/windows/apps/br207850) et définissez l’emplacement de démarrage sur la vidéothèque de l’utilisateur.

Appelez [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) pour ouvrir la boîte de dialogue du sélecteur de fichiers. La méthode renvoie un objet [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) représentant le fichier vidéo. Vérifiez que la valeur de ce fichier n’est pas null, ce qui est le cas si l’utilisateur annule l’opération de sélection. Appelez la méthode [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227221.aspx) du fichier pour obtenir un élément [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731) pour le fichier. Enfin, créez un objet **MediaSource** à partir du fichier sélectionné en appelant [**CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909) et attribuez-le à la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.Source) de l’objet **MediaPlayerElement** pour définir le fichier source du contrôle.

[!code-cs[OpenButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetOpenButtonClick)]

Une fois la vidéo chargée dans l’élément **MediaPlayerElement**, il suffit à l’utilisateur d’appuyer sur le bouton de diffusion sur les contrôles de transport pour lancer une boîte de dialogue intégrée qui lui permet de choisir un appareil sur lequel diffuser le média chargé.

![Bouton de diffusion MediaElement](images/media-element-casting-button.png)

> [!NOTE] 
> À compter de Windows10, version1607, nous vous recommandons d’utiliser la classe **MediaPlayer** pour lire des éléments multimédias. **MediaPlayerElement** est un contrôle XAML léger utilisé pour afficher le contenu d’un **MediaPlayer** dans une page XAML. Le contrôle **MediaElement** continue d’être pris en charge pour la compatibilité descendante. Pour plus d’informations sur l’utilisation de **MediaPlayer** et **MediaPlayerElement** pour lire du contenu multimédia, voir [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md). Pour plus d’informations sur l’utilisation de **MediaSource** et des API associées pour utiliser du contenu multimédia, voir [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md).

## <a name="media-casting-with-the-castingdevicepicker"></a>Diffusion multimédia à l’aide de la classe CastingDevicePicker

L’autre méthode pour procéder à une diffusion multimédia sur un appareil consiste à utiliser la classe [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/dn972525). Afin de l’utiliser, vous devez inclure l’espace de noms [**Windows.Media.Casting**](https://msdn.microsoft.com/library/windows/apps/dn972568) dans votre projet.

[!code-cs[CastingNamespace](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastingNamespace)]

Déclarez une variable de membre pour l’objet **CastingDevicePicker**.

[!code-cs[DeclareCastingPicker](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDeclareCastingPicker)]

Une fois la page initialisée, créez une instance du sélecteur de diffusion et définissez [**Filter**](https://msdn.microsoft.com/library/windows/apps/dn972540) sur la propriété [**SupportsVideo**](https://msdn.microsoft.com/library/windows/apps/dn972526) pour indiquer que les appareils de diffusion répertoriés par le sélecteur doivent prendre en charge la vidéo. Enregistrez un gestionnaire pour l’événement [**CastingDeviceSelected**](https://msdn.microsoft.com/library/windows/apps/dn972539), qui est déclenché lorsque l’utilisateur sélectionne un appareil pour la diffusion.

[!code-cs[InitCastingPicker](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetInitCastingPicker)]

Dans votre fichier XAML, ajoutez un bouton pour permettre à l’utilisateur de lancer le sélecteur.

[!code-xml[CastPickerButton](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetCastPickerButton)]

Dans le gestionnaire d’événements **Click** du bouton, appelez [**TransformToVisual**](https://msdn.microsoft.com/library/windows/apps/br208986) pour obtenir la transformation d’un élément d’interface utilisateur par rapport à un autre élément. Dans cet exemple, la transformation est la position du bouton du sélecteur de diffusion par rapport à la racine visuelle de la fenêtre d’application. Appelez la méthode [**Show**](https://msdn.microsoft.com/library/windows/apps/dn972542) de l’objet [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/dn972525) pour lancer la boîte de dialogue du sélecteur de diffusion. Indiquez l’emplacement et les dimensions du bouton du sélecteur de diffusion afin que le système puisse afficher la boîte de dialogue à partir du bouton sur lequel l’utilisateur a appuyé.

[!code-cs[CastPickerButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastPickerButtonClick)]

Dans le gestionnaire d’événements **CastingDeviceSelected**, appelez la méthode [**CreateCastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972547) de la propriété [**SelectedCastingDevice**](https://msdn.microsoft.com/library/windows/apps/dn972546) des arguments d’événement, qui représente l’appareil de diffusion sélectionné par l’utilisateur. Enregistrez des gestionnaires pour les événements [**ErrorOccurred**](https://msdn.microsoft.com/library/windows/apps/dn972519) et [**StateChanged**](https://msdn.microsoft.com/library/windows/apps/dn972523). Enfin, appelez [**RequestStartCastingAsync**](https://msdn.microsoft.com/library/windows/apps/dn972520) pour lancer la diffusion en transmettant le résultat à la méthode [**GetAsCastingSource**](https://msdn.microsoft.com/library/windows/apps/dn920012) de l’objet **MediaPlayer** du contrôle **MediaPlayerElement** pour indiquer que le contenu multimédia à diffuser correspond à celui du **MediaPlayer** associé à **MediaPlayerElement**.

> [!NOTE] 
> La connexion de la diffusion doit être lancée sur le thread d’interface utilisateur. Étant donné que **CastingDeviceSelected** n’est pas appelé sur le thread d’interface utilisateur, vous devez placer ces appels à l’intérieur d’un appel à [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) pour changer cela.

[!code-cs[CastingDeviceSelected](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastingDeviceSelected)]

Dans les gestionnaires d’événements **ErrorOccurred** et **StateChanged**, vous devez mettre à jour votre interface utilisateur pour informer l’utilisateur de l’état de diffusion actuel. Ces événements sont décrits en détail dans la section suivante portant sur la création d’un sélecteur d’appareil de diffusion personnalisé.

[!code-cs[EmptyStateHandlers](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetEmptyStateHandlers)]

## <a name="media-casting-with-a-custom-device-picker"></a>Diffusion multimédia à l’aide d’un sélecteur d’appareil personnalisé

La section suivante décrit comment créer votre propre interface utilisateur de sélecteur d’appareil de diffusion par l’énumération des appareils de diffusion et le lancement de la connexion à partir de votre code.

Pour énumérer les appareils de diffusion disponibles, incluez l’espace de noms [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459) dans votre projet.

[!code-cs[EnumerationNamespace](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

Ajoutez les contrôles suivants à votre page XAML pour implémenter l’interface utilisateur rudimentaire pour cet exemple:

-   Un bouton pour démarrer l’observateur d’appareils qui recherche les appareils de diffusion disponibles.
-   Un contrôle [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) pour signaler à l’utilisateur que l’énumération de la diffusion est en cours.
-   Une classe [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868) pour répertorier les appareils de diffusion détectés. Définissez une propriété [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/br242830) pour le contrôle afin que nous puissions affecter les objets de l’appareil de diffusion directement au contrôle et toujours afficher la propriété [**FriendlyName**](https://msdn.microsoft.com/library/windows/apps/dn972549).
-   Un bouton permettant à l’utilisateur de déconnecter l’appareil de diffusion.

[!code-xml[CustomPickerXAML](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetCustomPickerXAML)]

Dans le code sous-jacent, déclarez des variables de membre pour [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446) et [**CastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972510).

[!code-cs[DeclareDeviceWatcher](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatcher)]

Dans le gestionnaire **Click** de l’événement *startWatcherButton*, commencez par mettre à jour l’interface utilisateur en désactivant le bouton et en activant l’anneau de progression pendant l’énumération des appareils. Effacez la zone de liste des appareils de diffusion.

Ensuite, créez un observateur d’appareils en appelant [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/br225427). Cette méthode peut être utilisée pour observer de nombreux types d’appareils différents. Indiquez que vous souhaitez observer les appareils prenant en charge la diffusion vidéo en utilisant la chaîne de sélecteur d’appareil renvoyée par [**CastingDevice.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn972551).

Enfin, enregistrez des gestionnaires d’événements pour les événements [**Added**](https://msdn.microsoft.com/library/windows/apps/br225450), [**Removed**](https://msdn.microsoft.com/library/windows/apps/br225453), [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451) et [**Stopped**](https://msdn.microsoft.com/library/windows/apps/br225457).

[!code-cs[StartWatcherButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetStartWatcherButtonClick)]

L’événement **Added** est déclenché lorsqu’un nouvel appareil est détecté par l’observateur. Dans le gestionnaire de cet événement, créez un objet [**CastingDevice**](https://msdn.microsoft.com/library/windows/apps/dn972524) en appelant [**CastingDevice.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn972550) et en transmettant l’ID de l’appareil de diffusion détecté, qui est contenu dans l’objet **DeviceInformation** transmis au gestionnaire.

Ajoutez le **CastingDevice** à l’appareil de diffusion **ListBox** afin que l’utilisateur puisse le sélectionner. En raison de la propriété [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/br242830) définie dans le code XAML, la propriété [**FriendlyName**](https://msdn.microsoft.com/library/windows/apps/dn972549) est utilisée comme texte d’élément dans la zone de liste. Dans la mesure où ce gestionnaire d’événements n’est pas appelé sur le thread d’interface utilisateur, vous devez mettre à jour l’interface utilisateur à partir d’un appel à [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317).

[!code-cs[WatcherAdded](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherAdded)]

L’événement **Removed** est déclenché lorsque l’observateur détecte qu’un appareil de diffusion n’est plus présent. Comparez la propriété de l’ID de l’objet **Added** transmis par le biais du gestionnaire à l’ID de chaque **Added** de la collection [**Items**](https://msdn.microsoft.com/library/windows/apps/br242823) de la zone de liste. Si l’ID correspond, supprimez cet objet de la collection. Là encore, dans la mesure où l’interface utilisateur est mise à jour, cet appel doit être effectué depuis un appel **RunAsync**.

[!code-cs[WatcherRemoved](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherRemoved)]

L’événement **EnumerationCompleted** est déclenché lorsque l’observateur a terminé la détection des appareils. Dans le gestionnaire de cet événement, mettez à jour l’interface utilisateur pour informer l’utilisateur que l’énumération des appareils est terminée et arrêter l’observateur en appelant [**Stop**](https://msdn.microsoft.com/library/windows/apps/br225456).

[!code-cs[WatcherEnumerationCompleted](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherEnumerationCompleted)]

L’événement Stopped est déclenché lorsque l’observateur d’appareils a terminé l’arrêt. Dans le gestionnaire de cet événement, arrêtez le contrôle [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) et réactivez le paramètre *startWatcherButton* afin que l’utilisateur puisse redémarrer le processus d’énumération des appareils.

[!code-cs[WatcherStopped](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherStopped)]

L’événement [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) est déclenché, dès que l’utilisateur sélectionne un des appareils de diffusion à partir de la zone de liste. La création de la connexion de la diffusion et le démarrage de la diffusion s’effectuent dans ce gestionnaire.

Tout d’abord, assurez-vous que l’observateur d’appareils est arrêté afin que l’énumération des appareils n’interfère pas avec la diffusion multimédia. Créez une connexion de diffusion en appelant [**CreateCastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972547) sur l’objet **CastingDevice** sélectionné par l’utilisateur. Ajoutez des gestionnaires d’événements pour les événements [**StateChanged**](https://msdn.microsoft.com/library/windows/apps/dn972523) et [**ErrorOccurred**](https://msdn.microsoft.com/library/windows/apps/dn972519).

Lancez la diffusion multimédia en appelant [**RequestStartCastingAsync**](https://msdn.microsoft.com/library/windows/apps/dn972520) et en transmettant la source de diffusion renvoyée en appelant la méthode [**GetAsCastingSource**](https://msdn.microsoft.com/library/windows/apps/dn920012) de **MediaPlayer**. Enfin, faites en sorte que le bouton de déconnexion soit visible pour permettre à l’utilisateur d’arrêter la diffusion multimédia.

[!code-cs[SelectionChanged](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetSelectionChanged)]

Dans le gestionnaire de changement d’état, l’action effectuée dépend du nouvel état de la connexion de diffusion:

-   Si l’état est **Connected** ou **Rendering**, assurez-vous que le contrôle **ProgressRing** est inactif et que le bouton de déconnexion est visible.
-   Si l’état est **Disconnected**, désélectionnez l’appareil de diffusion actuel dans la zone de liste, assurez-vous que le contrôle **ProgressRing** est inactif et masquez le bouton de déconnexion.
-   Si l’état est **Connecting**, activez le contrôle **ProgressRing** et masquez le bouton de déconnexion.
-   Si l’état est **Disconnecting**, activez le contrôle **ProgressRing** et masquez le bouton de déconnexion.

[!code-cs[StateChanged](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetStateChanged)]

Dans le gestionnaire de l’événement **ErrorOccurred**, mettez à jour votre interface utilisateur pour informer l’utilisateur qu’une erreur de diffusion s’est produite et désélectionnez l’objet **CastingDevice** dans la zone de liste.

[!code-cs[ErrorOccurred](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetErrorOccurred)]

Enfin, implémentez le gestionnaire pour le bouton de déconnexion. Arrêtez la diffusion multimédia et déconnectez l’appareil de diffusion en appelant la méthode [**DisconnectAsync**](https://msdn.microsoft.com/library/windows/apps/dn972518) de l’objet **CastingConnection**. Cet appel doit être transmis au thread d’interface utilisateur en appelant [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317).

[!code-cs[DisconnectButton](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDisconnectButton)]

 

 




