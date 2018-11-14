---
author: drewbatgit
ms.assetid: 9146212C-8480-4C16-B74C-D7F08C7086AF
description: Cet article montre comment énumérer des périphériques MIDI et envoyer et recevoir des messages MIDI à partir d’une application Windows universelle.
title: MIDI
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 36d1e4afd620b871d4273699aea5c02cc9faec80
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6649793"
---
# <a name="midi"></a>MIDI



Cet article vous montre comment énumérer des périphériques MIDI (Musical Instrument Digital Interface) et envoyer et recevoir des messages MIDI à partir d’une application Windows universelle. Windows 10 prend en charge MIDI via USB (classe conforme et plus propriétaires de pilotes) MIDI via Bluetooth LE (Édition anniversaire Windows 10 et versions ultérieures) et par le biais de disponibles gratuitement des produits tiers, MIDI via une connexion Ethernet et routé MIDI.

## <a name="enumerate-midi-devices"></a>Énumérer des périphériques MIDI

Avant d’énumérer et d’utiliser des périphériques MIDI, ajoutez les espaces de noms suivants à votre projet.

[!code-cs[Using](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetUsing)]

Ajoutez un contrôle [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868) à votre page XAML qui permet à l’utilisateur de sélectionner l’un des périphériques d’entrée MIDI associés au système. Ajoutez-en un autre pour répertorier les périphériques de sortie MIDI.

