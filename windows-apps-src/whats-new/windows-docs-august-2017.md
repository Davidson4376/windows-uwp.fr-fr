---
title: Nouveautés apportées dans la documentation Windows en août 2017 - Développer des applications UWP
description: De nouvelles fonctionnalités, des vidéos et des conseils aux développeurs ont été ajoutés à la documentation du développeur Windows 10 en août 2017
keywords: nouveautés, mise à jour, fonctionnalités, conseils aux développeurs, Windows 10, 1708
ms.date: 08/03/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 52b929f2b6f95c2be2feb68a6221a606ca3715e5
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67821130"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2017"></a>Nouveautés apportées dans la documentation du développeur Windows en août 2017

La documentation du développeur Windows est constamment mise à jour avec des informations sur les nouvelles fonctionnalités mises à la disposition des développeurs sur la plateforme Windows. Les présentations de fonctionnalités, les conseils aux développeurs et les vidéos qui suivent ont été récemment mis à disposition. Ils contiennent des informations nouvelles et mises à jour destinées aux développeurs Windows.

[Installez les outils et le kit de développement logiciel (SDK)](https://go.microsoft.com/fwlink/?LinkId=821431) sur Windows 10 et vous pourrez ainsi [créer une application universelle Windows](../get-started/your-first-app.md) ou explorer la procédure permettant d’utiliser votre [code d’application existant sur Windows](../porting/index.md).

## <a name="features"></a>Fonctionnalités

### <a name="windows-template-studio"></a>Windows Template Studio

Utilisez la nouvelle extension [Windows Template Studio](https://aka.ms/wtsinstall) pour Visual Studio 2019 afin de créer rapidement une application UWP avec les pages, le framework et les fonctionnalités que vous souhaitez. Cette expérience de type Assistant implémente des modèles éprouvés et les meilleures pratiques pour vous permettre d’ajouter des fonctionnalités dans votre application, rapidement et sans aucun problème.

![Windows Template Studio](images/template-studio.png)

### <a name="conditional-xaml"></a>XAML conditionnel

Vous pouvez désormais afficher un aperçu du code [XAML conditionnel](../debug-test-perf/conditional-xaml.md) afin de créer des [applications adaptatives de version](../debug-test-perf/version-adaptive-apps.md). Le XAML conditionnel vous permet d’utiliser la méthode ApiInformation.IsApiContractPresent dans le balisage XAML, afin que vous puissiez définir des propriétés et instancier des objets dans le balisage en fonction de la présence d’une API, sans avoir à utiliser le code-behind.

### <a name="game-mode"></a>Mode jeu

Les API [Mode Jeu](https://docs.microsoft.com/previous-versions/windows/desktop/gamemode/game-mode-portal) pour la plateforme Windows universelle (UWP) vous permettent de créer une expérience de jeu optimale, en tirant parti du mode jeu de Windows 10. Ces API sont situées dans l’en-tête **&lt;expandedresources.h&gt;** .

![Mode jeu](images/game-mode.png)

### <a name="submission-api-supports-video-trailers-and-gaming-options"></a>L’API de soumission prend en charge les bandes-annonces vidéo et les options de jeu

L’[API de soumission au Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) vous permet désormais d’inclure des [bandes-annonces vidéo](../monetize/manage-app-submissions.md#trailer-object) et des [options de jeu](../monetize/manage-app-submissions.md#gaming-options-object) avec vos soumissions d’applications.


## <a name="developer-guidance"></a>Conseils aux développeurs

### <a name="data-schemas-for-store-products"></a>Schémas de données des produits du Store

Nous avons ajouté l’article [Schémas de données des produits du Store](../monetize/data-schemas-for-store-products.md). Cet article fournit des schémas pour les données relatives au Store disponibles pour plusieurs objets dans l’espace de noms [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store), y compris [StoreProduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) et [StoreAppLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense).

### <a name="desktop-bridge"></a>Pont du bureau

Nous avons ajouté deux guides afin de vous aider à ajouter des expériences modernes qui se déclenchent pour les utilisateurs de Windows 10.

Voir [Améliorer votre application de bureau pour Windows 10](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-enhance) pour rechercher et référencer les fichiers appropriés, puis écrire le code pour déclencher les expériences UWP pour les utilisateurs de Windows 10.  

Voir [Étendre votre application de bureau avec des composants UWP modernes](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-extend) pour incorporer des interfaces utilisateur XAML modernes et d’autres expériences UWP qui doivent s’exécuter dans un conteneur d’application UWP.

### <a name="getting-started-with-point-of-service"></a>Prise en main de la technologie de point de vente

Nous avons ajouté un guide pour vous aider avec la [prise en main des appareils de point de vente](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-get-started). Il aborde des sujets comme l’énumération des appareils, la vérification des fonctionnalités des appareils, la revendication d’appareils et le partage d’appareils. 

### <a name="xbox-live"></a>Xbox Live

Nous avons ajouté de la documentation pour les développeurs Xbox Live, à la fois pour les jeux du Kit de développement Xbox (XDK) et de la plateforme Windows universelle (UWP).

Consultez le [Guide du développeur Xbox Live](https://docs.microsoft.com//gaming/xbox-live/index) pour savoir comment utiliser les API Xbox Live afin de connecter votre jeu au réseau social de jeux Xbox Live.

Avec le [Programme Créateurs Xbox Live](https://docs.microsoft.com//gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators), tout développeur de jeux UWP peut développer et publier un jeu Xbox Live à la fois sur PC et sur Xbox One.

Consultez la [Vue d’ensemble du programme pour les développeurs Xbox Live](https://docs.microsoft.com//gaming/xbox-live/developer-program-overview) pour plus d’informations sur les programmes et les fonctionnalités disponibles pour les développeurs Xbox Live.

## <a name="videos"></a>Vidéos

### <a name="mixed-reality"></a>Réalité mixte

Une série de didacticiels sous forme vidéo a été publiée pour [Microsoft HoloLens cours 250](https://developer.microsoft.com/en-us/windows/mixed-reality/mixed_reality_250). Si vous avez déjà installé les outils et que vous êtes déjà familiarisé avec les principes de développement de base pour la réalité mixte, consultez ces formations vidéo pour plus d’informations sur la création d’expériences partagées sur des appareils de réalité mixte.

### <a name="narrator-and-dev-mode"></a>Narrateur et mode développeur

Vous savez peut-être déjà que vous pouvez utiliser le [Narrateur](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator) pour tester l’expérience de lecture d’écran de votre application. Toutefois, le Narrateur intègre également un mode développeur, qui vous offre une bonne représentation visuelle des informations qui lui sont exposées. [Regardez la vidéo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode), puis découvrez plus en détail le [mode développeur du Narrateur](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode).

### <a name="windows-template-studio"></a>Windows Template Studio

Une présentation plus détaillée de Windows Template Studio est fournie dans [cette vidéo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Getting-Started-with-Windows-Template-Studio). Lorsque vous êtes prêt, [installez l’extension](https://aka.ms/wtsinstall) ou [vérifiez le code source et la documentation](https://aka.ms/wtsinstall).