---
title: Vue d’ensemble de stockage connectés
description: En savoir plus sur l’utilisation de stockage connecté pour enregistrer et charger des données de jeu sur des appareils.
ms.assetid: a0bacf59-120a-4ffc-85e1-fbeec5db1308
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, stockage connecté
ms.localizationpriority: medium
ms.openlocfilehash: 40ad13e46e074154d72d7aad236747c3374110ef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639054"
---
# <a name="connected-storage"></a>Stockage connecté
Stockage connecté est conçu pour autoriser votre titre enregistrer les données de jeu et autres données d’état concerné doivent se déplacer entre les appareils. L’API de stockage connecté permet des titres sur Xbox One et Universal Windows Platform(UWP) pour enregistrer, charger et supprimer les données stockées localement et également synchronisées avec le cloud dès que le titre de la Xbox One ou UWP est connecté à internet. Données enregistrées seront disponibles sur n’importe quel autre appareil qui exécute votre titre après que la synchronisation se produit. Les développeurs sont invités à enregistrer aussi précisément que possible pour offrir que la meilleure pas chez lire l’expérience des état de titre. Stockage connecté est ce qui vous permet de progression dans un jeu à la maison, puis reprendre votre jeu là où vous en étiez sur n’importe quel autre appareil qui prend en charge le même jeu.

## <a name="connected-storage-features"></a>Fonctionnalités de stockage connecté

L’API de stockage connecté fournit les fonctionnalités suivantes :

- Les applications peuvent enregistrer rapidement jusqu'à 16 Mo de données à la fois dans la mémoire tampon, qui est ensuite mis en cache localement sur le disque dur par le système et téléchargé vers le cloud.
- Pour les partenaires gérés et ID@Xbox les développeurs :
    - 256 Mo par utilisateur/application de stockage cloud.
- Pour les développeurs de programme Xbox Live Creators :
    - 64 Mo par utilisateur/application de stockage cloud.
- Réponse robuste aux pannes d’alimentation, les applications n’êtes pas obligé de gérer des données partielles en cours d’enregistrement.
- Données sont automatiquement téléchargées vers le cloud, même lorsque l’application n’est pas en cours d’exécution.
- Données seront disponibles sur Xbox One ou UWP appareils qui sont connectés à la Xbox Live.
- Xbox Live de gère la gestion de la synchronisation et de conflit entre les périphériques sans nécessiter une intervention par l’application.

Le système de stockage connecté permet de s’assurer que toutes les sauvegardes sont effectuées dans leur intégralité ou pas du tout. Cela signifie qu’en tant que développeur jamais avoir à vous soucier d’affecter votre état en cas de panne d’alimentation ou de l’utilisateur de fermer l’application soudainement, du jeu de données partiellement enregistrées manuellement ou en ouvrant une autre application/jeu sur la console. Cela garantit également un jeu plus lisse joue expérience pour les utilisateurs de votre titre, comme le stockage connecté est une partie importante de ce qui permet aux utilisateurs de basculer entre les jeux et applications rapidement et gratuitement, mais toujours reviennent au jeu d’origine dans l’état dans lequel ils conservés. Pour implémenter ces fonctionnalités dans votre propre titre, vous devez comprendre les API de stockage connectés.

## <a name="connected-storage-structure"></a>Structure de stockage connecté

Le système de stockage connecté permet aux applications de stocker des données sous la forme d’un ou plusieurs objets BLOB des conteneurs. À un niveau élevé, toutes les données dans le système de stockage connecté est associé à un utilisateur ou un utilisateur ou un ordinateur dans le cas du Kit de développement Xbox développé des titres. Toutes les données enregistrées par une application pour un utilisateur particulier ou d’ordinateur est stocké dans un espace de stockage connectés. Chaque utilisateur de votre application obtient un espace de stockage connectés avec une limite de stockage total de 256 ou 64 Mo. Il est important de noter que ce stockage est dédié à votre application autonome, il n’est pas partagé avec d’autres applications.

### <a name="containers-and-blobs"></a>Conteneurs et objets BLOB

Le conteneur de stockage connecté, ou un conteneur pour faire court, est l’unité de stockage. Chaque espace de stockage connecté peut contenir de nombreux conteneurs, comme illustré dans le diagramme suivant.

