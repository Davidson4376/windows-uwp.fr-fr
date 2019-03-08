---
title: Connecté les bonnes pratiques de stockage
description: Recommandations sur la façon d’obtenir les meilleures performances et l’expérience à partir du stockage connecté
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox un, le stockage connecté
ms.localizationpriority: medium
ms.openlocfilehash: dbdb8f95e9afc44a3280d897e74f74d327f94b97
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632924"
---
# <a name="connected-storage-best-practices"></a>Meilleures pratiques relatives à Stockage connecté

Les développeurs doivent diviser enregistrez les données en regroupements logiques qui sont modifiables indépendamment au lieu d’écriture enregistre monolithique. Cela permet des titres réduire la quantité de données, qu'ils écrivent dans diverses situations, ce qui réduit les deux la consommation de ressources locales et téléchargez l’utilisation de la bande passante. L’API permet également de titres mettre à jour plusieurs éléments de données dans une opération atomique, ce qui garantit réussissent entièrement ou pas en vigueur tout (par exemple dans le cas d’enregistrer en milieu de la défaillance catastrophique).

Étant donné que Xbox One permet aux utilisateurs de basculer rapidement entre les titres, les développeurs doivent concevoir leur titre à conserver leur état actuel prêt à enregistrer dans un délai en prévision de la réception d’un événement d’interruption, ce qui peut se produire pratiquement à tout moment. L’API de stockage connecté utilise RAM en dehors de la réservation de titre comme premier point de stockage afin d’optimiser la vitesse d’écriture titre pendant un court suspend la fenêtre de temps. Le système conserve les données dans un stockage durable, rapproche avec toutes les écritures de données depuis le dernier téléchargement, puis planifie les transferts de données. Une fois stockés et en file d’attente pour le chargement, le système est robuste à différentes défaillances telles que problème de perte ou d’alimentation de la connectivité réseau.

## <a name="when-to-load-a-users-connected-storage-space-data"></a>Lorsque charger des données d’espace d’un utilisateur connecté de stockage

La durée d’exécution pour le chargement d’espace de données d’un utilisateur connecté de stockage peut varier. Les applications doivent tirer cette action au cours d’exécution principal, plutôt qu’en réponse à l’utilisateur commence à se déconnecter ou en réponse à la réception d’une notification d’interruption à partir du système.
En règle générale, les applications doivent charger un espace de stockage connecté comme un utilisateur se connecte et indique le désir de jouer, sauf si le jeu est dans un mode dans lequel il est certain aucune fonctionnalité d’enregistrement est nécessaire. Vous devez également envisager d’alignement le chargement d’un espace de stockage connectés avec longues séquences de chargement de données, afin que les opérations peuvent s’exécuter en parallèle.
Une fois que votre application a chargé les données de l’espace d’un utilisateur connecté de stockage, il doit conserver cette valeur pour les sauvegardes ultérieures. En conservant un espace de stockage connecté au fil du temps n’a pas d’effets négatifs sur les performances ou de robustesse. Étant donné que le chargement d’un espace de stockage connecté d’entraîne une vérification de la synchronisation avec le cloud si le système est en ligne, libération et du rechargement connecté espace d’un utilisateur pendant lente ou conditions réseau non fiables peuvent entraîner l’utilisateur d’afficher une interface utilisateur de la synchronisation jusqu'à ce que le système arrive à expiration. Connecté stockage espaces n’avez pas besoin d’être libérées explicitement pour entrainer une synchronisation de cloud. Une fois le Gestionnaire d’achèvement spécifié dans un `SubmitUpdatesAsync` appel a retourné avec `AsyncStatus::Completed`, le système prend en charge de la synchronisation avec le cloud ou non l’application libère le `ConnectedStorageSpace` objet.

## <a name="when-to-save"></a>Quand l’enregistrer

Chaque fois qu’une application reçoit une notification d’interruption, l’application doit enregistrer au moins les données pertinentes, l’activation du système revenir à un état en fonction du contexte approprié pour l’utilisateur.

