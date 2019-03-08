---
title: Migration d’un arbitre
description: Découvrez comment migrer un arbitre dans Xbox Live multijoueurs 2015
ms.assetid: 9fb5d2c0-d548-4a22-b64e-6b215f78d22e
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, arbitre, 2015 multijoueurs
ms.localizationpriority: medium
ms.openlocfilehash: 7e250841fb0731a903c0e9c5bdeeecd42f07ac84
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629704"
---
# <a name="migrating-an-arbiter"></a>Migration d’un arbitre

À un moment donné pendant une session terminée, vous devrez peut-être sélectionner un arbitre de nouveau à l’aide de la migration de l’arbitre. Il existe deux types de migration :

-   **Migration de l’arbitre sans perte de données**
-   **Migration d’arbitre de basculement**

L’organigramme suivant illustre comment migrer un arbitre.

![](../../images/multiplayer/Multiplayer_2015_HostMigration.png)

## <a name="graceful-arbiter-migration"></a>Migration de l’arbitre sans perte de données

Dans la migration de l’arbitre sans perte de données, l’arbitre sortant peut faciliter la tâche de migration et déterminer un arbitre de nouveau. Ce type de migration utilise le paramètre d’un arbitre, comme décrit dans [Comment : Définir un arbitre pour une Session MPSD](multiplayer-how-tos.md).


## <a name="failover-arbiter-migration"></a>Migration de l’arbitre de basculement

Dans une migration arbitre de basculement, connexion à l’arbitre précédente est perdue et les autres homologues doivent déterminer un arbitre nouveau pour la session. Migration d’arbitre de basculement définit l’hôte de jeton d’appareil et gère les codes d’état HTTP/412 est simplement comme sans perte de données migration arbitre. Toutefois, il existe plusieurs approches pour la sélection d’un arbitre nouveau lors d’une migration d’arbitre de basculement.
## <a name="select-arbiter-using-the-host-candidate-list"></a>Sélectionnez l’arbitre à l’aide de la liste de candidats hôte

Vous pouvez configurer MPSD pour fournir une liste de candidats hôte ordonné selon matchmaking métriques de qualité de service sont mesurées au cours de certaines opérations. Le client peut utiliser cette liste pour déterminer un arbitre de nouveau. Pour tirer parti de cette liste pendant la migration de l’arbitre, chaque homologue peut :

1.  Identifier la position de la liste de l’arbitre précédente.
2.  Évaluer la console suivante dans la liste.
3.  Si la console est la console locale, utilisez-le comme nouveau l’arbitre.
4.  Si la console n’est plus présente dans la session multijoueur, ou s’est déconnecté de ses pairs, passer à la prochaine candidate dans la liste et l’évaluer en comme dans les étapes précédentes.
5.  Si vous atteignez la fin de la liste avec aucune nouvelle arbitre sélectionné, utilisez une approche prudente pour la sélection d’arbitre, qui permettre interrompre la connectivité. Consultez « Utiliser la sélection arbitre gourmand ».

| Remarque                                                                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Il n’est pas recommandé pour créer une liste de candidats hôte dans le jeu après matchmaking au moyen de sondes de QoS dans titre explicites. Si ce mécanisme est indispensable, ont votre client à utiliser le jeton d’appareil hôte au lieu des informations utilisateur, par exemple, les ID utilisateur Xbox, pour déterminer les candidats de l’arbitre. |


### <a name="select-arbiter-using-peer-voting"></a>Sélectionnez l’arbitre à l’aide d’homologue de vote

S’il existe une connectivité totale entre tous les homologues, ils permettent messages homologue voter et sélectionnez un nouveau arbitre. L’arbitre nouveau puis met à jour le jeton d’appareil hôte pour la session à l’aide d’une mise à jour synchronisée. Consultez [Comment : Mettre à jour d’une Session multijoueur](multiplayer-how-tos.md).


### <a name="use-greedy-arbiter-selection"></a>Utiliser la sélection de l’arbitre gourmand

Parfois, aucune liste de candidats hôte n’est disponible ou connectivité QoS n’est pas nécessaire, par exemple, pour les responsabilités de l’arbitre pure. Dans ce cas, votre client peut utiliser une approche de sélection d’arbitre gourmand. Dans ce cas, un homologue doit définir l’arbitre nouveau dès qu’il détecte que l’arbitre d’origine a quitté la session de jeu, comme indiqué par le **MultiplayerSessionChanged** événement. Tous les autres homologues voient un code d’état HTTP/412 lorsque vous tentez de définir l’hôte de jeton d’appareil, en supposant qu’aucune autre modification à la session n’est apportées à ce stade. Seul homologue réussit à en sélectionnant l’arbitre de nouveau.


## <a name="see-also"></a>Voir également

[Détails de la session MPSD](mpsd-session-details.md)
