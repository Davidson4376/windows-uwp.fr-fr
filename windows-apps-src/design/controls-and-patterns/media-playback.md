---
author: Jwmsft
Description: The media player is used to view and listen to video, audio, and images.
title: Lecteur multimédia
ms.assetid: 9AABB5DE-1D81-4791-AB47-7F058F64C491
dev.assetid: AF2F2008-9B53-430C-BBC3-8888F631B0B0
label: Media player
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 113acbe1f4e9bb3814b2f9b61beb79488995d3e3
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5969848"
---
# <a name="media-player"></a>Lecteur multimédia



Le lecteur multimédia permet d’afficher et d’écouter des fichiers vidéo et audio. La lecture de fichiers multimédias peut se faire «inline» (insérée dans une page ou dans un groupe d’autres contrôles) ou dans un affichage plein écran dédié. Vous pouvez modifier le jeu de boutons du lecteur, modifier l’arrière-plan de la barre de contrôle et organiser les dispositions selon vos besoins. N’oubliez pas que les utilisateurs s’attendent à un ensemble de contrôles de base (lecture/pause, retour rapide, avance rapide).

![Élément multimédia avec contrôles de transport](images/controls/mtc_double_video_inprod.png)

> **API importantes**: [classe MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx), [classe MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediatransportcontrols)


