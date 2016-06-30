---
author: drewbatgit
ms.assetid: C5623861-6280-4352-8F22-80EB009D662C
description: "La classe MediaSource offre une méthode courante de référencement et de lecture de contenu multimédia à partir de différentes sources telles que des fichiers locaux ou à distance et elle présente un modèle commun d’accès aux données multimédias, quel que soit le format multimédia sous-jacent."
title: "Lecture de contenu multimédia avec MediaSource"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: d64f4484566d80eaf2a353b1aba954c15079343c

---

# Lecture de contenu multimédia avec MediaSource

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


\[Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.\]

La classe [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930905) offre une méthode courante de référencement et de lecture de contenu multimédia à partir de différentes sources telles que des fichiers locaux ou à distance et elle présente un modèle commun d’accès aux données multimédias, quel que soit le format multimédia sous-jacent. La classe [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) étend les fonctionnalités de **MediaSource**, vous permettant ainsi de gérer et de sélectionner à partir de plusieurs pistes audio, vidéo et de métadonnées contenues dans un élément multimédia. [
            **MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) vous permet de créer des listes de lecture à partir d’un ou plusieurs éléments de la lecture de contenu multimédia.

Le code figurant dans cet article a été adapté à partir de l’exemple [Kit de développement logiciel (SDK) de lecture vidéo](http://go.microsoft.com/fwlink/p/?LinkId=620020&clcid=0x409). Vous pouvez télécharger cet exemple pour voir le code utilisé en contexte ou pour vous en servir comme point de départ pour votre propre application.

## Créer et lire un MediaSource

Créez une instance de **MediaSource** en appelant l’une des méthodes de fabrique exposées par la classe :

-   [**CreateFromAdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930906)
-   [**CreateFromIMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn965527)
-   [**CreateFromMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930907)
-   [**CreateFromMseStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930908)
-   [**CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909)
-   [**CreateFromStream**](https://msdn.microsoft.com/library/windows/apps/dn930910)
-   [**CreateFromStreamReference**](https://msdn.microsoft.com/library/windows/apps/dn930911)
-   [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912)

Après avoir créé un **MediaSource**, vous pouvez lire la source directement avec un [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) en appelant [**SetPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/dn899085), ou avec un [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652535) en définissant la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/dn987010). L’exemple suivant montre comment lire un fichier multimédia sélectionné par l’utilisateur dans un **MediaElement** à l’aide de **MediaSource**.

Vous devez inclure les espaces de noms [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) et [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) pour pouvoir terminer ce scénario.

[!code-cs[Using](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetUsing)]

Déclarez une variable de type **MediaSource**. Pour les exemples de cet article, la source multimédia est déclarée en tant que membre de classe. Elle est donc accessible à partir de plusieurs emplacements.

[!code-cs[DeclareMediaSource](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaSource)]

Pour permettre à l’utilisateur de sélectionner un fichier multimédia à lire, utilisez un [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847). Avec l’objet [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) retourné de la méthode [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) du sélecteur, initialisez un nouveau MediaObject en appelant [**MediaSource.CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909). Enfin, définissez la source du média en tant que source de lecture de **MediaElement** en appelant la méthode [**SetPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/dn899085).

[!code-cs[PlayMediaSource](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPlayMediaSource)]

## Gérer plusieurs pistes audio, vidéo et de métadonnées avec MediaPlaybackItem

L’utilisation d’un [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930905) pour la lecture est pratique car il offre une méthode courante de lecture de contenu multimédia à partir de différents types de sources. Un comportement plus avancé est toutefois disponible à l’aide d’un [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939). Il inclut la possibilité d’accéder et de gérer plusieurs pistes audio, vidéo et de données pour un élément multimédia.

Déclarez une variable pour stocker votre **MediaPlaybackItem**.

[!code-cs[DeclareMediaPlaybackItem](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackItem)]

Créez un **MediaPlaybackItem** en appelant le constructeur et en le transmettant à un objet **MediaSource** initialisé.

Si votre application prend en charge plusieurs pistes audio, vidéo ou de données dans un élément de lecture multimédia, inscrivez des gestionnaires d’événements pour les événements [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948), [**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) ou [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952).

Enfin, définissez la source de la lecture de **MediaElement** ou **MediaPlayer** sur votre **MediaPlaybackItem**.

[!code-cs[PlayMediaPlaybackItem](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackItem)]

**Remarque**  
Un **MediaSource** ne peut être associé qu’à un seul **MediaPlaybackItem**. Après avoir créé un **MediaPlaybackItem** à partir d’une source, toute tentative de création d’un autre élément de lecture à partir de la même source entraînera une erreur. De plus, après avoir créé un **MediaPlaybackItem** à partir d’une source multimédia, vous ne pouvez pas définir l’objet **MediaSource** directement en tant que source d’un **MediaElement** ou **MediaPlayer**, mais vous devez plutôt utiliser le **MediaPlaybackItem**.

L’événement [**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) est déclenché après qu’un **MediaPlaybackItem** contenant plusieurs pistes vidéo a été affecté en tant que source de lecture, et peut être déclenché à nouveau si la liste des pistes vidéo change pour l’élément. Le gestionnaire de cet événement vous permet de mettre à jour votre interface utilisateur, permettant ainsi à l’utilisateur de basculer entre les pistes disponibles. Cet exemple utilise un [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348) pour afficher les pistes vidéo disponibles.

[!code-xml[VideoComboBox](./code/MediaSource_Win10/cs/MainPage.xaml#SnippetVideoComboBox)]

Dans le gestionnaire **VideoTracksChanged**, parcourez toutes les pistes de la liste [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953) de l’élément de lecture. Un nouveau [**ComboBoxItem**](https://msdn.microsoft.com/library/windows/apps/br209349) est créé pour chaque piste. Si la piste ne porte déjà une étiquette, une étiquette est générée à partir de l’index de piste. La propriété [**Tag**](https://msdn.microsoft.com/library/windows/apps/br208745) de l’élément de zone de liste déroulante est définie sur l’index de piste pour pouvoir l’identifier ultérieurement. Enfin, l’élément est ajouté à la zone de liste déroulante. Notez que ces opérations sont effectuées dans le cadre d’un appel [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317), car toutes les modifications de l’interface utilisateur doivent être apportées sur le thread d’interface utilisateur et que cet événement est déclenché sur un autre thread.

[!code-cs[VideoTracksChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetVideoTracksChanged)]

Dans le gestionnaire [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) de la zone de liste déroulante, l’index de piste est récupéré à partir de la propriété **Tag** de l’élément sélectionné. La définition de la propriété [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn956634) de la liste [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953) de l’élément de lecture multimédia amène **MediaElement** ou **MediaPlayer** à basculer la piste vidéo active sur l’index spécifié.

[!code-cs[VideoTracksSelectionChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetVideoTracksSelectionChanged)]

La gestion des éléments multimédias avec des pistes audio multiples est strictement identique à celle des pistes vidéo. Gérez le [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948) pour mettre à jour votre interface utilisateur avec les pistes audio trouvées dans la liste [**AudioTracks**](https://msdn.microsoft.com/library/windows/apps/dn930947) de l’élément de lecture. Lorsque l’utilisateur sélectionne une piste audio, définissez la propriété [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn930937) de la liste **AudioTracks** pour que **MediaElement** ou **MediaPlayer** bascule la piste audio active sur l’index spécifié.

[!code-xml[AudioComboBox](./code/MediaSource_Win10/cs/MainPage.xaml#SnippetAudioComboBox)]

[!code-cs[AudioTracksChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAudioTracksChanged)]

[!code-cs[AudioTracksSelectionChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAudioTracksSelectionChanged)]

En plus de l’audio et de la vidéo, un objet **MediaPlaybackItem** peut contenir zéro ou plusieurs objets [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580). Une piste de métadonnées synchronisée peut contenir du texte de sous-titre. Elle peut également contenir des données personnalisées propriétaires de votre application. Une piste de métadonnées synchronisée contient une liste d’indicateurs représentés par des objets qui héritent de [**IMediaCue**](https://msdn.microsoft.com/library/windows/apps/dn930899), un [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) ou un [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655) par exemple. Chaque indicateur a une heure de début et une durée qui déterminent quand l’indicateur est activé et pour combien de temps.

Comme les pistes audio et vidéo, les pistes de métadonnées synchronisées d’un élément multimédia peuvent être détectées en gérant l’événement [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952) d’un **MediaPlaybackItem**. Toutefois, avec les pistes de métadonnées synchronisées, l’utilisateur souhaitera peut-être activer plusieurs pistes de métadonnées simultanément. De plus, en fonction de votre scénario d’application, vous souhaiterez peut-être activer ou désactiver automatiquement des pistes de métadonnées, sans l’intervention de l’utilisateur. À des fins d’illustration, l’exemple suivant ajoute un [**ToggleButton**](https://msdn.microsoft.com/library/windows/apps/br209795) pour chaque piste de métadonnées d’un élément multimédia pour permettre à l’utilisateur d’activer et de désactiver la piste. La propriété **Tag** de chaque bouton est définie sur l’index de la piste de métadonnées associée pour pouvoir l’identifier lorsque le bouton est activé.

[!code-xml[MetaStackPanel](./code/MediaSource_Win10/cs/MainPage.xaml#SnippetMetaStackPanel)]

[!code-cs[TimedMetadataTrackschanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedMetadataTrackschanged)]

Étant donné que plusieurs pistes de métadonnées peuvent être actives simultanément, vous ne définissez pas simplement l’index actif de la liste de pistes de métadonnées. Appelez plutôt le **MediaPlaybackItem** de la méthode [**SetPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn986977) de l’objet, en indiquant l’index de la piste que vous souhaitez activer/désactiver, puis en spécifiant une valeur à partir de l’énumération [**TimedMetadataTrackPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn987016). Le mode de présentation que vous choisissez dépend de l’implémentation de votre application. Dans cet exemple, la piste de métadonnées est définie sur **PlatformPresented** lorsqu’elle est activée. Pour les pistes de texte, cela signifie que le système affichera automatiquement les indicateurs de texte dans la piste. Lorsque le bouton d’activation/de désactivation est désactivé, le mode de présentation est défini sur **Disabled**, ce qui signifie qu’aucun texte ne s’affiche et qu’aucun événement d’indicateur n’est déclenché. Les événements d’indicateur sont présentés plus loin dans cet article.

[!code-cs[ToggleChecked](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetToggleChecked)]

[!code-cs[ToggleUnchecked](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetToggleUnchecked)]

## Ajouter du texte synchronisé externe avec TimedTextSource

Dans certains scénarios, vous pouvoir disposer de fichiers externes contenant du texte synchronisé associé à un élément multimédia, des fichiers distincts contenant des sous-titres pour différents paramètres régionaux par exemple. Utilisez la classe [**TimedTextSource**](https://msdn.microsoft.com/library/windows/apps/dn956679) pour charger les fichiers de texte synchronisé externe à partir d’un flux ou d’une URI.

Cet exemple utilise une collection **Dictionary** pour stocker une liste de sources de texte synchronisé pour l’élément multimédia à l’aide de l’URI source et de l’objet **TimedTextSource** en tant que paire clé/valeur afin d’identifier les pistes une fois qu’elles ont été résolues.

[!code-cs[TimedTextSourceMap](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedTextSourceMap)]

Créez un **TimedTextSource** pour chaque fichier de texte synchronisé externe en appelant la méthode [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn708190). Ajoutez une entrée pour le **Dictionary** de la source de texte synchronisé. Ajoutez un gestionnaire pour l’événement [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540) à gérer si l’élément n’a pas pu être chargé ou pour définir des propriétés supplémentaires lorsque l’élément a bien été chargé.

Inscrivez tous vos objets **TimedTextSource** avec **MediaSource** en les ajoutant à la collection [**ExternalTimedTextSources**](https://msdn.microsoft.com/library/windows/apps/dn930916). Notez que les sources de texte synchronisé externe sont ajoutées directement à **MediaSource** et non au **MediaPlaybackItem** créé à partir de la source. Pour mettre à jour votre interface utilisateur afin de refléter les pistes de texte externe, inscrivez et gérez l’événement **TimedMetadataTracksChanged** comme décrit précédemment dans cet article.

[!code-cs[TimedTextSource](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedTextSource)]

Dans le gestionnaire de l’événement [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540), vérifiez la propriété **Error** de [**TimedTextSourceResolveResultEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn965537) transmise au gestionnaire pour déterminer si une erreur s’est produite lors de la tentative de chargement des données de texte synchronisé. Si l’élément a bien été résolu, vous pouvez utiliser ce gestionnaire pour mettre à jour d’autres propriétés de la piste résolue. Cet exemple ajoute une étiquette à chaque piste en fonction de l’URI préalablement enregistrée dans **Dictionary**.

[!code-cs[TimedTextSourceResolved](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedTextSourceResolved)]

## Ajouter des pistes de métadonnées supplémentaires

Vous pouvez créer des pistes de métadonnées personnalisées de manière dynamique dans le code et les associer à une source de média. Les pistes que vous créez peuvent contenir du texte de sous-titre. Elles peuvent également contenir vos données d’application propriétaires.

Créez un [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580) en appelant le constructeur et en spécifiant un ID, l’identificateur de langue et une valeur de l’énumération [**TimedMetadataKind**](https://msdn.microsoft.com/library/windows/apps/dn956578). Enregistrez des gestionnaires pour les événements [**CueEntered**](https://msdn.microsoft.com/library/windows/apps/dn956583) et [**CueExited**](https://msdn.microsoft.com/library/windows/apps/dn956584). Ces événements sont déclenchés lorsque l’heure de début d’un indicateur est atteinte et lorsque la durée d’un indicateur s’est écoulée, respectivement.

Créez un nouvel objet d’indicateur, adapté au type de piste de métadonnées que vous avez créée, puis définissez l’ID, l’heure de début et la durée de la piste. Cet exemple crée une piste de données, un ensemble d’objet [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) est donc généré et une mémoire tampon contenant des données spécifiques à l’application est fournie pour chaque indicateur. Pour inscrire la nouvelle piste, ajoutez-la à la collection [**ExternalTimedMetadataTracks**](https://msdn.microsoft.com/library/windows/apps/dn930915) de l’objet **MediaSource**.

[!code-cs[AddDataTrack](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAddDataTrack)]

L’événement **CueEntered** est déclenché lorsque l’heure de début d’un indicateur est atteinte alors que la piste associée dispose d’un mode de présentation **ApplicationPresented**, **Hidden** ou **PlatformPresented.**. Les événements d’indicateur ne sont pas déclenchés pour les pistes de métadonnées lorsque le mode de présentation de la piste est **Disabled**. Cet exemple présente simplement les données personnalisées associées à l’indicateur dans la fenêtre de débogage.

[!code-cs[DataCueEntered](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDataCueEntered)]

Cet exemple ajoute une piste de texte personnalisé en spécifiant **TimedMetadataKind.Caption** lors de la création de la piste et de l’utilisation d’objets [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655) pour ajouter des indicateurs à la piste.

[!code-cs[AddTextTrack](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAddTextTrack)]

[!code-cs[TextCueEntered](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTextCueEntered)]

## Lire une liste d’éléments multimédias avec MediaPlaybackList

[
            **MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) vous permet de créer une playlist d’éléments multimédias, qui sont représentés par des objets **MediaPlaybackItem**.

**Remarque** Les éléments figurant dans [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) sont rendus à l’aide de la lecture sans blanc. Le système utilise les métadonnées fournies dans les fichiers codés MP3 ou AAC pour déterminer la compensation de délai ou de remplissage nécessaire pour la lecture sans blanc. Si les fichiers codés MP3 ou AAC ne fournissent pas ces métadonnées, le système détermine alors le délai ou le remplissage de manière heuristique. Pour les formats sans perte, tels que PCM, FLAC ou ALAC, le système n’exécute aucune action, car ces encodeurs n’introduisent ni retard ni remplissage.

Pour commencer, déclarez une variable pour stocker votre **MediaPlaybackList**.

[!code-cs[DeclareMediaPlaybackList](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackList)]

Créez un **MediaPlaybackItem** pour chaque élément multimédia que vous voulez ajouter à votre liste en suivant la procédure décrite précédemment dans cet article. Initialisez votre objet **MediaPlaybackList** et ajoutez-y les éléments de lecture multimédia. Inscrivez un gestionnaire pour l’événement [**CurrentItemChanged**](https://msdn.microsoft.com/library/windows/apps/dn930957). Cet événement vous permet de mettre à jour votre interface utilisateur afin de refléter l’élément multimédia en cours de lecture. Enfin, définissez la source de la lecture de **MediaElement** ou **MediaPlayer** sur votre **MediaPlaybackList**.

[!code-cs[PlayMediaPlaybackList](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackList)]

Dans le gestionnaire d’événements **CurrentItemChanged**, mettez à jour votre interface utilisateur afin de refléter l’élément en cours de lecture, qui peut être récupéré à l’aide de la propriété [**NewItem**](https://msdn.microsoft.com/library/windows/apps/dn930930) de l’objet [**CurrentMediaPlaybackItemChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn930929) transmis dans l’événement. N’oubliez pas que si vous mettez à jour l’interface utilisateur à partir de cet événement, vous devez le faire dans le cadre d’un appel à [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) afin que les mises à jour soient effectuées sur le thread d’interface utilisateur.

[!code-cs[MediaPlaybackListItemChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetMediaPlaybackListItemChanged)]

Appelez [**MovePrevious**](https://msdn.microsoft.com/library/windows/apps/mt146455) ou [**MoveNext**](https://msdn.microsoft.com/library/windows/apps/mt146454) pour que le lecteur multimédia lise l’élément précédent ou suivant de votre **MediaPlaybackList**.

[!code-cs[PrevButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPrevButton)]

[!code-cs[NextButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetNextButton)]

Définissez la propriété [**ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146457) pour spécifier si le lecteur multimédia doit lire les éléments de votre liste dans un ordre aléatoire.

[!code-cs[ShuffleButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetShuffleButton)]

Définissez la propriété [**AutoRepeatEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146452) pour spécifier si le lecteur multimédia doit lire votre liste en boucle.

[!code-cs[RepeatButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetRepeatButton)]

 

 







<!--HONumber=Jun16_HO4-->


