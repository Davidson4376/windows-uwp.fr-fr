---
title: Single Point of Presence (SPOP)
description: Découvrez comment utiliser Xbox Live unique Point de présence (SPOP) pour vous assurer qu’un titre est lu sur un seul appareil à la fois.
ms.assetid: 40f19319-9ccc-4d35-85eb-4749c2cf5ef8
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, spop, unique point de présence
ms.localizationpriority: medium
ms.openlocfilehash: f1503a6168a50175e861e17027e8b642d1b7834d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595004"
---
# <a name="single-point-of-presence-spop"></a>Single Point of Presence (SPOP)

## <a name="overview"></a>Vue d’ensemble
Unique Point de présence (SPOP) est une condition de Xbox Live appliquée où un titre ne peut être lu sur un périphérique à la fois. Cela est appliquée pour XDK une Xbox et titres UWP sur n’importe quel appareil.
Un titre de Xbox Live, quel que soit l’appareil, qu'il est activé, peut initialiser un utilisateur qui est connecté à un titre sur un autre appareil Xbox One ou Windows 10.

## <a name="how-to-handle-spop"></a>Comment gérer SPOP
SPOP doit être géré par le titre de la même façon que n’importe quel autre type d’événement de déconnexion. L’utilisateur sera toujours être averti par le biais de l’interface utilisateur quand ils en ont une action qui initiait un SPOP pour vérifier qu’ils souhaiteraient provoquer le titre d’être déconnectés sur l’autre appareil.

* Pour les titres XDK le `User::SignOutCompleted` événement se déclenche lorsque cela se produit.
* Pour les titres UWP, ils seront informés de la déconnexion via le `sign_out_complete` gestionnaire à partir de la `xbox_live_user` classe. Consultez [l’authentification pour les projets UWP](authentication-for-UWP-projects.md) pour plus de détails
