---
author: drewbatgit
ms.assetid: EFCF84D0-2F4C-454D-97DA-249E9EAA806C
description: "La classe SystemMediaTransportControls permet à votre application d’utiliser les contrôles de transport de média système intégrés à Windows et de mettre à jour les métadonnées affichées par les contrôles concernant le média lu actuellement par votre application."
title: "Contrôles de transport de média système"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 5a94ce4112f7662d3fe9bf3c8a7d3f60b1569931

---

# Contrôles de transport de média système

\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


La classe [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) permet à votre application d’utiliser les contrôles de transport de média système intégrés à Windows et de mettre à jour les métadonnées affichées par les contrôles concernant le média lu actuellement par votre application.

Les contrôles de transport de média système sont différents des contrôles de transport de l’objet [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926). Les contrôles de transport système sont les contrôles qui s’affichent quand l’utilisateur appuie sur une touche de média matériel, telle que la commande de volume d’un casque ou les boutons de média d’un clavier. Si l’utilisateur appuie sur la touche Pause d’un clavier et que votre application prend en charge la classe [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677), votre application reçoit une notification et vous pouvez effectuer l’action appropriée.

Votre application peut également mettre à jour les informations sur un média, telles que le titre de la chanson et l’image miniature affichée par [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677).

**Remarque**  
L’[exemple de contrôles de transport de média système UWP](http://go.microsoft.com/fwlink/?LinkId=619488) implémente le code décrit dans cette vue d’ensemble. Vous pouvez télécharger l’exemple pour voir le code en contexte ou pour vous en servir comme point de départ pour votre propre application.

## Installer les contrôles de transport

Dans le fichier XAML de la page, définissez une classe [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) qui sera contrôlée par les contrôles de transport de média système. Les événements [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) et [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/br227394) sont utilisés pour mettre à jour les contrôles de transport de média système et seront abordés plus loin dans cet article.

[!code-xml[MediaElementSystemMediaTransportControls](./code/SMTCWin10/cs/MainPage.xaml#SnippetMediaElementSystemMediaTransportControls)]

Ajoutez un bouton au fichier XAML qui permet à l’utilisateur de sélectionner un fichier à lire.

[!code-xml[OpenButton](./code/SMTCWin10/cs/MainPage.xaml#SnippetOpenButton)]

Dans la page code-behind, ajoutez des directives using pour les espaces de noms suivants.

[!code-cs[Espace de noms](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetNamespace)]

Ajoutez un gestionnaire de clics sur le bouton qui utilise une classe [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) pour permettre à l’utilisateur de sélectionner un fichier, puis appelez [**SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338) afin de l’activer pour l’élément **MediaElement**.

[!code-cs[OpenMediaFile](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetOpenMediaFile)]

Pour obtenir une instance de la classe [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677), appelez [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708).

Activez les boutons utilisés par votre application en définissant la propriété « is enabled » correspondante de l’objet **SystemMediaTransportControls**, telle que [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714), [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713), [**IsNextEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278712) et [**IsPreviousEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278715). Voir la documentation de référence de **SystemMediaTransportControls** pour obtenir la liste complète des contrôles disponibles.

Enregistrez un gestionnaire pour l’événement [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) afin de recevoir des notifications dès que l’utilisateur appuie sur un bouton.

[!code-cs[SystemMediaTransportControlsSetup](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsSetup)]

## Gérer les pressions sur les boutons de contrôles de transport de média système

L’événement [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) est déclenché par les contrôles de transport système quand l’utilisateur appuie sur l’un des boutons activés. La propriété [**Button**](https://msdn.microsoft.com/library/windows/apps/dn278685) de la classe [**SystemMediaTransportControlsButtonPressedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn278683) transmise au gestionnaire d’événements est un membre de l’énumération [**SystemMediaTransportControlsButton**](https://msdn.microsoft.com/library/windows/apps/dn278681) qui indique de quel bouton activé il s’agit.

Pour mettre à jour les objets du thread d’interface utilisateur à partir du gestionnaire d’événements [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706), tel qu’un objet [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), vous devez marshaler les appels via [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211). Cela vient du fait que le gestionnaire d’événements **ButtonPressed** n’est pas appelé à partir du thread d’interface utilisateur. Par conséquent, une exception est générée si vous tentez de modifier directement l’interface utilisateur.

[!code-cs[SystemMediaTransportControlsButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsButtonPressed)]

## Mettre à jour les contrôles de transport de média système en tenant compte de l’état actuel du média

Vous devez avertir la classe [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) en cas de modification de l’état du média afin que le système puisse mettre à jour les contrôles de manière à refléter l’état actuel. Pour ce faire, définissez la propriété [**PlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278719) sur la valeur [**MediaPlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278665) appropriée depuis l’événement [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) de la classe [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), qui est déclenché dès que l’état du média change.

[!code-cs[SystemMediaTransportControlsStateChange](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsStateChange)]

## Mettre à jour les contrôles de transport de média système en tenant compte des informations relatives au média et des miniatures

Utilisez la classe [**SystemMediaTransportControlsDisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278686) pour mettre à jour les informations du média affichées par les contrôles de transport, telles que le titre de la chanson ou la pochette de l’album pour l’élément multimédia en cours de lecture. Obtenez une instance de cette classe avec la propriété [**SystemMediaTransportControls.DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707). Pour les scénarios classiques, le mode de transmission des métadonnées recommandé consiste à appeler [**CopyFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn278694) en transmettant le fichier multimédia en cours de lecture. L’outil de mise à jour de l’affichage extrait automatiquement les métadonnées et l’image miniature du fichier.

Appelez [**Update**](https://msdn.microsoft.com/library/windows/apps/dn278701) pour que les contrôles de transport de média système mettent à jour son interface utilisateur en tenant compte des nouvelles métadonnées et des miniatures.

[!code-cs[SystemMediaTransportControlsUpdater](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsUpdater)]

Si votre scénario l’exige, vous pouvez mettre à jour manuellement les métadonnées affichées par les contrôles de transport de média système en définissant les valeurs des objets [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/dn278696), [**ImageProperties**](https://msdn.microsoft.com/library/windows/apps/dn278695) ou [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/dn278702) exposés par la classe [**DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707).

[!code-cs[SystemMediaTransportControlsUpdaterManual](./code/SMTCWin10/cs/MainPage.xaml.cs#SystemMediaTransportControlsUpdaterManual)]

## Mettre à jour les propriétés de chronologie des contrôles de transport de média système

Les contrôles de transport système affichent des informations sur la chronologie de l’élément multimédia en cours de lecture, y compris la position de lecture actuelle, son heure de début et son heure de fin. Pour mettre à jour les propriétés de chronologie des contrôles de transport système, créez un nouvel objet [**SystemMediaTransportControlsTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218746). Définissez les propriétés de l’objet afin de refléter l’état actuel de l’élément multimédia en cours de lecture. Appelez [**SystemMediaTransportControls.UpdateTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218760) pour que les contrôles mettent à jour la chronologie.

[!code-cs[UpdateTimelineProperties](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetUpdateTimelineProperties)]

-   Vous devez indiquer une valeur pour les propriétés [**StartTime**](https://msdn.microsoft.com/library/windows/apps/mt218751), [**EndTime**](https://msdn.microsoft.com/library/windows/apps/mt218747) et [**Position**](https://msdn.microsoft.com/library/windows/apps/mt218755) pour que les contrôles système affichent une chronologie relative à l’élément en cours de lecture.

-   [**MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749) et [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748) vous permettent de spécifier la plage de la chronologie dans laquelle l’utilisateur peut effectuer une recherche. Le scénario classique dans ce cas consiste à permettre aux fournisseurs de contenus d’inclure des pauses publicitaires dans leur contenu multimédia.

    Vous devez définir [**MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749) et [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748) afin de déclencher l’événement [**PositionChangeRequest**](https://msdn.microsoft.com/library/windows/apps/mt218755).

-   Il est recommandé de synchroniser les contrôles système avec la lecture multimédia en mettant à jour ces propriétés environ toutes les 5 secondes pendant la lecture et à nouveau lors de chaque changement d’état de la lecture, par exemple, lorsque cette dernière est mise en pause ou en cas de recherche d’une nouvelle position.

## Répondre aux modifications des propriétés du lecteur

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

## Utiliser les contrôles de transport de média système pour le son en arrière-plan

Pour utiliser les contrôles de transport de média système pour le son en arrière-plan, vous devez activer les boutons de lecture et de pause en définissant [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714) et [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713) sur true. L’application doit également gérer l’événement [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706).

Pour obtenir une instance de [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) depuis la tâche en arrière-plan de votre application, vous devez utiliser [**BackgroundMediaPlayer.Current.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635) à la place de [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708), qui ne peut être utilisée qu’à partir de l’application au premier plan.

Pour plus d’informations sur la lecture audio en arrière-plan, voir [Audio d’arrière-plan](background-audio.md).

 

 







<!--HONumber=Jun16_HO4-->


