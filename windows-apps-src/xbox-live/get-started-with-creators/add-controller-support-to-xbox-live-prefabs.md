---
title: Ajouter le Support de contrôleur à Prefabs Live Xbox
description: Ajouter la prise en charge du contrôleur à Xbox Live Prefabs est à l’aide du plug-in Xbox Live Unity
ms.assetid: ''
ms.date: 07/14/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, unity, prise en charge du contrôleur
ms.localizationpriority: medium
ms.openlocfilehash: 4d32ec62b8beec10256ed9a695866c2fd9bdd03e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622874"
---
# <a name="add-controller-support-to-xbox-live-prefabs"></a>Ajouter la prise en charge du contrôleur à prefabs Xbox Live

> [!IMPORTANT]
> Le plug-in de Xbox Live Unity ne prend pas en charge les primes ou mode multijoueur en ligne et qu’il est recommandé uniquement pour [programme Xbox Live Creators](../developer-program-overview.md) membres.

Tous les Prefabs de plug-in Xbox Live Unity prennent en charge en spécifiant contrôleur entrée dans l’inspecteur.

Par exemple, supposons que vous avez un objet de jeu nommé `UserProfile1` qui est basé sur le `UserProfile` prefab. Si vous souhaitez lier cet objet de jeu sur le lecteur 1 et demandez-leur de se connecter avec le `A` bouton sur leur manette Xbox, simplement écrire `joystick 1 button 0` dans le `Input Controller Button` champ dans l’inspecteur.

  ![Prise en charge de contrôleur dans UserProfile préfabriqué](../images/unity/controller-support-example.png)

## <a name="all-prefab-controller-input-fields"></a>Contrôleur Prefab tous les champs d’entrée
### <a name="userprofile-prefab"></a>UserProfile préfabriqué
- **Bouton de contrôleur d’entrée :** Ajoute et se connecte un utilisateur de Xbox Live.

### <a name="social-prefab"></a>Réseaux sociaux préfabriqué
- **Bouton de contrôleur bascule filtre :** Active ou désactive le filtre pour afficher les amis « All » ou « En ligne » amis.

### <a name="leaderboard-prefab"></a>Classement préfabriqué
- **Premier bouton de contrôleur :** Prend le lecteur pour la première page de classement des entrées.
- **Dernier bouton de contrôleur :** Prend le lecteur vers la dernière page de classement des entrées.
- **Bouton de contrôleur suivant :** Prend le lecteur à la page suivante des entrées de classement.
- **Bouton de Prev contrôleur :** Prend le lecteur à la page précédente d’entrées de classement.
- **Actualiser le bouton du contrôleur :** Actualise la vue de classement.


### <a name="game-save-ui-prefab"></a>Préfabriqué de l’interface utilisateur d’enregistrer jeu
- **Générer un nouveau bouton de contrôleur :** Génère un entier de nouvelles données de sauvegarde.
- **Enregistrer un bouton du contrôleur de données :** Enregistre les données actuelles dans le stockage connecté.
- **Bouton de contrôleur de données de charge :** Charge les données actuellement enregistrées dans le stockage connecté.
- **Obtenir des informations contrôleur bouton :** Récupère des informations sur les conteneurs enregistrés dans le stockage connecté.
- **Supprimer un bouton du contrôleur de conteneur :** Supprime le conteneur enregistré à partir du stockage connecté

## <a name="xbox-controller-button-mappings"></a>Mappages de bouton manette Xbox

Pour les mappages de bouton du contrôleur Xbox dans Unity, regardez cette [page Wiki de contrôleur Unity](https://wiki.unity3d.com/index.php?title=Xbox360Controller).
