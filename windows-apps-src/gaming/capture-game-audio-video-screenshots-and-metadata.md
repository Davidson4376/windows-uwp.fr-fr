---
author: drewbatgit
ms.assetid: ''
description: Décrit comment enregistrer l’audio, la vidéo et les métadonnées de jeu dans une application UWP.
title: Capturer l’audio, la vidéo, les captures d’écran et les métadonnées de jeu
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, jeu, capturer, audio, vidéo, métadonnées
ms.localizationpriority: medium
ms.openlocfilehash: 6c1641cb4c50b86d7f678bf18fa85ad0215b4b15
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7423025"
---
# <a name="capture-game-audio-video-screenshots-and-metadata"></a>Capturer l’audio, la vidéo, les captures d’écran et les métadonnées de jeu
Cet article explique comment capturer l’audio, la vidéo et les captures d’écran de jeu et soumettre les métadonnées afin que le système les incorpore dans les médias capturés et de diffusion. Ainsi, les applications et notamment la vôtre, sont en mesure de créer des expériences dynamiques qui sont synchronisées avec les événements de jeu. 

Il existe deux façons de capturer un jeu dans une application UWP. L’utilisateur peut démarrer la capture à l’aide de l’interface utilisateur système intégrée. Les médias capturés à l’aide de cette technique sont ingérés par l’écosystème de jeu Microsoft. Il peuvent être affichés et partagés par le biais d’expériences internes telles que l’application XBox; ils ne sont pas directement accessibles à votre application ou aux utilisateurs. Les premières sections de cet article vous montrent comment activer et désactiver la capture d’application implémentée par le système et comment recevoir des notifications lorsqu’elle démarre ou s’arrête.

