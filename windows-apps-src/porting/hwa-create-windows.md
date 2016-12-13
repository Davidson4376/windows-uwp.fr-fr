---
author: seksenov
title: "Apps web hébergées - Convertir une app web en app Windows avec Visual Studio"
description: Utilisez Visual Studio pour transformer votre site web en application de plateforme Windows universelle (UWP) pour Windows&nbsp;10.
kw: Hosted Web Apps tutorial, Porting to Windows 10 with Visual Studio, How to convert website to Windows, How to add website to Windows Store, Packaging web application for Microsoft Store, Test Windows 10 native features and runtime APIs with CodePen, How to use Windows Cortana Live Tiles Built-in Camera on my Website with remote JavaScript
translationtype: Human Translation
ms.sourcegitcommit: 9dc441422637fe6984f0ab0f036b2dfba7d61ec7
ms.openlocfilehash: 1cb4757896c9fecf05224a26949cd6e2ddffdb45

---

# <a name="convert-your-web-application-to-a-universal-windows-platform-uwp-app"></a>Convertir votre application&nbsp;web en une application UWP (plateforme Windows universelle)

Découvrez comment créer rapidement une application de plateforme Windows universelle pour Windows&nbsp;10 à partir d’une simple URL de site web. 

> [!NOTE]
> Les instructions suivantes sont à utiliser avec une plateforme de développement Windows. Les utilisateurs de Mac peuvent consulter la page relative aux [instructions sur l’utilisation d’une plateforme de développement Mac](/hwa-create-mac.md).

## <a name="what-you-need-to-develop-on-windows"></a>Éléments nécessaires au développement sur Windows

