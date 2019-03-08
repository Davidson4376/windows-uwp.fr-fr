---
title: Effectuer une recherche contextuelle de configuration
description: Découvrez comment configurer une recherche contextuelle pour marquer des clips de jeu et diffusions.
ms.assetid: 6cb2cb10-811a-4b20-9b9b-a3fc59a033c2
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox un, configuration du service, effectuer une recherche contextuelle, clip de jeu, de diffusion
ms.localizationpriority: medium
ms.openlocfilehash: c8c78f115a160c82a28881a3c551a958cdf4b68c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607924"
---
# <a name="configuring-contextual-search"></a>Effectuer une recherche contextuelle de configuration

## <a name="configuration-info"></a>Informations de configuration

### <a name="designing-your-data"></a>Conception de vos données
Lorsque vous configurez votre titre pour effectuer une recherche contextuelle, il est important de réfléchir à ce qui rend votre titre unique.  Si un utilisateur de recherche pour le contenu vidéo, ce qui serait haut d’esprit à rechercher ?  Degré de granularité les données doivent-elles être ?

Effectuer une recherche contextuelle prend en charge la possibilité pour filtrer et trier le contenu, vos métadonnées doivent refléter ces constructions de recherche pour garantir une couverture excellente.  Règles générales est de considérer les données de votre jeu dans deux tableaux croisés dynamiques :
1. Jeux d’état - ex.  Niveau, caractère, arme, Map, rôle, le Point de point de contrôle/un récit, DLC
2. Compétences de l’utilisateur - ex. Ratio de K/D, Prestige, rang, vitesse du moment de l’exécution

Le premier tableau croisé dynamique est très utile pour le filtrage de contenu, la seconde est très utile pour le tri.  En outre, selon le type de contenu SURFACÉE, différents niveaux de granularité stat sont importantes.  Pour les diffusions, en supposant qu’il soit un nombre limité de diffusions actives à un moment donné, les données de granularité inférieur garantira une probabilité plus élevée d’une recherche réussie.  Pour les éléments de jeu, le nombre en permanence augmente, donc les données de granularité plus fine garantit des résultats plus précis.

Il est probable que vous avez déjà ces données créées pour les fonctionnalités de Xbox Live, comme la présence de riches ou statistiques héros, c’est une formidable occasion d’exploiter déjà effectué le travail.

Si vous utilisez des statistiques pour filtrer le contenu, n’oubliez pas que toutes les statistiques d’effectuer une recherche contextuelle sont disponibles pour les requêtes à tout moment.  Vous devez concevoir votre gestion des événements en conséquence pour prendre en charge cela.

Par exemple, si vous utilisez des statistiques telles que SinglePlayerMap et MultiplayerMap pour filtrer le contenu, le lecteur ne va être à la fois dans un d’eux.  Toutefois, les deux valeurs seront disponibles pour les requêtes à partir du service à tout moment.  Il est important que lorsque vous définissez une option, vous désactivez les autres.  Pour les statistiques en fonction de chaîne, une chaîne vide est très utile (veillez ne pas à inclure dans votre configuration de l’interface utilisateur en tant qu’option).

### <a name="configuring-a-stat-for-contextual-search"></a>Configuration d’un Stat pour effectuer une recherche contextuelle
Il est facile de configurer votre titre pour effectuer une recherche contextuelle une fois que vous avez configuré les événements et les statistiques qui alimentent le balisage.  Consultez les autres documents XDP ou partenaires existants sur la configuration de d’effectuer une recherche contextuelle si vous n’êtes pas familiarisé.

![](../images/contextual_search/config02.png)

L’image ci-dessus est un exemple de la page principale après avoir cliqué sur la vignette d’effectuer une recherche contextuelle dans la partie de configuration de Service principale de XDP.

Même s’il s’agit de la première fois que vous avez visité cette page, il est possible que vous ne verrez statistiques déjà configurés pour effectuer une recherche contextuelle.  Il s’agit, car toutes les statistiques de votre héros sont automatiquement configurés pour effectuer une recherche contextuelle. Vous n’êtes pas obligé de conserver ces statistiques héros dans votre configuration, mais ils sont généralement excellente pour le tri (et déjà terminé pour vous).

