---
author: Jwmsft
Description: "Le lecteur multimédia permet d’afficher et d’écouter des fichiers vidéo, audio et image."
title: "Lecteur multimédia"
ms.assetid: 9AABB5DE-1D81-4791-AB47-7F058F64C491
dev.assetid: AF2F2008-9B53-430C-BBC3-8888F631B0B0
label: Media player
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 2dbc4e7fa227de3f37b8a337eded0004496dbe36

---
# Lecteur multimédia

Le lecteur multimédia permet d’afficher et d’écouter des fichiers vidéo, audio et image. La lecture de média peut utiliser soit un affichage en ligne (incorporé dans une page ou à un groupe d’autres contrôles) soit un affichage plein écran dédié. Vous pouvez modifier le jeu de boutons du lecteur, modifier l’arrière-plan de la barre de contrôle et organiser les dispositions selon vos besoins. Gardez à l’esprit que les utilisateurs s’attendent à un ensemble de contrôles de base (lecture/pause, retour rapide, avance rapide).

![Élément multimédia avec contrôles de transport](images/controls/media-transport-controls.png)

<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [**Classe MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)
-   [**Classe MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediatransportcontrols)

## Est-ce le contrôle approprié ?

Utilisez un lecteur multimédia lorsque vous voulez lire des fichiers audio ou vidéo dans votre application. Pour afficher des collections d’images, utilisez une [vue symétrique](flipview.md).

## Exemples

Un élément multimédia dans l’application Prise en main de Windows 10.

![Un élément multimédia dans l’application Prise en main de Windows 10.](images/control-examples/media-element-getstarted.png)

## Créer un lecteur multimédia
Vous pouvez ajouter un média à votre application en créant un objet [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) en XAML et attribuer à [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) un URI (Uniform Resource Identifier) qui pointe vers un fichier audio ou vidéo.

Cet élément XAML crée un élément [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) et définit sa propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) sur l’URI d’un fichier vidéo stocké en local sur l’application. La lecture de **MediaElement** commence lors du chargement de la page. Pour empêcher la lecture immédiate du contenu multimédia, vous pouvez attribuer à la propriété [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/br227360) la valeur **false**.

```xaml
<MediaElement x:Name="mediaSimple" 
              Source="Videos/video1.mp4" 
              Width="400" AutoPlay="False"/>
```

Le code XAML ci-dessous crée un objet [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926). Les contrôles de transport intégrés y sont activés et la propriété [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/br227360) a pour valeur **false.**.


```csharp
<MediaElement x:Name="mediaPlayer" 
              Source="Videos/video1.mp4" 
              Width="400" 
              AutoPlay="False"
              AreTransportControlsEnabled="True"/>
```

### Contrôles de transport multimédia
MediaElement présente des contrôles de transport qui permettent de gérer la lecture, l’arrêt, la mise en pause, le volume, la désactivation du son (muet), la recherche/progression et la sélection de la piste audio. Pour activer ces contrôles, attribuez à [**AreTransportControlsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn298977) la valeur **true**. Pour les désactiver, attribuez à **AreTransportControlsEnabled** la valeur **false**. Les contrôles de transport sont représentés par la classe [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn831962). Vous pouvez utiliser les contrôles de transport en l’état ou les personnaliser de différentes manières. Pour plus d’informations, voir la référence de classe [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn831962) et [Créer des contrôles de transport personnalisés](custom-transport-controls.md).

