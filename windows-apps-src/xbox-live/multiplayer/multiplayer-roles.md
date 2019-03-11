---
title: Rôles multijoueurs
description: En savoir plus sur l’utilisation de rôles pour définir des rôles de lecteur en mode multijoueur de Xbox Live.
ms.date: 06/29/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, rôles multijoueurs, one, xbox
ms.localizationpriority: medium
ms.openlocfilehash: ac5e7758bd8e068681d1c8dab2d47d11374c2616
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662084"
---
# <a name="roles"></a>Rôles

Pour des sessions de jeu, vous souhaiterez sans doute spécifier que certains membres disposent de certains rôles de jeu, telles que la prise en charge, medic, assaut, etc. Vous pouvez également wat pour réserver des emplacements de jeu pour les joueurs qui remplissent un rôle de jeu spécifique. À l’aide de la fonctionnalité des rôles Xbox Live, le service peut suivre les jeu des rôles sont attribués à quels lecteurs et appliquer un nombre maximal de lecteurs que vous pouvez sélectionner un rôle de jeu spécifique.

L’utilisation la plus courante des rôles consiste à déterminer les rôles spécifiques jeu pour cette session de jeu. Par exemple, vous pouvez avoir un mode de jeu nécessite entre 1 et 2 classes de prise en charge, au moins 1 classe tank/lourds et pas plus de 5 assaut classes.

Dans un autre scénario possible, vous souhaiterez peut-être spécifier qu’une session de jeu peut avoir exactement 4 joueurs, jusqu'à 8 spectateurs et 1 annonceur.

Vous pouvez également utiliser des rôles pour réserver des emplacements à vos amis pendant le remplissage des créneaux restants par d’autres moyens, tels que de la navigation dans la session.

## <a name="role-types"></a>Types de rôle

Un type de rôle représente un groupe de définitions de rôles. Chaque rôle doit être défini en tant que partie d’un type de rôle. Types de rôles sont définis dans le document de session multijoueurs.

Un membre peut uniquement avoir un rôle à partir d’un type de rôle donné. Par exemple, si un type de rôle « classe » inclut les dommages, tanks et healers, puis un membre peut appartenir qu’à un de ces rôles.

Vous pouvez définir plusieurs types de rôle, et un membre peut être assigné à un rôle de chaque type de rôle. Dans le scénario précédent, un membre peut avoir choisi le rôle guérisseur, mais peut aussi être assigné un escouade rôle de responsable, si le rôle de responsable d’équipe est défini dans un type de rôle distinct.

> **Important :** Le Kit de développement Xbox Live actuellement prend uniquement en charge un type de rôle unique et un seul rôle par membre.

## <a name="role-type-properties"></a>Propriétés de type de rôle

Lorsque vous définissez un type de rôle, vous devez spécifier les informations suivantes pour les types de rôles :

* Le nom du type de rôle. Le nom doit être en minuscules et alphanumérique et pas plus de 100 caractères.
* Si les rôles définis dans le type de rôle sont propriétaire géré ou non.
* Si les propriétés des rôles définis dans le type de rôle peuvent être modifiées pendant la durée de vie de la session.
* Les définitions de rôles qui sont inclus dans le type de rôle.

Si un type de rôle est géré de propriétaire, cela signifie que seuls les membres qui sont des propriétaires de la session peuvent affecter des rôles de ce type aux membres. Si le type de rôle n’est pas propriétaire géré, les membres peuvent attribuer des rôles à eux-mêmes.

Vous ne pouvez spécifier qu’un type de rôle est propriétaire gérée sur les sessions qui ont la possibilité de « hasOwners » à définir.

> Le Xbox Live SDK ne prend pas en charge les propriétaires de l’attribution de rôles à d’autres membres.

## <a name="role-properties"></a>Propriétés du rôle

Lorsque vous définissez un rôle, vous devez spécifier les informations suivantes pour chaque rôle :

* Le nom du rôle. Le nom doit être en minuscules et alphanumérique et pas plus de 100 caractères.
* Le nombre maximal de membres qui sont autorisés pour remplir le rôle. Doit être supérieur à zéro.
* Nombre de membres que doit remplir le rôle cible. La cible doit être supérieure à zéro et inférieur ou égal au nombre maximal de membres autorisé pour remplir le rôle.

Lorsqu’un membre de la session est affecté à un rôle, cette information est enregistrée dans les rôles de membre dans le document de session multijoueurs.

Le service applique le nombre maximal de membres qui peuvent être affectés à un rôle, mais n’applique pas le nombre cible.

