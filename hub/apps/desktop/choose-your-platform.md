---
Description: Lorsque vous souhaitez créer une nouvelle application de bureau, la première décision que vous apportez est s’il faut utiliser Win32 et de API COM ou .NET.
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: Choisissez votre plateforme d’application
ms.topic: article
ms.date: 03/18/2019
ms.openlocfilehash: 960dda5e4cb7e8edc1edf7ce2e81da8306555f1c
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984489"
---
# <a name="choose-your-app-platform"></a>Choisissez votre plateforme d’application

Lorsque vous souhaitez créer une nouvelle application de bureau pour les PC Windows, la première décision que vous apportez est la plateforme sur laquelle application à utiliser. Windows fournit quatre plateformes d’application principale, chacune avec différents points forts :

* [Plateforme Windows universelle (UWP)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows Forms (.NET)](#windows-forms)
* [Win32](#win32)

Toutes ces plateformes d’application vous permettent de créer des applications de bureau tels que Word, Excel et Photoshop qui s’exécutent dans le classique Windows desktop et take pleinement parti des fonctionnalités spécifiques de cet environnement. Toutefois, certaines de ces plateformes partagent certaines caractéristiques et sont mieux adaptés à certains types d’applications :

* **UWP, WPF et Windows Forms**. Ces plateformes fournissent des environnements d’exécution managé (le Runtime Windows pour UWP et .NET pour Windows Forms et WPF) de nombreux avantages, notamment dans les domaines de la productivité des développeurs, l’interface utilisateur sophistiquée et personnalisable et la sécurité de l’application. Étant donné que ces infrastructures prennent en charge les concepteurs visuels et balisage de l’interface utilisateur pour créer rapidement de l’interface utilisateur, ils sont particulièrement bien adaptées pour les applications line-of-business.

* **API Win32**. L’API Win32 (également appelé l’API Windows) est une plateforme d’origine natif C /C++ les applications Windows qui nécessitent un accès direct au matériel et de Windows. Il fournit une expérience de développement de premier ordre sans en fonction d’un environnement d’exécution managé comme .NET et WinRT. Cela rend l’API Win32 de la plate-forme de choix pour les applications nécessitant le plus haut niveau de performances et l’accès direct au matériel système.

Cet article décrit ces plateformes plus en détail et vous permet de déterminer le mieux adapté pour votre application.

## <a name="uwp"></a>UWP

UWP est la plateforme de pointe pour jeux et applications Windows 10. Il est une plateforme hautement personnalisable qui utilise le balisage XAML pour séparer l’expérience utilisateur (présentation) à partir du code (logique métier). UWP est approprié pour les applications nécessitant une interface utilisateur sophistiquée, la personnalisation des styles et des scénarios de graphiques. UWP a également une prise en charge intégrée pour le [Fluent Design System](/windows/uwp/design/fluent-design-system/) pour la valeur par défaut, l’expérience utilisateur rencontrer et fournit l’accès à la [Windows Runtime (WinRT) API](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis). En adoptant Fluent, UWP prend automatiquement en charge les méthodes d’entrée courantes telles que l’encre, tactile, gamepad, clavier et souris.

Non seulement vous pouvez utiliser UWP pour créer des applications de bureau pour les PC Windows, mais UWP est également la seule plateforme prise en charge pour les applications Xbox, HoloLens et Surface Hub. UWP est notre plateforme d’application plus récente, de pointe.

Pour plus d’informations sur UWP, consultez [prise en main les applications Windows 10](/windows/uwp/get-started/).

## <a name="wpf"></a>WPF

WPF est la plateforme établie pour les applications Windows gérées avec un accès pour le .NET Framework complet, et il utilise également les balisage XAML pour séparer l’expérience utilisateur à partir du code. Cette plateforme est conçue pour les applications de bureau qui requièrent une interface utilisateur sophistiquée, personnalisation des styles et des scénarios de graphiques. Compétences de développement WPF sont semblables aux compétences de développement UWP, migration de WPF vers les applications UWP est plus facile que la migration à partir de Windows Forms.

Pour plus d’informations sur WPF, consultez [mise en route (WPF)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/).

## <a name="windows-forms"></a>Windows Forms

Windows Forms est la plate-forme d’origine pour les applications Windows gérées avec un modèle d’interface utilisateur légère et l’accès pour le .NET Framework complet. Il convient parfaitement à permettant aux développeurs de commencer rapidement à créer des applications, même pour les développeurs qui découvrent la plateforme. Il s’agit d’une plate-forme de développement d’applications basée sur des formulaires rapides avec une grande collection intégrée de contrôles de glisser-déplacer visuel et non visuel. Windows Forms n’utilise pas de XAML, donc vous décidez ultérieurement d’étendre votre application UWP implique une réécriture complète de votre interface utilisateur.

Pour plus d’informations sur les Windows Forms, consultez [mise en route avec Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms).

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>Comparaison de la plateforme : UWP, WPF et Windows Forms

Le tableau suivant compare les différentes caractéristiques des Windows Forms, WPF et UWP en détail.

| Fonctionnalité ou scénario  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **Versions prises en charge**      |  Windows 10   |  Windows 7 et versions ultérieures |  Windows 7 et versions ultérieures  |
| **Traduction**      |   C\#, C++/WinRT, C++/CX, VB, JavaScript   |  C\#, C++/CLI (Extensions managées pour C++), F\#, VB |  C\#, C++/CLI (Extensions managées pour C++), F\#, VB   |
| **Exécution de l’interface utilisateur** |    Natif (C++/WinRT et C++/CX) et gérées (.NET Native)  |  Managé (.NET Framework)<br/><br/>Prise en charge de .NET Core 3 est bientôt disponible  |   Managé (.NET Framework)<br/><br/>Prise en charge de .NET Core 3 est bientôt disponible    |
| **Open source** | [Oui (bibliothèque d’interface utilisateur Windows uniquement)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [Oui (.NET Core uniquement)](https://github.com/dotnet/wpf) | [Oui (.NET Core uniquement)](https://github.com/dotnet/winforms)  |
| **Prend en charge XAML** |   Oui   |  Oui  |   Non   |
| **Points forts**  |  <ul><li>Balisage XAML pour l’interface utilisateur</li><li>Expérience utilisateur riche et personnalisable</li><li>Vos bases de code existantes sont conformes .NET Standard</li><li>Prise en charge de haute résolution</li><li>Prise en charge de plusieurs types d’entrée sur les appareils Windows (y compris tactiles, pen, gamepad, souris et clavier)</li><li>Prise en charge de la Xbox, HoloLens, IoT ou Surface Hub</li><li>Prise en charge de natifC++</li><li>Autonomie optimisé</li><li>Prise en charge de l’accessibilité modernes (tels que les lecteurs d’écran)</li><li>Fonctionnalités de données de texte enrichi (par exemple, la vérification orthographique intégrée)</li><li>Prise en charge pour l’écriture manuscrite</li><li>Sécuriser l’exécution via des conteneurs d’application (par exemple, le contenu est sandbox approuvée)</li></ul>  |  <ul><li>Balisage XAML pour l’interface utilisateur</li><li>Expérience utilisateur riche et personnalisable</li><li>Grande collection de contrôles à partir de Microsoft et partenaires</li><li>Interface utilisateur de haute densité</li><li>Prise en charge de Windows 7</li><li>Plateforme prend en charge pour la validation d’entrée</li></ul> | <ul><li>développement rapide d’applications</li><li>Éditeur WYSIWYG pour la création de l’interface utilisateur</li><li>Grande collection de contrôles à partir de Microsoft et partenaires</li><li>Interface utilisateur de haute densité</li><li>Prise en charge de Windows 7</li><li>Entrée clavier et souris</li></ul>          |
| **Scénarios qui ont de la prise en charge limitée** |  <ul><li>L’interface utilisateur dense (création d’une interface utilisateur dense nécessite des styles personnalisés)<sup>1</sup></li><li>Prise en charge de plusieurs fenêtres<sup>1</sup></li><li>Plates-formes prises en charge la validation des entrées<sup>1</sup></li><li>Windows 7 n’est pas pris en charge.</li><li>Certaines API UWP nécessitent des versions spécifiques de minimale de Windows 10</li><li>Intégration de prise en charge et le shell de plate-forme complète (par exemple, UWP actuellement ne prend en charge l’intégration barre d’état système ou un accès complet à tous les appareils)</li><li>Accès direct à tous les fichiers sur le disque</li><li>ADO.NET</li><li>Les bibliothèques de classes de base de code existantes qui utilisent non - .NET Standard ou des API conforme non - Windows App Certification Kit</li><li>Prise en charge de bouclage de réseau local (autrement dit, si votre application a besoin communiquer avec localhost sans créer une exemption de bouclage sur le périphérique cible)</li><li>Fichier gourmandes en e/s</li></ul>     |  <ul><li>Prise en charge de haute résolution<sup>2</sup></li><li>Entrée tactile<sup>2</sup></li></ul>  |  <ul><li>Prise en charge de haute résolution<sup>2</sup></li><li>Entrée tactile<sup>2</sup></li><li>Interface utilisateur personnalisable</li><li>Utilisateur et les graphiques de riches expériences (par exemple, tactile et animations)</li><li>Abstraction riche de modèles de données et de vues</li></ul>    |   |

<sup>1</sup> nous avons annoncé publiquement les fonctionnalités qui répondront à ce scénario dans une prochaine version de Windows 10.

<sup>2</sup> bien que la plateforme n’a pas de prise en charge de première classe API pour ce scénario, les développeurs peuvent prendre en charge ce scénario avec les solutions de contournement.

## <a name="win32"></a>Win32

À l’aide de l’API Win32 avec C++ rend possible d’atteindre les niveaux les plus élevés de performances et l’efficacité en tirant plus de contrôle de la plateforme cible avec le code non managé que possible sur un environnement d’exécution managé tels que WinRT et .NET. Toutefois, un niveau de ce type de contrôle de l’exécution de votre application nécessite supérieur soins et attention à coup et la mise en productivité du développement pour les performances d’exécution de transactions.

Voici quelques extraits de quel l’API Win32 et C++ offre à vous permettent de créer des applications hautes performances.

-   Optimisations au niveau du matériel, y compris un contrôle étroit sur l’allocation des ressources, durées de vie des objets, mise en page de données, alignement octets de livraison et bien plus encore.
-   Il définit, tels que SSE et AVX accès à instruction axé sur les performances, par le biais des fonctions intrinsèques.
-   Programmation générique efficace, de type sécurisé à l’aide de modèles.
-   Des algorithmes et des conteneurs efficaces et sûr.
-   DirectX, notamment Direct3D et DirectCompute (Notez que UWP offre également l’interopérabilité DirectX).
-   C++ AMP.

Pour plus d’informations, consultez [prise en main les applications de bureau Windows qui utilisent l’API Win32](/windows/desktop/desktop-programming) et [technologies de l’application de bureau](/windows/desktop/desktop-app-technologies).

### <a name="win32-and-c-for-traditional-desktop-applications"></a>Win32 et C++ pour les applications de bureau traditionnelles

Lors de l’écriture d’une application de bureau C++, vous pouvez choisir Win32 ou MFC pour l’interface utilisateur ou un ordinateur hôte d’infrastructures d’applications de fournisseurs tiers qui prennent également en charge les plateformes non Windows.

-   **Win32 :** Il s’agit de l’API basée sur le handle, en langage C de la plateforme Windows, y compris de manière non limitative, les fonctionnalités d’interface utilisateur tels que le fenêtrage, de dessin et de l’interface utilisateur de contrôles. S’agissant d’une API de bas niveau, en langage C basée sur les handles, c’est un choix peu fréquent pour la création d’applications modernes utilisant beaucoup de l’interface utilisateur. Toutefois, il fournit les API de base nécessaires pour interagir avec la plateforme Windows et est un choix approprié pour les applications qui ont des exigences de l’interface utilisateur simples ou qui veulent juste l’interface utilisateur de Windows pour rester disparaître autant que possible, par exemple, des jeux.
-   **MFC (bibliothèque Microsoft Foundation Class) :** C’est l’infrastructure d’application vénérable et la bibliothèque d’interface utilisateur qui a servi aux développeurs Windows depuis 1992. Il s’agit une mince C++ wrapper sur l’API Win32 basée sur le handle, en langage C et fournit des interfaces et orienté objet pour un grand nombre de windows prédéfini, les contrôles communs et les autres objets de Windows. Bien que plusieurs infrastructures d’interface utilisateur modernes dans l’écosystème .NET dépassent MFC dans plus de commodité, il est toujours l’infrastructure d’interface utilisateur native de choix pour de nombreuses C++ les développeurs qui créent des applications pour le bureau Windows.
-   **Infrastructures d’application tierce :** Étant donné que C++ peuvent s’exécuter sur un large éventail de plateformes et n’est pas liée à Windows ou le runtime .NET, à des tiers ont développé nouvelle application et des infrastructures d’interface utilisateur pour C++ pour faciliter le développement d’applications multiplateformes avec des interfaces utilisateur riches. Certaines de ces infrastructures fournissent leurs propres aspect et la convivialité, tandis que d’autres, comme wxWidgets ou Qt utilisent ou émulent l’ensemble du contrôle natif de la plateforme. À l’aide de ces bibliothèques, il est possible de partager presque tout code de source d’une application entre les versions de l’application qui s’exécutent sur Windows ou d’autres plateformes, tels que OSX ou Linux.

## <a name="other-app-platforms"></a>Autres plateformes d’applications

### <a name="progressive-web-apps-pwas"></a>Applications Web progressive (PWA)

Des instances PWA permettent les développeurs réunissent son site Web de code afin de pouvoir être installé et exécuté comme une application sur les PC Windows 10. Pour plus d’informations, consultez [Progressive Web Apps](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started).

### <a name="xamarin"></a>Xamarin

Utilisez Xamarin pour générer des applications multiplateformes pour Windows 10 qui peut également exécuté sur iOS et Android. Pour plus d’informations, consultez [Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index).
