---
Description: Lorsque vous souhaitez créer une application de bureau, la première décision à prendre est l’utilisation de l’API Win32 et COM ou de .NET.
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: Choisir votre plateforme d’application
ms.topic: article
ms.date: 03/18/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: ff32e20c42f613bd9ac3dba9eada2cced0baa64c
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71316991"
---
# <a name="choose-your-app-platform"></a>Choisir votre plateforme d’application

Lorsque vous souhaitez créer une application de bureau pour les PC Windows, la première décision à prendre est la plateforme d’application à utiliser. Windows propose quatre plates-formes d’applications principales, chacune présentant des avantages différents :

* [Plateforme Windows universelle (UWP)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows Forms (.NET)](#windows-forms)
* [32](#win32)

Toutes ces plateformes d’application vous permettent de créer des applications de bureau telles que Word, Excel et Photoshop qui s’exécutent sur le bureau Windows classique et tirent pleinement parti des fonctionnalités spécifiques de l’environnement. Toutefois, certaines de ces plateformes partagent des caractéristiques et sont mieux adaptées à certains types d’applications :

* **UWP, WPF et Windows Forms**. Ces plateformes fournissent des environnements de Runtime gérés (les Windows Runtime pour UWP et .NET pour Windows Forms et WPF) avec de nombreux avantages, en particulier dans les domaines de la productivité des développeurs, de l’interface utilisateur sophistiquée et personnalisable et de la sécurité des applications. Étant donné que ces frameworks prennent en charge les concepteurs visuels et les balises d’interface utilisateur pour créer rapidement une interface utilisateur, ils sont particulièrement adaptés aux applications métier.

* **API Win32**. L’API Win32 (également appelée API Windows) est la plateforme d’origine pour les applications CC++ /Windows natives qui requièrent un accès direct à Windows et au matériel. Il fournit une expérience de développement de premier ordre sans dépendre d’un environnement d’exécution managé tel que .NET et WinRT. Cela fait de l’API Win32 la plate-forme de choix pour les applications qui ont besoin du niveau de performances le plus élevé et un accès direct au matériel système.

Cet article décrit plus en détail ces plateformes et vous aide à déterminer celle qui convient le mieux à votre application. 

> [!NOTE]
> Quelle que soit la plate-forme d’application que vous choisissez, vous pouvez utiliser de nombreuses fonctionnalités de la plateforme Windows universelle (UWP) pour fournir une expérience moderne dans votre application sur Windows 10. Par exemple, même si votre application de bureau est créée à l’aide de WPF, Windows Forms ou l’API Win32, vous pouvez toujours utiliser de nombreuses fonctionnalités introduites en premier avec UWP, telles que le déploiement de packages MSIX et les contrôles XAML UWP. Pour plus d’informations, consultez [moderniser vos applications de bureau](modernize/index.md).

## <a name="uwp"></a>UWP

UWP est la plateforme de pointe pour les applications et les jeux Windows 10. Il s’agit d’une plateforme hautement personnalisable qui utilise le balisage XAML pour séparer l’expérience utilisateur (présentation) du code (logique métier). UWP convient pour les applications de bureau qui nécessitent une interface utilisateur sophistiquée, une personnalisation des styles et des scénarios nécessitant de nombreuses ressources graphiques. UWP offre également une prise en charge intégrée du [système de conception Fluent](/windows/uwp/design/fluent-design-system/) pour l’expérience utilisateur par défaut et donne accès aux [API Windows Runtime (WinRT)](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis). En adoptant Fluent, UWP prend en charge automatiquement des méthodes d’entrée courantes telles que l’encre, le toucher, le boîtier, le clavier et la souris.

Non seulement vous pouvez utiliser UWP pour créer des applications de bureau pour les PC Windows, mais UWP est également la seule plateforme prise en charge pour les applications Xbox, HoloLens et Surface Hub. UWP est notre plate-forme d’applications de pointe la plus récente.

Pour plus d’informations sur UWP, consultez [prise en main des applications Windows 10](/windows/uwp/get-started/).

## <a name="wpf"></a>WPF

WPF est la plateforme établie pour les applications Windows gérées ayant accès à la .NET Framework complète, et utilise également le balisage XAML pour séparer l’expérience utilisateur du code. Cette plateforme est conçue pour les applications de bureau qui nécessitent une interface utilisateur sophistiquée, une personnalisation des styles et des scénarios nécessitant de nombreuses ressources graphiques. Les compétences de développement WPF sont similaires aux compétences de développement UWP. la migration de WPF vers les applications UWP est donc plus facile que la migration à partir de Windows Forms.

Pour plus d’informations sur WPF, consultez prise en main [(WPF)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/).

## <a name="windows-forms"></a>Windows Forms

Windows Forms est la plateforme d’origine pour les applications Windows gérées avec un modèle d’interface utilisateur léger et l’accès à la .NET Framework complète. Il est plus rapide de permettre aux développeurs de commencer rapidement à créer des applications, même pour les développeurs qui débutent avec la plate-forme. Il s’agit d’une plateforme de développement d’applications rapide et basée sur des formulaires, avec une vaste collection intégrée de contrôles de glisser-déplacer visuels et non visuels. Windows Forms n’utilise pas XAML, si vous décidez ultérieurement d’étendre votre application à UWP, vous devez réécrire la totalité de votre interface utilisateur.

Pour plus d’informations sur les Windows Forms, consultez [prise en main de Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms).

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>Comparaison de plateforme : UWP, WPF et Windows Forms

Le tableau suivant compare en détail différentes caractéristiques de Windows Forms, WPF et UWP.

| Fonctionnalité ou scénario  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **Versions prises en charge**      |  Windows 10   |  Windows 7 et versions ultérieures |  Windows 7 et versions ultérieures  |
| **Traduction**      |   C\#, C++/WinRT, C++/CX, VB, JavaScript   |  C\#, C++/CLI (extensions managées C++pour),\#F, VB |  C\#, C++/CLI (extensions managées C++pour),\#F, VB   |
| **Runtime d’interface utilisateur** |    Natif (C++/WinRT et C++/CX) et managé (.net native)  |  Géré (.NET Framework et .NET Core 3) |   Géré (.NET Framework et .NET Core 3)   |
| **Open source** | [Oui (bibliothèque d’interface utilisateur Windows uniquement)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [Oui (.NET Core uniquement)](https://github.com/dotnet/wpf) | [Oui (.NET Core uniquement)](https://github.com/dotnet/winforms)  |
| **Prend en charge XAML** |   Oui   |  Oui  |   Non   |
| **Forces**  |  <ul><li>Balisage XAML pour l’interface utilisateur</li><li>EXPÉRIENCE utilisateur riche et personnalisable</li><li>Vos bases de code existantes sont .NET Standard conformes</li><li>Prise en charge des résolutions élevées</li><li>Prise en charge de plusieurs types d’entrée sur des appareils Windows (y compris les touches tactiles, le stylet, le boîtier, la souris et le clavier)</li><li>Prise en charge de Xbox, HoloLens, IoT ou Surface Hub</li><li>Prise en charge de NativeC++</li><li>Autonomie de batterie optimisée</li><li>Prise en charge de l’accessibilité moderne (par exemple, les lecteurs d’écran)</li><li>Fonctionnalités de données de texte enrichi (telles que la vérification orthographique intégrée)</li><li>Prise en charge de l’encrage</li><li>Exécution sécurisée via des conteneurs d’applications (par exemple, le contenu non approuvé est sandbox)</li></ul>  |  <ul><li>Balisage XAML pour l’interface utilisateur</li><li>EXPÉRIENCE utilisateur riche et personnalisable</li><li>Grande collection de contrôles de la part de Microsoft et de ses partenaires</li><li>Interface utilisateur dense</li><li>Prise en charge de Windows 7</li><li>Prise en charge de la plateforme pour la validation des entrées</li></ul> | <ul><li>Développement rapide d’applications</li><li>Éditeur WYSIWYG pour générer l’interface utilisateur</li><li>Grande collection de contrôles de la part de Microsoft et de ses partenaires</li><li>Interface utilisateur dense</li><li>Prise en charge de Windows 7</li><li>Entrée au clavier et à la souris</li></ul>          |
| **Scénarios avec prise en charge limitée** |  <ul><li>Dense UI (la création d’une interface utilisateur dense nécessite des styles personnalisés)<sup>1</sup></li><li>Prise en charge de plusieurs fenêtres<sup>1</sup></li><li>Prise en charge de la plateforme pour la validation d’entrée<sup>1</sup></li><li>Windows 7 n’est pas pris en charge</li><li>Certaines API UWP requièrent des versions minimales spécifiques de Windows 10</li><li>Prise en charge complète de la plateforme et intégration de Shell (par exemple, UWP ne prend actuellement pas en charge l’intégration de la barre d’état système ou l’accès complet à tous les appareils)</li><li>Accès direct à tous les fichiers sur le disque</li><li>ADO.NET</li><li>Des bibliothèques de classes de base de code existantes qui utilisent des API conformes non-.NET standard ou non-Windows application Certificate Kit</li><li>Prise en charge de la boucle réseau locale (autrement dit, si votre application doit communiquer avec localhost sans créer d’exemption de bouclage sur l’appareil cible)</li><li>E/s de fichier intensives</li></ul>     |  <ul><li>Prise en charge des résolutions élevées<sup>2</sup></li><li>Entrée tactile<sup>2</sup></li></ul>  |  <ul><li>Prise en charge des résolutions élevées<sup>2</sup></li><li>Entrée tactile<sup>2</sup></li><li>Interface utilisateur personnalisable</li><li>Des graphiques et des expériences utilisateur riches (tels que les touches tactiles et les animations)</li><li>Abstraction étendue des vues et des modèles de données</li></ul>    |   |

<sup>1</sup> nous avons annoncé publiquement des fonctionnalités qui traiteront ce scénario dans une prochaine version de Windows 10.

<sup>2</sup> bien que la plateforme ne dispose pas de la prise en charge de l’API de première classe pour ce scénario, les développeurs peuvent prendre en charge ce scénario avec des solutions de contournement.

## <a name="win32"></a>Win32

L’utilisation de l’API C++ Win32 avec permet d’atteindre les plus hauts niveaux de performances et d’efficacité en contrôlant davantage la plateforme cible avec du code non géré que dans un environnement d’exécution managé comme WinRT et .net. Toutefois, l’exercice d’un tel niveau de contrôle sur l’exécution de votre application nécessite une attention et une attention accrues pour obtenir des performances et une productivité de développement pour les performances d’exécution.

Voici quelques-unes des principales fonctionnalités de l’API C++ Win32 qui vous permettent de créer des applications à hautes performances.

-   Optimisations au niveau du matériel, y compris un contrôle étroit sur l’allocation des ressources, les durées de vie des objets, la disposition des données, l’alignement, la compression d’octets et bien plus encore.
-   Accès aux jeux d’instructions orientés performance tels que SSE et AVX par le biais de fonctions intrinsèques.
-   Programmation générique efficace de type sécurisé à l’aide de modèles.
-   Des conteneurs et des algorithmes efficaces et sûrs.
-   DirectX, en particulier Direct3D et DirectCompute (Notez que UWP offre également l’interopérabilité DirectX).
-   C++MATÉRIEL.

Pour plus d’informations, consultez [prise en main des applications Windows bureautiques qui utilisent l’API Win32 et les](/windows/desktop/desktop-programming) [technologies d’application de bureau](/windows/desktop/desktop-app-technologies).

### <a name="win32-and-c-for-traditional-desktop-applications"></a>Win32 et C++ pour les applications de bureau traditionnelles

Lors de l’écriture d’une C++application de bureau dans, vous pouvez choisir Win32 ou MFC pour l’interface utilisateur, ou un hôte d’infrastructures d’application tierces qui prennent également en charge les plateformes non-Windows.

-   **32** Il s’agit de l’API du langage C, basée sur les descripteurs, de la plate-forme Windows, y compris, mais sans s’y limiter, les fonctionnalités de l’interface utilisateur telles que les contrôles de fenêtrage, de dessin et d’interface utilisateur. Étant donné qu’il s’agit d’une API en langage C de bas niveau basée sur des handles, il est peu fréquent de créer des applications modernes gourmandes en IU. Toutefois, il fournit les API de base nécessaires à l’interaction avec la plate-forme Windows, et est un choix approprié pour les applications qui ont des exigences d’interface utilisateur simples ou qui veulent simplement que l’interface utilisateur de Windows reste la plus à même possible, par exemple, des jeux.
-   **MFC (bibliothèque MFC (Microsoft Foundation Class)) :** Il s’agit de l’infrastructure d’application vénérable et de la bibliothèque d’interface utilisateur qui a servi aux développeurs Windows depuis 1992. Il s’agit d' C++ un wrapper léger sur l’API Win32 basée sur les descripteurs et en langage C, qui fournit des interfaces orientées objet pour la plupart des fenêtres prédéfinies, des contrôles communs et d’autres objets Windows. Bien que de nombreuses infrastructures d’interface utilisateur modernes dans l’écosystème .NET dépassent MFC en pratique, il s’agit toujours de l' C++ infrastructure d’interface utilisateur native de Choice pour de nombreux développeurs créant des applications pour le bureau Windows.
-   **Infrastructures d’application tierces :** Étant C++ donné que peut s’exécuter sur un large éventail de plateformes et n’est pas lié à Windows ou au Runtime .net, des tiers ont développé de nouvelles C++ infrastructures d’application et d’interface utilisateur pour pour faciliter le développement d’applications multiplateformes avec des interfaces utilisateur riches. Certaines de ces infrastructures fournissent leur propre apparence &, tandis que d’autres telles que wxWidgets ou QT utilisent ou émulent le jeu de contrôle natif de la plateforme. À l’aide de ces bibliothèques, il est possible de partager presque tout le code source d’une application entre les versions de l’application qui s’exécutent sur Windows ou d’autres plateformes, telles que OSX ou Linux.

## <a name="other-app-platforms"></a>Autres plateformes d’applications

### <a name="progressive-web-apps-pwas"></a>Web Apps progressif (PWA)

PWA permet aux développeurs de créer un package du code de leur site Web afin qu’il puisse être installé et exécuté comme une application sur les PC Windows 10. Pour plus d’informations, consultez [progressif Web Apps](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started).

### <a name="xamarin"></a>Xamarin

Utilisez Xamarin pour générer des applications multiplateformes pour Windows 10 qui peuvent également s’exécuter sur iOS et Android. Pour plus d’informations, consultez [Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index).
