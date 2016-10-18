---
author: QuinnRadich
title: "Choisir une version d’UWP"
description: "Quand vous écrivez une application UWP dans Microsoft Visual Studio, vous pouvez choisir la version que vous ciblez. Découvrez ce qui distingue les différentes versions d’UWP et apprenez à configurer vos choix dans les projets nouveaux et existants."
redirect_url: ../updates-and-versions/choose-a-uwp-version/
translationtype: Human Translation
ms.sourcegitcommit: a86002c944841536d37735bb8c4b657905582144
ms.openlocfilehash: d6d2be6c91ddf5fb85cdec759c753db1561f066f

---

# Choisir une version d’UWP

**Cette page a été déplacée vers ../updates-and-versions/choose-a-uwp-version/**

Quand vous écrivez une application UWP dans Microsoft Visual Studio, vous pouvez choisir la version que vous ciblez. Découvrez ce qui distingue les différentes versions d’UWP et apprenez à configurer vos choix dans les projets nouveaux et existants.

## Qu’est-ce qui différencie chaque version d’UWP?

Les API nouvelles et modifiées pour UWP sont disponibles dans toutes les versions successives de Windows10. Pour connaître le détail des fonctionnalités qui ont été ajoutées dans chaque version, voir [Nouveautés pour les développeurs Windows10](../whats-new/windows-10-version-1607.md).

Pour consulter des rubriques de référence qui énumèrent toutes les familles d’appareils et leurs versions, ainsi que tous les contrats d’API et leurs versions, voir [Familles d’appareils](https://msdn.microsoft.com/library/windows/apps/dn706137.aspx) et [Contrats d’API](https://msdn.microsoft.com/library/windows/apps/dn706135.aspx).

## Choisir la version à utiliser pour votre application

Dans la boîte de dialogue **Nouveau projet Windows universel** de Visual Studio, vous pouvez choisir une version pour **Version cible** et **Version minimale**.

* **Version cible**: cette valeur définit le paramètre *TargetPlatformVersion* dans votre fichier projet. Elle détermine aussi la valeur de l’attribut *TargetDeviceFamily@MaxVersionTested* dans le manifeste de votre package d’application. La valeur que vous choisissez détermine la version de la plateforme UWP ciblée par votre projet et par conséquent, l’ensemble d’API auxquelles votre application a accès. Nous vous recommandons donc de choisir la version la plus récente. Pour plus d’informations sur le manifeste de votre package d’application et pour obtenir des recommandations sur la configuration manuelle de TargetDeviceFamily, voir [TargetDeviceFamily](https://msdn.microsoft.com/library/windows/apps/dn986903).
* **Version minimale**: cette valeur définit le paramètre *TargetPlatformMinVersion* dans votre fichier projet. Elle détermine aussi la valeur de l’attribut *TargetDeviceFamily@MinVersion* dans le manifeste de votre package d’application. La valeur que vous choisissez détermine la version minimale de la plateforme UWP que votre projet peut utiliser.

Sachez que vous déclarez que votre application fonctionne sur n’importe quelle version de Windows entre la **Version minimale** et la **Version cible**. Si ces deux versions sont identiques, aucune action de votre part n’est nécessaire. Si elles sont différentes, voici ce que vous devez savoir.

* Dans votre code, vous pouvez appeler librement (autrement dit, sans contrôles conditionnels) toute API qui existe dans la version spécifiée par **Version minimale**.
* La valeur de **Version cible** permet d’identifier toutes les références (contrat winmd) utilisées pour compiler votre projet. Mais ces références vous permettent de compiler votre code avec des appels à des API qui n’existent pas nécessairement sur les appareils que vous avez déclaré prendre en charge (via **Version minimale**). Par conséquent, toute API introduite après la **Version minimale** doit être appelée via un code adaptatif. Pour plus d’informations sur le code adaptatif, voir [Guide des applications de plateforme Windows universelle (UWP)](universal-application-platform-guide.md).


<!--HONumber=Aug16_HO5-->


