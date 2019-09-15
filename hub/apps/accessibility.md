---
title: Accessibilité dans Windows 10
description: Cette page fournit des informations vous permettant de commencer à développer des applications Windows accessibles.
ms.topic: article
ms.date: 09/12/2019
keywords: Accessibilité dans Windows 10, accessibilité, création d’applications Win32 accessibles, création d’applications UWP accessibles, création d’applications WPF accessibles, création d’applications WinForms accessibles
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: 76bbd3f0e04bbb2f729ad0950bae190b2fffb6ac
ms.sourcegitcommit: 6e7665b457ec4585db19b70acfa2554791ad6e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70987197"
---
# <a name="accessibility-in-windows-10"></a>Accessibilité dans Windows 10

![Hero-Accessibility-bar-Smaller. png](images/hero-accessibility-bar-smaller.png)

## <a name="build-accessibility-into-your-applications-to-empower-people-of-all-abilities"></a>Créez l’accessibilité dans vos applications pour permettre aux personnes de toutes les possibilités

Les produits et services, y compris les supports électroniques, sont accessibles lorsqu’ils sont conçus pour fournir une expérience complète et réussie pour autant de personnes que possible.

Créez des applications Windows accessibles et inclusives, avec des fonctionnalités améliorées et une plus grande facilité d’utilisation, pour les personnes handicapées (temporaires et permanentes), les préférences personnelles, les styles de travail spécifiques ou les contraintes de situation (telles que les espaces de travail partagés). conduite, cuisson, reflets, etc.). Certaines solutions courantes incluent l’apport d’informations dans d’autres formats (tels que les légendes sur une vidéo) ou l’activation de l’utilisation de technologies d’assistance (telles que les lecteurs d’écran).

**Tout le monde doit avoir accès aux mêmes salles dans un bâtiment, qu’il doive utiliser les escaliers ou l’ascenseur.**

Cette page fournit des informations sur la façon dont les différentes infrastructures de développement Windows assurent la prise en charge de l’accessibilité pour les développeurs qui créent des applications Windows, les développeurs de technologie d’assistance qui créent des outils tels que les lecteurs d’écran et les loupes et les logiciels les ingénieurs de test qui créent des scripts automatisés pour tester les applications.

## <a name="platform-specific-documentation"></a>Documentation spécifique à la plateforme

