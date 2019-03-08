---
title: Configuration du service multijoueur
description: Découvrez comment configurer le Service de mode multijoueur Xbox Live.
ms.assetid: d042d4d5-1c75-4257-8a6f-07eddd39ca7e
ms.date: 07/12/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, mode multijoueur, configuration du service, modèle de session, chaîne d’invitation personnalisé, smartmatch hopper
ms.localizationpriority: medium
ms.openlocfilehash: bf829069824443cdc1c8c0658fcfdfcbe72d0b93
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655874"
---
# <a name="multiplayer-service-configuration"></a>Configuration du service multijoueur
Dans l’ordre pour votre titre tirer parti des services qui fournit de Xbox Live, vous devez d’abord définir la configuration de votre service. Cette configuration de service existe dans le service cloud Xbox Live et définit la façon dont le service Xbox Live interagit avec tous les appareils qui exécutent votre titre/jeu.

Pour des services multijoueurs, il existe trois aspects du mode multijoueur que vous pouvez configurer :
* Modèles de session
* Exploits en SmartMatch
* Chaînes d’invitation personnalisé

## <a name="session-templates"></a>Modèles de session
Le service multijoueur Xbox permet aux joueurs créer et rejoindre des sessions, pour échanger des messages de session avec d’autres joueurs dans la même session et pour valider les résultats de leur lecture à la session. (Les résultats de validation nettoie la session et met également à jour les classements de tous les joueurs dans la session.)

Par exemple, une session multijoueur peut être un jeu unique de chess entre deux joueurs. Il peut également être une session continue d’une action et adventure titre joué par un plus grand nombre de joueurs.

Lorsqu’un jeu crée une nouvelle session, il crée la session basé sur un modèle de session prédéfini. Ce modèle est essentiellement un objet JSON qui contient les attributs qui décrivent la session.

Lorsque vous créez un modèle de nouvelle session, vous devez définir les éléments suivants :

| Champ | Description |
| --- | --- |
| Nom de session | Entrez un nom qui caractérise le modèle de session multijoueur, et que vous allez facilement mémoriser et reconnaître. Le nom doit être une chaîne de texte, avec un maximum de 100 caractères. |
| Version de contrat | Cette valeur est rempli automatiquement par le système et indique la version actuelle du système du contrat JSON. Ne le modifiez pas. |
| Modèle de session (texte JSON) | Spécifiez les données JSON qui décrit les différents attributs associés à une session multijoueur. |

Pour plus d’informations sur les modèles de session multijoueur, y compris plusieurs modèles prédéfinis que vous pouvez utiliser comme base pour le texte JSON, consultez [modèles de session multijoueur](session-templates.md).

> **Important :** Après qu’un titre est écoulé Certification finale, les sessions multijoueurs existantes dans ce titre peuvent n’est plus être modifiées ou supprimées.

## <a name="smartmatch-hoppers"></a>Exploits en SmartMatch

Un ajout facultatif au service Xbox multijoueur est le Xbox basée sur le serveur de service, qui fournit une méthode de regroupement joueurs ensemble en fonction des informations fournies par le titre ou stockées dans les statistiques de l’utilisateur ou en fonction des préférences de l’utilisateur, ou selon la qualité de service.

Étant donné que Xbox One matchmaking est basé sur le serveur, les utilisateurs peuvent fournir une demande au service et ensuite notifiées plus tard, chaque fois qu’une correspondance est trouvée. Autrement dit : l’utilisateur n’est pas obligé de patienter votre titre, le processus de matchmaking se produit, elles sont gratuites lire le joueur de votre titre, ou même pour lire d’autres titres et toujours être candidats pour matchmaking. Cela élimine la nécessité d’atteindre une masse « critique » de joueurs avant la correspondance peut être trouvée.

Un hopper matchmaking doit être basé sur un modèle de session précédemment défini.

Lorsque vous créez un nouveau hopper matchmaking, vous devez définir les éléments suivants :

| Champ | Description |
|---|---|
|Nom| Entrez un nom qui caractérise la hopper matchmaking et que vous allez facilement mémoriser et reconnaître. Le nom doit être une chaîne de texte, avec un maximum de 140 caractères. |
| Taille du groupe de min | Spécifiez le nombre minimal acceptable de joueurs. Valeur minimale est 1. |
| Taille maximale du groupe | Spécifiez le nombre maximal acceptable de joueurs. Valeur maximale est 256. |
| Doit règle Cycles d’Expansion | La valeur par défaut est 3. La valeur par défaut ne devez pas être modifié pour le remplissage de lecteur normal. |
| Hopper classé | Si un hopper est marqué comme un Hopper classés il autorise les joueurs dans ce hopper à mettre en correspondance ensemble même si elles sont bloquées mutuellement. Cela permet d’empêcher des personnes d’essayer d’éviter les joueurs avec des compétences approfondies en bloquant les. |
| Mise à jour automatique à partir de la session | Lorsque ce champ est activé, les modifications apportées à la liste des membres de la session ou propriétés personnalisées des membres sont automatiquement propagées à un ticket précédemment soumis. |

> **Important :** Après qu’un titre est écoulé Certification finale, exploits en de matchmaking existant dans ce titre peuvent ne plus être modifiées ou supprimées.

## <a name="custom-invite-strings"></a>Chaînes d’invitation personnalisé
Lorsque votre titre envoie une invitation à un lecteur pour joindre un jeu multijoueur, vous pouvez choisir d’afficher une chaîne de texte d’invitation personnalisé au lieu de la chaîne d’invitation par défaut.

Lorsque vous créez une nouvelle chaîne d’invitation personnalisé, vous devez définir les éléments suivants :

| Champ | Description |
|---|---|
| ID | L’ID de la personnalisation inviter chaîne qui sera utilisée pour identifier la chaîne. « custominvitestrings_ » sont ajoutés automatiquement au début de votre code. Maximum 100 caractères |
| Valeur | Le texte de la chaîne d’invitation personnalisé qui s’affiche dans votre invitation personnalisé de toast. Maximum 100 caractères |

## <a name="additional-information"></a>Informations complémentaires

Pour plus d’informations sur la configuration du service multijoueur, consultez les articles suivants :

**Article** | **Description**
--- | ---
[Configurer votre AppXManifest pour le mode multijoueur](configure-your-appxmanifest-for-multiplayer.md) | Décrit comment configurer un fichier UWP AppXManifest fonctionne avec le service de multijoueur avec Xbox Live.
[Modèles de session multijoueurs](session-templates.md) | Donne un bref aperçu des modèles de session multijoueur et fournit plusieurs exemples de modèles que vous pouvez copier et modifier pour vos sessions multijoueurs.
[Constantes de modèle de session](session-template-constants.md) | Décrit les éléments prédéfinis d’un modèle de session multijoueurs.
[Sessions de grande taille](large-sessions.md) | Explique quand et comment utiliser des sessions de grande taille.