Les contrôles de transport permettent à l’utilisateur de contrôler la plupart des aspects du [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), mais le **MediaElement** fournit également de nombreuses propriétés et méthodes que vous pouvez utiliser pour contrôler la lecture audio et vidéo. Pour plus d’informations, voir la section [Contrôle du MediaElement par programme](#control_mediaelement_programmatically) plus loin dans cet article.

Les contrôles de transport prennent en charge les dispositions sur une seule et deux lignes. Le premier exemple ci-dessous correspond à une disposition sur une ligne, avec le bouton lecture/pause situé à gauche de la chronologie des médias. Cette disposition est recommandée pour les écrans compacts. 

![Exemple de contrôles MTC sur téléphone répartis sur une ligne](images/controls_mtc_singlerow_phone.png)

La disposition des contrôles sur deux lignes (voir ci-dessous) est recommandée pour la plupart des scénarios d’utilisation, en particulier sur les écrans plus grands. Cette disposition offre davantage d’espace pour les contrôles et simplifie l’utilisation de la chronologie.

![Exemple de contrôles MTC sur téléphone répartis sur deux lignes](images/controls_mtc_doublerow_phone.png)

**Contrôles de transport de média système**

Vous pouvez également intégrer [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) avec les contrôles de transport de média système. Les contrôles de transport système sont les contrôles qui s’affichent quand l’utilisateur appuie sur une touche de média matériel, comme les boutons de média d’un clavier. Si l’utilisateur appuie sur la touche Pause d’un clavier et que votre application prend en charge la classe [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677), votre application reçoit une notification et vous pouvez effectuer l’action appropriée. Pour plus d’informations, voir [Contrôles de transport de média système](https://msdn.microsoft.com/library/windows/apps/mt228338).

### Définir la source du média
Pour lire des fichiers sur le réseau ou des fichiers incorporés à l’application, affectez à la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) le chemin d’accès du fichier.

**Conseil** Pour ouvrir des fichiers à partir d’Internet, vous devez déclarer la fonctionnalité **Internet (Client)** dans le manifeste de votre application (Package.appxmanifest). Pour plus d’informations sur la déclaration des fonctionnalités, voir [Déclarations des fonctionnalités d’application](https://msdn.microsoft.com/library/windows/apps/mt270968).

 

Ce code tente d’affecter à la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) de l’objet [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) défini en XAML le chemin d’accès d’un fichier entré dans un objet [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683).

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
        mediaPlayer.Source = pathUri;
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

Pour définir un fichier multimédia incorporé dans l’application comme source de média, créez un objet [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017) avec le chemin d’accès précédé du préfixe **ms-appx:///**, puis affectez ce dernier à [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419). Par exemple, pour un fichier nommé **video1.mp4** situé dans un sous-dossier **Videos**, le chemin d’accès se présenterait ainsi : **ms-appx:///Videos/video1.mp4**

Ce code attribue à la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) de l’objet [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) défini précédemment en XAML la valeur **ms-appx:///Videos/video1.mp4**.

