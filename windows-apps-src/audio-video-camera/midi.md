---
ms.assetid: 9146212C-8480-4C16-B74C-D7F08C7086AF
description: Cet article montre comment énumérer des périphériques MIDI et envoyer et recevoir des messages MIDI à partir d’une application Windows universelle.
title: MIDI
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 73806735401f53a73b1051f37c72119b45b574be
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318292"
---
# <a name="midi"></a>MIDI



Cet article montre comment énumérer des périphériques MIDI et envoyer et recevoir des messages MIDI à partir d’une application Windows universelle. Windows 10 prend en charge MIDI via USB (pilotes compatibles à la classe et plus propriétaires), MIDI via Bluetooth LE (Windows 10 Édition anniversaire et versions ultérieures) et par le biais disponible gratuitement les produits tiers, MIDI over Ethernet et routé MIDI.

## <a name="enumerate-midi-devices"></a>Énumérer des périphériques MIDI

Avant d’énumérer et d’utiliser des périphériques MIDI, ajoutez les espaces de noms suivants à votre projet.

[!code-cs[Using](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetUsing)]

Ajoutez un contrôle [**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) à votre page XAML qui permet à l’utilisateur de sélectionner l’un des périphériques d’entrée MIDI associés au système. Ajoutez-en un autre pour répertorier les périphériques de sortie MIDI.

