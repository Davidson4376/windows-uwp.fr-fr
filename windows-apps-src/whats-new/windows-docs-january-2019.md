---
title: Nouveautés dans Windows Docs janvier 2019 - développer des applications UWP
description: Guide du développeur, des vidéos et des nouvelles fonctionnalités ont été ajoutés à la documentation pour développeurs Windows 10 pour janvier 2019
keywords: Quelles sont les nouveautés, mise à jour, des fonctionnalités, des instructions destinées aux développeurs, Windows 10, janvier
ms.date: 01/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: beb80c28866b8f8207f203b70cb504dcd034098d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636574"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>Nouveautés dans la documentation du développeur Windows janvier 2019

La documentation du développeur Windows est constamment mise à jour afin d'intégrer les informations sur les nouvelles fonctionnalités disponibles pour les développeurs sur la plateforme Windows. Les présentations des fonctionnalités, Guide du développeur et vidéos suivants ont été apportées disponibles dans le mois de janvier.

[Installez les outils et le kit de développement logiciel (SDK)](https://go.microsoft.com/fwlink/?LinkId=821431) sur Windows 10 et vous pourrez ainsi [créer une application universelle Windows](../get-started/create-uwp-apps.md) ou explorer la procédure permettant d’utiliser votre [code d’application existant sur Windows](../porting/index.md).

## <a name="features"></a>Fonctionnalités

### <a name="windows-development-on-microsoft-learn"></a>Développement de Windows sur Microsoft Learn

Microsoft Learn offre de nouvelles formations et les opportunités de formation aux développeurs de Microsoft. Si vous souhaitez apprendre à développer des applications de Windows, consultez [notre nouveau parcours d’apprentissage](https://docs.microsoft.com/learn/paths/develop-windows10-apps/) pour une introduction complète à la plateforme, les outils et comment écrire vos premières applications peu.

![Image du développement Windows parcours d’apprentissage](images/windows-learn.png)

### <a name="direct-3d-12"></a>Direct3D 12

[Direct3D 12 restituer passes](/windows/desktop/direct3d12/direct3d-12-render-passes) peut améliorer les performances de votre convertisseur si elle a basée sur le rendu de différé en mosaïque (TBDR), entre autres techniques. La technique vous aide à votre convertisseur pour améliorer l’efficacité GPU en permettant à votre application pour mieux identifier les dépendances de rendu de ressources tris de données et des besoins, ce qui réduit le trafic de mémoire à désactiver puce mémoire.

### <a name="msix-modification-packages"></a>Packages de modification MSIX

Windows 10 version 1809 prise en charge améliorée pour [packages de modification MSIX](https://docs.microsoft.com/windows/msix/modification-package-1809-update). Packages de modification peuvent inclure en fonction du Registre de plug-ins et de personnalisation associée et activera une application déployée via MSIX à utiliser un Registre virtuel et de s’exécuter comme prévu.

![Création d’un package MSIX modification](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>Open Source de WPF, Windows Forms et WinUI

Les infrastructures WPF, Windows Forms et WinUI UX sont désormais disponibles pour les contributions open source sur GitHub. Pour plus d’informations et des liens, consultez le [création de blog sur les applications Windows](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97).

### <a name="progressive-web-apps-for-xbox"></a>Applications Web progressif pour Xbox

Avec [Progressive Web Apps pour Xbox One](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations), vous pouvez étendre une application web et le rendre disponible en tant qu’une application Xbox One via Microsoft Store tout en continuant de toujours utiliser vos infrastructures existantes, le CDN et le serveur principal. La plupart du temps, vous pouvez empaqueter votre PWA pour Xbox One dans la même façon que vous le feriez pour Windows, toutefois, il existe plusieurs différences clés, que ce guide vous guidera.

### <a name="windows-machine-learning"></a>Apprentissage de Windows

Nous avons restructuré [la page d’accueil pour WinML APIs](https://docs.microsoft.com/windows/ai/api-reference)et ajouté la nouvelle documentation pour opérateur personnalisé WinML et des API natives.

[Former un modèle avec PyTorch](https://docs.microsoft.com/windows/ai/train-model-pytorch) fournit des conseils sur la façon de former un modèle à l’aide de l’infrastructure PyTorch localement ou dans le cloud. Vous pouvez ensuite télécharger ce modèle en tant que ONNX fichier et l’utiliser dans vos applications WinML.

![Graphique de WinML](images/winml-graphic.png)

## <a name="developer-guidance"></a>Conseils aux développeurs

### <a name="choose-your-platform"></a>Choisissez votre plateforme

Vous souhaitez créer une nouvelle application de bureau ? Découvrez notre version revisitée [Choisissez votre plateforme](https://docs.microsoft.com/windows/desktop/choose-your-technology) page pour obtenir une description détaillée et des comparaisons des plateformes UWP, WPF et Windows Forms et plus d’informations sur l’API Win32.

### <a name="faqs-on-win32-webview"></a>Questions fréquentes sur Win32 WebView

Notre [Forum aux questions](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs) fournit des réponses aux questions courantes lors de l’utilisation de Microsoft Edge WebView dans les applications de bureau, ainsi que des liens vers des exemples et ressources supplémentaires.

### <a name="japanese-era-change"></a>Modification de l’ère japonais

[Préparer votre application pour la modification de l’ère japonais](../design/globalizing/japanese-era-change.md) vous montre comment votre application est prête pour prendre la valeur de modification de l’ère japonais de Windows mettent sur 1 mai 2019. [Cette page est également disponible en japonais](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change).

## <a name="videos"></a>Vidéos

### <a name="progressive-web-apps"></a>Applications Web progressives

Les applications Web progressif sont des sites web qui fonctionnent comme des applications natives sur différents navigateurs et d’un large éventail d’appareils Windows 10. [Regardez la vidéo](https://youtu.be/ugAewC3308Y) pour en savoir plus, puis [consulter les documents](https://aka.ms/Windows-PWA) pour commencer.

### <a name="vs-code-series"></a>Série de Code de Visual Studio

Découvrez notre [nouvelle série de vidéos sur Visual Studio Code](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx) pour plus d’informations sur VSCode, comment l’utiliser, et comment elle a été créée.

### <a name="one-dev-question"></a>Une Question de développement

Dans la série de vidéos d’une Question de développement, les développeurs Microsoft contribue depuis longtemps couvrent une série de questions sur le développement Windows, la culture de l’équipe et l’historique. Voici les dernières questions que nous avons une réponse !

Raymond Chen :

* [Pourquoi vous avez les fichiers de programme et les fichiers de programme (x86) ?](https://youtu.be/N7o9eJpFYco)

Larry Osterman :

* [Pourquoi est COM si compliqué ?](https://youtu.be/-gkXAV-StVA )
* [Quel était votre premier entretien comme pour Microsoft ?](https://youtu.be/qRb6otsHG5c)
