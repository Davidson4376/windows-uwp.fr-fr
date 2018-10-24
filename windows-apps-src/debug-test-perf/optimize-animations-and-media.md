---
author: jwmsft
ms.assetid: DE5B084C-DAC1-430B-A15B-5B3D5FB698F7
title: Optimiser les animations, les éléments multimédias et les images
description: Créez des applications de plateforme Windows universelle (UWP) avec des animations fluides, une fréquence d’images élevée et une capture/lecture multimédia hautement performante.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2eebb967a7bf11163dc2e0ba502b40495901b39b
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5476449"
---
# <a name="optimize-animations-media-and-images"></a>Optimiser les animations, les éléments multimédias et les images


Créez des applications de plateforme Windows universelle (UWP) avec des animations fluides, une fréquence d’images élevée et une capture/lecture multimédia hautement performante.

## <a name="make-animations-smooth"></a>Rendre les animations fluides

Un aspect clé des applications UWP est la fluidité des interactions. Cela suppose des entrées tactiles « du bout des doigts », des transitions et des animations fluides, ainsi que des petits mouvements fournissant un retour d’entrée. L’infrastructure XAML comporte un thread nommé « thread de composition » qui est dédié à la composition et à l’animation des éléments visuels d’une application. Le thread de composition étant indépendant du thread de l’interface utilisateur (celui qui exécute le code de l’infrastructure et le code de l’application), une application peut conserver une fréquence d’images continue et des animations fluides même si elle doit effectuer des passes de disposition et des calculs complexes. Cette section décrit comment utiliser le thread de composition pour que les animations d’une application restent le plus fluides possible. Pour plus d’informations sur les animations, voir [Vue d’ensemble des animations](https://msdn.microsoft.com/library/windows/apps/Mt187350). Pour en savoir plus sur l’amélioration de la réactivité d’une application pendant les opérations de calcul complexes, voir [Garantir la réactivité du thread de l’interface utilisateur](keep-the-ui-thread-responsive.md).

### <a name="use-independent-instead-of-dependent-animations"></a>Utiliser des animations indépendantes au lieu d’animations dépendantes

Les animations indépendantes peuvent être calculées du début à la fin lors de la création, car les modifications apportées à la propriété animée n’affectent pas le reste des objets d’une scène. Des animations indépendantes peuvent donc s’exécuter sur le thread de composition au lieu du thread d’interface utilisateur. Étant donné que le thread de composition est mis à jour à une fréquence régulière, ces animations restent toujours fluides.

Tous les types d’animations suivants sont indépendants :

-   Animations d’objet utilisant des images clés
-   Animations d’une durée nulle
-   Animations sur les propriétés [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/Hh759771) et [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/Hh759772)
-   Animations sur la propriété [**UIElement.Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)
-   Animations sur des propriétés de type [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) lors du ciblage de la sous-propriété [**SolidColorBrush.Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color)
-   Animations sur les propriétés [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) suivantes lors du ciblage de sous-propriétés des types de valeur de retour suivants :

    -   [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.rendertransform)
    -   [**Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d)
    -   [**Projection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.projection)
    -   [**Clip**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.clip)

Les animations dépendantes ont un impact sur la disposition. Leur calcul nécessite des données supplémentaires du thread de l’interface utilisateur. Les animations dépendantes incluent les modifications apportées à des propriétés telles que [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) et [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height). Par défaut, les animations dépendantes ne sont pas exécutées et nécessitent d’être activées par le développeur de l’application. Lorsqu’elles sont activées, ces animations s’exécutent avec fluidité tant que le thread de l’interface utilisateur reste actif. En revanche, elles commencent à se cadencer si l’infrastructure ou l’application exécute en même temps beaucoup d’autres tâches sur le thread d’interface utilisateur.

Dans l’infrastructure XAML, la quasi-totalité des animations sont indépendantes par défaut. Vous avez toutefois la possibilité de passer outre cette optimisation. Soyez particulièrement attentif aux situations suivantes :

-   Vous définissez la propriété [**EnableDependentAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210356) de manière à autoriser l’exécution d’une animation dépendante sur le thread de l’interface utilisateur. Convertissez ces animations en une version indépendante. Par exemple, animez les propriétés [**ScaleTransform.ScaleX**](https://msdn.microsoft.com/library/windows/apps/BR242946) et [**ScaleTransform.ScaleY**](https://msdn.microsoft.com/library/windows/apps/BR242948) d’un objet à la place de ses propriétés [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) et [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height). Mettez à l’échelle les objets tels que les images et le texte. L’infrastructure applique une mise à l’échelle bilinéaire uniquement si la propriété [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940) est animée. L’image et le texte seront rerastérisés à leur taille finale pour qu’ils s’affichent correctement.
-   Vous mettez à jour des images individuellement alors que celles-ci sont des animations dépendantes. C’est le cas, par exemple, des transformations qui sont appliquées dans le gestionnaire de l’événement [**CompositonTarget.Rendering**](https://msdn.microsoft.com/library/windows/apps/BR228127).
-   Vous exécutez une animation considérée comme indépendante dans un élément dont la propriété [**CacheMode**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.cachemode) a la valeur **BitmapCache**. Il s’agit en réalité d’une animation dépendante dans la mesure où le cache doit être rerastérisé pour chaque image.

### <a name="dont-animate-a-webview-or-mediaplayerelement"></a>Éviter d’animer un contrôle WebView ou un objet MediaPlayerElement

Le contenu Web d’un contrôle [**WebView**](https://msdn.microsoft.com/library/windows/apps/BR227702) n’est pas directement rendu par l’infrastructure XAML et sa composition avec le reste de la scène nécessite des manipulations supplémentaires. Ce supplément de travail s’ajoute lors de l’animation du contrôle sur l’écran et peut éventuellement provoquer des problèmes de synchronisation (par exemple, le contenu HTML risque de ne pas se déplacer de manière synchronisée avec le reste du contenu XAML de la page). Lorsque vous souhaitez animer un contrôle **WebView**, permutez-le avec un [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webviewbrush.aspx) pendant toute la durée de l’animation.

L’animation d’un objet [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) est également une mauvaise idée. Au-delà de l’impact négatif sur les performances, elle peut provoquer des dégradations ou d’autres artefacts sur le contenu vidéo qui est lu.

> **Remarque**  les recommandations de cet article **mediaplayerelement** s’appliquent également aux [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926). L’objet **MediaPlayerElement** étant uniquement disponible dans Windows10, version1607, si vous créez une application pour une version précédente de Windows, vous avez besoin d’utiliser l’objet **MediaElement**.

### <a name="use-infinite-animations-sparingly"></a>Utiliser avec parcimonie les animations infinies

La plupart des animations s’exécutent pendant un laps de temps déterminé, mais si la propriété [**Timeline.Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) d’une animation est définie à la valeur Forever, cette animation est alors susceptible de s’exécuter indéfiniment. Nous vous recommandons d’utiliser le moins possible d’animations infinies, car elles consomment des ressources processeur en continu et peuvent empêcher le passage du processeur en mode de faible consommation d’énergie ou dans un état d’inactivité, ce qui épuise plus rapidement la batterie.

L’ajout d’un gestionnaire pour l’événement [**CompositionTarget.Rendering**](https://msdn.microsoft.com/library/windows/apps/BR228127) revient à exécuter une animation infinie. Normalement, le thread de l’interface utilisateur n’est actif que si des tâches lui sont affectées, mais si un gestionnaire est ajouté pour cet événement, il doit exécuter chaque image. Supprimez le gestionnaire quand il n’y a plus de tâche à effectuer et réinscrivez-le selon les besoins.

### <a name="use-the-animation-library"></a>Utiliser la bibliothèque d’animations

L’espace de noms [**Windows.UI.Xaml.Media.Animation**](https://msdn.microsoft.com/library/windows/apps/BR243232) comprend une bibliothèque d’animations fluides à hautes performances qui offrent une apparence cohérente avec les autres animations Windows. Les classes pertinentes ont un nom qui contient « Theme » et sont décrites dans [Vue d’ensemble des animations](https://msdn.microsoft.com/library/windows/apps/Mt187350). Cette bibliothèque prend en charge de nombreuses situations courantes d’animation, comme l’animation du premier affichage de l’application et la création de transitions d’état et de contenu. Nous vous recommandons d’utiliser cette bibliothèque d’animations le plus souvent possible pour améliorer les performances et garantir une cohérence maximale pour l’interface utilisateur UWP.

> **Remarque**  la bibliothèque d’animations ne peut pas animer toutes les propriétés possibles. Dans le cas de scénarios XAML dans lesquels la bibliothèque d’animations ne s’applique pas, voir [Animations dans une table de montage séquentiel](https://msdn.microsoft.com/library/windows/apps/Mt187354).


### <a name="animate-compositetransform3d-properties-independently"></a>Animer indépendamment les propriétés CompositeTransform3D

Vous pouvez animer indépendamment chaque propriété d’une [**CompositeTransform3D**](https://msdn.microsoft.com/library/windows/apps/Dn914714), ce qui vous permet d’appliquer uniquement les animations dont vous avez besoin. Pour en savoir plus et voir des exemples, voir [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d). Pour en savoir plus sur l’animation des transformations, voir [Animations dans une table de montage séquentiel](https://msdn.microsoft.com/library/windows/apps/Mt187354) et [Animations par images clés et animations de fonctions d’accélération](https://msdn.microsoft.com/library/windows/apps/Mt187352).

## <a name="optimize-media-resources"></a>Optimiser les ressources multimédias

L’audio, la vidéo et les images sont des formes de contenu incontournables qui sont utilisées par la majorité des applications. Dans la mesure où les fréquences de captures multimédias s’intensifient et que le contenu en définition standard passe en haute définition, la quantité de ressources nécessaires pour stocker, décoder et lire le contenu s’accroît. L’infrastructure XAML repose sur les dernières fonctionnalités ajoutées aux moteurs multimédias UWP. Les applications reçoivent ainsi ces améliorations gratuitement. Voici quelques astuces supplémentaires à suivre pour tirer le meilleur parti du contenu multimédia dans votre application UWP.

### <a name="release-media-streams"></a>Libérer les flux multimédias

Les fichiers multimédias sont l’une des ressources les plus courantes, mais aussi les plus onéreuses, que les applications utilisent. Dans la mesure où les ressources des fichiers multimédias peuvent considérablement augmenter l’encombrement mémoire de votre application, vous devez vous souvenir de libérer le handle vers le média dès que l’application ne l’utilise plus.

Par exemple, si votre application utilise un objet [**RandomAccessStream**](/uwp/api/Windows.Storage.Streams.RandomAccessStream) ou [**IInputStream**](https://msdn.microsoft.com/library/windows/apps/BR241718), veillez à appeler la méthode close sur l’objet lorsque votre application ne l’utilise plus, de manière à libérer l’objet sous-jacent.

### <a name="display-full-screen-video-playback-when-possible"></a>Afficher la lecture vidéo en plein écran dès que possible

Dans les applications UWP, utilisez toujours la propriété [**IsFullWindow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.isfullwindow.aspx) sur l’objet [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) pour activer et désactiver le rendu de fenêtre entière. L’utilisation des optimisations au niveau système est ainsi garantie pendant la lecture multimédia.

L’infrastructure XAML permet d’optimiser l’affichage du contenu vidéo lorsque la vidéo est le seul élément rendu: l’expérience utilisateur fait alors appel à moins de puissance et offre un meilleur rendement en termes de fréquence d’images. Pour une lecture multimédia plus efficace, définissez la taille d’un objet **MediaPlayerElement** conformément à la largeur et la hauteur de l’écran et n’affichez aucun autre élément XAML.

Plusieurs raisons légitimes justifient la décision de superposer des éléments XAML sur un objet **MediaPlayerElement** qui occupe toute la largeur et la hauteur de l’écran, par exemple des sous-titres ou des contrôles de transport provisoires. Prenez soin de masquer ces éléments (définissez `Visibility="Collapsed"`) lorsqu’ils ne sont pas requis pour rétablir l’état le plus efficace de la lecture multimédia.

### <a name="display-deactivation-and-conserving-power"></a>Désactivation de l’affichage et économie d’énergie

Pour empêcher que l’affichage ne soit désactivé lorsque plus aucune action utilisateur n’est détectée (par exemple quand une application lit une vidéo), vous pouvez appeler [**DisplayRequest.RequestActive**](https://msdn.microsoft.com/library/windows/apps/BR241818).

Pour économiser l’énergie et prolonger la durée de vie de la batterie, vous devez appeler [**DisplayRequest.RequestRelease**](https://msdn.microsoft.com/library/windows/apps/BR241819) pour libérer la demande d’affichage dès qu’elle n’est plus nécessaire.

Voici quelques situations où vous devez libérer la demande d’affichage :

-   La lecture vidéo est suspendue, par exemple suite à une action de l’utilisateur, une mise en mémoire tampon ou un réglage en raison de la bande passante limitée.
-   La lecture s’arrête. Par exemple, la lecture de la vidéo ou la présentation est terminée.
-   Une erreur de lecture s’est produite. Il peut s’agir de problèmes de connectivité réseau ou d’un fichier endommagé.

### <a name="put-other-elements-to-the-side-of-embedded-video"></a>Placer d’autres éléments à côté de la vidéo incorporée

Les applications offrent souvent un affichage incorporé dans lequel la vidéo est lue au sein de la page. Vous avez vraisemblablement perdu l’optimisation en plein écran, car l’objet [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) n’est pas à la taille de la page et d’autres objets XAML ont été dessinés. Prenez garde à ne pas entrer dans ce mode de manière involontaire en dessinant une bordure autour d’un objet **MediaPlayerElement**.

Ne dessinez pas d’éléments XAML sur la vidéo lorsqu’elle est en mode incorporé. Dans le cas contraire, l’infrastructure doit fournir davantage d’effort pour composer la scène. Le placement de contrôles de transport sous un élément multimédia incorporé plutôt que sur la vidéo est un bon exemple d’optimisation dans cette situation. Dans cette image, la barre rouge correspond à un ensemble de contrôles de transport (lire, suspendre, arrêter, etc.).

![Objet MediaPlayerElement avec des éléments superposés](images/videowithoverlay.png)

Ne placez pas ces contrôles sur un média qui n’est pas affiché en plein écran. Positionnez plutôt les contrôles de transport quelque part à l’extérieur de la zone dans laquelle le média est rendu. Dans l’image suivante, les contrôles sont placés sous le média.

![Objet MediaPlayerElement avec des éléments voisins](images/videowithneighbors.png)

### <a name="delay-setting-the-source-for-a-mediaplayerelement"></a>Retarder la définition de la source d’un objet MediaPlayerElement

Les moteurs multimédias sont des objets coûteux et l’infrastructure XAML retarde le chargement des fichiers .dll et la création des objets volumineux le plus longtemps possible. L’objet [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) doit se charger de ce travail une fois que la source est définie via la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx). Lorsque ce réglage a lieu une fois que l’utilisateur est prêt à lire le média, la majorité du coût lié à l’objet **MediaPlayerElement** est retardée aussi longtemps que possible.

### <a name="set-mediaplayerelementpostersource"></a>Définir MediaPlayerElement.PosterSource

La définition de [**MediaPlayerElement.PosterSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.postersource.aspx) permet au code XAML de libérer certaines ressources du GPU qui auraient été utilisées d’une autre façon. Cette API permet à une application d’utiliser aussi peu de mémoire que possible.

### <a name="improve-media-scrubbing"></a>Améliorer le nettoyage des plateformes multimédias

Le nettoyage est une tâche délicate pour que les plateformes multimédias soient vraiment réactives. En général, les utilisateurs préfèrent s’acquitter de cette tâche en modifiant la valeur d’un curseur. Voici quelques conseils qui vous permettront de rendre le nettoyage aussi efficace que possible.

-   Mettez à jour la valeur de l’objet [**Slider**](https://msdn.microsoft.com/library/windows/apps/BR209614) en fonction d’un minuteur qui interroge la propriété [**Position**](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.position.aspx) sur l’élément [**MediaPlayerElement.MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx). Veillez à utiliser une fréquence de mise à jour raisonnable pour votre minuteur. La propriété **Position** est seulement mise à jour toutes les 250millisecondes lors de la lecture.
-   La taille de la fréquence d’incrémentation du l’objet Slider doit s’adapter à la longueur de la vidéo.
-   Abonnez-vous aux événements [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx), [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointermoved.aspx) et [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerreleased.aspx) sur l’objet Slider pour affecter à la propriété [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackrate.aspx) la valeur 0 lorsque l’utilisateur fait glisser le curseur.
-   Dans le gestionnaire d’événements [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerreleased.aspx), définissez manuellement la position du média sur la valeur de la position du curseur afin d’obtenir un alignement du curseur optimal lors du nettoyage.

### <a name="match-video-resolution-to-device-resolution"></a>Aligner la résolution vidéo sur celle de l’appareil

Le décodage vidéo occupe beaucoup de mémoire et de cycles de GPU. Pensez alors à choisir un format vidéo proche de la résolution dans laquelle il sera affiché. Il n’est pas justifié d’utiliser les ressources pour décoder une vidéo de 1080p si elle est susceptible d’être réduite à une échelle nettement inférieure. De nombreuses applications ne disposent pas de la même vidéo codée avec différentes résolutions. Toutefois, si cette option est disponible, optez pour un codage plus proche de la résolution du périphérique d’affichage.

### <a name="choose-recommended-formats"></a>Choisir des formats recommandés

La sélection du format de média peut s’avérer difficile et le format choisi dépend souvent des décisions prises au niveau de l’entreprise. Pour obtenir des performances optimales avec UPW, nous recommandons le format vidéo H.264 en tant que format principal, et AAC et MP3 en tant que formats audio préférés. Pour la lecture de fichiers en local, MP4 est le conteneur de fichiers préféré pour le contenu vidéo. Le décodage H.264 est accéléré sur le matériel vidéo le plus récent. À cet égard, bien que l’accélération matérielle pour le décodage VC-1 soit largement disponible sur de nombreux appareils vidéo commercialisés, l’accélération est, dans de nombreux cas, partielle (ou limitée au niveau IDCT), au lieu de permettre un déchargement total du matériel (comme avec le mode VLD).

Si vous contrôlez totalement le processus de génération de contenu vidéo, vous devez rechercher un bon équilibre entre l’efficacité de la compression et la structure GOP. Une taille GOP relativement plus petite avec des images B peut améliorer les performances dans le mode de recherche ou le mode « truqué ».

Si vous souhaitez insérer des effets audio brefs à faible latence, par exemple dans des jeux, utilisez des fichiers WAV avec des données PCM non compressées pour réduire la surcharge du traitement qui est courante pour les formats audio compressés.


## <a name="optimize-image-resources"></a>Optimiser les ressources de type image

### <a name="scale-images-to-the-appropriate-size"></a>Définir les images à la bonne taille

Les images sont capturées dans des résolutions très élevées, ce qui peut générer une consommation plus élevée du processeur de la part des applications lors du décodage des données image et une plus grosse consommation de mémoire après le chargement des images à partir du disque. Mais cela ne fait aucun sens de décoder et d’enregistrer une image en haute résolution en mémoire si elle doit être affichée uniquement dans une taille plus petite que sa taille d’origine. Créez plutôt une version de l’image à la taille correspondant à celle qui sera affichée à l’écran à l’aide des propriétés [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) et [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241).

À ne pas faire :

```xaml
<Image Source="ms-appx:///Assets/highresCar.jpg"
       Width="300" Height="200"/>    <!-- BAD CODE DO NOT USE.-->
```

À faire à la place :

```xaml
<Image>
    <Image.Source>
    <BitmapImage UriSource="ms-appx:///Assets/highresCar.jpg"
                 DecodePixelWidth="300" DecodePixelHeight="200"/>
    </Image.Source>
</Image>
```

Les unités pour [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) et [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) sont en pixels physiques par défaut. La propriété [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545) peut être utilisée pour modifier ce comportement : en définissant **DecodePixelType** sur **Logical**, la taille de décodage tient compte automatiquement du facteur d’échelle actuel du système, comme c’est le cas pour d’autres contenus XAML. Il est donc généralement approprié de définir **DecodePixelType** sur **Logical** si, par exemple, vous souhaitez que **DecodePixelWidth** et **DecodePixelHeight** correspondent aux propriétés de hauteur et de largeur du contrôle d’image dans lequel s’affichera l’image. Avec le comportement par défaut consistant à utiliser des pixels physiques, vous devez tenir compte du facteur d’échelle du système actuel et vous devez prendre en considération les notifications de modification de mise à l’échelle au cas où l’utilisateur modifie ses préférences d’affichage.

Si les paramètres DecodePixelWidth/Height sont explicitement définis plus grands que la taille de l’image affichée à l’écran, alors l’application utilisera inutilement de la mémoire supplémentaire (jusqu’à 4octets par pixel), ce qui devient rapidement coûteux pour les grandes images. L’image sera également réduite à l’aide d’une mise à l’échelle bilinéaire, ce qui risque de la faire apparaître floue pour les grands facteurs d’échelle.

Si les paramètres DecodePixelWidth/DecodePixelHeight sont explicitement définis plus petits que la taille de l’image affichée à l’écran, elle sera agrandie et risque d’apparaître pixelisée.

Dans certains cas, lorsqu’une taille de décodage appropriée ne peut pas être déterminée à l’avance, vous devez vous en remettre au décodage automatique à la taille adéquate qui tentera de décoder au mieux l’image à la taille appropriée si aucun paramètre DecodePixelWidth/DecodePixelHeight explicite n’est spécifié.

Il est conseillé de définir une taille de décodage explicite si vous connaissez la taille du contenu image à l’avance. Il est également recommandé de définir conjointement [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545) sur **Logical** si la taille de décodage fournie dépend des tailles d’autres éléments XAML. Par exemple, si vous définissez explicitement la taille du contenu avec Image.Width et Image.Height, vous pourriez définir DecodePixelType sur DecodePixelType.Logical pour utiliser les mêmes dimensions de pixels logiques comme contrôle d’image, puis utiliser explicitement BitmapImage.DecodePixelWidth et/ou BitmapImage.DecodePixelHeight pour contrôler la taille de l’image afin de parvenir éventuellement à économiser une grande capacité de mémoire.

Remarque : il convient de prendre en considération Image.Stretch lors de la détermination de la taille du contenu décodé.

### <a name="right-sized-decoding"></a>Décodage de taille adéquate

Dans le cas où vous ne définissez pas de taille de décodage explicite, XAML tentera d’économiser la mémoire au mieux par le décodage d’une image à la taille exacte de son affichage à l’écran en fonction de la disposition initiale de la page qui la contient. Il est conseillé de développer votre application de façon à utiliser cette fonctionnalité lorsque cela est possible. Cette fonctionnalité sera désactivée si l’une des conditions suivantes est remplie.

-   La [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235) est associée à l’arborescence XAML dynamique après la définition du contenu avec [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522) ou [**UriSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.urisource.aspx).
-   L’image est décodée à l’aide d’un décodage synchrone comme [**SetSource**](https://msdn.microsoft.com/library/windows/apps/BR243255).
-   L’image est masquée en définissant [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) sur 0 ou [**Visibility**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility) sur **Collapsed** sur l’élément d’image hôte, le pinceau, ou tout autre élément parent.
-   Le contrôle d’image ou le pinceau utilise un [**Stretch**](https://msdn.microsoft.com/library/windows/apps/BR242968) de **None**.
-   L’image est utilisée en tant que [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756).
-   `CacheMode="BitmapCache"` est défini sur l’élément image ou sur tout élément parent.
-   Le pinceau image est non rectangulaire (comme lorsqu’il est appliqué à une forme ou à du texte).

Dans les scénarios ci-dessus, la définition d’une taille de décodage explicite est la seule façon de réaliser des économies de mémoire.

Vous devez toujours associer une [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235) dans l’arborescence dynamique avant de définir la source. Dès lors qu’un élément ou pinceau image est spécifié dans le balisage, ce sera automatiquement le cas. Des exemples sont fournis ci-dessous sous le titre «Exemples d’arborescences dynamiques». Vous devez toujours éviter d’utiliser [**SetSource**](https://msdn.microsoft.com/library/windows/apps/BR243255) et utiliser plutôt [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522) lors de la définition d’une source de flux. Il est conseillé d’éviter de masquer du contenu image (soit avec zéro opacité ou avec une visibilité réduite) dans l’attente du déclenchement de l’événement [**ImageOpened**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.imageopened.aspx). L’exécution de cette opération est laissée à l’appréciation de chacun: vous ne pourrez pas profiter du décodage automatique à la bonne taille si vous l’exécutez. Si votre application doit masquer le contenu image initialement, alors elle doit également définir la taille de décodage explicitement si possible.

**Exemples d’arborescences dynamiques**

Exemple1 (correct): Uniform Resource Identifier (URI) spécifié dans le balisage.

```xaml
<Image x:Name="myImage" UriSource="Assets/cool-image.png"/>
```

Exemple2 balisage: URI spécifié dans le code-behind.

```xaml
<Image x:Name="myImage"/>
```

Exemple2 code-behind (correct): association de la BitmapImage à l’arborescence avant de définir son UriSource.

```csharp
var bitmapImage = new BitmapImage();
myImage.Source = bitmapImage;
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
```

Exemple 2 code-behind (incorrect): définition associer de la BitmapImage avant de le connecter à l’arborescence.

```csharp
var bitmapImage = new BitmapImage();
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
myImage.Source = bitmapImage;
```

### <a name="caching-optimizations"></a>Optimisations de la mise en cache

Les optimisations de la mise en cache sont appliquées pour les images qui utilisent [**UriSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.urisource.aspx) pour charger du contenu à partir d’un package d’application ou à partir du web. L’URI est utilisé pour identifier de manière unique le contenu sous-jacent, et en interne, l’infrastructure XAML n’aura pas à télécharger et décoder le contenu à plusieurs reprises. Il utilise à la place les ressources logicielles ou matérielles mises en cache pour afficher le contenu à plusieurs reprises.

Il existe une exception à cette optimisation dans le cas où l’image est affichée plusieurs fois dans différentes résolutions (qui peuvent être spécifiées explicitement ou via un décodage automatique à la taille adaptée). Chaque entrée du cache stocke également la résolution de l’image, et si XAML ne peut pas trouver d’URI source correspondant à la résolution requise, il décodera alors une nouvelle version à cette taille. Toutefois, il ne retéléchargera pas les données de l’image encodée.

Par conséquent, vous devez utiliser [**UriSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.urisource.aspx) pour l’intégration lors du chargement d’images à partir d’un package d’application et éviter d’utiliser un flux de fichier et [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522) lorsque ce n’est pas obligatoire.

### <a name="images-in-virtualized-panels-listview-for-instance"></a>Images dans les volets virtualisés (ListView, par exemple)

Si une image est supprimée de l’arborescence (soit explicitement par l’application, soit implicitement lorsqu’elle défile en dehors de l’affichage du fait de son intégration dans un volet virtualisé moderne), alors XAML optimisera l’utilisation de la mémoire en libérant les ressources matérielles de l’image dans la mesure où elles ne sont plus requises. La mémoire n’est pas libérée immédiatement, mais plutôt lors de la mise à jour de trame qui se produit une seconde après que l’élément image ne figure plus dans l’arborescence.

Par conséquent, vous devez veiller à utiliser des volets virtualisés modernes pour héberger des listes de contenus d’image.

### <a name="software-rasterized-images"></a>Images rastérisées par logiciel

Lorsqu’une image est utilisée pour un pinceau non rectangulaire ou pour une [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756), elle utilise un chemin de rastérisation logicielle qui ne mettra pas du tout les images à l’échelle. En outre, elle doit stocker une copie de l’image dans la mémoire logicielle et dans la mémoire matérielle. Par exemple, si une image est utilisée comme un pinceau pour une ellipse, l’image complète, potentiellement de grandes dimensions, sera stockée deux fois en interne. Lorsque vous utilisez **NineGrid** ou un pinceau non rectangulaire, alors votre application doit effectuer la mise à l’échelle préalable de ses images à leur taille de rendu approximativement.

### <a name="background-thread-image-loading"></a>Chargement d’images thread en arrière-plan

XAML a une optimisation interne qui lui permet de décoder le contenu d’une image en mode asynchrone sur une surface dans la mémoire matérielle sans nécessiter de surface intermédiaire dans la mémoire logicielle. Cela réduit l’utilisation maximale de la mémoire et la latence de rendu. Cette fonctionnalité sera désactivée si l’une des conditions suivantes est remplie.

-   L’image est utilisée en tant que [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756).
-   `CacheMode="BitmapCache"` est défini sur l’élément image ou sur tout élément parent.
-   Le pinceau image est non rectangulaire (comme lorsqu’il est appliqué à une forme ou à du texte).

### <a name="softwarebitmapsource"></a>SoftwareBitmapSource

La classe [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Dn997854) échange des images non compressées interopérables entre les différents espaces de noms WinRT, tels que [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/BR226176), les API de caméra et XAML. Cette classe évite une copie supplémentaire qui d’ordinaire serait nécessaire avec [**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/BR243259) et qui aide à réduire la mémoire maximale et la latence de la source à l’écran.

Le [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Dn887358) qui fournit les informations sources peut également être configuré pour utiliser un [**IWICBitmap**](https://msdn.microsoft.com/library/windows/desktop/Ee719675) personnalisé pour fournir un magasin de stockage rechargeable qui permet à l’application de remapper la mémoire comme il convient. Il s’agit d’un exemple d’utilisation avancée de C++.

Votre application doit utiliser [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Dn887358) et [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Dn997854) pour interopérer avec d’autres API WinRT qui produisent et utilisent des images. Et votre application doit utiliser **SoftwareBitmapSource** lors du chargement de données d’image non compressées au lieu d’utiliser [**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/BR243259).

### <a name="use-getthumbnailasync-for-thumbnails"></a>Utiliser GetThumbnailAsync pour les miniatures

Un exemple d’utilisation des images mises à l’échelle : la création des miniatures. Bien que vous puissiez utiliser [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) et [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) pour fournir des petites versions des images, UWP fournit des API plus efficaces pour récupérer les miniatures. [**GetThumbnailAsync**](https://msdn.microsoft.com/library/windows/apps/BR227210) fournit les miniatures pour les images dont le système de fichiers est déjà mis en cache. Vous obtenez ainsi une bien meilleure performance qu’avec les API XAML, car l’image n’a pas besoin d’être ouverte ou décodée.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> FileOpenPicker picker = new FileOpenPicker();
> picker.FileTypeFilter.Add(".bmp");
> picker.FileTypeFilter.Add(".jpg");
> picker.FileTypeFilter.Add(".jpeg");
> picker.FileTypeFilter.Add(".png");
> picker.SuggestedStartLocation = PickerLocationId.PicturesLibrary;
>
> StorageFile file = await picker.PickSingleFileAsync();
>
> StorageItemThumbnail fileThumbnail = await file.GetThumbnailAsync(ThumbnailMode.SingleItem, 64);
>
> BitmapImage bmp = new BitmapImage();
> bmp.SetSource(fileThumbnail);
>
> Image img = new Image();
> img.Source = bmp;
> ```
> ```vb
> Dim picker As New FileOpenPicker()
> picker.FileTypeFilter.Add(".bmp")
> picker.FileTypeFilter.Add(".jpg")
> picker.FileTypeFilter.Add(".jpeg")
> picker.FileTypeFilter.Add(".png")
> picker.SuggestedStartLocation = PickerLocationId.PicturesLibrary
>
> Dim file As StorageFile = Await picker.PickSingleFileAsync()
>
> Dim fileThumbnail As StorageItemThumbnail = Await file.GetThumbnailAsync(ThumbnailMode.SingleItem, 64)
>
> Dim bmp As New BitmapImage()
> bmp.SetSource(fileThumbnail)
>
> Dim img As New Image()
> img.Source = bmp
> ```

### <a name="decode-images-once"></a>Décoder une fois les images

Pour éviter de décoder les images plus d’une fois, assignez la propriété [**Image.Source**](https://msdn.microsoft.com/library/windows/apps/BR242760) à partir d’un Uri plutôt qu’à l’aide de flux de mémoire. L’infrastructure XAML peut associer le même URI dans plusieurs endroits à une seule image décodée, mais ne peut pas effectuer la même chose pour plusieurs flux de mémoire qui contiennent les mêmes données, et crée une image décodée différente pour chaque flux de mémoire.
