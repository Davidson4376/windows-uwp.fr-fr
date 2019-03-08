---
title: Configuration de présence enrichie
description: Découvrez comment configurer des chaînes de la présence de riches Xbox Live.
ms.assetid: 7b08d46d-d3aa-42eb-93f2-ecd9338fccea
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, présence riche
ms.localizationpriority: medium
ms.openlocfilehash: d69aaadaa1f64d1cb6eb40b52016a330d924b9b9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590474"
---
# <a name="rich-presence-configuration"></a>Configuration de présence enrichie

Il y aura une partie de la portail (XDP Xbox développeur) dédié à la configuration des chaînes de présence enrichie. Tant qu’ils sont disponible dans XDP, une configuration manuelle doit être utilisée. Configuration manuelle, vous devez définir les chaînes dans un modèle de feuille de calcul fournies par votre responsable de compte de développeur et de l’envoyer à votre responsable de compte de développeur pour l’ingestion dans l’environnement.

-   **Chaînes**
-   **Énumérations**
-   **Ingestion**
-   **Jeu d’exemple**
-   **Configuration des statistiques pour la présence enrichie**
-   **Exemple de Configuration d’énumération**
-   **Exemple de chaîne de Configuration**


## <a name="strings"></a>Chaînes

Pour vos chaînes de présence riche, vous devez définir les jeux de chaîne. Chaque ensemble de chaîne doit disposer d’un nom convivial à utiliser en tant qu’identificateur. Chaque ensemble de chaîne créée doit contenir un groupe de chaînes, localisées en conséquence pour les paramètres régionaux dans lequel le titre est libéré. Outre les chaînes localisées, une chaîne de culture neutre doit également être fournie. Cette chaîne est utilisée si une demande effectuée pour une variable locale qui n’est pas spécifiée.

Chaque ensemble de chaîne peut également rendre à utiliser dans le jeu données par le paramétrage des chaînes pour améliorer davantage les. Un ensemble de chaîne peut avoir autant de paramètres comme vous le souhaitez, tant que les chaînes résultantes ne transitent pas par les restrictions de caractères. Consultez [présence enrichie des chaînes : Stratégies et les Limitations](rich-presence-strings-policies-and-limitations.md) pour plus d’informations.


## <a name="enumerations"></a>Énumérations

Les énumérations ci-dessous sont définies via les événements à partir de la plateforme de données.


## <a name="statistics"></a>Statistiques

Vous pouvez utiliser des valeurs numériques de statistiques dans vos chaînes de présence enrichie. Par exemple, dans un jeu de tir, vous pouvez la chaîne de présence enrichie pour montrer combien tue le joueur a obtenu jusqu’ici. La configuration doit simplement montrer que le paramètre doit être remplacé par la statistique « Kill nombre ». La statistique « Kill nombre » a été créée via le système d’événements de schéma commune via à la plateforme de données et doit être définie via la configuration de statistiques. Lorsque la chaîne est lu par un autre utilisateur, le service sera allez ensuite et récupérer la valeur la plus récente pour la statistique « Kill nombre » ce qui permet de la chaîne soit toujours à jour.


## <a name="state"></a>État

Vous pouvez également choisir d’utiliser les statistiques qui représentent les valeurs d’état réel de vos chaînes de présence enrichie. Par exemple, dans un jeu de course, vous pouvez utiliser une chaîne de présence enrichie à afficher quelle voiture dirige actuellement le joueur, ou les mappage ou le suivre le joueur est racing sur. Il existe deux étapes pour configurer les informations d’état :

1.  Pour plus d’informations d’état, définir des énumérations pour toutes les chaînes dans le jeu. Par exemple, si vous avez cinq cartes dans votre jeu et que vous souhaitez que les mappages pour faire partie de vos chaînes de présence enrichie, les cinq cartes doivent être énumérés dans les chaînes localisées.
2.  Lier les énumérations pour les statistiques. Ici, là encore, le service récupère la valeur la plus récente de la plateforme de données en recherchant la valeur de la statistique. Il détermine ensuite la valeur d’énumération se traduit par la statistique.


