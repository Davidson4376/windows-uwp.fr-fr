---
title: Qu’est-ce qu’une application de plateforme Windows universelle (UWP) ?
description: Découvrez les applications de plateforme Windows universelle (UWP) qui peuvent s’exécuter sur un large éventail d’appareils exécutant Windows 10.
ms.assetid: 59849197-B5C7-493C-8581-ADD6F5F8800B
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, uwp, universal
ms.localizationpriority: medium
ms.openlocfilehash: 1a43cdd5c16e4ab7ec254c263df75c182ce3faba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618174"
---
# <a name="whats-a-universal-windows-platform-uwp-app"></a>Qu’est-ce qu’une application de plateforme Windows universelle (UWP) ?

![Applications de plateforme de Windows universelle s’exécutent sur divers appareils, prennent en charge d’interface utilisateur adaptative, une entrée utilisateur naturelle, un magasin, partenaires et services cloud](images/universalapps-overview.png)

Description :

- Sécurisé : Les applications UWP déclarent les données auxquelles elles accèdent et les ressources de l’appareil. L’utilisateur doit autoriser cet accès.
- Possibilité d’utiliser une API commune sur tous les appareils qui exécutent Windows 10.
- En mesure d’utiliser des fonctionnalités spécifiques des appareils et adapter l’interface utilisateur pour les tailles d’écran de périphérique différent, résolutions et PPP.
- Disponible à partir du Microsoft Store sur tous les appareils (ou uniquement ceux que vous spécifiez) qui s’exécutent sur Windows 10. Microsoft Store offre plusieurs moyens de gagner de l’argent sur votre application.
- Possibilité d’installation et de désinstallation sans risque pour l’ordinateur et sans « détérioration » de celui-ci.
- Attrayante : possibilité d’utiliser des vignettes dynamiques, des notifications push et des activités des utilisateurs qui interagissent avec la Chronologie de Windows et la fonctionnalité Pick Up Where I Left Off de Cortana pour fidéliser les utilisateurs.
- Programmable en C#, C++, Visual Basic et Javascript. Pour l’interface utilisateur, possibilité d’utiliser DirectX, HTML ou XAML.

Examinons cela plus en détail.

## <a name="secure"></a>Sécurisée

Les applications UWP déclarent dans leur manifeste les fonctionnalités de l’appareil dont elles ont besoin, comme l’accès au microphone, l’emplacement, la Webcam, les périphériques USB, les fichiers et ainsi de suite. L’utilisateur doit confirmer et autoriser cet accès pour que la fonctionnalité soit accordée à l’application.

## <a name="a-common-api-surface-across-all-devices"></a>Surface d’API commune à tous les appareils

Windows 10 intègre la Universal Windows Platform (UWP), qui fournit une plateforme d’application commune sur chaque appareil qui exécute Windows 10. Les API au cœur d’UWP sont les mêmes pour toutes les familles d’appareils Windows. Si votre application utilise uniquement l’API de base, il sera exécuté sur n’importe quel appareil Windows 10, peu importe que vous ciblez un PC de bureau, Xbox, casque de réalité mixte et ainsi de suite.

Une application UWP ou un composant Windows Runtime écrits en C++ /WinRT ou C++ /CX ont accès aux API Win32 faisant partie de l’UWP. Ces API Win32 sont implémentées par tous les appareils Windows 10.

## <a name="extension-sdks-expose-the-unique-capabilities-of-specific-device-types"></a>Les kits de développement logiciel (SDK) d’extension exposent les fonctionnalités uniques des types spécifiques d’appareils

Si vous ciblez des API universelles, votre application peut être lancée sur tous les appareils exécutant Windows 10. Si vous souhaitez que votre application UWP tire parti d’API de périphériques spécifiques, c’est possible.

