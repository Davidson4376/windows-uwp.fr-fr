---
title: Périphériques perdus
description: Un périphérique Direct3D peut présenter l’état opérationnel ou perdu.
ms.assetid: 1639CC02-8000-4208-AA95-91C1F0A3B08D
keywords:
- Périphériques perdus
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2f0b42a10c2cdd61aef84e08d6bd4f6408a978c3
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8922079"
---
# <a name="lost-devices"></a>Périphériques perdus


Un périphérique Direct3D peut présenter l’état opérationnel ou perdu. L’état *opérationnel* correspond à l’état normal du périphérique, dans lequel ce dernier s’exécute correctement et présente la totalité du rendu attendu. Le périphérique passe à l’état *perdu* lorsqu’un événement, tel que la perte du focus clavier dans une application en plein écran, empêche le rendu. L’état de perte se manifeste par la défaillance silencieuse de l’ensemble des opérations de rendu. Concrètement, les méthodes de rendu peuvent renvoyer des codes de réussites même si les opérations de rendu échouent.

Par conception, l’ensemble complet de scénarios pouvant entraîner la perte d’un périphérique n’est pas spécifié. Parmi les configurations possibles, citons tout de même la perte de focus, quand l’utilisateur active la combinaisonALT+TAB ou lors de l’initialisation d’une boîte de dialogue système. Les périphériques peuvent également être perdus suite à un événement de gestion de l’alimentation, ou lorsqu’une autre application fonctionne en plein écran. Par ailleurs, toute défaillance intervenant après la réinitialisation d’un périphérique définit ce dernier sur l’état de perte.

Toutes les méthodes dérivant de [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) fonctionnent obligatoirement après la perte d’un périphérique. Après la perte d’un périphérique, chaque fonction présente généralement les 3options suivantes:

-   Défaillance après une erreur de «périphérique perdu» - L’application doit reconnaître que le périphérique a été perdu, afin qu’elle enregistre un événement inattendu.
-   Défaillance silencieuse, renvoyant S\_OK ou tout autre code - Si une fonction échoue en silence, l’application ne peut généralement pas faire la distinction entre le résultat de réussite et la défaillance silencieuse.
-   Renvoi d’un code de retour.

## <a name="span-idrespondingtoalostdevicespanspan-idrespondingtoalostdevicespanspan-idrespondingtoalostdevicespanresponding-to-a-lost-device"></a><span id="Responding_to_a_Lost_Device"></span><span id="responding_to_a_lost_device"></span><span id="RESPONDING_TO_A_LOST_DEVICE"></span>Répondre à un périphérique perdu


Un périphérique perdu doit recréer les ressources (notamment les ressources de mémoire vidéo) après sa réinitialisation. Si un périphérique est perdu, l’application interroge ce dernier afin de déterminer si son état opérationnel peut être restauré. Si ce n’est pas possible, l’application attend qu’une fenêtre de restauration s’ouvre.

Si le périphérique peut être restauré, l’application le prépare en détruisant l’ensemble des ressources de mémoire vidéo et les chaînes d’échange. La réinitialisation est la seule procédure qui a une incidence après la perte d’un périphérique. C’est le seul moyen pouvant être mis à profit par une application pour restaurer l’état opérationnel. La réinitialisation échouera, sauf si l’application libère toutes les ressources allouées, notamment les cibles de rendu et les surfaces de profondeur-gabarit.

Dans la plupart des cas, les appels haute fréquence de Direct3D ne renvoient pas d’informations sur l’éventuelle perte du périphérique. L’application peut continuer à appeler des méthodes de rendu, sans recevoir de notification de périphérique perdu. En interne, ces opérations sont ignorées, jusqu’à restauration de l’état opérationnel du périphérique.

## <a name="span-idlockingoperationsspanspan-idlockingoperationsspanspan-idlockingoperationsspanlocking-operations"></a><span id="Locking_Operations"></span><span id="locking_operations"></span><span id="LOCKING_OPERATIONS"></span>Opérations de verrouillage


En interne, Direct3D s’implique suffisamment pour garantir la réussite de l’opération de verrouillage après la perte d’un périphérique. Toutefois, l’exactitude des données de ressources de mémoire de vidéo n’est pas garantie durant l’opération de verrouillage. Aucun code d’erreur ne sera renvoyé, c’est une certitude. Ainsi, l’écriture des applications peut se dérouler sans que le doute sur l’éventuelle perte du périphérique plane durant l’opération de verrouillage.

## <a name="span-idresourcesspanspan-idresourcesspanspan-idresourcesspanresources"></a><span id="Resources"></span><span id="resources"></span><span id="RESOURCES"></span>Ressources


Les ressources peuvent consommer de la mémoire vidéo. Dans la mesure où un périphérique perdu est déconnecté de la mémoire vidéo détenue par la carte, il n’est pas possible de garantir l’application de la mémoire vidéo après la perte du périphérique. Par conséquent, l’ensemble des méthodes de création de ressources sont implémentées pour réussir, pour finalement n’allouer qu’une mémoire système factice. Les ressources de mémoire vidéo devant être détruites avant le redimensionnement du périphérique, il n’existe aucune problématique de surallocation de mémoire vidéo. Grâce à ces surfaces factices, les opérations de copie et de verrouillage semblent fonctionner normalement, jusqu’à ce que l’application découvre la perte du périphérique.

Avant que l’état opérationnel du périphérique puisse être rétabli, l’ensemble de la mémoire vidéo doit être libéré. Les autres données d’état sont automatiquement détruites par la transition vers l’état opérationnel.

Nous vous encourageons à développer les applications avec un chemin de code unique, afin de répondre à la perte de périphérique. Probablement, ce chemin de code sera similaire, voire identique, au chemin de code emprunté pour initialiser le périphérique au démarrage.

## <a name="span-idretrieveddataspanspan-idretrieveddataspanspan-idretrieveddataspanretrieved-data"></a><span id="Retrieved_Data"></span><span id="retrieved_data"></span><span id="RETRIEVED_DATA"></span>Données récupérées


Direct3D permet aux applications de valider les états de texture et de rendu en fonction du rendu à une passe du matériel.

Direct3D permet également aux applications de copier les images générées ou précédemment écrites des ressources de mémoire vidéo sur des ressources non volatiles de mémoire système. Les images sources de tels transferts pouvant être à tout moment perdues, Direct3D prend en charge la défaillance de ces opérations de copies lorsqu’un périphérique est perdu.

Les opérations de copie peuvent échouer, dans la mesure où il n’existe aucune surface principale lorsque le périphérique est perdu. Les opérations de création de chaînes d’échange peuvent également échouer, car il est impossible de créer une mémoire tampon d’arrière-plan lorsque le périphérique est perdu.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Appareils](devices.md)

 

 