L’autre façon de capturer des médias consiste à utiliser les API de l’espace de noms **[Windows.Media.AppRecording](https://docs.microsoft.com/uwp/api/windows.media.apprecording)**. Si la capture est activée sur l’appareil, votre application peut démarrer la capture de jeu et, après un certain laps de temps, vous pouvez l’arrêter et les médias sont alors écrits dans un fichier. Si l’utilisateur a activé la capture historique, vous pouvez également enregistrer le jeu qui a déjà eu lieu en spécifiant une heure de début dans le passé et la durée d’enregistrement. Ces deux techniques produisent un fichier vidéo auquel votre application peut accéder et, selon l’emplacement où vous enregistrez les fichiers, auquel l’utilisateur a également accès. Les sections centrales de cet article vous guident tout au long de l’implémentation de ces scénarios.

L’espace de noms **[Windows.Media.Capture](https://docs.microsoft.com/uwp/api/windows.media.capture)** fournit des API permettant de créer des métadonnées qui décrivent le jeu en cours de capture ou de diffusion. Elles peuvent inclure du texte ou des valeurs numériques, avec une étiquette de texte identifiant chaque élément de données. Les métadonnées peuvent représenter un «événement» qui survient à un moment précis, par exemple lorsque l’utilisateur termine un tour de circuit dans un jeu de course, ou un «état» qui persiste pendant un intervalle de temps, par exemple la carte de jeu actuelle de l’utilisateur. Les métadonnées sont écrites dans un cache qui est alloué et géré pour votre application par le système. Les métadonnées sont incorporées aux flux de diffusion et aux fichiers vidéo capturés, y compris les techniques de capture système intégrée et de capture d’application personnalisée. Les dernières sections de cet article vous montrent comment écrire les métadonnées de jeu.

> [!NOTE] 
> Dans la mesure où les métadonnées de jeu peuvent être incorporées dans des fichiers multimédias susceptibles d’être partagés sur le réseau, hors du contrôle de l’utilisateur, vous ne devez pas inclure d’informations d’identification personnelle ou autres données potentiellement sensibles dans les métadonnées.


## <a name="enable-and-disable-system-app-capture"></a>Activer et désactiver la capture d’application système
La capture d’application système est lancée par l’utilisateur à l’aide de l’interface utilisateur système intégrée. Les fichiers sont ingérés par l’écosystème de jeu Windows et ne sont pas accessibles à votre application ni à l’utilisateur, excepté pour des expériences internes telles que l’application XBox. Votre application peut désactiver et activer la capture d’application initiée par le système, ce qui vous permet d’empêcher l’utilisateur de capturer certains contenus ou jeux. 

Pour activer ou désactiver la capture d’application système, il vous suffit d’appeler la méthode statique **[AppCapture.SetAllowedAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.setallowedasync)** et de passer la valeur **false** pour désactiver la capture ou **true** pour activer l’activer.

[!code-cpp[SetAppCaptureAllowed](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetSetAppCaptureAllowed)]


## <a name="receive-notifications-when-system-app-capture-starts-and-stops"></a>Recevoir des notifications lors du démarrage et de l’arrêt de la capture d’application système
Pour recevoir une notification lorsque la capture d’application système démarre ou se termine, commencez par obtenir une instance de la classe **[AppCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture)** en appelant la méthode de fabrique **[GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.GetForCurrentView)**. Ensuite, inscrivez un gestionnaire pour l’événement **[CapturingChanged](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.CapturingChanged)**.

[!code-cpp[RegisterCapturingChanged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRegisterCapturingChanged)]

Dans le gestionnaire de l’événement **CapturingChanged**, vous pouvez vérifier les propriétés **[IsCapturingAudio](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.IsCapturingAudio)** et **[IsCapturingVideo](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.IsCapturingVideo)** afin de déterminer si l’audio ou la vidéo sont en cours de capture respectivement. Vous pouvez également mettre à jour l’interface utilisateur de votre application pour indiquer l’état actuel de la capture.

[!code-cpp[OnCapturingChanged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnCapturingChanged)]

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>Ajouter les extensions de bureau Windows pour UWP à votre application
Les API utilisées pour l’enregistrement audio et vidéo et pour effectuer les captures d’écran directement à partir de votre application, disponibles dans l’espace de noms **[Windows.Media.AppRecording](https://docs.microsoft.com/uwp/api/windows.media.apprecording)**, ne sont pas incluses dans le contrat API universelle. Pour accéder à ces API, vous devez ajouter une référence aux extensions de bureau Windows pour UWP à votre application en procédant comme suit.

1. Dans VisualStudio, dans l’**Explorateur de solutions**, développez votre projet UWP, cliquez avec le bouton droit sur **Références** et sélectionnez **Ajouter une référence...**. 
2. Développez le nœud **Windows universel** et sélectionnez **Extensions**.
3. Dans la liste d’extensions, cochez la case en regard de l’entrée **Extensions de bureau Windows pour UWP** qui correspond à la version cible de votre projet. Pour les fonctionnalités de diffusion d’application, il doit s’agir au minimum de la version1709.
4. Cliquez sur **OK**.

## <a name="get-an-instance-of-apprecordingmanager"></a>Obtenir une instance de AppRecordingManager
La classe **[AppRecordingManager](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager)** est l’API centrale que vous allez utiliser pour gérer l’enregistrement de l’application. Obtenez une instance de cette classe en appelant la méthode de fabrique **[GetDefault](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.GetDefault)**. Avant d’utiliser les API de l’espace de noms **Windows.Media.AppRecording**, vous devez vérifier leur présence sur l’appareil actuel. Les API ne sont pas disponibles sur les appareils exécutant une version du système d’exploitation antérieure à Windows10, version1709. Plutôt que de rechercher une version spécifique du système d’exploitation, utilisez la méthode **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** pour rechercher *Windows.Media.AppBroadcasting.AppRecordingContract* version1.0. Si ce contrat est présent, les API d’enregistrement sont disponibles sur l’appareil. L’exemple de code utilisé dans cet article vérifie la présence des API à une reprise, puis vérifie si la valeur de **AppRecordingManager** est null avant les opérations suivantes.

[!code-cpp[GetAppRecordingManager](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetGetAppRecordingManager)]

## <a name="determine-if-your-app-can-currently-record"></a>Déterminer si votre application peut enregistrer actuellement
Il existe plusieurs raisons pour lesquelles votre application peut ne pas être en mesure de capturer l’audio ou la vidéo actuellement. Ceci est notamment le cas si l’appareil utilisé ne satisfait pas la configuration matérielle requise pour l’enregistrement ou si une autre application est en cours de diffusion. Avant de lancer un enregistrement, vous pouvez vérifier si votre application est en mesure d’effectuer l’enregistrement actuellement. Appelez la méthode **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** de l’objet **AppRecordingManager**, puis vérifiez la propriété **[CanRecord](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecord)** de l’objet **[AppRecordingStatus](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus)** renvoyé. Si **CanRecord** renvoie la valeur **false**, ce qui signifie que votre application ne peut pas enregistrer actuellement, vous pouvez vérifier la propriété **[Details](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.Details)** pour en déterminer la raison. En fonction de la raison, vous pouvez afficher l’état à l’utilisateur ou afficher des instructions pour l’activation de l’enregistrement par l’application.



[!code-cpp[CanRecord](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCanRecord)]

## <a name="manually-start-and-stop-recording-your-app-to-a-file"></a>Démarrer et arrêter manuellement l’enregistrement de votre application dans un fichier

Après avoir vérifié que votre application est en mesure d’enregistrer, vous pouvez démarrer un nouvel enregistrement en appelant la méthode **[StartRecordingToFileAsync](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.startrecordingtofileasync)** de l’objet **AppRecordingManager**.

Dans l’exemple suivant, le premier bloc **then** s’exécute en cas d’échec de la tâche asynchrone. Le deuxième bloc **then** tente d’accéder au résultat de la tâche et, si le résultat est null, la tâche est terminée. Dans les deux cas, la méthode d’assistance **OnRecordingComplete**, illustrée ci-dessous, est appelée pour gérer le résultat. 

[!code-cpp[StartRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartRecordToFile)]

Lorsque l’opération d’enregistrement est terminée, vérifiez la propriété **[Succeeded](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** de l’objet **[AppRecordingResult](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult)** renvoyé pour déterminer si l’opération d’enregistrement a réussi. Si c’est le cas, vous pouvez vérifier la propriété **[IsFileTruncated](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** pour déterminer si, pour des raisons de stockage, le système a été contraint de tronquer le fichier capturé. Vous pouvez vérifier la propriété **[Duration](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** pour établir la durée réelle du fichier enregistré. Si le fichier est tronqué, celle-ci risque d’être plus courte que la durée de l’opération d’enregistrement.

[!code-cpp[OnRecordingComplete](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnRecordingComplete)]

Les exemples suivants illustrent un code de base pour le démarrage et l’arrêt de l’opération d’enregistrement décrite dans l’exemple précédent.

[!code-cpp[CallStartRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallStartRecordToFile)]

[!code-cpp[FinishRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetFinishRecordToFile)]

## <a name="record-a-historical-time-span-to-a-file"></a>Enregistrer un intervalle de temps historique dans un fichier
Si l’utilisateur a activé l’enregistrement historique pour votre application dans les paramètres système, vous pouvez enregistrer un intervalle de temps de jeu déjà passé. Un exemple précédent de cet article vous a montré comment vérifier si votre application pouvait enregistrer le jeu actuellement. Une vérification supplémentaire s’impose pour déterminer si la capture historique est activée. Appelez de nouveau **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** et vérifiez la propriété **[CanRecordTimeSpan](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecordTimeSpan)** de l’objet **AppRecordingStatus** renvoyé. Cet exemple renvoie également la propriété **[HistoricalBufferDuration](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.HistoricalBufferDuration)** de l’objet **AppRecordingStatus** qui sera utilisée pour déterminer une heure de début valide pour l’opération d’enregistrement.

[!code-cpp[CanRecordTimeSpan](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCanRecordTimeSpan)]

Pour capturer un intervalle de temps historique, vous devez spécifier l’heure de début et la durée de l’enregistrement. L’heure de début est fournie en tant que structure **[DateTime](https://docs.microsoft.com/uwp/api/windows.foundation.datetime)**. L’heure de début doit être antérieure à l’heure actuelle et être comprise dans la longueur de la mémoire tampon d’historique. Pour cet exemple, la longueur du tampon est récupérée dans le cadre de la vérification effectuée pour établir si l’enregistrement historique est activé, ce qui est illustré dans l’exemple de code précédent. La durée de l’enregistrement historique est fournie en tant que structure **[TimeSpan](https://docs.microsoft.com/uwp/api/windows.foundation.timespan)**. Elle doit également être égale ou inférieure à la durée de la mémoire tampon d’historique. Une fois que vous avez défini l’heure de début et la durée voulues, appelez **[RecordTimeSpanToFileAsync](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.recordtimespantofileasync)** pour démarrer l’opération d’enregistrement.

À l’instar d’un enregistrement démarré et arrêté manuellement, à la fin d’un enregistrement historique, vous pouvez vérifier la propriété **[Succeeded](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** de l’objet **[AppRecordingResult](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult)** renvoyé pour déterminer si l’opération d’enregistrement a réussi. Vous pouvez également vérifier la propriété **[IsFileTruncated](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** et **[Duration](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** pour déterminer la durée réelle du fichier enregistré qui, si le fichier a été tronqué, risque d’être plus courte que la durée de l’intervalle de temps demandé.

[!code-cpp[RecordTimeSpanToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRecordTimeSpanToFile)]

L’exemple suivant illustre un code de base utilisé pour le démarrage de l’opération d’enregistrement historique décrite dans l’exemple précédent.

[!code-cpp[CallRecordTimeSpanToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallRecordTimeSpanToFile)]

## <a name="save-screenshot-images-to-files"></a>Enregistrer les images de capture d’écran dans des fichiers
Votre application peut initier une capture d’écran qui enregistre le contenu actuel de la fenêtre de l’application dans un ou plusieurs fichiers image avec des encodages d’image différents. Pour spécifier les encodages d’image à utiliser, créez une liste de chaînes dans laquelle chaque chaîne représente un type d’image. Les propriétés de **[ImageEncodingSubtypes](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingsubtypes)** fournissent la chaîne appropriée pour chaque type d’image pris en charge, par exemple **MediaEncodingSubtypes.Png** ou **MediaEncodingSubtypes.JpegXr**.

Lancez la capture d’écran en appelant la méthode **[SaveScreenshotToFilesAsync](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.savescreenshottofilesasync)** de l’objet **AppRecordingManager**. Le premier paramètre de cette méthode est un **StorageFolder** dans lequel les fichiers image seront enregistrés. Le deuxième paramètre est un préfixe de nom de fichier auquel le système ajoutera l’extension de chaque type d’image enregistré, par exemple «.png».

Le troisième paramètre de **SaveScreenshotToFilesAsync** est requis par le système pour effectuer la conversion d’espace de couleurs appropriée si la fenêtre à capturer affiche du contenu HDR. S’il existe un contenu HDR, ce paramètre doit être défini sur **AppRecordingSaveScreenshotOption.HdrContentVisible**. Sinon, utilisez **AppRecordingSaveScreenshotOption.None**. Le dernier paramètre de la méthode est la liste des formats d’image dans lesquels l’écran doit être capturé.

L’appel asynchrone de **SaveScreenshotToFilesAsync** renvoie un objet **[AppRecordingSavedScreenshotInfo](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingsavedscreenshotinfo)** qui fournit le **StorageFile** et la valeur **MediaEncodingSubtypes** associée indiquant le type d’image pour chaque image enregistrée.

[!code-cpp[SaveScreenShotToFiles](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetSaveScreenShotToFiles)]

L’exemple suivant illustre un code de base utilisé pour le démarrage de l’opération de capture d’écran décrite dans l’exemple précédent.

[!code-cpp[CallSaveScreenShotToFiles](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallSaveScreenShotToFiles)]

## <a name="add-game-metadata-for-system-and-app-initiated-capture"></a>Ajouter des métadonnées de jeu pour la capture initiée par le système et par l’application
Les sections suivantes de cet article décrivent comment fournir des métadonnées que le système va ensuite incorporer dans le flux MP4 du jeu capturé ou diffusé. Les métadonnées peuvent être incorporées dans les médias capturés à l’aide de l’interface utilisateur système intégrée et les médias capturés par l’application avec **AppRecordingManager**. Ces métadonnées peuvent être extraites par votre application et d’autres applications pendant la lecture des médias afin de fournir des expériences, prenant en charge le contexte, qui sont synchronisées avec le jeu capturé ou diffusé.

### <a name="get-an-instance-of-appcapturemetadatawriter"></a>Obtenir une instance de AppCaptureMetadataWriter
La classe principale pour la gestion des métadonnées de capture d’application est **[AppCaptureMetadataWriter](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter)**. Avant d’initialiser une instance de cette classe, utilisez la méthode **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** pour rechercher *Windows.Media.Capture.AppCaptureMetadataContract* version1.0 afin de vérifier que l’API est disponible sur l’appareil actuel.

[!code-cpp[GetMetadataWriter](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetGetMetadataWriter)]

### <a name="write-metadata-to-the-system-cache-for-your-app"></a>Écrire les métadonnées dans le cache système pour votre application
Chaque élément de métadonnées est doté d’une étiquette de chaîne qui identifie l’élément de métadonnées, une valeur de données associée, qui peut être une chaîne, un entier ou une valeur double, et une valeur de l’énumération **[AppCaptureMetadataPriority](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatapriority)** indiquant le priorité relative de l’élément de données. Un élément de métadonnées peut être considéré comme un «événement», qui se produit à un moment unique dans le temps, ou un «état» qui conserve une valeur durant un intervalle de temps. Les métadonnées sont écrites dans un cache mémoire qui est alloué et géré pour votre application par le système. Le système applique une limite de taille au cache mémoire de métadonnées. Lorsque cette limite est atteinte, il vide les données en fonction de la priorité avec laquelle chaque élément de métadonnées a été écrit. La section suivante de cet article montre comment gérer l’allocation de mémoire de métadonnées de votre application.

Une application standard peut opter d’écrire des métadonnées au début de la session de capture afin de fournir un contexte pour les données suivantes. Pour ce scénario, il est recommandé d’utiliser des données d’«événement» instantanées. Cet exemple appelle **[AddStringEvent](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.addstringevent)**, **[AddDoubleEvent](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.adddoubleevent)** et **[AddInt32Event](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.addint32event)** pour définir des valeurs instantanées pour chaque type de données.

[!code-cpp[StartSession](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartSession)]

Un scénario courant pour l’utilisation de données d’«état» qui persistent dans le temps consiste à effectuer le suivi de la carte de jeu actuellement utilisée par le lecteur. Cet exemple appelle **[StartStringState](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.startstringstate)** pour définir la valeur d’état. 

[!code-cpp[StartMap](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartMap)]

Appelez **[StopState](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.stopstate)** pour enregistrer la fin d’un état particulier.

[!code-cpp[EndMap](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetEndMap)]

Vous pouvez remplacer un état en définissant une nouvelle valeur avec une étiquette d’état existante.

[!code-cpp[LevelUp](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetLevelUp)]

Vous pouvez mettre fin à tous les états actuellement ouverts en appelant **[StopAllStates](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.StopAllStates)**.

[!code-cpp[RaceComplete](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRaceComplete)]

### <a name="manage-metadata-cache-storage-limit"></a>Gérer la limite de stockage de cache de métadonnées
Les métadonnées que vous écrivez avec **AppCaptureMetadataWriter** sont mises en cache par le système jusqu’à ce qu’elles soient écrites dans le flux de média associé. Le système définit une limite de taille pour le cache de métadonnées de chaque application. Une fois la limite de taille du cache atteinte, le système commence à vider les métadonnées mises en cache. Le système supprimera les métadonnées écrites avec la valeur de priorité **[AppCaptureMetadataPriority.Informational](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatapriority)** avant les métadonnées ayant la priorité **[AppCaptureMetadataPriority.Important](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatapriority)**.

À tout moment, vous pouvez vérifier le nombre d’octets disponibles dans le cache de métadonnées de votre application en appelant **[RemainingStorageBytesAvailable](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.RemainingStorageBytesAvailable)**. Vous pouvez choisir de définir le seuil défini par l’application. Une fois ce seuil atteint, vous pouvez choisir de réduire la quantité de métadonnées écrites dans le cache. L’exemple suivant montre une implémentation simple de ce modèle.

[!code-cpp[CheckMetadataStorage](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCheckMetadataStorage)]

[!code-cpp[ComboExecuted](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetComboExecuted)]

### <a name="receive-notifications-when-the-system-purges-metadata"></a>Recevoir des notifications lorsque le système vide les métadonnées
Vous pouvez vous inscrire pour recevoir une notification lorsque le système commence le vidage des métadonnées de votre application en inscrivant un gestionnaire pour l’événement **[MetadataPurged](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.MetadataPurged)**.

[!code-cpp[RegisterMetadataPurged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRegisterMetadataPurged)]

Dans le gestionnaire de l’événement **MetadataPurged**, vous pouvez libérer de l’espace dans le cache de métadonnées en mettant fin aux états de faible priorité. Vous pouvez également implémenter une logique définie par l’application pour réduire la quantité de métadonnées écrites dans le cache ou choisir de ne rien faire et laisser le système continuer à vider le cache en fonction de la priorité avec lequel elles ont été écrites.

[!code-cpp[OnMetadataPurged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnMetadataPurged)]

## <a name="related-topics"></a>Rubriques associées

* [Jeux](index.md)
 

 




