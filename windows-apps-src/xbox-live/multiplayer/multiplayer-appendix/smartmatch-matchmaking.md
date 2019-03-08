---
title: Matchmaking SmartMatch
description: En savoir plus sur le service Xbox Live SmartMatch pour les joueurs correspondants dans un jeu multijoueur.
ms.assetid: ba0c1ecb-e928-4e86-9162-8cb456b697ff
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, matchmaking de multijoueurs, one, xbox, smartmatch
ms.localizationpriority: medium
ms.openlocfilehash: 6bc5f15e6fdb7d2f393daef8d21c134579610a9b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57647294"
---
# <a name="smartmatch-matchmaking"></a>Matchmaking SmartMatch

Cet article comporte les sections suivantes
* Introduction à SmartMatch
* Opérations d’exécution de SmartMatch
* Configuration SmartMatch pour votre titre
* Définition des règles de l’équipe lors de la configuration de SmartMatch
* Initialisation de la Session cible et la qualité de service

## <a name="introduction-to-smartmatch"></a>Introduction à SmartMatch

Le XSAPI fournit un service équivalent, appelé SmartMatch, qui est encapsulée par la [API du Gestionnaire de mode multijoueur](../multiplayer-manager/multiplayer-manager-api-overview.md).  Pour utilisation avancée de l’API, vous pouvez vous reporter à la **MatchmakingService classe**, mais si vous constatez que vous avez un scénario de matchmaking pas possible d’implémenter à l’aide du Gestionnaire de mode multijoueur, veuillez nous fournir des commentaires via votre mère.  Quel que soit l’API que vous utilisez, les informations conceptuelles dans cet article s’applique.