Espace de stockage connecté (titre/machine ou par titre/utilisateur)

![connected_storage_space_containers.png](../../images/connected_storage/connected_storage_space_containers.png)

 En tant qu’un ou plusieurs tampons appelés des objets BLOB, les données sont stockées dans des conteneurs. Le diagramme suivant illustre la représentation interne du système des conteneurs sur le disque. Pour chaque conteneur, il existe un fichier de conteneur qui contient des références au fichier de données pour chaque objet blob dans le conteneur.

Diagramme d’un conteneur

![container_storage_blobs.png](../../images/connected_storage/container_storage_blobs.png)

Pour stocker des données dans un conteneur, appelez la méthode de conteneur API appropriée SubmitUpdatesAsync, en fournissant un mappage de noms et les objets BLOB (objets de mémoire tampon). Toutes les modifications décrites dans un appel SubmitUpdatesAsync sont appliquées de façon atomique, autrement dit, soit tous les objets BLOB sont mis à jour comme demandé, ou l’intégralité de l’opération est terminée et le conteneur reste dans son état avant l’appel.

Personne les opérations qui utilisent SubmitUpdatesAsync d’enregistrement sont limités à 16 Mo de données à la fois.

## <a name="connected-storage-api"></a>API de stockage connecté

Stockage connecté a API distincte pour les applications XDK et UWP. Heureusement, ces API ressemblent à eux très étroitement. Les deux API diffèrent principalement dans leurs noms d’espace de noms et classe. Les fonctions requises pour [enregistrer](connected-storage-saving.md), [charger](connected-storage-loading.md), et [supprimer](connected-storage-deleting.md) données avec l’API sont nommées de manière identique.

Autres différences entre les deux API de stockage connectés sont détaillées dans la section de stockage connecté de [code portage Xbox Live à partir du XDK vers UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md).

Vous pouvez trouver que les API de stockage connecté XDK documentées dans le fichier .chm XDK sous le chemin d’accès : **Xbox un XDK >> référence de l’API >> référence des API de plateforme >> référence de l’API système >> Windows.Xbox.Storage**.
Les API XDK sont également documentées sur le [developer.microsoft.com site](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n).
Le lien vers les API XDK nécessite que vous avez un Account(MSA) Microsoft qui a été activée pour un accès Kit(XDK) de développeur Xbox.
Windows.Xbox.Storage est le nom de l’espace de noms de stockage connecté pour les consoles Xbox One.

Vous pouvez trouver les API UWP connecté stockage documentés dans le fichier .chm de Xbox Live SDK sous le chemin d’accès : **Les API Live Xbox >> Xbox Live plateforme référence de l’API SDK Extensions >> Windows.Gaming.XboxLive.Storage**.
Les API de stockage connecté UWP sont également documentées en ligne sous la [référence d’espace de noms Windows.Gaming.XboxLive.Storage](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage).
Windows.Gaming.XboxLive.Storage est le nom de l’espace de noms de stockage connecté pour les applications UWP.

Pour commencer à utiliser le stockage connecté, vous devez acquérir un stockage connecté *espace*. Un espace de stockage connecté est associé à un utilisateur ou un ordinateur et qu’il contient toutes les données de stockage connecté associées à cet utilisateur ou un ordinateur sous la forme de *conteneurs* et *blobs*. L’acquisition d’un espace de stockage connecté à un ordinateur ou un utilisateur vous donnera accès en lecture et écriture à ces données d’entités stockées. Pour acquérir un espace de stockage connectés, les titres XDK et UWP appellera le `GetForUserAsync` (méthode), les titres XDK peuvent également appeler le `GetForMachineAsync` (méthode), les titres UWP ne pourront appeler `GetForMachineAsync`. `GetForUserAsync` et `GetForMachineAsync` sont contenus dans le `ConnectedStorageSpace` classe dans le XDK. `GetForUserAsync` est contenue dans le `GameSaveProvider` classe dans l’API UWP. Ces méthodes sont des opérations potentiellement longue, en particulier si l’utilisateur a enregistré des données sur un périphérique et reprend le jeu pour la première fois sur un autre appareil. `GetForUserAsync` Récupère un espace de stockage connecté pour un utilisateur dont vous pouvez ensuite utiliser pour créer, accéder et supprimer des conteneurs.