> [!NOTE]
> **MediaPlayerElement** est uniquement disponible dans Windows10, version1607 ou ultérieure. Si vous développez une application pour une version antérieure de Windows10, vous devez utiliser [MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926) à la place. Toutes les recommandations de cette page s’appliquent à MediaElement également.

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Utilisez un lecteur multimédia lorsque vous voulez lire des fichiers audio ou vidéo dans votre application. Pour afficher des collections d’images, utilisez une [vue symétrique](flipview.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour ouvrir l’application et voir l'objet <a href="xamlcontrolsgallery:/item/MediaPlayerElement">MediaPlayerElement</a> ou <a href="xamlcontrolsgallery:/item/MediaPlayer">MediaPlayer</a> en action.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Un lecteur multimédia dans l’application Prise en main de Windows10.

![Un élément multimédia dans l’application Prise en main de Windows10.](images/control-examples/mtc_getstarted_example.png)

## <a name="create-a-media-player"></a>Créer un lecteur multimédia
Ajoutez du contenu multimédia à votre application en créant un objet [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) en XAML et définissez la [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) sur un [MediaSource](https://msdn.microsoft.com/library/windows/apps/windows.media.core.mediasource.aspx) qui pointe sur un fichier audio ou vidéo.

Ce code XAML crée un [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) et définit sa propriété [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) sur l’URI d’un fichier vidéo stocké localement dans l’application. **MediaPlayerElement** commence la lecture quand la page se charge. Pour empêcher le démarrage immédiat du contenu multimédia, vous pouvez définir la propriété [AutoPlay](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.autoplay.aspx) sur **false**.

```xaml
<MediaPlayerElement x:Name="mediaSimple"
                    Source="Videos/video1.mp4"
                    Width="400" AutoPlay="True"/>
```

Ce code XAML crée un [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) dans lequel les contrôles de transport intégrés sont activés et la propriété [Lecture automatique](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.autoplay.aspx) est définie sur **false.**


```xaml
<MediaPlayerElement x:Name="mediaPlayer"
                    Source="Videos/video1.mp4"
                    Width="400"
                    AutoPlay="False"
                    AreTransportControlsEnabled="True"/>
```

### <a name="media-transport-controls"></a>Contrôles de transport multimédia
[MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) intègre des contrôles de transport qui gèrent la lecture, l’arrêt, la mise en pause, la désactivation du micro, la recherche/progression, les sous-titres codés et la sélection de pistes audio. Pour activer ces contrôles, définissez [AreTransportControlsEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.AreTransportControlsEnabled.aspx) sur **true**. Pour les désactiver, attribuez à **AreTransportControlsEnabled** la valeur **false**. Les contrôles de transport sont représentés par la classe [MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn831962). Vous pouvez utiliser les contrôles de transport en l’état ou les personnaliser de différentes manières. Pour plus d’informations, voir les informations de référence de la classe [MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn831962) et la rubrique [Créer des contrôles de transport personnalisés](custom-transport-controls.md).

Les contrôles de transport prennent en charge les dispositions sur une seule et deux lignes. Le premier exemple ci-dessous correspond à une disposition sur une ligne, avec le bouton lecture/pause situé à gauche de la chronologie des médias. Cette disposition est réservée à la lecture multimédia insérée et aux écrans compacts.

![Exemple de contrôles MTC répartis sur une ligne](images/controls/mtc_single_inprod_02.png)

La disposition des contrôles sur deux lignes (voir ci-dessous) est recommandée pour la plupart des scénarios d’utilisation, en particulier sur les écrans plus grands. Cette disposition offre davantage d’espace pour les contrôles et simplifie l’utilisation de la chronologie.

![Exemple de contrôles MTC sur téléphone répartis sur deux lignes](images/controls/mtc_double_inprod.png)

**Contrôles de transport de média système**

[MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) est automatiquement intégré aux contrôles de transport de média système. Les contrôles de transport de média système sont les contrôles qui s’affichent quand l’utilisateur appuie sur une touche de média matériel, comme les boutons de média d’un clavier. Pour plus d’informations, voir [SystemMediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn278677).

> **Remarque**&nbsp;&nbsp; [MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926) n’est pas automatiquement intégré au système de contrôles de transport de média, vous devez le connecter vous-même. Pour plus d’informations, voir [Contrôles de transport de média système](https://msdn.microsoft.com/library/windows/apps/mt228338).


### <a name="set-the-media-source"></a>Définir la source du média
Pour lire des fichiers sur le réseau ou des fichiers incorporés dans l’application, définissez la propriété [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) sur un [MediaSource](https://msdn.microsoft.com/library/windows/apps/windows.media.core.mediasource.aspx) avec le chemin du fichier.

**Conseil**pour ouvrir des fichiers à partir d’internet, vous devez déclarer la fonctionnalité **Internet (Client)** dans le manifeste de votre application (Package.appxmanifest). Pour plus d’informations sur la déclaration des fonctionnalités, voir [Déclarations des fonctionnalités d’application](https://msdn.microsoft.com/library/windows/apps/mt270968).

 

Ce code tente de définir la propriété [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) de [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) défini en XAML sur le chemin d’un fichier entré dans un [TextBox](https://msdn.microsoft.com/library/windows/apps/br209683).

```xaml
<TextBox x:Name="txtFilePath" Width="400"
         FontSize="20"
         KeyUp="TxtFilePath_KeyUp"
         Header="File path"
         PlaceholderText="Enter file path"/>
```

```csharp
private void TxtFilePath_KeyUp(object sender, KeyRoutedEventArgs e)
{
    if (e.Key == Windows.System.VirtualKey.Enter)
    {
        TextBox tbPath = sender as TextBox;

        if (tbPath != null)
        {
            LoadMediaFromString(tbPath.Text);
        }
    }
}

private void LoadMediaFromString(string path)
{
    try
    {
        Uri pathUri = new Uri(path);
        mediaPlayer.Source = MediaSource.CreateFromUri(pathUri);
    }
    catch (Exception ex)
    {
        if (ex is FormatException)
        {
            // handle exception.
            // For example: Log error or notify user problem with file
        }
    }
}
```

Pour définir une source de média sur un fichier multimédia incorporé dans l’application, initialisez un [Uri](https://msdn.microsoft.com/library/windows/apps/br226017) avec le chemin précédé du préfixe **ms-appx:///**, créez un [MediaSource](https://msdn.microsoft.com/library/windows/apps/windows.media.core.mediasource.aspx) avec l’URI, puis définissez la propriété [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) sur l’URI. Par exemple, pour un fichier nommé **video1.mp4** situé dans un sous-dossier **Videos**, le chemin est de type: **ms-appx:///Videos/video1.mp4**

Ce code définit la propriété [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) du [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) défini précédemment en XAML sur **ms-appx:///Videos/video1.mp4**.

```csharp
private void LoadEmbeddedAppFile()
{
    try
    {
        Uri pathUri = new Uri("ms-appx:///Videos/video1.mp4");
        mediaPlayer.Source = MediaSource.CreateFromUri(pathUri);
    }
    catch (Exception ex)
    {
        if (ex is FormatException)
        {
            // handle exception.
            // For example: Log error or notify user problem with file
        }
    }
}
```

### <a name="open-local-media-files"></a>Ouvrir des fichiers multimédias locaux
Pour ouvrir des fichiers sur le système local ou à partir de OneDrive, vous pouvez utiliser [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) pour obtenir le fichier et [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) pour définir la source du média, ou vous pouvez accéder par programmation aux dossiers de médias de l’utilisateur.

Si votre application a besoin d’accéder aux dossiers **Musique** ou **Vidéo** sans l’interaction de l’utilisateur, par exemple, si vous énumérez tous les fichiers de musique ou vidéo de la collection de l’utilisateur pour ensuite les afficher dans votre application, vous devez déclarer les fonctionnalités **Music Library** et **Video Library**. Pour plus d’informations, voir [Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos](https://msdn.microsoft.com/library/windows/apps/mt188703).

Le contrôle [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) ne nécessite pas de fonctionnalités spéciales pour accéder aux fichiers résidant sur le système de fichiers local, comme les dossiers **Musique** ou **Vidéo** de l’utilisateur, car ce dernier bénéficie d’un contrôle total sur l’accès aux fichiers. Du point de vue de la sécurité et de la confidentialité, il est préférable de limiter le nombre de fonctionnalités utilisées par votre application.

**Pour ouvrir un média local à l’aide de FileOpenPicker**

1.  Appelez [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) pour permettre à l’utilisateur de choisir un fichier multimédia.

    Utilisez la classe [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) pour sélectionner un fichier multimédia. Définissez [FileTypeFilter](https://msdn.microsoft.com/library/windows/apps/br207850) pour spécifier les types de fichiers affichés par **FileOpenPicker**. Appelez [PickSingleFileAsync](https://msdn.microsoft.com/library/windows/apps/jj635275) pour lancer le sélecteur de fichiers et obtenir le fichier.

2.  Utilisez un [MediaSource](https://msdn.microsoft.com/library/windows/apps/windows.media.core.mediasource.aspx) pour définir le fichier multimédia choisi en tant que [MediaPlayerElement.Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx).

    Pour utiliser le [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171) renvoyé par le [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847), vous devez appeler la méthode [CreateFromStorageFile](https://msdn.microsoft.com/library/windows/apps/windows.media.core.mediasource.createfromstoragefile.aspx) sur [MediaSource](https://msdn.microsoft.com/library/windows/apps/windows.media.core.mediasource.aspx) et la définir en tant que [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) de [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx). Ensuite, appelez [Play](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.play.aspx) sur [MediaPlayerElement.MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) pour démarrer le média.


Cet exemple montre comment utiliser le [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) pour choisir un fichier et définir le fichier en tant que [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) d’un [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx).

```xaml
<MediaPlayerElement x:Name="mediaPlayer"/>
...
<Button Content="Choose file" Click="Button_Click"/>
```

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    await SetLocalMedia();
}

async private System.Threading.Tasks.Task SetLocalMedia()
{
    var openPicker = new Windows.Storage.Pickers.FileOpenPicker();

    openPicker.FileTypeFilter.Add(".wmv");
    openPicker.FileTypeFilter.Add(".mp4");
    openPicker.FileTypeFilter.Add(".wma");
    openPicker.FileTypeFilter.Add(".mp3");

    var file = await openPicker.PickSingleFileAsync();

    // mediaPlayer is a MediaPlayerElement defined in XAML
    if (file != null)
    {
        mediaPlayer.Source = MediaSource.CreateFromStorageFile(file);

        mediaPlayer.MediaPlayer.Play();
    }
}
```

### <a name="set-the-poster-source"></a>Définir la source de l’affiche
La propriété [PosterSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.PosterSource.aspx) vous permet de fournir à votre [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) une représentation visuelle avant le chargement du média. Un **PosterSource** est une image (par exemple, une capture d’écran ou une affiche de film) qui est affichée à la place du média. Le **PosterSource** est affiché dans les situations suivantes:

-   Quand aucune source valide n’est définie. Par exemple, si la propriété [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) n’est pas définie, si **Source** est définie sur **Null** ou si la source n’est pas valide (comme lorsqu’un événement [MediaFailed](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.mediafailed.aspx) se produit).
-   Lors du chargement du média. Par exemple, une source valide est définie mais l’événement [MediaOpened](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.mediaopened.aspx) ne s’est toujours pas produit.
-   Quand le média est diffusé vers un autre appareil.
-   Quand le média est un fichier audio uniquement.

Voici un [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) dont la [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) est définie sur une piste d’album et [PosterSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.PosterSource.aspx) est défini sur une image de la couverture d’album.

```xaml
<MediaPlayerElement Source="Media/Track1.mp4" PosterSource="Media/AlbumCover.png"/>
```

### <a name="keep-the-devices-screen-active"></a>Maintenir l’écran de l’appareil actif
Normalement, un appareil estompe l’affichage (et finit par le désactiver) pour préserver l’autonomie de la batterie quand l’utilisateur s’absente, mais les applications vidéo doivent maintenir l’écran allumé pour que l’utilisateur puisse voir la vidéo. Pour empêcher la désactivation de l’affichage quand aucune action de l’utilisateur n’est détectée (par exemple, quand une application lit une vidéo), vous pouvez appeler [DisplayRequest.RequestActive](https://msdn.microsoft.com/library/windows/apps/br241818). La classe [DisplayRequest](https://msdn.microsoft.com/library/windows/apps/br241816) vous permet d’indiquer à Windows de laisser activé l’affichage pour permettre à l’utilisateur de voir la vidéo.

Pour économiser l’énergie et prolonger l’autonomie de la batterie, vous devez appeler [DisplayRequest.RequestRelease](https://msdn.microsoft.com/library/windows/apps/br241819) pour libérer la demande d’affichage lorsqu’elle n’est plus nécessaire. Windows désactive automatiquement les demandes d’affichage actives de votre application lorsque cette dernière n’est plus à l’écran, et les réactive quand votre application revient au premier plan.

Voici quelques situations où vous devez libérer la demande d’affichage :

-   La lecture vidéo est suspendue, par exemple suite à une action de l’utilisateur, une mise en mémoire tampon ou un réglage en raison de la bande passante limitée.
-   La lecture s’arrête. Par exemple, la lecture de la vidéo ou la présentation est terminée.
-   Une erreur de lecture s’est produite. Il peut s’agir de problèmes de connectivité réseau ou d’un fichier endommagé.

> **Remarque**&nbsp;&nbsp; Si [MediaPlayerElement.IsFullWindow](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.IsFullWindow.aspx) a la valeur true et que le contenu multimédia est en cours de lecture, la désactivation de l’affichage est automatiquement empêchée.

**Pour maintenir l’écran actif**

1.  Créez une variable [DisplayRequest](https://msdn.microsoft.com/library/windows/apps/br241816) globale. Initialisez-la sur la valeur Null.
```csharp
// Create this variable at a global scope. Set it to null.
private DisplayRequest appDisplayRequest = null;
```

2.  Appelez [RequestActive](https://msdn.microsoft.com/library/windows/apps/br241818) pour signaler à Windows que l’application requiert que l’affichage reste activé.

3.  Appelez [RequestRelease](https://msdn.microsoft.com/library/windows/apps/br241819) pour supprimer la demande d’affichage chaque fois que la lecture vidéo est arrêtée, suspendue ou interrompue par une erreur de lecture. Quand votre application n’a plus de demande d’affichage active, Windows préserve l’autonomie de la batterie en estompant l’affichage (et finit par le désactiver) quand l’appareil n’est pas utilisé.

    Chaque [MediaPlayerElement.MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) a un [PlaybackSession](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.playbacksession.aspx) de type [MediaPlaybackSession](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.aspx) qui contrôle divers aspects de la lecture multimédia comme [PlaybackRate](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackrate.aspx), [PlaybackState](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackstate.aspx) et [Position](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.position.aspx). Ici, vous utilisez l’événement [PlaybackStateChanged](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackstatechanged.aspx) sur [MediaPlayer.PlaybackSession](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.playbacksession.aspx) pour détecter les situations où vous devez libérer la demande d’affichage. Utilisez ensuite la propriété [NaturalVideoHeight](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.naturalvideoheight.aspx) pour déterminer si un fichier audio ou vidéo est en cours de lecture, et maintenez l’écran actif uniquement si une vidéo est en cours de lecture.
    ```xaml
<MediaPlayerElement x:Name="mpe" Source="Media/video1.mp4"/>
    ```

    ```csharp
    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        mpe.MediaPlayer.PlaybackSession.PlaybackStateChanged += MediaPlayerElement_CurrentStateChanged;
        base.OnNavigatedTo(e);
    }

    private void MediaPlayerElement_CurrentStateChanged(object sender, RoutedEventArgs e)
    {
        MediaPlaybackSession playbackSession = sender as MediaPlaybackSession;
        if (playbackSession != null && playbackSession.NaturalVideoHeight != 0)
        {
            if(playbackSession.PlaybackState == MediaPlaybackState.Playing)
            {
                if(appDisplayRequest == null)
                {
                    // This call creates an instance of the DisplayRequest object
                    appDisplayRequest = new DisplayRequest();
                    appDisplayRequest.RequestActive();
                }
            }
            else // PlaybackState is Buffering, None, Opening or Paused
            {
                if(appDisplayRequest != null)
                {
                      // Deactivate the displayr request and set the var to null
                      appDisplayRequest.RequestRelease();
                      appDisplayRequest = null;
                }
            }
        }

    }
    ```

### <a name="control-the-media-player-programmatically"></a>Contrôler le lecteur multimédia par programmation
[MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) propose diverses propriétés, méthodes et événements destinés à contrôler la lecture audio et vidéo par le biais de la propriété [MediaPlayerElement.MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx). Pour obtenir la liste complète des propriétés, méthodes et événements, voir la page de référence de [MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.aspx).

### <a name="advanced-media-playback-scenarios"></a>Scénarios de lecture multimédia avancés
Pour les scénarios de lecture multimédia plus complexes, comme la lecture d’une playlist, le basculement entre des langues audio ou la création de pistes de métadonnées personnalisées, définissez le [MediaPlayerElement.Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) sur un [MediaPlaybackItem](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybackitem.aspx) ou un [MediaPlaybackList](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacklist.aspx). Voir la page [Lecture multimédia](https://msdn.microsoft.com/windows/uwp/audio-video-camera/media-playback-with-mediasource) dans le centre de développement pour plus d’informations sur l’activation de plusieurs fonctionnalités multimédias avancées.

### <a name="enable-full-window-video-rendering"></a>Activer le rendu vidéo dans une fenêtre entière

Définissez la propriété [IsFullWindow](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.isfullwindow.aspx) pour activer et désactiver le rendu dans une fenêtre entière. Lorsque vous définissez le rendu dans une fenêtre entière par programme dans votre application, vous devez toujours utiliser la propriété **IsFullWindow** au lieu de le faire manuellement. Avec la propriété **IsFullWindow**, vous êtes assuré que les optimisations au niveau du système sont exécutées, ce qui améliore les performances et l’autonomie de la batterie. Si l’affichage du rendu dans une fenêtre entière n’est pas configuré correctement, il se peut que ces optimisations ne soient pas activées.

Le code ci-dessous crée un objet [AppBarButton](https://msdn.microsoft.com/library/windows/apps/dn279244) qui active/désactive le rendu dans une fenêtre entière.

```xaml
<AppBarButton Icon="FullScreen"
              Label="Full Window"
              Click="FullWindow_Click"/>>
```

```csharp
private void FullWindow_Click(object sender, object e)
{
    mediaPlayer.IsFullWindow = !media.IsFullWindow;
}
```

### <a name="resize-and-stretch-video"></a>Redimensionner et étirer une vidéo

Utilisez la propriété [Stretch](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.stretch.aspx) pour modifier la façon dont le contenu vidéo et/ou le [PosterSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.postersource.aspx) remplit son conteneur. Le redimensionnement et l’étirement de la vidéo dépendent de la valeur de [Stretch](https://msdn.microsoft.com/library/windows/apps/br242968). Les états de **Stretch** sont comparables aux réglages du format de l’image de nombreux téléviseurs. Vous pouvez l’accrocher à un bouton et permettre ainsi à l’utilisateur d’effectuer les réglages de son choix.

-   [None](https://msdn.microsoft.com/library/windows/apps/br242968) affiche la résolution native du contenu dans sa taille d’origine.
-   [Uniform](https://msdn.microsoft.com/library/windows/apps/br242968) remplit autant d’espace que possible tout en préservant les proportions et le contenu de l’image. Des bandes noires horizontales ou verticales peuvent dès lors apparaître sur les bordures de la vidéo. L’effet obtenu est comparable aux modes grand écran.
-   [UniformToFill](https://msdn.microsoft.com/library/windows/apps/br242968) remplit l’espace entier tout en préservant les proportions. Dans ce cas, une partie de l’image peut être rognée. L’effet obtenu est comparable aux modes plein écran.
-   [Fill](https://msdn.microsoft.com/library/windows/apps/br242968) remplit l’espace entier, mais ne conserve pas les proportions. Si l’image n’est pas rognée, un étirement peut en revanche être observé. L’effet obtenu est comparable aux modes Étirer.

![Valeurs de l’énumération Stretch](images/Image_Stretch.jpg)

Ici, un [AppBarButton](https://msdn.microsoft.com/library/windows/apps/dn279244) est utilisé pour faire défiler les options [Stretch](https://msdn.microsoft.com/library/windows/apps/br242968). Une instruction **switch** vérifie l’état actif de la propriété [Stretch](https://msdn.microsoft.com/library/windows/apps/br227422) et lui attribue la valeur suivante dans l’énumération **Stretch**. Cela permet à l’utilisateur de parcourir les différents états d’étirement.

```xaml
<AppBarButton Icon="Switch"
              Label="Resize Video"
              Click="PictureSize_Click" />
```

```csharp
private void PictureSize_Click(object sender, RoutedEventArgs e)
{
    switch (mediaPlayer.Stretch)
    {
        case Stretch.Fill:
            mediaPlayer.Stretch = Stretch.None;
            break;
        case Stretch.None:
            mediaPlayer.Stretch = Stretch.Uniform;
            break;
        case Stretch.Uniform:
            mediaPlayer.Stretch = Stretch.UniformToFill;
            break;
        case Stretch.UniformToFill:
            mediaPlayer.Stretch = Stretch.Fill;
            break;
        default:
            break;
    }
}
```

### <a name="enable-low-latency-playback"></a>Activer la lecture à faible latence

Définissez la propriété [RealTimePlayback](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.realtimeplayback.aspx) sur **true** sur un [MediaPlayerElement.MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) pour activer l’élément de lecteur multimédia qui réduit la latence initiale de lecture. Ce mode est essentiel pour les applications de communication bidirectionnelle et peut être appliqué à certains scénarios de jeu. Sachez qu’il consomme plus de ressources et qu’il est moins économique en termes d’énergie.

Cet exemple crée un [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) et définit [RealTimePlayback](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.realtimeplayback.aspx) sur **true**.


```csharp
MediaPlayerElement mp = new MediaPlayerElement();
mp.MediaPlayer.RealTimePlayback = true;
```

## <a name="recommendations"></a>Recommandations

Le lecteur multimédia prend en charge les thèmes clairs et sombres, mais un thème sombre offre une meilleure expérience pour la plupart des scénarios de divertissement. L’arrière-plan sombre offre un meilleur contraste, notamment en cas de faible luminosité, et empêche la barre de contrôle de gêner l’affichage.

Quand vous lisez du contenu vidéo, privilégiez un affichage dédié en encourageant l’utilisation du mode plein écran au lieu du mode inséré. L’affichage plein écran offre une expérience optimale, à l’inverse du mode inséré où les options sont limitées.

Si vous disposez d’une grande surface d’écran ou que vous concevez pour une interface à 3mètres («10-foot experience»), adoptez la disposition sur deux lignes. Cette disposition propose plus d’espace pour les contrôles que la disposition plus compacte sur une ligne, et facilite la navigation à l’aide du boîtier de commande pour une interface à 3mètres.

> **Remarque**&nbsp;&nbsp; Consultez l’article [Conception pour Xbox et télévision](../devices/designing-for-tv.md) pour plus d’informations sur l’optimisation de votre application pour une interface à 3mètres.

Les contrôles par défaut ont été optimisés pour la lecture multimédia, toutefois, vous avez la possibilité d’ajouter au lecteur multimédia les options personnalisées dont vous avez besoin afin de fournir la meilleure expérience pour votre application. Consultez [Créer des contrôles de transport personnalisés](custom-transport-controls.md) pour en savoir plus sur l’ajout de contrôles personnalisés.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

- [Informations de base relatives à la conception des commandes pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/dn958433)
- [Informations de base relatives à la conception de contenu pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/dn958434)