Actuellement, le nombre maximal d’instances statistiques, que vous pouvez configurer pour effectuer une recherche contextuelle est 10.

Dans cette page, chacun des statistiques est représenté de la manière suivante : Priorité : nom complet (nom de l’Instance Stat)

Chacune d'entre elles est expliquée plus en détail dans la section suivante.

![](../images/contextual_search/config01.png)

L’illustration ci-dessus est l’écran qui s’affiche si vous choisissez de créer une nouvelle recherche contextuelles stat ou modifier et existant.

Pour configurer correctement un stat pour effectuer une recherche contextuelle, procédez comme suit :
1. Sélectionnez votre instance stat.

  ![](../images/contextual_search/config03.png)

  Veuillez noter que seule stat instances sont prises en charge - stat modèles ne sont pas acceptées.  Vous devez également connaître la visibilité que vous avez défini pour l’instance stat (configuré dans la partie de statistiques de XDP ou partenaires).  Uniquement les statistiques qui sont marqués comme **Open** apparaîtra dans les expériences de tiers.

2. Choisissez votre priorité stat. Il s’agit d’un moyen de délimiter l’importance de cette statistique par rapport à d’autres personnes pour des expériences/algorithmes de recherche.  Les valeurs acceptables sont 1-10 (1 est le plus élevé).  Laissez que 0 ou vide pour cette option pour être ignoré.
3. Ajoutez votre nom d’affichage.  Il s’agit d’une type chaîne localisables qui est exposée à l’utilisateur final.
4. Vérifiez si vous utiliserez cette statistique pour filtrer ou trier les résultats.  Vous pouvez vérifier à la fois dans certains cas (si la valeur de la statistique est un nombre - de préférence dans une plage définie par).
5. Choisissez la façon dont votre valeur stat est représentée.  Dans le cas, vous avez 3 options.
   * Valeur brute - la statistique est représenté comme est et n’a aucune exigence de plage.  Cela est surtout pour le tri.
   * Définissez - les valeurs sont mappées à des chaînes localisables individuelles.  Cela est surtout pour le filtrage.
   * Plage de - les valeurs comprises entre un min et de la plage maximale.  Ce type est très utile si vous souhaitez filtrer et trier à partir de ces données.

#### <a name="explaining-sets"></a>Explication des jeux
Si vous n’êtes pas familiarisé avec les jeux, ils sont un moyen de mapper des valeurs statistiques potentielles pour les chaînes localisables qui peuvent être exposées à un utilisateur final.  Ils sont très importantes pour les statistiques d’effectuer une recherche contextuelle que vous souhaitez filtrer avec.

{% brutes %} Un titre d’exemple, supposons que j’ai un entier Stat appelé SinglePlayerMap.  Affichage d’une série de nombres pour les utilisateurs finaux n’aucun sens.  Par conséquent, au lieu de cela, je crée un ensemble de mappages comme ```{{0, “Blood Gulch”}, {1, “Lockout”}, {2, “Zanzibar”}}```.  L’utilisateur verra la chaîne, mais nous utilisons le numéro dans le cadre de la requête de recherche recherche contextuelle.  Vous pouvez mapper des entiers ou des chaînes à des chaînes localisées dans des jeux.
{% endraw %}

### <a name="updating-your-contextual-search-document"></a>La mise à jour de votre Document de recherche contextuelle
À l’heure actuelle, vous pouvez facilement modifier les statistiques sur votre document de recherche contextuelle que nous prenons en charge la diffusion pour le moment (c'est-à-dire les données volatiles).  Une fois que nous avons commencé prendre en charge le clip de jeu de balisage (septembre 2015), si vous avez décidé d’ajouter ou supprimer des statistiques, vous affecte les éléments qui ont déjà été indexées.  Si vous ajoutez un nouveau stat, clips uniquement de jeux et diffusions créées après que la modification entrera en vigueur seront marquées et signalées dans les requêtes à l’aide de la nouvelle statistique. Ancien clips jeu ne sera pas interrogé à partir de cette statistique. Si vous supprimez un stat à partir du document d’effectuer une recherche contextuelle, l’ancien stat attaché à l’élément de jeu sera inutile et n’est pas retourné.
