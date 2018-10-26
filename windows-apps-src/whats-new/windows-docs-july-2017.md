---
author: QuinnRadich
title: Nouveautés apportées dans la documentation Windows en juillet2017 - Développer des applicationsUWP
description: De nouvelles fonctionnalités, des exemples et des conseils aux développeurs ont été ajoutés à la documentation du développeur Windows10 en juillet2017.
keywords: nouveautés, mise à jour, fonctionnalités, conseils aux développeurs, Windows10
ms.author: quradic
ms.date: 07/05/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 62afbef1cc1f47bbc88c45a166572deca28d47a4
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5665535"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2017"></a>Nouveautés apportées dans la documentation du développeur Windows en juillet2017

La documentation du développeur Windows est constamment mise à jour afin d'intégrer les informations sur les nouvelles fonctionnalités disponibles pour les développeurs sur la plateforme Windows. Les présentations de fonctionnalités, les conseils aux développeurs et les vidéos qui suivent ont été récemment mis à disposition. Ils contiennent des informations nouvelles et mises à jour destinées aux développeurs Windows.

[Installez les outils et le kit de développement logiciel (SDK)](http://go.microsoft.com/fwlink/?LinkId=821431) sur Windows10 et vous pourrez ainsi [créer une application universelle Windows](../get-started/your-first-app.md) ou découvrir comment utiliser votre [code d’application existant sur Windows](../porting/index.md).

## <a name="features"></a>Fonctionnalités

### <a name="fluent-design"></a>Fluent Design

Disponibles pour les [WindowsInsiders](https://insider.windows.com/) dans les versions d'évaluation du Kit de développement logiciel (SDK), ces nouveaux effets utilisent la profondeur, la perspective et le mouvement pour aider les utilisateurs à se concentrer sur les éléments d’interface utilisateur importants.

[Matière acrylique](../design/style/acrylic.md) est un type de pinceau qui crée des textures transparentes. 

![Acrylique en thème clair](../design/style/images/Acrylic_DarkTheme_Base.png)

L’[effet de parallaxe](../design/motion/parallax.md) ajoute une profondeur en 3dimensions ainsi que de la perspective à votre application.

![Un exemple de parallaxe avec une liste et une image d’arrière-plan](../design/style/images/_Parallax_v2.gif)

L’[effet de révélation](../design/style/reveal.md) met en évidence les éléments importants de votre application. 

![Visuel de l’effet de révélation](../design/style/images/Nav_Reveal_Animation.gif)

### <a name="ui-controls"></a>Contrôles d’interface utilisateur

Disponibles pour les [WindowsInsiders](https://insider.windows.com/) dans les versions d’évaluation du Kit de développement logiciel (SDK), ces nouveaux contrôles facilitent la création rapide d’une interface utilisateur esthétique.

Le [contrôle du sélecteur de couleurs](../design/controls-and-patterns/color-picker.md) permet aux utilisateurs de parcourir et de sélectionner les couleurs.  

![Un sélecteur de couleur par défaut](../design/controls-and-patterns/images/color-picker-default.png)

Le [contrôle d’affichage de la navigation](../design/controls-and-patterns/navigationview.md) facilite l’ajout de navigation de niveau supérieur à votre application.

![Sections NavigationView](../design/controls-and-patterns/images/navview_sections.png)

Le [contrôle de la photo de la personne](../design/controls-and-patterns/person-picture.md) affiche l’image d’avatar d’une personne.

![Contrôle de la photo de la personne](../design/controls-and-patterns/images/person-picture/person-picture_hero.png)

Le [contrôle des évaluations](../design/controls-and-patterns/rating.md) permet aux utilisateurs de facilement visualiser et définir des évaluations qui reflètent le degré de satisfaction vis-à-vis du contenu et des services.

![Exemple de contrôle des évaluations](../design/controls-and-patterns/images/rating_rs2_doc_ratings_intro.png)

### <a name="design-toolkits"></a>Kits de ressources de conception

Les [kits de ressources et ressources de conception pour les applications UWP](../design/downloads/index.md) ont été étendus avec l’ajout des kits de ressources Sketch et AdobeXD. Les kits de ressources existants ont également été mis à jour et repensés, afin de fournir des contrôles plus robustes et des modèles de disposition pour vos applications UWP.

### <a name="dashboard-monetization-and-store-services"></a>Tableau de bord, monétisation et services du WindowsStore

Les nouvelles fonctionnalités suivantes sont désormais disponibles:

* Le SDKMicrosoftAdvertising vous permet désormais d’afficher les [publicités natives](../monetize/native-ads.md) dans vos applications. Une publicité native est un format de publicité basée sur un composant où chaque élément de l'annonce publicitaire (par exemple, le titre, l’image, la description et le texte de l’appel à l’action) est fourni à votre application sous forme d'élément individuel. Les publicités natives ne sont aujourd'hui disponibles que pour les développeurs qui rejoignent un programme pilote, mais nous envisageons de les rendre disponibles pour tous les développeurs bientôt.

* L'[API d'analyse du MicrosoftStore](../monetize/access-analytics-data-using-windows-store-services.md) fournit désormais une méthode que vous pouvez utiliser pour [télécharger le fichier CAB pour une erreur dans votre application](../monetize/download-the-cab-file-for-an-error-in-your-app.md).

* Les [offres ciblées](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md) vous permettent de cibler des segments de clients spécifiques avec un contenu attractif et personnalisé pour augmenter l’engagement, la rétention et la monétisation. 

* La description de votre application dans le WindowsStore peut désormais inclure des [bandes-annonces vidéo](../publish/app-screenshots-and-images.md#trailers).

* De nouvelles options de tarification et de disponibilité vous permettent de [planifier des changements de prix](../publish/set-and-schedule-app-pricing.md) et de [définir les dates de lancement précise](..//publish/configure-precise-release-scheduling.md).

* Vous pouvez [importer et exporter des descriptions du WindowsStore](../publish/import-and-export-store-listings.md) pour effectuer des mises à jour plus rapides, en particulier si vous avez des descriptions dans de nombreuses langues.

### <a name="my-people"></a>Mes Contacts

Disponible pour les [WindowsInsiders](https://insider.windows.com/) dans les versions d’évaluation du Kit de développement logiciel (SDK), la future fonctionnalité Mes Contacts permet aux utilisateurs d'épingler des contacts à partir d’une application directement dans leur barre des tâches. [Découvrez comment ajouter la prise en charge Mes Contacts à votre application.](../contacts-and-calendar/my-people-support.md)

![Volet de contact Mes Contacts](images/my-people.png)

[Partage de Mes Contacts](../contacts-and-calendar/my-people-sharing.md) permet aux utilisateurs de partager des fichiers par le biais de votre application, directement depuis la barre des tâches.

![Partage de Mes Contacts](images/my-people-sharing.png)

[Notifications de Mes Contacts](../contacts-and-calendar/my-people-support.md) est un nouveau type de notification toast que les utilisateurs peuvent envoyer à leurs contacts épinglés.

![Notification de Mes Contacts](images/my-people-notification.png)

### <a name="pin-to-taskbar"></a>Épingler à la barre des tâches

Disponible pour les [WindowsInsiders](https://insider.windows.com/) dans les versions d’évaluation du Kit de développement logiciel (SDK), la nouvelle classe TaskbarManager vous permet de demander à votre utilisateur d'[épingler votre application à la barre des tâches](../design/shell/pin-to-taskbar.md).

## <a name="developer-guidance"></a>Conseils aux développeurs

### <a name="media-playback"></a>Lecture de contenu multimédia

De nouvelles sections ont été ajoutées à l’article relatif à la lecture de contenu multimédia de base, [Lire du contenu audio et vidéo avec MediaPlayer](../audio-video-camera/play-audio-and-video-with-mediaplayer.md). La section [Lire du contenu vidéo sphérique avec MediaPlayer](../audio-video-camera/play-audio-and-video-with-mediaplayer.md) vous explique comment lire des vidéos encodées de façon sphérique, y compris ajuster le champ de vue et l'orientation de vue pour les formats pris en charge. La section [Utiliser MediaPlayer en mode serveur d’images](../audio-video-camera/play-audio-and-video-with-mediaplayer.md#use-mediaplayer-in-frame-server-mode) vous indique comment copier des images à partir du média lu avec [MediaPlayer](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) sur une surface Direct3D. Cela permet des scénarios tels que l’application d’effets en temps réel avec des nuanceurs de pixels. L’exemple de code montre une implémentation rapide d’un effet de flou pour la lecture vidéo à l’aide de Win2D.

### <a name="media-capture"></a>Capture multimédia

L’article [Traiter des images multimédias avec MediaFrameReader](../audio-video-camera/process-media-frames-with-mediaframereader.md) a été mis à jour pour présenter l’utilisation de la nouvelle classe [MultiSourceMediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader), qui vous permet d'obtenir des images corrélées dans le temps à partir de plusieurs sources multimédias. Cela est utile si vous devez traiter des images provenant de différentes sources, telles qu'un appareil photo couleur et de profondeur, et que vous devez vérifier que les images de chaque source ont été capturées dans un intervalle proche. Pour plus d'informations, voir [Utiliser MultiSourceMediaFrameReader pour obtenir des images corrélées dans le temps à partir de plusieurs sources](../audio-video-camera/process-media-frames-with-mediaframereader.md#use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources).

### <a name="scoped-search"></a>Étendue de recherche

Une étendue «UWP» a été ajoutée à la [documentation conceptuelle UWP](../get-started/universal-application-platform-guide.md) et aux [Informations de référence sur les API](https://docs.microsoft.com/en-us/uwp/api/) sur docs.microsoft.com. Sauf si cette étendue est désactivée, les recherches effectuées dans ces domaines retourneront uniquement des documents UWP.

![Étendue de recherche](images/scoped-search.png)

### <a name="test-your-windows-app-for-windows-10-s"></a>Tester votre application Windows pour Windows10S

Testez votre application Windows afin de vous assurer qu’elle fonctionnera correctement sur les appareils qui exécutent WindowsS. Utilisez [ce nouveau guide](../porting/desktop-to-uwp-test-windows-s.md) pour savoir comment procéder. 

## <a name="samples"></a>Exemples

### <a name="annotated-audio-app-sample"></a>Exemple d’application de lecture audio annotée

[Un exemple de mini-application qui illustre des scénarios de lecture audio, d’entrée manuscrite et d’itinérance de données OneDrive.](https://github.com/Microsoft/Windows-appsample-annotated-audio). Cet exemple enregistre du son tout en autorisant la capture synchronisée d'annotations manuscrites, afin que vous puissiez vous rappeler ensuite de ce qui a été dit au moment de la prise de note.

![Capture d’écran de l'exemple de lecture audio annotée](images/Playback.png)  

### <a name="shopping-app-sample"></a>Exemple d’application d’achat

[Une mini-application qui présente une expérience d'achat de base où un utilisateur peut acheter des emoji](https://github.com/Microsoft/Windows-appsample-shopping). Cette application montre comment utiliser les [API de demande de paiement](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments) pour implémenter l’expérience de validation d’achat.

![Capture d’écran de l’exemple d’application d’achat](images/shoppingcart.png)  

## <a name="videos"></a>Vidéos

### <a name="accessibility"></a>Accessibilité

L’accessibilité permet de rendre vos applications disponibles à un public plus large. [Regardez la vidéo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility), puis découvrez plus en détail le [Développement d’applications pour l’accessibilité](https://developer.microsoft.com/en-us/windows/accessible-apps).

### <a name="payments-request-api"></a>API de demande de paiement

L’API de demande de paiement permet aux clients et aux vendeurs de terminer le processus de validation d’achat en ligne en toute transparence. [Regardez la vidéo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-the-Payments-Request-API), puis explorez la [documentation relative à l'API de demande de paiement](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-the-Payments-Request-API).

### <a name="windows-10-iot-core"></a>Windows10IoTStandard

Grâce à Windows10IoTStandard et à la plateforme Windows universelle, vous pouvez rapidement concevoir des prototypes et créer des projets en connectant des systèmes de vision et divers composants, comme cette chatière avec reconnaissance de l'animal. [Regardez la vidéo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Building-a-Pet-Recognition-Door-Using-Windows-10-IoT-Core), puis apprenez-en davantage sur la [prise en main de Windows10IoTStandard](https://developer.microsoft.com/en-us/windows/iot).