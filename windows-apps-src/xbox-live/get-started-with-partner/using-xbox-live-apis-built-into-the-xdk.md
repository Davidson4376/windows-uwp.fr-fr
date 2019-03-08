---
title: À l’aide des API de Xbox Live intégrées dans le XDK
description: Découvrez comment utiliser les API intégrées Xbox Live dans votre projet de Kit de développement Xbox (XDK).
ms.assetid: 539caca3-58bc-49d9-8432-ca8e57755be2
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c762dd8abbbc80948d232610e4123b6e4893936d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598404"
---
# <a name="using-xbox-live-apis-built-into-the-xdk"></a>À l’aide des API de Xbox Live intégrées dans le XDK

1. Cliquez avec le bouton droit sur votre projet dans Visual Studio et choisissez « Références ».
1. Choisissez « Ajouter une nouvelle référence »
1. Cliquez sur « Durango. <build number>" et « Extensions » dans le volet gauche
1. Au milieu, choisissez :
- Si vous souhaitez utiliser l’API de XSAPI WinRT, choisissez « API de Services Xbox »
- Si vous souhaitez utiliser l’API de XSAPI C++, choisissez « Xbox Services API Cpp »
1. Cliquez sur OK

Remarque: Si votre système de génération ne prend pas en charge les fichiers de propriétés, vous devez ajouter manuellement les définitions de préprocesseur et les bibliothèques comme indiqué dans `%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox.Services.API.Cpp\8.0\DesignTime\CommonConfiguration\Neutral\Xbox.Services.API.Cpp.props`
