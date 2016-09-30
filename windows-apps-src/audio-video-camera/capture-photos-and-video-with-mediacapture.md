---
author: drewbatgit
ms.assetid: 1361E82A-202F-40F7-9239-56F00DFCA54B
description: "Est décrite ici la capture à l’aide de MediaCapture, notamment l’initialisation et l’arrêt de MediaCapture, et la gestion des modifications d’orientation."
title: "Capturer des photos et des vidéos à l’aide de MediaCapture"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c20c735d38e6baabe2f8bc0c7c682706d3946ed9

---

# Capturer des photos et des vidéos à l’aide de MediaCapture

\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Cet article décrit les étapes permettant de capturer des photos et des vidéos à l’aide de l’API [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124), y compris d’initialiser et d’arrêter **MediaCapture** et de gérer les modifications relatives à l’orientation de l’appareil.

**MediaCapture** est fourni pour prendre en charge les applications nécessitant un niveau de contrôle faible du processus de capture multimédia et qui mettent en œuvre des scénarios nécessitant des fonctionnalités de capture avancées. L’utilisation de **MediaCapture** nécessite également de fournir votre propre interface utilisateur de capture. Si votre application a uniquement besoin de capturer une photo ou une vidéo et ne requiert pas de techniques de captures avancées, la classe [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) permet la capture de photos ou de vidéos au moyen de quelques lignes de code seulement. Pour plus d’informations, voir [Capturer des photos et des vidéos avec CameraCaptureUI](capture-photos-and-video-with-cameracaptureui.md).

