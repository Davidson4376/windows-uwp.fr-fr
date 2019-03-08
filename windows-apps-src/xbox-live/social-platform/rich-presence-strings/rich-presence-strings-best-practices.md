---
title: Meilleures pratiques de présence enrichie
description: Découvrez les meilleures pratiques pour l’utilisation de présence de riches Xbox Live.
ms.assetid: 51a84137-37e4-4f98-b3d3-5ae70e27753d
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, présence riche, meilleures pratiques
ms.localizationpriority: medium
ms.openlocfilehash: 75268575dd9dce59141d8909a59909bc973edbec
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615664"
---
# <a name="rich-presence-best-practices"></a>Meilleures pratiques de présence enrichie

Les conseils suivants vous aideront à tirer le meilleur parti de présence riche dans votre jeu. N’oubliez pas que la présence riche plus chaînes vous définissez le plus riche expérience d’autres joueurs qui découvrent les personnes de votre jeu.

-   Permet de vos statistiques dans vos chaînes, vous permettent de définir votre chaîne et ensuite inquiétez pas.

    Si votre chaîne contient le nom de mappage qu’il contient, et que vous utilisez la statistique CurrentMap pour remplir les champs vides, puis le service met à jour votre chaîne de façon appropriée, comme vos joueurs à partir d’un mappage pour mapper de voyage dans le jeu. Cette approche vous permet ne pas à vous soucier de maintenir la chaîne à jour, tant que votre titre envoie les événements appropriés à la plateforme de données.

    Votre titre doit définir la chaîne de base de présence enrichie avec le service de présence régulièrement, pour vous assurer que les informations de présence enrichie pour un utilisateur sont correctes et le service à l’aide de la chaîne de base correcte.

-   Présence enrichie permet d’ouvrir de nouvelles opportunités de conversation. Créer des chaînes qui sont susceptibles de susciter l’intérêt dans un jeu pour nouveaux joueurs ainsi que les joueurs occasionnelles qui peuvent avoir manqué une fonctionnalité spéciale.

-   Créer des chaînes de présence enrichie qui motivent les joueurs entrent en action. Par exemple, au lieu de dire « Lecture sur Mausoleum, « par exemple, « demande l’assistance ; S’agissant de Mausoleum ». Utilisez l’état de présence enrichie permettant des scénarios froid, par exemple ajouter un jeu en cours. Puis un autre gamers peut participer et aider à.

-   Créer des chaînes de présence enrichie qui permettent aux joueurs pour présenter leurs résultats, telles que la saisie semi-automatique de niveaux ou une découverte des zones secrets.

-   Localisez vos chaînes de présence enrichie et leurs paramètres associés afin que les joueurs Xbox dans le monde entier peuvent faire partie de la Communauté que vous encourager.

-   Certains paramètres changent très rapidement, mais peut prendre du temps pour les nouvelles données de présence enrichie à apparaître dans un ami. Si votre chaîne contient « arme actuel » et le joueur a la possibilité de basculer entre leurs PISTOLET et les analysent leur présence enrichie chaîne ne peut pas être totalement exact à un moment donné. Toutefois, dans certains cas, qui n’est pas un problème. Si votre chaîne de présence enrichie contient la valeur de total ennemis vaincus, et la valeur est désactivée par 1 ou 2 pendant quelques secondes, qui peut être OK pour votre scénario.
