---
title: Windows 10 sur ARM
description: Cet article présente comment les expériences et applications sont exécutées sur ARM ainsi que leurs limites concernées. Vous découvrirez également les références présentant de plus amples informations.
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, toujours connecté, ARM, ARM64, émulation x86
ms.localizationpriority: medium
ms.openlocfilehash: 740956480323d7c201e81071a444026b8d155462
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682731"
---
# <a name="windows-10-on-arm"></a>Windows 10 sur ARM
À l’origine, Windows 10 (par comparaison avec Windows 10 Mobile) peut s’exécuter uniquement sur les ordinateurs munis de processeurs x86 et x64. À présent, le bureau Windows 10 (éditions Pro et S) peut s'exécuter sur des postes munis de processeurs ARM64 avec la mise à jour Fall Creators Update. La nature axée sur l'économie d'énergie de l'architecture CPU ARM permet à tous les PC de profiter d'une autonomie de batterie durant toute la journée et de la prise en charge de réseaux de données mobiles. Ces PC fournissent une excellente compatibilité d'application et vous permette d'exécuter les applications win32 x86 héritées et non modifiées. Par exemple, Le lecteur Adobe. Pour plus d'informations ou pour obtenir une version de démonstration, regardez la [Vidéo de Channel 9 pour les PC toujours connectés](https://channel9.msdn.com/Events/Build/2017/P4171).

Nous utilisons le terme *ARM* ici comme raccourci pour les PC qui exécutent la version bureau de Windows 10 sur les processeurs ARM64 (également communément appelés *AArch64*) processeurs.  Nous utilisons le terme *ARM32* ici comme raccourci pour l’architecture ARM 32 bits (communément appelé *ARM* dans d'autres documentations).

## <a name="apps-and-experiences-on-arm"></a>Applications et expériences sur ARM

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>Expériences, applications et pilotes intégrés à Windows 10
Les expériences Windows 10 intégrées, notamment Edge, Cortana, le menu Démarrer et Explorer, sont toutes natives et exécutés en tant qu'ARM64 (ou ARM32). Ceci inclut également tous les pilotes de périphériques tels que les pilotes graphiques, de mise en réseau et de disque dur. De cette manière, vous obtenez la meilleur expérience utilisateur et la meilleure autonomie de batterie de vos appareils exécutés à la vitesse native complète du processeur Qualcomm Snapdragon.

### <a name="universal-windows-platform-uwp-apps"></a>Applications de plateforme Windows universelle (UWP)
Windows 10 sur ARM exécute toutes les [applications UWP](../get-started/universal-application-platform-guide.md) x86, ARM32 et ARM64 à partir du Microsoft Store. Les applications ARM32 et ARM64 s’exécutent en mode natif sans émulation, tandis que les applications x86 s’exécutent sous une émulation. Si vous êtes un développeur UWP, assurez-vous de soumettre un package ARM pour vos applications dans la mesure où cela permettra d'offrir la meilleure expérience utilisateur pour l'appareil. Pour plus d’informations, consultez [Architecture de package de l'application](/windows/msix/package/device-architecture).

>[!NOTE]
> Pour créer votre application UWP en vue de cibler la plateforme ARM64 en mode natif, vous devez disposer de Visual Studio 2017 version 15,9 ou ultérieure, ou de Visual Studio 2019. Pour plus d’informations, consultez ce billet de [blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).


>[!IMPORTANT]
> Lorsqu'un utilisateur télécharge une application UWP depuis le Microsoft Store, la version ARM32 est installée sur un appareil ARM64, à moins que la version x86 ne soit disponible. Pour plus d’informations sur les architectures, consultez [Architecture de package de l'application](/windows/msix/package/device-architecture).

### <a name="win32-apps"></a>Applications Win32
En plus des applications UWP, Windows 10 sur ARM peut également exécuter vos applications Win32 Win32 (comme Adobe Reader) sans modification, avec des performances optimales et une expérience utilisateur transparente, comme n’importe quel PC. Ces applications Win32 x86 n’ont pas besoin d’être recompilées pour ARM et ne se rendent même pas compte qu’elles s’exécutent sur un processeur ARM. Notez bien que les applications 64 bits Wind32 x64 ne sont pas prises en charge, mais la vaste majorité des applications comportent les versions x86 de leurs applications. De ce fait, du point de vue de l'utilisateur, il suffit de choisir le programme d'installation x86 32 bits pour une exécution sur Windows sur un PC ARM.

## <a name="in-this-section"></a>Dans cette section
|Rubrique | Description |
|-----|-----|
|[Fonctionnement de l’émulation x86 sur ARM](apps-on-arm-x86-emulation.md)|Une présentation détaillée du mode d'émulation es applications x86 sur ARM.|
|[Résolution des problèmes relatifs aux applications x86 sur ARM](apps-on-arm-troubleshooting-x86.md)|Problèmes courants avec les applications x86 lors de l'exécution sur ARM, et comment les résoudre. |
|[Dépannage des applications ARM sur ARM](apps-on-arm-troubleshooting-arm32.md)|Problèmes courants avec les applications ARM32 et ARM64 lors de l’exécution sur ARM, et comment les corriger. |
|[Utilitaire de résolution de problèmes de compatibilité des programmes sur ARM](apps-on-arm-program-compat-troubleshooter.md)|Conseils de réglage des paramètres de compatibilité si votre application ne fonctionne pas correctement sur ARM. |

## <a name="related-topics"></a>Rubriques connexes
|Rubrique | Description |
|-----|-----|
|[Génération de pilotes ARM64 avec le kit WDK](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers)|Instructions pour la création d’un pilote ARM64. |
| [Débogage des applications x86 sur ARM](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) | Conseils pour le débogage des applications x86 sur ARM. |
