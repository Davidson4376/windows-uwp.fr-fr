---
title: Feedback (JSON)
assetID: 726117c1-f01b-18c0-3b75-a7a7d27d84a2
permalink: en-us/docs/xboxlive/rest/json-feedback.html
description: " Feedback (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9cc1cb4ecc12219d54af1c4ab420671f2bbfa81f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644764"
---
# <a name="feedback-json"></a>Feedback (JSON)
Contient des informations de commentaires sur un lecteur.
<a id="ID4EN"></a>


## <a name="feedback"></a>Commentaires

L’objet de commentaires a la spécification suivante.

| Membre| Type| Description|
| --- | --- | --- |
| sessionRef| objet | Objet décrivant la session MPSD ces commentaires concerne, ou null. |
| feedbackType| chaîne | Le type de commentaires. Les valeurs possibles sont définies dans le <b>Microsoft.Xbox.Services.Social.ReputationFeedbackType</b>. |
| textReason| chaîne| Texte fourni par l’utilisateur que l’expéditeur est ajouté pour expliquer la raison pour laquelle le commentaire a été envoyé. |
| voiceReasonId| chaîne| L’ID d’un fichier de voix fournie par l’utilisateur à partir de Kinect l’expéditeur ajouté pour expliquer la raison pour laquelle le commentaire a été soumis (Base-64). |
| evidenceId| chaîne| L’ID d’une ressource qui peut être utilisée en tant que preuve des commentaires en cours d’envoi, par exemple, un fichier vidéo enregistré au cours du jeu. |

<a id="ID4EVC"></a>


### <a name="feedback-types"></a>Types de commentaires

La colonne « Envoyé par » indique qui peut soumettre des commentaires.

   * « Utilisateur » signifie qu’il puisse être soumis à la console à l’aide d’un XToken pour l’authentification, par conséquent, l’API peut accepter **SubmitFeedback**.
   * « Partenaire » signifie qu’il puisse être soumis par un partenaire à l’aide d’un certificat de revendications, par conséquent, l’API peut accepter **SubmitBatchFeedback**.
   * « Confidentialité » ne signifie que le service de confidentialité de SLS peut envoyer les commentaires.
   * « None » signifie que les commentaires sont généré en interne par le service de réputation SLS pour l’audit et ne peut pas être envoyé par un appelant.

