---
title: Bonnes pratiques pour Xbox
description: Découvrez comment optimiser votre application pour Xbox.
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3c1a67936a403edc718ca5a38e7bcb8af02e042f
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372083"
---
# <a name="xbox-best-practices"></a>Bonnes pratiques pour Xbox

Par défaut, toutes les applications UWP sont exécutées sur Xbox One sans aucune action de votre part. Toutefois, si vous voulez que votre application sorte du lot, suscite l’enthousiasme de vos clients et compte parmi les meilleures expériences d’applications sur Xbox, vous devriez suivre les pratiques ci-dessous.
  > [!NOTE]
  > Avant de commencer, lisez les recommandations de conception présentées dans [Conception pour Xbox et télévision](../design/devices/designing-for-tv.md).   

## <a name="to-build-the-best-experiences-for-xbox-one"></a>Pour créer des expériences optimales sur Xbox One

### <a name="do-turn-off-mouse-mode"></a>*Procédez comme :* Désactiver le mode de la souris

Les utilisateurs de Xbox adorent leurs contrôleurs. Pour optimiser pour l’entrée de contrôleur, [désactiver le mode de la souris](how-to-disable-mouse-mode.md) et activer la navigation directionnelle (également appelé [XY concentrer la navigation et interaction](../design/input/gamepad-and-remote-interactions.md#xy-focus-navigation-and-interaction)). Méfiez-vous des pièges de focus et de l’interface utilisateur sont inaccessible.

### <a name="do-draw-a-focus-rectangle-that-is-appropriate-for-a-10-foot-experience"></a>*Procédez comme :* Dessiner un rectangle de focus qui convient à une expérience de la fièvre 10

La plupart des utilisateurs Xbox sont assis dans leur salon face à leur télévision, donc gardez en tête que le rectangle de focus standard est difficile à voir à une distance de 3 mètres ( 10-foot). Pour être sûr que l’utilisateur voit toujours clairement l’élément d’interface utilisateur qui a le focus d’entrée, suivez les recommandations présentées dans [Visuel du focus](../design/input/gamepad-and-remote-interactions.md#focus-visual). En XAML, vous obtenez ce comportement sans rien faire lorsque votre application s’exécute sur Xbox. Par contre, vous devez utiliser un style CSS personnalisé pour les applications HTML.

### <a name="do-integrate-with-the-systemmediatransportcontrols-class"></a>*Procédez comme :* Intégrer avec la classe SystemMediaTransportControls

Les utilisateurs Xbox veulent pouvoir contrôler les applications multimédias avec la télécommande multimédia Xbox, Cortana (en particulier les commandes vocales « Lecture » et « Pause ») et Xbox SmartGlass. Pour bénéficier gratuitement de ces fonctionnalités, votre application doit utiliser la classe [SystemMediaTransportControls](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols), qui est automatiquement incluse dans les commandes multimédias de la Xbox. Si votre application utilise des commandes multimédias personnalisées, veillez à les intégrer à la classe **SystemMediaTransportControls** afin de fournir ces fonctionnalités à vos utilisateurs. Si vous créez une application de musique en arrière-plan, intégrez la classe **SystemMediaTransportControls** afin que les contrôles de musique en arrière-plan fonctionnent correctement dans l’onglet multitâche de la Xbox.

<!-- ### *Do:* Use adaptive UI to account for snapped apps
One of the unique features of Xbox One is that users can snap apps such as Cortana next to any other app, so your app should respond gracefully when it runs in *fill mode*. Implement [adaptive UI](../get-started/universal-application-platform-guide.md#design-adaptive-ui-with-adaptive-panels) and make sure to test your app during development by snapping an app next to it. -->

### <a name="consider-draw-to-the-edge-of-the-screen"></a>*Prenez en compte :* Dessiner sur le bord de l’écran

Sur de nombreux téléviseurs, les bords de l’affichage sont coupés. Tout le contenu important de votre application doit donc être affiché dans la [zone adaptée à l’écran de TV](../design/devices/designing-for-tv.md#tv-safe-area). UWP utilise le *surbalayage* pour maintenir le contenu dans la zone adaptée à l’écran de TV, mais ce comportement par défaut peut dessiner une bordure visible autour de votre application. Pour une expérience optimale, désactivez le comportement par défaut et suivez les instructions de l’article [Comment étirer l’IU vers le bord de l’écran](turn-off-overscan.md).
> [!IMPORTANT]
  > Si vous désactivez le surbalayage, vous devez vous-même vous assurer que les éléments et le texte interactifs demeurent dans la zone adaptée à l’écran de TV. 

### <a name="consider-use-tv-safe-colors"></a>*Prenez en compte :* Utiliser des couleurs TV-safe

Les écrans de télévision ne gèrent pas les intensités de couleurs extrêmes aussi bien que les écrans d’ordinateur. Évitez d’utiliser des couleurs trop intenses dans votre application. Celles-ci peuvent en effet produire un effet de bandes ou apparaître délavées. En outre, n’oubliez pas que les téléviseurs présentent des différences, donc les couleurs qui semblent parfaites sur *votre* téléviseur peuvent apparaître autrement chez vos utilisateurs. Lecture [couleurs](../design/devices/designing-for-tv.md#colors) pour comprendre comment rendre votre application l’aspect souhaité à tout le monde !

### <a name="remember-you-can-disable-scaling"></a>*N’oubliez pas :* Vous pouvez désactiver la mise à l’échelle

Les applications UWP sont automatiquement dimensionnées pour garantir la lisibilité des éléments d’interface utilisateur tels que les commandes et les polices sur tous les appareils. Les applications sont mises à une échelle de 200 % pour XAML et de 150 % pour les applications HTML. Si vous souhaitez contrôler davantage l’apparence de votre application sur Xbox, désactivez le facteur d’échelle par défaut et utilisez les dimensions en pixels réelles d’un téléviseur haute définition (1920 x 1080). Consultez les articles [Comment désactiver la mise à l’échelle](disable-scaling.md) et [Pixels effectifs et mise à l’échelle](../design/basics/design-and-ui-intro.md#effective-pixels-and-scaling) pour plus d’informations sur la personnalisation de votre application pour un rendu optimal sur Xbox.

Si vous souhaitez obtenir un aperçu de ces pratiques appliquées à une application UWP, regardez cette vidéo !
</br>
</br>
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Tailoring-your-UWP-app-for-Xbox/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="channel-9"></a>Channel 9

Les discussions suivantes sur [Channel 9](https://channel9.msdn.com/) sont une excellente source d’informations pour créer des applications sur Xbox :

- [Building Great Universal Windows Platform (UWP) Apps for Xbox](https://channel9.msdn.com/Events/Build/2016/B883)
- [Adapt Your App for Xbox One and TV](https://channel9.msdn.com/Events/Build/2016/T651-R1)
- [UWP Development 1: Building an Adaptive UI](https://channel9.msdn.com/Events/Build/2016/L724-R1)
- [Web Apps Beyond the Browser: Cross-Platform Meets Cross Device](https://channel9.msdn.com/Events/Build/2016/B888)

## <a name="app-dev-on-xbox"></a>Développement d’application sur Xbox

Le **développement d’application sur Xbox** événement est un excellent point de départ pour les développeurs qui découvrent la création d’applications sur Xbox.

* [Regarder les sessions enregistrées](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event#WatchNow)
* [Lisez les billets de blog](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event#BlogSeries)

## <a name="see-also"></a>Voir aussi

- [UWP sur Xbox One](index.md)
- [Conception pour Xbox et TV](../design/devices/designing-for-tv.md)
- [Applications web progressives pour Xbox One](https://docs.microsoft.com/en-us/microsoft-edge/progressive-web-apps/xbox-considerations)