Le code figurant dans cet article a été adapté à partir de l’[exemple CameraStarterKit](http://go.microsoft.com/fwlink/?LinkId=619479). Vous pouvez télécharger l’exemple pour voir le code utilisé en contexte ou pour vous en servir comme point de départ pour votre propre application.

## Configurer votre projet

### Ajouter des déclarations de fonctionnalités au manifeste de l’application

Afin que votre application puisse accéder à l’appareil photo d’un appareil, vous devez déclarer que cette application utilise les fonctionnalités de l’appareil *webcam* et *microphone*. Si vous souhaitez enregistrer dans la bibliothèque d’images ou dans la vidéothèque de l’utilisateur des photos et des vidéos capturées, vous devez également déclarer les fonctionnalités *picturesLibrary* et *videosLibrary*.

**Ajouter des fonctionnalités au manifeste de l’application**

1.  Dans Microsoft Visual Studio, dans l’**Explorateur de solutions**, ouvrez le concepteur pour le manifeste de l’application en double-cliquant sur l’élément **package.appxmanifest**.
2.  Sélectionnez l’onglet **Fonctionnalités**.
3.  Activez les cases à cocher **Webcam** et **Microphone**.
4.  Pour accéder à la bibliothèque d’images et à la vidéothèque, cochez les cases correspondant à **Bibliothèque d’images** et **Vidéothèque**.

### Ajouter des directives using pour les API relatives à la capture multimédia

La liste de codes suivante répertorie les espaces de noms auxquels l’exemple de code fait référence dans cet article et décrit la fonctionnalité fournie par chacun de ces espaces.

[!code-cs[Using](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## Initialiser l’objet MediaCapture

La classe [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) de l’espace de noms [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) est l’interface essentielle pour toutes les opérations de capture multimédia. En général, les applications déclarent une variable de ce type dont la portée se limite à une seule page. Votre application doit effectuer le suivi de l’état actuel de l’élément **MediaCapture**, ce qui implique la déclaration de variables booléennes relatives à l’état d’initialisation, de prévisualisation et d’enregistrement de l’objet.

[!code-cs[MediaCaptureVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMediaCaptureVariables)]

Pour permettre d’orienter correctement l’aperçu de la vidéo, créez des variables de membre pour savoir s’il s’agit d’un appareil photo externe et si l’application procède actuellement à la mise en miroir du flux d’aperçu. Votre application doit mettre en miroir le flux d’aperçu lorsque vous pensez que le flux vidéo capture l’utilisateur pour une expérience utilisateur plus naturelle.

[!code-cs[PreviewVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewVariables)]

La méthode de l’exemple suivant initialise l’objet de capture multimédia. Tout d’abord, le code recherche un appareil de capture vidéo qui peut être utilisé pour la capture multimédia. Une fois trouvé, l’objet **MediaCapture** est initialisé et les gestionnaires de ces événements sont enregistrés. Ensuite, un objet [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/desktop/hh802710) est créé à l’aide de l’ID de l’appareil de capture vidéo. Le **MediaCapture** est ensuite initialisé en faisant appel à [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598).

[!code-cs[InitializeCameraAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitializeCameraAsync)]

-   La méthode [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) peut être utilisée pour trouver tous les appareils d’un type donné. Dans cet exemple, la valeur d’énumération **DeviceClass.VideoCapture** est transmise pour indiquer que seuls les appareils de capture vidéo doivent être renvoyés. Notez qu’un appareil de capture vidéo est utilisé pour capturer à la fois les photos et les vidéos.

-   **FindAllAsync** renvoie un objet [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) qui contient un objet [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) pour chaque appareil trouvé correspondant au type demandé. La méthode d’extension **FirstOrDefault** issue de l’espace de noms **System.Linq** propose une syntaxe simple permettant de sélectionner un élément d’une liste reposant sur des conditions définies. Le premier appel essaie de sélectionner le premier élément **DeviceInformation** de la liste dont la valeur [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) est de **Panel.Back** indiquant que l’appareil photo se trouve à l’arrière du boîtier de l’appareil. Si l’appareil ne dispose pas d’un appareil photo à l’arrière, le premier appareil photo disponible est utilisé.

-   Si vous ne spécifiez pas d’ID d’appareil lors de l’initialisation de [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573), le système choisit le premier appareil de sa liste d’appareils internes.

-   L’appel à [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) initialise l’objet afin d’utiliser l’appareil de capture spécifié. Cet appel est effectué à l’intérieur d’un bloc **try**, car il génère un élément **UnauthorizedAccessException** si l’utilisateur refuse que l’application appelante accède à l’appareil photo. Si l’appel aboutit, la variable **\_isInitialized** est définie sur true pour que les appels de méthode suivants puissent déterminer si l’appareil de capture a été initialisé.

- **Important** Sur certaines familles d’appareils, l’utilisateur doit approuver une invite de consentement avant que votre application puisse avoir accès à l’appareil photo de l’appareil. Pour cette raison, vous devez uniquement appeler [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) à partir du thread d’interface utilisateur principal. Toute tentative d’initialisation de l’appareil photo à partir d’un autre thread peut entraîner un échec de l’initialisation.

-   Si l’initialisation de l’appareil de capture est réussie, les variables sont définies de manière à indiquer si l’appareil de capture est externe, ou s’il se trouve à l’avant de l’appareil. Ces valeurs seront utilisées pour orienter correctement l’aperçu de la capture pour l’utilisateur. Enfin, l’interface utilisateur est mise à jour de manière à indiquer que la capture est disponible et que le flux d’aperçu de l’appareil de capture est lancé. Toutes ces tâches sont exécutées dans les méthodes d’assistance qui seront décrites plus loin dans cet article.

## Démarrer l’aperçu de capture

Pour que l’utilisateur soit en mesure d’afficher ce qu’il capture, vous devez fournir un aperçu de ce que l’appareil de capture vidéo voit actuellement dans votre interface utilisateur.

**Important** Vous devez démarrer l’aperçu de capture afin que l’appareil de capture active la mise au point, l’exposition et la balance des blancs automatiques.

Le contrôle [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) est fourni pour activer l’aperçu de capture. Voici un exemple de code XAML qui définit l’élément de capture.

[!code-xml[CaptureElement](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetCaptureElement)]

Les utilisateurs s’attendent à ce que l’écran reste allumé lorsqu’ils regardent l’écran de capture vidéo et pas à ce qu’il s’éteigne en raison d’une inactivité. Pour ce faire, vous devez créer un objet [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816). Déclarez cette variable avec une portée de page afin qu’elle persiste pendant toute la session de capture.

[!code-cs[DisplayRequest](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayRequest)]

La méthode suivante démarre l’aperçu de capture multimédia. Tout d’abord, elle demande que l’écran reste actif en appelant [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818) sur la classe [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816). Ensuite, l’aperçu est lancé par un appel à [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613).

[!code-cs[Méthode StartPreviewAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]

-   La méthode [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818) est appelée sur l’objet **DisplayRequest** pour demander au système de laisser l’écran actif.

-   La propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) de l’élément **CaptureElement** est définie sur l’objet de l’application **MediaCapture** pour définir la source de l’aperçu.

-   La propriété [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) est fournie par l’infrastructure XAML pour la prise en charge des interfaces utilisateur bidirectionnelles. Définissez la direction du flux de **CaptureElement** sur [**FlowDirection.RightToLeft**](https://msdn.microsoft.com/library/windows/apps/br242397) pour retourner horizontalement l’aperçu vidéo. Ce retournement est utilisé lorsque l’appareil de capture se trouve à l’avant de l’appareil de manière à placer l’aperçu dans la bonne direction du point de vue de l’utilisateur.

-   La méthode [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613) démarre l’affichage du flux d’aperçu dans l’élément **CaptureElement**. Si l’aperçu est lancé avec succès, la variable **\_isPreviewing** est configurée de manière à permettre aux autres parties de l’application de savoir que l’application affiche actuellement un aperçu et la méthode d’assistance permettant de définir la rotation de l’aperçu est appelée. Cette méthode est définie dans la section suivante.

## Détecter l’orientation de l’écran et de l’appareil

Lors de leur exécution sur un appareil mobile comme un téléphone ou une tablette, plusieurs zones d’une application de capture multimédia sont affectées par l’orientation actuelle de l’appareil. Ces zones impliquent une rotation adéquate du flux d’aperçu à partir de l’appareil photo et un codage approprié des images et des vidéos capturées, de sorte qu’elles soient correctement orientées lorsqu’elles sont visualisées par l’utilisateur.

Le terme « orientation de l’affichage » désigne la façon dont le système fait pivoter la page XAML sur l’appareil pour qu’elle s’affiche de manière appropriée pour l’utilisateur. Le terme « Orientation de l’appareil » fait référence à l’orientation de l’appareil dans l’espace et, par conséquent, à l’orientation de l’appareil photo physique. Les deux types d’orientation sont importants pour une application de capture multimédia. Pour gérer l’orientation de l’affichage, déclarez et initialisez une variable portant sur une page pour la classe [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/dn264258). Déclarez une variable de type [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/br226142) pour suivre l’orientation actuelle de l’affichage. Déclarez une variable [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400) et [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399) pour suivre l’orientation de l’appareil.

[!code-cs[DisplayInformationAndOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayInformationAndOrientation)]

Les méthodes d’assistance suivantes enregistrent et annulent l’enregistrement des gestionnaires d’événements pour les événements [**DisplayInformation.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) et [**SimpleOrientationSensor.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/br206407) et initialisent les variables de suivi avec l’orientation actuelle. Notez que tous les appareils ne disposent pas d’une classe [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400). Vous devez donc vérifier ce point avant de procéder à l’enregistrement du gestionnaire ou de tenter d’obtenir l’orientation actuelle.

[!code-cs[RegisterOrientationEventHandlers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterOrientationEventHandlers)]

[!code-cs[UnregisterOrientationEventHandlers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterOrientationEventHandlers)]

Mettez à jour la variable d’orientation de l’appareil en tenant compte de l’orientation actuelle dans le gestionnaire d’événements pour l’événement **SimpleOrientationSensor.OrientationChanged**. Vous ne devez pas mettre à jour l’orientation si l’appareil est orienté vers le haut ou vers le bas.

[!code-cs[SimpleOrientationChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSimpleOrientationChanged)]

Mettez à jour la variable d’orientation d’affichage en tenant compte de l’orientation actuelle dans le gestionnaire d’événements pour l’événement **DisplayInformation.OrientationChanged**. Si l’aperçu vidéo de l’appareil de capture est affiché, mettez à jour la rotation du flux vidéo d’aperçu. La méthode d’assistance **SetPreviewRotationAsync** est décrite dans la section suivante.

[!code-cs[DisplayOrientationChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayOrientationChanged)]

## Définir la rotation de l’aperçu de capture multimédia

Les utilisateurs s’attendent à ce que les commandes de l’interface utilisateur pivotent lorsque l’orientation de leur appareil mobile change afin que le texte de cette interface s’affiche de manière verticale et soit lisible. Pour le contrôle **CaptureElement**, toutefois, en règle générale, les utilisateurs ne souhaitent pas que l’orientation de l’aperçu vidéo change en fonction de l’orientation de l’appareil. Pour offrir l’expérience utilisateur attendue, vous devez faire pivoter le flux d’aperçu pour qu’il corresponde à l’orientation de l’appareil.

L’orientation du flux d’aperçu doit être exprimée en degrés. La méthode d’assistance suivante permet de convertir les valeurs d’énumération [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/br226142) en degrés.

[!code-cs[ConvertDisplayOrientationToDegrees](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertDisplayOrientationToDegrees)]

Cette méthode d’assistance convertit une valeur d’énumération [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399), qui est utilisée par [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400) pour exprimer la rotation de l’appareil en degrés.

[!code-cs[ConvertDeviceOrientationToDegrees](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertDeviceOrientationToDegrees)]

À un niveau inférieur, la rotation d’un flux est réellement effectuée par l’infrastructure Microsoft Media Foundation. La rotation est spécifiée à l’aide de l’attribut [MF\_MT\_VIDEO\_ROTATION](https://msdn.microsoft.com/library/windows/desktop/hh162880). Puisqu’il s’agit d’une application Windows Runtime, la rotation est définie à l’aide du GUID pour l’attribut, plutôt que pour le nom d’attribut. Définissez le GUID suivant pour identifier l’attribut de rotation vidéo.

[!code-cs[RotationKey](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRotationKey)]

La méthode suivante définit la rotation du flux d’aperçu. La méthode [**GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995) de la classe [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) de capture multimédia renvoie un jeu de propriétés constitué de paires clé/valeur. [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640) est spécifié pour indiquer que nous voulons les propriétés relatives au flux d’aperçu vidéo, et non pas au flux d’enregistrement vidéo ou au flux audio. La propriété définie est une interface générale permettant de définir les propriétés de flux. Toutefois, pour cette tâche le GUID de rotation vidéo défini ci-dessus est ajouté comme clé de propriété et l’orientation de flux vidéo voulue exprimée en degrés est spécifiée comme étant la valeur. [**SetEncodingPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/dn297781) met à jour les propriétés de codage en utilisant les nouvelles valeurs. Une fois encore, l’élément **MediaStreamType.VideoPreview** est spécifié pour indiquer que les propriétés définies sont relatives au flux d’aperçu vidéo.

[!code-cs[Méthode SetPreviewRotation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetPreviewRotation)]

-   Pour les appareils avec appareils photo externes, l’utilisateur ne s’attend pas à ce que le flux de l’appareil photo pivote en même temps que l’appareil.

-   Si l’aperçu est en miroir pour un appareil photo situé à l’avant, le sens de rotation doit être inversé pour correspondre à la rotation de l’appareil.

-   Certains appareils, généralement des téléphones, prennent en charge la définition de la propriété [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/dn264259) sur une valeur d’orientation telle que [**DisplayOrientations.Landscape**](https://msdn.microsoft.com/library/windows/apps/br226142) pour forcer l’affichage à pivoter en même temps que l’appareil. Vous devez définir cette valeur, car elle est intéressante à utiliser sur les appareils qui la prennent en charge. Cependant, vous devez tout de même implémenter le modèle ci-dessus dans votre application pour prendre en charge les appareils qui ne gèrent pas les préférences de rotation automatique.

-   Dans les versions précédentes, la méthode [**SetPreviewRotation**](https://msdn.microsoft.com/library/windows/apps/br226611) a été le seul moyen de faire pivoter le flux d’aperçu. Cette méthode est toujours présente à la surface de l’API pour la prise en charge des applications existantes. Toutefois, cette méthode est inefficace et ne doit pas être utilisée pour les nouvelles applications.

## Capturer une photo

La méthode suivante permet de capturer une photo à l’aide de la méthode [**CapturePhotoToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/hh700840) et en transmettant les propriétés de codage demandées et un objet [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241720) qui contiendra le résultat de l’opération de capture. La classe [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) fournit des méthodes d’assistance, telles que [**CreateJpeg**](https://msdn.microsoft.com/library/windows/apps/hh700994), pour générer les propriétés de codage pour les types de fichiers pris en charge par la capture multimédia.

[!code-cs[TakePhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetTakePhotoAsync)]

Avant d’enregistrer la photo dans un fichier, vous devez déterminer l’orientation appropriée de la photo. L’objet **MediaCapture** ne connaît pas l’orientation de l’appareil, et par conséquent, encode les données de photo capturées comme si l’orientation par défaut était appliquée à l’appareil de capture. Le fait de consulter une photo capturée orientée de manière incorrecte peut avoir un effet négatif sur l’expérience utilisateur. Les méthodes d’assistance suivantes déterminent l’orientation correcte de la photo, puis enregistrent le fichier avec l’orientation correcte.

La méthode d’assistance **GetCameraOrientation** commence par l’orientation actuelle de l’appareil, puis fait pivoter cette valeur en fonction de l’orientation native de l’appareil et l’emplacement de l’appareil photo sur ce dernier. Si l’appareil photo se trouve à l’avant de l’appareil, comme indiqué dans cet exemple par la variable **\_mirroringPreview**, l’orientation de l’appareil photo doit être inversée.

[!code-cs[GetCameraOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetCameraOrientation)]

La méthode d’assistance suivante se contente de convertir les valeurs issues des valeurs d’énumération [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399) utilisées par le capteur d’orientation dans la valeur [**PhotoOrientation**](https://msdn.microsoft.com/library/windows/apps/hh965476) équivalente utilisée par l’encodeur d’image bitmap qui sera utilisé pour enregistrer le fichier.

[!code-cs[ConvertOrientationToPhotoOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertOrientationToPhotoOrientation)]

Enfin, la photo capturée peut être codée et enregistrée. Créez un objet [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) à partir du flux d’entrée qui contient les données de photo capturées. Créez un nouveau [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) et ouvrez-le pour la lecture et l’écriture. Créez un objet [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206) en transmettant le fichier de sortie et le décodeur contenant les données d’image. Créez une nouvelle classe [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/hh974338) et ajoutez une nouvelle propriété. La clé pour la propriété, «System.Photo.Orientation» indique que la propriété représente l’orientation de la photo. La valeur correspond à la valeur [**PhotoOrientation**](https://msdn.microsoft.com/library/windows/apps/hh965476) calculée précédemment. Appelez [**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252) pour mettre à jour les propriétés sur l’encodeur, puis appelez [**FlushAsync**](https://msdn.microsoft.com/library/dn237883) pour enregistrer la photo dans le fichier de stockage.

[!code-cs[ReencodeAndSavePhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetReencodeAndSavePhotoAsync)]

-   Définissez la propriété de bitmap «System.Photo.Orientation» pour coder l’orientation de la photo dans les métadonnées du fichier. Cela n’entraîne pas un codage différent des données d’image réelles. Pour plus d’informations sur l’incorporation des métadonnées dans les fichiers image, voir [Métadonnées d’image](image-metadata.md).

-   Pour plus d’informations sur l’utilisation des images, y compris sur le codage et le décodage d’images, voir [Acquisition d’images](imaging.md).

## Capturer une vidéo

Pour démarrer la capture de vidéos, commencez par créer un fichier de stockage dans lequel la vidéo sera enregistrée. Créez ensuite la classe [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) que l’élément **MediaCapture** utilisera pour coder la vidéo dans le fichier. La classe **MediaEncodingProfile** fournit des méthodes, telles que [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/hh701078), qui créent des profils de codage pour les formats vidéo pris en charge. Utilisez les méthodes d’assistance décrites précédemment pour obtenir la rotation de vidéo appropriée exprimée en degrés. À la différence du scénario relatif à la photo, les informations de rotation vidéo sont codées dans le flux par l’élément **MediaCapture**. Ajoutez les informations de rotation au profil de codage en l’ajoutant à la collection [**VideoEncodingProperties.Properties**](https://msdn.microsoft.com/library/windows/apps/hh701254). Le GUID précédemment défini pour la rotation vidéo est utilisé comme clé et la rotation exprimée en degrés comme valeur. Enfin, appelez [**MediaCapture.StartRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh700863) en indiquant les propriétés de codage et le fichier de sortie pour commencer l’enregistrement.

[!code-cs[StartRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartRecordingAsync)]

Pour arrêter l’enregistrement, appelez simplement [**MediaCapture.StopRecordAsync**](https://msdn.microsoft.com/library/windows/apps/br226623).

[!code-cs[StartRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStopRecordingAsync)]

Un gestionnaire pour l’événement [**MediaCapture.RecordLimitationExceeded**](https://msdn.microsoft.com/library/windows/apps/hh973312) a été enregistré lors de l’initialisation de **MediaCapture**. Dans le gestionnaire, appelez la méthode **StopRecordingAsync** pour arrêter l’enregistrement vidéo.

[!code-cs[Événement RecordLimitationExceeded](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceeded)]

## Suspendre et reprendre la capture vidéo

Dans certains scénarios, vous pouvez être amené à suspendre et à reprendre une capture vidéo en cours au lieu de l’arrêter et d’en démarrer une nouvelle. Pour suspendre l’enregistrement, appelez [**PauseRecordAsync**](https://msdn.microsoft.com/library/windows/apps/dn858102). Si vous spécifiez [**MediaCapturePauseBehavior.RetainHardwareResources**](https://msdn.microsoft.com/library/windows/apps/dn926686), l’application ne pourra pas capturer de photos pendant l’interruption de la vidéo sur les appareils qui ne prennent pas en charge la capture simultanée de photos et de vidéos. Pour plus d’informations sur la façon de déterminer si un appareil prend en charge la capture simultanée de photos et de vidéos, voir [Profils d’appareil photo](camera-profiles.md).

[!code-cs[PauseRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPauseRecordingAsync)]

Pour reprendre une capture vidéo préalablement suspendue, appelez [**ResumeRecordAsync**](https://msdn.microsoft.com/library/windows/apps/dn858103).

[!code-cs[ResumeRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetResumeRecordingAsync)]

## Nettoyer les ressources de capture

Il est très important d’arrêter et de libérer correctement les ressources de capture multimédia. Ne pas procéder ainsi peut entraîner un comportement inattendu de l’appareil photo à la fermeture de l’application et se traduire par une expérience utilisateur négative. Les sections suivantes décrivent les différentes étapes à suivre pour arrêter l’appareil photo.

### Arrêter l’aperçu de capture

Pour arrêter l’aperçu de capture, commencez par appeler [**MediaCapture.StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622). Définissez la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) de votre [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) sur null. Ensuite, appelez [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819) sur votre variable [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) pour permettre au système de désactiver l’affichage si nécessaire.

[!code-cs[StopPreviewAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStopPreviewAsync)]

-   Vous ne pouvez pas accéder à l’interface utilisateur à partir d’un thread sans interface utilisateur. Par conséquent, la définition de la propriété **MediaElement.Source** et l’appel de **RequestRelease** doivent être effectués à l’aide de la méthode [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) de sorte que les appels s’exécutent sur le thread d’interface utilisateur.

### Arrêter et supprimer l’objet MediaCapture

Avant de supprimer l’objet **MediaCapture**, arrêtez tout enregistrement en cours et le flux d’aperçu en appelant les méthodes d’assistance définies précédemment. Une fois cette opération effectuée, supprimez tous les gestionnaires d’événements enregistrés avec l’objet **MediaCapture**, puis appelez [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858) pour libérer les ressources associées à l’objet et définissez la variable **MediaCapture** sur null.

[!code-cs[CleanupCameraAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

Vous devez appeler cette méthode pour arrêter et supprimer l’objet **MediaCapture** depuis le gestionnaire d’événements pour l’événement [**MediaCapture.Failed**](https://msdn.microsoft.com/library/windows/apps/br226593).

[!code-cs[CaptureFailed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureFailed)]

### Gérer les événements de durée de vie d’application et de navigation entre les pages

Les événements de durée de vie d’application permettent à votre application d’initialiser et de libérer des ressources. Cela est particulièrement important avec l’événement **Application.Suspending**, pour lequel il est essentiel que votre application supprime correctement les ressources de capture multimédia.

Vous pouvez enregistrer des gestionnaires pour les événements de durée de vie d’application dans le constructeur de votre page.

[!code-cs[RegisterAppLifetimeEvents](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterAppLifetimeEvents)]

Dans le gestionnaire de l’événement **Application.Suspending**, vous devez annuler l’enregistrement des gestionnaires pour les événements d’orientation de l’affichage et de l’appareil et arrêter l’objet **MediaCapture**. L’événement [**SystemMediaTransportControls.PropertyChanged**](https://msdn.microsoft.com/library/windows/apps/dn278720) dont l’enregistrement a été supprimé ici est nécessaire pour une autre tâche relative au cycle de vie décrite plus loin dans cet article.

**Attention** Vous devez demander un report de suspension en appelant [**SuspendingOperation.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) au début de votre gestionnaire d’événements de suspension. En procédant ainsi, vous demandez au système d’attendre que vous ayez signalé la fin de l’opération avant l’arrêt de votre application. Cela est nécessaire car les opérations d’arrêt de **MediaCapture** sont asynchrones, c’est pourquoi le gestionnaire d’événements **Application.Suspending** peut s’arrêter avant que l’appareil photo soit correctement éteint. Une fois vos appels asynchrones attendus terminés, vous devez libérer le report en appelant [**SuspendingDeferral.Complete**](https://msdn.microsoft.com/library/windows/apps/br224685).

[!code-cs[Suspending](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSuspending)]

Dans le gestionnaire de l’événement **Application.Resuming**, vous devez enregistrer des gestionnaires pour les événements d’orientation d’affichage et de l’appareil, enregistrer l’événement **SystemMediaTransportControls.PropertyChanged** et initialiser l’objet **MediaCapture**.

[!code-cs[Resuming](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetResuming)]

L’événement [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) donne la possibilité d’enregistrer initialement des gestionnaires pour les événements d’orientation d’affichage et de l’appareil et initialiser l’objet **MediaCapture**.

[!code-cs[OnNavigatedTo](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnNavigatedTo)]

Si votre application comporte plusieurs pages, vous devez nettoyer vos objets de capture multimédia dans le gestionnaire d’événements [**OnNavigatingFrom**](https://msdn.microsoft.com/library/windows/apps/br227509).

[!code-cs[OnNavigatingFrom](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnNavigatingFrom)]

Pour que votre application fonctionne correctement sur les appareils qui prennent en charge l’affichage simultané de plusieurs fenêtres, vous devez répondre lorsque votre application est réduite ou restaurée. Pour ce faire, vous devez gérer l’événement [**SystemMediaTransportControls.PropertyChanged**](https://msdn.microsoft.com/library/windows/apps/dn278720). Initialisez une variable de membre pour stocker une référence à l’objet [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) de votre application.

[!code-cs[DeclareSystemMediaTransportControls](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareSystemMediaTransportControls)]

Le code pour enregistrer et annuler l’enregistrement de l’événement **PropertyChanged** doit être ajouté aux événements de cycle de vie de l’application, comme indiqué dans les exemples ci-dessus. Dans le gestionnaire de l’événement, vérifiez si la modification de propriété qui a déclenché l’événement était la propriété [**SystemMediaTransportControlsProperty.SoundLevel**](https://msdn.microsoft.com/library/windows/apps/dn278721). S’il s’agit bien de cette propriété, vérifiez sa valeur. Si la valeur est [**SoundLevel.Muted**](https://msdn.microsoft.com/library/windows/apps/hh700852), cela signifie que votre application a été réduite et que vous devez nettoyer vos ressources de capture multimédia. Sinon, l’événement signale que votre fenêtre d’application a été restaurée et que vous devez réinitialiser les ressources de capture multimédia. La propriété **SoundLevel** doit être accessible sur le thread d’interface utilisateur, afin que le code de cette méthode soit encapsulé dans un appel à [**Dispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317).

[!code-cs[SystemMediaControlsPropertyChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSystemMediaControlsPropertyChanged)]

## Considérations supplémentaires relatives à l’interface utilisateur concernant la capture multimédia

### Définir des préférences de rotation automatique

Comme indiqué dans la section précédente sur la rotation du flux d’aperçu, certains appareils prennent en charge la définition de [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/dn264259) afin d’éviter que la page, y compris l’élément [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) qui affiche l’aperçu, pivote selon la rotation de l’appareil. Cela permet d’améliorer l’expérience utilisateur sur les appareils qui prennent en charge cette fonctionnalité. Définissez cette valeur lors du lancement de votre application ou lorsque vous commencez à afficher l’aperçu. Notez que vous devez toujours implémenter la gestion de rotation de l’aperçu pour les appareils qui ne prennent pas en charge les préférences de rotation automatique.

[!code-cs[SetAutoRotationPreferences](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetAutoRotationPreferences)]

### Gérer l’orientation des éléments d’interface utilisateur

Les utilisateurs s’attendent généralement à ce que les éléments d’interface utilisateur d’une application d’appareil photo, tels que les boutons de lancement d’une capture photo ou vidéo, pivotent selon l’aperçu vidéo. La méthode suivante illustre l’utilisation des méthodes d’assistance d’orientation précédemment définies pour orienter correctement les boutons dans l’interface utilisateur de l’appareil photo. Vous devez appeler cette méthode à partir des gestionnaires d’événements [**DisplayInformation.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) et [**SimpleOrientationSensor.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/br206407), et au premier lancement de votre application. Votre implémentation peut varier en fonction de l’interface utilisateur de votre application.

[!code-cs[UpdateButtonOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateButtonOrientation)]

Lorsque votre application s’arrête ou si vous naviguez vers une page qui n’est pas associée à une capture multimédia, définissez la préférence de rotation automatique sur [**None**](https://msdn.microsoft.com/library/windows/apps/br226142) pour permettre à l’interface utilisateur de pivoter normalement.

[!code-cs[RevertAutoRotationPreferences](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRevertAutoRotationPreferences)]

### Prise en charge de la capture photo et vidéo

L’API [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) vous permet de capturer des photos et des vidéos simultanément sur les appareils qui prennent en charge cette fonctionnalité. Pour des raisons de concision, cet exemple utilise la propriété [**ConcurrentRecordAndPhotoSupported**](https://msdn.microsoft.com/library/windows/apps/dn278843) pour déterminer si la capture simultanée de photos et de vidéos est prise en charge, mais l’utilisation des profils d’appareil photo constitue un moyen plus fiable et recommandé pour cela. Pour plus d’informations voir [Profils d’appareil photo](camera-profiles.md).

La méthode d’assistance suivante met à jour les contrôles de l’application pour les faire correspondre à l’état actuel de capture de l’application. Appelez cette méthode chaque fois que l’état de capture de votre application change, par exemple lors du démarrage ou de l’arrêt d’une capture vidéo.

[!code-cs[UpdateCaptureControls](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateCaptureControls)]

### Prise en charge des fonctionnalités d’interface utilisateur mobiles spécifiques

Tout le code indiqué dans cet article fonctionne dans une application Windows universelle. L’ajout de quelques lignes de code vous permet de tirer parti de fonctionnalités d’interface utilisateur spécifiques présentes uniquement sur les appareils mobiles. Pour utiliser ces fonctionnalités, vous devez ajouter à votre projet une référence au kit de développement logiciel (SDK) Microsoft Mobile Extension pour la plateforme d’application universelle.

**Pour ajouter une référence au kit de développement logiciel (SDK) de l’extension mobile pour la prise en charge du bouton de l’appareil photo, procédez comme suit :**

1.  Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis cliquez sur **Ajouter une référence**.

2.  Développez le nœud **Universelle Windows** et sélectionnez**Extensions**.

3.  Cochez la case située en regard de **kit de développement logiciel (SDK) Microsoft Mobile Extension pour la plateforme d’application universelle**.

Les appareils mobiles disposent d’un contrôle [**StatusBar**](https://msdn.microsoft.com/library/windows/apps/dn633864) qui fournit à l’utilisateur des informations relatives à l’appareil. Ce contrôle occupe de l’espace sur l’écran , ce qui peut interférer avec l’interface utilisateur de capture multimédia. Vous pouvez masquer la barre d’état en appelant [**HideAsync**](https://msdn.microsoft.com/library/windows/apps/dn610339). Toutefois cet appel doit être effectué depuis un bloc conditionnel où vous utilisez la méthode [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/dn949016) pour déterminer si l’API est disponible. Cette méthode renvoie uniquement la valeur true sur les appareils mobiles qui prennent en charge la barre d’état. Vous devez masquer la barre d’état au lancement de votre application ou lorsque vous commencez à afficher un aperçu à partir de l’appareil photo.

[!code-cs[HideStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHideStatusBar)]

Lorsque votre application s’arrête ou lorsque l’utilisateur quitte la page de capture multimédia de votre application, vous rendez le contrôle de nouveau visible.

[!code-cs[ShowStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetShowStatusBar)]

Certains appareils mobiles disposent d’un bouton matériel dédié à l’appareil photo que certains utilisateurs préfèrent à une commande tactile. Pour être averti de l’utilisation du bouton matériel de l’appareil photo, enregistrez un gestionnaire pour l’événement [**HardwareButtons.CameraPressed**](https://msdn.microsoft.com/library/windows/apps/dn653805). Cette API est disponible sur les appareils mobiles, par conséquent, vous devez utiliser de nouveau l’élément **IsTypePresent** pour vous assurer que l’API est prise en charge sur l’appareil actuel avant d’essayer d’y accéder.

[!code-cs[PhoneUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhoneUsing)]

[!code-cs[RegisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterCameraButtonHandler)]

Dans le gestionnaire de l’événement **CameraPressed**, vous pouvez lancer une capture de photos.

[!code-cs[CameraPressed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCameraPressed)]

Lorsque votre application s’arrête ou que l’utilisateur quitte la page de capture multimédia de votre application, annulez l’enregistrement du gestionnaire de boutons matériels.

[!code-cs[UnregisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterCameraButtonHandler)]

**Remarque** Cet article s’adresse aux développeurs Windows10 qui développent des applications de plateforme Windows universelle (UWP). Si vous développez une application pour Windows8.x ou Windows Phone8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Rubriques connexes

* [Profils d’appareil photo](camera-profiles.md)
* [Capture photo avec plage dynamique élevée (HDR)](high-dynamic-range-hdr-photo-capture.md)
* [Contrôles de l’appareil de capture pour la photo et la vidéo](capture-device-controls-for-photo-and-video-capture.md)
* [Contrôles de l’appareil de capture pour la vidéo](capture-device-controls-for-video-capture.md)
* [Effets de capture vidéo](effects-for-video-capture.md)
* [Analyse de scène de capture multimédia](scene-analysis-for-media-capture.md)
* [Séquence de photos variables](variable-photo-sequence.md)
* [Obtenir une image d’aperçu](get-a-preview-frame.md)
* [Exemple CameraStarterKit](http://go.microsoft.com/fwlink/?LinkId=619479)



<!--HONumber=Jun16_HO4-->


