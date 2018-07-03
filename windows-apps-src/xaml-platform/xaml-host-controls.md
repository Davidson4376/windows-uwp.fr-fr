---
author: normesta
description: Ce guide vous aide à créer des interfaces utilisateur UWP Fluent directement dans vos applications WPF et Windows Forms
title: Hébergez des contrôles UWP dans les applications WPF et Windows Forms
ms.author: normesta
ms.date: 05/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp, windows forms, wpf
keywords: windows10, uwp, windows forms, wpf
ms.localizationpriority: medium
ms.openlocfilehash: 4823654bce3373ace5b04ced8ec14c4b6c1b6f1d
ms.sourcegitcommit: 3500825bc2e5698394a8b1d2efece7f071f296c1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
ms.locfileid: "1862068"
---
# <a name="host-uwp-controls-in-wpf-and-windows-forms-applications"></a>Hébergez des contrôles UWP dans les applications WPF et Windows Forms

> [!NOTE]
> Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.

Nous apportons les contrôles UWP aux ordinateurs de bureau pour que vous puissiez améliorer l’apparence et les fonctionnalités de vos applications WPF ou Windows existantes avec des fonctionnalités Fluent Design. Il y a deux façons de le faire.

Tout d’abord, vous pouvez ajouter des contrôles directement sur la surface de conception de votre projet WPF ou Windows Forms et les utiliser comme tout autre contrôle dans votre concepteur.  Essayez cette solution dès aujourd'hui avec le nouveau contrôle **WebView**. Ce contrôle utilise le moteur de rendu MicrosoftEdge et n'était jusqu'à présent disponible que pour les applications UWP. Vous pouvez trouver le contrôle **WebView** dans la dernière version du [Kit de ressources de la Communauté Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/).

Bientôt, vous aurez accès à encore plus de fonctionnalités Fluent Design: nous fournirons un contrôle qui vous permettra d’héberger de nombreux contrôles UWP. Recherchez ce contrôle et de nombreux autres contrôles dans les futures versions du Kit de ressources de la Communauté Windows.

Voici un aperçu de la façon dont ces contrôles sont organisés du point de vue architectural. Les noms utilisés dans ce diagramme sont sujets à modification.  

![Architecture de contrôle de l'hôte](images/host-controls.png)

Les API qui s’affichent en bas de ce diagramme sont livrés avec le SDK Windows.  Les contrôles que vous allez ajouter à votre concepteur sont fournis sous forme de packages Nuget dans le Kit de ressources de la Communauté Windows.

Ces nouveaux contrôles ayant des limitations, prenez le temps, avant de les utiliser, d’examiner ce qui n’est pas encore pris en charge et ce qui ne fonctionne qu'avec des solutions de contournement.

### <a name="whats-supported"></a>Fonctionnalités prises en charge

Pour l’essentiel, tout est pris en charge, sauf ce qui est explicitement indiqué sur la liste ci-dessous.

### <a name="whats-supported-only-with-workarounds"></a>Fonctionnalités prises en charge uniquement avec des solutions de contournement

:heavy_check_mark: héberge plusieurs contrôles de la boîte de réception dans les fenêtres multiples. Vous devrez placer chaque fenêtre dans son propre thread.

:heavy_check_mark: utilisation de ``x:Bind`` avec des contrôles hébergés. Vous devrez déclarer le modèle de données dans une bibliothèque .NET Standard.

:heavy_check_mark: contrôles tiers en C#. Si vous avez le code source d'un contrôle tiers, vous pouvez compiler en fonction de celui-ci.

### <a name="whats-not-yet-supported"></a>Fonctionnalités non encore prises en charge

:no_entry_sign: outils d'accessibilité qui fonctionnent en toute transparence dans l’application et les contrôles hébergés.

:no_entry_sign: contenu localisé dans les contrôles que vous ajoutez à des applications qui ne contiennent pas de package d’application Windows.

:no_entry_sign: références de ressources en XAML dans des applications qui ne contiennent pas de package d’application Windows.

:no_entry_sign: contrôles qui répondent correctement aux modifications de PPP et d'échelle.

:no_entry_sign: ajout d'un contrôle **WebView** à un contrôle utilisateur personnalisé (sur thread, hors thread ou hors processus).

:no_entry_sign: l'effet Fluent [surlignage Révéler](https://docs.microsoft.com/windows/uwp/design/style/reveal).

: no_entry_sign: entrée manuscrite Inline, @Places et @People pour les contrôles d’entrée.

:no_entry_sign: assignation de touches accélératrices.

:no_entry_sign: contrôles tiers en C++.

:no_entry_sign: hébergement de contrôles utilisateur personnalisés.

Les éléments de cette liste sont susceptibles de changer à mesure que nous continuons à améliorer l’’intégration de Fluent dans les ordinateurs de bureau.  