## <a name="ingestion"></a>Ingestion

Une fois que vous avez terminé votre feuille de calcul, l’envoyer à votre responsable de compte de développeur pour l’ingestion. L’équipe de présence enrichie sera passez en revue votre configuration et assurez-vous qu’il est configuré correctement et puis avez il ingéré par le service.


## <a name="example-game"></a>Jeu d’exemple

Pour le reste de l’exemple, supposons que je travaille sur le compartiment de dernière exécution de jeu. Il existe un grand nombre de types différents de compartiments dans mon jeu comme compartiments en bois, compartiments silver et compartiments golden. Je décide de créer une statistique pour chaque type de compartiment ont commencé. Il existe également des emplacements différents de mon jeu dans lequel lancer les compartiments, comme la très volumineuse, le désert et de la plage. Une fois un compartiment est déclenchée, elle fait voler, vous donc toujours être recherche de nouveaux compartiments. Certains compartiments, vous devez disposer d’engrenage spéciale (vous avez besoin d’un démarrage résister un kick) pour lancer correctement le compartiment. Le jeu inclut aussi un mode multijoueur Si suffisamment joueurs essayez de lancer le compartiment en même temps, vous n’avez pas besoin de l’amorçage approprié.

Jetons un œil à quelle configuration de chaîne de présence enrichie ressemble pour ce jeu.


## <a name="configuring-statistics-for-rich-presence"></a>Configuration des statistiques pour la présence enrichie

Pour utiliser une statistique générée par la plateforme de données dans vos chaînes de présence riche, le service doit connaître les noms des statistiques. Vous devez inclure ces informations dans la section du modèle de feuille de calcul qui vous invite à spécifier les statistiques que vous utilisez dans vos chaînes.


## <a name="enumeration-configuration-example"></a>Exemple de Configuration d’énumération

Voici un exemple de définition de deux énumérations. Nous commençons par créer une énumération pour les mappages dans le jeu. Le jeu a trois mappages, laquelle obtenir un nom convivial afin que nous pouvons y faire référence facilement. Pour chacun des paramètres régionaux, nous créons une chaîne localisée pour le nom de ce mappage. Notez qu’il existe une localisation par défaut globale que bien une culture neutre.

Ensuite, nous allons examiner les armes dans le jeu. Il existe trois armes dans me jeux, où ils noms conviviaux et les chaînes localisées. Nous faisons cela pour toutes les énumérations.

Énumération | Statistiques connexes | Nom convivial | Paramètres régionaux | Chaîne
----------- | ----------------- | ------------- | ------ | ----
Map | CurrentMap | Map_Mountains | par défaut | Montagnes
 |  |  |  | fr | Montagnes
 |  |  |  | fr-FR | Montagnes
 |  |  |  | en-GB | Montagnes
 |  |  |  |  de | Gebirge
 | |  |  | etc |
 | |  | Map_Desert | par défaut | Desert
 |  ||  |  fr | Desert
 |  ||  |  fr-FR | Desert
 |  ||  |   en-GB | Desert
 |  ||  |  de | Wuste
 |  ||  |  etc |
| |  |  Map_Beach | par défaut | Plage
 ||    |  | fr | Plage
 ||    |  | fr-FR | Plage
 ||    |  | en-GB | Plage
 ||    |  | de | Strand
 | |   |  | etc |
Démarrage | CurrentWeapon | Boot_Light | par défaut | Maigre
 |  ||  |  fr | Maigre
 |  ||  |   fr-FR | Maigre
 |  ||  |   en-GB | Maigre
 |  ||  |   de | Leicht
 |  ||  |   etc  |
 |  | |  Boot_Medium | par défaut | Moyen
 |  |  |  | fr | Moyen
 |  |  |  | fr-FR | Moyen
 |  |  |  | en-GB | Moyen
 |  |  |  | de | Mittel
 |  |  |  | etc |
 |  | |  Boot_Strong | par défaut | Strong
 |  |  |  | fr | Strong
 |  |  |  | fr-FR | Strong
 |  |  |  | en-GB | Strong
 |  |  |  | de | Fortement avec
 |  ||  | etc

