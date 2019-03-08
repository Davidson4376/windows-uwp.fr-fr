---
title: Méthodes recommandées RTA
description: En savoir plus sur les meilleures pratiques lors de l’utilisation du service Xbox Live en temps réel activité.
ms.assetid: 543b78e3-d06b-4969-95db-2cb996a8bbd3
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, activité en temps réel
ms.localizationpriority: medium
ms.openlocfilehash: 7733aab9330c316ad5938cf9a2ef763e06f19b9a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613564"
---
# <a name="real-time-activity-rta-best-practices"></a>Meilleures pratiques de l’activité (RTA) en temps réel
Ces recommandations vous aideront à tirer le meilleur parti d’utilisation de votre titre d’agrégation RTA.


## <a name="the-basics"></a>Les principes de base

RTA utilise des sessions de WebSocket pour créer une connexion persistante avec le client. C’est la façon dont le service fournira des statistiques à vous. Un client doit envoyer une demande de connexion authentifiée et RTA utilisera le jeton fourni dans la demande pour vérifier la connexion peut être effectuée et puis d’établir.

Une fois que la connexion a été établie, votre application peut envoyer des requêtes pour vous abonner aux statistiques particulier. Sur un abonnement réussi, RTA retournera la valeur actuelle et des métadonnées supplémentaires, comme le type de statistique, dans le cadre de la charge utile de réponse. Ensuite, RTA transmettront les mises à jour qui se produisent à la statistique, tandis que le client est abonné.

Lorsque votre titre ne nécessite pas les mises à jour en temps réel sur une statistique, il doit se terminer son abonnement à cette statistique.


## <a name="handling-disconnects"></a>Gestion des déconnexions

Titres doivent être conscients que lorsque le jeton d’authentification pour l’utilisateur arrive à expiration, leur session va se terminer par le service. Le titre doit être capable de détecter lorsque ces événements se produit, reconnectez-vous et se réabonner à toutes les statistiques dont il a été précédemment abonné au.

Connexions de l’agrégation RTA sont fermées après deux heures par sa conception, ce qui force le client à se reconnecter. Pour cela, car le jeton d’authentification pour la connexion est mis en cache pour enregistrer la bande passante du message. Par la suite ce jeton expire. Par la fermeture de la connexion et l’obliger à se reconnecter, le client est obligé d’actualiser le jeton d’authentification.

Un client peut également obtenir déconnecté en raison de fournisseur de services Internet d’un utilisateur rencontre des problèmes ou lorsque le processus pour le titre est suspendu. Dans ce cas, un événement de WebSocket est déclenché pour informer le client. En général, il est meilleure pratique consiste à pouvoir gérer déconnecte du service.

> [!WARNING]
> Si un client utilise la valeur RTA pour les sessions multijoueurs et est déconnecté pendant 30 secondes, le [Directory(MPSD) de Session multijoueur](../multiplayer/multiplayer-appendix/multiplayer-session-directory.md) détecte que la session de l’agrégation RTA est fermée et intervient l’utilisateur de la session. Il incombe au client d’agrégation RTA pour détecter lorsque la connexion est fermée et lancer une reconnexion et se réabonner avant le MPSD termine la session.

## <a name="managing-subscriptions"></a>La gestion des abonnements

Par rapport à la gestion des abonnements lorsqu’un jeton arrive à expiration, le titre de la doit effectuer le suivi à tout moment abonnements ont été apportées. Lors de l’abonnement avec succès, RTA retourne un identificateur unique pour chaque statistique souscrit. Dans toutes les futures mises à jour cette statistique, l’identificateur sera être utilisé au lieu du nom de la statistique. Pour cela, pour économiser la bande passante. Les clients sont chargés de gérer leur propre mappage des statistiques pour l’ID d’abonnement RTA.


## <a name="unsubscribing"></a>Désabonnement

Avoir abonnements inutilisés n’est pas recommandée. Le service limite le nombre d’abonnements, qu'un utilisateur peut avoir par titre à un moment donné. Si vous vous abonnez à tous les éléments et quoi que ce soit, vous pouvez atteindre cette limite, et cela peut empêcher l’abonnement pour certaines statistiques importantes. (Pour plus d’informations sur les limites d’abonnement, consultez **limite**, plus loin dans cette rubrique.)

Par exemple, votre titre peut uniquement besoin d’un abonnement à une certaine scène. Lorsque l’utilisateur entre cette scène, votre titre doit s’abonner. Lorsque l’utilisateur quitte cette scène, ces statistiques doivent être annulés. De même, il est un message de résiliation d’abonnement global qui peut être utilisé si aucun abonnement n’est nécessaires.

Après la résiliation d’abonnement, l’identificateur d’abonnement pour cette statistique est n’est plus utilisable.


## <a name="awareness-of-latent-items-in-the-queue"></a>Reconnaissance d’éléments latentes dans la file d’attente

Lors du désabonnement à partir d’une statistique, il est possible qu’il existe une mise à jour des statistiques de toujours en cours d’atteindre votre client. Par conséquent, même si votre titre a annulé son abonnement uniquement à partir d’une statistique, sachez qu’il peut toujours obtenir une mise à jour ou deux liés à cette statistique.

Il est recommandé d’ignorer les mises à jour des statistiques lorsque l’identificateur d’abonnement n’est pas reconnu.


## <a name="ignore-messages-you-do-not-understand"></a>Ignorer les Messages que vous ne comprenez pas

Il est possible que le protocole de messagerie sera mis à jour. Pour que votre application reste indépendant de tous les nouveaux messages, nous recommandons que votre titre simplement ignorer les types de message inconnu.


## <a name="throttles"></a>Limitations

RTA impose certaines limitations sur le service :

-   UserStats : 1000
-   Présence : 2500

Si un client atteint la limite de limitation qu'est soit reçoivent une erreur dans le cadre d’un appel de s’abonner/annuler l’abonnement, ou elle va être déconnectée. Dans les deux cas, plus d’informations sur les limites de limitation qui a été atteint est disponible pour le client, ainsi que le message d’erreur ou message de déconnexion.

Lorsque vous développez votre titre, n’oubliez pas ces concepts. Si vous effectuez une opération d’extrême, vous pouvez avoir une expérience d’application détérioré parce que le service peut être limite vos appels. Droite permet désormais de, le service 1 000 abonnements aux statistiques par instance d’une application. En plus de celles, une instance d’une application peut également vous abonner à la longueur de la liste d’un utilisateur tout le monde des mises à jour de présence. Ce nombre est en cours de paramétrage, et il peut changer dans les futures versions.
