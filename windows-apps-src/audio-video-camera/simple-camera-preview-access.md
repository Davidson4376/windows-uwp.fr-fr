---
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: Cet article décrit comment afficher rapidement le flux d’aperçu de l’appareil photo sur une page XAML dans une application de plateforme Windows universelle (UWP).
title: Accès à l’aperçu simple de l’appareil photo
---

# Accès à l’aperçu simple de l’appareil photo

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Cet article décrit comment afficher rapidement le flux d’aperçu de l’appareil photo sur une page XAML dans une application de plateforme Windows universelle (UWP). La création d’une application qui capture des photos et des vidéos à l’aide de l’appareil photo nécessite que vous effectuiez des tâches telles que la gestion de l’orientation de l’appareil et de la caméra ou la définition des options de codage pour le fichier capturé. Pour certains scénarios d’application, vous pouvez simplement afficher le flux d’aperçu à partir de l’appareil photo sans tenir compte de ces autres considérations. Cet article vous montre comment effectuer cette opération avec un minimum de code. Vous devez toujours arrêter le flux d’aperçu correctement lorsque vous avez fini de l’utiliser en suivant les étapes ci-dessous.

Pour plus d’informations sur l’écriture d’une application d’appareil photo qui capture des photos ou des vidéos, voir [Capturer des photos et des vidéos à l’aide de MediaCapture](capture-photos-and-video-with-mediacapture.md).

## Ajouter des déclarations de fonctionnalités au manifeste de l’application

Afin que votre application puisse accéder à l’appareil photo d’un appareil, vous devez déclarer que cette application utilise les fonctionnalités de l’appareil *webcam* et *microphone*. Si vous souhaitez enregistrer dans la bibliothèque d’images ou dans la vidéothèque de l’utilisateur des photos et des vidéos capturées, vous devez également déclarer les fonctionnalités *picturesLibrary* et *videosLibrary*.

**Ajouter des fonctionnalités au manifeste de l’application**

1.  Dans Microsoft Visual Studio, dans l’**Explorateur de solutions**, ouvrez le concepteur pour le manifeste de l’application en double-cliquant sur l’élément **package.appxmanifest**.
2.  Sélectionnez l’onglet **Fonctionnalités**.
3.  Activez les cases à cocher **Webcam** et **Microphone**.
4.  Pour accéder à la bibliothèque d’images et à la vidéothèque, cochez les cases correspondant à **Bibliothèque d’images** et **Vidéothèque**.

## Ajouter un objet CaptureElement à votre page

Utilisez un [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) pour afficher le flux d’aperçu sur votre page XAML.

[!code-xml[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]

## Utiliser MediaCapture pour démarrer le flux d’aperçu

L’objet [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) est l’interface de votre application pour l’appareil photo de l’appareil. Cette classe est membre de l’espace de noms Windows.Media.Capture. L’exemple de cet article utilise également des API à partir des espaces de noms [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691) et [System.Threading.Tasks](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.aspx), en plus de celles fournies par le modèle de projet par défaut.

[!code-cs[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]

Ajoutez des directives using pour inclure les espaces de noms suivants dans le fichier .cs de votre page.

[!code-cs[SimpleCameraPreviewUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSimpleCameraPreviewUsing)]

Déclarez une variable de classe pour l’objet **MediaCapture**.

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

Créez une instance de la classe **MediaCapture** et appelez [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) pour initialiser l’appareil de capture. Cette méthode pouvant par exemple échouer sur des appareils n’ayant pas d’appareil photo, vous devez donc l’appeler à partir d’un bloc **try**. Une **UnauthorizedAccessException** est levée lorsque vous tentez d’initialiser l’appareil photo si l’utilisateur a désactivé l’accès à ce dernier dans les paramètres de confidentialité de l’appareil. Vous verrez également cette exception au cours du développement si vous n’avez pas ajouté les fonctionnalités appropriées au manifeste de votre application.

**Important** Sur certaines familles d’appareils, l’utilisateur doit approuver une invite de consentement avant que votre application puisse avoir accès à l’appareil photo de l’appareil. Pour cette raison, vous devez uniquement appeler [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) à partir du thread d’interface utilisateur principal. Toute tentative d’initialiser l’appareil photo à partir d’un autre thread peut entraîner un échec de l’initialisation.

Connectez **MediaCapture** à **CaptureElement** en définissant la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280). Enfin, démarrez l’aperçu en appelant [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613).

[!code-cs[StartPreviewAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]


## Arrêter le flux d’aperçu

Lorsque vous avez fini d’utiliser le flux d’aperçu, vous devez toujours arrêter le flux et supprimer correctement les ressources associées afin de garantir que l’appareil photo est disponible pour d’autres applications sur l’appareil. Les étapes nécessaires à l’arrêt du flux d’aperçu sont les suivantes :

-   Appelez [**StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622) pour arrêter le flux d’aperçu.
-   Définissez la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280) de **CaptureElement** sur null.
-   Appelez la méthode [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858) de l’objet **MediaCapture** pour libérer l’objet.
-   Définissez la variable membre **MediaCapture** sur null.

[!code-cs[CleanupCameraAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

Vous devez arrêter le flux d’aperçu lorsque l’utilisateur quitte votre page en remplaçant la méthode [**OnNavigatedFrom**](https://msdn.microsoft.com/library/windows/apps/br227507).

[!code-cs[OnNavigatedFrom](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

Vous devez également arrêter le flux d’aperçu correctement si votre application est interrompue. Pour ce faire, inscrivez un gestionnaire pour l’événement [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) dans le constructeur de votre page.

[!code-cs[RegisterSuspending](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterSuspending)]

Dans le gestionnaire d’événements **Suspending**, vérifiez que la page est affichée par le [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) de l’application en comparant le type de page à la propriété [**CurrentSourcePageType**](https://msdn.microsoft.com/library/windows/apps/hh702390). Si la page n’est pas affichée, l’événement **OnNavigatedFrom** doit déjà avoir été déclenché et le flux d’aperçu arrêté. Si la page est affichée, obtenez un objet [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) à partir des arguments d’événement transmis au gestionnaire pour vous assurer que le système n’interrompt pas votre application avant l’arrêt du flux d’aperçu. Après l’arrêt du flux, appelez la méthode [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) de report afin que le système continue d’interrompre votre application.

[!code-cs[SuspendingHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSuspendingHandler)]

## Capturer une image fixe à partir du flux d’aperçu

Il est très facile d’obtenir une image fixe à partir du flux d’aperçu de capture multimédia sous la forme d’un [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358). Pour plus d’informations, voir [Obtenir une image d’aperçu](get-a-preview-frame.md).

## Rubriques connexes

* [Capturer des photos et des vidéos à l’aide de MediaCapture](capture-photos-and-video-with-mediacapture.md)
* [Obtenir une image d’aperçu](get-a-preview-frame.md)


<!--HONumber=Mar16_HO5-->


