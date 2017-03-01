---
author: QuinnRadich
title: "Nouveautés de Windows 10 version 1607 Preview"
description: "Windows 10 version 1607 Preview et les nouveaux outils de développement offrent les outils, fonctionnalités et expériences optimisés par la nouvelle plateforme Windows universelle."
keywords: "nouveautés, nouveauté, mise à jour, mises à jour, fonctionnalités, nouveau, Windows 10, 1607 Preview"
ms.author: quradic
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.assetid: 835d5393-427f-4155-a737-d509ea1de99f
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 51076aed4ae55956164efaf3a3c01461e4b7338b
ms.lasthandoff: 02/08/2017

---

# <a name="whats-new-in-windows"></a>Nouveautés de Windows

Windows 10, version 1607 Preview et les mises à jour des outils de développement de Windows continuent de fournir les outils, fonctionnalités et expériences optimisés par la plateforme Windows universelle. [Installez les outils et le kit de développement logiciel (SDK)](http://go.microsoft.com/fwlink/?LinkId=821431) sur Windows 10 et vous pourrez ainsi [créer une application universelle Windows](https://msdn.microsoft.com/library/windows/apps/bg124288) ou explorer la procédure permettant d’utiliser votre [code d’application existant sur Windows](https://msdn.microsoft.com/library/windows/apps/mt238321).

## <a name="windows-10-version-1607-preview"></a>Windows 10, version 1607 Preview

Fonctionnalité | Description
 :---- | :----
Mise en réseau | Vous pouvez maintenant fournir votre propre validation personnalisée des certificats SSL/TLS de serveur en vous abonnant à l’événement [HttpBaseProtocolFilter.ServerCustomValidationRequest](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx#_blank). Vous pouvez aussi entièrement désactiver la lecture des réponses HTTP à partir du cache en spécifiant la valeur d’énumération [HttpCacheReadBehavior.NoCache](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpcachereadbehavior.aspx#_blank) dans une requête HTTP. Il est désormais possible d’effacer les informations d’identification d’authentification pour rendre possible un scénario de « fin de session » en appelant la méthode [HttpBaseProtocolFilter.ClearAuthenticationCache](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx#_blank).
Extensions | Microsoft Edge permet désormais d’utiliser des extensions. Avec les extensions, les utilisateurs peuvent étendre les capacités de Microsoft Edge, fournissant ainsi une fonctionnalité de niche essentielle pour les audiences ciblées. Pour plus d’informations, consultez la [documentation sur les extensions](https://developer.microsoft.com/microsoft-edge/platform/documentation/extensions/#_blank).
API Bluetooth | Les applications peuvent maintenant accéder aux services RFCOMM sur des périphériques Bluetooth distants par le biais de [Windows.Devices.Bluetooth et Windows.Devices.Bluetooth.Rfcomm](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.aspx#_blank) sans avoir à s’associer d’abord avec le périphérique. De nouvelles méthodes permettent aux applications de rechercher et d’accéder aux services RFCOMM sur des appareils non associés.
API de chat | Avec la nouvelle classe [ChatSyncManager](https://msdn.microsoft.com/library/windows/apps/mt414181.aspx#_blank), vous pouvez synchroniser des SMS à destination et à partir du cloud.
[Mappage de concepts d’applications Windows pour développeurs iOS et Android](https://msdn.microsoft.com/windows/uwp/porting/android-ios-uwp-map#_blank) | Si vous êtes un développeur doté de compétences relatives aux systèmes d’exploitation Android ou iOS et au code et que vous souhaitez migrer vers Windows 10 et la plateforme Windows universelle (UWP), cette ressource vous permettra de mapper les fonctionnalités de la plateforme, et vos connaissances, entre les trois plateformes.
[Protection des données d’entreprise, Enterprise data protection (EDP)](https://msdn.microsoft.com/windows/uwp/enterprise/wip-hub) | EDP est un ensemble de fonctionnalités pour la gestion des périphériques mobiles (GPM) sur les postes de travail, les ordinateurs de bureau, les tablettes et les téléphones. EDP permet à une entreprise de mieux contrôler la gestion de ses données (fichiers d’entreprise et objets Blob relatifs aux données) sur les périphériques qu’elle gère.
[Windows.ApplicationModel.AppExtensions](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appextensions.aspx#_blank) | Le nouvel espace de noms AppExtensions permet à votre application Windows Store d’héberger le contenu fourni par d’autres applications Windows Store. Vous pouvez découvrir, énumérer et accéder à du contenu en lecture seule à partir de ces applications.
Windows IoT | Windows 10 IoT Standard vous permet de créer des applications IoT au sein de l’environnement Windows familier, et est maintenant disponible sur Raspberry Pi 3, la nouvelle carte Raspberry Pi.
API de médias | Les nouvelles API MediaBreak dans l’espace de noms Windows.Media.Playback vous permettent de planifier et de gérer facilement les coupures de médias lors de la lecture de médias à l’aide de MediaSource et MediaPlaybackItem. Les nouvelles API AudioGraph dans l’espace de noms Windows.Media.Audio ajoutent un traitement audio spatial qui vous permet d’attribuer des émetteurs et des écouteurs en position 3D aux nœuds de graphiques audio.
API de cartes | Le contrôle [MapControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx#_blank) a été amélioré pour permettre aux développeurs d’obtenir une région visible proche de l’appareil photo, en excluant les régions très éloignées et proches de l’horizon dans une vue très nette. La classe [MapLocationFinder](https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.aspx#_blank) a été étendue, permettant aux développeurs d’optimiser le trafic réseau lors d’un géocodage inverse en spécifiant une précision souhaitée. Les développeurs peuvent maintenant tirer parti du téléchargement de cartes hors ligne à l’aide de la méthode [LaunchUriAsync](https://msdn.microsoft.com/library/windows/apps/hh701480.aspx#_blank) et en spécifiant la latitude et la longitude. Pour plus d’informations, voir [Lancer l’application Cartes Windows](https://msdn.microsoft.com/windows/uwp/launch-resume/launch-maps-app#_blank).

