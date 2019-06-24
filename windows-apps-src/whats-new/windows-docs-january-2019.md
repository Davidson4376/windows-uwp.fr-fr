---
title: Nouveautés apportées dans la documentation Windows en janvier 2019 – Développer des applications UWP
description: De nouvelles fonctionnalités, des vidéos et des conseils aux développeurs ont été ajoutés à la documentation du développeur Windows 10 en janvier 2019
keywords: nouveautés, mise à jour, fonctionnalités, conseils aux développeurs, Windows 10
ms.date: 01/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: beb80c28866b8f8207f203b70cb504dcd034098d
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63800582"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>Nouveautés apportées dans la documentation du développeur Windows en janvier 2019

La documentation du développeur Windows est constamment mise à jour avec des informations sur les nouvelles fonctionnalités mises à la disposition des développeurs sur la plateforme Windows. Les présentations de fonctionnalités, conseils aux développeurs et vidéos ci-dessous ont été mis à disposition au mois de janvier.

[Installez les outils et le kit de développement logiciel (SDK)](https://go.microsoft.com/fwlink/?LinkId=821431) sur Windows 10 et vous pourrez ainsi [créer une application universelle Windows](../get-started/create-uwp-apps.md) ou explorer la procédure permettant d’utiliser votre [code d’application existant sur Windows](../porting/index.md).

## <a name="features"></a>Fonctionnalités

### <a name="windows-development-on-microsoft-learn"></a>Développement Windows sur Microsoft Learn

Microsoft Learn offre de nouvelles possibilités d’apprentissage et de formation pratiques aux développeurs Microsoft. Si vous souhaitez apprendre à développer des applications Windows, notre [nouveau parcours d’apprentissage](https://docs.microsoft.com/learn/paths/develop-windows10-apps/) fournit une présentation complète de la plateforme, des outils et de la manière d’écrire vos premières applications.

![Image du parcours d’apprentissage du développement Windows](images/windows-learn.png)

### <a name="direct-3d-12"></a>Direct 3D 12

[Les passes de rendu Direct 3D 12](/windows/desktop/direct3d12/direct3d-12-render-passes) peuvent améliorer les performances de votre convertisseur si celui-i est basé, entre autres techniques, sur celle de rendu de différé basé sur une vignette (Tile-Based Deferred Rendering, TBDR). Cette technique aide votre convertisseur à améliorer l’efficacité de l’unité centrale graphique (GPU) en permettant à votre application de mieux identifier les besoins en rendu de ressources et les dépendances de données, ce qui contribue à réduire le trafic d’échange avec la mémoire sur puce.

### <a name="msix-modification-packages"></a>Packages de modification MSIX

Windows 10 version 1809 a amélioré la prise en charge des [packages de modification MSIX](https://docs.microsoft.com/windows/msix/modification-package-1809-update). Les packages de modification peuvent inclure des plug-ins basés sur un registre et une personnalisation associée. Ils permettent à une application déployée via MSIX d’utiliser un registre virtuel et de s’exécuter comme prévu.

![Création de package de modification MSIX](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>Open source de WPF, Windows Forms et WinUI

Les infrastructures WPF, Windows Forms et WinUI UX sont désormais disponibles pour les contributions open source sur GitHub. Pour plus d’informations et des liens, voir le [blog sur la création d’applications Windows](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97).

### <a name="progressive-web-apps-for-xbox"></a>Applications web progressives (PWA) pour Xbox

Les [Applications web progressives (PWA) pour Xbox One](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations) vous permettent d’étendre une application web afin de la rendre disponible en tant qu’application Xbox One via le Microsoft Store tout en continuant d’utiliser vos infrastructures existantes, le réseau de distribution de contenu (CDN) et le serveur principal. Dans l’ensemble, vous pouvez empaqueter votre PWA pour Xbox One de la même manière que pour Windows. Il existe néanmoins quelles différences importantes décrites dans ce guide.

### <a name="windows-machine-learning"></a>Windows Machine Learning

Nous avons restructuré [la page d’accueil pour les API WinML](https://docs.microsoft.com/windows/ai/api-reference) et ajouté une nouvelle documentation pour l’opérateur personnalisé WinML et les API natives.

La rubrique [Former un modèle avec PyTorch](https://docs.microsoft.com/windows/ai/train-model-pytorch) fournit des conseils sur la façon d’effectuer l’apprentissage d’un modèle à l’aide de l’infrastructure PyTorch localement ou dans le cloud. Vous pouvez ensuite télécharger ce modèle en tant que ONNX fichier et l’utiliser dans vos applications WinML.

![Graphique WinML](images/winml-graphic.png)

## <a name="developer-guidance"></a>Conseils aux développeurs

### <a name="choose-your-platform"></a>Choisir votre plateforme

Vous souhaitez créer une application de bureau ? Découvrez notre page revisitée [Choisir votre plateforme](https://docs.microsoft.com/windows/desktop/choose-your-technology) qui fournit des descriptions et comparaisons détaillées des plateformes UWP, WPF et Windows Forms, ainsi que des informations supplémentaires sur l’API Win32.

### <a name="faqs-on-win32-webview"></a>FAQ sur Win32 WebView

Notre [Forum aux questions](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs) fournit des réponses aux questions fréquemment posées sur l’utilisation de Microsoft Edge WebView dans des applications de bureau, ainsi que des liens vers des exemples et ressources supplémentaires.

### <a name="japanese-era-change"></a>Changement d’ère impériale japonaise

La rubrique [Préparer votre application pour le changement d’ère du Japon](../design/globalizing/japanese-era-change.md) explique comment vous assurer que votre application Windows est prête pour le changement d’ère impériale japonaise le 1er mai 2019. [Cette page est également disponible en japonais](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change).

## <a name="videos"></a>Vidéos

### <a name="progressive-web-apps"></a>Applications Web progressives

Les Applications web progressives sont des sites web qui fonctionnent comme des applications natives dans différents navigateurs et sur un vaste éventail d’appareils Windows 10. [Regardez la vidéo](https://youtu.be/ugAewC3308Y) pour en savoir plus, puis [consultez les documents](https://aka.ms/Windows-PWA) pour commencer.

### <a name="vs-code-series"></a>Série sur Visual Studio Code

Découvrez notre [nouvelle série de vidéos sur Visual Studio Code](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx) pour en savoir plus sur VSCode, la manière de l’utiliser, et comment il a été créé.

### <a name="one-dev-question"></a>One Dev Question

Dans le cadre de la série de vidéos One Dev Question, des développeurs Microsoft expérimentés répondent à toutes sortes de questions sur le développement, la culture d’équipe et l’histoire de Windows. Voici les dernières questions auxquelles nous avons répondu !

Raymond Chen :

* [Pourquoi avoir Program Files et Program Files (x86) ?](https://youtu.be/N7o9eJpFYco)

Larry Osterman :

* [Pourquoi COM est-il si compliqué ?](https://youtu.be/-gkXAV-StVA )
* [Comment s’est passé votre premier entretien pour Microsoft ?](https://youtu.be/qRb6otsHG5c)
