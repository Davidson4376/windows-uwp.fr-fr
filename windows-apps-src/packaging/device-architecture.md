---
author: laurenhughes
title: Architectures de package d'application
description: En savoir plus sur les architectures de processeur que vous devez utiliser lorsque vous générez votre package d’application UWP.
ms.author: lahugh
ms.date: 7/13/2017
ms.topic: article
keywords: windows10, uwp, création de packages, architecture, configuration de package
ms.localizationpriority: medium
ms.openlocfilehash: 3e265df32a8c4168cddced905e7b0712e4601264
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5831958"
---
# <a name="app-package-architectures"></a>Architectures de package d'application

Les packages d’application sont configurés pour s’exécuter sur une architecture de processeur spécifique. En sélectionnant une architecture, vous spécifiez les appareils sur lesquels vous souhaitez que votre application s’exécute. Les applications plateforme Windows universelle (UWP) peuvent être configurées pour s’exécuter sur les architectures suivantes:
- x86
- x64
- ARM

Il est **vivement** recommandé de générer votre package d’application afin de cibler toutes les architectures. En désélectionnant une architecture d’appareil, vous limitez le nombre de périphériques sur lesquels votre application peut s’exécuter, ce qui limite également le nombre de personnes qui peuvent utiliser votre application!

## <a name="windows-10-devices-and-architectures"></a>Appareils et architectures Windows10

> [!div class="mx-tableFixed"]
| ArchitectureUWP | Bureau (x86)      | Bureau (x64)      | Bureau (ARM)      | Appareils mobiles             | HoloLens           | Xbox               | IoT standard (dépendant du périphérique) | Surface Hub        |
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|-----------------------------|--------------------|
| x86              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :x:                | :heavy_check_mark: | :x:                | :heavy_check_mark:          | :heavy_check_mark: |
| x64              | :x:                | :heavy_check_mark: | :x:                | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: |
| ARM              | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :x:                | :x:                | :heavy_check_mark:          | :x:                |
 

Parlons de ces architectures plus en détail. 

### <a name="x86"></a>x86
Le choix de l’option x86 est généralement la configuration la plus sûre pour un package d’application, car l'application s’exécutera sur presque tous les appareils. Un package d’application avec la configuration x86 ne s’exécutera pas sur certains appareils, par exemple la console Xbox ou certains appareils IoT standard. Toutefois, pour un PC, un package x86 représente le choix le plus sûr et qui offre la plus grande portée de déploiement sur les appareils. Un nombre important d'appareils Windows10 continuent à exécuter la version x86 de Windows. 

### <a name="x64"></a>x64
Cette configuration est utilisée moins fréquemment que la configuration x86. Il convient de noter que cette configuration est réservée aux postes de travail utilisant les versions 64bits de Windows10, [les applications UWP sur Xbox](https://docs.microsoft.com/windows/uwp/xbox-apps/system-resource-allocation) et Windows10 IoT Standard sur le Joule Intel.

### <a name="arm"></a>ARM
La configuration Windows10 sur ARM inclut certains PC de bureau, appareils mobiles et quelques appareils IoT Standard (Rasperry Pi 2, Raspberry Pi 3 et DragonBoard). Les PC de bureau Windows10 sur ARM sont une nouveauté de la famille Windows, donc si vous êtes développeur d’applications UWP, vous devriez soumettre des packages ARM au Windows Store pour une expérience optimale sur ces PC. 

Découvrez cette discussion //Build pour voir une démonstration de [Windows10 sur ARM](https://channel9.msdn.com/Events/Build/2017/P4171) et en savoir plus sur son fonctionnement. 

Pour plus d’informations sur des sujets spécifiques à IoT, voir [Déploiement d’une application avec Visual Studio](https://developer.microsoft.com/windows/iot/Docs/AppDeployment).