[!code-xml[MidiListBoxes](./code/MIDIWin10/cs/MainPage.xaml#SnippetMidiListBoxes)]

La classe [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) de la méthode [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) sert à énumérer les nombreux types de périphériques différents qui sont reconnus par Windows. Pour indiquer que vous voulez seulement que la méthode recherche des périphériques d’entrée MIDI, utilisez la chaîne de sélecteur renvoyée par [**MidiInPort.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn894779). **FindAllAsync** renvoie une classe [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) qui contient une classe **DeviceInformation** pour chaque périphérique d’entrée MIDI enregistré auprès du système. Si la collection renvoyée ne contient aucun élément, cela signifie qu’il n’y a aucun périphérique d’entrée MIDI disponible. Si la collection comporte des éléments, parcourez les objets **DeviceInformation** et ajoutez le nom de chaque périphérique au périphérique d’entrée MIDI **ListBox**.

[!code-cs[EnumerateMidiInputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiInputDevices)]

L’énumération des appareils de sortie MIDI fonctionne exactement comme l’énumération des appareils d’entrée, excepté le fait que vous devez spécifier la chaîne de sélecteur renvoyée par [**MidiOutPort.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn894845) lors de l’appel de **FindAllAsync**.

[!code-cs[EnumerateMidiOutputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiOutputDevices)]



## <a name="create-a-device-watcher-helper-class"></a>Créer une classe d’assistance d’observateur de périphériques

L’espace de noms [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459) fournit la classe [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446) qui permet de signaler à votre application si des périphériques ont été ajoutés au système ou supprimés de celui-ci ou si les informations d’un périphérique sont mises à jour. Étant donné que les applications compatibles MIDI sont généralement intéressées par les périphériques d’entrée et de sortie, cet exemple crée une classe d’assistance qui implémente le modèle **DeviceWatcher**, afin que le même code puisse être utilisé sur ces deux types de périphériques sans avoir besoin d’être dupliqué.

Ajoutez une nouvelle classe à votre projet utilisée comme observateur de périphériques. Dans cet exemple, la classe est nommée **MyMidiDeviceWatcher**. Le reste du code de cette section est utilisé pour implémenter la classe d’assistance.

Ajoutez des variables de membre à la classe :

-   Un objet [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446) qui surveille les changements de périphérique.
-   Une chaîne de sélecteur de périphérique qui contiendra la chaîne de sélecteur de port d’entrée MIDI pour une instance et la chaîne de sélecteur de port de sortie MIDI pour une autre instance.
-   Un contrôle [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868) qui comportera les noms des périphériques disponibles.
-   Une classe [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) requise pour mettre à jour l’interface utilisateur à partir d’un thread autre que le thread d’interface utilisateur.

[!code-cs[WatcherVariables](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherVariables)]

Ajoutez une propriété [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) utilisée pour accéder à la liste actuelle des périphériques depuis l’extérieur de la classe d’assistance.

[!code-cs[DeclareDeviceInformationCollection](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetDeclareDeviceInformationCollection)]

Dans le constructeur de classe, l’appelant transmet la chaîne de sélecteur de périphérique MIDI, l’élément **ListBox** nécessaire pour répertorier les périphériques et l’élément **Dispatcher** requis pour la mise à jour de l’interface utilisateur.

Appelez [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/br225427) pour créer une nouvelle instance de la classe **DeviceWatcher**, en transmettant la chaîne de sélecteur de périphérique MIDI.

Enregistrez des gestionnaires pour les gestionnaires d’événements de l’observateur.

[!code-cs[WatcherConstructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherConstructor)]

L’élément **DeviceWatcher** présente les événements suivants:

-   [**Added**](https://msdn.microsoft.com/library/windows/apps/br225450) - Déclenché lorsqu’un nouveau périphérique est ajouté au système.
-   [**Removed**](https://msdn.microsoft.com/library/windows/apps/br225453) - Déclenché lorsqu’un périphérique est supprimé du système.
-   [**Updated**](https://msdn.microsoft.com/library/windows/apps/br225458) - Déclenché lorsque les informations associées à un périphérique existant sont mises à jour.
-   [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451) - Déclenché lorsque l’observateur a terminé son énumération du type de périphérique demandé.

Dans le gestionnaire d’événements, pour chacun de ces événements, une méthode d’assistance **UpdateDevices** est appelée pour mettre à jour l’élément **ListBox** en tenant compte de la liste actuelle des périphériques. Dans la mesure où **UpdateDevices** met à jour les éléments d’interface utilisateur et que ces gestionnaires d’événements ne sont pas appelés sur le thread d’interface utilisateur, chaque appel doit être encapsulé dans un appel à [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317), ce qui entraîne l’exécution du code sur le thread d’interface utilisateur.

[!code-cs[WatcherEventHandlers](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherEventHandlers)]

La méthode d’assistance **UpdateDevices** appelle [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) et met à jour l’élément **ListBox** en tenant compte des noms des périphériques renvoyés comme décrit précédemment dans cet article.

[!code-cs[WatcherUpdateDevices](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherUpdateDevices)]

Ajoutez des méthodes pour démarrer l’observateur, à l’aide de la méthode [**Start**](https://msdn.microsoft.com/library/windows/apps/br225454) de l’objet **DeviceWatcher** et pour arrêter l’observateur à l’aide de la méthode [**Stop**](https://msdn.microsoft.com/library/windows/apps/br225456).

[!code-cs[WatcherStopStart](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherStopStart)]

Fournissez un destructeur pour annuler l’enregistrement des gestionnaires d’événements de l’observateur et définir l’observateur de périphériques sur null.

[!code-cs[WatcherDestructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherDestructor)]

## <a name="create-midi-ports-to-send-and-receive-messages"></a>Créer des ports MIDI pour envoyer et recevoir des messages

Dans le code-behind de votre page, déclarez des variables de membre destinées à contenir deux instances de la classe d’assistance **MyMidiDeviceWatcher**, une pour les périphériques d’entrée et une autre pour les périphériques de sortie.

[!code-cs[DeclareDeviceWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatchers)]

Créez une instance des classes d’assistance de l’observateur en transmettant la chaîne de sélecteur de périphérique, le **ListBox** à remplir et l’objet **CoreDispatcher** accessible par le biais de la propriété **Dispatcher** de la page. Appelez ensuite la méthode pour démarrer l’élément **DeviceWatcher** de chaque objet.

Peu après avoir démarré chaque **DeviceWatcher**, elle terminera l’énumération des périphériques actuels connectés au système et générera son événement [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451), qui entraînera la mise à jour de chaque **ListBox** avec les périphériques MIDI en cours.

[!code-cs[StartWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetStartWatchers)]

La sélection par l’utilisateur d’un élément dans l’élément **ListBox** de l’entrée MIDI, déclenche l’événement [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776). Dans le gestionnaire, pour cet événement, accédez à la propriété **DeviceInformationCollection** de la classe d’assistance pour obtenir la liste des périphériques actuels. Si la liste comporte des entrées, sélectionnez l’objet **DeviceInformation** avec l’index correspondant à la propriété [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/br209768) du contrôle **ListBox**.

Créez l’objet [**MidiInPort**](https://msdn.microsoft.com/library/windows/apps/dn894770) représentant le périphérique d’entrée sélectionné en appelant [**MidiInPort.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn894776) et en transmettant la propriété [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437) du périphérique sélectionné.

Enregistrez un gestionnaire pour l’événement [**MessageReceived**](https://msdn.microsoft.com/library/windows/apps/dn894781), qui est déclenché chaque fois qu’un message MIDI est reçu par le biais du périphérique spécifié.

[!code-cs[DeclareMidiPorts](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareMidiPorts)]

[!code-cs[InPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetInPortSelectionChanged)]

Lorsque le gestionnaire **MessageReceived** est appelé, le message est contenu dans la propriété [**Message**](https://msdn.microsoft.com/library/windows/apps/dn894783) de la classe **MidiMessageReceivedEventArgs**. La propriété [**Type**](https://msdn.microsoft.com/library/windows/apps/dn894726) de l’objet du message est une valeur provenant de l’énumération [**MidiMessageType**](https://msdn.microsoft.com/library/windows/apps/dn894786) indiquant le type de message reçu. Les données du message dépendent de son type. Cet exemple vérifie si le message est une note figurant sur un message et, si tel est le cas, génère le canal midi, la note et la vitesse du message.

[!code-cs[MessageReceived](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetMessageReceived)]

Le gestionnaire [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) de l’élément **ListBox** du périphérique de sortie fonctionne comme le gestionnaire des périphériques d’entrée, excepté qu’aucun gestionnaire d’événements n’est enregistré.

[!code-cs[OutPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetOutPortSelectionChanged)]

Une fois le périphérique de sortie créé, vous pouvez envoyer un message en créant un nouvel élément [**IMidiMessage**](https://msdn.microsoft.com/library/windows/apps/dn911508) pour le type de message que vous voulez envoyer. Dans cet exemple, le message est une classe [**NoteOnMessage**](https://msdn.microsoft.com/library/windows/apps/dn894817). La méthode [**SendMessage**](https://msdn.microsoft.com/library/windows/apps/dn894730) de l’objet [**IMidiOutPort**](https://msdn.microsoft.com/library/windows/apps/dn894727) est appelée pour envoyer le message.

[!code-cs[SendMessage](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetSendMessage)]

Lorsque votre application est désactivée, veillez à nettoyer les ressources de vos applications. Annulez l’enregistrement de vos gestionnaires d’événements et définissez les objets des ports MIDI d’entrée et de sortie sur null. Arrêtez les observateurs de périphériques et affectez-leur la valeur null.

[!code-cs[CleanUp](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetCleanUp)]

## <a name="using-the-built-in-windows-general-midi-synth"></a>Utilisation du synthétiseur General MIDI Windows intégré

Lors de l’énumération des périphériques de sortie MIDI à l’aide de la technique décrite ci-dessus, votre application détecte un périphérique MIDI appelé «Synthé. de table de sons Microsoft GS». Il s’agit d’un synthétiseur General MIDI intégré que vous pouvez utiliser à partir de votre application. Toutefois, toute tentative de création d’un port de sortie MIDI pour ce périphérique échouera, sauf si vous avez inclus l’extension du Kit de développement logiciel (SDK) pour le synthétiseur intégré dans votre projet.

**Pour inclure l’extension du Kit de développement logiciel (SDK) pour le synthétiseur General MIDI dans votre projet d’application**

1.  Dans l’**Explorateur de solutions**, sous votre projet, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence…**.
2.  Développez le nœud **Windows universel**.
3.  Sélectionnez **Extensions**.
4.  Dans la liste des extensions, sélectionnez **DLS General MIDI Microsoft pour les applications Windows universelles**.
    > [!NOTE] 
    > S’il existe plusieurs versions de l’extension, veillez à sélectionner la version qui correspond à la cible de votre application. Pour connaître la version du Kit de développement logiciel (SDK) ciblée par votre application, cliquez sur l’onglet **Application** des propriétés du projet.

 

 




