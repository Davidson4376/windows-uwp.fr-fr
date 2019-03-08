---
title: Vue d'ensemble technique de Stockage connecté
description: Une présentation approfondie sur le fonctionnement interne de stockage connectés.
ms.assetid: a0bacf59-120a-4ffc-85e1-fbeec5db1308
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, stockage connecté
ms.localizationpriority: medium
ms.openlocfilehash: 6eddd11a370b8dcadc5108fe00539c2c6d1d9d1a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624054"
---
# <a name="connected-storage"></a>Stockage connecté

> [!NOTE]
> Ce document a été écrit à l’origine pour les développeurs gérés partenaire Xbox One.  Partie du contenu Xbox One spécifique telles que le stockage Local et le titre peut être ignoré pour UWP sur Windows.  Le contenu conceptuel et les API dans ce document sont toujours applicables.  Veuillez contacter votre représentant Microsoft (le cas échéant) avec des questions.

Le stockage et enregistrer des modèles sur Xbox One sont très différentes de celles de la Xbox 360 ; Xbox One possède un modèle d’application plus flexible qui prend en charge rapide de commutation, plusieurs applications simultanées, des applications et rapide suspendre et reprendre des applications. Données stockées à l’aide de l’API de stockage connecté automatiquement se déplace pour les utilisateurs sur plusieurs consoles Xbox One et est également disponibles pour une utilisation hors connexion.

Cette rubrique traite des sujets suivants :

-   À l’aide de l’API de stockage connectés à des jeux de magasin enregistré et les données d’application sur Xbox One.
-   Meilleures pratiques pour la conception d’une application enregistrement des systèmes, afin qu’ils s’intègrent également les avantages d’expérience utilisateur fournies par le modèle d’application Xbox One.
-   Comment le système de stockage connectés augmente la vitesse avec laquelle votre application peut enregistrer des données
-   Comment le système gère les conflits de données et la synchronisation des données à partir de plusieurs consoles sans nécessiter de l’interface d’application.
-   Le modèle de résilience connecté de stockage, qui est conçu pour que l’application a toujours un affichage cohérent des données stockées dans des conteneurs individuels même en cas de perte de connectivité ou d’alimentation réseau interrompue.

> [!NOTE]
> Le terme *application*, comme utilisé ici, fait référence à n’importe quelle application en cours d’exécution sur la console, y compris les jeux.

## <a name="overview"></a>Vue d’ensemble

Le modèle d’application Xbox One permet aux utilisateurs d’utiliser plusieurs applications à la fois, ce qui signifie que votre application ne peut pas demander à l’utilisateur d’attente de données doit être enregistré avant la désactivation de la console ou passer à une autre application. Xbox One ils bénéficient également d’avoir leurs données soient valables sur consoles automatiquement, afin que chaque console Xbox One semble leur propre console. La plateforme Xbox One fournit l’API de stockage connecté pour aider à votre application de répondre à ces impératifs.

Le système de stockage connecté permet aux applications de stocker les données car une ou plusieurs *blobs* dans *conteneurs*. Lorsqu’une application enregistre des données, il est rapidement copié à partir de la partition exclusive dans la partition partagée afin que les tâches de stockage des données sur le disque et le télécharger dans Xbox Live titre stockage peuvent être gérés en dehors de la durée de vie de votre application.

Lorsque votre application demande des données d’un utilisateur spécifique à partir du système de stockage connecté, le système vérifie avec le cloud pour les données mises à jour automatiquement et avertit les utilisateurs s’ils doivent attendre les données à télécharger. Le système vous demande également aux utilisateurs de choisir entre les données en conflit dans certains cas, par exemple quand un utilisateur a joué hors connexion sur plusieurs consoles, ou quand une autre console est chargement données enregistrées pour cet utilisateur.

Votre application a également une quantité dédiée, mais limitée, de l’espace de stockage cloud pour chaque utilisateur, donc les utilisateurs n’aura à faire des choix difficiles sur la suppression définitive des données à partir d’une application pour créer un espace pour les sauvegardes d’une autre application. Toutefois, il existe une quantité limitée d’espace de stockage sur le disque dur Xbox One pour la mise en cache enregistre localement, par conséquent, le système fournit une expérience utilisateur pour la libération d’espace de cache local. Les utilisateurs sont dans le contrôle de ce qui est mis en cache localement ; jamais perdent l’accès aux données que qui les intéressent lors de la lecture hors connexion. Le système de stockage connecté permet également une petite quantité de données indépendamment de l’utilisateur soient stockées localement pour l’application. Ces données par ordinateur, n’erre pas et ne sont pas téléchargées vers le cloud.

Xbox One système de stockage connecté s’occupe de la gestion de l’alimentation système afin que les écritures sur le disque dur et les téléchargements vers le cloud en attente sont gérées automatiquement, il est inutile de titres afficher l’interface utilisateur de dire « enregistrer en cours, veuillez ne pas Mettez hors tension votre console. »

### <a name="the-xbox-one-app-model-and-app-navigation"></a>La navigation d’application et le modèle application Xbox One

Xbox One permet aux utilisateurs de basculer rapidement entre plusieurs applications. Des applications bien écrites permettent aux utilisateurs de reprendre là où ils étiez pendant leur dernière activité avec contexte pertinent chargement rapidement, même si l’application a été arrêtée depuis la dernière fois que l’utilisateur a accédé.

