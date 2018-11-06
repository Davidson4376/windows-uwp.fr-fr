---
author: jwmsft
ms.assetid: 3A477380-EAC5-44E7-8E0F-18346CC0C92F
title: Virtualisation des données ListView et GridView
description: Améliorez les performances et le délai de démarrage des éléments ListView et GridView via la virtualisation des données.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 92b81c79eb1be9e21aa7c306ef31b0b3bb62e7d1
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6042539"
---
# <a name="listview-and-gridview-data-virtualization"></a>Virtualisation des données ListView et GridView


**Remarque**pour plus d’informations, voir la session //build/ [Accroître considérablement les performances lorsque les utilisateurs interagissent avec de grandes quantités de données dans GridView et ListView](https://channel9.msdn.com/Events/Build/2013/3-158).

Améliorez les performances et le délai de démarrage des éléments [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) et [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) via la virtualisation des données. Pour la virtualisation de l’interface utilisateur, la réduction d’un élément et la mise à jour progressive des éléments, voir [Optimisation des options d’interface ListView et GridView](optimize-gridview-and-listview.md).

Une méthode de virtualisation des données est requise pour tout jeu de données si volumineux qu’il ne peut pas ou ne doit pas être entièrement stocké dans la mémoire. Une première partie du jeu de données est chargée en mémoire (à partir du disque local, du réseau ou du cloud), puis la virtualisation de l’interface utilisateur est appliquée à ce jeu de données partiel. Par la suite, les données sont chargées à la demande dans le jeu de données principal, de façon incrémentielle ou à partir de points arbitraires (accès aléatoire). La pertinence de la virtualisation des données pour vous dépend de nombreux facteurs :

-   la taille du jeu de données ;
-   la taille de chaque élément ;
-   la source du jeu de données (disque local, réseau ou cloud) ;
-   la consommation de mémoire totale de votre application.

