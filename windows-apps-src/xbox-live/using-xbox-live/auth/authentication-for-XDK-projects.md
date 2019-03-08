---
title: Authentification pour les projets XDK
description: Découvrez comment connecter des utilisateurs de Xbox Live dans le titre d’un Kit de développement Xbox (XDK).
ms.assetid: 713bb2e3-80c5-4ac9-8697-257525f243d3
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 597b3becfa2083955d8bd4e0adc91e4ae9b827a1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613494"
---
# <a name="authentication-for-xdk-projects"></a>Authentification pour les projets XDK

Pour tirer parti des fonctionnalités Xbox Live dans les jeux, un utilisateur doit créer un profil Xbox Live pour s’identifier auprès de la Communauté Xbox Live.  Les services Xbox Live suivi de jeu lié activités à l’aide de ce profil Xbox Live, telles que l’utilisateur gamertag et gamer image, qui sont des amis de jeu de l’utilisateur, ce que l’utilisateur des jeux a joué, les primes que l’utilisateur qui a déverrouillé, où l’utilisateur est le classement pour un, etc. de jeu, particulier.

À un niveau élevé, vous utilisez l’API Live Xbox en suivant ces étapes :
1. Identifier l’interaction utilisateur
2. Création d’un contexte de Xbox Live en fonction de l’utilisateur
3. Utiliser le contexte de Xbox Live pour accéder aux services Xbox Live
4. Lorsque les issues de jeux ou de l’utilisateur se connecte à la sortie, libérer l’objet de XboxLiveContext en lui affectant la valeur null

### <a name="creating-an-xboxliveuser-object"></a>Création d’un objet XboxLiveUser
La plupart des activités de Xbox Live sont liées aux utilisateurs Xbox Live.  En tant que développeur de jeux, vous devez d’abord créer un objet XboxLiveUser pour représenter l’utilisateur local.

C++ :
```
Windows::Xbox::System::User^ user; // the interacting user.  From User::Users, etc
std::shared_ptr<xbox::services::xbox_live_context> xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>( user );
```

WinRT :
```
Windows::Xbox::System::User^ user; // the interacting user.  From User::Users, etc
Microsoft::Xbox::Services::XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext( user );
```