Consoles Xbox One peuvent exécuter qu’une seule application dans la partition exclusive à la fois. Présenter une expérience utilisateur de commutation rapide nécessite l’application en cours d’exécution exclusive d’arrêter rapidement lorsque l’utilisateur veut exécuter un autre. Lorsqu’un utilisateur tente de basculer vers une autre application, le système envoie une notification d’interruption à l’application active. Pendant ce temps, l’application doit enregistrer l’état et retourner à partir de sa fonction d’interruption. Le système impose une limite de temps maximale de 1 seconde pour cette opération. Si l’application n’a pas renvoyé dans 1 seconde, le système termine de force l’application. Les applications ne peuvent pas empêcher les utilisateurs de vous quittez cette page, comme avec le modèle de navigation sur les smartphones modernes et des tablettes.

Il existe également d’autres cas dans lequel une application exclusive est suspendue, telles que le système saisir un état inactif ou de faible puissance lorsque l’utilisateur appuie sur le bouton d’alimentation sur la console. Une fois que l’application est suspendue, il peut reprendre sans être déchargés par le système. Cela active la fonctionnalité de reprise de rapide. La chose importante à retenir est qu’applications suspendues peuvent également être arrêtées ou reprises. L’application doit toujours enregistrer l’état dans le cas où elle est arrêtée.

Pour fonctionner correctement avec le modèle d’application Xbox One, les applications doivent être préparées à sérialiser rapidement l’état dans la mémoire tampon afin que l’état concerné peut être enregistré dans la seconde suspendre laps de temps.

Notez qu’il existe une différence entre les données enregistrées sur le jeu de l’utilisateur et les données sur l’état de l’application, comme l’emplacement dans les menus. Outre les économies jeu lors de l’application est interrompue, vous devez envisager de rendre persistant l’état menu si l’utilisateur se trouve au milieu de la configuration d’un paramètre ou en personnalisant un caractère n’appartenant pas le moteur de jeu principal.

Les utilisateurs peuvent laisser un jeu suspendu pour très longtemps. Envisagez d’offrir une expérience différente lorsque le jeu est repris après suspension long. Suppression d’un utilisateur dans un combat à partir de leur campagne de retour peut être une expérience souffrent et inattendue si l’utilisateur n’a pas lu pendant deux semaines, tandis qu’un saut d’une heure serait plus courant et garantissez un retour rapide au jeu.

Pour plus d’informations sur le modèle d’application Xbox One, consultez les ressources suivantes :