SmartMatch matchmaking regroupe des joueurs en fonction des informations utilisateur et de la demande de matchmaking pour les utilisateurs qui souhaitent lire ensemble. Matchmaking est basée sur serveur, ce qui signifie que les utilisateurs fournissent une demande au service, et ils sont notifiés ultérieurement lorsqu’une correspondance est trouvée. Lorsque vous créez un titre pour Xbox One, vous pouvez utiliser SmartMatch comme décrit dans [à l’aide des SmartMatch Matchmaking](using-smartmatch-matchmaking.md). Vous pouvez également votre propre service matchmaking comme décrit dans [à l’aide de votre propre service matchmaking](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/multiplayer-and-networking/using-your-own-matchmaking-service). Notez que l’accès à ce lien nécessite que vous avez un [partenaires](https://partner.microsoft.com/dashboard) compte qui est activée pour le développement Xbox Live.

### <a name="about-smartmatch"></a>À propos de SmartMatch

Le service travaille en étroite collaboration avec MPSD afin de simplifier matchmaking. Il permet de titres pour le faire facilement matchmaking en arrière-plan, par exemple, tandis que l’utilisateur en cours de lecture seul joueur dans le titre.

Personnes ou groupes que vous souhaitez entrer matchmaking créer une session de ticket de correspondance, puis demander le service pour rechercher d’autres acteurs avec lesquels vous souhaitez configurer une correspondance. Cela entraîne la création d’une table temporaire « correspondance ticket » résidant dans le service pendant une période de temps.

Le service choisit les sessions à lire en fonction de la configuration, les statistiques stockées pour chaque lecteur et d’autres informations donné au moment de la faire correspondre la demande. Le service crée ensuite une session de cible de correspondance qui contient tous les joueurs ont été mis en correspondance et notifie les titres des utilisateurs de la correspondance.

Lorsque la session cible est prête, titres effectuer des vérifications de qualité de service pour confirmer que le groupe peut jouer ensemble et ensuite commencer play si tout va bien. Cours de la QoS processus et matchmade partie, titres informé l’état de session dans MPSD, et ils recevoir des notifications de MPSD sur les modifications apportées à la session. Ces modifications incluent les utilisateurs entrants et sortants et passe à l’arbitre de session.


### <a name="match-ticket-session"></a>Session de Ticket de correspondance

Une session de ticket correspondance représente les clients pour les lecteurs qui souhaitent établir une correspondance. Il est créé en fonction sur un jeu, ou sur un groupe de personnes étrangères qui se trouvent dans un ensemble de la salle d’attente, ou sur les autres regroupements spécifiques à titre de joueurs. Dans certains cas, la session de ticket peut être une session de jeu déjà en cours d’exécution qui cherche à plus de joueurs.


### <a name="match-ticket"></a>Ticket de correspondance

Envoi d’une session de ticket à matchmaking entraîne la création d’un ticket de correspondance qui assure le suivi de la tentative de matchmaking. Attributs dans le ticket, par exemple, le mappage de jeu ou au niveau du lecteur, ainsi que les attributs des acteurs dans la session de ticket, sont utilisés pour déterminer la correspondance.
#### <a name="hoppers"></a>Exploits en

Exploits en sont des endroits logiques où les tickets de correspondance sont collectés. Que les tickets dans le même hopper peuvent être mis en correspondance. Un titre peut avoir d’exploits en plusieurs. Par exemple, un titre peut créer un hopper quel lecteur compétence est l’élément le plus important pour la correspondance. Il peut utiliser un autre hopper dans lequel les joueurs sont mis en correspondance uniquement si elles ont acheté le même contenu téléchargeable.


#### <a name="hopper-rules"></a>Règles Hopper

Règles de Hopper fournissent des définitions des critères que le service utilise pour le choix d’acteurs à regrouper. Il existe deux types de règles :   DOIT disposer de règles--à satisfaire pour la correspondance des tickets être considéré comme compatible.
-   DOIT règles--un ticket de correspondance correspondant à une règle est privilégié par rapport à l’autre pas.

Dans chacune de ces catégories, il existe plusieurs types spécifiques de règles. Pour plus d’informations, consultez les informations de configuration d’opération runtime dans **opérations d’exécution SmartMatch**.


### <a name="match-target-session"></a>Session de cible de correspondance

Une fois un groupe de mise en correspondance a été trouvé, le service crée une session de cible de correspondance et réserve des emplacements de tous les joueurs depuis les sessions de ticket sont mis en correspondance ensemble.


## <a name="smartmatch-runtime-operations"></a>Opérations d’exécution de SmartMatch



| Remarque                                                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------|
| Pour plus d’informations sur l’utilisation de SmartMatch matchmaking votre titre, consultez [à l’aide des SmartMatch Matchmaking](using-smartmatch-matchmaking.md). |


### <a name="creating-a-match-ticket-session-and-a-match-ticket"></a>Création d’une Session de Ticket de correspondance et un Ticket de correspondance

Un client désigné comme le « scout « matchmaking configure une session de ticket de correspondance pour représenter un groupe de joueurs souhaitant Entrez matchmaking. Tous les acteurs de ce groupe de rejoindre la session à l’aide de la méthode MultiplayerSession.Join. Une fois que la session est remplie avec des lecteurs, le titre soumet la session pour le service à l’aide de la méthode MatchmakingService.CreateMatchTicketAsync, qui crée un ticket de correspondance qui représente la session.

| Remarque                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Le service SmartMatch met à jour le champ /servers/matchmaking/properties/system/status dans la session de ticket à la « recherche » lors de l’opération CreateMatchTicketAsync. |

La réponse à partir de la méthode de création de tickets de correspondance est un objet CreateMatchTicketResponse. Cette réponse contient l’ID de ticket de correspondance, un GUID qui peut être utilisé pour annuler matchmaking en supprimant le ticket. En outre, un temps d’attente moyen pour le hopper est inclus dans la réponse à utiliser dans la réponse aux attentes du lecteur.

| Remarque                                                                                                                                                                                                                                                                                                                                 |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Sauf s’il existe des données statistiques jeu spécifiques qui doivent être préservées, il est recommandé que les titres utilisent la valeur de PreserveSessionMode.Never lors de l’appel CreateMatchTicketAsync. À l’aide de PreserveSessionMode.Never permet matchmaking plus rapide et supprime la nécessité d’avoir des options d’interface utilisateur pour l’hébergement d’un jeu et non à la recherche d’un match. |


### <a name="setting-matchmaking-attributes-on-the-session-and-players"></a>Définition des attributs de Matchmaking sur la Session et lecteurs

Lors de l’envoi de la session de ticket, le titre définit une session et les attributs de lecteur qui le service utilise pour regrouper la session avec les autres sessions. Attributs peuvent être spécifiés au niveau du ticket ou au niveau d’un membre.


### <a name="setting-matchmaking-attributes-at-the-match-ticket-level"></a>Définition des attributs de Matchmaking au niveau du Ticket de correspondance

Le titre soumet des attributs au niveau du ticket de correspondance dans le paramètre ticketAttributesJson de la méthode MatchmakingService.CreateMatchTicketAsync. Attributs spécifiés au niveau du ticket remplacent les mêmes attributs spécifiés au niveau d’un membre.


#### <a name="setting-matchmaking-attributes-at-the-per-member-level"></a>Définition des attributs de Matchmaking au niveau d’un membre

Le titre spécifie les attributs d’un membre sur chaque membre au sein de la session de ticket de correspondance. Ces propriétés sont définies en appelant la méthode MultiplayerSession.SetCurrentUserMemberCustomPropertyJson, à l’aide d’un nom de propriété de « matchAttrs ». Cet appel place les attributs dans la base de données /members/ {index} / propriétés/personnalisé/matchAttrs champ sur chaque lecteur au sein de la session de ticket.

Le processus de matchmaking « aplanit » chaque membre par dans un seul attribut au niveau du ticket, selon la méthode de mise à plat spécifiée pour cet attribut dans la configuration de Xbox Live l’interface utilisateur pour le hopper. Cela peut être configuré sur [XDP](https://xdp.xboxlive.com) ou [partenaires](https://partner.microsoft.com/dashboard).


### <a name="making-the-match"></a>La correspondance

Avec la session de ticket et la correspondance ticket configuré, les correspondances de service matchmaking la session de ticket représenté avec les autres sessions de ticket représentant les autres groupes et crée ou identifie une session de cible de correspondance. Le service crée également des réservations pour les lecteurs de mise en correspondance dans la session cible et marque ensuite les sessions de ticket comme mis en correspondance. MPSD notifie ensuite le titre de cette modification à la session de ticket.

Maintenant le titre doit initialiser la session cible afin de confirmer que suffisamment de lecteurs sont affichées et effectuer la qualité de service (QoS) des vérifications pour vous assurer qu’ils puissent se connecter entre eux avec succès. Si l’initialisation et/ou de qualité de service échoue, le titre de la marque de nouveau soumis à matchmaking afin que vous pouvez trouver un autre groupe de la session de ticket. Pour plus d’informations sur ce processus, consultez **initialisation de la Session cible et QoS**.

Pendant l’activité de correspondance, les modifications suivantes sont apportées aux objets JSON pour la session :

-   champ de /Servers/Matchmaking/Properties/System/Status défini sur « trouvé ».
-   champ de /Servers/Matchmaking/Properties/System/targetSessionRef défini sur session cible.
-   /Members/ {index} / propriétés/custom/champ matchAttrs pour chaque session de ticket copiés vers les membres / {index} / constantes/custom/matchmakingResult/playerAttrs champ.
-   Pour chaque joueur, ticket attributs copiés à partir du champ ticketAttributes dans le ticket de correspondance dans /members/ {index} / constantes/custom/matchmakingResult/ticketAttrs champ.


### <a name="maintaining-the-match-ticket"></a>Maintenir le Ticket de correspondance

Le service utilise un instantané de la session de ticket à l’heure à laquelle le ticket de correspondance est créé pour la session. Ainsi, si des lecteurs sont rejoindre ou quitter la session de ticket, le titre doit utiliser le service à supprimer, puis recréer le ticket de correspondance.


### <a name="filling-spots-in-an-in-progress-game-session"></a>Remplissage des taches dans une Session de jeu en cours d’exécution

Un titre peut réutiliser une session de jeu existante en tant qu’une session de ticket de correspondance pour rechercher plus de joueurs pour joindre un jeu qui est déjà en cours ou une session de jeu une fois terminé un cycle de remplissage et certains lecteurs gauche. Ce processus est appelé « renvoi ».

Pour effectuer renvoi consiste à créer un ticket de correspondance qui sera « preserve » la session en cours d’exécution, en appelant **MatchmakingService.CreateMatchTicketAsync méthode** avec la valeur du paramètre preserveSession PreserveSessionMode.Always. Le service puis garantit que la session existante est conservée tout au long du processus de matchmaking et devient la session résultante cible.

Il existe des inconvénients potentiels de ce modèle, car deux sessions avec preserveSession toujours la valeur ne sera pas capable de faire correspondre avec eux dans la mesure où ils ne peuvent pas être combinés. Cela peut entraîner le renvoi très lent matchmaking. Pour éviter ce scénario, un titre doit utiliser uniquement les PreserveSessionMode.Always s’il requiert la préservation de l’état du jeu. Otherwis définir PreserveSessionMode.Never et migrez les lecteurs vers la nouveau targetSession lorsqu’une correspondance est trouvée.


### <a name="deleting-the-match-ticket"></a>Supprimer le Ticket de correspondance

Pour supprimer le ticket de correspondance le titre appelle **MatchmakingService.DeleteMatchTicketAsync méthode**. La suppression du ticket le service :

1.  Matchmaking s’arrête pour les utilisateurs dans la session de ticket.
2.  Mises à jour le champ /servers/matchmaking/properties/system/status dans la session de ticket à « annulée ».


### <a name="matchmaking-for-games-using-xbox-live-compute"></a>Matchmaking pour des jeux à l’aide de calcul en direct Xbox

L’exemple suivant montre l’équivalent de haut niveau à l’aide des services Live Compute. Une approche similaire peut appliquer aux jeux hébergés sur des ressources tierces.

1.  Le client « scout » crée une session de ticket pour représenter le groupe. Cette session contient une liste des centres de données potentielle, situés dans la configuration de session dans /constants/system/measurementServerAddresses. Il provient soit le modèle de session, si la liste de centre de données est statique, ou à partir du client lors de la création de session après sa réception à partir du service Live Compute. Cette session contient également des valeurs maxAllowedPlayers dans l’objet personnalisé/targetSessionConstants/gameServerPlatform gsiSetId et gameVariantId.
2.  Tous les autres clients dans le groupe de rejoignent la session de ticket.
3.  Tous les membres du groupe de téléchargement les valeurs measurementServerAddresses à partir de l’objet /constants/system pour la session de ticket. Chaque membre, puis un test ping sur chaque adresse à l’aide de l’API de la plateforme et télécharger une liste ordonnée des centres de données par défaut à la session, tel que défini dans /members/ {index} / propriétés/système/serverMeasurements.

| Remarque                                                                                                                                                                                                                                                                                                                                                                       |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Le titre peut définir et récupérer les valeurs de measurementServerAddresses à partir de la session à l’aide **MultiplayerSession.SetMeasurementServerAddresses méthode** et  **Propriété de MultiplayerSessionConstants.MeasurementServerAddressesJson**. |

4.  Les appels de client 'scout' **MatchmakingService.CreateMatchTicketAsync méthode**, en passant une référence à la session de ticket.

| Remarque                                                                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Si les objets de session de ticket ont des constantes ne correspondent pas, la méthode CreateMatchTicket peut échouer. Cela peut être évité en ajoutant indispensable de règle pour le hopper et empêcher la mise en correspondance des joueurs avec des constantes ne correspondent pas. |

5.  Si le **MatchTicketDetailsResponse.PreserveSession propriété** est définie sur jamais, le service copie les mesures de serveur à partir de chaque membre dans la représentation interne du ticket avant la mise à plat le collectées dans une collection unique pour le ticket et stockées sous la forme d’un attribut du ticket « spéciales ».
6.  Si **MatchTicketDetailsResponse.PreserveSession propriété** est défini sur toujours, le serveur de mesures ne sont pas utilisés. Au lieu de cela, le service copie la valeur de /properties/system/matchmaking/serverConnectionString pour la session dans la représentation interne du ticket, comme une collection de serverMeasurements de taille 1 avec une latence de zéro.
7.  Le service correspond à la session de ticket avec d’autres personnes représentant les autres groupes, en prenant le serveur de collections de mesure en compte. Il essaie de correspond au groupe avec d’autres groupes qui ont les mêmes centres de données préféré hautement.
8.  Une fois un groupe de mise en correspondance a été trouvé, le service crée ou identifie une session de la cible et ajoute tous les joueurs depuis les sessions de ticket sont mis en correspondance ensemble. Le service écrit les mesures de serveur aplati final pour le groupe de mise en correspondance dans /properties/system/serverConnectionStringCandidates. Il écrit ensuite les mesures de serveur d’origine pour chaque nouveau membre dans la session cible dans /members/ {index} / constantes/système/matchmakingResult/serverMeasurements.
9.  Tous les clients effectuent une initialisation sur la session cible, comme indiqué ci-dessus. Toutefois, étant donné que les clients doivent se connecter à Live Compute, ils n’effectuent pas de QoS avec eux pour vérifier la connectivité.
10. Certains ou tous les appels clients **GameServerPlatformService.AllocateClusterAsync méthode**. La première condition déclenche l’allocation, tandis que les autres reçoivent uniquement un accusé de réception. La méthode obtient la session cible à partir de MPSD et choisit un dataceter basé sur les préférences de centre de données dans la session cible, ainsi que d’une charge et d’autres connaissances spécifiques de calcul en direct.
11. Tous les clients jouent.

## <a name="configuring-smartmatch-for-your-title"></a>Configuration SmartMatch pour votre titre


### <a name="configuration-of-smartmatch-matchmaking-runtime-operations"></a>Configuration des opérations d’exécution SmartMatch Matchmaking

La configuration de SmartMatch matchmaking s’effectue via le [Xbox Developer Portal (XDP)](https://xdp.xboxlive.com) ou [partenaires](https://partner.microsoft.com/dashboard). Configuration utilise ServiceConfiguration -&gt;section multijoueurs & Matchmaking pour un titre.


#### <a name="matchmaking-session-template-configuration"></a>Configuration du modèle Matchmaking Session

Comme indiqué dans [SmartMatch Matchmaking](/windows/uwp/xbox-live/multiplayer/multiplayer-appendix/smartmatch-matchmaking), il existe deux types de session liée à matchmaking, de la session de ticket de correspondance et de la session de cible de correspondance. En fait, une session de ticket est l’entrée pour le service, tandis que la session cible est la sortie. Lorsque vous configurez des modèles de session, vous devez créer un modèle pour chaque type de session.

Pour une session de ticket, vous pouvez utiliser un modèle dédié. Vous pouvez également réutiliser un modèle pour une session d’introduction ou une autre session ne pas destinée à être utilisée pour le jeu.

| Important                                                                                      |
|-------------------------------------------------------------------------------------------------------------|
| La session de ticket ne doit pas avoir de contrôles de qualité de service sont activés et ne doit pas être marquée avec la fonctionnalité de « a ». |

Pour une session de la cible, vous devez utiliser un modèle qui est destiné aux jeux matchmade. Il doit avoir des paramètres qui permettent des vérifications de qualité de service entre homologues avant le début de jeu et doivent être marqués avec la fonctionnalité de « a ».

Avec la configuration de l’interface utilisateur pour XDP ou partenaires, vous pouvez mapper chaque session à un ou plusieurs exploits en, chaque conteneur règles qui déterminent la façon dont les sessions sont associées dans ce hopper. Pour plus d’informations, consultez base Configuration Hopper pour Matchmaking.


#### <a name="basic-hopper-configuration-for-matchmaking"></a>Configuration de base Hopper pour Matchmaking

Cette section définit les champs utilisés pour configurer les champs de base hopper. Une fois cette configuration, vous devez configurer les règles hopper, comme décrit dans la Configuration des règles de Hopper.

![](../../images/multiplayer/session_template_hopper_edit.png)


###### <a name="name"></a>Nom

Le nom de la hopper qui est utilisé lors de l’envoi d’une session sur matchmaking. Ce nom doit correspondre à la valeur passée en tant que paramètre à la **CreateMatchTicketAsync** méthode lors de la création du ticket de correspondance.


###### <a name="minmax-group-size"></a>Taille du groupe de Min/Max

Les tailles minimales et maximales pour le groupe de lecteur qui doit être créé à partir de sessions dans le hopper. Le service tente de créer un groupe de mise en correspondance est aussi grand que possible, jusqu'à la taille maximale de groupe. Toutefois, il crée un groupe de mise en correspondance si elle peut assembler suffisamment joueurs pour répondre à la taille minimale de groupe.


###### <a name="should-rule-expansion-cycles"></a>Doit règle Cycles d’Expansion

Pour un SHOULD de règle, le service tente d’augmenter l’espace de recherche et d’assouplir les règles de matchmaking fourni au fil du temps si aucune correspondance n’est trouvée. Ce processus est exécuté sur plusieurs cycles, comme spécifié à l’aide du champ doit les Cycles de règle d’extension. Lors du dernier cycle d’extension, les règles SHOULD sont supprimés afin qu’elles ne sont plus empêchent tickets correspondant. Toutefois, ils sont toujours utilisés pour déterminer la meilleure correspondance si plusieurs tickets sont disponibles. Seuls les types de qualité de service et de nombre sont développées avant qu’ils sont supprimés. Consultez également les règles de Configuration de Hopper dans cette rubrique.

Augmenter la valeur doit règle Explansion des Cycles de paramètre offre plus de cycles pour doit de règle d’extension, mais augmente également la durée de matchmaking. La valeur par défaut est 3, ce qui est généralement suffisant pour la plupart des configurations.

| Important                                                                                                                                                                        |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cycles d’expansion se produisent à intervalles de temps fixe de 5 secondes. Lors du dernier cycle d’expansion, toutes les règles de « Doit » sont ne sont plus prises en compte pour le reste de la tentative de matchmaking. |

##### <a name="ranked-hopper"></a>Hopper classé
En règle générale SmartMatch empêchera joueurs bloqués en cours de mise en correspondance. Si Hopper classés est activée, cette logique est ignorée pour empêcher que des lecteurs à l’aide de ce système afin d’éviter les joueurs de qualification supérieure.

#### <a name="configuration-of-hopper-rules"></a>Configuration des règles de Hopper

Cette section définit les champs utilisés pour configurer des règles pour un hopper.

###### <a name="common-rule-fields"></a>Champs de règle courants

Les champs définis dans cette section sont communes à toutes les règles hopper.

**Nom de la règle**

Le nom convivial affiché pour la règle à des fins de configuration.

**Type de règle**

Le type de règle. Les options sont doit et SHOULD. Règles doivent avoir à satisfaire pour matchmaking réussie. DOIVENT règles peuvent être moins stricte et/ou supprimées pour rechercher une correspondance réussie. Pour plus d’informations sur ce processus, consultez Configuration d’Expansion de règle doit.


**Type de données**

Le type de données de l’attribut de la règle de matchmaking. Les valeurs possibles sont :   Nombre. Spécifiez une valeur numérique de 32 bits simple.
-   Chaîne. Spécifiez une chaîne Unicode de 128 caractères.
-   collection. Spécifiez un tableau de chaînes. Utilisez cette valeur pour identifier le contenu téléchargeable (DLC), l’appartenance escouade ou préférence de rôle pour les joueurs.
-   Qualité de Service. Spécifiez un type de données personnalisé pour inclure la latence des données de qualité de service dans matchmaking. Qu’une telle règle doit être utilisée par les hopper matchmaking.

| Remarque |
|----------|
| Si cette limite peut être problématique pour votre titre, contactez votre responsable de compte de développeur. |

-   Valeur totale. Spécifiez un type de données personnalisé qui additionne les valeurs de matchmaking soumis. Vous pouvez utiliser cette valeur pour vous assurer que la somme résultante est dans une plage spécifique, soit une valeur exacte.
-   Équipe. Spécifiez un type de données personnalisé pour les équipes de lecteurs inclus dans les demandes de matchmaking. Vous pouvez utiliser cette valeur pour éviter de fractionner des lecteurs au sein d’un ticket de correspondance unique entre plusieurs équipes.


###### <a name="data-type-specific-rule-fields"></a>Champs de règle spécifique du Type de données

Cette section définit les champs utilisés pour définir des règles qui s’appliquent à certains types de données, mais pas à d’autres personnes. L’interface utilisateur doit être en mesure de préciser quelles données types s’appliquent aux règles particulières.

**Autorise les caractères génériques**

Une valeur qui indique si l’attribut peut être omis dans le ticket de correspondance. S’il est omis, le ticket est compatible avec n’importe quel autre ticket, quelle que soit la valeur de cet attribut.


**Attribut Source**

La source de la valeur de type de données. Les sources possibles sont :
- Titre fourni. La valeur de données est envoyée dans le ticket de correspondance.
-   Instance stat utilisateur. La valeur de données est automatiquement récupérée à partir du service UserStatistics.


**Nom de l’attribut**

Le nom de la source de valeur d’attribut. Il est le nom de propriété dans le ticket de correspondance ou le nom d’une statistique de l’utilisateur.


**Valeur par défaut**

La valeur par défaut pour le type de données, si aucune valeur n’est disponible pour la demande de matchmaking ou spécifié. La valeur par défaut n’est pas appliquée lorsque le champ Autoriser les caractères génériques est sélectionné et aucune valeur n’est spécifiée.


**Poids**

L’importance de la règle. Le poids peut être utilisé pour indiquer les règles qui sont prioritaires pendant l’expansion matchmaking et de la règle. La valeur de poids doit être positive et la valeur par défaut est 1.


**Flatten (méthode)**

Types de données numériques uniquement. Une valeur qui indique la façon dont plusieurs valeurs sont combinées afin de satisfaire une correspondance. Il s’applique à plusieurs valeurs pour les joueurs dans un ticket de correspondance unique et entre plusieurs tickets. Les valeurs possibles sont :
-  Min/Max. Utilisez la valeur minimale ou maximale de plusieurs valeurs à partir de tickets de match différents.
-   Moyenne. Utilisez la valeur moyenne de plusieurs valeurs à partir de tickets de match différents.


**Max Diff**

Types de données numériques uniquement. La différence maximale acceptable numérique entre deux valeurs comparées pour répondre à une règle. Pour un SHOULD de règle, cette valeur est le point de départ pour l’extension de la règle.


**Opération de définition**

Types de collection données uniquement. L’opération à effectuer sur le groupe de valeurs de jeu de mise en correspondance. Les options possibles sont :
- Intersection. Mettre en correspondance deux collections basées sur la quantité d’intersection entre eux. Ce paramètre entraîne des valeurs de la collection similaires ou identiques.
-   Différence. Mettre en correspondance deux collections en fonction du montant de différence entre eux.
-   Préférence de rôle. Mettre en correspondance les regroupements basés sur les préférences pour le rôle d’un lecteur dans les modes de jeu en fonction du rôle.


**Intersection de la cible**

Partie de la configuration d’une opération Set. L’intersection minimale ou la différence maximale pour les deux collections avant qu’ils sont mis en correspondance.


**Topologie de réseau**

Qualité de service du type de données uniquement. La topologie du réseau qui est utilisée pour la qualité de service. Les valeurs possibles sont pair à pair, homologue de l’hôte et Client/serveur.


**Maximum maximale latence/mise à l’échelle**  
Qualité de service du type de données uniquement. Latence maximale de matchmaking réussie au sein de la topologie du réseau spécifié. Cette valeur est traitée comme une valeur mise à l’échelle (par opposition à une latence requise) à l’aide d’un Client-serveur de qualité de service doit règle when.


| Remarque                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| En outre, les règles de réputation par défaut sont également appliquées à un hopper. Ces règles ne peuvent pas être supprimés et sont utilisés pour garantir une gestion correcte de réputation pendant matchmaking. |


**Autoriser en attente pour les rôles**

Le type de données de préférences de rôle de collection uniquement. Spécifie si le service de correspondance conserve le ticket matchmaking afin de remplir tous les rôles disponibles.


###### <a name="expansion-delta"></a>Delta d’expansion

Valeur qui indique la quantité pour assouplir la règle soumise pour chaque génération d’extension. Le delta d’extension est appliqué en plus de la valeur Max Diff. Pour plus d’informations, voir exemple 1 (règle d’extension).

Vous pouvez également utiliser le delta d’extension pour développer plusieurs valeurs numériques à des vitesses différentes. Cela n’est pas possible via l’expansion cycle de paramètre de configuration, car il s’applique à toutes les règles. Au lieu de cela, l’approche consiste à utiliser les valeurs d’extension décimal, par exemple, 0.4. Une expansion se produit uniquement lorsqu’un nouveau entier est atteinte, ce qui permet des vitesses différentes d’extension, même pour le même nombre de cycles d’expansion.


###### <a name="qos-expansion-peer-to-peer-peer-to-host"></a>Expansion de qualité de service (Peer-to-Peer, pair-à-Host)
Pour une qualité d’expansion de type de service pour les jeux d’homologue, le delta d’extension ne peut pas être configuré. Au lieu de cela, vous devez utiliser une des méthodes d’extension suivantes.

<table>

<tr>

<tr>
<td>
<b>1. MaxLatency < 256.</b><p/><p/>

Expansion est effectuée au MaxLatency * Cycle d’Expansion.<p/>
Par exemple, si la valeur initiale est 200, 200 est utilisé dans le premier cycle, 400 dans le deuxième cycle et ainsi de suite.
</td>
</tr>

<tr>
<td>
<b>2. MaxLatency > ou égale à 256</b><p/><p/>

Expansion linéairement met à l’échelle de 50 à MaxLatency - 256.<p/>
Par exemple, si la valeur initiale est 556, la valeur linéairement de 50 à 300 par rapport au nombre de cycles. Autrement dit, si six cycles ont été choisis, puis les valeurs serait 50, 100, 150, 200, 250 et 300. Si cinq des cycles ont été choisis, les valeurs serait 50 112,5, 175, 237.5 et 300.
</td>
</tr>

</tr>
</table>

##### <a name="qos-expansion-client-server"></a>Expansion de qualité de service (Client-serveur)
Lorsque vous utilisez des serveurs dédiés, l’expansion est basée sur les préférences relatives. Uniquement préféré seront considérée comme serveurs au début des cycles d’extension. Au fil du temps, des autres, moins de serveurs par défaut sera utilisé. Pour vous assurer d’extension appropriée, nous avons besoin d’une valeur semblable à MaxLatency, appelée mise à l’échelle maximale. Cette mise à l’échelle maximale doit toujours indiquer le plus grand temps ping acceptable, toutefois, cette valeur donne une échelle relative pour les heures de ping autre serveur fournis par un lecteur, au lieu de fournir une exigence absolue pour les commandes ping. Notez que vous pourrez exclure volontairement les serveurs avec des commandes ping inacceptable en les supprimant de la liste dans la demande.

#### <a name="example-1-rule-expansion"></a>Exemple 1 (Expansion des règle)

Au niveau du lecteur est utilisé pour matchmaking et lecteurs sont mises en correspondance faiblement, selon l’étroitesse de leurs niveaux. Lecteurs avec le moins de différence entre leurs niveaux sont préférables.

-   Règle de niveau de lecteur
-   Type de règle : DOIT
-   Type de données : Numéro
-   Max Diff : 1
-   Expansion Delta : 2
-   Flatten (méthode) : Moyenne

Par défaut, la différence requise entre les niveaux de lecteur est inférieur ou égal à 1. Si une correspondance est trouvée au sein de cette différence, les lecteurs sont mises en correspondance. Si aucune correspondance initiale est trouvée, la valeur du niveau le lecteur est développée par 2 pour chaque itération (trois itérations par défaut). Ce scénario entraîne un comportement équivalent d’un lecteur au niveau 20, comme indiqué dans le tableau ci-dessous.

| Étape                    | Valeur de niveau pour les candidats potentiels de correspondance | Distance de niveau efficace de correspondance |
|-----|
| Initial soumis valeur | 19-21                                      | 1                                             |
| Cycle d’extension 1       | 17-23                                      | 3                                             |
| Cycle d’extension 2       | 15-25                                      | 5                                             |
| Cycle d’extension 3       | 13-27                                      | 7                                             |

À mesure que les cycles d’extension, la distance de niveau efficace pour une correspondance réussie augmente sans modifier la valeur Max Diff. Seule la valeur de niveau de lecteur est assouplie.


#### <a name="example-2-collection-rule"></a>Exemple 2 (règle de collecte)

Le jeu libère les trois types de DLC qui sont disponibles pour les joueurs. Cette règle matchmaking est appliquée à matchmaking du jeu « DLC uniquement », et un lecteur doit posséder au moins un DLC pour être matchmade avec d’autres joueurs.

-   Règle de joueur DLC
-   Type de règle : DOIT
-   Type de données : Collection
-   Opération de définition : Intersection
-   Intersection de la cible : 1

Lecteurs évaluent leurs DLCs et envoyez les valeurs indiquées dans le tableau suivant dans leurs tickets de correspondance.

| Remarque                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Dans le tableau ci-dessous, la valeur de la collection indique la propriété de la DLC. Si le DLC est disponible pour le lecteur et la valeur 0 dans le cas contraire, elle est définie sur 1. |

| Lecteur   | Valeur de la collection | Mise en correspondance avec des lecteurs | Remarque           |
|----------|------------------|----------------------|----------------|
| Lecteur 1 | { "1", "1", "1"} | 3                    | Propriétaire de tous les DLCs  |
| Le lecteur 2 | { "0", "0", "0"} | Aucune                 | Ne possède aucun DLC    |
| Le lecteur 3 | { "1", "0", "0"} | 1                    | Possède la première DLC |

Si l’intersection de la cible dans l’exemple est définie sur 2, lecteurs 1 et 3 ne seront pas trouvés, étant donné que l’intersection entre eux est seulement de 1.

#### <a name="example-3-avoid-previous-players"></a>Exemple 3 (évitez des lecteurs précédentes)
Le titre préfère éviter un jeu avec le lecteur récemment utilisés.
* Type de règle : DOIT
* Type de données : Collection
* Opération de définition : Différence
* Intersection de la cible : 0

## <a name="defining-team-rules-during-smartmatch-configuration"></a>Définition des règles de l’équipe lors de la configuration de SmartMatch

### <a name="configuring-team-rules"></a>Configuration des règles de l’équipe

Pour configurer la règle de l’équipe, commencez par créer un dans sur votre plateforme de configuration choisie (XDP ou partenaires). Remplissez les tailles d’équipe que votre jeu s’attend à créer à partir des tickets mis en correspondance dans cette hopper. Par exemple, si votre jeu attend 4v4, vous devez créer deux entrées, attend une taille maximale de 4 chaque et un autre nom. Il existe un minimum taille de l’équipe ainsi--Utilisez cette option si un jeu peut être lu avec des lecteurs moins d’une équipe. Sinon, le minimum et maximum doivent être la même valeur.


#### <a name="using-team-rules"></a>À l’aide de règles de l’équipe

Une fois que la règle d’équipe est configurée, les tickets dans le hopper ne pourra correspondant s’il n’existe aucun moyen pour tenir leurs groupes dans teams sans provoque un fractionnement. La règle écrit l’allocation de l’équipe qui en résulte dans la session cible, sous membres/constantes/custom/matchmakingresult/initialTeam. Notez qu’il s’agit d’une allocation suggérée simplement--le titre peut trouver que, en réorganisant les joueurs cela peut créer un meilleur jeu, tout en évitant les tickets du fractionnement dans différentes équipes.

Si un ticket est créé pour un jeu qui est en cours d’exécution, la règle d’équipe requiert des informations supplémentaires. Par exemple, supposons que 8 joueurs se trouvent dans une liste de joueurs jeu, lorsque deux 4v4 ou sont déconnectés. Le titre souhaitons vous renseignez ces emplacements vides, mais il ne peut pas réorganiser les équipes pendant la lecture du jeu.

Tente de remplir des jeux en cours d’exécution est représentées par les tickets avec le champ PreserveSession défini sur « toujours ». Dans ce cas, étant donné que les équipes ont déjà été allouées aux lecteurs, le titre devez spécifier l’allocation d’équipe actuel, afin que correspondance connaîtra le nombre d’espaces est ouverts sur chaque équipe. Pour fournir les noms des équipes de chaque lecteur est activé, chaque joueur écrit leur nom d’équipe dans la session de jeu, sous membres/me/propriétés/système/groupes. Ce champ est un JArray.

Une fois que les propriétés ci-dessus sont écrits dans la session de jeu, un seul lecteur crée un ticket pour la session, dans une tentative pour trouver plus de joueurs. Lorsque le ticket est rempli, correspondance à nouveau écrira l’équipe suggérée de n’importe quel joueurs qui rejoignent dans membres/constantes/custom/matchmakingresult/initialTeam


###### <a name="preferring-even-teams"></a>Équipes mêmes utilisation préférentielle

En outre, les correspondances seront effectuées avec les équipes plus grandes tout d’abord. Cela signifie que dans un hopper 4v4 hypothétique, tickets de 4 joueurs répondra ensemble tout d’abord, jusqu'à ce qu’aucun ticket de 4 ne reste. Tickets de 3 puis se poursuivre, extraction de singletons comme il faut et ainsi de suite. De cette manière, les tickets de taille similaire généralement jouera par rapport aux autres, s’ils sont présents et pas empêchée par d’autres règles. Notez que cela donne la priorité relativement forte équipe règle sur d’autres règles. Par exemple, supposons que vous avez un remplissage limité, consistant en un ticket de taille 4 avec skill(A) élevé, un ticket de taille 4 avec une faible skill(B) et quatre tickets de taille 1 avec skill(C-F) haute. La règle de l’équipe entraîne la correspondance de préférer la correspondance A et B, par opposition à A, C, D, E et F.


###### <a name="should-variant"></a>Doit Variant

Dans la doivent règle empêche le fractionnement dans toutes les générations de ticket, ainsi que le tri prefer même les équipes. Le doit règle est identique jusqu'à ce que la dernière génération--une fois, les tickets peuvent être fractionnés, bien que le tri des équipes même prefer sera toujours actif.

## <a name="target-session-initialization-and-qos"></a>Initialisation de la Session cible et la qualité de service

Un groupe de lecteurs est mis en correspondance dans une session de la cible par SmartMatch matchmaking, comme décrit dans [SmartMatch Matchmaking](/windows/uwp/xbox-live/multiplayer/multiplayer-appendix/smartmatch-matchmaking). Le titre des étapes pour confirmer que suffisamment joueurs ont rejoint qu’ils puissent se connecter entre eux si nécessaire. Ce processus est appelé d’initialisation de la session cible.

Pour les jeux à l’aide de topologies de réseau pair à pair, un aspect important de l’initialisation de la session cible est d’évaluation et de mesure de qualité de service. Opérations associées sont la mesure de la latence et la bande passante entre les consoles Xbox One (ou entre les consoles et les serveurs) et l’évaluation des mesures qui en résulte pour déterminer si la connexion réseau entre les nœuds est bonne.

L’organigramme suivant illustre comment implémenter l’initialisation de la session cible et les opérations de qualité de service.

![](../../images/multiplayer/Multiplayer_2015_Matchmaking_QoS.png)

### <a name="managed-initialization"></a>Initialisation managée

MPSD prend en charge une fonctionnalité appelée « géré par l’initialisation » par le biais duquel il coordonne le processus d’initialisation de session cible les clients impliqués dans une session. MPSD automatiquement suit les étapes d’initialisation et les délais d’expiration associé pour la session et évalue la connectivité entre les clients si nécessaire. L’initialisation managée est représentée par le **MultiplayerManagedInitialization classe**.

| Remarque                                                                                                                  |
|------------------------------------------------------------------------------------------------------------------------------------|
| Il est recommandé pour votre titre utiliser SmartMatch matchmaking pour tirer parti de la MPSD gérés fonctionnalité de l’initialisation. |


#### <a name="managed-initialization-episodes-and-stages"></a>Épisodes de l’initialisation managée et étapes

Une session de cible subit l’initialisation managée tout moment matchmaking ajoute de nouveaux participants à la session. SmartMatch ajoute des membres de la session en tant qu’état d’utilisateur réservé, ce qui signifie que chaque membre occupe un emplacement, mais n’a pas encore rejoint la session. Chaque groupe de nouveaux lecteurs déclenche un nouvel épisode de l’initialisation.

À l’achèvement de l’initialisation, chaque joueur réussit ou échoue le processus. Un joueur qui réussit à l’initialisation peut diffuser à l’aide de la session cible. Un lecteur qui échoue doit être renvoyé au matchmaking à mettre en correspondance dans une autre session. Pour les cas dans lesquels une session est envoyée à matchmaking avec le *preserveSession* paramètre la valeur Always, les membres existants de la session ne subissent pas l’initialisation, comme MPSD suppose leur être correctement configuré.

Chaque épisode de l’initialisation managée se compose de ces étapes :

-   Jointure--session membres écrire eux-mêmes à la session pour déplacer leur état de l’utilisateur de réservé à l’état actif et charger des données de base, telles que de l’adresse de l’appareil sécurisé.
-   --Pour les topologies homologue, membres de la session mesurent la qualité de service entre eux et téléchargement les résultats dans la session.
-   Évaluation--MPSD évalue les résultats des deux dernières étapes et détermine si la session et/ou de membres ont été initialisé avec succès.

Le code de titre opère sur la session pour passer chaque utilisateur (et par conséquent, la session) dans les phases de jointure et de mesure. Il puis peut lire ou revenez en arrière pour matchmaking après que l’étape d’évaluation a réussi ou échoué.


### <a name="configuring-the-target-session-for-initialization"></a>Configuration de la Session de cible pour l’initialisation

Le titre peut configurer le processus de l’initialisation managée à l’aide de constantes dans la session cible en cours d’initialisation. Ces constantes sont définies sous /constants/system dans le modèle de session avec la version 107, la version du modèle recommandé. Deux types de paramètres de configuration peuvent être effectués : paramètres qui configurent le processus d’initialisation géré dans son ensemble et configurer les exigences de qualité de service. Consultez [MPSD Session modèles](multiplayer-session-directory.md) pour obtenir des exemples de modèles de session pour les scénarios courants de titre.

| Remarque                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------|
| La phase de mesure pendant l’initialisation est ignorée si les exigences de qualité de service ne sont pas définis dans la configuration de l’initialisation de session cible. |


#### <a name="configuring-managed-initialization-as-a-whole"></a>Configuration gérés d’initialisation dans sa globalité

Voici les champs à définir pour contrôler l’initialisation managée globale. Ils font partie de l’objet /constants/system/memberInitialization :
- joinTimeout. Spécifie la durée d’attente MPSD pour chaque membre se joindre, après que l’épisode de l’initialisation a commencé. Valeur par défaut est 10 secondes.
-   measurementTimeout. Spécifie la durée d’attente MPSD pour chaque membre télécharger les résultats de mesure de qualité de service, après que la phase de mesure a commencé. Valeur par défaut est secondes 30000.
-   membersNeededToStart. Spécifie le nombre de membres qui doivent réussir lors de l’initialisation pour le premier épisode de l’initialisation réussisse. Valeur par défaut est 1.

| Remarque                                              |
|----------------------------------------------------------------|
| Si ce seuil n’est pas remplie, tous les membres de l’échouent de l’initialisation. |


#### <a name="configuring-qos-requirements"></a>Configuration requise de QoS

Qualité de service est nécessaire uniquement lors de l’initialisation si le titre utilise une topologie peer-to-peer ou hôte de l’homologue. Chaque topologie est mappé à une constante de topologie spécifique sous /constants/system /.
###### <a name="configuring-qos-requirements-for-peer-to-peer-topology"></a>Configuration requise de QoS pour la topologie d’égal à égal

| Remarque                                                                                                                                                                                         |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Il est rare pour les titres rendre les paramètres d’exigence de qualité de service pour la topologie d’égal à égal. Ces paramètres sont très restrictifs et entraînent des problèmes pour les joueurs avec traductions d’adresses réseau strict (NAT). |

Exigences de qualité de service de topologie d’égal à égal sont définies dans l’objet peerToPeerRequirements. Chaque client doit être en mesure de se connecter à tous les autres clients. L’objet a les champs pertinents suivants :
-   latencyMaximum. Spécifie la latence maximale entre deux clients.
-   bandwidthMinimum. Spécifie la bande passante minimale entre les deux clients.


###### <a name="configuring-qos-requirements-for-peer-to-host-topology"></a>Configuration requise de QoS pour la topologie d’égal à hôte

Configuration requise de QOS de topologie d’égal à l’hôte est définies dans l’objet peerToHostRequirements. Chaque client doit être en mesure de se connecter à un seul hôte commun. Si cet objet est configuré et que l’initialisation a abouti, MPSD créera une liste initiale des clients qui sont des ordinateurs hôtes potentiels, appelés les candidats « hôte ». Voici les champs à définir :
-   latencyMaximum. Spécifie la latence maximale entre chaque homologue et l’hôte.
-   bandwidthDownMinimum. Spécifie la bande passante minimale en aval entre chaque homologue et l’hôte.
-   bandwidthUpMinimum. Spécifie la bande passante en amont minimale entre chaque homologue et l’hôte.
-   hostSelectionMetric. Spécifie la mesure utilisée pour sélectionner l’ordinateur hôte.

## <a name="see-also"></a>Voir également

[Modèles de Session MPSD](multiplayer-session-directory.md)

[Opérations d’exécution de SmartMatch](/windows/uwp/xbox-live/multiplayer/multiplayer-appendix/smartmatch-matchmaking)
