---
title: Architectures de package d’application
description: En savoir plus sur les architectures de processeur que vous devez utiliser lorsque vous générez votre package d’application UWP.
ms.date: 07/13/2017
ms.topic: article
keywords: windows 10, uwp, création de packages, architecture, configuration de package
ms.localizationpriority: medium
ms.openlocfilehash: 338dac1d43e08257fa00b51c0c311a090f3d95c0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619934"
---
# <a name="app-package-architectures"></a>Architectures de package d’application

Les packages d’application sont configurés pour s’exécuter sur une architecture de processeur spécifique. En sélectionnant une architecture, vous spécifiez les appareils sur lesquels vous souhaitez que votre application s’exécute. Les applications plateforme Windows universelle (UWP) peuvent être configurées pour s’exécuter sur les architectures suivantes :
- x86
- x64
- ARM
- ARM64

Il est **vivement** recommandé de générer votre package d’application afin de cibler toutes les architectures. En désélectionnant une architecture d’appareil, vous limitez le nombre de périphériques sur lesquels votre application peut s’exécuter, ce qui limite également le nombre de personnes qui peuvent utiliser votre application !

## <a name="windows-10-devices-and-architectures"></a>Appareils et architectures Windows 10

> [!div class="mx-tableFixed"]
| Architecture UWP | Bureau (x86)      | Bureau (x64)      | Bureau (ARM)      | Mobile             | HoloLens et Windows Mixed Reality           | Xbox               | IoT standard (dépendant du périphérique) | Surface Hub        |
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|-----------------------------|--------------------|
| x86              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :x:                | :heavy_check_mark: | :x:                | :heavy_check_mark:          | :heavy_check_mark: |
| x64              | :x:                | :heavy_check_mark: | :x:                | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: |
| ARM et ARM64              | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :x:                | :x:                | :heavy_check_mark:          | :x:                |


Parlons de ces architectures plus en détail.

### <a name="x86"></a>x86
Le choix de l’option x86 est généralement la configuration la plus sûre pour un package d’application, car l'application s’exécutera sur presque tous les appareils. Un package d’application avec la configuration x86 ne s’exécutera pas sur certains appareils, par exemple la console Xbox ou certains appareils IoT standard. Toutefois, pour un PC, un package x86 représente le choix le plus sûr et qui offre la plus grande portée de déploiement sur les appareils. Un nombre important d'appareils Windows 10 continuent à exécuter la version x86 de Windows.

### <a name="x64"></a>x64
Cette configuration est utilisée moins fréquemment que la configuration x86. Il convient de noter que cette configuration est réservée aux postes de travail utilisant les versions 64 bits de Windows 10, [les applications UWP sur Xbox](https://docs.microsoft.com/windows/uwp/xbox-apps/system-resource-allocation) et Windows 10 IoT Standard sur le Joule Intel.

### <a name="arm-and-arm64"></a>ARM et ARM64
La configuration Windows 10 sur ARM inclut certains PC de bureau, appareils mobiles et quelques appareils IoT Standard (Rasperry Pi 2, Raspberry Pi 3 et DragonBoard). Les PC de bureau Windows 10 sur ARM sont une nouveauté de la famille Windows, donc si vous êtes développeur d’applications UWP, vous devriez soumettre des packages ARM au Windows Store pour une expérience optimale sur ces PC.

>[!NOTE]
> Pour générer votre application UWP en mode natif de cibler la plateforme ARM64, vous devez disposer de Visual Studio 2017 version 15.9 ou ultérieure. Pour plus d’informations, consultez [ce billet de blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).

Pour plus d’informations, consultez [Windows 10 sur ARM](../porting/apps-on-arm.md). Découvrez cette discussion //Build pour voir une démonstration de [Windows 10 sur ARM](https://channel9.msdn.com/Events/Build/2017/P4171) et en savoir plus sur son fonctionnement.

Pour plus d’informations sur les rubriques spécifiques IoT, consultez [déploiement d’une application avec Visual Studio](https://developer.microsoft.com/windows/iot/Docs/AppDeployment).
