---
title: Gestion des erreurs
description: Découvrez comment gérer les erreurs lors d’un service Xbox Live à appeler.
ms.assetid: e433dfbd-488b-44ff-8333-1dcf0329cd60
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, erreur, l’appel de service
ms.localizationpriority: medium
ms.openlocfilehash: 637b9da84ef3ae50ea6235ca86e6318ae62acd11
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637004"
---
# <a name="error-handling"></a>Gestion des erreurs

Les développeurs devraient veiller lors d’un appel de service. Le titre doit toujours gérer les erreurs lors des appels de réseau. Même pour les appels de service qui ont travaillé régulièrement pendant le développement, les appels de service peuvent échouer dans des environnements réels pour un certain nombre de raisons. Par exemple, votre appel peut échouer en raison :

* Le réseau n’est pas disponible.
* Le service est trop occupé, retournant un code d’état HTTP 503.
* La demande n’est pas valide, en retournant un code d’état HTTP 400.
* L’utilisateur n’a pas les autorisations appropriées, retournant un code d’état HTTP 403.
* L’utilisateur a été interdit, retournant un code d’état HTTP 401.
IXMLHTTPRequest2, qui est appelée par l’API de service WinRT, est impossible d’envoyer la demande (ERROR_TIMEOUT, RPC_S_CALL_FAILED_DNE, etc.).
* La liste ci-dessus n’est pas exhaustive. La majorité de ces problèmes entraîne une exception levée à partir de la couche API de Services. Le titre doit capturer ces exceptions et traiter de manière appropriée.

Il un deux styles différents de dans l’API Services Xbox (XSAPI) en fonction de l’API de gestion des erreurs que vous utilisez :

Consultez [gestion des erreurs de l’API C++](error-handling-cpp.md).

Consultez [gestion d’erreur de l’API WinRT](error-handling-winrt.md).
