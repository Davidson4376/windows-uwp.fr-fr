---
author: TylerMSFT
title: API expérimentales
description: Comprendre les API expérimentales
ms.author: twhitney
ms.date: 11/13/2017
ms.topic: article
keywords: windows10, uwp, expérimentale, api
ms.localizationpriority: medium
ms.openlocfilehash: fe5fa437c5a1e564be07b7277de0f190d6eab862
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7570342"
---
# <a name="experimental-apis"></a>API expérimentales

Les API expérimentales en sont à un stade précoce de conception et sont susceptibles d'être modifiées, car les propriétaires intègrent des commentaires et ajoutent du support pour des scénarios supplémentaires.

Ces API sont évaluées en externe à l’aide de [SDK Windows Insider](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK) afin que les développeurs puissent les essayer et fournir des commentaires avant de les intégrer à la plateforme officielle. Lors de leur évaluation, il n’existe aucune promesse de stabilité ou d'engagement.

## <a name="consuming-experimental-apis"></a>Utilisation d'API expérimentales
IntelliSense vous indique si une API est expérimentale. Lorsque vous utilisez une API expérimentale, vous recevez également un avertissement du compilateur tel que «... sert uniquement à des fins d’évaluation et est susceptible d’être modifiée ou supprimée dans de futures mises à jour.»

Ces avertissements contribuent à vous protéger contre la création de dépendances sur des API expérimentales dans le cadre de projets de production. Pour les projets expérimentaux, vous pouvez ignorer ou désactiver ces avertissements.

Par défaut, ces API sont désactivées au moment de l’exécution et les appeler entraîne une exception d’exécution. Il s’agit d’une autre protection contribuant à empêcher les dépendances par inadvertance et la large distribution d'applications qui utilisent des API expérimentales.

Afin d'activer ces API pour expérimentation, utilisez le module de fonctionnalités du [Portail d'appareil Windows (WDP)](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal) sur l’appareil cible pour activer la fonctionnalité correspondant à l’API que vous souhaitez appeler.

La documentation dédiée à une API expérimentale spécifique est à la discrétion de l’équipe qui en est propriétaire.

## <a name="providing-feedback"></a>Formulation de commentaires

Si vous avez essayé une API expérimentale et que vous souhaitez formuler des commentaires, utilisez la catégorie **Plateforme de développement** dans le [Hub de commentaires Windows](https://support.microsoft.com/en-us/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).