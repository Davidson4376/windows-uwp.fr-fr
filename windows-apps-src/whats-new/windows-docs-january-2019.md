---
title: Quelles sont les nouveautés dans la documentation Windows en janvier 2019 - développer des applications UWP
description: Nouvelles fonctionnalités, des vidéos et des conseils aux développeurs ont été ajoutées à la documentation du développeur Windows 10 janvier 2019
keywords: Quelles sont les nouveautés, mise à jour, fonctionnalités, conseils de développeur, Windows 10, janvier
ms.date: 01/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: beb80c28866b8f8207f203b70cb504dcd034098d
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116251"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>Quelles sont les nouveautés dans la documentation du développeur Windows en janvier 2019

La documentation du développeur Windows est constamment mise à jour afin d'intégrer les informations sur les nouvelles fonctionnalités disponibles pour les développeurs sur la plateforme Windows. Les présentations de fonctionnalités, conseils aux développeurs et les vidéos suivantes ont été mis à disposition en janvier.

[Installez les outils et le kit de développement logiciel (SDK)](https://go.microsoft.com/fwlink/?LinkId=821431) sur Windows10 et vous pourrez ainsi [créer une application universelle Windows](../get-started/create-uwp-apps.md) ou découvrir comment utiliser votre [code d’application existant sur Windows](../porting/index.md).

## <a name="features"></a>Fonctionnalités

### <a name="windows-development-on-microsoft-learn"></a>Développement de Windows sur Microsoft Learn

Learn Microsoft fournit des nouveaux pratique et offres de formation pour les développeurs Microsoft. Si vous êtes intéressé par apprendre à développer des applications Windows, consultez [notre nouveau cursus](https://docs.microsoft.com/learn/paths/develop-windows10-apps/) pour une présentation détaillée de la plateforme, les outils et l’écriture de vos applications peu premier.

![Image du chemin d’accès d’apprentissage de développement Windows](images/windows-learn.png)

### <a name="direct-3d-12"></a>Direct3D 12

[Passes de rendu Direct3D 12](/windows/desktop/direct3d12/direct3d-12-render-passes) peut améliorer les performances de votre convertisseur s’il est basé sur le rendu de différé basée sur la vignette (TBDR), entre autres techniques. La technique permet de votre convertisseur pour améliorer l’efficacité GPU en permettant à votre application à mieux identifier les dépendances de rendu de ressource classement de demandes et des données et en réduisant ainsi le trafic de mémoire vers/depuis mémoire off puce.

### <a name="msix-modification-packages"></a>Packages de modification MSIX

Prise en charge de Windows 10 version 1809 améliorée pour les [packages de modification MSIX](https://docs.microsoft.com/windows/msix/modification-package-1809-update). Les packages de modification peuvent inclure des plug-ins basé sur Registre et personnalisation associée et permet d’activer une application déployée par le biais de MSIX d’utiliser un Registre virtuel et de fonctionner comme prévu.

![Création de package de modification MSIX](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>Open Source de WPF, Windows Forms et WinUI

Les infrastructures WPF, Windows Forms et WinUI UX sont désormais disponibles pour les contributions open source sur GitHub. Pour plus d’informations et des liens, consultez le [blog des applications Windows de création](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97).

### <a name="progressive-web-apps-for-xbox"></a>Applications Web progressives pour Xbox

Avec les [Applications Web progressives pour Xbox One](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations), vous pouvez étendre une application web et la rendre disponible en tant qu’une application Xbox One via le Microsoft Store tout en continuant de toujours utiliser vos infrastructures existantes, CDN et le serveur principal. Dans la plupart des cas, vous pouvez l’empaqueter votre PWA pour Xbox One de la même manière que vous le feriez pour Windows, toutefois, il existe plusieurs différences importantes que ce guide vous guide à travers.

### <a name="windows-machine-learning"></a>Apprentissage automatique Windows

Nous avons réorganisé [la page d’accueil pour les API WinML](https://docs.microsoft.com/windows/ai/api-reference)et ajoutés nouvelle documentation WinML d’opérateur personnalisée et les API natives.

[Entraîner un modèle avec PyTorch](https://docs.microsoft.com/windows/ai/train-model-pytorch) fournit des conseils sur la façon d’entraîner un modèle à l’aide de l’infrastructure PyTorch en local ou dans le cloud. Vous pouvez ensuite télécharger ce modèle sous forme de fichier ONNX et l’utiliser dans vos applications WinML.

![Graphique de WinML](images/winml-graphic.png)

## <a name="developer-guidance"></a>Conseils aux développeurs

### <a name="choose-your-platform"></a>Choisissez votre plateforme

Vous souhaitez créer une nouvelle application de bureau? Consultez notre page [Choisir votre plateforme](https://docs.microsoft.com/windows/desktop/choose-your-technology) revisitée pour obtenir une description détaillée et les comparaisons des plateformes UWP, WPF et Windows Forms et plus d’informations sur l’API Win32.

### <a name="faqs-on-win32-webview"></a>FAQ sur Win32 WebView

Notre [Forum aux questions](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs) fournit des réponses aux questions courantes lors de l’utilisation de Microsoft Edge WebView dans les applications de bureau, ainsi que des liens vers des exemples et des ressources supplémentaires.

### <a name="japanese-era-change"></a>Modification de l’ère japonais

[Préparer votre application pour l’ère japonais modifier](../design/globalizing/japanese-era-change.md) vous montre comment votre application est prête pour prendre la valeur de modification de l’ère japonais de Windows mettent 1 mai 2019. [Cette page est également disponible en japonais](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change).

## <a name="videos"></a>Vidéos

### <a name="progressive-web-apps"></a>Applications web progressives

Applications Web progressives sont des sites web qui fonctionnent comme des applications natives sur les navigateurs et un large éventail d’appareils Windows 10. [Regardez la vidéo](https://youtu.be/ugAewC3308Y) pour en savoir plus, puis [consultez les documents](https://aka.ms/Windows-PWA) de prise en main.

### <a name="vs-code-series"></a>Série de Code Visual Studio

Consultez notre [nouvelle série de vidéos sur Code Visual Studio](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx) pour plus d’informations sur ce qui est VSCode, son utilisation et comment il a été créé.

### <a name="one-dev-question"></a>Question sur le développement

Dans la série de vidéos Question sur le développement, les développeurs Microsoft longtime décrire une série de questions sur le développement Windows, la culture de l’équipe et l’historique. Voici les dernières questions que nous avons répondu!

Raymond Chen:

* [Pourquoi recourir Program Files et Program Files (x86)?](https://youtu.be/N7o9eJpFYco)

Larry Osterman:

* [Pourquoi est-il COM si compliqué?](https://youtu.be/-gkXAV-StVA )
* [Ce qui a été votre premier entretien tels que Microsoft?](https://youtu.be/qRb6otsHG5c)