**Remarque**n’oubliez pas qu’une fonctionnalité est activée par défaut pour ListView et GridView qui affiche les éléments visuels d’espace réservé temporaire lorsque l’utilisateur effectue un mouvement panoramique/défilement rapide. Ces éléments visuels d’espace réservé sont remplacés par votre modèle d’élément lors du chargement des données. Vous pouvez désactiver la fonctionnalité en définissant [**ListViewBase.ShowsScrollingPlaceholders**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders) sur false. Toutefois, si vous procédez ainsi, nous vous recommandons d’utiliser l’attribut x:Phase pour restituer progressivement les éléments dans votre modèle d’élément. Voir [Mettre à jour les éléments ListView et GridView de façon progressive](optimize-gridview-and-listview.md#update-items-incrementally).

Voici plus d’informations sur les techniques de virtualisation des données incrémentielles et à accès aléatoire.

## <a name="incremental-data-virtualization"></a>Virtualisation incrémentielle des données

La virtualisation incrémentielle des données assure le chargement séquentiel des données. Un contrôle [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) faisant appel à la virtualisation incrémentielle des données peut être utilisé pour afficher une collection d’un million d’éléments, mais seulement 50 éléments sont chargés initialement. À mesure que l’utilisateur parcourt la collection, les 50 éléments suivants sont chargés. Au fil du chargement des éléments, la taille curseur de la barre de défilement diminue. Pour appliquer ce type de virtualisation des données, vous devez écrire une classe de source de données qui implémente ces interfaces.

-   [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx)
-   [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) (C#/VB) ou [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) (C++/CX)
-   [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)

Une source de données de ce type constitue une liste en mémoire qui peut être étendue en permanence. Le contrôle d’éléments demande les éléments à l’aide des propriétés de nombre et d’indexeur [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx) standard. Le nombre doit représenter le nombre d’éléments localement et non la taille réelle du jeu de données.

Lorsque le contrôle d’éléments approche de la fin des données existantes, il appelle [**ISupportIncrementalLoading.HasMoreItems**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading.hasmoreitems). Si vous renvoyez **true**, il appellera [**ISupportIncrementalLoading.LoadMoreItemsAsync**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading.loadmoreitemsasync) en transmettant un nombre conseillé d’éléments à charger. Selon la source des données chargées (disque local, réseau ou cloud), vous pouvez choisir de charger un nombre d’éléments différent de celui recommandé. Par exemple, si votre service prend en charge les lots de 50 éléments, mais que le contrôle d’éléments en demande seulement 10, vous pouvez en charger 50. Chargez les données depuis votre back end, ajoutez-les à votre liste et déclenchez une notification de modification via [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) ou [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) afin que le contrôle d’éléments soit informé des nouveaux éléments. Renvoyez également le nombre d’éléments que vous avez effectivement chargés. Si vous chargez moins d’éléments que le nombre recommandé ou que l’utilisateur a parcouru le contrôle d’éléments encore plus loin entre-temps, de nouveaux éléments sont demandés à votre source de données et le cycle se poursuit. Vous pouvez en savoir plus en téléchargeant l' [exemple de liaison de données XAML](https://code.msdn.microsoft.com/windowsapps/Data-Binding-7b1d67b5) pour Windows8.1 et réutilisez son code source dans votre application Windows 10.

## <a name="random-access-data-virtualization"></a>Virtualisation des données par accès aléatoire

La virtualisation des données par accès aléatoire permet le chargement à partir d’un point arbitraire du jeu de données. Un contrôle [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) faisant appel à la virtualisation des données par accès aléatoire et utilisé pour afficher une collection d’un million d’éléments peut charger les éléments 100 000 à 100 050. Si l’utilisateur affiche ensuite le début de la liste, le contrôle charge les 50 premiers éléments. À tout moment, le curseur de la barre de défilement indique que le contrôle **ListView** contient un million d’éléments. La position du curseur de la barre de défilement est déterminée par rapport à l’emplacement des éléments visibles dans le jeu de données de la collection. Ce type de virtualisation des données peut considérablement réduire les besoins en mémoire et les temps de chargement pour la collection. Pour l’appliquer, vous devez écrire une classe de source de données qui récupère les données à la demande, gère un cache local et implémente les interfaces suivantes:

-   [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx)
-   [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) (C#/VB) ou [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) (C++/CX)
-   (Facultatif) [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070)
-   (Facultatif) [**ISelectionInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877074)

[**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070) fournit des informations sur les éléments utilisés activement par le contrôle. Le contrôle d’éléments appelle cette méthode chaque fois que son affichage est modifié et inclut les deux ensembles de plages suivants :

-   l’ensemble des éléments qui se trouvent dans la fenêtre d’affichage ;
-   un ensemble d’éléments non virtualisés utilisés par le contrôle qui ne se trouvent pas forcément dans la fenêtre d’affichage :
    -   une mémoire tampon d’éléments autour de la fenêtre d’affichage que le contrôle d’éléments conserve afin d’assurer la fluidité du défilement panoramique tactile ;
    -   l’élément sélectionné ;
    -   le premier élément.

En implémentant [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070), votre source de données sait quels éléments doivent être récupérés et mis en cache et quand nettoyer les données du cache qui ne sont plus requises. **IItemsRangeInfo** utilise des objets [**ItemIndexRange**](https://msdn.microsoft.com/library/windows/apps/Dn877081) pour décrire un ensemble d’éléments en fonction de leur index dans la collection. Cela permet d’éviter l’utilisation de pointeurs d’éléments, qui peuvent ne pas être corrects ou stables. L’interface **IItemsRangeInfo** est conçue pour être utilisée par une seule instance d’un contrôle d’éléments, car elle s’appuie sur les informations d’état de ce contrôle d’éléments. Si plusieurs contrôles d’éléments ont besoin d’accéder aux mêmes données, une instance distincte de la source de données est requise pour chacun d’eux. Ils peuvent partager un cache commun, mais la logique de vidage du cache sera plus complexe.

Voici la stratégie de base pour votre source de données dans le cadre de la virtualisation des données par accès aléatoire :

-   Lorsqu’un élément est demandé
    -   S’il est disponible dans la mémoire, renvoyez-le.
    -   S’il n’est pas disponible, renvoyez une valeur null ou un élément d’espace réservé.
    -   Utilisez la demande relative à un élément (ou les informations de plage de [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070)) pour connaître les éléments requis et pour récupérer les données des éléments de façon asynchrone à partir de votre back end. Une fois les données récupérées, déclenchez une notification de modification via [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) or [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) afin que le contrôle d’éléments soit informé du nouvel élément.
-   (Facultatif) À mesure que la fenêtre d’affichage du contrôle d’éléments change, identifiez les éléments requis à partir de la source de données via votre implémentation de [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070).

En outre, la stratégie permettant de déterminer à quel moment charger les éléments de données, le nombre d’éléments à charger et les éléments à conserver en mémoire dépend de votre application. Voici quelques considérations générales à retenir :

-   Effectuez des demandes asynchrones pour les données ; ne bloquez pas le thread d’interface utilisateur.
-   Trouvez la fourchette idéale pour la taille des lots dans lesquels vous récupérez des éléments. Préférez les lots de grande taille aux lots « bavards ». Pas trop petits pour ne pas avoir à faire de trop nombreuses petites demandes ; pas trop volumineux pour ne pas que la récupération prenne trop de temps.
-   Prenez en compte le nombre souhaité de demandes en attente simultanées. Il est plus facile d’en effectuer une à la fois, mais cela peut s’avérer trop lent si le temps de réponse est élevé.
-   Pouvez-vous annuler les demandes de données ?
-   Si vous utilisez un service hébergé, existe-t-il un coût par transaction ?
-   Quels types de notifications sont fournis par le service lorsque les résultats d’une requête sont modifiés ? Serez-vous informé si un élément est inséré à l’index 33 ? Si votre service prend en charge les requêtes basées sur une instruction key-plus-offset, cela peut s’avérer préférable à la simple utilisation d’un index.
-   Quel degré d’intelligence recherchez-vous pour la prérécupération des éléments ? Allez-vous essayer de suivre la direction et la vitesse du défilement pour prédire les éléments requis ?
-   Quel degré d’agressivité recherchez-vous pour le vidage du cache ? Il s’agit d’un compromis entre la mémoire et l’expérience.




