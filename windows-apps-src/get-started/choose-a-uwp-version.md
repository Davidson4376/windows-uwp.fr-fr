---
author: QuinnRadich
title: Choisir une version UWP
description: "Lors de l’écriture d’applications UWP dans Microsoft Visual Studio, vous pouvez choisir la version à cibler. Découvrez ce qui distingue les différentes versions d’UWP et apprenez à configurer vos choix dans les projets nouveaux et existants."
redirect_url: ../updates-and-versions/choose-a-uwp-version/
ms.openlocfilehash: d6d2be6c91ddf5fb85cdec759c753db1561f066f
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="choose-a-uwp-version"></a>Choisir une version d’UWP

**Cette page a été déplacée vers ../updates-and-versions/choose-a-uwp-version/**

Quand vous écrivez une application UWP dans Microsoft Visual Studio, vous pouvez choisir la version que vous ciblez. Découvrez ce qui distingue les différentes versions d’UWP et apprenez à configurer vos choix dans les projets nouveaux et existants.

## <a name="whats-different-in-each-uwp-version"></a>Qu’est-ce qui différencie chaque version d’UWP?

Les API nouvelles et modifiées pour UWP sont disponibles dans toutes les versions successives de Windows10. Pour savoir quelles fonctionnalités ont été ajoutées dans quelle version, consultez [Nouveautés de Windows](../whats-new/windows-10-version-1607.md).

Pour consulter des rubriques de référence qui énumèrent toutes les familles d’appareils et leurs versions, ainsi que les contrats d’API et leurs versions, voir [Familles d’appareils](https://msdn.microsoft.com/library/windows/apps/dn706137.aspx) et [Contrats d’API](https://msdn.microsoft.com/library/windows/apps/dn706135.aspx).

## <a name="choose-which-version-to-use-for-your-app"></a>Choisir la version à utiliser pour votre application

Dans la boîte de dialogue **Nouveau projet Windows universel** de Visual Studio, vous pouvez choisir une version pour **Version cible** et pour **Version minimale**.

* **Version cible**. Définit le paramètre *TargetPlatformVersion* de votre fichier projet. Détermine également la valeur de l’attribut *TargetDeviceFamily@MaxVersionTested* dans le manifeste de votre package d’application. La valeur que vous choisissez spécifie la version de la plateforme UWP ciblée par votre projet et par conséquent, l’ensemble d’API disponibles pour votre application. Nous vous recommandons donc de choisir la version la plus récente. Pour plus d’informations sur le manifeste de votre package d’application et pour obtenir des recommandations sur la configuration manuelle de TargetDeviceFamily, voir [TargetDeviceFamily](https://msdn.microsoft.com/library/windows/apps/dn986903).
* **Version minimale**. Définit le paramètre *TargetPlatformMinVersion* de votre fichier projet. Détermine également la valeur de l’attribut *TargetDeviceFamily@MinVersion* dans le manifeste de votre package d’application. La valeur que vous choisissez spécifie la version minimale de la plateforme UWP avec laquelle votre projet peut fonctionner.

Sachez que vous déclarez que votre application fonctionne sur n’importe quelle version de Windows entre la **Version minimale** et la **Version cible**. Si ces deux versions sont les mêmes, aucune action de votre part n’est nécessaire. Si elles sont différentes, voici quelques points à noter.

* Dans votre code, vous pouvez appeler librement (autrement dit, sans contrôles conditionnels) toute API qui existe dans la version spécifiée par **Version minimale**.
* La valeur de **Version cible** permet d’identifier toutes les références (contrat winmd) utilisées pour compiler votre projet. Mais ces références vous permettront de compiler votre code avec des appels à des API qui n’existeront pas nécessairement sur les appareils que vous avez déclaré prendre en charge (via **Version minimale**). Par conséquent, toute API introduite après la **Version minimale** doit être appelée via un code adaptatif. Pour plus d’informations sur le code adaptatif, voir [Guide des applications de plateforme Windows universelle (UWP)](universal-application-platform-guide.md).