Les SDK d’extension vous permettent d’appeler des API spécifiques à différents appareils. Par exemple, si votre application UWP cible un appareil IoT, vous pouvez ajouter le SDK d’extension IoT à votre projet pour cibler des fonctionnalités spécifiques aux appareils IoT. Pour plus d’informations sur l’ajout de SDK d’extension, consultez la section **Kits de développement logiciel (SDK) d’extension** dans [Vue d’ensemble des familles d’appareils](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview#extension-sdks).

Vous pouvez écrire votre application afin qu’elle s’exécute uniquement sur un type particulier d’appareil, puis limiter sa distribution à partir du Microsoft Store simplement à ce type d’appareil. Ou bien, vous pouvez tester de manière conditionnelle la présence d’une API lors de l’exécution et adapter le comportement de votre application en conséquence. Pour plus d’informations, voir la section **Écriture de code** dans [Vue d’ensemble des familles d’appareils](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview#writing-code).<br>

La vidéo suivante fournit une vue d’ensemble des familles d’appareils et du code adaptatif :
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-UWP-and-Device-Families/player" width="640" height="360" allowFullScreen frameBorder="0"></iframe>

## <a name="adaptive-controls-and-input"></a>Contrôles adaptatifs et entrée

Les éléments d’interface utilisateur s'adaptent à la taille et à la résolution de l’écran de l’application en cours d’exécution en ajustant leur disposition et leur mise à l’échelle. Les applications UWP fonctionnent bien avec plusieurs types d’entrée, par exemple, le clavier, la souris, les impulsions tactiles, le stylet et les manettes Xbox One. Si vous avez besoin d’adapter davantage votre interface utilisateur à une taille d’écran ou à un appareil spécifique, de nouveaux panneaux de disposition sont là pour vous aider à adapter votre interface utilisateur aux appareils et aux facteurs de forme sur lesquels votre application s’exécute.

![Appareils avec Windows](images/1894834-hig-device-primer-01-500.png)

Windows vous aide à cibler votre interface utilisateur sur plusieurs appareils avec les fonctionnalités suivantes :

- Les contrôles universels et panneaux de disposition vous aident à optimiser votre interface utilisateur pour la résolution d’écran de l’appareil. Par exemple, des contrôles tels que des boutons et des curseurs s’adaptent automatiquement à la taille d’écran de l’appareil et à la densité PPP. Les panneaux de disposition aident à ajuster la disposition du contenu en fonction de la taille de l’écran. La mise à l’échelle adaptative s’ajuste en fonction des différentes résolutions des appareils.
- La gestion commune des entrées vous permet de recevoir des entrées par le biais d’impulsions tactiles, d’un stylet, d’une souris, d’un clavier ou d’un contrôleur tel qu’une manette Microsoft Xbox.
- Les outils qui vous permettent de concevoir une interface utilisateur peuvent s’adapter à différentes résolutions d’écran.

Certains aspects de l’interface utilisateur de votre application s’adaptent automatiquement aux divers appareils. La conception de l’expérience utilisateur de votre application peut cependant nécessiter une adaptation en fonction de l’appareil sur lequel l’application s’exécute. Par exemple, une application de photos doit adapter son interface utilisateur quand elle s’exécute sur un appareil portatif de petite taille, afin d’être utilisable d’une seule main. Quand une application de photographie s’exécute sur un ordinateur de bureau, l’interface utilisateur doit s’adapter pour tirer parti de l’espace d’écran supplémentaire.

## <a name="theres-one-store-for-all-devices"></a>Il existe un seul magasin pour tous les appareils.

Un magasin d’applications unifié rend votre application disponible sur les appareils Windows 10 tels que les PC, Tablet PC, Xbox, HoloLens, Surface Hub et appareils Internet des objets (IoT). Vous pouvez soumettre votre application au Store et la rendre disponible sur tous les types d’appareils ou uniquement sur ceux que vous choisissez. Vous soumettez et gérez toutes vos applications pour appareils Windows au même endroit. Vous avez une application de bureau C++ que vous souhaitez moderniser grâce aux fonctionnalités UWP et commercialiser dans le Microsoft Store ? Cela ne pose aucun problème.

À des fins de télémétrie et d’analyse détaillées, les applications UWP s’intègrent avec [Application Insights](https://azure.microsoft.com/services/application-insights/) : un outil crucial pour comprendre vos utilisateurs et améliorer vos applications.

### <a name="monetize-your-app"></a>Monétiser votre application

Vous pouvez choisir la manière de monétiser votre application. Il existe plusieurs moyens de gagner de l’argent avec votre application. Il vous suffit simplement de choisir le procédé qui vous convient le mieux, par exemple :

- L’option la plus simple est de faire payer le téléchargement. Il vous suffit de fixer le prix.
- Les périodes d’essai permettent à l’utilisateur d’essayer votre application avant de l’acheter. Il peut ainsi la découvrir et l’adopter plus facilement que si vous aviez choisi l’option plus classique du « freemium ».
- Prix de vente à prix réduit pour inciter les utilisateurs.
- Des achats et des publicités dans les applications sont également disponibles.

### <a name="apps-from-the-microsoft-store-provide-a-seamless-install-uninstall-and-upgrade-experience"></a>Les applications du Microsoft Store fournissent une expérience d’installation, de désinstallation et de mise à niveau transparente.

Toutes les applications UWP sont distribuées à l’aide d’un système de création de packages qui protège l’utilisateur, l’appareil et le système. Les utilisateurs n'ont pas à regretter d’avoir installé une application, en effet les applications UWP peuvent être désinstallées sans laisser de trace, à l’exception des documents créés avec l’application.

Les applications peuvent être déployées et mises à jour en toute transparence. Les packages d’application sont modulables, vous pouvez donc télécharger le contenu et les extensions à la demande.

## <a name="deliver-relevant-real-time-info-to-your-users-to-keep-them-coming-back"></a>Possibilité de fournir des informations pertinentes en temps réel à vos utilisateurs afin de les fidéliser

Il existe plusieurs façons de fidéliser les utilisateurs avec votre application UWP :

- Les vignettes dynamiques et l’écran de verrouillage permettent de voir d’un seul coup d’œil, à partir de l’application, des informations fournies au moment opportun et adaptées au contexte.
- Les notifications Push permettent d’alerter vos utilisateurs en temps réel.
- Les activités de l’utilisateur permettent aux utilisateurs de reprendre là où ils s’étaient arrêtés dans l’application, même sur différents appareils.
- Le Centre de maintenance organise les notifications à partir de votre application.
- L’exécution en arrière-plan et les déclencheurs activent votre application au moment où l’utilisateur en a besoin.
- Votre application peut utiliser des appareils vocaux et Bluetooth LE pour permettre aux utilisateurs d’interagir avec leur environnement.
- Vous pouvez intégrer Cortana pour ajouter la fonctionnalité de commande vocale à votre application.

##  <a name="use-a-language-you-already-know"></a>Utilisez un langage que vous connaissez déjà

Les applications UWP peuvent utiliser Windows Runtime, l’API native fournie par le système d’exploitation. Cette API est implémentée en C++ et est prise en charge dans C#, Visual Basic, C++ et JavaScript. Parmi les options d’écriture d’applications UWP on trouve :

- XAML UI et C#, VB ou C++
- DirectX UI et C++
- JavaScript et HTML

## <a name="links-to-help-you-get-going"></a>Liens pour vous aider à commencer

### <a name="get-set-up"></a>Préparation

Consultez [Se préparer](get-set-up.md) pour télécharger les outils dont vous avez besoin pour commencer à créer des applications, puis à [écrire votre première application](your-first-app.md) !

### <a name="design-your-app"></a>Concevoir votre application

Le nom du système de conception Microsoft est Fluent. Le système Fluent Design est un ensemble de fonctionnalités UWP combinées avec les meilleures pratiques pour créer des applications qui s’exécutent parfaitement sur tous les types d’appareils fonctionnant sous Windows. Les expériences Fluent s’adaptent parfaitement aux différents appareils (tablettes ou ordinateurs portables, PC ou téléviseurs, ainsi que sur des appareils de réalité virtuelle). Consultez la section [Système Fluent Design pour les applications UWP](https://docs.microsoft.com/windows/uwp/design/fluent-design-system) pour obtenir une présentation du système Fluent Design.

Une bonne [conception](https://go.microsoft.com/fwlink/?LinkId=258848) est le processus consistant à décider comment les utilisateurs doivent interagir avec votre application, ainsi que la manière dont celle-ci se présentera et fonctionnera. L’expérience utilisateur jouant un rôle considérable dans la satisfaction que procurera votre application, ne lésinez pas sur cette étape. La rubrique [Notions de base de conception](https://developer.microsoft.com/en-us/windows/apps/design) présente la conception d’une application Windows universelle. Pour plus d’informations sur la conception d’applications UWP susceptibles d’enchanter les utilisateurs, voir [Présentation des applications de la plateforme Windows universelle (UWP) pour les concepteurs](https://msdn.microsoft.com/library/windows/apps/dn958439). Avant de commencer à coder, voir [Notions fondamentales sur les appareils](../design/devices/index.md) pour réfléchir à l’expérience d’interaction de l’utilisation de votre application sur les différents facteurs de forme que vous voulez cibler.

En plus de l’interaction sur différents appareils, [planifiez votre application](https://msdn.microsoft.com/library/windows/apps/hh465427) pour bénéficier des avantages liés à l’utilisation de plusieurs appareils. Exemple :

- Concevez votre flux de travail en vous basant sur les [Informations de base relatives à la conception de la navigation pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/dn958438) afin d’adapter votre application aux appareils mobiles, petit et grand écran. [Disposez votre interface utilisateur](https://msdn.microsoft.com/library/windows/apps/dn958435) pour l’adapter à différentes tailles et résolutions d’écran.

- Songez à la manière de prendre en charge plusieurs types d’entrée. Pour savoir comment les utilisateurs peuvent interagir avec votre application via [Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233), la fonction [Voix](https://msdn.microsoft.com/library/windows/apps/dn596121), des [interactions tactiles](https://msdn.microsoft.com/library/windows/apps/hh465370), du [clavier tactile](https://msdn.microsoft.com/library/windows/apps/hh972345) et bien plus encore, voir les [Recommandations en matière d’interactions](https://msdn.microsoft.com/library/windows/apps/dn611861).  Pour des expériences d’interaction plus classiques, voir les [Recommandations en matière de texte et de saisie de texte](https://msdn.microsoft.com/library/windows/apps/dn611864).

### <a name="add-services"></a>Ajouter des services

- Utilisez les [services cloud](https://go.microsoft.com/fwlink/?LinkId=526377) pour synchroniser les données sur tous les appareils.
- Découvrez comment vous [connecter aux services Web](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504) pour renforcer l’expérience de votre application.
- Découvrez comment [Ajouter Cortana sur votre application](https://mva.microsoft.com/training-courses/integrating-cortana-in-your-apps-8487?l=20D3s5Xz_5904984382) pour que votre application puisse répondre aux commandes vocales.
- Incluez des [Notifications Push](https://msdn.microsoft.com/library/windows/apps/mt187203) et des [achats dans l’application](https://msdn.microsoft.com/library/windows/apps/mt219684) dans votre planification. Ces fonctionnalités doivent opérer sur tous les appareils.

### <a name="submit-your-app-to-the-store"></a>Soumettre votre application sur le Windows Store

[Partenaires](https://partner.microsoft.com/dashboard) vous permet de gérer et envoyer toutes vos applications pour les appareils Windows au même endroit. Consultez [Windows de publier des applications et des jeux](../publish/index.md) pour savoir comment soumettre vos applications pour la publication dans le Microsoft Store.

De nouvelles fonctionnalités simplifient les processus tout en vous offrant un contrôle accru. Le nouveau tableau de bord vous propose également des [rapports d’analyse détaillés](https://msdn.microsoft.com/library/windows/apps/mt148522), des [informations sur les revenus](https://msdn.microsoft.com/library/windows/apps/dn986925), des méthodes pour [promouvoir votre application et susciter l’intérêt de vos clients](https://msdn.microsoft.com/library/windows/apps/mt148526), et bien davantage.

Pour obtenir une introduction plus générale, voir [Introduction au développement d’applications Windows pour appareils Windows 10](https://msdn.microsoft.com/magazine/dn973012.aspx)

### <a name="more-advanced-topics"></a>Rubriques plus avancées

- Découvrez comment utiliser les [Activités de l’utilisateur](https://blogs.windows.com/buildingapps/2017/12/19/application-engagement-windows-timeline-user-activities/#tHuZ6tLPtCXqYKvw.97) afin que l’activité utilisateur dans votre application apparaisse dans la Chronologie de Windows et dans la fonctionnalité Pick Up Where I Left Off de Cortana.
- Découvrez comment utiliser les [Vignettes, badges et notifications pour les applications UWP](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/).
- Pour obtenir la liste complète des API Win32 disponibles pour les applications UWP, voir [Ensembles d’API pour les applications UWP](https://msdn.microsoft.com/library/windows/desktop/mt186421) et [DLL pour les applications UWP](https://msdn.microsoft.com/library/windows/desktop/mt186422).
- Voir [Universal Windows apps in .NET (Applications Windows universelles dans .NET)](https://blogs.msdn.microsoft.com/dotnet/2015/07/30/universal-windows-apps-in-net) pour obtenir une vue d’ensemble de l’écriture des applications UWP .NET.
- Pour obtenir la liste des types .NET que vous pouvez utiliser dans une application UWP, voir [.NET pour les applications UWP](https://msdn.microsoft.com/library/mt185501.aspx)
- [Compilation d’applications avec .NET Native](https://docs.microsoft.com/dotnet/framework/net-native/)
- Découvrez comment ajouter des expériences modernes pour les utilisateurs Windows 10 à votre application de bureau et comment les distribuer dans le Microsoft Store avec le [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop).

## <a name="how-the-universal-windows-platform-relates-to-windows-runtime-apis"></a>La relation avec la plateforme Windows universelle Windows Runtime APIs
Si vous créez une application de plateforme universelle Windows (UWP), vous pouvez obtenir un grand nombre de kilométrage et commodité hors en traitant les termes « Universal Windows Platform (UWP) » et « Windows Runtime (WinRT) » comme des synonymes plus ou moins. Mais il *est* possible de rechercher dans les coulisses de la technologie et de déterminer simplement la différence entre ces idées. Si vous êtes curieux de savoir que, cette dernière section est pour vous.

Le Windows Runtime, WinRT APIs et, sont une évolution des API de Windows. À l’origine, Windows a été programmé via plat, l’API Win32 de style C. Ceux ont été ajoutés APIs COM ([DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) étant un exemple important). Permet de mettre leur façon d’écrire des applications Windows et leur propre version de la technologie de l’API Windows Forms, WPF, .NET et les langages managés. Le Runtime de Windows est, en coulisses, l’étape suivante de COM. Dans la couche d’interface binaire (ABI) application réelle, ses racines dans COM deviennent visibles. Mais le Runtime Windows a été conçu pour être appelées à partir d’une large gamme de langages de programmation différents. Et pouvant être appelées de manière très naturelle pour chacune de ces langues. À cette fin, un accès à l’exécution de Windows sont rendu disponible par le biais de ce que l'on appelle projections de langage. Il existe une projection de langage Windows Runtime dans C#, dans Visual Basic, en C++ standard, dans JavaScript et ainsi de suite. En outre, une fois empaqueté correctement (voir [pont du bureau](/windows/uwp/porting/desktop-to-uwp-root)), vous pouvez appeler WinRT APIs à partir d’une application créée dans un d’une large gamme de modèles d’application : Win32, .NET, WinForms et WPF.

Et, bien sûr, vous pouvez appeler WinRT APIs à partir de votre application UWP. UWP est un modèle d’application reposant sur l’exécution de Windows. Techniquement, selon le modèle d’application UWP [CoreApplication](/uwp/api/windows.applicationmodel.core.coreapplication), bien que ce détail peut être masqué pour vous, selon votre choix de langage de programmation. Comme cette rubrique a expliqué, à partir d’une proposition de valeur point de vue, la plateforme Windows universelle est prête à l’écriture d’un fichier binaire unique qui peut, si vous choisissez, publié dans le Microsoft Store et s’exécuter sur n’importe laquelle d'entre une large gamme de facteurs de forme de périphérique. La portée de l’appareil de votre application UWP varie selon le sous-ensemble d’API UWP que vous limitez votre application à l’appel, ou que vous appelez de manière conditionnelle.

J’espère que cette section a réussi dans la description de la différence entre la technologie sous-jacente Windows APIs Runtime et le mécanisme et la valeur commerciale de la plateforme Windows universelle.