```csharp
private void LoadEmbeddedAppFile()
{
    try
    {
        Uri pathUri = new Uri("ms-appx:///Videos/video1.mp4");
        mediaPlayer.Source = pathUri;
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

### Ouvrir des fichiers multimédias locaux
Pour ouvrir des fichiers sur le système local ou à partir de OneDrive, vous pouvez utiliser [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) pour obtenir le fichier et [**SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338) pour définir la source du média, ou vous pouvez accéder par programme aux dossiers de médias de l’utilisateur.

Si votre application a besoin d’accéder aux dossiers **Musique** ou **Vidéo** sans l’interaction de l’utilisateur, par exemple si vous énumérez tous les fichiers de musique ou vidéo de la collection de l’utilisateur pour ensuite les afficher dans votre application, vous devez déclarer les fonctionnalités **Music Library** et **Video Library**. Pour plus d’informations, voir [Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos](https://msdn.microsoft.com/library/windows/apps/mt188703).

Le contrôle [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) ne nécessite pas de fonctionnalités spéciales pour accéder aux fichiers résidant sur le système de fichiers local, comme les dossiers **Musique** ou **Vidéo** de l’utilisateur, car ce dernier bénéficie d’un contrôle total sur l’accès aux fichiers. Du point de vue de la sécurité et de la confidentialité, il est préférable de limiter le nombre de fonctionnalités utilisées par votre application.

**Pour ouvrir un média local à l’aide de FileOpenPicker**

1.  Appelez [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) pour permettre à l’utilisateur de choisir un fichier multimédia.

    Utilisez la classe [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) pour sélectionner un fichier multimédia. Définissez [**FileTypeFilter**](https://msdn.microsoft.com/library/windows/apps/br207850) pour spécifier les types de fichiers affichés par **FileOpenPicker**. Appelez la méthode [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) pour lancer le sélecteur de fichiers et obtenir le fichier.

2.  Appelez la méthode [**SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338) pour définir le fichier multimédia choisi en tant que [**MediaElement.Source**](https://msdn.microsoft.com/library/windows/apps/br227419).

    Pour définir la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) de l’objet [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) sur l’objet [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) retourné par l’objet [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847), vous devez ouvrir un flux. Appelez la méthode [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851) de l’objet **StorageFile**qui retourne un flux que vous pouvez transmettre à la méthode [**MediaElement.SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338). Ensuite, appelez la méthode [**Play**](https://msdn.microsoft.com/library/windows/apps/br227402) sur l’objet **MediaElement** pour démarrer le média.

Cet exemple montre comment utiliser [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) pour choisir un fichier et définir le fichier en tant que propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) d’un [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926).

```xaml
<MediaElement x:Name="mediaPlayer"/>
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
    
    // mediaPlayer is a MediaElement defined in XAML
    if (file != null)
    {
        var stream = await file.OpenAsync(Windows.Storage.FileAccessMode.Read);
        mediaPlayer.SetSource(stream, file.ContentType);

        mediaPlayer.Play();
    }
}
```

### Définir la source de l’affiche
La propriété [**PosterSource**](https://msdn.microsoft.com/library/windows/apps/br227409) vous permet de fournir à votre [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) une représentation visuelle avant le chargement du média. Un **PosterSource** est une image (par exemple, une capture d’écran ou une affiche de film) qui est affichée à la place du média. Le **PosterSource** est affiché dans les situations suivantes :

-   Quand aucune source valide n’est définie. Par exemple, si la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) n’est pas définie, si **Source** est définie sur **Null** ou si la source n’est pas valide (comme lorsqu’un événement [**MediaFailed**](https://msdn.microsoft.com/library/windows/apps/br227393) se produit).
-   Lors du chargement du média. Par exemple, une source valide est définie mais l’événement [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/br227394) ne s’est toujours pas produit.
-   Quand le média est diffusé vers un autre appareil.
-   Quand le média est un fichier audio uniquement.

Voici un [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) dont les propriétés [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) sont définies sur une piste d’album, et dont la propriété [**PosterSource**](https://msdn.microsoft.com/library/windows/apps/br227409) est définie sur une image de la couverture d’album.

```xaml
<MediaElement Source="Media/Track1.mp4" PosterSource="Media/AlbumCover.png"/> 
```

### Maintenir l’écran de l’appareil actif
Normalement, un appareil estompe l’affichage (et finit par le désactiver) pour préserver l’autonomie de la batterie lorsque l’utilisateur s’absente, mais les applications vidéo doivent maintenir l’écran allumé pour que l’utilisateur puisse voir la vidéo. Pour empêcher que l’affichage soit désactivé lorsque plus aucune action utilisateur n’est détectée (par exemple quand une application lit une vidéo plein écran), vous pouvez appeler [**DisplayRequest.RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818). La classe [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) vous permet d’indiquer à Windows de laisser l’affichage activé pour permettre à l’utilisateur de voir la vidéo.

Pour économiser l’énergie et prolonger l’autonomie de la batterie, vous devez appeler [**DisplayRequest.RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819) pour libérer la demande d’affichage lorsqu’elle n’est plus nécessaire. Windows désactive automatiquement les demandes d’affichage actives de votre application lorsque cette dernière n’est plus à l’écran, et les réactive quand votre application revient au premier plan.

Voici quelques situations où vous devez libérer la demande d’affichage :

-   La lecture vidéo est suspendue, par exemple suite à une action de l’utilisateur, une mise en mémoire tampon ou un réglage en raison de la bande passante limitée.
-   La lecture s’arrête. Par exemple, la lecture de la vidéo ou la présentation est terminée.
-   Une erreur de lecture s’est produite. Il peut s’agir de problèmes de connectivité réseau ou d’un fichier endommagé.

**Pour maintenir l’écran actif**

1.  Créez une variable [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) globale. Initialisez-la sur la valeur Null.
```csharp
// Create this variable at a global scope. Set it to null.
private DisplayRequest appDisplayRequest = null;
```

2.  Appelez [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818) pour signaler à Windows que l’application requiert que l’affichage reste activé.

3.  Appelez [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819) pour supprimer la demande d’affichage chaque fois que la lecture vidéo est arrêtée, suspendue ou interrompue par une erreur de lecture. Lorsque votre application n’a plus de demandes d’affichage actives, Windows préserve l’autonomie de la batterie en estompant l’affichage (et finit par le désactiver) lorsque l’appareil n’est pas utilisé.

    Ici, l’événement [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) est utilisé pour détecter ces situations. Utilisez ensuite la propriété [**IsAudioOnly**](https://msdn.microsoft.com/library/windows/apps/hh965334) pour déterminer si un fichier audio ou vidéo est en cours de lecture, et maintenir l’écran actif uniquement si une vidéo est en cours de lecture.
    ```xaml
<MediaElement Source="Media/video1.mp4"
              CurrentStateChanged="MediaElement_CurrentStateChanged"/>
    ```
 
    ```csharp