:::row:::
   :::column:::
      ![Plateforme Windows universelle (UWP)](images/platform-uwp.png)

      **Plateforme Windows universelle (UWP)**

      Développez des applications et des outils accessibles sur la plateforme moderne pour les applications et les jeux Windows 10 sur n’importe quel appareil Windows (y compris les PC, les téléphones, Xbox One, HoloLens, etc.) et publiez-les sur le Microsoft Store.

      [Conception de logiciels inclusifs](https://docs.microsoft.com/windows/uwp/accessibility/designing-inclusive-software)

      [Développement d’applications Windows inclusives](https://docs.microsoft.com/windows/uwp/accessibility/developing-inclusive-windows-apps)

      [Test de l’accessibilité](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-testing)

      [Accessibilité dans le Windows Store](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-in-the-store)
   :::column-end:::
   :::column:::
      ![Applications de plateforme Win32](images/platform-win32.png)

      **Plateforme Win32**

      Développez des applications et des outils accessibles sur la plateforme d'C++ origine pour les applications C/Windows.

      [Nouveautés de l’accessibilité et de l’automatisation Windows](https://docs.microsoft.com/windows/desktop/accessibility-whatsnew)

      [Développement d’applications accessibles pour Windows](https://docs.microsoft.com/windows/desktop/accessibility-appdev)

      [Développement d’infrastructures d’interface utilisateur accessibles pour Windows](https://docs.microsoft.com/windows/desktop/accessibility-uiframeworkdev)

      [Développement de la technologie d’assistance pour Windows](https://docs.microsoft.com/windows/desktop/accessibility-atdev)

      [Test de l’accessibilité](https://docs.microsoft.com/windows/desktop/accessibility-testwithuia)

      [Technologie d’accessibilité et d’automatisation héritée-MSAA à UI Automation](https://docs.microsoft.com/windows/desktop/accessibility-legacy)

      [Fonctionnalités d’accessibilité de Windows](https://docs.microsoft.com/windows/desktop/winauto/about-windows-accessibility-features)

      [Recommandations en matière de conception d’applications accessibles](https://docs.microsoft.com/windows/desktop/uxguide/inter-accessibility)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![Plateforme WPF](images/platform-wpf2-small.png)

      **Windows Presentation Foundation (WPF)**

      Développez des applications et des outils accessibles sur la plateforme établie pour les applications Windows gérées avec un modèle d’interface utilisateur XAML et le .NET Framework.

      [Meilleures pratiques d’accessibilité](https://docs.microsoft.com/dotnet/framework/ui-automation/accessibility-best-practices)

      [Notions de base d’UI Automation](https://docs.microsoft.com/dotnet/framework/ui-automation/index)

      [Fournisseurs UI Automation pour le code managé](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-providers-for-managed-code)

      [Clients UI Automation pour le code managé](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-clients-for-managed-code)

      [Modèles de contrôle UI Automation](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-patterns)

      [Modèle de texte UI Automation](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-text-pattern)

      [Types de contrôle UI Automation](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-types)

      [Spécification UI Automation et promesse de la communauté](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-specification-and-community-promise)
   :::column-end:::
   :::column:::
      ![Applications de plateforme Windows Forms](images/platform-winforms.png)

      **Windows Forms (WinForms)**

      Développez des applications et des outils accessibles pour les applications Windows managées avec un modèle d’interface utilisateur XAML et le .NET Framework.

      [Accessibilité Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)

      [Création d’une application Windows accessible](https://docs.microsoft.com/dotnet/framework/winforms/advanced/walkthrough-creating-an-accessible-windows-based-application)

      [Propriétés sur les contrôles Windows Forms qui prennent en charge les règles d’accessibilité](https://docs.microsoft.com/dotnet/framework/winforms/advanced/properties-on-windows-forms-controls-that-support-accessibility-guidelines)

      [Fournir des informations d’accessibilité pour les contrôles d’un Windows Form](https://docs.microsoft.com/dotnet/framework/winforms/controls/providing-accessibility-information-for-controls-on-a-windows-form)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **Accessibilité Web**

      Concevez, créez et testez des sites Web accessibles dans Microsoft Edge.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Présentation de l’accessibilité du Web](https://docs.microsoft.com/microsoft-edge/accessibility)

      [Conception de sites web accessibles](https://docs.microsoft.com/microsoft-edge/accessibility/design)
   :::column-end:::
   :::column:::
      [Développement de sites web accessibles](https://docs.microsoft.com/microsoft-edge/accessibility/build)

      [Test de sites Web accessibles](https://docs.microsoft.com/microsoft-edge/accessibility/test)
   :::column-end:::
:::row-end:::

## <a name="samples"></a>Exemples

Téléchargez et exécutez des exemples Windows complets qui illustrent diverses fonctionnalités et fonctionnalités d’accessibilité.

:::row:::
   :::column:::
      [Navigateur de l’exemple de code](https://docs.microsoft.com/en-us/samples/browse/)

      Le nouveau navigateur d’exemples qui remplace la Galerie de code MSDN.
   :::column-end:::
   :::column:::
      [Galerie de code MSDN (retirée)](https://code.msdn.microsoft.com/site/search?query=accessibility&f%5B0%5D.Value=accessibility&f%5B0%5D.Type=SearchText&ac=2)

      Téléchargez des exemples pour Windows, Windows Phone, Microsoft Azure, Office, SharePoint, Silverlight et d’autres produits.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Exemples classiques Windows sur GitHub](https://github.com/microsoft/Windows-classic-samples/search?q=accessibility&unscoped_q=accessibility)

      Ces exemples illustrent les fonctionnalités et le modèle de programmation pour Windows et Windows Server. 
   :::column-end:::
   :::column:::
      [Exemples de plateforme Windows universelle (UWP) sur GitHub](https://github.com/microsoft/Windows-universal-samples/search?q=accessibility&unscoped_q=accessibility)

      Ces exemples illustrent les modèles d’utilisation des API pour la plateforme Windows universelle (UWP) dans le kit de développement logiciel (SDK) Windows pour Windows 10.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [Galerie de contrôles XAML](https://github.com/microsoft/Xaml-Controls-Gallery)

      Cette application montre les différents contrôles XAML pris en charge dans le système de conception Fluent.
   :::column-end:::
:::row-end:::

## <a name="videos"></a>Vidéos

Différentes vidéos qui décrivent comment créer des applications Windows accessibles à des problèmes d’accessibilité généraux et comment Microsoft les traite.

:::row:::
   :::column:::
      **Comment prendre en main l’accessibilité dans les applications Windows**
   :::column-end:::
   :::column:::
      **Une minute de développement : Développement d’applications pour l’accessibilité**
   :::column-end:::
   :::column:::
      **Présentation de l’invalidité et de l’accessibilité**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2017/P4072/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/Kl4CT4DaypM]
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      **Du pirate au produit, contrôle visuel pour Windows 10**
   :::column-end:::
   :::column:::
      **Accessibilité sur Windows 10**
   :::column-end:::
   :::column:::
      **Présentation de la création d’applications UWP accessibles**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/AShNPfmAkvY]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2016/P541/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2016/P497/player]
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      **Conception des fonctionnalités d’accessibilité Windows**
   :::column-end:::
   :::column:::
      **Les fonctionnalités d’accessibilité de Windows 10 confèrent à tous**
   :::column-end:::
   :::column:::
      **Rendre les pointeurs de souris plus faciles à voir**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/Y_NJbE7wxlk]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/BseTf-4q9GA]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/4UzaF7_T3bw]
   :::column-end:::
:::row-end:::

## <a name="other-resources"></a>Autres ressources

:::row:::
   :::column span="3":::
      **Blogs et Actualités**

      La dernière version du monde de l’accessibilité Microsoft.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Dans les actualités](https://news.microsoft.com/presskits/accessibility/)
   :::column-end:::
   :::column:::
      [Blogs sur l’accessibilité](https://blogs.microsoft.com/accessibility/)
   :::column-end:::
   :::column:::
      [Blogs Windows UI Automation](https://blogs.msdn.microsoft.com/winuiautomation/)
   :::column-end:::
:::row-end:::

:::row:::
   :::column span="3":::
      **Communauté et support**

      Emplacement où les développeurs et les utilisateurs Windows rencontrent et apprennent ensemble.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Communauté Windows-accessibilité](https://community.windows.com/search?q=accessibility)
   :::column-end:::
   :::column:::
      [Forum de développement de l’accessibilité et de l’accessibilité Windows](https://social.msdn.microsoft.com/Forums/windows/home?forum=windowsaccessibilityandautomation)
   :::column-end:::
   :::column:::
      [Service de réponse d’invalidité](https://www.microsoft.com/Accessibility/disability-answer-desk)
   :::column-end:::
:::row-end:::
