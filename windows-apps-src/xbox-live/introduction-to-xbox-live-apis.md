---
title: Présentation des API Xbox Live
description: En savoir plus sur les différents modèles d’API que vous pouvez utiliser pour interagir avec le service Xbox Live.
ms.assetid: 5918c3a2-6529-4f07-b44d-51f9861f91ec
ms.date: 06/05/2018
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1f52e379b524952c3361b432a577a7137b02155b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657694"
---
# <a name="introduction-to-xbox-live-apis"></a>Présentation des API Xbox Live

## <a name="use-xbox-live-services"></a>Utiliser les services Xbox Live

Il existe deux façons d’obtenir des informations à partir des services Xbox Live :

- Utiliser une API de côté client appelée Xbox Live Services API (**XSAPI**)
- Appelez le **points de terminaison Xbox Live REST** directement

Les avantages de l’utilisation de l’API de Services Xbox Live (**XSAPI**) incluent :

- Détails de l’authentification, de codage et HTTP d’envoi et de réception sont pris en charge pour vous.
- Arguments et les données provenant de, que l’API de wrapper est gérée dans des données natives types ; Par conséquent, vous n’avez pas besoin effectuer de codage / décodage JSON.
- Appeler directement les services web implique plusieurs étapes asynchrones, qui encapsule les API wrapper ; Cela rend le code de titre plus facile à lire et écrire.
- Certaines fonctionnalités, telles que l’écriture d’événements de jeu, sont uniquement disponibles dans XSAPI.

Les avantages de l’utilisation de la **points de terminaison Xbox Live REST** directement incluent :

- La possibilité d’appeler des points de terminaison Xbox Live à partir d’un service web
- La possibilité d’appeler des points de terminaison qui ne sont pas inclus dans XSAPI.  XSAPI inclut uniquement les API que nous pensons jeux utilisera, donc si vous permettent de manque quelque chose nous savoir via les forums.
- Certaines fonctionnalités disponibles via les points de terminaison REST ne peuvent pas avoir un wrapper XSAPI correspondant.

Vos jeux et les applications ne sont pas limitées à en utilisant un seul de ces méthodes. Vous pouvez utiliser le wrapper XSAPI et toujours appeler directement les points de terminaison REST si nécessaire.

## <a name="xbox-live-services-api-overview"></a>Vue d’ensemble des API de Services Live Xbox ##

L’API de Services Xbox Live (**XSAPI**) expose trois ensembles d’API côté client qui prend en charge un large éventail de scénarios de client :

- [XSAPI WinRT API](#xsapi-winrt-based-api)
- [API basée sur XSAPI C ++ 11](#xsapi-c11-based-api)
- [API basée sur XSAPI C](#xsapi-c-based-api) (**New à compter de juin 2018**)

Comparaison de l’API :

### <a name="xsapi-winrt-based-api"></a>API basée sur XSAPI WinRT

- Prend en charge les applications écrites avec C / c++ / CX, C#et JavaScript.
    - C++ / c++ / CX est une extension Microsoft C++ pour faciliter la programmation de WinRT par exemple à l’aide de ^ en tant que pointeurs de WinRT.
- Prend en charge les applications qui ciblent la plateforme du XDK une Xbox et Universal Windows Platform (UWP) x86, x64 et les architectures ARM.
- Les erreurs sont gérées via des exceptions dans toutes les langues, y compris C++ / c++ / CX.
- C++ / c++ / WinRT prend également en charge.  Plus d’informations sur C + c++ / WinRT, consultez [https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/](https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/)

Voici un exemple d’appel de l’API WinRT XSAPI à l’aide de C++ / c++ / WinRT :

```c++
winrt::Windows::Xbox::System::User cppWinrtUser = winrt::Windows::Xbox::System::User::Users().GetAt(0);
winrt::Microsoft::Xbox::Services::XboxLiveContext xblContext(cppWinrtUser);
```

Si vous souhaitez combiner C + c++ / CX et c++ / WinRT en tant que vous sont la migration du code vous pouvez effectuer cette opération trop mais êtes un peu plus complexe.  
Voici un exemple d’appel de l’API WinRT XSAPI à l’aide de C++ / c++ / WinRT donné C + c++ / CX utilisateur ^ objet.

```c++
::Windows::Xbox::System::User^ user1 = ::Windows::Xbox::System::User::Users->GetAt(0);
winrt::Windows::Xbox::System::User cppWinrtUser(nullptr);
winrt::copy_from(cppWinrtUser, reinterpret_cast<winrt::ABI::Windows::Xbox::System::IUser*>(user1));
winrt::Microsoft::Xbox::Services::XboxLiveContext xblContext(cppWinrtUser);
```


### <a name="xsapi-c11-based-api"></a>API basée sur XSAPI C ++ 11

- Utilise inter-plateforme norme ISO C ++ 11
- Applications prend en charge écrites avec C++
- Prend en charge les applications qui ciblent la plateforme du XDK une Xbox et Universal Windows Platform (UWP) x86, x64 et les architectures ARM.
- Erreurs sont gérées par le biais de std::error_code.
- L’API d’en 11 C ++ est l’API recommandée à utiliser pour les moteurs de jeux C++ pour meilleures performances et une meilleure du débogage.
- Si vous êtes dans le cadre du programme Xbox Live Creators, avant d’inclure l’en-tête XSAPI définir XBOX_LIVE_CREATORS_SDK. Cela limite la surface API à ceux qui sont utilisables par les développeurs dans le cadre du programme Xbox Live Creators et modifie la méthode d’authentification fonctionne pour les titres dans le programme Creators.  Exemple :

```c++
#define XBOX_LIVE_CREATORS_SDK
#include "xsapi\services.h"
```

- C++ / c++ / WinRT prend également en charge.  Plus d’informations sur C + c++ / WinRT, consultez [https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/](https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/)

Pour utiliser C++ / c++ / WinRT avec l’API C++ XSAPI, avant d’inclure l’en-tête XSAPI définir XSAPI_CPPWINRT.  Exemple :

```c++
#define XSAPI_CPPWINRT
#include "xsapi\services.h"
```

Voici un exemple d’appel de l’API C++ XSAPI à l’aide de C++ / c++ / WinRT :

```c++
winrt::Windows::Xbox::System::User cppWinrtUser = winrt::Windows::Xbox::System::User::Users().GetAt(0);
std::shared_ptr<xbox::services::xbox_live_context> xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>(cppWinrtUser);
```

### <a name="xsapi-c-based-api"></a>API basée sur XSAPI C

- Permet de titres contrôler les allocations de mémoire lors de l’appel XSAPI.
- Permet de titres d’assumer le contrôle complet de gestion lors de l’appel XSAPI de thread.
- Utilise une nouvelle bibliothèque HTTP, libHttpClient, conçue pour les développeurs de jeux.

Pour plus d’informations, consultez [Introduction à l’API C de Xbox Live](xsapi-flat-c.md).
