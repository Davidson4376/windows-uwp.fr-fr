---
author: drewbatgit
ms.assetid: c43d4af3-9a1a-4eae-a137-1267c293c1b5
description: Cet article vous explique comment valoriser les fonctionnalités spécifiques d’interface utilisateur d’appareil photo présentes de manière exclusive sur les appareils mobiles.
title: Fonctionnalités d’interface utilisateur de caméra pour les appareils mobiles
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 062ca589b2a63db1f56ada4e2df4f99bc7f8b0fb
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5921342"
---
#<a name="camera-ui-features-for-mobile-devices"></a>Fonctionnalités d’interface utilisateur de caméra pour les appareils mobiles

Cet article vous explique comment valoriser les fonctionnalités spécifiques d’interface utilisateur d’appareil photo présentes de manière exclusive sur les appareils mobiles. 

## <a name="add-the-mobile-extension-to-your-project"></a>Ajouter l’extension mobile à votre projet 

Pour utiliser ces fonctionnalités, vous devez ajouter à votre projet une référence au kit de développement logiciel (SDK) Microsoft Mobile Extension pour la plateforme d’application universelle.

**Pour ajouter une référence au kit de développement logiciel (SDK) de l’extension mobile pour la prise en charge du bouton de l’appareil photo, procédez comme suit:**

1.  Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis cliquez sur **Ajouter une référence**.

2.  Développez le nœud **Universelle Windows** et sélectionnez**Extensions**.

3.  Sélectionnez la case **Kit de développement logiciel Microsoft Mobile Extension pour la plateforme d’application universelle**.

## <a name="hide-the-status-bar"></a>Masquer la barre d’état

Les appareils mobiles disposent d’un contrôle [**StatusBar**](https://msdn.microsoft.com/library/windows/apps/dn633864) qui fournit à l’utilisateur des informations relatives à l’appareil. Ce contrôle occupe de l’espace sur l’écran , ce qui peut interférer avec l’interface utilisateur de capture multimédia. Vous pouvez masquer la barre d’état en appelant [**HideAsync**](https://msdn.microsoft.com/library/windows/apps/dn610339). Toutefois cet appel doit être effectué depuis un bloc conditionnel où vous utilisez la méthode [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/dn949016) pour déterminer si l’API est disponible. Cette méthode renvoie uniquement la valeur true sur les appareils mobiles qui prennent en charge la barre d’état. Vous devez masquer la barre d’état au lancement de votre application ou lorsque vous commencez à afficher un aperçu à partir de l’appareil photo.

[!code-cs[HideStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHideStatusBar)]

Lorsque votre application s’arrête ou lorsque l’utilisateur quitte la page de capture multimédia de votre application, vous pouvez rendre le contrôle de nouveau visible.

[!code-cs[ShowStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetShowStatusBar)]

## <a name="use-the-hardware-camera-button"></a>Utiliser le bouton matériel de l’appareil photo

Certains appareils mobiles disposent d’un bouton matériel dédié à l’appareil photo que certains utilisateurs préfèrent à une commande tactile. Pour être averti de l’utilisation du bouton matériel de l’appareil photo, enregistrez un gestionnaire pour l’événement [**HardwareButtons.CameraPressed**](https://msdn.microsoft.com/library/windows/apps/dn653805). Cette API est disponible sur les appareils mobiles, par conséquent, vous devez utiliser de nouveau l’élément **IsTypePresent** pour vous assurer que l’API est prise en charge sur l’appareil actuel avant d’essayer d’y accéder.

[!code-cs[PhoneUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhoneUsing)]

[!code-cs[RegisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterCameraButtonHandler)]

Dans le gestionnaire de l’événement **CameraPressed**, vous pouvez lancer une capture de photos.

[!code-cs[CameraPressed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCameraPressed)]

Lorsque votre application s’arrête ou que l’utilisateur quitte la page de capture multimédia de votre application, annulez l’enregistrement du gestionnaire de boutons matériels.

[!code-cs[UnregisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterCameraButtonHandler)]

## <a name="related-topics"></a>Rubriquesconnexes

* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)