Si votre conception de jeux utilise périodiques, automatique ou initiée par l’utilisateur enregistre, connecté de stockage peut être appelée plus souvent que lors de la réception d’une notification d’interruption ; Cette opération est donc un bon moyen de réduire le risque de perte de données en raison d’une panne de courant ou une panne.
Si vous développez votre jeu avec le XDK, lorsqu’un utilisateur se déconnecte, l’objet utilisateur pour cet utilisateur reste valide, et à ce moment-là, l’application peut exécuter finale opérations de sauvegarde en utilisant le stockage connecté.

## <a name="robustness"></a>Robustesse

Étant donné que les données enregistrées sont toujours synchronisées avec le cloud, un bogue dans les données enregistrées et le code d’application qui provoque le blocage de l’application peut être sauvegardé dans le cloud et répartie sur les appareils. Pour empêcher les utilisateurs d’une application se bloque lors de chaque lancement, concevoir l’application pour vous assurer que :

-   Les utilisateurs peuvent accéder à un point dans l’application à laquelle ils peuvent gérer un état enregistré, même si certaines données enregistrées sont incorrecte.
-   L’application peut traiter les données endommagées automatiquement, récupérer autant de données que possible et de réinitialisation de tout le reste dans un état sécurisé.

## <a name="use-cases-for-save-game-designs"></a>Cas d’utilisation de conceptions de jeu de sauvegarde

La conception d’un jeu de l’enregistrement système qui rend le meilleur parti de conteneurs d’espace de stockage connecté varie selon le type d’application :

### <a name="single-save"></a>Enregistrement unique

Pour les applications qui utilisent un seul, style de campagne enregistrer système, par exemple une première shooter personne :

-   Placez toutes les données dans un seul conteneur et toujours écrire dans le même conteneur, identifié par nom.
-   Vous pouvez exposer une option pour réinitialiser toutes les données qui effacera toutes les données enregistrées d’un utilisateur, au cas où il souhaite commencer à lire l’application depuis le début, avec aucune progression précédente étant conservée.

### <a name="multiple-saves"></a>Enregistre plusieurs

Pour les applications qui ont un nombre fixe d’enregistrer les emplacements, nous allons dire que cinq, il existe deux façons d’utiliser des conteneurs pour enregistrer les données de jeu :

-   Store toutes les 5 emplacements au sein d’un conteneur de nom fixe à l’aide d’objets 1 blob pour chaque emplacement de sauvegarde. À l’aide de cette méthode, toutes les 5 emplacements seront entièrement synchronisées et disponibles, ou, dans le cas où la synchronisation échoue à tout moment, aucun des connecteurs seront synchronisés et restent dans leur état précédent. Si un utilisateur lit l’application en mode hors connexion sur les deux consoles différentes, l’enregistrement des progrès dans emplacement 1 sur la première console et dans l’emplacement 2 sur la deuxième console, l’utilisateur doit choisir les données à conserver sur la connexion de deux consoles Xbox Live ; la logique de fusion pour les conteneurs produira un conflit.
-   Store chaque emplacement dans un conteneur avec son propre nom. Cela permet la progression indépendante dans chaque emplacement, même sur plusieurs ordinateurs pouvant être hors connexion. Toutefois, si un utilisateur annule en cours via une synchronisation, il est possible que seuls certains des emplacements seront disponibles au cours de cette session ; certains des conteneurs ne peuvent pas avoir terminé le téléchargement. Dans ce cas, l’utilisateur est averti que la synchronisation est incomplète, et que certaines des données cloud n’est pas sur la console locale.

L’approche est utilisée, l’application doit fournir l’utilisateur avec l’interface utilisateur pour supprimer des enregistrements individuels à partir d’emplacements.

### <a name="warning"></a>Avertissement

**Ne stockez pas de données dépendantes sur conteneurs.** Ne stockez pas de données avec des dépendances entre plusieurs conteneurs. Les conteneurs peuvent être séparés en raison d’une synchronisation incomplète, panne de courant ou autres conditions d’erreur. Données stockées dans un seul conteneur doivent être autonome et cohérent.

