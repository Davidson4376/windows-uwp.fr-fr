---
Description: Découvrez comment ajouter des expériences utilisateur modernes pour les utilisateurs Windows 10 dans une application de bureau que vous avez empaqueté dans un package d’application Windows.
title: Moderniser les applications de bureau empaquetées
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 191a8b8a007a866f37780a7c52cd40047dc9817f
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215200"
---
# <a name="features-that-require-package-identity"></a>Fonctionnalités nécessitant l’identité du package

Si vous souhaitez mettre à jour de votre application de bureau avec [expériences modernes Windows 10](index.md), de nombreuses fonctionnalités sont disponibles uniquement dans les applications de bureau qui sont empaquetées dans un package MSIX.

MSIX est un format de package d’une application de Windows modern qui fournit une expérience de l’empaquetage universelle pour toutes les applications de Windows, WPF, Windows Forms et applications Win32. Empaquetage de vos applications de bureau Windows vous permet d’intégrer des expériences Windows 10 modernes tels que les vignettes dynamiques et des notifications dans vos applications. Il obtient également accès à une installation robuste et d’une expérience mise à jour, d’un modèle de sécurité gérée avec un système flexible de fonctionnalité, prise en charge pour le Microsoft Store, de gestion d’entreprise et de nombreux modèles de distribution personnalisée. Pour plus d’informations, consultez [Empaqueter des applications de bureau](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) dans la documentation MSIX.

Si vous empaquetez votre application de bureau, vous pouvez ensuite utiliser les API UWP qui nécessitent l’identité du package, les extensions de package et les composants UWP dans votre application empaquetée. Pour plus d’informations, consultez ces articles.

## <a name="use-uwp-apis-that-require-package-identity"></a>Utilisez des API UWP qui nécessitent l’identité du package

Certaines API UWP nécessitent [package identité](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) à utiliser dans une application de bureau. Le package MSIX (y compris le manifeste du package) fournit cette identité.

Pour plus d’informations, consultez [cette liste d’API](desktop-to-uwp-supported-api.md#list-of-apis).

## <a name="integrate-with-package-extensions"></a>Intégrer avec des extensions de package

Si votre application doit intégrer le système (par exemple : établir des règles de pare-feu), décrire ces éléments dans le manifeste du package de votre application et que le système s’occupe du reste. Pour la plupart de ces tâches, vous n’avez pas à écrire du code. Avec un peu de XML dans le manifeste, vous pouvez faire choses tels que démarrer un processus lorsque l’utilisateur ouvre une session, intégrer votre application dans l’Explorateur de fichiers et ajouter votre application une liste de cibles impression qui s’affichent dans d’autres applications.

Pour plus d’informations, consultez [intégrer votre application de bureau avec des extensions de package](desktop-to-uwp-extensions.md).

## <a name="extend-with-uwp-components"></a>Étendre à l’aide de composants UWP

Certaines expériences Windows 10 (par exemple: une page d'interface utilisateur tactile) doivent s'exécuter à l'intérieur d'un conteneur d'application moderne. En règle générale, vous devez d’abord déterminer si vous pouvez ajouter votre expérience en [amélioration](desktop-to-uwp-enhance.md) votre application de bureau existante avec les API UWP. Si vous devez utiliser un composant UWP, pour obtenir de l’expérience, vous pouvez ajouter un projet UWP à votre solution et utiliser des services d’application pour la communication entre votre application de bureau et le composant UWP.

Pour plus d’informations, consultez [étendre votre application de bureau avec des composants UWP](desktop-to-uwp-extend.md).

## <a name="distribute"></a>Distribuer

Vous pouvez distribuer votre application en la publiant le Microsoft Store ou par chargement de version test sur d’autres systèmes.

Consultez [distribuer votre application de bureau empaquetée](desktop-to-uwp-distribute.md).
