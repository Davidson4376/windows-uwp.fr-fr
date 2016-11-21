---
author: normesta
Description: "Il s’agit d’une rubrique de hub destinée aux développeurs abordant de manière exhaustive la relation de la Protection des informations Windows avec les fichiers, les mémoires tampons, le Presse-papiers, la mise en réseau, les tâches en arrière-plan et la protection des données verrouillées."
MS-HAID: dev\_enterprise.edp\_hub
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Protection des informations Windows (WIP)
translationtype: Human Translation
ms.sourcegitcommit: 724d9edf67d0f73ceb3eb2ac323e0a0f42f2dd0d
ms.openlocfilehash: f9cfa8d1d7ea4e78208a4fb3fc853884a13a676c

---

# Protection des informations Windows (WIP)

__Remarque__ La stratégie de Protection des informations Windows peut être appliquée à Windows10, version1607.

Cette stratégie protège les données qui appartiennent à une organisation en appliquant des stratégies qui sont définies par l’organisation. Si votre application est incluse dans ces stratégies, toutes les données générées par votre application sont soumises aux restrictions de stratégie. Cette rubrique vous aide à créer des applications qui appliquent ces stratégies plus en douceur sans avoir d’impact sur les données personnelles de l’utilisateur.

## Tout d’abord, qu’est-ce que la Protection des informations Windows?

WIP est un ensemble de fonctionnalités prenant en charge la gestion des périphériques mobiles (GPM) de l’organisation sur les postes de travail, les ordinateurs de bureau, les tablettes et les téléphones. WIP permet à l’organisation de mieux maîtriser comment ses données sont gérées sur les appareils gérés par l’organisation. Par exemple, les administrateurs peuvent identifier les applications autorisées à accéder aux fichiers appartenant à l’organisation et indiquent si les utilisateurs peuvent copier des données à partir de ces fichiers pour les coller ensuite dans des documents personnels.

Voici le principe: Les utilisateurs inscrivent leurs appareils dans le système de gestion des appareils mobiles (GPM) de l’organisation. Un administrateur de l’organisation de gestion utilise Microsoft Intune ou System Center Configuration Manager (SCCM) pour définir, puis déployer une stratégie sur les appareils inscrits.

Cette stratégie identifie les applications qui peuvent accéder aux données d’entreprise (appelée *liste des applications autorisées* de la stratégie). Ces applications peuvent accéder aux fichiers d’entreprise protégés, aux réseaux privés virtuels (VPN) et aux données d’entreprise sur le Presse-papiers ou via un contrat de partage. La stratégie définit également les règles qui régissent les données. Par exemple, si les données peuvent être copiées à partir de fichiers appartenant à l’entreprise, puis collées dans des fichiers n’appartenant pas à l’entreprise.

Si les utilisateurs désinscrivent leur appareil à partir du système GPM de l’organisation, les administrateurs peuvent effacer à distance les données d’entreprise de l’appareil.

![Cycle de vie de la Protection des informations Windows](images/wip-lifecycle.png)

> **En savoir plus sur la Protection des informations Windows** <br>
* [Présentation de la Protection des informations Windows](https://blogs.technet.microsoft.com/windowsitpro/2016/06/29/introducing-windows-information-protection/)
* [Protéger vos données d’entreprise à l’aide de la Protection des informations Windows (WIP)](https://technet.microsoft.com/library/dn985838(v=vs.85).aspx)

Si votre application se trouve sur la liste autorisée, toutes les données générées par votre application sont soumises aux restrictions de stratégie. Cela signifie que si les administrateurs révoquent l’accès de l’utilisateur aux données d’entreprise, ces utilisateurs perdent l’accès à toutes les données produites par votre application.

Cela est approprié si votre application est conçue uniquement à des fins professionnelles Mais si votre application crée des données que les utilisateurs considèrent comme personnelles, vous souhaiterez rendre votre application *compatible* afin qu’elle fasse la distinction de manière intelligente entre les données personnelles et les données d’entreprise. Cette application est considérée comme *compatible avec l’entreprise* dans la mesure où elle peut appliquer la stratégie d’entreprise de manière fluide tout en préservant l’intégrité des données personnelles de l’utilisateur.

## Créer une application compatible avec l’entreprise

Utiliser les API WIP pour rendre votre application compatible avec l’entreprise et la déclarer comme tel.

Rendez votre application compatible si elle doit être utilisée à la fois à des fins personnelles et professionnelles.

Rendez votre application compatible si vous voulez gérer de manière fluide l’application des éléments de stratégie.

Par exemple, si la stratégie permet aux utilisateurs de coller des données d’entreprise dans un document personnel, vous pouvez empêcher les utilisateurs d’avoir à répondre à une boîte de dialogue de consentement avant le collage des données. De même, vous pouvez présenter des boîtes de dialogue informatives personnalisées en réponse à ces types d’événements.

Si vous êtes prêt à rendre votre application compatible, reportez-vous à l’un de ces guides:

**Pour les applications de plateforme Windows universelle (UWP) générées à l’aide de C#**

[Créer une application compatible qui utilise des données d’entreprise et personnelles](wip-dev-guide.md)

**Pour les applications de bureau que vous créez à l’aide de C++**

[Créer une application compatible qui utilise des données d’entreprise et personnelles (C++)](http://go.microsoft.com/fwlink/?LinkId=822192)

Entre autres choses, les applications d’entreprise compatibles partagent les qualités suivantes:

* Elles protègent les données d’entreprise, que celles-ci soient au repos, en cours d’utilisation ou en cours de transfert.
* Elles reconnaissent les données personnelles et évitent que ces données soient soumises aux restrictions de stratégie.
* Elles reconnaissent les données d’entreprise et protègent ces données lorsqu’elles arrivent dans l’application.
* Elles protègent les données d’entreprise qui quittent l’application.

  Par exemple elles empêchent l’envoi de données vers un point de terminaison réseau n’appartenant pas à l’entreprise, habillent les données sous forme chiffrée portable avant d’autoriser leur itinérance et demandent potentiellement (selon les paramètres de stratégie) l’autorisation à l’utilisateur avant de coller les données d’entreprise dans une application qui ne figure pas sur la liste d’autorisation.







 



<!--HONumber=Nov16_HO1-->