private void MediaElement_CurrentStateChanged(object sender, RoutedEventArgs e)
{
    MediaElement mediaElement = sender as MediaElement;
    if (mediaElement != null && mediaElement.IsAudioOnly == false)
    {
        if (mediaElement.CurrentState == Windows.UI.Xaml.Media.MediaElementState.Playing)
        {                
            if (appDisplayRequest == null)
            {
                // This call creates an instance of the DisplayRequest object. 
                appDisplayRequest = new DisplayRequest();
                appDisplayRequest.RequestActive();
            }
        }
        else // CurrentState is Buffering, Closed, Opening, Paused, or Stopped. 
        {
            if (appDisplayRequest != null)
            {
                // Deactivate the display request and set the var to null.
                appDisplayRequest.RequestRelease();
                appDisplayRequest = null;
            }
        }            
    }
} 
    ```

### Contrôler le lecteur multimédia par programme
[
            **MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) propose divers événements, propriétés et méthodes pour contrôler la lecture audio et vidéo. Pour obtenir la liste complète des propriétés, méthodes et événements, voir la page de référence de [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926).
    

### Sélectionner des pistes audio de langues différentes

Utilisez la propriété [**AudioStreamIndex**](https://msdn.microsoft.com/library/windows/apps/br227358) et la méthode [**GetAudioStreamLanguage**](https://msdn.microsoft.com/library/windows/apps/br227384) pour modifier la langue d’une piste audio de vidéo. Les vidéos peuvent aussi contenir plusieurs pistes audio de même langue, comme les commentaires du réalisateur d’un film. Cet exemple de code montre comment basculer entre différentes langues, mais vous pouvez le modifier pour basculer entre des pistes audio.

**Pour sélectionner des pistes audio de langues différentes**

1.  Obtenez les pistes audio.

    Pour rechercher une piste dans une langue spécifique, commencez par parcourir chaque piste audio de la vidéo. Utilisez [**AudioStreamCount**](https://msdn.microsoft.com/library/windows/apps/br227356) comme valeur maximale d’une boucle **for**.

2.  Obtenez la langue de la piste audio.

    Utilisez la méthode [**GetAudioStreamLanguage**](https://msdn.microsoft.com/library/windows/apps/br227384) pour obtenir la langue de la piste. La langue de la piste est identifiée par un [code langue](http://msdn.microsoft.com/library/ms533052(vs.85).aspx), tel que **"en"** pour l’anglais ou **"ja"** pour le japonais.

3.  Définissez la piste audio active.

    Une fois que vous avez trouvé la piste dans la langue souhaitée, associez l’index de la piste à [**AudioStreamIndex**](https://msdn.microsoft.com/library/windows/apps/br227358). Le fait de définir **AudioStreamIndex** sur **null** permet de sélectionner la piste audio par défaut qui est définie par le contenu.

Le code ci-dessous vise à associer la langue spécifiée à la piste audio. Cet exemple parcourt les pistes audio sur un objet [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) et utilise [**GetAudioStreamLanguage**](https://msdn.microsoft.com/library/windows/apps/br227384) pour obtenir la langue de chaque piste. Si la piste existe dans la langue souhaitée, la propriété [**AudioStreamIndex**](https://msdn.microsoft.com/library/windows/apps/br227358) est associée à l’index de cette piste.

```csharp
/// <summary>
/// Attemps to set the audio track of a video to a specific language
/// </summary>
/// <param name="lcid">The id of the language. For example, "en" or "ja"</param>
/// <returns>true if the track was set; otherwise, false.</returns>
private bool SetAudioLanguage(string lcid, MediaElement media)
{
    bool wasLanguageSet = false;

    for (int index = 0; index < media.AudioStreamCount; index++)
    {
        if (media.GetAudioStreamLanguage(index) == lcid)
        {
            media.AudioStreamIndex = index;
            wasLanguageSet = true;
        }
    }

    return wasLanguageSet;
}
```

### Activer le rendu vidéo dans une fenêtre entière

Définissez la propriété [**IsFullWindow**](https://msdn.microsoft.com/library/windows/apps/dn298980) pour activer et désactiver le rendu dans une fenêtre entière. Lorsque vous définissez le rendu dans une fenêtre entière par programme dans votre application, vous devez toujours utiliser la propriété **IsFullWindow** au lieu de le faire manuellement. Avec la propriété **IsFullWindow**, vous êtes assuré que les optimisations au niveau du système sont exécutées, ce qui améliore les performances et l’autonomie de la batterie. Si l’affichage du rendu dans une fenêtre entière n’est pas configuré correctement, il se peut que ces optimisations ne soient pas activées.

Le code ci-dessous crée un objet [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) qui active/désactive le rendu dans une fenêtre entière.

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

### Redimensionner et étirer une vidéo

Utilisez la propriété [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br227422) pour modifier la façon dont le contenu vidéo remplit son conteneur. Le redimensionnement et l’étirement de la vidéo qui s’ensuit dépendent de la valeur de [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br242968). Les états de **Stretch** sont comparables aux réglages du format de l’image de nombreux téléviseurs. Vous pouvez l’accrocher à un bouton et permettre ainsi à l’utilisateur d’effectuer les réglages de son choix.

-   [
            **None**](https://msdn.microsoft.com/library/windows/apps/br242968) affiche la résolution native du contenu dans sa taille d’origine.
-   [
            **Uniform**](https://msdn.microsoft.com/library/windows/apps/br242968) remplit autant d’espace que possible tout en préservant les proportions et le contenu de l’image. Des bandes noires horizontales ou verticales peuvent dès lors apparaître sur les bordures de la vidéo. L’effet obtenu est comparable aux modes grand écran.
-   [
            **UniformToFill**](https://msdn.microsoft.com/library/windows/apps/br242968) remplit l’espace entier tout en préservant les proportions. Dans ce cas, une partie de l’image peut être rognée. L’effet obtenu est comparable aux modes plein écran.
-   [
            **Fill**](https://msdn.microsoft.com/library/windows/apps/br242968) remplit l’espace entier, mais ne conserve pas les proportions. Si l’image n’est pas rognée, un étirement peut en revanche être observé. L’effet obtenu est comparable aux modes Étirer.

![Valeurs de l’énumération Stretch](images/Image_Stretch.jpg) Ici, une classe [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) est utilisée pour faire défiler les options [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br242968). Une instruction **switch** vérifie l’état actif de la propriété [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br227422) et lui attribue la valeur suivante dans l’énumération **Stretch**. Cela permet à l’utilisateur de parcourir les différents états d’étirement.

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

### Activer la lecture à faible latence

Attribuez à la propriété [**RealTimePlayback**](https://msdn.microsoft.com/library/windows/apps/br227414) la valeur **true** sur un [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) pour activer l’élément média qui réduit la latence initiale de lecture. En plus d’être essentiel pour les applications de communication bidirectionnelle, ce mode peut être appliqué à certains scénarios de jeu. Sachez que ce mode consomme plus de ressources et est moins économique en termes d’énergie.

Cet exemple crée un objet [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) et définit la propriété [**RealTimePlayback**](https://msdn.microsoft.com/library/windows/apps/br227414) sur **true**.

```xaml
<MediaElement x:Name="mediaPlayer" RealTimePlayback="True"/>
```

```csharp
MediaElement mediaPlayer = new MediaElement();
mediaPlayer.RealTimePlayback = true;
```
    
## Recommandations 

Le lecteur multimédia existe dans un thème foncé et un thème clair. Le thème foncé est toutefois recommandé dans la plupart des situations. L’arrière-plan foncé offre un meilleur contraste, notamment en cas de faible luminosité, et empêche la barre de contrôle de gêner l’affichage.

Privilégiez un affichage dédié en encourageant l’utilisation du mode plein écran au lieu du mode en ligne. L’affichage plein écran offre une expérience optimale, à l’inverse du mode en ligne, où les options sont limitées.

Si vous disposez d’un espace suffisant à l’écran, optez pour la disposition sur deux lignes. Cette disposition offre davantage d’espace pour les contrôles que la disposition compacte répartie sur une ligne.

Ajoutez toutes les options personnalisées dont vous avez besoin pour le lecteur multimédia, afin de fournir la meilleure expérience possible pour votre application, tout en gardant à l’esprit les éléments suivants :

-   Limitez la personnalisation des contrôles par défaut, qui ont été optimisés pour l’expérience de lecture multimédia.
-   Le chrome de l’appareil reste noir sur les téléphones et autres appareils mobiles, mais hérite de la couleur du thème de l’utilisateur sur les ordinateurs portables et ordinateurs de bureau.
-   Essayez de ne pas charger la barre de contrôle en ajoutant trop d’options.
-   Ne réduisez pas la chronologie de médias en dessous de sa taille minimale par défaut. Cela limitera considérablement son efficacité.

## Articles connexes

- [Informations de base relatives à la conception des commandes pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/dn958433)
- [Informations de base relatives à la conception de contenu pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/dn958434)



<!--HONumber=Jun16_HO4-->


