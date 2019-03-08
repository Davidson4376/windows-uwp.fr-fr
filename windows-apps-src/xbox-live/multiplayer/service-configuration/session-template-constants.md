---
title: Constantes du modèle de session
description: Décrit les constantes de système définies dans les modèles de session multijoueur Xbox Live.
ms.assetid: d51b2f12-1c56-4261-8692-8f73459dc462
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox one, modèle de session multijoueurs,
ms.localizationpriority: medium
ms.openlocfilehash: 5bb4c6f39bab8779b5675a7b9c81b883965676c2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646904"
---
# <a name="session-template-constants"></a>Constantes du modèle de session

Les tableaux suivants décrivent les éléments prédéfinis d’un modèle de session multijoueur, à l’aide de la version du modèle session 107.

## <a name="system"></a>système

constante de système  | Description | Valeurs valides | valeur par défaut
--|-- | -- | --
version | La version du modèle de session. | 1 - n | aucune
maxMembersCount | Le nombre d’emplacements de membre de nombre total de sessions pris en charge pour l’activité multijoueur. | 1 - 100 pour une session normale, 101 + pour une session de grande taille | 100
visibility | L’état de visibilité de la session, ce qui indique si les autres utilisateurs peuvent voir et/ou rejoindre la session. | Private, visible, ouvrez | ouvrir
inviteProtocol | Définition de cette constante pour « game » permet d’invités à recevoir une notification toast lorsqu’ils sont invités à la session. | jeu, tournamentgame, chat, gameparty | aucune
reservedRemovalTimeout  | Le délai d’expiration pour une réservation de membre, en millisecondes. La valeur 0 indique un délai d’expiration immédiat. Si le délai d’expiration est null, il est considéré comme infini. | 0 - n, null | 30000
inactiveRemovalTimeout  | Le délai d’expiration pour un membre être considéré comme inactif, en millisecondes. La valeur 0 indique un délai d’expiration immédiat. Si le délai d’expiration est null, il est considéré comme infini. | 0 - n, null | 0
readyRemovalTimeout | Le délai d’expiration pour un membre être considérées comme prêtes, en millisecondes. La valeur 0 indique un délai d’expiration immédiat. Si le délai d’expiration est null, il est considéré comme infini. | 0 - n, null | 180000
sessionEmptyTimeout | Le délai d’expiration pour une session vide, en millisecondes. La valeur 0 indique un délai d’expiration immédiat. Si le délai d’expiration est null, il est considéré comme infini. | 0 - n, null | 0
[**capabilities**](#capabilities) | Spécifie les fonctionnalités de la session. Consultez la section fonctionnalités ci-dessous. | Non applicable | Non applicable
[**Métriques**](#metrics) | Spécifie un ensemble de titre défini impératifs de qualité de service, telles que la vitesse de latence et de bande passante, qui doivent satisfaire les membres dans la session.  | Non applicable | Non applicable
[**memberInitialization**](#memberInitialization) | Spécifie les exigences de délais d’attente et d’initialisation qui sont appliquées lorsque de nouveaux membres rejoignent la session. Consultez la section de l’initialisation de membres ci-dessous. | Non applicable | Non applicable
[**peerToPeerRequirements**](#peerToPeerRequirements) | Spécifie le réseau impératifs de qualité de service pour les connexions de maillage de pair à pair. Consultez la section Configuration requise de pair à pair ci-dessous. |Non applicable | Non applicable
[**peerToHostRequirements**](#peerToHostRequirements) | Spécifie le réseau impératifs de qualité de service pour l’homologue pour les connexions de l’hôte. Consultez l’homologue à la section de configuration requise d’hôte ci-dessous. | Non applicable | Non applicable
[**measurementServerAddresses**](#measurementserveraddresses) | Spécifie une collection de centres de données potentielles qui sont utilisées pour déterminer les mesures de qualité de service. Consultez la section measurementServerAddresses ci-dessous. | Non applicable | Non applicable
[**cloudComputePackage**](#cloudComputePackage) | ? | Non applicable | Non applicable
[**arbitration**](#arbitration) | Spécifie les délais d’expiration pour les membres à soumettre les résultats d’arbitrage dans tournois. Consultez la section cloudComputePackage ci-dessous. | Non applicable | Non applicable
[**broadcastViewerTitleIds**](#broadcastViewerTitleIds) | Spécifie une liste d’ID qui doit toujours avoir un accès en lecture à la session de titre. Consultez la section broadcastViewerTitleIds ci-dessous. | Non applicable | Non applicable
[**ownershipPolicies**](#ownershipPolicies) | Spécifie les stratégies relatives à la propriété de session. Consultez la section OwnershipPolicies ci-dessous. | Non applicable | Non applicable


## <a name="capabilities"></a>capabilities
Fonctionnalités sont des valeurs booléennes qui sont éventuellement définis dans le modèle de session. Si aucune capacité n’est nécessaires, un objet vide « capabilities » doit être dans le modèle pour empêcher la fonctionnalités spécifiés sur la création de session, à moins que le titre désire des fonctionnalités de la session dynamique.

Fonctionnalité |  description | Valeurs valides | valeur par défaut
-- | -- | -- | -- |
réseau | Indique si la session prend en charge la connectivité de l’homologue. Si cette valeur est false, puis la session ne peut pas activer les métriques et les membres de la session ne peut pas définir leurs SecureDeviceAddress. Ne peut pas être définie sur les sessions de grande taille. | la valeur est true, false | false
suppressPresenceActivityCheck | Si la valeur est true, désactive les contrôles de présence. | la valeur est true, false | false
déroulement du jeu | Indique si la session représente le jeu réel, par opposition à la fois le programme d’installation/menu comme une salle d’attente ou un matchmaking. Si la valeur est true, la session est en mode de jeu. | la valeur est true, false | false
grande | Indique si la session est une session de grande taille (plus de 100 membres). Sessions volumineuses ne sont pas prises en charge pour une utilisation avec le gestionnaire multijoueur. | la valeur est true, false | false
connectionRequiredForActiveMembers | Indique si une connexion est nécessaire pour un membre être active. | la valeur est true, false | false
cloudCompute | Permet aux clients de demander qu’une instance de calcul cloud être allouée pour le compte de la session. | la valeur est true, false | false
autoPopulateServerCandidates | Automatiquement calculer et 'serverConnectionStringCandidates' de 'serverMeasurements'. Impossible de définir cette fonctionnalité sur les sessions de grande taille. | la valeur est true, false | false
userAuthorizationStyle | Indique si la session prend en charge les appels à partir des plateformes sans identité du titre fort. Impossible de définir cette fonctionnalité sur les sessions de grande taille.</br></br>Définissant le `userAuthorizationStyle` possibilité `true` valeurs par défaut le `readRestriction` et `joinRestriction`de la session à `local` au lieu de `none`. Cela signifie que les titres doivent utiliser des handles de recherche ou transférer les poignées pour rejoindre une session de jeu.| la valeur est true, false | false
crossplay | Indique que la session prend en charge la lecture croisée entre les appareils PC et Xbox One. | la valeur est true, false | false
Diffusion | Indique que la session représente une diffusion. Le nom de la session doit être le xuid de la chaîne de télévision. Nécessite la fonction « grande ». | la valeur est true, false | false
Équipe | Indique que la session représente une équipe tournoi. Cette fonctionnalité ne peut pas être définie sur les sessions de « large » ou « a ». | la valeur est true, false | false
arbitration | Indique que la session doit être créée par un principal de service qui ajoute l’entrée de serveur « arbitrage ». Ne peut pas être définie sur les sessions de « large », mais nécessite le « jeu ». | la valeur est true, false | false
hasOwners | Indique que la session a une stratégie de sécurité basée sur certains membres étant propriétaires. | la valeur est true, false | false
possibilité de recherche | Indique que la session peut être une session de la cible d’un handle de recherche. Si la fonctionnalité « userAuthorizationStyle » est définie, la fonctionnalité « searchable » ne peut pas être définie si la fonctionnalité 'hasOwners' n’est pas définie. | la valeur est true, false | false

Exemple :

```json
"capabilities": {
    "connectivity": true,  
    "suppressPresenceActivityCheck": true,
    "gameplay": true,
    "large": true,
    "connectionRequiredForActiveMembers": true,
    "cloudCompute": true,
    "autoPopulateServerCandidates": true,
    "userAuthorizationStyle": true,
    "crossPlay": true,  
    "broadcast": true,  
    "team": true,   
    "arbitration": true,   
    "hasOwners": true,   
    "searchable": true  
},
```

## <a name="metrics"></a>Métriques
Si le `metrics` propriétés ne sont pas spécifiées, leur valeur par défaut pour les valeurs qui sont nécessaires pour satisfaire les exigences de qualité de service.  
S’ils sont spécifiés, les valeurs doivent être suffisantes pour satisfaire les exigences de qualité de service.
Cet élément est valide uniquement si la session a la `connectivity` ensemble de la fonctionnalité.

Métrique | Description | Valeurs valides | valeur par défaut
-- | -- | -- | --
latence | | la valeur est true, false | consultez la Description
bandwidthDown | | la valeur est true, false | consultez la Description
bandwidthUp | | la valeur est true, false | consultez la Description
Personnalisé | | la valeur est true, false | consultez la Description

Exemple :
```json
"metrics": {
    "latency": true,
    "bandwidthDown": true,
    "bandwidthUp": true,
    "custom": true
},
```

## <a name="memberinitialization"></a>memberInitialization
Si un `memberInitialization` objet est défini, la session attend le système client ou le titre pour effectuer une initialisation après la création de session et/ou en tant que nouveaux membres rejoindre la session.  
Les étapes des délais d’expiration et d’initialisation sont automatiquement suivies par la session, y compris les mesures de qualité de service si toutes les mesures sont définies.  
Ces délais d’attente remplacent la réservation et les délais d’expiration de prêt de la session pour les membres qui ont « initializationEpisode » définie.  
Ne peut pas être spécifié sur les sessions de grande taille.

Élément  | Description | Valeurs valides | valeur par défaut
-- | -- | -- | --
joinTimeout | Indique le nombre de millisecondes pendant lesquelles un membre peut se joindre à la session. Réservations d’utilisateurs qui ne parviennent pas à joindre sont supprimées.</br>**Remarque :** La durée par défaut est suffisante pour l’exécution de titre normale, mais elle peut entraîner à joindre des délais d’attente si un titre est en cours de débogage pendant le flux MPSD. Pour ces scénarios, remplacer et augmenter cette valeur par défaut pour la session.| 0 - n | 10000
measurementTimeout | Indique le nombre de millisecondes pendant lesquelles un membre de la session a pour charger des mesures. Un membre qui ne parvient pas à télécharger des mesures est marqué avec un motif de l’échec de « timeout ».  | 0 - n | 30000
evaluationTimeout | Indique le nombre de millisecondes pendant lequel une évaluation externe doit charger des mesures. | 0 -n | 5000
externalEvaluation | Si la valeur est true, indique que le code de titre effectue l’évaluation de qui a une jointure basée sur les mesures de qualité de service. Le service multijoueur n’effectue pas de logique de qualité de service, et le titre est responsable de l’avancement de l’étape d’initialisation. Titres est généralement inutile cela. | la valeur est true, false | false
membersNeededToStart | Le nombre de membres nécessaires pour démarrer la session, pour l’épisode de l’initialisation de zéro uniquement. | 1 - maxMembersCount | 1

Exemple :
```json
"memberInitialization": {
    "joinTimeout": 10000,  
    "measurementTimeout": 30000,  
    "evaluationTimeout": 5000,
    "externalEvaluation": false,
    "membersNeededToStart": 1
},
```


## <a name="peertopeerrequirements"></a>peerToPeerRequirements

configuration requise du réseau d’égal à égal | Description | valeur par défaut
-- | -- |--
latencyMaximum | La latence maximale, en millisecondes, entre deux clients. | 250
bandwidthMinimum | La bande passante minimale en kilobits par seconde entre tous les deux clients. | 10000

Exemple :
```json
"peerToPeerRequirements": {
    "latencyMaximum": 250,  
    "bandwidthMinimum": 10000
},
```


## <a name="peertohostrequirements"></a>peerToHostRequirements

configuration requise du réseau hôte de l’homologue | Description | Valeurs valides | valeur par défaut
-- | -- | -- | --
latencyMaximum | La latence maximale, en millisecondes, pour l’homologue pour la connexion de l’hôte. | | 250
bandwidthDownMinimum | La bande passante minimale en kilobits par seconde pour les informations envoyées à partir de l’hôte de l’homologue. | | 100000
bandwidthUpMinimum | La bande passante minimale en kilobits par seconde pour les informations envoyées à partir de l’homologue à l’hôte. | | 1000
hostSelectionMetric | Indique la métrique est utilisée pour sélectionner l’ordinateur hôte. | bandwidthup, bandwidthdown, la bande passante et latence | latence

Exemple :
```json
"peerToHostRequirements": {
    "latencyMaximum": 250,
    "bandwidthDownMinimum": 100000,
    "bandwidthUpMinimum": 1000,  
    "hostSelectionMetric": "bandwidthup"
},
```

## <a name="measurementserveraddresses"></a>measurementServerAddresses
Le jeu potentiel server des chaînes de connexion qui doit être évalué. Les chaînes de connexion doivent être en minuscules.
Ne peut pas être spécifié sur les sessions de grande taille.

Les chaînes de connexion sont définies dans le format suivant :

`"<server name>" : {deviceAddress}`

Où l’adresse du périphérique est décrit comme suit :

chaîne de connexion de serveur | Description
-- | --
secureDeviceAddress | La base-64 codé adresse périphérique sécurisé du serveur

Exemple :
```json
"measurementServerAddresses": {
    "server farm a": {
        "secureDeviceAddress": "r5Y="
    },
    "datacenter b": {
        "secureDeviceAddress": "rwY="
    }
},
```

## <a name="cloudcomputepackage"></a>cloudComputePackage
Spécifie que les propriétés du cloud calcul package à allouer. Exige que le `cloudCompute` fonctionnalité est définie.

propriété de calcul de cloud | Description
-- | -- | -- | --
titleId | Indique le titre de package pour allouer de calcul de l’ID du cloud.
gsiSet | Indique l’ensemble GSI du package de calcul de cloud à allouer.
Type Variant | Indique la variante du package de calcul de cloud à allouer.

Exemple :
```json
"cloudComputePackage": {
    "titleId": "4567",
    "gsiSet": "128ce92a-45d0-4319-8a7e-bd8e940114ec",
    "vaiant": "30ebca60-d96e-4629-930b-6957aa6bfbfa"
},
```

## <a name="arbitration"></a>arbitration
Spécifie les délais d’attente pour le processus d’arbitrage. Exige que le `arbitration` fonctionnalité est définie. L’heure de début d’arbitrage est défini dans une session dans le */servers/arbitration/constants/system/startTime* élément.

timeout | Description | Valeurs valides | par défaut
-- | -- | -- | --
forfeitTimeout | Indique la durée, en millisecondes, entre l’heure de début d’arbitrage, qui un TBD | 0 - n | 60000
arbitrationTimeout | Indique la durée, en millisecondes, entre l’heure de début arbitrage, le résultat de l’arbitrage arrive à expiration. Cette valeur ne peut pas être inférieure à la `forfeitTimeout` valeur | 0 - n | 300000

Exemple :
```json
"arbitration": {
    "forfeitTimeout": 60000,
    "arbitrationTimeout": 300000
},
```

## <a name="broadcastviewertitleids"></a>broadcastViewerTitleIds

Spécifie un tableau du titre de l’ID de titres qui doivent toujours avoir un accès en lecture à la session de diffusion.

Exemple :
```json
"broadcastViewerTitleIds" : ["34567", "8910"],
```

## <a name="ownershippolicies"></a>ownershipPolicies
Spécifie comment gérer une session lorsque le propriétaire du dernier quitte la session. Exige que le `hasOwners` fonctionnalité est définie.

stratégie de propriété | Description | Valeurs valides | par défaut
-- | -- | -- | --
Migration | Indique le comportement qui se produit lorsque le propriétaire du dernier quitte la session. Si la stratégie de migration est définie sur « endsession », faire expirer la session. Si la stratégie de migration est définie sur « plus ancien », sélectionnez le membre avec l’heure de jointure plus ancien à devenir le nouveau propriétaire de la session. | "oldest", "endsession" | « endsession »

Exemple :
```json
"ownershipPolicies": {
     "migration": "oldest"
}
```