-   [Changement d’Application moderne pour le salon](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Documents/Xfest%202012/Xfest%202012%20-%20Modern%20Application%20Switching%20for%20the%20Living%20Room.pptx), une présentation au Xfest 2012
-   [L’expérience d’interpréteur de commandes](https://developer.xboxlive.com/_layouts/xna/download.ashx?file=PROD-D_Experience.pptx&folder=platform/xfest2013), une présentation au Xfest 2013
-   [Processus de gestion de la durée de vie pour Xbox One](https://developer.xboxlive.com/en-us/platform/development/education/Documents/PLM%20for%20Xbox%20One.aspx), un livre blanc sur GDN

> [!NOTE]
> Certains des présentations dans [Xfest 2012](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2012.aspx) et [Xfest 2013](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2013.aspx) contiennent des informations qui sont obsolètes en raison de l’annonce que Xbox One prend en charge la lecture hors connexion.


### <a name="storage-options-on-xbox-one"></a>Options de stockage sur Xbox One

Xbox One fournit plusieurs options de stockage, chacun avec ses propres avantages et les contraintes. Les applications devront peut-être utiliser une combinaison d’options en fonction des spécifications des applications.


<a name="connected-storage"></a>Stockage connecté
-----------------
Stockage connecté est conçu pour aider les applications à enregistrer des données de jeu Xbox One et autres États-données d’application approprié qui doit se déplacer entre les consoles. L’API de stockage connecté, spécifiques à la Xbox One, aide à enregistrer et de charger ces données. L’API fonctionne en conjonction avec le modèle d’application Xbox One.

L’API de stockage connecté fournit les fonctionnalités suivantes :

-   Les applications peuvent enregistrer rapidement jusqu'à 16 Mo de données à la fois dans la mémoire tampon dans la partition système, qui est ensuite mis en cache localement sur le disque dur par le système et téléchargée vers le cloud.
- Pour les partenaires gérés et ID@Xbox les développeurs :
  - 256 Mo par utilisateur/application de stockage cloud.
- Pour les développeurs de programme Xbox Live Creators :
  - 64 Mo par utilisateur/application de stockage cloud.
-   Réponse robuste aux pannes d’alimentation, les applications n’êtes pas obligé de gérer des données partielles en cours d’enregistrement.
-   Données sont automatiquement téléchargées vers le cloud, même lorsque l’application n’est pas en cours d’exécution.
-   Données seront disponibles sur les consoles Xbox One qui sont connectés à Xbox Live.
-   Xbox Live de gère la gestion de la synchronisation et de conflit entre les périphériques sans nécessiter une intervention par l’application.

Pour plus d’informations sur l’API de stockage connecté, consultez la section appropriée dans xblesdk.chm (qui documente les API de SDK Xbox Live Extension) dans le Kit de développement Xbox Live.


<a name="xbox-live-title-storage"></a>Xbox Live titre stockage
-----------------------
Le service de stockage de titre offre une API REST d’inter-plateformes pour le stockage de données avec les fonctionnalités suivantes :

-   Fournit des données à partager entre les utilisateurs, les applications et les différentes plateformes
-   Prend en charge les binaires, fichiers JSON et de configuration
-   Pour les partenaires gérés et ID@Xbox les développeurs :
    - 256 Mo par utilisateur/application de stockage cloud
    - 256 Mo de par le stockage global de titre
- Pour les développeurs de programme Xbox Live Creators :
  -   64 Mo par utilisateur/application de stockage cloud
  -   256 Mo de par le stockage global de titre

Configuration requise pour l’utilisation du service :

-   Console Xbox One doit être en ligne pour accéder au service
-   Toutes les interactions de service doivent être effectuées pendant l’exécution de l’application. transfert de données n’est pas effectué automatiquement en arrière-plan.

Pour plus d’informations, consultez *Xbox Live titre stockage*, dans la documentation XDK.


<a name="local-temporary-storage"></a>Stockage temporaire local
-----------------------
Dans la console, une application a accès à un stockage local temporaire avec les caractéristiques suivantes :

-   2 Go de stockage de disque dur dédié, accessible par le chemin d’accès t :\\.
-   Contenu de ce stockage peut-être être supprimées lors de l’application n’est pas en cours d’exécution.

Pour plus d’informations sur le stockage local, consultez stockage Local, dans la documentation de XDK.


<a name="configuring-your-app-for-connected-storage"></a>Configuration de votre application pour le stockage connecté
------------------------------------------
Lorsque vous utilisez l’API de stockage connecté, tous les lire et écrire les opérations sont associées avec un Xbox Live principal Service Configuration ID (SCID), défini dans le fichier manifeste de votre application, AppXManifest.xml :

```xml
      <Extensions>
        <mx:Extension Category="xbox.live">
        <mx:XboxLive TitleId="<your title ID>" PrimaryServiceConfigId="<your SCID>"
        RequireXboxLive="<boolean indicating Live requirement>" />
        </mx:Extension>
      </Extensions>
```

Pour plus d’informations sur l’acquisition de titre ID et SCID pour votre application, consultez *paramètre des bacs à sable pour Xbox Live Development*, dans la documentation XDK.



## <a name="connected-storage-system-concepts"></a>Connecté de stockage : concepts de système

Cette section décrit les composants de système de stockage connecté, leurs relations et leurs utilisations appropriées.

### <a name="connected-storage-space"></a>Espace de stockage connectés

À un niveau élevé, toutes les données dans le système de stockage connecté est associé à un utilisateur ou un ordinateur (par exemple, une console Xbox One individuel). Toutes les données enregistrées par une application pour un utilisateur ou un ordinateur est stocké dans un espace de stockage connectés.

Chaque utilisateur de votre application obtient un espace de stockage connectés avec une limite de stockage total de 256 Mo. Il est important de noter que ce stockage est dédié à votre application autonome, il n’est pas partagé avec d’autres applications.

Votre application a également 64 Mo d’espace dans un espace de stockage connectés local pour l’ordinateur. Cet espace de stockage est indépendant des utilisateurs et sont accessibles même si aucun utilisateur n’êtes connecté.

Pour acquérir un espace de stockage connectés, les appels de l’application le *ConnectedStorageSpace.GetForUserAsync méthode* ou *ConnectedStorageSpace.GetForMachineAsync méthode*. Cette une opération potentiellement longue, en particulier si l’utilisateur a enregistré des données sur un périphérique et reprend le jeu pour la première fois sur un autre appareil. Pour plus d’informations sur ce processus et les conditions d’erreur possibles qui peuvent se produire pendant que l’application attend pour acquérir un espace de stockage connecté, consultez *synchronisation d’un espace de stockage connectés*, plus loin dans ce document.

Une fois que l’application acquiert un **ConnectedStorageSpace** objet, les appels à toutes les méthodes sous le *Windows.Storage Namespace* qui utilisent objet ou autres objets dérivés de celle-ci, ne dépendent pas de réponse à partir de services web pour terminer. Toutefois, car l’accès à la Xbox un disque dur n’est pas exclusif à l’application active, des limites supérieures stricts sur les performances de ces méthodes ne peut pas être garanties.


### <a name="connected-storage-containers-and-blobs"></a>Connecté de stockage : conteneurs et objets BLOB

Le *conteneur de stockage connectés*, ou *conteneur* pour faire court, est l’unité de stockage. Chaque espace de stockage connecté peut contenir de nombreux conteneurs, comme illustré dans le diagramme suivant.

**Figure 1.  Espace de stockage connectés (titre/machine ou par titre/utilisateur)**

![](../../images/connected_storage/connected_storage_space_containers.png) Données sont stockées dans des conteneurs comme l’une ou plusieurs mémoires tampons appelées *blobs*. Le diagramme suivant illustre la représentation interne du système des conteneurs sur le disque. Pour chaque conteneur, il existe un fichier de conteneur qui contient des références au fichier de données pour chaque objet blob dans le conteneur.

**Figure 2.  Diagramme d’un conteneur**

![](../../images/connected_storage/container_storage_blobs.png)

Pour stocker des données dans un conteneur, appelez le *ConnectedStorageContainer.SubmitUpdatesAsync méthode*, en fournissant un mappage de noms et les objets BLOB (objets de mémoire tampon). Toutes les modifications décrites dans un **SubmitUpdatesAsync** appel sont appliquées de façon atomique, autrement dit, soit tous les objets BLOB sont mis à jour comme demandé, ou l’intégralité de l’opération est terminée et le conteneur reste dans son état avant l’appel.

Des opérations qui utilisent d’enregistrement **SubmitUpdatesAsync** sont limités à 16 Mo de données à la fois.


### <a name="submitupdatesasync-behavior"></a>Comportement de SubmitUpdatesAsync

Lorsque **SubmitUpdatesAsync** est appelée, les tampons fournis à l’appel sont rapidement copiés à partir de la partition d’application dans un espace de mémoire dédiée dans la partition système. Une fois que la mémoire a été copiée dans la partition système, le Gestionnaire d’achèvement fourni dans le **SubmitUpdatesAsync** appel est effectué au sein de l’application, qui indique à l’application qu’il est sûr libérer la mémoire, elle allouée localement pour les données.

Le système enregistre les objets BLOB sur le disque dur de la console, puis termine l’opération avec une mise à jour du conteneur finale qui valide l’intégralité de l’opération sur ce conteneur.

Il existe une limite de 16 Mo sur la mémoire dans la partition partagée pour la réception **SubmitUpdatesAsync** données. Si un appel à **SubmitUpdatesAsync** ne peut pas être traité immédiatement par le système, car il n’est pas suffisamment de mémoire libre dans le tampon de 16 Mo dédié, l’appel est en file d’attente pour la maintenance. Le système transfère en continu les données à partir de la mémoire tampon de 16 Mo sur le disque dur, et les mises à jour en file d’attente sont fournies dans l’ordre qu’ils ont été demandées comme espace est disponible dans la mémoire tampon de 16 Mo.

**Figure 3.  SubmitUpdatesAsync behavior**

![](../../images/connected_storage/submitupdatesasync_behavior.png) Téléchargement vers le cloud qui se produit de manière similaire : Les objets BLOB individuels sont téléchargées vers le service et l’opération de mise à jour est validée par une mise à jour finale dans un fichier conteneur qui fait référence à tous les autres objets BLOB chargés. Dans le téléchargement dans le cloud, cette consolidation dans une mise à jour unique et dernière permet de s’assurer que toutes les données référencées dans un **SubmitUpdatesAsync** appel est soit validé dans son intégralité ou le conteneur reste inchangé. De cette façon, même si un système est mis hors connexion ou perd de la puissance pendant une opération de chargement, un utilisateur peut accéder à une autre console Xbox One, télécharger des données à partir du cloud et continuer avec une vue cohérente de tous les conteneurs.

> [!IMPORTANT]
> Dépendances de données au sein de conteneurs ne sont pas sécurisés.  Les résultats de l’individu *SubmitUpdatesAsync* appels sont garanties à appliquer entièrement ou pas du tout.

**SubmitUpdatesAsync** appels ne doivent pas considérer qu’un futur **SubmitUpdatesAsync** appel sera terminé avec succès afin de laisser le conteneur dans un état valide. En d’autres termes, les applications ne peuvent pas s’appuient sur plusieurs **SubmitUpdatesAsync** appel à enregistrer toutes les données requises dans un conteneur. Chaque **SubmitUpdatesAsync** appel doit laisser le contenu du conteneur spécifié dans un état valide pour l’application à lire plus tard.

Pour illustrer ce problème, envisagez un scénario où un conteneur effectue le suivi de la quantité de gold et produits alimentaires détenues par un caractère nommé Bob. Le titre peut stocker deux objets BLOB, nommé *food* et *or*. Bob commence par 100 gold et aucun alimentaires dans son inventaire.

**Figure 4.  Exemple de scénario : Bob commence par 100 gold.**

![](../../images/connected_storage/submitupdatesasync_example_scenario1.png)

Maintenant Bob est occupé à 50 gold. Le titre prépare un **SubmitUpdatesAsync** appeler, ce qui met à jour la valeur de l’objet blob gold à 50.

Le système capture le blob mis à jour et les informations sur la mise à jour du conteneur à la mémoire tampon de mises à jour. Le système copie ensuite la valeur de l’objet blob de nouveau sur le disque dur.

**Figure 5.  Le système capture les informations de mise à jour et copie les valeurs sur le disque dur.**

![](../../images/connected_storage/submitupdatesasync_example_scenario2.png)

Enfin, le système met à jour le fichier conteneur sur le disque dur pour référencer le nouvel objet blob. Finalement, le système supprime l’objet blob non référencé dans une opération de garbage collection.

**Figure 6.  Le système met à jour le fichier conteneur sur le disque dur et supprime l’objet blob non référencé.**

![](../../images/connected_storage/submitupdatesasync_example_scenario3.png)

Notez que les objets BLOB plus que vous utilisez par **SubmitUpdatesAsync** appel, le plus de temps est nécessaire pour terminer les opérations atomiques nécessaires des opérations de système de fichiers pour stocker les données de manière fiable. La granularité du stockage de données dans l’exemple précédent est trop petite, mais il est destiné à illustrer clairement le comportement de la mise à jour atomique de plusieurs objets BLOB dans un conteneur.


### <a name="updating-multiple-blobs--the-wrong-way"></a>La mise à jour plusieurs objets BLOB, la mauvaise façon

Imaginez un scénario dans lequel Bob souhaite acheter des produits alimentaires. Par souci de simplicité, nous indiquons que 1 unité d’or achète 1 unité de produits alimentaires, et Bob souhaite acheter 25 unités de nourriture. L’application peut émettre un **SubmitUpdatesAsync** appel visant à ajouter 25 unités de nourriture, puis un autre pour soustraire des 25 unités d’or à partir de le Bob\_conteneur de l’inventaire. Mais même si les gestionnaires terminés pour les deux **SubmitUpdatesAsync** appels ont été appelées, il est possible pour des résultats incorrects en raison d’événements comme la perte de puissance, ce qui peut arrêter les données en cours d’écriture sur le disque dur, ou un synchronisation incomplète vers le cloud. Les diagrammes suivants expliquent les étapes effectuées par le système et le résultat d’une perte d’alimentation à l’une des étapes.

Supposons que les données à partir des deux **SubmitUpdatesAsync** appels est déjà en mémoire tampon de mise à jour du système et les gestionnaires d’achèvement du titre pour les deux appels ont été appelées.

Le système écrit d’abord les données pour la nouvelle valeur de l’objet blob de produits alimentaires sur le disque.

**Figure 7.  Le système écrit la valeur de l’objet blob de produits alimentaires sur le disque.**

![](../../images/connected_storage/update_method_wrong_way_1.png) Ensuite, le système met à jour le conteneur pour faire référence à la valeur qui vient d’être écrite. Comme l’illustre le diagramme suivant, si power ont été perdue après cette étape et avant celle qui suit, Bob finiriez avec une grande, obtention de 25 produits alimentaires sans avoir le correspondante or déduit de son inventaire.

**Figure 8.  Le système met à jour le conteneur pour faire référence à la valeur qui vient d’être écrite.**

![](../../images/connected_storage/update_method_wrong_way_2.png)

Ensuite, le système écrit les données de la nouvelle valeur de l’objet blob gold sur le disque. La valeur de gold référencé par le Bob\_conteneur d’inventaire toujours n’a pas été mis à jour, et Bob a 25 gold plus qu’il le devrait, mais nous sommes rapprochez le résultat souhaité.

**Figure 9.  Le système écrit les données de la nouvelle valeur de l’objet blob gold sur le disque.**

![](../../images/connected_storage/update_method_wrong_way_3.png)

Enfin, le système met à jour le fichier conteneur pour faire référence à l’objet blob qui vient d’être écrite pour or, le résultat souhaité.

**Figure 10.  Le système met à jour le fichier conteneur pour faire référence à l’objet blob de gold qui vient d’être écrite.**

![](../../images/connected_storage/update_method_wrong_way_4.png)

### <a name="updating-multiple-blobs--the-right-way"></a>La mise à jour plusieurs objets BLOB, la bonne façon

La méthode appropriée pour vous assurer de la quantité de gold et aliments dans l’inventaire de Bob est atomiquement mis à jour, sans risque d’un état intermédiaire incorrect en raison d’une panne de courant, consiste à mettre à jour les deux objets BLOB dans un seul **SubmitUpdatesAsync** appeler. Le système sera puis procédez comme suit.

Le système écrit d’abord les données pour la nouvelle valeur de l’objet blob de produits alimentaires sur le disque.

**Figure 11.  Le système écrit les données de la nouvelle valeur de l’objet blob de produits alimentaires.**

![](../../images/connected_storage/update_method_right_way_1.png) Puis le système écrit les données de la nouvelle valeur de l’objet blob gold sur le disque.

**Figure 12.  Le système écrit les données de la nouvelle valeur de l’objet blob gold.**

![](../../images/connected_storage/update_method_right_way_2.png) Enfin, le système met à jour le fichier conteneur pour faire référence à la fois des nouveaux objets BLOB.

**Figure 13.  Le système met à jour le fichier de conteneur pour référencer les deux nouveaux objets BLOB.**

![](../../images/connected_storage/update_method_right_way_3.png) Cet exemple est très simple, il illustre l’importance d’apporter toutes les modifications apportées aux données dans un conteneur qui doit être appliqué de manière atomique en émettant un seul **SubmitUpdatesAsync** appeler avec toutes les mises à jour souhaitées. En procédant ainsi, dans le cas d’achat de produits alimentaires avec gold, l’application permet d’éviter une condition de concurrence potentielle susceptible de mettre à jour uniquement une des valeurs incorrectement et laisser le caractère trop d’or.

### <a name="performance-characteristics-and-considerations"></a>Considérations et les caractéristiques de performances

La mémoire tampon mise à jour de 16 Mo dans la partition partagée permet à un nombre limité d’opérations de mise à jour à effectuer très rapidement. La vitesse à laquelle le système peut conserver les données sur disque dépend de la quantité de données dans la mémoire tampon et le nombre d’objets BLOB. Étant donné que chaque objet blob est écrit sur le disque avec une résilience, le plus grand nombre d’objets BLOB dans la mémoire tampon, plus le temps nécessaire pour les conserver sur le disque.

Figure 13 montre un exemple pour le temps de traitement pour un **SubmitUpdatesAsync** opération toutes les 2 secondes avec les mises à jour de blob deux 512 Ko et l’autre 1024 Ko d’objets blob mise à jour, lorsqu’il n’existe aucune autre activité du disque dur sur le système. Le système peut fonctionner à un état stable, traitement de chaque mise à jour au sein de 14 – 18ms.

**Figure 14.  Le temps de traitement pour une opération SubmitUpdatesAsync toutes les 2 secondes avec les mises à jour de blob deux 512 Ko et l’autre 1024 Ko d’objets blob mise à jour et aucune autre activité du disque dur.**

![](../../images/connected_storage/submitupdatesasync_proc_time_mixed_size_fixed_interval.png) La figure 14 illustre le temps de traitement pour trois objets BLOB de 1 024 Ko à différents intervalles de temps.

Le système peut traiter ces mises à jour à intervalles de 3 secondes à l’état stable de 87ms. Augmenter la fréquence à toutes les 2 secondes, le système peut toujours traiter les mises à jour dans un état stable 87ms.

Ce qui réduit l’intervalle sur 1 seconde entre les mises à jour modifie le comportement d’un état stable. Le système peut traiter les mises à jour de 60 à 87ms par mise à jour, mais chaque mise à jour au-delà qui dure plus longtemps, atteindre un temps de traitement de l’état stable de 500 millisecondes par seconde de mise à jour, avec les fluctuations importantes. Il s’agit, car la mémoire tampon de 16 Mo est remplie plus rapidement qu’il peut vider les données sur le disque ; mises à jour sont forcés d’attendre pour les mises à jour précédentes à écrire.

L’effet augmente considérablement lorsque la mise à jour de l’intervalle à une mise à jour toutes les 0,5 secondes. Le système peut traiter à nouveau les mises à jour seulement 7 à cet intervalle, à 87ms par mise à jour, avant d’atteindre un état stable dans lequel chaque mise à jour prend plus de 1 seconde à traiter, avec des variations très élevées.

**Figure 15.  Le temps de traitement de trois objets BLOB de 1 024 Ko à différents intervalles de temps.**

![](../../images/connected_storage/submitupdatesasync_proc_time_fixed_size_various_intervals.png) Il s’agit d’exemples uniquement. Votre application générale ne doit pas être l’enregistrement des données cela souvent, mais il ne sont pas généralement être fonctionnant également dans un environnement gratuit d’e/s disque.

Il est important de comprendre les caractéristiques du système en fonction de ces exemples, pour mesurer l’application sous différentes conditions d’exploitation, de s’assurer que votre enregistrement opérations peuvent se terminer en moins de 1 seconde au cours de votre application de suspendre gestionnaire.


## <a name="synchronizing-a-connected-storage-space"></a>Synchronisation d’un espace de stockage connectés

-   Vérification de la connectivité
-   Acquisition de verrou
-   Logique de liste, de comparaison et fusion de conteneur
-   Téléchargement de conteneur

Lorsque votre application demande l’accès à un espace de stockage connecté, le système exécute un processus de synchronisation pour conserver les données enregistrées de l’utilisateur dans un état cohérent entre les consoles Xbox One et rendre ses données disponibles pour une lecture hors connexion. Étant donné que la synchronisation peut prendre différentes quantités de temps et peut nécessiter que l’utilisateur de prendre des décisions, le système peut afficher l’interface utilisateur à l’utilisateur à différents stades du processus.

L’utilisateur peut accéder à partir de votre application en appuyant sur le bouton de la Xbox à tout moment, même si la synchronisation de l’interface utilisateur est actif. Le système masque l’interface utilisateur, et la synchronisation continue autant que possible sans intervention de l’utilisateur. Lorsque l’utilisateur navigue vers l’application, l’interface utilisateur s’affiche à nouveau, sauf si la synchronisation est terminée. Le système ne lance jamais une hypothèse sur la sélection d’un utilisateur lors de l’interface utilisateur est masquée.

Étant donné que le système n’affiche aucune interface utilisateur de la synchronisation lorsque l’utilisateur est à l’écran d’accueil, et rendu de votre application est toujours visible dans la vignette de l’application Big, il est important que l’application restituer les visuels appropriés en fonction du contexte tout en un **GetForUserAsync** appel se termine. Le rendu continu indique à l’utilisateur que l’application est toujours interactive et attend des données à charger.

Le diagramme suivant présente la séquence de haut niveau que le système suit lorsqu’une application demande un espace de stockage connectés. Si la séquence entière prend plus de quelques secondes, la synchronisation system-drawn l’interface utilisateur s’affiche.

**Figure 16.  Séquence suivi par le système lorsqu’une application demande l’espace de stockage connectés.**

![](../../images/connected_storage/app_requests_connected_storage_space.png) Le système passe par les étapes suivantes lorsqu’il traite un **GetForUserAsync** demande :

-   Vérification de la connectivité
-   Acquisition de verrou
-   Logique de liste, de comparaison et fusion de conteneur
-   Téléchargement de conteneur

### <a name="connectivity-check"></a>Vérification de la connectivité

Pour commencer à traiter un **GetForUserAsync** demande, le système vérifie pour la connectivité. Si la console est hors connexion, le processus de synchronisation entier est ignoré et l’espace de stockage connecté pour l’utilisateur spécifié est marqué comme étant hors connexion pour la session active. Toutes les données modifiées sera rapprochées avec le stockage cloud lors de la prochaine fois que votre application accède à l’espace de stockage connectés du même utilisateur et le système peut atteindre le service de stockage de titre. Aucune interface utilisateur n’est jamais affichée pour ce cas.

Pour plus d’informations sur la gestion hors connexion en dehors du contexte de stockage connecté, consultez *résilience d’Interruption de Service pour les titres une Xbox*.

### <a name="lock-acquisition"></a>Acquisition de verrou

Après avoir vérifié la connectivité, le système tente d’acquérir un accès exclusif à l’espace de stockage cloud associée à votre application et l’utilisateur actuel. Pour cela, vous devez placer un fichier de verrouillage dans la zone de stockage connectés de votre stockage de titre. Si la console est en ligne peut atteindre le service et est en mesure d’acquérir le verrou sur une courte période de temps, aucune interface utilisateur n’est présentée et continue le processus de synchronisation.

Une fois que le système a acquis un verrou pour un espace de stockage connectés particulier et a retourné une instance d’un espace de stockage connectés à votre application, aucune des API de votre application appelle exploitant des données au sein d’espace de stockage connectés bloque sur les requêtes web réussie. Le verrou fournit une protection suffisante, afin que même si un utilisateur à débrancher le câble réseau à partir du système une fois que votre application a acquis un espace de stockage connectés, les appels d’API fonctionnent selon les données disponibles localement.

Il existe quelques scénarios d’erreur possibles lors de l’étape d’acquisition de verrou :

 **La synchronisation de l’interface utilisateur** si la console est en ligne mais n’a pas acquis le verrou à partir du service au sein d’une courte période, une interface utilisateur « synchronisation » s’affiche.

 **Avec rupture du verrou** si l’utilisateur a lu l’application sur une autre console dans la mesure où il ou elle lu en dernier sur l’objet actuel, il est possible que la console autres a un accès exclusif à l’espace de stockage et est en cours de chargement des données. Il est également possible qu’une autre console a démarré le chargement des données mais qu’il a perdu sa connexion ou l’alimentation avant la fin.

Ces deux cas sont appelés *contention de verrouillage*, et dans les deux cas, le système présente l’interface utilisateur pour expliquer qu’une autre console chargement des données. L’utilisateur peut attendre de ce processus se termine ou travailler avec les données actuellement disponibles dans le cloud. Si l’utilisateur choisit d’utiliser les données de cloud, le système qui acquiert le verrou elle-même (il arrête le verrou), l’acquisition d’un accès exclusif au stockage cloud pour l’utilisateur et l’application. Le chargement à partir de la console autres est annulé et le processus de synchronisation continue.

### <a name="container-listing-comparison-and-merger-logic"></a>Logique de liste, de comparaison et fusion de conteneur

Après avoir acquis un verrou, le système demande une liste de tous les conteneurs dans le cloud pour l’application donnée et l’utilisateur. Ensuite, il compare le contenu du disque dur local avec les données dans le cloud et s’effectue d’après les résultats de la comparaison :

 **Données locales correspond à la cloud** si aucune modification à partir d’autres consoles n’ont été, et les données dans le cloud et local durs lecteur est identique, la synchronisation est terminée, au gestionnaire d’achèvement de la **GetForUserAsync**est appelé pour l’instant, et votre application de poursuivre les charges et les enregistre.

 **Aucune donnée locale** si le cloud a des données, mais n’en contienne la console locale, les données à partir du cloud sont téléchargées localement. Cela peut se produire, par exemple, lorsque l’utilisateur est lue à un ami pour la première fois.

 **Mêmes conteneurs, modifiés localement et dans le cloud** si l’utilisateur a modifié des conteneurs dans le cloud en lisant sur une autre console et a modifié les mêmes conteneurs lors de l’utilisation de la console actuelle en mode hors connexion, les données ne peuvent pas être fusionnées automatiquement. L’utilisateur sera invité à choisir les données à conserver. En cas de conflits, l’utilisateur peut choisir une stratégie de remplacement : Les données locales ou les données de cloud sont toujours conservées, soit l’utilisateur peut sélectionner **Annuler** et différer faire votre choix. Si l’utilisateur choisit de cloud ou des données locales en tant qu’une stratégie de remplacement, les conteneurs avec le même nom, mais avec un contenu différent : sera résolu en conséquence.

Si l’utilisateur sélectionne **Annuler**, le titre auront accès à l’enregistrement système dans un état non résolu, comme si l’utilisateur est en lecture hors connexion. Dans ce cas, l’interface utilisateur de la résolution de conflit est présentée à la prochaine fois que l’application demande l’accès à l’espace de stockage connecté, si la console est en ligne.

### <a name="container-download"></a>Téléchargement de conteneur

Une fois que tous les conflits ont été résolus, le système a toutes les informations nécessaires pour identifier les conteneurs qui doivent être téléchargés à partir du cloud. Tous les conteneurs nécessaires sont téléchargés, le Gestionnaire d’achèvement de la *ConnectedStorageSpace.GetForUserAsync méthode* sera appelé pour l’instant, et que votre application de poursuivre les charges et les enregistre.

Certaines erreurs possibles au cours de cette étape :

**Stockage local insuffisant**  
Dans le cas d’espace insuffisant de disque dur local pour les conteneurs requis, les utilisateurs sont présentées avec interface utilisateur lui demander d’espace disque en supprimant les données localement enregistrées. Pour les aider à éviter de supprimer définitivement les données importantes qui ne sont pas sauvegardées dans le cloud, l’interface utilisateur indique clairement les données du cache local simplement et données qui est uniques dans la console active.

Lorsque l’interface utilisateur est présentée à l’utilisateur :

-   Si l’utilisateur permet de libérer suffisamment d’espace, la synchronisation continue et se termine.
-   Si l’utilisateur quitte l’interface utilisateur sans la libérer suffisamment d’espace, le Gestionnaire d’achèvement de la **GetForUserAsync** appeler retourne **OutOfLocalStorage**— consultez *ConnectedStorageErrorStatus Énumération*. L’application doit confirmer que l’utilisateur a l’intention de lire sans être en mesure d’enregistrer des données. Si l’utilisateur accepte, l’application doit continuer sans enregistrer les données pour cet utilisateur. Si l’utilisateur indique qu’il souhaite enregistrer des données en lecture, l’application doit se répéter le **GetForUserAsync** appeler, puis pour afficher l’interface utilisateur pour libérer de l’espace.

**Utilisateur annule la synchronisation**  
Si l’utilisateur ne souhaite pas attendre la fin de synchronisation, et annuler les instructions SELECT, l’utilisateur est informé que toutes les données enregistrées seront disponibles. Le Gestionnaire d’achèvement de la **GetForUserAsync** appel est effectué pour l’instant, et l’application de poursuivre les charges et les enregistre.

**Délai d’expiration réseau**  
Si les données téléchargement arrive à expiration en raison d’un problème de connectivité réseau ou de disponibilité du service, l’utilisateur est proposé pour relancer la synchronisation. Si elle choisit de ne pas, l’utilisateur est informé que toutes les données enregistrées seront disponibles. Le Gestionnaire d’achèvement de la **GetForUserAsync** appel est effectué pour l’instant, et l’application de poursuivre les charges et les enregistre.

## <a name="development-tools"></a>Outils de développement

Deux outils vous aideront avec le développement d’utilisation de votre application de stockage connecté : XbStorage et Fiddler.

### <a name="managing-connected-storage-with-xbstorage"></a>Gestion du stockage connecté avec XbStorage

XbStorage est un outil de développement qui vous permet de gérer les données de stockage connectés local sur un kit de développement Xbox One à partir d’un PC de développement.

L’outil permet l’effacement d’espaces de stockage connectés local depuis le disque dur, ainsi que l’importation et exportation des espaces de stockage individuels utilisateur - ou ordinateur-connecté à l’aide de fichiers XML.

Lorsqu’une opération est effectuée sur un espace de stockage connectés local, le système se comporte comme si cette opération a été effectuée par l’application elle-même. Copie des données à partir d’un espace de stockage connecté à un fichier local provoque la synchronisation avec le cloud avant de copier.

De même, la copie des données à partir d’un fichier XML sur le PC de développement vers un conteneur de stockage connectés sur le kit de développement Xbox One, la console démarrer le chargement de ces données vers le cloud. Il existe une exception : si le kit de développement ne peut pas acquérir le verrou, ou s’il existe un conflit entre les conteneurs sur la console et celles figurant dans le cloud. Dans ce cas, la console se comporte comme si l’utilisateur avait décidé de ne pas résoudre le conflit-par exemple, en sélectionnant une version du conteneur à conserver- et la console se comporte comme si elle est en lecture hors connexion jusqu'à ce que la prochaine fois que le titre est démarré.

Commande de réinitialisation du XbStorage efface le stockage local de 'SCIDs et des utilisateurs toutes les données enregistrées, mais ne modifie pas les données stockées dans le cloud. Cela est utile pour la définition d’une console à l’état, qu'il serait dans si un utilisateur ont été itinérance à une console et télécharger des données à partir du cloud lors de la lecture d’un titre.

Pour plus d’informations sur XbStorage, consultez *gérer le stockage connecté (xbstorage.exe)*, dans la documentation XDK.

### <a name="monitoring-connected-storage-network-activity-using-fiddler"></a>Surveillance de l’activité réseau de stockage connectés à l’aide de Fiddler

Il peut être utile déterminer si votre console interagit avec le service lorsque les opérations de stockage cloud sont effectuées. À l’aide de Fiddler peut aider à déterminer si votre console effectue des appels au service avec succès ou si elle rencontre des erreurs d’autorisation. Pour plus d’informations sur la configuration de Fiddler sur une Xbox One, consultez *comment utiliser Fiddler avec Xbox One*, dans cette documentation XDK.

## <a name="resources"></a>Ressources

En plus des ressources suggérés ci-dessus, les éléments suivants peuvent être utiles dans le développement de votre application ou le titre :

-   Vue d’ensemble de stockage connecté, dans la documentation XDK
-   [Gestion de la durée de vie des processus](https://developer.xboxlive.com/_layouts/xna/download.ashx?file=ProcessLifetimeManagement_08_2013_qfe5.zip&folder=platform/aug2013xdk_qfe5/samples), un exemple disponible à partir d’exemples sur Game Developer Network (GDN)
-   [« Processus de gestion de durée de vie (PLM) pour Xbox One »](https://developer.xboxlive.com/en-us/platform/development/education/Documents/PLM%20for%20Xbox%20One.aspx), un livre blanc disponible à partir des livres blancs sur GDN
