---
author: payzer
title: Bonnes pratiques pour Xbox
description: "Découvrez comment optimiser votre application pourXbox."
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: fb896ed67b79e97b504be0253081a4c29ee5fcc9
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2017
---
# <a name="xbox-best-practices"></a>Bonnes pratiques pour Xbox
Par défaut, toutes les applications UWP sont exécutées sur Xbox One sans aucune action de votre part. Toutefois, si vous voulez que votre application sorte du lot, suscite l’enthousiasme de vos clients et compte parmi les meilleures expériences d’applications sur Xbox, vous devriez suivre les pratiques ci-dessous.
  > [!NOTE]
  > Avant de commencer, lisez les recommandations de conception présentées dans [Conception pour Xbox et télévision](../input-and-devices/designing-for-tv.md).   


## <a name="to-build-the-best-experiences-for-xbox-one"></a>Pour créer des expériences optimales sur Xbox One

### <a name="do-turn-off-mouse-mode"></a>*À faire:* Désactiver le mode souris
Les utilisateurs Xbox adorent leur manette. Pour optimiser les entrées de la manette, [désactivez le mode souris](how-to-disable-mouse-mode.md) et activez la navigation directionnelle (également appelée [focus X-Y](../input-and-devices/designing-for-tv.md#xy-focus-navigation-and-interaction)). Faites attention aux interruptions du focus et à l’interface utilisateur inaccessible.

### <a name="do-draw-a-focus-rectangle-that-is-appropriate-for-a-10-foot-experience"></a>*À faire:* Tracer un rectangle de focus qui soit approprié à une expérience TV («10-foot experience»).
La plupart des utilisateurs Xbox sont assis dans leur salon face à leur télévision, donc gardez en tête que le rectangle de focus standard est difficile à voir à une distance de 3mètres ( 10-foot). Pour être sûr que l’utilisateur voit toujours clairement l’élément d’interface utilisateur qui a le focus d’entrée, suivez les recommandations présentées dans [Visuel du focus](../input-and-devices/designing-for-tv.md#focus-visual). En XAML, vous obtenez ce comportement sans rien faire lorsque votre application s’exécute sur Xbox. Par contre, vous devez utiliser un style CSS personnalisé pour les applications HTML.

### <a name="do-integrate-with-the-systemmediatransportcontrols-class"></a>*À faire:* Intégrer la classe SystemMediaTransportControls 
Les utilisateurs Xbox veulent pouvoir contrôler les applications multimédias avec la télécommande multimédia Xbox, Cortana (en particulier les commandes vocales «Lecture» et «Pause») et Xbox SmartGlass. Pour bénéficier gratuitement de ces fonctionnalités, votre application doit utiliser la classe [SystemMediaTransportControls](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.systemmediatransportcontrols.aspx), qui est automatiquement incluse dans les commandes multimédias de la Xbox. Si votre application utilise des commandes multimédias personnalisées, veillez à les intégrer à la classe **SystemMediaTransportControls** afin de fournir ces fonctionnalités à vos utilisateurs. Si vous créez une application de musique en arrière-plan, intégrez la classe **SystemMediaTransportControls** afin que les contrôles de musique en arrière-plan fonctionnent correctement dans l’onglet multitâche de la Xbox.

### <a name="do-use-adaptive-ui-to-account-for-snapped-apps"></a>*À faire:* Utiliser une interface utilisateur adaptative pour prendre en compte les applications ancrées
L’une des fonctionnalités uniques de Xbox One est que les utilisateurs peuvent ancrer des applications telles que Cortana à côté de toute autre application. Votre application doit donc répondre de façon fluide quand elle s’exécute en *mode remplissage*. Implémentez l’[interface utilisateur adaptative](../get-started/universal-application-platform-guide.md#design-adaptive-ui-with-adaptive-panels) et veillez à tester votre application au cours du développement en ancrant une autre application à côté d’elle.

### <a name="consider-draw-to-the-edge-of-the-screen"></a>*À envisager:* Étendre jusqu’au bord de l’écran
Sur de nombreux téléviseurs, les bords de l’affichage sont coupés. Tout le contenu important de votre application doit donc être affiché dans la [zone adaptée à l’écran de TV](../input-and-devices/designing-for-tv.md#tv-safe-area). UWP utilise le *surbalayage* pour maintenir le contenu dans la zone adaptée à l’écran de TV, mais ce comportement par défaut peut dessiner une bordure visible autour de votre application. Pour une expérience optimale, désactivez le comportement par défaut et suivez les instructions de l’article [Comment étirer l’IU vers le bord de l’écran](turn-off-overscan.md).
> [!IMPORTANT]
  > Si vous désactivez le surbalayage, vous devez vous-même vous assurer que les éléments et le texte interactifs demeurent dans la zone adaptée à l’écran de TV. 

### <a name="consider-use-tv-safe-colors"></a>*À envisager:* Utiliser des couleurs adaptées aux écrans de télévision 
Les écrans de télévision ne gèrent pas les intensités de couleurs extrêmes aussi bien que les écrans d’ordinateur. Évitez d’utiliser des couleurs trop intenses dans votre application. Celles-ci peuvent en effet produire un effet de bandes ou apparaître délavées. En outre, n’oubliez pas que les téléviseurs présentent des différences, donc les couleurs qui semblent parfaites sur *votre* téléviseur peuvent apparaître autrement chez vos utilisateurs. Lisez l’article [Couleurs TV](../input-and-devices/designing-for-tv.md#colors) pour comprendre comment satisfaire tout le monde avec votre application!

### <a name="remember-you-can-disable-scaling"></a>*Ne pas oublier:* Vous pouvez désactiver la mise à l’échelle
Les applications UWP sont automatiquement dimensionnées pour garantir la lisibilité des éléments d’interface utilisateur tels que les commandes et les polices sur tous les appareils. Les applications sont mises à une échelle de 200% pour XAML et de 150% pour les applications HTML. Si vous souhaitez contrôler davantage l’apparence de votre application sur Xbox, désactivez le facteur d’échelle par défaut et utilisez les dimensions en pixels réelles d’un téléviseur haute définition (1920 x 1080). Consultez les articles [Comment désactiver la mise à l’échelle](disable-scaling.md) et [Pixels effectifs et mise à l’échelle](../layout/design-and-ui-intro.md#effective-pixels-and-scaling) pour plus d’informations sur la personnalisation de votre application pour un rendu optimal sur Xbox.


Si vous souhaitez obtenir un aperçu de ces pratiques appliquées à une application UWP, regardez cette vidéo!
</br>
</br>
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Tailoring-your-UWP-app-for-Xbox/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>


## <a name="channel-9"></a>Channel 9
Les discussions suivantes sur [Channel 9](https://channel9.msdn.com/) sont une excellente source d’informations pour créer des applications sur Xbox:

- [Building Great Universal Windows Platform (UWP) Apps for Xbox](https://channel9.msdn.com/Events/Build/2016/B883)
- [Adapt Your App for Xbox One and TV](https://channel9.msdn.com/Events/Build/2016/T651-R1)
- [UWP Development 1: Building an Adaptive UI](https://channel9.msdn.com/Events/Build/2016/L724-R1)
- [Web Apps Beyond the Browser: Cross-Platform Meets Cross Device](https://channel9.msdn.com/Events/Build/2016/B888)


## <a name="see-also"></a>Voir également
- [UWP sur XboxOne](index.md)

