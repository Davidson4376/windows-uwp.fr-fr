---
author: jken
Description: Design your app so that it looks good and functions well in Mixed Reality.
title: Conception pour la réalité mixte
ms.assetid: ''
label: Designing for Mixed Reality
template: detail.hbs
isNew: true
keywords: réalité mixte, hololens, réalité augmentée, pointer du regard, voix, contrôleur
ms.author: jken
ms.date: 2/5/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: ''
doc-status: ''
ms.localizationpriority: medium
ms.openlocfilehash: e6bba6c22b7f0055a93bfd1826bc3a2acc0f2164
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7426793"
---
# <a name="designing-for-mixed-reality"></a>Conception pour la réalité mixte

Concevez une application qui s’affiche correctement dans la réalité mixte et tirez parti des nouvelles méthodes d’entrée.

## <a name="overview"></a>Vue d'ensemble

La [réalité mixte](https://developer.microsoft.com/windows/mixed-reality/mixed_reality) est le résultat de la fusion du monde physique avec le monde numérique. L’éventail des expériences de réalité mixte inclut à une extrémité les appareils de type HoloLens (appareil qui associe du contenu généré par ordinateur avec le monde réel), et à l’autre, une vue complètement immersive de réalité virtuelle (telle qu’affichée avec un casque WindowsMixedReality). Voir [Types d’applications de réalité mixte](https://developer.microsoft.com/en-us/windows/mixed-reality/types_of_mixed_reality_apps) pour des exemples illustrant les différentes expériences.

Presque toutes les applications UWP existantes s’exécuteront dans l’environnement de réalité mixte en tant qu’applications2D sans modification, même si l’expérience pour l’utilisateur peut être améliorée en suivant certaines des recommandations de cette rubrique.

![Vue de réalité mixte](images/MR-01.png)

Les casques HoloLens et WindowsMixedReality prennent en charge les applications qui s’exécutent sur la plateforme UWP et prennent en charge deux types distincts d’expériences. 

### <a name="2d-vs-immersive-experience"></a>2D et expérience immersive

Une application immersive remplace la totalité de l’affichage visible par l’utilisateur, en le plaçant au centre d’une vue créée par l’application. Par exemple, un jeu immersif peut placer l’utilisateur sur la surface d’une planète extraterrestre, ou une application servant de guide peut placer l’utilisateur dans un village d’Amérique du Sud. La création d’une application immersive nécessite des graphiques3D ou des vidéos stéréographiques capturées. Les applications immersives sont souvent développées à l’aide d’un moteur de jeu tiers comme Unity, ou avec DirectX.

Si vous créez des applications immersives, visitez le [Centre de développement WindowsMixedReality](https://developer.microsoft.com/windows/mixed-reality) pour plus d’informations.

Une application2D s’exécute en tant que fenêtre traditionnelle plate dans la vue de l’utilisateur. Sur HoloLens, cela signifie une vue épinglée au mur ou un point dans l’espace dans le salon ou le bureau réel de l’utilisateur. Dans un casque WindowsMixedReality, l’application est épinglée à un mur dans la [page d’accueil de réalité mixte](https://docs.microsoft.com/windows/mixed-reality/enthusiast-guide/your-mixed-reality-home) (parfois appelée *Maison sur la falaise*).

![Plusieurs applications s’exécutant en réalité mixte](images/MR-multiple.png)


Ces applications2D ne remplacent pas la vue entière: elles sont placées au sein de celle-ci. Plusieurs applications2D peuvent exister ensemble dans l’environnement.

Le reste de cette rubrique traite des considérations de conception pour l’expérience2D.

## <a name="launching-2d-apps"></a>Lancement des applications2D

![Menu Démarrer de la réalité mixte](images/MR-start-options.png)

Toutes les applications sont lancées à partir du Menu Démarrer, mais il est également possible de créer un objet3D qui agira comme un lanceur d’application. Voir [cette vidéo](https://www.youtube.com/watch?v=TxIslHsEXno) pour plus d’informations.

## <a name="the-2d-app-input-overview"></a>Vue d’ensemble des entrées pour les applications2D

Les claviers et les souris sont pris en charge sur les plates-formes HoloLens et de réalité mixte. Vous pouvez associer un clavier et une souris directement avec l’appareil HoloLens via Bluetooth. Les applications de réalité mixte prennent en charge la souris et le clavier connectés à l’ordinateur hôte. Les deux peuvent être utiles lorsqu’un contrôle très précis est nécessaire.

D’autres méthodes d’entrée, plus naturelles, sont également prises en charge et peuvent être particulièrement utiles lorsque l’utilisateur n’est pas assis à un bureau avec un clavier réel en face de lui, ou quand un contrôle précis est nécessaire.

Sans aucun matériel ou codage supplémentaire, les applications utiliseront le mode pointage du regard-le vecteur le long duquel regarde l’utilisateur-comme pointeur de souris lorsque vous travaillez avec des applications2D. Il est implémenté comme si un pointeur de souris passait sur un élément quelconque dans la scène virtuelle.

Dans une interaction classique, votre utilisateur regardera un contrôle dans votre application, ce qui le mettra en surbrillance. L’utilisateur déclenchera alors une action à l’aide d’un mouvement (sur HoloLens), d’un contrôleur ou d’une commande vocale. Si l’utilisateur sélectionne un champ d’entrée de texte, le clavier logiciel s’affiche. 


![Clavier contextuel en réalité mixte](images/MR-keyboard.png)

Il est important de noter que toutes ces interactions auront lieu automatiquement sans codage supplémentaire de votre part, en raison de l’exécution sur la plateforme UWP. Les entrées à partir du casque HoloLens ou de réalité mixte s’afficheront comme des entrées tactiles pour l’application2D. Cela signifie que de nombreuses applications UWP s’exécuteront et seront utilisables en réalité mixte, par défaut. 

Cela dit, avec un peu de travail supplémentaire, l’expérience peut être considérablement améliorée. Par exemple, le [contrôle vocal](https://developer.microsoft.com/windows/mixed-reality/voice_design) peut être particulièrement efficace. Les environnements HoloLens et de réalité mixte prennent en charge les commandes vocales pour lancer et interagir avec les applications, et même le support vocal semblera être une extension naturelle de cette approche. Voir [Interactions vocales]( https://docs.microsoft.com/windows/uwp/design/input/speech-interactions) pour plus d’informations sur l’ajout d’un support vocal à votre application UWP. 


### <a name="selecting-the-right-controller"></a>Sélectionner le bon contrôleur

![Contrôleurs de mouvement de réalité mixte](images/MR-controllers.png)

Plusieurs méthodes d’entrée inédites ont été conçues spécialement pour la réalité mixte, en particulier:

* [Mouvements de la main](https://developer.microsoft.com/windows/mixed-reality/gestures) (HoloLens uniquement, mais uniquement utilisé pour le lancement des applications2D)
* [Prise en charge de la manette de jeu](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories) (les deux environnements)
* [Appareils Clicker](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories) (HoloLens uniquement)
* [Contrôleurs de mouvement](https://developer.microsoft.com/windows/mixed-reality/motion_controllers) (appareils de réalité mixte uniquement, comme illustrés ci-dessus.)

Ces contrôleurs rendent l’interaction avec des objets virtuels naturelle et précise. Certaines de ces interactions sont gratuites. Par exemple, mouvement de sélection HoloLens ou en cliquant sur un déclencheur ou une clé de Windows du contrôleur de mouvement génère la réponse d’entrée que vous attendez, là encore, sans codage de votre part.

À d’autres moments, vous voudrez ajouter du code pour tirer parti des informations et entrées supplémentaires qui sont disponibles. Par exemple, les contrôleurs de mouvement peuvent servir à manipuler des objets avec un niveau précis de contrôle si vous écrivez du code qui prend en compte leur position et les pressions sur leurs boutons.

> [!NOTE]
> En résumé: le principe directeur doit être de toujours fournir à l’utilisateur une méthode d’entrée aussi naturelle et fluide que possible.


## <a name="2d-app-design-considerations-functionality"></a>Considérations relatives à la conception d’applications2D: fonctionnalités

Lors de la création d’une application UWP qui pourra être potentiellement utilisée sur une plateforme de réalité mixte, voici plusieurs points à prendre en considération.

* La fonction glisser-déplacer peut ne pas fonctionner correctement avec des contrôleurs de mouvement, des manettes de jeu ou des mouvements. Si votre application dépend fortement de la fonction glisser -déplacer, vous devrez fournir une autre méthode pour prendre en charge cette action, telle que la présentation d’une boîte de dialogue confirmant si des objets doivent être déplacés vers un nouvel emplacement.

* Soyez conscient de la façon dont les sons changent. Si votre application génère des effets sonores, le son proviendra de l’emplacement où est localisée votre application dans le monde virtuel. À mesure que l’utilisateur s’éloignera de l’application, le son diminuera. Voir [Son spatial](https://developer.microsoft.com/windows/mixed-reality/spatial_sound) pour plus d’informations.

* Tenez compte du champ de vision et fournissez des affordances. Tous les appareils n’offrent pas un champ de vision aussi étendu qu’un moniteur d’ordinateur. Voir [Image holographique](https://developer.microsoft.com/windows/mixed-reality/holographic_frame) pour plus de détails. En outre, l’utilisateur peut se trouver éloigné d’une application en cours d’exécution. Autrement dit, l’application peut apparaître épinglée au mur à un emplacement différent dans le monde (réel ou virtuel). Il faudra peut-être que votre application attire l’attention des utilisateurs, ou prenez en compte que la vue entière n'est pas visible en permanence. Les notifications toast sont disponibles, mais un autre moyen d’obtenir l’attention de l’utilisateur peut être de générer une alerte sonore ou [vocale](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/SpeechRecognitionAndSynthesis/cs/Scenario_SynthesizeText.xaml.cs).

* Une application2D reçoit automatiquement une [barre de l’application](https://developer.microsoft.com/windows/mixed-reality/app_bar_and_bounding_box) pour permettre à l’utilisateur de la déplacer et la mettre à l’échelle dans l’environnement virtuel. Les vues peuvent être redimensionnées verticalement ou en conservant les mêmes proportions.


## <a name="2d-app-design-considerations-uiux"></a>Considérations relatives à la conception d’applications2D: UI/UX

* Les contrôles XAML qui implémentent le [système Fluent Design](https://docs.microsoft.com/windows/uwp/design/fluent-design-system/) tels que l’[affichage de navigation](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview) et les effets comme [Acrylique](https://docs.microsoft.com/windows/uwp/design/style/acrylic) fonctionnent tous particulièrement bien dans les applications2D de réalité mixte.

* Testez la taille du texte et des fenêtres de votre application dans un appareil de réalité mixte, ou au moins dans le simulateur de réalité mixte. La taille de fenêtre par défaut de votre application sera de 853x480pixels effectifs. Utilisez des tailles de police supérieures (une taille de point d’environ 32 est recommandée) et lisez [Mise à jour de votre application universelle existante pour Hololens](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens). L’article [Typographie](https://developer.microsoft.com/windows/mixed-reality/typography) traite de ce sujet en détail. Lorsque vous travaillez dans Visual Studio, un paramètre d’éditeur de conception XAML pour une application2D HoloLens57" permet d’obtenir une vue avec une échelle et des dimensions correctes. 

![Le texte affiché dans les applications de réalité mixte doit être grand.](images/MR-text.png)

* [Votre regard est votre souris](https://developer.microsoft.com/windows/mixed-reality/gaze_targeting). Lorsque l’utilisateur regarde un élément, cela agit comme un événement **touch hover** (pointage tactile). De ce fait, le simple fait de regarder un objet peut déclencher une fenêtre contextuelle accidentelle ou toute autre interaction indésirable. Vous devrez peut-être détecter si l’application est en cours d’exécution dans la réalité mixte et modifier ce comportement. Voir **Prise en charge lors de l’exécution** ci-dessous. 

* Lorsqu’un utilisateur pointe du regard vers un élément ou pointe avec un contrôleur de mouvement, un événement **touch hover** est généré. Il s’agit d’un **PointerPoint** où **PointerType** est **Touch**, mais **IsInContact** est **false**. Lorsqu’une forme de validation se produit (comme une pression sur le boutonA de la manette de jeu, une pression sur un appareil Clicker, une pression sur la gâchette d’un contrôleur de mouvement ou une commande vocale «Sélectionner»), un événement **touch press** est généré, avec **PointerPoint** où **IsInContact** se change en **true**. Voir [Interactions tactiles](https://docs.microsoft.com/windows/uwp/design/input/touch-interactions) pour plus d’informations sur ces événements d’entrée.

* N’oubliez pas, pointer du regard n’est pas aussi précis que pointer avec la souris. Des cibles ou des boutons de souris plus petits peuvent provoquer de la frustration chez vos utilisateurs, de ce fait redimensionnez les contrôles en conséquence. S’ils sont conçus pour les entrées tactiles, ils fonctionneront dans la réalité mixte, mais vous pouvez décider d’agrandir certains boutons lors de l’exécution. Voir [Mise à jour de votre application universelle existante pour Hololens](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens).

* HoloLens définit la couleur noir comme l’absence de lumière. Il n’y a pas de rendu, ce qui permet au «monde réel» d’apparaître. Votre application ne doit pas utiliser de noir si cela peut prêter à confusion. Dans un casque de réalité mixte, le noir est noir.

* HoloLens ne gère pas les thèmes de couleur dans les applications et affiche par défaut du bleu pour garantir la meilleure expérience pour les utilisateurs. Pour plus d’informations sur la sélection des couleurs, vous devez consulter [cette rubrique](https://developer.microsoft.com/windows/mixed-reality/color,_light_and_materials) qui traite de l’utilisation de la couleur et des matières dans les conceptions de réalité mixte.


## <a name="other-points-to-consider"></a>Autres points à prendre en compte

* Même si le [Pont du bureau](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root) peut aider à importer des applications de bureau (Win32) existantes dans Windows10 et le MicrosoftStore, il ne peut pas créer des applications qui s’exécutent sur HoloLens ou dans la réalité mixte pour l’instant.




## <a name="runtime-support"></a>Prise en charge lors de l’exécution

Il est possible pour votre application de déterminer si elle s’exécute sur un appareil de réalité mixte lors de l’exécution, et utiliser cette information pour redimensionner les contrôles ou optimiser d’autres façons l’application pour une utilisation sur un casque.

Voici un morceau de code qui redimensionne le texte à l’intérieur d’un contrôle TextBlock XAML uniquement si l’application est utilisée sur un appareil de réalité mixte.

```csharp

bool isViewingInMR = Windows.ApplicationModel.Preview.Holographic.HolographicApplicationPreview.IsCurrentViewPresentedOnHolographicDisplay();

            if (isViewingInMR)
            {
                // Running on headset, resize the XAML text
                textBlock.Text = "I'm running in Mixed Reality!";
                textBlock.FontSize = 32;
            }
            else
            {
                // Running on desktop
                textBlock.Text = "I'm running on the desktop.";
                textBlock.FontSize = 16;
            }

```





## <a name="related-articles"></a>Articles associés


* [Limitations actuelles pour les applications utilisant des API à partir du shell](https://developer.microsoft.com/windows/mixed-reality/current_limitations_for_apps_using_apis_from_the_shell)
* [Création d’applications2D](https://developer.microsoft.com/windows/mixed-reality/building_2d_apps)
* [HoloLens: Création d’applications2D UWP pour MicrosoftHoloLens](https://channel9.msdn.com/Events/Build/2016/B854)
* [XAML conditionnel](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/conditional-xaml)


