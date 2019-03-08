---
description: Utilisez votre ordinateur Mac actuel pour développer des applications pour Windows.
title: Configuration de votre Mac avec Windows 10
ms.assetid: 6D520610-5DE0-476E-A792-AA57E002D309
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e4f7b0469ae8c50e6da40fb248678a69a2827931
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663824"
---
# <a name="setting-up-your-mac-with-windows-10"></a>Configuration de votre Mac avec Windows 10


Utilisez votre ordinateur Mac actuel pour développer des applications pour Windows.

## <a name="run-windows-on-your-mac-and-use-visual-studio"></a>Exécuter Windows sur votre Mac et utiliser Visual Studio

Vous êtes prêt à développer des applications Windows universelles, mais vous n'avez pas de PC à portée de main ? Ce n’est pas un problème, vous pouvez utiliser votre Mac ! Avec les solutions tierces populaires tels que Apple Boot Camp, Oracle VirtualBox, VMware Fusion et Parallels Desktop, vous pouvez installer Windows 10 et Microsoft Visual Studio sur votre ordinateur Apple.

**Remarque**  vous devez une image de démarrage Windows 10 sur le disque ou lecteur flash USB. Si vous êtes un abonné MSDN, vous pouvez télécharger l’image d’installation à partir du centre Téléchargements des abonnés MSDN. Si vous n’êtes pas abonné, le programme d’installation peut être acheté à partir de la [Microsoft Store](https://apps.microsoft.com/windows/app). Vous pouvez également le télécharger à partir de [cet emplacement](https://go.microsoft.com/fwlink/?LinkId=623906) ; très utile si vous exécutez déjà Windows et que vous souhaitez effectuer une mise à jour.

Une fois que vous avez en cours d’exécution Windows, vous pouvez ensuite installer la dernière version de Visual Studio à partir de [téléchargements pour les développeurs pour Windows 10](https://developer.microsoft.com/en-us/windows/downloads) et commencer à écrire des applications !

**Remarque**  si vous envisagez d’utiliser les émulateurs Visual Studio, vous **doit** installer une version 64 bits (x 64) de Windows 10 Professionnel ou mieux. Malheureusement, certains anciens modèles de Mac ne peuvent pas exécuter Windows en version 64 bits. Vérifiez auprès d’Apple si votre matériel est compatible sur cette[page de support Apple](https://go.microsoft.com/fwlink/p/?LinkID=397959).

## <a name="apple-boot-camp"></a>Apple Boot Camp

L’application de l’Assistant de Boot Camp est préinstallée sur chaque Mac récents et lancer vous guidera dans le processus d’installation de Windows 10. Tout ce dont vous avez besoin est une copie de Windows (obtenue à partir des sources énumérées ci-dessus) et au moins 30 Go d’espace disque libre. Une fois l’installation effectuée, vous pouvez choisir de démarrer dans Mac OSX ou dans Windows 10. Pour plus d'informations, voir la [page d’instructions Boot Camp](https://go.microsoft.com/fwlink/?LinkId=623912) d'Apple.

## <a name="parallels-desktop"></a>Parallels Desktop

Avec Parallels Desktop 11, vous pouvez exécuter des applications Windows côte à côte avec les applications Mac existantes, y compris Visual Studio et Cortana. Une version pro est disponible et inclut des fonctionnalités supplémentaires pour les développeurs, notamment le débogage amélioré et la prise en charge de Docker et Jenkins. Pour plus d'informations et une version d'évaluation gratuite, voir [Parallels Desktop](https://go.microsoft.com/fwlink/p/?LinkId=281827).

## <a name="vmware-fusion"></a>VMWare Fusion

Fusion 8 de VMware vous permet d'exécuter Visual Studio directement sur votre bureau Mac. Une version pro est disponible pour offrir aux développeurs certaines fonctionnalités plus avancées, telles que la prise en charge de vSphere. Pour plus d'informations et une version d'évaluation gratuite, voir [VMware Fusion](https://go.microsoft.com/fwlink/p/?LinkId=281826).

## <a name="oracle-virtualbox"></a>Oracle VirtualBox

VirtualBox est une application gratuite qui permet d’exécuter des ordinateurs virtuels sur votre ordinateur. Elle prend en charge l’exécution de Windows sur Mac. C'est une option sans superflu, mais le prix est attrayant. Pour plus d’informations, voir [VirtualBox](https://go.microsoft.com/fwlink/p/?LinkId=280599).