[!code-xml[MidiListBoxes](./code/MIDIWin10/cs/MainPage.xaml#SnippetMidiListBoxes)]

La classe [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) de la méthode [**FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) sert à énumérer les nombreux types de périphériques différents qui sont reconnus par Windows. Pour indiquer que vous voulez seulement que la méthode recherche des périphériques d’entrée MIDI, utilisez la chaîne de sélecteur renvoyée par [**MidiInPort.GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.midi.midiinport.getdeviceselector). **FindAllAsync** renvoie une classe [**DeviceInformationCollection**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection) qui contient une classe **DeviceInformation** pour chaque périphérique d’entrée MIDI enregistré auprès du système. Si la collection renvoyée ne contient aucun élément, cela signifie qu’il n’y a aucun périphérique d’entrée MIDI disponible. Si la collection comporte des éléments, parcourez les objets **DeviceInformation** et ajoutez le nom de chaque périphérique au périphérique d’entrée MIDI **ListBox**.

[!code-cs[EnumerateMidiInputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiInputDevices)]

L’énumération des appareils de sortie MIDI fonctionne exactement comme l’énumération des appareils d’entrée, excepté le fait que vous devez spécifier la chaîne de sélecteur renvoyée par [**MidiOutPort.GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.midi.midioutport.getdeviceselector) lors de l’appel de **FindAllAsync**.

[!code-cs[EnumerateMidiOutputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiOutputDevices)]



## <a name="create-a-device-watcher-helper-class"></a>Créer une classe d’assistance d’observateur de périphériques

L’espace de noms [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) fournit la classe [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) qui permet de signaler à votre application si des périphériques ont été ajoutés au système ou supprimés de celui-ci ou si les informations d’un périphérique sont mises à jour. Étant donné que les applications compatibles MIDI sont généralement intéressées par les périphériques d’entrée et de sortie, cet exemple crée une classe d’assistance qui implémente le modèle **DeviceWatcher**, afin que le même code puisse être utilisé sur ces deux types de périphériques sans avoir besoin d’être dupliqué.

Ajoutez une nouvelle classe à votre projet utilisée comme observateur de périphériques. Dans cet exemple, la classe est nommée **MyMidiDeviceWatcher**. Le reste du code de cette section est utilisé pour implémenter la classe d’assistance.

Ajoutez des variables de membre à la classe :

-   Un objet [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) qui surveille les changements de périphérique.
-   Une chaîne de sélecteur de périphérique qui contiendra la chaîne de sélecteur de port d’entrée MIDI pour une instance et la chaîne de sélecteur de port de sortie MIDI pour une autre instance.
-   Un contrôle [**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) qui comportera les noms des périphériques disponibles.
-   Une classe [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) requise pour mettre à jour l’interface utilisateur à partir d’un thread autre que le thread d’interface utilisateur.

[!code-cs[WatcherVariables](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherVariables)]

Ajoutez une propriété [**DeviceInformationCollection**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection) utilisée pour accéder à la liste actuelle des périphériques depuis l’extérieur de la classe d’assistance.

[!code-cs[DeclareDeviceInformationCollection](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetDeclareDeviceInformationCollection)]

Dans le constructeur de classe, l’appelant transmet la chaîne de sélecteur de périphérique MIDI, l’élément **ListBox** nécessaire pour répertorier les périphériques et l’élément **Dispatcher** requis pour la mise à jour de l’interface utilisateur.

Appelez [**DeviceInformation.CreateWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher) pour créer une nouvelle instance de la classe **DeviceWatcher**, en transmettant la chaîne de sélecteur de périphérique MIDI.

Enregistrez des gestionnaires pour les gestionnaires d’événements de l’observateur.

[!code-cs[WatcherConstructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherConstructor)]

L’élément **DeviceWatcher** présente les événements suivants :

-   [**Ajouté** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added) -déclenché lorsqu’un nouveau périphérique est ajouté au système.
-   [**Supprimé** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed) -déclenché lorsqu’un appareil est supprimé à partir du système.
-   [**Mise à jour** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) -déclenché lorsque les informations associées à un périphérique existant sont mis à jour.
-   [**EnumerationCompleted** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted) -déclenché lorsque l’Observateur a terminé son énumération du type de périphérique demandé.

Dans le gestionnaire d’événements, pour chacun de ces événements, une méthode d’assistance **UpdateDevices** est appelée pour mettre à jour l’élément **ListBox** en tenant compte de la liste actuelle des périphériques. Dans la mesure où **UpdateDevices** met à jour les éléments d’interface utilisateur et que ces gestionnaires d’événements ne sont pas appelés sur le thread d’interface utilisateur, chaque appel doit être encapsulé dans un appel à [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync), ce qui entraîne l’exécution du code sur le thread d’interface utilisateur.

[!code-cs[WatcherEventHandlers](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherEventHandlers)]

La méthode d’assistance **UpdateDevices** appelle [**DeviceInformation.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) et met à jour l’élément **ListBox** en tenant compte des noms des périphériques renvoyés comme décrit précédemment dans cet article.

[!code-cs[WatcherUpdateDevices](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherUpdateDevices)]

Ajoutez des méthodes pour démarrer l’observateur, à l’aide de la méthode [**Start**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.start) de l’objet **DeviceWatcher** et pour arrêter l’observateur à l’aide de la méthode [**Stop**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop).

[!code-cs[WatcherStopStart](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherStopStart)]

Fournissez un destructeur pour annuler l’enregistrement des gestionnaires d’événements de l’observateur et définir l’observateur de périphériques sur null.

[!code-cs[WatcherDestructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherDestructor)]

## <a name="create-midi-ports-to-send-and-receive-messages"></a>Créer des ports MIDI pour envoyer et recevoir des messages

Dans le code-behind de votre page, déclarez des variables de membre destinées à contenir deux instances de la classe d’assistance **MyMidiDeviceWatcher**, une pour les périphériques d’entrée et une autre pour les périphériques de sortie.

[!code-cs[DeclareDeviceWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatchers)]

Créez une instance des classes d’assistance de l’observateur en transmettant la chaîne de sélecteur de périphérique, le **ListBox** à remplir et l’objet **CoreDispatcher** accessible par le biais de la propriété **Dispatcher** de la page. Appelez ensuite la méthode pour démarrer l’élément **DeviceWatcher** de chaque objet.

Peu après avoir démarré chaque **DeviceWatcher**, elle terminera l’énumération des périphériques actuels connectés au système et générera son événement [**EnumerationCompleted**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted), qui entraînera la mise à jour de chaque **ListBox** avec les périphériques MIDI en cours.

[!code-cs[StartWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetStartWatchers)]

La sélection par l’utilisateur d’un élément dans l’élément **ListBox** de l’entrée MIDI, déclenche l’événement [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged). Dans le gestionnaire, pour cet événement, accédez à la propriété **DeviceInformationCollection** de la classe d’assistance pour obtenir la liste des périphériques actuels. Si la liste comporte des entrées, sélectionnez l’objet **DeviceInformation** avec l’index correspondant à la propriété [**SelectedIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) du contrôle **ListBox**.

Créez l’objet [**MidiInPort**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.MidiInPort) représentant le périphérique d’entrée sélectionné en appelant [**MidiInPort.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.midi.midiinport.fromidasync) et en transmettant la propriété [**Id**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) du périphérique sélectionné.

Enregistrez un gestionnaire pour l’événement [**MessageReceived**](https://docs.microsoft.com/uwp/api/windows.devices.midi.midiinport.messagereceived), qui est déclenché chaque fois qu’un message MIDI est reçu par le biais du périphérique spécifié.

[!code-cs[DeclareMidiPorts](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareMidiPorts)]

[!code-cs[InPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetInPortSelectionChanged)]

Lorsque le gestionnaire **MessageReceived** est appelé, le message est contenu dans la propriété [**Message**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.MidiMessageReceivedEventArgs) de la classe **MidiMessageReceivedEventArgs**. La propriété [**Type**](https://docs.microsoft.com/uwp/api/windows.devices.midi.imidimessage.type) de l’objet du message est une valeur provenant de l’énumération [**MidiMessageType**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.MidiMessageType) indiquant le type de message reçu. Les données du message dépendent de son type. Cet exemple vérifie si le message est une note figurant sur un message et, si tel est le cas, génère le canal midi, la note et la vitesse du message.

[!code-cs[MessageReceived](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetMessageReceived)]

Le gestionnaire [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) de l’élément **ListBox** du périphérique de sortie fonctionne comme le gestionnaire des périphériques d’entrée, excepté qu’aucun gestionnaire d’événements n’est enregistré.

[!code-cs[OutPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetOutPortSelectionChanged)]

Une fois le périphérique de sortie créé, vous pouvez envoyer un message en créant un nouvel élément [**IMidiMessage**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.IMidiMessage) pour le type de message que vous voulez envoyer. Dans cet exemple, le message est une classe [**NoteOnMessage**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.MidiNoteOnMessage). La méthode [**SendMessage**](https://docs.microsoft.com/uwp/api/windows.devices.midi.imidioutport.sendmessage) de l’objet [**IMidiOutPort**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.IMidiOutPort) est appelée pour envoyer le message.

[!code-cs[SendMessage](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetSendMessage)]

Lorsque votre application est désactivée, veillez à nettoyer les ressources de vos applications. Annulez l’enregistrement de vos gestionnaires d’événements et définissez les objets des ports MIDI d’entrée et de sortie sur null. Arrêtez les observateurs de périphériques et affectez-leur la valeur null.

[!code-cs[CleanUp](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetCleanUp)]

## <a name="using-the-built-in-windows-general-midi-synth"></a>Utilisation du synthétiseur General MIDI Windows intégré

Lors de l’énumération des périphériques de sortie MIDI à l’aide de la technique décrite ci-dessus, votre application détecte un périphérique MIDI appelé « Synthé. de table de sons Microsoft GS ». Il s’agit d’un synthétiseur General MIDI intégré que vous pouvez utiliser à partir de votre application. Toutefois, toute tentative de création d’un port de sortie MIDI pour ce périphérique échouera, sauf si vous avez inclus l’extension du Kit de développement logiciel (SDK) pour le synthétiseur intégré dans votre projet.

**Pour inclure l’extension de SDK de synthétiseur MIDI général dans votre projet d’application**

1.  Dans l’**Explorateur de solutions**, sous votre projet, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence…** .
2.  Développez le nœud **Windows universel**.
3.  Sélectionnez **Extensions**.
4.  Dans la liste des extensions, sélectionnez **DLS General MIDI Microsoft pour les applications Windows universelles**.
    > [!NOTE] 
    > S’il existe plusieurs versions de l’extension, veillez à sélectionner la version qui correspond à la cible de votre application. Pour connaître la version du Kit de développement logiciel (SDK) ciblée par votre application, cliquez sur l’onglet **Application** des propriétés du projet.

 

 




