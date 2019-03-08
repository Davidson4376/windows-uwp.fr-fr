---
title: Présence enrichie
description: Découvrez comment Xbox Live riche présence peut aider à promouvoir votre titre.
ms.assetid: 00042359-f877-4b26-9067-58834590b1dd
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, présence riche
ms.localizationpriority: medium
ms.openlocfilehash: 0ae0d0189954ed8fa9a0bc7651d6a90b9789c388
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604114"
---
# <a name="rich-presence"></a>Présence enrichie

À l’aide de présence enrichie, votre jeu peut publier ce qu’un lecteur fait dès maintenant. Par exemple, votre jeu peut utiliser des chaînes de présence enrichie pour afficher, tous les joueurs l’état des lecteurs de votre jeu, tels que *Away*. Informations de présence enrichie sont visibles par les joueurs connectés à Xbox Live. Dans l’idéal, une chaîne de présence enrichie indique autres joueurs Xbox Live ce que fait un lecteur, et où dans votre jeu le joueur fait. Le concept de chaînes de présence enrichie est le même sur Xbox One, comme c’était sur la Xbox 360, mais la nouvelle implémentation suit le divertissement comme une initiative de Service. Les rubriques de cette section décrivent comment configurer vos chaînes de présence enrichie, puis comment définir la chaîne pour un utilisateur de lecture de votre titre.


## <a name="definitions"></a>DÉFINITIONS

**Énumérations**  
Une énumération est une liste d’une dimension dans le jeu. Exemples de ces dimensions dans le jeu sont des armes, classes de caractères, cartes et ainsi de suite. Nous souhaitons afficher la liste des armes possible dans votre jeu, une liste de toutes les classes de caractères possibles ou les cartes et ainsi de suite.

**Paire de la chaîne de paramètres régionaux**  
Chaque chaîne de présence enrichie possible doit avoir des paramètres régionaux associée à spécifier dans les paramètres régionaux la chaîne doit/peut être utilisée. Chaque énumération aura également un ensemble de paires de chaîne de paramètres régionaux ainsi.

**String-set**  
Un ensemble de la chaîne se compose d’un groupe de paires de chaîne de paramètres régionaux. Ce jeu définit les valeurs possibles d’une chaîne de présence enrichie pour tous les paramètres régionaux possibles ou les valeurs possibles pour une énumération de tous les paramètres régionaux possibles.

**Noms conviviaux**  
Il existe deux types de noms conviviaux :

**Chaîne de présence enrichie**  
Le nom convivial pour un ensemble de chaîne est un identificateur unique sous la forme d’une chaîne utilisée pour faire référence à un ensemble de chaîne.

**Énumération**  
Ces noms conviviaux sont utilisés pour identifier de manière unique une énumération particulier, comme l’énumération des armes ou l’énumération de classe de caractères.


## <a name="in-this-section"></a>Dans cette section

[Configuration de présence enrichie](rich-presence-strings-configuration.md)  
Comment configurer la présence enrichie pour une utilisation dans votre titre.

[Présence riche la mise à jour des chaînes](rich-presence-strings-updating-strings.md)  
Comment mettre à jour les chaînes de présence riche à partir de votre titre.

[Meilleures pratiques de présence enrichie](rich-presence-strings-best-practices.md)  
Meilleures pratiques pour utilisent de présence enrichie dans votre titre.

[Limitations et les stratégies de présence enrichie](rich-presence-strings-policies-and-limitations.md)  
Les stratégies sur l’utilisation de présence enrichie dans votre titre.

[Annexe de présence enrichie](rich-presence-strings-appendix.md)  
Obtenir des exemples supplémentaires et informations sur la plateforme de données pertinentes pour la présence enrichie.

[Programmation Xbox Live présence enrichie](programming-rich-presence.md)  
Montre comment utiliser la présence de riches avec Xbox Live.