## <a name="string-configuration-example"></a>Exemple de chaîne de Configuration

Maintenant que les énumérations et les statistiques ont été définies, vous pouvez créer vos chaînes.

Dans cette table d’exemple ci-dessous, la première colonne est un nom convivial pour aider à en faisant référence à un ensemble de chaîne par rapport à un autre. Dans cet exemple, nous avons décidé d’utiliser la chaîne « playingMap » pour le premier jeu de chaîne, puis « totalKicked » pour le deuxième jeu de chaîne, puis « mode multijoueur » pour la troisième.

Les deux colonnes assemblent. Voici les paires de chaîne de paramètres régionaux pour la chaîne de présence enrichie. Dans le premier exemple, nous souhaitons utiliser la chaîne en anglais « jouer sur {0}». Le {0} jeton sera remplacé par une valeur. Nous localiserez ensuite la chaîne dans d’autres paramètres régionaux. Comme dans les énumérations, il doit y avoir une valeur par défaut, ainsi que les chaînes de culture neutre à utiliser dans le cas qu’un utilisateur à l’aide de paramètres régionaux que vous n’avez pas spécifié d’une paire de chaîne de paramètres régionaux. Les paramètres régionaux utilisés pour les énumérations, doivent correspondre les paramètres régionaux pour vos chaînes.

La dernière colonne est pour les paramètres utilisés pour remplir les champs vides dans la chaîne de présence enrichie. Si vous souhaitez que le Service de présence pour rechercher la valeur du paramètre dans le service de statistiques, vous devez utiliser le nom de Stat pour faire référence au paramètre ici. À l’aide de vos statistiques dans vos chaînes vous permettra à définir votre chaîne et ensuite inquiétez pas. Si votre chaîne contient le nom de mappage qu’il contient, et que vous utilisez la statistique CurrentMap pour remplir les champs vides, puis le service met à jour votre chaîne de façon appropriée, comme vos joueurs à partir d’un mappage pour mapper de voyage dans le jeu. Si vous n’utilisez pas de statistiques dans vos chaînes de présence enrichie, vous ne pouvez spécifier aucun paramètre.

Dans l’exemple ci-dessous, nous voulons utiliser le mappage actuel généré par les statistiques dans la première chaîne, puis la statistique BucketsKicked dans la deuxième chaîne. Notez que le troisième exemple n’inclut pas tous les paramètres. La chaîne est définie sur sa propre. Il ne fait pas référence à toutes les statistiques.

Nom convivial | Paramètres régionaux | Chaîne | Paramètres
--- | --- | --- | ---
playingMap | par défaut | Lecture sur la carte :{0} | CurrentMap
 |  | fr | Lecture sur la carte :{0} |
 |  | fr-FR | Lecture sur la carte :{0} |
 |  | en-GB | Lecture sur la carte :{0} |
 |  | de | Auf Spielt Karte : {0} |
 |  | etc. | |
totalKicked | par défaut | Déclenchée {0} compartiments ! | BucketsKicked
 |  | fr | Déclenchée {0} compartiments ! |
 |  | fr-FR | Déclenchée {0} compartiments ! |
 |  | en-GB | Déclenchée {0} compartiments ! |
 |  | de | {0} Eimer getreten ! |
 |  | etc. | |
multijoueur | par défaut | Lecture multijoueurs |
 |  | fr | Lecture multijoueurs |
 |  | fr-FR | Lecture multijoueurs |
 |  | en-GB | Lecture multijoueurs |
 |  | de | Spielt Mehrspieler |
 |  | etc. | 

Il n’existe aucune limite sur le nombre de chaînes que vous pouvez créer, mais vous devez disposer au moins une chaîne pour votre titre.

« BucketsKicked » est simplement un nombre qui peut obtenir remplacé dans votre chaîne. « CurrentMap » est un exemple d’une statistique où la chaîne provenance de l’énumération. Lorsque votre titre définit présence enrichie à une de ces chaînes, nous utilisons la configuration pour savoir quels paramètres sont nécessaires.
