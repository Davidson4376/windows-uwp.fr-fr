---
author: Xansky
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: Révision des notes de publication des bibliothèques Microsoft Advertising.
title: Notes de publication des bibliothèques de publicités
ms.author: mhopkins
ms.date: 08/23/2017
ms.topic: article
keywords: windows 10, uwp, annonces, publicité, notes de publication
ms.localizationpriority: medium
ms.openlocfilehash: dbe932eb9391a4de0304b4be42944b2bced3287a
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6151263"
---
# <a name="release-notes-for-the-advertising-libraries"></a>Notes de publication des bibliothèques de publicités




Cette section propose des notes de publication relatives à la version actuelle des bibliothèques Microsoft Advertising. Ces bibliothèques prennent en charge les applications XAML et JavaScript/HTML pour Windows 10, Windows8.1, Windows Phone 8.1 et WindowsPhone8.

## <a name="installation"></a>Installation


Les bibliothèques de publicités Microsoft sont désormais disponibles au sein du kit [SDK Microsoft Advertising](http://aka.ms/ads-sdk-uwp). Pour plus d’informations sur l’installation du SDK, voir la section [Installer le SDK Microsoft Advertising](install-the-microsoft-advertising-libraries.md).

## <a name="uninstall-previous-versions"></a>Désinstaller les versions précédentes

Avant d’installer la toute dernière version du SDK Microsoft Advertising, il est recommandé de désinstaller toutes ses instances précédentes. Pour plus d’informations, voir [Installer le SDK MicrosoftAdvertising](install-the-microsoft-advertising-libraries.md).

## <a name="target-architecture-specific-build-outputs"></a>Cibler des sorties de génération propres à une architecture

Lorsque vous utilisez les bibliothèques de publicités Microsoft, vous ne pouvez pas cibler **TouteUC** dans votre projet. Si votre projet cible la plateforme **TouteUC**, vous pouvez voir un message d’avertissement dans votre projet une fois que vous avez ajouté une référence aux bibliothèques de publicités Microsoft. Pour supprimer cet avertissement, mettez à jour votre projet pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Pour plus d’informations, voir [Problèmes connus](known-issues-for-the-advertising-libraries.md).

## <a name="c-support"></a>Support C++

Les bibliothèques de publicités Microsoft (qui incluent les classes **AdControl** et **InterstitialAd**) prennent en charge les applications écrites en C++ et DirectX à l’aide de l’interopérabilité de Windows Runtime (*interop*). Pour plus d’informations et d’exemples sur la programmation en C++ et XAML, voir [Système de types](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx).

## <a name="no-toolbox-control"></a>Aucun contrôle de boîte à outils

Dans la version actuelle des bibliothèques de publicités Microsoft contenues dans le kit [SDK Microsoft Advertising](http://aka.ms/ads-sdk-uwp), il n’existe aucun contrôle de boîte à outils permettant de glisser une classe **AdControl** ou **InterstitialAd** vers une aire de conception de votre application. Pour obtenir des instructions sur l’ajout de ces contrôles dans votre balisage et votre code, voir [Procédures pas à pas de développement](developer-walkthroughs.md).

## <a name="latitude-and-longitude-properties-no-longer-available"></a>Les propriétés latitude et longitude ne sont plus disponibles

La classe **AdControl** ne possède plus les propriétés **Latitude** et **Longitude** pour les applicationsUWP. À la place, le code intégré au contrôle de publicité détecte et envoie ces valeurs aux serveurs de publicités pour le compte de l’application.


 

 
