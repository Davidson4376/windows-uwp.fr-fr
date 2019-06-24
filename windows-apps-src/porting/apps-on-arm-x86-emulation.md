---
title: Fonctionnement du système x86 et de l'émulation ARM32 sur ARM
description: Une présentation de l'émulation des applications x86 sur ARM.
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, toujours connecté, x86, émulation sur ARM
ms.localizationpriority: medium
ms.openlocfilehash: 8af6ba39468dd16a043040b797a03c862d6ce7db
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319657"
---
# <a name="how-x86-emulation-works-on-arm"></a>Fonctionnement de l'émulation x86 sur ARM
Pour les application x86, l'émulation enrichit l'écosystème des applications Win32 sur ARM. Ainsi, l'utilisateur profite d'une expérience sans pareil dans l'exécution d'une application Win32 x86 existante, sans que l'application ne souffre de modification. L'application n'a pas conscience d'être exécutée sur un PC ARM avec Windows, à moins qu'elle n'appelle les API spécifiques ([IsWoW64Process2](https://docs.microsoft.com/windows/desktop/api/wow64apiset/nf-wow64apiset-iswow64process2)).

La couche [WOW64](https://docs.microsoft.com/windows/desktop/WinProg64/running-32-bit-applications) de Windows 10 permet au code x86 de s'exécuter sur une version ARM64 de Windows 10. L'émulation x86 fonctionne en compilant les blocs d'instructions x86 en instructions ARM64 avec des optimisations permettant d'améliorer la performance. Un service met en cache ces blocs de cde traduits afin de réduire le traitement de la traduction des instructions et de permette une optimisation lorsque le code s'exécute à nouveau. Les caches sont produits pour chaque module afin que d'autres applications soient en mesure de les utiliser lors de leur premier lancement. 

Pour plus d’informations sur ces technologies, visionnez la vidéo Channel9 [Windows 10 sur ARM](https://channel9.msdn.com/Events/Build/2017/P4171) 