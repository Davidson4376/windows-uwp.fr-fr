---
title: Limitations et les stratégies de présence enrichie
description: En savoir plus sur les stratégies et les limitations du système de présence de riches Xbox Live.
ms.assetid: 0ad21a75-0524-45a8-8d8a-0dec0f7d6d6f
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, présence riche, stratégies
ms.localizationpriority: medium
ms.openlocfilehash: f85974c0ccb38f3c33fb214ddaf24b98dd8dcdd0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643594"
---
# <a name="rich-presence-policies-and-limitations"></a>Limitations et les stratégies de présence enrichie

Lorsque vous implémentez présence enrichie pour votre titre, vous devez respecter les stratégies et les limitations suivantes.

-   Chaque titre doit avoir au moins 1-définie pour la chaîne, mais il n’existe aucune limite supérieure sur les jeux de chaînes combien vous pouvez avoir.
-   Vous devez définir une chaîne par défaut, ainsi que des chaînes de culture neutre pour chaque énumération et pour chaque chaîne de présence enrichie.
-   Vous pouvez utiliser numérique ou chaîne de statistiques pour remplir les paramètres dans vos chaînes. Vous ne pouvez pas utiliser les statistiques de date/heure.
-   Si vous utilisez des statistiques dans vos chaînes de présence enrichie, des statistiques (y compris les énumérations pour les statistiques) doivent être disponibles dans le même SCID & bac à sable.
-   Vous avez 1 ligne 44 caractères total (y compris les valeurs des paramètres). Cela revient aux limites de la présence de riches Xbox 360. Nous travaillons avec le client pour voir si la longueur de la chaîne peut atteindre. Il y aura une annonce si la chaîne peut être plus longue.
    -   Caractères Unicode sont requis et doivent être en mesure de fonctionner avec encodage UTF-8 pour l’affichage.
-   Les noms conviviaux doivent suivre ces règles :
    -   Les caractères autorisés sont 'A' à 'Z', 'a' à 'z', un trait de soulignement («\_») et la valeur ' 0' à ' 9'.

        Il n’existe aucune limite de caractère.

-   Aucune vérification de la chaîne n’est effectuée sur vos chaînes ; Vous devez effectuer toute vérification de la chaîne, telles que la vérification orthographique et en vérifiant que la chaîne a été localisée correctement.
    -   Si nous pensons à qu'un ensemble de chaîne est inapproprié (par exemple, le langage injurieux ou choquant), Microsoft empêche les titres d’à l’aide de la présence de riches jusqu'à ce que les chaînes ont été mis à jour à notre satisfaction.
-   Si votre titre n’est pas l’intégration avec la plateforme de données, il n’existe aucune option pour utiliser des statistiques en tant que paramètres dans vos chaînes.
    -   Toutes les chaînes doivent être complètement prédéfinis dans ce cas (aucuns jetons ne sont autorisés).
-   Noms de l’énumération doivent être uniques parmi toutes les énumérations et doivent être uniques pour les noms de statistiques.
-   Si une ligne dépasse le nombre de caractères qui peuvent être affichées, et qu’un saut de ligne, la ligne est automatiquement tronquée.
