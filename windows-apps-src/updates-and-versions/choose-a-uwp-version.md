---
author: QuinnRadich
title: Choisir une version UWP
description: "Lors de l’écriture d’applications UWP dans Microsoft Visual Studio, vous pouvez choisir la version à cibler. Découvrez ce qui distingue les différentes versions d’UWP et apprenez à configurer vos choix dans les projets nouveaux et existants."
ms.author: quradic
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: a8b7830f-4929-44c6-90be-91f38be5f364
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: e2bc7b4a12aed40093985000486b9f3f78967524
ms.lasthandoff: 02/08/2017

---

# <a name="choose-a-uwp-version"></a>Choisir une version d’UWP

Quand vous écrivez une application UWP dans Microsoft Visual Studio, vous pouvez choisir la version que vous ciblez. Actuellement, trois versions seulement sont possibles.

| Version | Description |
| --- | --- |
| Build 14393 (Édition anniversaire) | Il s’agit de la version la plus récente de Windows 10, publiée en juillet 2016. Voici quelques fonctionnalités phares de cette version : </br> \* **Windows Ink** : nouvelles commandes InkCanvas et InkToolbar. </br> \* **API Cortana** : les nouvelles actions de Cortana permettent d’intégrer la prise en charge de Cortana à des fonctions spécifiques de votre application. </br> \* **Windows Hello** : Microsoft Edge prend désormais en charge Windows Hello, ce qui donne aux développeurs web l’accès à l’authentification biométrique. </br> Pour plus d’informations sur ces fonctionnalités et de nombreuses autres ajoutées dans cette version de Windows, visitez le [Centre de développement](https://developer.microsoft.com/windows/windows-10-for-developers) ou consultez la page [Nouveautés de Windows 10 pour les développeurs](../whats-new/windows-10-version-1607.md).  |
| Build 10586 | Cette version de Windows 10 a été publiée en novembre 2015. Les fonctionnalités phares comprennent l’introduction des API ORTC (Object Real-Time Communications) pour la communication vidéo dans Microsoft Edge et des API fournisseurs permettant aux applications d’utiliser l’authentification faciale Windows Hello. [Plus d’informations sur les fonctionnalités introduites dans cette version.](../whats-new/windows-10-version-1511.md) |
| Build 10240 | Il s’agit de la version initiale de Windows 10 publiée en juillet 2015. [Plus d’informations sur les fonctionnalités introduites dans cette version.](../whats-new/windows-10-version-1507.md) |

Nous recommandons vivement aux nouveaux développeurs et aux développeurs qui écrivent du code pour le grand public de toujours utiliser la dernière version de Windows (build 14393). Les développeurs écrivant des applications d’entreprise doivent sérieusement envisager de prendre en charge une **version minimale** antérieure.

## <a name="whats-different-in-each-uwp-version"></a>Qu’est-ce qui différencie chaque version UWP ?

Les API nouvelles et modifiées pour UWP sont disponibles dans toutes les versions successives de Windows 10. Pour savoir quelles fonctionnalités ont été ajoutées dans quelle version, consultez [Nouveautés de Windows](../whats-new/windows-10-version-1607.md).

Pour consulter des rubriques de référence qui énumèrent toutes les familles d’appareils et leurs versions, ainsi que les contrats d’API et leurs versions, consultez [Familles d’appareils](https://msdn.microsoft.com/library/windows/apps/dn706137.aspx) et [Contrats d’API](https://msdn.microsoft.com/library/windows/apps/dn706135.aspx).

## <a name="choose-which-version-to-use-for-your-app"></a>Choisir la version à utiliser pour votre application

Dans la boîte de dialogue **Nouveau projet Windows universel** de Visual Studio, vous pouvez choisir une version pour **Version cible** et pour **Version minimale**.

* **Version cible**. Définit le paramètre *TargetPlatformVersion* de votre fichier projet. Détermine également la valeur de l’attribut *TargetDeviceFamily@MaxVersionTested* dans le manifeste de votre package d’application. La valeur que vous choisissez spécifie la version de la plateforme UWP ciblée par votre projet et par conséquent, l’ensemble d’API disponibles pour votre application. Nous vous recommandons donc de choisir la version la plus récente. Pour plus d’informations sur le manifeste de votre package d’application et pour obtenir des recommandations sur la configuration manuelle de TargetDeviceFamily, voir [TargetDeviceFamily](https://msdn.microsoft.com/library/windows/apps/dn986903).
* **Version minimale**. Définit le paramètre *TargetPlatformMinVersion* de votre fichier projet. Détermine également la valeur de l’attribut *TargetDeviceFamily@MinVersion* dans le manifeste de votre package d’application. La valeur que vous choisissez spécifie la version minimale de la plateforme UWP avec laquelle votre projet peut fonctionner.

Sachez que vous déclarez que votre application fonctionne sur n’importe quelle version de Windows entre la **Version minimale** et la **Version cible**. Si ces deux versions sont les mêmes, aucune action de votre part n’est nécessaire. Si elles sont différentes, voici quelques points à noter.

* Dans votre code, vous pouvez appeler librement (autrement dit, sans contrôles conditionnels) toute API existant dans la version spécifiée par **Version minimale**.
* Veillez à tester votre code sur un appareil exécutant la **Version minimale** pour vous assurer qu’il fonctionne sans avoir besoin d’API présentes uniquement dans la **Version cible**.
* La valeur de **Version cible** permet d’identifier toutes les références (contrat winmds) utilisées pour compiler votre projet. Mais ces références vous permettront de compiler votre code avec des appels à des API qui n’existeront pas nécessairement sur les appareils que vous avez déclaré prendre en charge (via **Version minimale**). Par conséquent, toute API introduite après la **Version minimale** doit être appelée via un code adaptatif. Pour plus d’informations sur le code adaptatif, voir [Guide des applications de plateforme Windows universelle (UWP)](../get-started/universal-application-platform-guide.md).