## <a name="create-roles"></a>Créer des rôles

Rôles et types de rôles sont généralement définis dans le [modèle de session](service-configuration/session-templates.md). Le service prend en charge des rôles et la définition de type de rôle lors de la création de session, mais n’est pas le cas de la Xbox Live SDK.

### <a name="define-role-types-and-roles-in-a-session-template"></a>Définir les types de rôles et des rôles dans un modèle de session

Vous pouvez définir des rôles et types de rôle lorsque vous créez un modèle de session lors de la configuration de Xbox Live.

Les informations de type et le rôle du rôle sont spécifiées comme un élément « roleTypes » de niveau de base dans le modèle de session, dans le format suivant :

```json
"roleTypes": {
  "myroletype1": { // must be lowercase alpha-numeric.
    "ownerManaged": true, // Can only be true on sessions with the "hasOwners" capability set. If true, only the owner of the session can assign this role to members.
    "mutableRoleSettings": ["max", "target"], // Which role settings for roles in this role type can be modified throughout the life of the session. Exclude role settings to lock them.
    "roles": {
      "role1": { // must be lowercase alpha-numeric.
        "max": 3, // Max number of members assigned to this role at a time, enforced by MPSD.
        "target": 2 // Target number of members to assign this role to. Like max, but not enforced (can be exceeded).
      },
      "role2": {
        ...
      }
  },
  "myroletype2": {
    ...
  }
},
```

## <a name="retrieve-role-information-for-a-multiplayer-session"></a>Récupérer des informations de rôle pour une session multijoueur

Vous pouvez obtenir les informations sur le rôle de types, les rôles et combien de membres affectés à chaque rôle à partir d’une session multijoueur, ou à partir d’un handle de recherche multijoueurs.

Dans le SDK Xbox Live, plus d’informations sur les types de rôles et les rôles sont stockées dans une structure de plan. L’utilisation de C++ APIs le `unordered_map` APIs WinRT utiliser et la classe la `IMapView` classe.

### <a name="get-the-role-information-from-a-search-handle"></a>Obtenir les informations de rôle à partir d’un handle de recherche

Dans le `multiplayer_search_handle_details` objet retourné à partir d’une requête de recherche, vous pouvez obtenir les informations de type de rôle en indexant le `role_types` mappage avec le nom du type de rôle que vous êtes intéressé.

Cette commande renvoie un `multiplayer_role_type` objet. Vous pouvez obtenir les rôles en indexant le `roles` mapper, qui retourne un `multiplayer_role_info` objet.

Le `multiplayer_role_info` objet contient des informations sur le rôle, y compris `max_members_count`, `member_xbox_user_ids`, `members_count`, et `target_count`.

### <a name="get-the-role-information-from-a-search-handle"></a>Obtenir les informations de rôle à partir d’un handle de recherche

Le flux pour obtenir des informations de rôle à partir d’une session est similaire au flux pour obtenir des informations à partir d’un handle de recherche, mais certaines classes différentes sont utilisées.

Dans le `multiplayer_session` de l’objet, vous pouvez obtenir les informations de type de rôle en référençant le `session_role_types` objet, qui est un `multiplayer_session_role_types` classe. Dans cet objet, vous pouvez indexer le `role_types` mappage avec le nom du type de rôle vous intéresse.

Cette commande renvoie un `multiplayer_role_type` objet. Vous pouvez obtenir les rôles en indexant le `roles` mapper, qui retourne un `multiplayer_role_info` objet.

Le `multiplayer_role_info` objet contient des informations sur le rôle, y compris `max_members_count`, `member_xbox_user_ids`, `members_count`, et `target_count`.

## <a name="change-mutable-role-settings"></a>Modifier les paramètres de rôle mutable

Si un type de rôle indique que certains paramètres de rôle peuvent être modifiés (altérables), vous pouvez utiliser la `multiplayer_role_type.set_roles()` méthode pour modifier les paramètres mutables. Seuls les membres qui sont marqués en tant que propriétaires de session peuvent modifier les paramètres de rôle.

## <a name="assign-a-role-to-a-member"></a>Attribuer un rôle à un membre

Actuellement, seul un membre peut affecter leur propre rôle dans le Kit de développement Xbox Live. Dans le `multiplayer_session` de l’objet, vous pouvez appeler la `set_current_user_role_info(role_type, role_name)` méthode pour spécifier le type de rôle et le rôle pour le membre actuel.

Si le rôle est déjà complète lorsque vous essayez d’écrire la session dans le service, MPSD rejette l’écriture.
