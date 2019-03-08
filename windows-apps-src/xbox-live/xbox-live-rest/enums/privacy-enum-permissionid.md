---
title: PermissionId Enumeration
assetID: 3e7ee317-4946-1105-ecd7-1e26346deccb
permalink: en-us/docs/xboxlive/rest/privacy-enum-permissionid.html
description: " PermissionId Enumeration"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: aff463e2268af972536984a00e29348bf0a6859e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594684"
---
# <a name="permissionid-enumeration"></a>PermissionId Enumeration
Décrit en détail l’énumération PermissionId.
ID d’autorisation peuvent être utilisés avec les URL de validation d’autorisation :

   * [OBTENIR (/users/ {requestorId} / autorisation/valider)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidateget.md)
   * [POST (/users/ {requestorId} / autorisation/valider)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidatepost.md)

Ces ID inclure des vérifications directes par rapport à un paramètre particulier d’un utilisateur, tels que la vérification d’un paramètre de confidentialité unique d’une cible ou un acteur de privilège unique. En outre, il existe des ID qui peut être utilisé avec l’autorisation d’API et incorporer des vérifications par rapport à plusieurs paramètres pour les actions de l’utilisateur spécifique d’autorisation.

<a id="ID4EIB"></a>


## <a name="permissions"></a>Permissions

Il s’agit de valeurs un appelant peut utiliser pour vérifier si une action spécifique peut être exécutée. Contrairement aux paramètres ci-dessus, ces encapsulent des stratégies définies par le service et ne peut pas être modifiées directement par les utilisateurs, bien que dans la plupart des cas, les stratégies basées sur un ou plusieurs paramètres dont les valeurs sont définies par les utilisateurs. Il s’agit généralement composites vérifications par rapport à plusieurs paramètres définies ci-dessus. Exemple : Le <b>ViewProfile</b> autorisation effectue une vérification de la cible <b>ShareProfile</b> paramètre de confidentialité et du demandeur <b>AllowProfileViewing</b> privilège.

En règle générale, il est recommandé que les appelants demander un code d’autorisation pour les actions qui doivent être vérifiées, au lieu de vérifier directement des privilèges et des paramètres de confidentialité. Ainsi, les politiques de confidentialité pour être modifié régulièrement au sein du service comme nouvelles vérifications sont incorporées.

| Nom de l'autorisation| Description|
| --- | --- |
| CommunicateUsingText| Vérifiez si l’utilisateur peut envoyer un message avec le contenu de texte à l’utilisateur cible|
| CommunicateUsingVideo| Vérifiez si l’utilisateur peut communiquer à l’aide de la vidéo avec l’utilisateur cible|
| CommunicateUsingVoice| Vérifiez si l’utilisateur peut communiquer à l’aide de la voix avec l’utilisateur cible|
| ViewTargetProfile| Vérifiez si l’utilisateur peut afficher le profil de l’utilisateur cible|
| ViewTargetGameHistory| Vérifiez si l’utilisateur peut afficher l’historique des jeux de l’utilisateur cible|
| ViewTargetVideoHistory| Vérifiez si l’utilisateur peut afficher l’historique de regarder vidéo détaillée de l’utilisateur cible|
| ViewTargetMusicHistory| Vérifiez si l’utilisateur peut visualiser l’historique à l’écoute de musique détaillées de l’utilisateur cible|
| ViewTargetExerciseInfo| Vérifiez si l’utilisateur peut visualiser les informations de l’exercice de l’utilisateur cible|
| ViewTargetPresence| Vérifiez si l’utilisateur peut voir le statut de connexion de l’utilisateur cible|
| ViewTargetVideoStatus| Vérifiez si l’utilisateur peut afficher les détails de l’état de vidéo cibles (étendue présence en ligne)|
| ViewTargetMusicStatus| Vérifiez si l’utilisateur peut afficher les détails de l’état de musique cibles (étendue présence en ligne)|
| ViewTargetUserCreatedContent| Vérifiez si l’utilisateur peut voir le contenu créé par l’utilisateur d’autres utilisateurs|
