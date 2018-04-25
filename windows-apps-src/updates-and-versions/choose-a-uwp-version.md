---
author: QuinnRadich
title: Choisir une version UWP
description: Lors de l’écriture d’applications UWP dans Microsoft Visual Studio, vous pouvez choisir la version à cibler. Découvrez ce qui distingue les différentes versions d’UWP et apprenez à configurer vos choix dans les projets nouveaux et existants.
ms.author: quradic
ms.date: 11/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, version, build, versions, windows, choisir, mettre à jour, mises à jour
ms.assetid: a8b7830f-4929-44c6-90be-91f38be5f364
ms.localizationpriority: high
ms.openlocfilehash: 61ca2a307fb2e6c5e69ba1801748336269ae7bc2
ms.sourcegitcommit: 1eabcf511c7c7803a19eb31f600c6ac4a0067786
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="choose-a-uwp-version"></a>Choisir une version d’UWP

Chaque version de Windows10 fournit de nouvelles fonctionnalités améliorées pour la plateforme UWP. Lors de la création d’applications UWP dans Microsoft Visual Studio, vous pouvez choisir la version à cibler. Pour les projets utilisant [.NET Standard2.0](https://docs.microsoft.com/dotnet/standard/net-standard), la **Version minimale** doit être définie sur la build16299 ou version ultérieure.

Veuillez noter que tous les projetsUWP créés dans les versions courantes de Visual Studio2017 ne peuvent pas être ouverts dans Visual Studio2015.

Le tableau suivant décrit les versions de Windows10disponibles. Veuillez noter que ce tableau s’applique uniquement à la création d’applications UWP, qui sont uniquement prises en charge sur Windows10. Vous ne pouvez pas développer des applications UWP pour les versions antérieures de Windows, et vous devez [installer la version appropriée du Kit de développement (SDK)](http://go.microsoft.com/fwlink/?LinkId=821431) afin de cibler cette version. 

| Version | Description |
| --- | --- |
| Build16299 (Fall Creators Update, version1709) | Il s’agit de la dernière version de Windows10, publiée en octobre2017. **Veuillez noter que vous _devez_ utiliser Visual Studio2017 afin de cibler cette version de Windows**. Voici quelques fonctionnalités phares de cette version: </br> \* **.Net Standard2.0:** profitez d’un important accroissement du nombre d’API .NET et incorporez vos packages NuGet favoris et bibliothèques tierces dans .NET Standard. Obtenez plus d’informations et explorez la documentation [ici](https://docs.microsoft.com/dotnet/standard/net-standard). Veuillez noter que vous devez définir votre **version minimale** sur la build16299 pour accéder à ces nouvelles API. </br> \* **Fluent Design:** utilisez la lumière, la profondeur, la perspective et le mouvement pour améliorer votre application et aider les utilisateurs à se concentrer sur les éléments d’interface utilisateur importants. </br> \* **XAML conditionnel:** définissez facilement des propriétés et instanciez des objets basés sur la présence d’une API lors de l’exécution, afin de permettre à vos applications de s’exécuter en toute transparence sur plusieurs appareils et versions. </br> Pour plus d’informations sur ces fonctionnalités et de nombreuses autres ajoutées dans cette version de Windows, visitez le [Centre de développement](https://developer.microsoft.com/windows/windows-10-for-developers) ou consultez la page [Nouveautés de Windows10 pour les développeurs](../whats-new/windows-10-build-16299.md).
| Build15063 (Creators Update, version1703) | Cette version de Windows10 a été publiée en mars2017. **Veuillez noter que vous _devez_ utiliser Visual Studio2017 afin de cibler cette version de Windows**. Voici quelques fonctionnalités phares de cette version:  </br> \* **Analyse de l’entrée manuscrite:** Windows Ink est désormais en mesure de classer les différents traits d'encre, selon qu'il s'agit de traits d'écriture ou de dessin, ainsi que les textes, formes et structures de disposition basiques reconnues. </br> \* **API Windows.Ui.Composition:** combinez et appliquez aisément des animations dans votre application. </br> \* **Modification dynamique:** modifiez le code XAML pendant que votre application est en cours d’exécution et constatez les modifications en temps réel. </br> Pour plus d’informations sur ces fonctionnalités et de nombreuses autres ajoutées dans cette version de Windows, visitez le [Centre de développement](https://developer.microsoft.com/windows/windows-10-for-developers) ou consultez la page [Nouveautés de Windows10 pour les développeurs](../whats-new/windows-10-build-15063.md).  |
| Build14393 (mise à jour anniversaire, version1607) | Cette version de Windows10 a été publiée en juillet2016. Voici quelques fonctionnalités phares de cette version: </br> \* **Windows Ink**: nouvelles commandes InkCanvas et InkToolbar. </br> \* **API Cortana**: les nouvelles actions de Cortana permettent d’intégrer la prise en charge de Cortana à des fonctions spécifiques de votre application. </br> \* **Windows Hello**: Microsoft Edge prend désormais en charge Windows Hello, ce qui donne aux développeurs web l’accès à l’authentification biométrique. </br> Pour plus d’informations sur ces fonctionnalités et de nombreuses autres ajoutées dans cette version de Windows, visitez le [Centre de développement](https://developer.microsoft.com/windows/windows-10-for-developers) ou consultez la page [Nouveautés de Windows10 pour les développeurs](../whats-new/windows-10-build-14393.md).  |
| Build10586 (mise à jour de novembre, version1511) | Cette version de Windows10 a été publiée en novembre2015. Les fonctionnalités phares comprennent l’introduction des API ORTC (Object Real-Time Communications) pour la communication vidéo dans Microsoft Edge et des API fournisseurs permettant aux applications d’utiliser l’authentification faciale Windows Hello. [Plus d’informations sur les fonctionnalités introduites dans cette version.](../whats-new/windows-10-build-10586.md) |
| Build10240 (Windows10, version1507) | Il s’agit de la version initiale de Windows10 publiée en juillet2015. [Plus d’informations sur les fonctionnalités introduites dans cette version.](../whats-new/windows-10-build-10240.md) |

Nous recommandons vivement aux nouveaux développeurs et aux développeurs qui écrivent du code pour le grand public de toujours utiliser la dernière version de Windows (build 16299). Les développeurs écrivant des applications d’entreprise doivent sérieusement envisager de prendre en charge une **version minimale** antérieure.

## <a name="whats-different-in-each-uwp-version"></a>Qu’est-ce qui différencie chaque version UWP?

Les API nouvelles et modifiées pour UWP sont disponibles dans toutes les versions successives de Windows10. Pour savoir quelles fonctionnalités ont été ajoutées dans quelle version, consultez [Nouveautés de Windows](../whats-new/windows-10-version-latest.md).

Pour consulter des rubriques de référence qui énumèrent toutes les familles d’appareils et leurs versions, ainsi que les contrats d’API et leurs versions, consultez [Familles d’appareils](https://msdn.microsoft.com/library/windows/apps/dn706137.aspx) et [Contrats d’API](https://msdn.microsoft.com/library/windows/apps/dn706135.aspx).

## <a name="choose-which-version-to-use-for-your-app"></a>Choisir la version à utiliser pour votre application

Dans la boîte de dialogue **Nouveau projet Windows universel** de Visual Studio, vous pouvez choisir une version pour **Version cible** et pour **Version minimale**. En outre, vous pouvez modifier la **Version cible** et la **Version minimale** de votre application UWP dans la section *Application* des **Propriétés** de l'application.

* **Version cible**. Définit le paramètre *TargetPlatformVersion* de votre fichier projet. Détermine également la valeur de l’attribut *TargetDeviceFamily@MaxVersionTested* dans le manifeste de votre package d’application. La valeur que vous choisissez spécifie la version de la plateforme UWP ciblée par votre projet et par conséquent, l’ensemble d’API disponibles pour votre application. Nous vous recommandons donc de choisir la version la plus récente. Pour plus d’informations sur le manifeste de votre package d’application et pour obtenir des recommandations sur la configuration manuelle de TargetDeviceFamily, voir [TargetDeviceFamily](https://msdn.microsoft.com/library/windows/apps/dn986903).
* **Version minimale**. Définit le paramètre *TargetPlatformMinVersion* de votre fichier projet. Détermine également la valeur de l’attribut *TargetDeviceFamily@MinVersion* dans le manifeste de votre package d’application. La valeur que vous choisissez spécifie la version minimale de la plateforme UWP avec laquelle votre projet peut fonctionner.

Sachez que vous déclarez que votre application fonctionne sur n’importe quelle version de Windows entre la **Version minimale** et la **Version cible**. Si ces deux versions sont les mêmes, aucune action de votre part n’est nécessaire. Si elles sont différentes, voici quelques points à noter.

* Dans votre code, vous pouvez appeler librement (autrement dit, sans contrôles conditionnels) toute API existant dans la version spécifiée par **Version minimale**.
* Veillez à tester votre code sur un appareil exécutant la **Version minimale** pour vous assurer qu’il fonctionne sans avoir besoin d’API présentes uniquement dans la **Version cible**.
* La valeur de **Version cible** permet d’identifier toutes les références (contrat winmds) utilisées pour compiler votre projet. Mais ces références vous permettront de compiler votre code avec des appels à des API qui n’existeront pas nécessairement sur les appareils que vous avez déclaré prendre en charge (via **Version minimale**). Par conséquent, toute API introduite après la **Version minimale** doit être appelée via un code adaptatif. Pour plus d’informations sur le code adaptatif, voir [Code adaptatif de version](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).