- [Visual&nbsp;Studio&nbsp;2015.](https://www.visualstudio.com/) Visual&nbsp;Studio&nbsp;Community&nbsp;2015, complet et gratuit, inclut les outils de développement Windows 10, des modèles d’applications universelles, un éditeur de code, un puissant débogueur, des émulateurs Windows&nbsp;Mobile, une prise en charge linguistique enrichie, et bien plus encore. Tous ces éléments sont prêts pour une utilisation en production.
- [Kit de développement logiciel (SDK) Windows autonome pour Windows&nbsp;10](https://dev.windows.com/downloads/windows-10-sdk) (en option). Si vous utilisez un environnement de développement autre que Visual&nbsp;Studio&nbsp;2015, vous pouvez télécharger un SDK Windows autonome pour le programme d’installation de Windows&nbsp;10. Notez que vous n’avez pas besoin d’installer ce SDK si vous utilisez Visual&nbsp;Studio&nbsp;2015, car ce logiciel l’inclut déjà.

## <a name="step-1-pick-a-website-url"></a>Étape 1&nbsp;: Sélectionner une URL de site web
Choisissez un site web existant qui fonctionnera de manière optimale en tant qu’application d’une seule page. Nous vous recommandons vivement d’être le propriétaire ou le développeur du site. Ainsi, vous serez en mesure d’effectuer toutes les modifications nécessaires. Si aucune URL ne vous vient à l’esprit, essayez d’utiliser cet [exemple Codepen](http://codepen.io/seksenov/pen/wBbVyb/?editors=101) en tant que site web. Copiez votre URL, ou l’URL Codepen, à utiliser tout au long de ce didacticiel. 

![Étape&nbsp;1&nbsp;: Sélectionner une URL de site web](images/hwa-to-uwp/windows_step1.png)

## <a name="step-2-create-a-blank-javascript-app"></a>Étape&nbsp;2&nbsp;: Créer une application JavaScript vide

Ouvrez Visual Studio.
1. Cliquez sur **Fichier**.
2. Cliquez sur **Nouveau projet**.
3. Sous **JavaScript**, puis **Windows universel**, cliquez sur **Application vide (Windows universel)**.

![Étape&nbsp;2&nbsp;: Créer une application JavaScript vide](images/hwa-to-uwp/windows_step2.png)

## <a name="step-3-delete-any-packaged-code"></a>Étape&nbsp;3&nbsp;: Supprimer tout code empaqueté

Étant donné qu’il s’agit d’une application web hébergée, où le contenu est servi à partir d’un serveur distant, vous n’aurez pas besoin de la plupart des fichiers d’application locaux qui accompagnent le modèle JavaScript par défaut. Supprimez les ressources locales HTML, JavaScript ou CSS. Seul doit rester le fichier `package.appxmanifest`, où vous configurez l’application et les ressources de type image.

![Étape&nbsp;3&nbsp;: Supprimer tout code empaqueté](images/hwa-to-uwp/windows_step3.png)

## <a name="step-4-set-the-start-page-url"></a>Étape&nbsp;4&nbsp;: Définir l’URL de la page de démarrage

1. Ouvrez le fichier `package.appxmanifest`.
2. Sous l’onglet **Application**, recherchez le champ de texte **Page de démarrage**.
3. Remplacez `default.html` par l’URL de votre site web.

![Étape&nbsp;4&nbsp;: Définir l’URL de la page de démarrage](images/hwa-to-uwp/windows_step4.png)

## <a name="step-5-define-the-boundaries-of-your-web-app"></a>Étape&nbsp;5&nbsp;: Définir les limites de votre application web

Les règles URI de contenu de l’application (ACUR) spécifient les URL distantes autorisées à accéder à votre application et aux API Windows universelles. Vous devrez ajouter au minimum une règle ACUR pour votre page de démarrage et toutes les ressources web utilisées par cette page. Pour plus d’informations sur les règles ACUR, [cliquez ici](./hwa-access-features.md).
1. Ouvrez le fichier `package.appxmanifest`.
2. Cliquez sur l’onglet **URI de contenu**.
3. Ajoutez tout URI nécessaire pour votre page de démarrage.

Par exemple&nbsp;:
```
1. http://codepen.io/seksenov/pen/wBbVyb/?editors=101
2. http://*.codepen.io/
```
4. Définissez **l’accès WinRT** sur **Tous** pour chaque URI que vous avez ajouté.

![Étape&nbsp;5&nbsp;: Définir les limites de votre application web](images/hwa-to-uwp/windows_step5.png)

## <a name="step-6-run-your-app"></a>Étape&nbsp;6&nbsp;: Exécuter votre application

À ce stade, votre application Windows&nbsp;10 est entièrement opérationnelle et capable d’accéder aux API Windows universelles&nbsp;!

Si vous continuez avec notre exemple Codepen, cliquez sur le bouton **Notification toast** pour appeler une API Windows à partir du script hébergé.

![Étape&nbsp;6&nbsp;: Exécuter votre application](images/hwa-to-uwp/windows_step6.png)

## <a name="bonus-add-camera-capture"></a>Bonus&nbsp;: Ajouter la capture vidéo

Copiez et collez le code JavaScript ci-dessous pour activer la capture vidéo. Si vous continuez avec votre propre site web, créez un bouton pour appeler la méthode `cameraCapture()`. Si vous continuez avec notre exemple Codepen, un bouton est déjà présent dans le code HTML. Cliquez sur le bouton et prenez une photo.

```JavaScript
function cameraCapture() {
  if(typeof Windows != 'undefined') {
   var captureUI = new Windows.Media.Capture.CameraCaptureUI();
   //Set the format of the picture that's going to be captured (.png, .jpg, ...)
   captureUI.photoSettings.format = Windows.Media.Capture.CameraCaptureUIPhotoFormat.png;
   //Pop up the camera UI to take a picture
   captureUI.captureFileAsync(Windows.Media.Capture.CameraCaptureUIMode.photo).then(function (capturedItem) {
      // Do something with the picture
   });
  }
}
```

## <a name="related-topics"></a>Rubriques connexes

- [Améliorer votre application&nbsp;web en accédant aux fonctionnalités de plateforme Windows universelle (UWP)](hwa-access-features.md)
- [Guide des applications de plateforme Windows universelle (UWP)](http://go.microsoft.com/fwlink/p/?LinkID=397871)
- [Télécharger des ressources de conception pour les applications du Windows&nbsp;Store](https://msdn.microsoft.com/library/windows/apps/xaml/bg125377.aspx)



<!--HONumber=Dec16_HO1-->