| Type| Envoyé par| Remarques|
| --- | --- | --- | --- | --- | --- |
| CommsAbusiveVoice| Utilisateur| Les utilisateurs envoient des commentaires pour les communications vocales inapproprié rapport à partir d’un titre et à partir du tableau de bord Xbox. |
| CommsInappropriateVideo| Utilisateur, partenaire| Les utilisateurs et les partenaires envoient des commentaires pour signaler inapproprié vidéo à partir d’un titre et à partir du tableau de bord Xbox. |
| CommsMuted| Confidentialité| Lorsqu’un utilisateur désactive le volume d’un autre lecteur, confidentialité envoie ces commentaires au service de réputation. |
| CommsPhishing| Utilisateur| Les utilisateurs envoient ces commentaires pour signaler un message de phishing. |
| CommsPictureMessage| Utilisateur| Le service de boîte de réception appelle le service de réputation, qui met à jour de la réputation de l’expéditeur basée sur la communication d’une image et signale les commentaires à l’équipe de mise en œuvre. |
| CommsSpam| Utilisateur| Les utilisateurs envoient ces commentaires pour signaler un message indésirable. |
| CommsTextMessage| Utilisateur| Le service de boîte de réception appelle le service de réputation, qui met à jour de la réputation de l’expéditeur et signale les commentaires à l’équipe de mise en œuvre. **Remarque :** L’UI Inbox doit avoir un bouton permettant aux utilisateurs de marquer le message. |
  | CommsVoiceMessage | Utilisateur | Le service de boîte de réception appelle le service de réputation, qui met à jour de la réputation de l’expéditeur basée sur la communication d’un message vocal et signale les commentaires à l’équipe de mise en œuvre.  |
  | FairPlayBlock | Confidentialité | Confidentialité envoie ces commentaires au service de réputation lorsqu’un utilisateur bloque un autre lecteur.  |
  | FairPlayCheater | Utilisateur, partenaire | Les titres qui déterminent qu’un utilisateur est de la triche peuvent envoyer ces commentaires sans intervention de l’utilisateur.  |
  | FairPlayConsoleBanRequest | Partenaire | Un partenaire envoie ces commentaires sous forme de recommandations à interdire une console Xbox Live.  |
  | FairPlayIdler | Utilisateur, partenaire | Les titres qui déterminent si un utilisateur est l’abréviation inactif exprès dans un jeu, généralement round après arrondi, peuvent envoyer ces commentaires sans intervention de l’utilisateur.  |
  | FairPlayKicked | Utilisateur, partenaire | Les titres qui détectent qu’un utilisateur est voté sortir d’un jeu (exclu) peuvent envoyer ces commentaires sans intervention de l’utilisateur.  |
  | FairPlayKillsTeammates | Utilisateur, partenaire | Titres peuvent déterminer automatiquement quand un lecteur killls son collègue peut envoyer ces commentaires sans intervention de l’utilisateur.  |
  | FairPlayQuitter | Utilisateur, partenaire | Les titres qui déterminent qu’un utilisateur quitté un jeu très tôt peuvent envoyer ces commentaires sans intervention de l’utilisateur.  |
  | FairPlayTampering | Utilisateur, partenaire | Les titres qui déterminent qu’un utilisateur a falsifié sur disque contenu peuvent envoyer ces commentaires sans intervention de l’utilisateur.  |
  | FairPlayUnblock | Confidentialité | Confidentialité envoie ces commentaires au service de réputation quand un utilisateur débloque un autre lecteur.  |
  | FairPlayUserBanRequest | Partenaire | Un partenaire envoie ces commentaires sous forme de recommandations pour exclure un utilisateur à partir de la Xbox Live.  |
  | InternalAmbassadorScoreUpdated | Aucune | Il s’agit d’un type de commentaire interne non pour être utilisés par les appelants.  |
  | InternalReputationReset | Aucune | Il s’agit d’un type de commentaire interne non pour être utilisés par les appelants.  |
  | InternalReputationUpdated | Aucune | Il s’agit d’un type de commentaire interne non pour être utilisés par les appelants.  |
  | PositiveHelpfulPlayer | Utilisateur, partenaire | Les utilisateurs et les partenaires signalez-le à soumettre des informations positif sur les joueurs témoignage utiles à partir d’au sein des jeux, des forums et ainsi de suite.  |
  | PositiveHighQualityUGC | Utilisateur, partenaire | Les utilisateurs et les partenaires envoient ces commentaires pour indiquer que les titres doivent autoriser les utilisateurs à envoyer des commentaires positifs sur UGC partagé à partir de dans le jeu, par exemple, réglage des configurations dans Forza.  |
  | PositiveSkilledPlayer | Utilisateur, partenaire | Les utilisateurs et les partenaires envoient vos commentaires pour indiquer que les titres permettent aux utilisateurs de voter sur MVP à la fin d’une session MPSD.  |
  | UserContentGamerpic | Utilisateur | Les utilisateurs envoient ces commentaires pour signaler une image gamer inappropriée directement à partir de la carte de joueur.  |
  | UserContentGamertag | Utilisateur | Les utilisateurs envoient ces commentaires pour signaler un Gamertag inapproprié directement à partir de la carte de joueur.  |
  | UserContentInappropriateUGC | Utilisateur, partenaire | Les utilisateurs et les partenaires envoient vos commentaires pour indiquer que les titres doivent permettre aux utilisateurs d’indicateur inapproprié UGC partagé à partir de dans le jeu, par exemple, les travaux de peinture dans Forza.  |
  | UserContentPersonalInfo | Utilisateur | Les utilisateurs envoient ces commentaires pour signaler une biographie et autres informations personnelles directement à partir de la carte de joueur.  |

<a id="ID4EFEAC"></a>


## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON


```json
{
"sessionRef": {
  "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
  "templateName": "CaptureFlag5",
  "name": "Halo556932",
  },
  "feedbackType": "CommsAbusiveVoice",
  "textReason": "He called me a doodoo-head!",
  "voiceReasonId": null,
  "evidenceId": null
}

```


<a id="ID4EOEAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EQEAC"></a>


##### <a name="parent"></a>Parent

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)
