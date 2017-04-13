---
author: mcleblanc
description: "Utilisez votre ordinateur Mac actuel pour développer des applications pour Windows."
title: Configuration de votre Mac avec Windows 10
ms.assetid: 6D520610-5DE0-476E-A792-AA57E002D309
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: c73a1d1695e3b8a2eee8f073a5d25586a24a9d24
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="setting-up-your-mac-with-windows-10"></a>Configuration de votre Mac avec Windows 10

\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Utilisez votre ordinateur Mac actuel pour développer des applications pour Windows.

## <a name="run-windows-on-your-mac-and-use-visual-studio"></a>Exécuter Windows sur votre Mac et utiliser Visual Studio

Vous êtes prêt à développer des applications Windows universelles, mais vous n'avez pas de PC à portée de main ? Ce n’est pas un problème, vous pouvez utiliser votre Mac ! Avec des solutions tierces populaires telles que Boot Camp d’Apple, VirtualBox d’Oracle, VMware Fusion et Parallels Desktop, vous pouvez installer Windows10 et Microsoft Visual Studio sur votre Mac.

**Remarque** Vous aurez besoin d’une image de démarrage Windows 10 sur disque ou sur un disque mémoire USB. Si vous êtes un abonné MSDN, vous pouvez télécharger l’image d’installation à partir du centre Téléchargements des abonnés MSDN. Si vous n’êtes pas un abonné, le programme d’installation peut être acheté dans le [Windows Store](http://apps.microsoft.com/windows/app). Vous pouvez également le télécharger à partir de [cet emplacement](http://go.microsoft.com/fwlink/?LinkId=623906) ; très utile si vous exécutez déjà Windows et que vous souhaitez effectuer une mise à jour.

Une fois que Windows est en cours d’exécution, vous pouvez ensuite installer Visual Studio 2015 à partir de [Téléchargements destinés aux développeurs Windows 10](https://developer.microsoft.com/en-us/windows/downloads) et commencer à écrire des applications !

**Remarque** Si vous envisagez d’utiliser les émulateurs Visual Studio, vous **devez** installer une version 64 bits (x64) de Windows 10 Professionnel ou une version supérieure. Malheureusement, certains anciens modèles de Mac ne peuvent pas exécuter Windows en version 64bits. Vérifiez auprès d’Apple si votre matériel est compatible sur cette[page de support Apple](http://go.microsoft.com/fwlink/p/?LinkID=397959).

## <a name="apple-boot-camp"></a>Apple Boot Camp

L’application Boot Camp Assistant est préinstallée sur tous les Mac récents. Lorsque vous la lancez, le processus d’installation de Windows 10 démarre. Tout ce dont vous avez besoin est une copie de Windows (obtenue à partir des sources énumérées ci-dessus) et au moins 30 Go d’espace disque libre. Une fois l’installation effectuée, vous pouvez choisir de démarrer dans Mac OSX ou dans Windows 10. Pour plus d'informations, voir la [page d’instructions Boot Camp](http://go.microsoft.com/fwlink/?LinkId=623912) d'Apple.

## <a name="parallels-desktop"></a>Parallels Desktop

Avec Parallels Desktop 11, vous pouvez exécuter des applications Windows côte à côte avec les applications Mac existantes, y compris Visual Studio et Cortana. Une version pro est disponible et inclut des fonctionnalités supplémentaires pour les développeurs, notamment le débogage amélioré et la prise en charge de Docker et Jenkins. Pour plus d'informations et une version d'évaluation gratuite, voir [Parallels Desktop](http://go.microsoft.com/fwlink/p/?LinkId=281827).

## <a name="vmware-fusion"></a>VMWare Fusion

Fusion 8 de VMware vous permet d'exécuter Visual Studio directement sur votre bureau Mac. Une version pro est disponible pour offrir aux développeurs certaines fonctionnalités plus avancées, telles que la prise en charge de vSphere. Pour plus d'informations et une version d'évaluation gratuite, voir [VMware Fusion](http://go.microsoft.com/fwlink/p/?LinkId=281826).

## <a name="oracle-virtualbox"></a>Oracle VirtualBox

VirtualBox est une application gratuite qui permet d’exécuter des ordinateurs virtuels sur votre ordinateur. Elle prend en charge l’exécution de Windows sur Mac. C'est une option sans superflu, mais le prix est attrayant. Pour plus d’informations, voir [VirtualBox](http://go.microsoft.com/fwlink/p/?LinkId=280599).

