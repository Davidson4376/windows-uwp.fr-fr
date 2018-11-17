---
author: drewbatgit
ms.assetid: EFCF84D0-2F4C-454D-97DA-249E9EAA806C
description: La classe SystemMediaTransportControls permet à votre application d’utiliser les contrôles dédiés intégrés à Windows et de mettre à jour les métadonnées affichées par les contrôles concernant le média lu actuellement par votre application.
title: Contrôle manuel des contrôles de transport de média système
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b0ece9a25a2fd2892553d66847c39637e7faae70
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7145974"
---
# <a name="manual-control-of-the-system-media-transport-controls"></a>Contrôle manuel des contrôles de transport de média système


À partir de Windows10, version1607, les applications UWP utilisant la classe [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) pour lire du contenu multimédia sont automatiquement intégrés avec les contrôles de transport de média système par défaut. Il s’agit de la méthode recommandée d’interaction avec les contrôles de transport de média système, pour la plupart des scénarios. Pour plus d’informations sur la personnalisation de l’intégration par défaut des contrôles de transport de média système avec **MediaPlayer**, consultez la section [Intégration avec les contrôles de transport de média système](integrate-with-systemmediatransportcontrols.md).

Certains scénarios peuvent nécessiter l’implémentation d’un contrôle manuel des contrôles de transport de média système. Cela vaut par exemple si vous utilisez une classe [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController) afin de contrôler la lecture d’un ou de plusieurs lecteurs multimédias. Cette implémentation est également nécessaire si vous utilisez plusieurs lecteurs multimédias et souhaitez obtenir une seule instance des contrôles de transport de média système pour votre application. Vous devez contrôler manuellement les contrôles de transport de média système si vous utilisez [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaElement) pour lire du contenu multimédia.

## <a name="set-up-transport-controls"></a>Installer les contrôles de transport
Si vous utilisez **MediaPlayer** pour lire du contenu multimédia, vous pouvez obtenir une instance de la classe [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.SystemMediaTransportControls) en accédant à la propriété [**MediaPlayer.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.SystemMediaTransportControls). SI vous envisagez de contrôler manuellement les contrôles de transport de média système, vous devez désactiver l’intégration automatique fournie par **MediaPlayer** en définissant la propriété [**CommandManager.IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) sur False.

> [!NOTE] 
> Si vous désactivez l’élément [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) de l’instance [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) en définissant [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) sur false, le lien entre **MediaPlayer** et [**TransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.TransportControls) fourni par **MediaPlayerElement** est rompu; autrement dit, les contrôles de transport intégrés ne contrôleront plus automatiquement la lecture du lecteur. Vous devrez donc implémenter vos propres contrôles pour pouvoir contrôler le **MediaPlayer**.

[!code-cs[InitSMTCMediaPlayer](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetInitSMTCMediaPlayer)]

Vous pouvez également récupérer une instance de la classe [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) en appelant [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708). Vous devez récupérer l’objet avec cette méthode si vous utilisez **MediaElement** pour lire du contenu multimédia.

[!code-cs[InitSMTCMediaElement](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetInitSMTCMediaElement)]

Activez les boutons utilisés par votre application en définissant la propriété «is enabled» correspondante de l’objet **SystemMediaTransportControls**, telle que [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714), [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713), [**IsNextEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278712) et [**IsPreviousEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278715). Voir la documentation de référence de **SystemMediaTransportControls** pour obtenir la liste complète des contrôles disponibles.

[!code-cs[EnableContols](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetEnableContols)]

Enregistrez un gestionnaire pour l’événement [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) afin de recevoir des notifications dès que l’utilisateur appuie sur un bouton.

[!code-cs[RegisterButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetRegisterButtonPressed)]

## <a name="handle-system-media-transport-controls-button-presses"></a>Gérer les pressions sur les boutons de contrôles de transport de média système

