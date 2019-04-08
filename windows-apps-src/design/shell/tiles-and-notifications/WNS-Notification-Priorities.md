---
title: Priorités de Notification WNS
description: Description des différentes priorités que vous pouvez définir sur une notification
ms.date: 01/10/2017
ms.topic: article
keywords: Windows 10, uwp, API WinRT, WNS
localizationpriority: medium
ms.openlocfilehash: 2c297a04786c6fbf1eb0600e63a04a6d88585864
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648704"
---
# <a name="wns-notification-priorities"></a>Priorités de Notification WNS
En définissant la priorité d’une notification avec un en-tête simple pour WNS publier des messages, vous pouvez contrôler la façon dont les notifications sont remises dans les situations sensibles de la batterie.

## <a name="power-on-windows"></a>Alimentation sur Windows
Lorsque vous utilisez davantage d’utilisateurs uniquement sur les appareils de l’alimentation par batterie, la réduction de consommation d’énergie est devenu une exigence standard pour toutes les applications. Si les applications consomment plus d’énergie que la valeur qu’ils fournissent, les utilisateurs peuvent désinstaller les applications. Bien que le système d’exploitation Windows réduise la consommation d’énergie sur la batterie lorsque cela est possible, il incombe l’application de fonctionner de manière efficace. 

Priorités WNS est une façon pour déplacer le travail non critiques hors tension de la batterie. Les priorités WNS informer le système qui reçoit les notifications doivent être remises immédiatement et qui peut attendre jusqu'à ce que le périphérique est connecté à une source d’alimentation. Avec ces indicateurs, le système peut remettre les notifications à l’heure exacte, ils sont les plus utiles à l’utilisateur et l’application. 

## <a name="power-modes-on-the-device"></a>Modes d’alimentation sur l’appareil
Tous les appareils Windows fonctionnement via une variété de modes d’alimentation (batterie économiseur de batterie et frais), et les utilisateurs attendent des comportements différents à partir d’applications dans les modes d’alimentation différentes. Lorsque l’appareil est activé, toutes les notifications doivent être remises. Dans le mode économiseur de batterie, uniquement les notifications les plus importantes doivent être remises. Quand l’appareil est branché, sync ou non au moment des opérations critiques peuvent être effectuées.

Windows ne sait pas quelles notifications sont importantes pour n’importe quel utilisateur ou d’une application, donc le système s’appuie totalement sur les applications pour définir la priorité de droite pour les notifications. 

## <a name="priorities"></a>Priorités
Quatre niveaux de priorité sont disponibles pour une application à utiliser lors de l’envoi de notifications push. La priorité est définie sur des notifications individuelles, ce qui vous permet de choisir quelles notifications doivent être remis immédiatement (par exemple, un message de messagerie instantanée) et celles qui peut attendre (par exemple, contactez les mises à jour de la photo).

Les priorités sont : 

|    Priority    |    Remplacement d’utilisateur    |    Description    |    Exemple    |
|----------------|---------------------|-------------------|---------------|
|    Élevée    |    Oui : l’utilisateur peut bloquer toutes les notifications à partir d’une application ou peut empêcher une application à partir de la limitation dans le mode économiseur de batterie.    |    Les notifications plus importantes qui doivent être remises immédiatement dans tous les cas lorsque l’appareil peut recevoir des notifications. Éléments tels que les appels VoIP ou les alertes critiques qui doivent sortir l’appareil appartiennent à cette catégorie.    |    Appels VoIP, les alertes critiques de temps-    |
|    Moyen    |    Oui : l’utilisateur peut bloquer toutes les notifications à partir d’une application ou peut empêcher une application à partir de la limitation dans le mode économiseur de batterie.    |    Voici ce qui n’est pas en tant qu’éléments importants, que vous n’avez pas besoin de se produire immédiatement, mais les utilisateurs seraient aurait ennuyés si elles ne sont pas exécutées en arrière-plan.    |    Synchronisation de compte de messagerie secondaire, une vignette dynamique met à jour.    |
|    Faible    |    Oui : l’utilisateur peut bloquer toutes les notifications à partir d’une application ou peut empêcher une application à partir de la limitation dans le mode économiseur de batterie.    |    Notifications qui conviennent uniquement lorsque l’utilisateur est à l’aide de l’appareil ou lors de l’activité en arrière-plan est logique. Ceux-ci sont mis en cache et pas traitées jusqu'à ce que l’utilisateur ne se connecte ou s’intègre dans leur appareil.    |    État du contact (en ligne/hors ligne)    |
|    Très faible     |    Non – il ne peut pas empêcher les notifications de priorité très faible de limitée dans le mode économiseur de batterie.    |    Il s’agit quasiment comme basse priorité, sauf les utilisateurs ne peuvent pas remplacer la stratégie d’économiseur de batterie. Ces notifications ne sont jamais fournies en économiseur de batterie.    |    Synchronisation des fichiers pour un service de synchronisation.    |