Pour créer un conteneur, ou accéder à un conteneur créé précédemment, appelez le `CreateContainer` fonction de votre `ConnectedStorageSpace` ou `GameSaveProvider` (classe), cela vous donnera accès à un conteneur nommé pour l’utilisateur ou associé à la machine avec le `ConnectedStorageSpace` ou `GameSaveProvider`utilisé pour créer le conteneur. Le `ConnectedStorageSpace` et `GameSaveProvider` classes incluent également la `DeleteContainerAsync` fonction qui vous permet de supprimer un conteneur fourni vous donner le nom du conteneur à supprimer.

Pour mettre à jour des objets BLOB dans votre appel de conteneur `SubmitUpdatesAsync` dans le `ConnectedStorageContainer` classe dans le XDK ou dans le `GameSaveContainer` classe de l’API UWP. `SubmitUpdatesAsync` vous permet de fournir une liste de noms et les mémoires tampons sous forme de données à écrire dans l’objet blob, une liste de noms d’objets BLOB à supprimer un nom d’affichage pour l’appel de sauvegarde et conteneur. Il s’agit de la fonction dont vous aurez finalement besoin d’appeler pour mettre à jour des données dans votre espace de stockage connecté.

Pour voir des exemples de l’API de stockage connectés en cours d’utilisation, consultez les articles suivants de stockage connecté : [Enregistrer les données](connected-storage-saving.md)
[charger des données](connected-storage-loading.md)
[supprimer des données](connected-storage-deleting.md)

> [!NOTE]
> Remarque sur la sécurité :
>
> Les applications universelles Windows Platform (UWP) en cours d’exécution sur les PC enregistrement des données locales dans un emplacement accessible à l’utilisateur local, et n’est pas intrinsèquement protégé contre la falsification de l’utilisateur.
>Vous devez envisager d’appliquer votre propre chiffrement et validité vérifie au jeu d’enregistrement des données afin d’empêcher les utilisateurs de modifier le jeu enregistre en dehors du jeu.
>Pour cette raison même, vous devez décider si vous souhaitez autoriser les versions de PC et Xbox de votre jeu pour partager des enregistrements ou les séparer.

## <a name="managing-local-storage"></a>Gestion du stockage local

Lorsque vous testez le stockage connecté sur votre application de Kit de développement Xbox ou UWP, vous devrez peut-être manipuler les données stockées localement sur votre appareil de développement.

L’outil xbstorage est fourni avec le XDK et est un outil de ligne de commande qui vous permet de manipuler le stockage local sur votre console de développement.
Pour les développeurs UWP, il existe un outil identiques pour PC appelé gamesaveutil.exe qui est fourni avec le SDK Windows 10 pour Fall Creators Update(10.0.16299.91) et versions ultérieures.

Deux de ces outils vous permettent de manipuler le stockage local sur votre appareil avec les commandes suivantes :

|Commande  |Description  |
|---------|---------|
|Réinitialiser    | Effectue une réinitialisation sur le stockage connecté des paramètres. |
|importer   | Importer des données à partir du fichier XML spécifié dans un espace de stockage connectés. |
|Exportation   | Exporte les données à partir d’un espace de stockage connecté dans le fichier XML spécifié. |
|supprimer   | Supprime les données à partir d’un espace de stockage connectés. |
|générer | Génère des données fictives et l’enregistre dans le fichier XML spécifié. |
|Simuler | Simule les conditions d’espace de stockage insuffisant. |

Pour en savoir plus sur les fonctions disponibles dans l’outil de xbstorage et les gamesaveutils.exe lire [la gestion de stockage connecté local](connected-storage-xb-storage.md).

## <a name="technical-overview"></a>Vue d’ensemble technique

Pour tirer le plus en profondeur et examiner comment fonctionne le stockage connecté sur le XDK lire le [présentation technique de stockage connecté](connected-storage-technical-overview.md). La vue d’ensemble technique a été écrit spécifiquement pour les développeurs XDK mais les concepts, contenus dans pour stockage connecté en général.