L’événement [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) est déclenché par les contrôles de transport système quand l’utilisateur appuie sur l’un des boutons activés. La propriété [**Button**](https://msdn.microsoft.com/library/windows/apps/dn278685) de la classe [**SystemMediaTransportControlsButtonPressedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn278683) transmise au gestionnaire d’événements est un membre de l’énumération [**SystemMediaTransportControlsButton**](https://msdn.microsoft.com/library/windows/apps/dn278681) qui indique de quel bouton activé il s’agit.

Pour mettre à jour les objets du thread d’interface utilisateur à partir du gestionnaire d’événements [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706), tel qu’un objet [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), vous devez marshaler les appels via [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211). Cela vient du fait que le gestionnaire d’événements **ButtonPressed** n’est pas appelé à partir du thread d’interface utilisateur. Par conséquent, une exception est générée si vous tentez de modifier directement l’interface utilisateur.

[!code-cs[SystemMediaTransportControlsButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsButtonPressed)]

## <a name="update-the-system-media-transport-controls-with-the-current-media-status"></a>Mettre à jour les contrôles de transport de média système en tenant compte de l’état actuel du média

Vous devez avertir la classe [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) en cas de modification de l’état du média afin que le système puisse mettre à jour les contrôles de manière à refléter l’état actuel. Pour ce faire, définissez la propriété [**PlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278719) sur la valeur [**MediaPlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278665) appropriée depuis l’événement [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) de la classe [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), qui est déclenché dès que l’état du média change.

[!code-cs[SystemMediaTransportControlsStateChange](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsStateChange)]

## <a name="update-the-system-media-transport-controls-with-media-info-and-thumbnails"></a>Mettre à jour les contrôles de transport de média système en tenant compte des informations relatives au média et des miniatures

Utilisez la classe [**SystemMediaTransportControlsDisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278686) pour mettre à jour les informations du média affichées par les contrôles de transport, telles que le titre de la chanson ou la pochette de l’album pour l’élément multimédia en cours de lecture. Obtenez une instance de cette classe avec la propriété [**SystemMediaTransportControls.DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707). Pour les scénarios classiques, le mode de transmission des métadonnées recommandé consiste à appeler [**CopyFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn278694) en transmettant le fichier multimédia en cours de lecture. L’outil de mise à jour de l’affichage extrait automatiquement les métadonnées et l’image miniature du fichier.

Appelez [**Update**](https://msdn.microsoft.com/library/windows/apps/dn278701) pour que les contrôles de transport de média système mettent à jour son interface utilisateur en tenant compte des nouvelles métadonnées et des miniatures.

[!code-cs[SystemMediaTransportControlsUpdater](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsUpdater)]

Si votre scénario l’exige, vous pouvez mettre à jour manuellement les métadonnées affichées par les contrôles de transport de média système en définissant les valeurs des objets [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/dn278696), [**ImageProperties**](https://msdn.microsoft.com/library/windows/apps/dn278695) ou [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/dn278702) exposés par la classe [**DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707).

[!code-cs[SystemMediaTransportControlsUpdaterManual](./code/SMTCWin10/cs/MainPage.xaml.cs#SystemMediaTransportControlsUpdaterManual)]

## <a name="update-the-system-media-transport-controls-timeline-properties"></a>Mettre à jour les propriétés de chronologie des contrôles de transport de média système

Les contrôles de transport système affichent des informations sur la chronologie de l’élément multimédia en cours de lecture, y compris la position de lecture actuelle, son heure de début et son heure de fin. Pour mettre à jour les propriétés de chronologie des contrôles de transport système, créez un nouvel objet [**SystemMediaTransportControlsTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218746). Définissez les propriétés de l’objet afin de refléter l’état actuel de l’élément multimédia en cours de lecture. Appelez [**SystemMediaTransportControls.UpdateTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218760) pour que les contrôles mettent à jour la chronologie.

[!code-cs[UpdateTimelineProperties](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetUpdateTimelineProperties)]

-   Vous devez indiquer une valeur pour les propriétés [**StartTime**](https://msdn.microsoft.com/library/windows/apps/mt218751), [**EndTime**](https://msdn.microsoft.com/library/windows/apps/mt218747) et [**Position**](https://msdn.microsoft.com/library/windows/apps/mt218755) pour que les contrôles système affichent une chronologie relative à l’élément en cours de lecture.

-   [**MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749) et [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748) vous permettent de spécifier la plage de la chronologie dans laquelle l’utilisateur peut effectuer une recherche. Le scénario classique dans ce cas consiste à permettre aux fournisseurs de contenus d’inclure des pauses publicitaires dans leur contenu multimédia.

    Vous devez définir [**MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749) et [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748) afin de déclencher l’événement [**PositionChangeRequest**](https://msdn.microsoft.com/library/windows/apps/mt218755).

-   Il est recommandé de synchroniser les contrôles système avec la lecture multimédia en mettant à jour ces propriétés environ toutes les 5 secondes pendant la lecture et à nouveau lors de chaque changement d’état de la lecture, par exemple, lorsque cette dernière est mise en pause ou en cas de recherche d’une nouvelle position.

## <a name="respond-to-player-property-changes"></a>Répondre aux modifications des propriétés du lecteur

Il existe un ensemble de propriétés de contrôles de transport système qui se rapportent à l’état actuel du lecteur multimédia lui-même, plutôt qu’à l’état de l’élément multimédia en cours de lecture. Chacune de ces propriétés est mise en correspondance avec un événement qui est déclenché lorsque l’utilisateur ajuste le contrôle associé. Ces propriétés et événements sont les suivants :

| Propriété                                                                  | Événement                                                                                                   |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [**AutoRepeatMode**](https://msdn.microsoft.com/library/windows/apps/mt218753) | [**AutoRepeatModeChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218754) |
| [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/mt218756)     | [**PlaybackRateChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218757)     |
| [**ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt218758) | [**ShuffleEnabledChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218759) |

 
Pour gérer l’interaction de l’utilisateur avec un de ces contrôles, commencez par enregistrer un gestionnaire pour l’événement associé.

[!code-cs[RegisterPlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetRegisterPlaybackChangedHandler)]

Dans le gestionnaire de cet événement, commencez par vérifier que la valeur demandée est comprise dans une plage valide et attendue. Si tel est le cas, définissez la propriété correspondante sur [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), puis la propriété correspondante sur l’objet [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677).

[!code-cs[PlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetPlaybackChangedHandler)]

-   Afin de déclencher un de ces événements de propriété de lecteur, vous devez définir une valeur initiale pour cette propriété. Par exemple, [**PlaybackRateChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218757) n’est pas déclenché tant que vous n’avez pas défini au moins une fois une valeur pour la propriété [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/mt218756).

## <a name="use-the-system-media-transport-controls-for-background-audio"></a>Utiliser les contrôles de transport de média système pour le son en arrière-plan

Si vous n’utilisez pas l’intégration automatique des contrôles de transport de média système fournie par **MediaPlayer**, vous devez procéder à une intégration manuelle pour activer le son en arrière-plan. Au minimum, votre application doit activer les boutons de lecture et de pause. Pour ce faire, les éléments [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714) et [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713) sont définis sur true. L’application doit également gérer l’événement [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706). Si votre application ne satisfait pas ces exigences, la lecture audio s’arrête quand votre application est déplacée vers l’arrière-plan.

Les applications utilisant le nouveau modèle à processus unique pour l’audio d’arrière-plan doivent récupérer une instance de la classe [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677), en appelant [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708). Les applications utilisant le modèle à deuxprocessus existants pour l’audio d’arrière-plan doivent utiliser la classe [**BackgroundMediaPlayer.Current.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635) pour obtenir un accès aux contrôles de transport de média système à partir de leur processus d’arrière-plan.

Pour plus d’informations sur la lecture de l’audio dans l’arrière-plan, consultez la section [Contenu audio en arrière-plan](background-audio.md).

## <a name="related-topics"></a>Rubriques connexes
* [Lecture de contenu multimédia](media-playback.md)
* [Intégration avec les contrôles de transport de média système](integrate-with-systemmediatransportcontrols.md) 
* [Exemple de transport de média système](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls) 

 