Notez que de nombreuses applications auront des notifications d’une priorité différente tout au long de leur cycle de vie. Étant donné que la priorité est définie sur une base par notification, ce n’est pas un problème. Une application VoIP peut envoyer une notification de priorité élevée pour un appel entrant et un suivi il avec une priorité basse un lorsqu’un contact est en ligne. 

## <a name="setting-the-priority"></a>Définition des priorités

Définition des priorités sur la demande de notification s’effectue via un en-tête supplémentaire sur la requête POST, `X-WNS-PRIORITY`. Il s’agit d’un entier compris entre 0 et 3 qui mappe à une priorité : 

| Nom de la priorité | Valeur de X-WNS-priorité | La valeur par défaut pour : |
|---------------|----------------------|------------------|
| Élevée | 1 | Toasts |
| Meduim | 2 | Vignettes et Badges |
| Faible | 3 | Brute |
| Très faible | 4 |  |

Pour être descendante compatible, définir une priorité n’est pas requis. Dans le cas où une application ne définit pas la priorité de leurs notifications, le système fournira une priorité par défaut. Les valeurs par défaut sont affichés dans le graphique ci-dessus et correspondent au comportement des versions existantes de Windows. 

## <a name="detailed-listing-of-desktop-behavior"></a>Liste détaillée du comportement du bureau 

Si vous commercialisez votre application entre les nombreux différentes références (SKU) de Windows, il est normalement préférable de suivre le graphique dans la section ci-dessus. 

Les comportements recommandées plus spécifiques pour chaque priorité sont répertoriées ci-dessous. Cela n’est pas une garantie que chaque appareil fonctionnera exactement selon le graphique. Les fabricants OEM sont gratuites configurer le comportement différemment, mais la plupart est proches de ce graphique. 

| État de l’appareil    | PRIORITÉ : Élevée    |    PRIORITÉ : Moyen        | PRIORITÉ : Faible    |    PRIORITÉ : Très faible    |
|-------------------------------------------------------|----------------------------------------------------|----------------------------------------------------|----------------------------------------------------|--------------------------|
|    Écran ou sur secteur    |    Fournir    |    Fournir    |    Fournir    |    Fournir    |
|    Écran et sur la batterie    |    Fournir    |    Si l’utilisateur exempté : fournir Else : traitement par lots     |    Si l’utilisateur exempté : fournir Else : cache *    |    Cache    |
|    Économiseur de batterie activée    |    Si l’utilisateur exempté : fournir Else : cache    |    Si l’utilisateur exempté : fournir Else : cache    |    Si l’utilisateur exempté : fournir Else : cache    |    Cache     |
|    Sur batterie + économiseur de batterie activée + écran    |    Si l’utilisateur exempté : fournir Else : cache    |    Si l’utilisateur exempté : fournir Else : cache    |    Si l’utilisateur exempté : fournir Else : cache    |    Cache    |

Notez que les notifications de basse priorité sont remises par défaut pour l’écran hors tension et batterie uniquement pour Windows Phone en fonction des appareils. Il s’agit de compatibilité généré avec la stratégie MPNS préexistante. Notez également que les quatrième et cinquième lignes sont les mêmes, en appelant simplement différents scénarios.

Pour exclure une application dans économiseur de batterie, les utilisateurs Accédez à « Batterie utilisation par App » dans les paramètres et sélectionnez « Autoriser l’application pour exécuter des tâches en arrière-plan ». Cette sélection utilisateur exempte de l’application à partir de l’économiseur de batterie pour la haute, moyenne et les notifications de basse priorité. Vous pouvez également appeler [BackgroundExecutionManager API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_) à demander par programmation l’autorisation de l’utilisateur.  

## <a name="related-topics"></a>Rubriques connexes
- [Vue d’ensemble des services de notifications Push Windows (WNS)](windows-push-notification-services--wns--overview.md)
- [Demander une autorisation d’exécuter en arrière-plan](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_)
- 