### <a name="tips"></a>Conseils

**Pas décourager les utilisateurs de la désactivation de la console ou que vous quittez cette page.** Votre titre ne devrait pas décourager les utilisateurs de la désactivation de la console ou de vous obliger à quitter votre application lors de l’enregistrement. Sur la Xbox 360, si un utilisateur désactive le système pendant l’enregistrement de votre titre, les données de l’utilisateur ne sont pas enregistrées. Sur Xbox One, votre titre reçoit un événement d’interruption et a 1 seconde à utiliser l’API de stockage connecté pour enregistrer l’état. Le système garantit que les données sont correctement validées sur le disque dur avant qu’il s’arrête complètement ou passe à son état de faible puissance. Le même processus d’interruption se produit si l’utilisateur éjecte le disque de votre titre pour lire un autre.

**Conserver les espaces de stockage connectés.** Conserve les objets de ConnectedStorageSpace plutôt que d’essayer de les charger à chaque fois qu’un événement de lecture ou d’écriture se produit. Il n’existe aucun impact négatif sur les performances ou de robustesse dû en conservant un objet ConnectedStorageSpace pendant une période prolongée.

**Limitez les tailles de données.** Minimiser la taille des données enregistrées. Toutes les données utilisateur dans le stockage connecté est téléchargée vers le cloud lorsque la console est en ligne. Optimiser vos formats de données pour garantir des retards minimale et l’utilisation de la bande passante.

**Vérifiez que les utilisateurs moquez ne pas l’enregistrement.** Consultez les erreurs de OutOfLocalStorage retournées à partir de GetForUserAsync et SubmitUpdatesAsync et utilisateurs de la requête pour voir s’ils souhaitent vraiment lire sans l’enregistrer. Si un utilisateur indique souhaite enregistrer des jeux, recommencez l’opération.

**Vérifier le quota et les invite à supprimer de l’espace de l’utilisateur.** Recherchez une erreur QuotaExceeded retournée par SubmitUpdatesAsync. Si votre application reçoit ce message, informez les utilisateurs qu’ils ne peuvent pas enregistrer toutes les données plus jusqu'à ce qu’ils ont libéré de l’espace et de les présentent avec l’interface utilisateur qui leur permet de faire. Chaque utilisateur obtienne 256 ou 64 Mo de données par application, et chaque titre XDK obtient 64 Mo de stockage par machine qui est local à la console.

**Enregistrer l’état de menus pour la restauration ultérieurement.** Enregistrer l’état de menu et autres paramètres de l’application en plus de l’enregistrement des données de jeu de base. Si l’utilisateur joue une autre application, puis revient à la vôtre, restaurez-les à un état de menu approprié en fonction du contexte.
Répondre aux modifications de l’utilisateur connecté. Les utilisateurs peuvent se déconnecter quand votre application est suspendue. Lors de la reprise de l’application, il doit déterminer si l’ensemble des utilisateurs connectés a changé. Prendre en compte la navigation vers un emplacement approprié dans l’application, tel qu’un menu, lorsque cela se produit.

**Fournit l’interface utilisateur pour la gestion des données enregistrées.** Votre application doit fournir une interface utilisateur qui permet aux utilisateurs de gérer leurs données enregistrées dans l’application. Pour les applications avec un enregistrement système automatique, l’application doit offrir une option pour réinitialiser les données enregistrées pour permettre aux utilisateurs de réinitialiser un état de lecture par défaut.

**Assurez-vous que les utilisateurs peuvent accéder toujours l’interface utilisateur pour gérer les jeux enregistrés.** Assurez-vous que votre application peut atteindre toujours son interface utilisateur de gestion pour les jeux enregistrés, même en présence de bogué données enregistrées. Si les données enregistrées d’un utilisateur devient illisibles en raison d’une altération de données ou de bogue application, l’application doit permettre aux utilisateurs de récupérer à un état qui ne se bloquer ou empêchez leur lecture.