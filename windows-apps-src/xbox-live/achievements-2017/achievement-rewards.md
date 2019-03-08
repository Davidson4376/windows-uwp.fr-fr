---
title: Récompenses des succès
description: Décrit comment vous pouvez configurer une prime pour remettre des récompenses.
ms.assetid: b6fc5bdb-ba7b-4687-985e-894182f066da
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, réalisation, récompenses
ms.localizationpriority: medium
ms.openlocfilehash: 0fbbcb06a2aba927301cf982d09fdb55192ec84c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617694"
---
# <a name="achievement-rewards"></a>Récompenses des succès

Le diagramme suivant illustre comment un développeur peut gérer le cycle de vie d’un titre. Le nouveau système de prime est conçu pour élever notre garagiste familiarisé avec beaucoup plus de souplesse, dans la façon dont les primes sont déverrouillées, comment et quand ils sont ajoutés et quels sont les avantages, elles fournissent aux utilisateurs, qui permet au développeur du titre de la série en tant que service par Ajout de valeur et la maintenance de l’intérêt des utilisateurs au fil du temps.

##### <a name="figure-1---how-a-title-might-drive-user-behavior"></a>Figure 1.   Comment un titre peut lecteur comportement de l’utilisateur. #####
![rewarding_achievements](../images/omega/achievements_overview_01_drive_behavior.png)

### <a name="flexible-options-for-rewarding-achievement"></a>Options flexibles pour récompenser prime ###
Avec Xbox One, nous avons étendu le système de réalisation pour prendre en charge des options plus flexibles de récompense. Score de joueur reste comme récompense précieuse qui assure le suivi d’un score de jeu unique et commun pour l’utilisateur sur l’écosystème Xbox Live, mais désormais vous, le développeur ou le serveur de publication : primes en servir comme un mécanisme de distribution pour une plage beaucoup plus large de récompenses, à la fois dans le titre de votre et en dehors de votre titre.

Une réalisation peut être configurée avec plusieurs récompenses, jusqu'à une récompense de chaque type de récompense. Une prime peut également être configurée avec aucun reward explicite ; Dans ce cas, icône de la réalisation agit comme un badge visual pour le lecteur qui a acquis la prime.

Xbox Live prend en charge les types de récompenses suivants :

* Score de joueur
* Art
* Récompenses dans l’application

#### <a name="gamerscore"></a>Score de joueur ####
Nous nous engageons à préserver l’intégrité de la valeur du score de joueur qui a été conçue avec nos utilisateurs Xbox Live. Il est uniquement un score de joueur par utilisateur ! N’importe quel score de joueur à un utilisateur gagne sur des plateformes de Xbox Live existantes telles que Xbox 360, Xbox One ou Windows 10 est comptabilisée dans un score de joueur unique pour cet utilisateur.

Quand un utilisateur déverrouille une prime en score de joueur, Xbox Live augmente automatiquement score de joueur de l’utilisateur par le montant configuré.

Il existe des restrictions sur lequel les titres peuvent offrir gamescore comme récompense sur leurs réussites. Consultez les documents de stratégie sur https://developer.xboxlive.com/ pour plus d’informations.

#### <a name="art"></a>Art ####
Vous avez des clipart concept intéressant que vos concepteurs avez dessinées au début de la phase de lancement de votre titre ? Vous disposez de belles images haute résolution qui peuvent décorer votre application de hub lorsque vous visitez de joueurs ? Peut-être votre application prend en charge plusieurs apparences ? Avec la récompense Art, vous pouvez mettre vidange, belles expériences dans vos titres et versions ultérieures vos joueurs doivent gagner.

Haute résolution concept art, dessins anticipée, créée spécialement des composants graphiques et autres ressources d’images numériques peuvent être proposés comme récompense aux utilisateurs pour le déverrouillage d’une prime. Ces ressources peuvent être affichées dans le tableau de bord Xbox rencontrer et peut être affichée dans le Guide des expériences simplement en interrogeant le service de primes pour récupérer les métadonnées pertinentes.

#### <a name="in-app-rewards"></a>Récompenses dans l’application ####
Nous vous présentons les récompenses dans l’application afin de permettre aux développeurs de beaucoup plus de souplesse et de contrôle sur les récompenses qui offre une prime. Dans l’application récompenses pouvoir utiliser primes pour fournir des récompenses dans le jeu personnalisés directement à vos utilisateurs sans nécessairement mettre à jour le titre de votre. Vous configurez simplement la récompense de réalisation avec un code, ID ou une expression qui reconnaît votre titre, et lorsque l’utilisateur déverrouille la réalisation, Xbox LIVE enverra ce code vers votre titre, informant ainsi le titre de la récompense pour fournir à l’utilisateur.

La récompense lui-même vous revient, le développeur. Idées de récompense sont les suivantes :

* Devise de jeu supplémentaires ou des points
* Accès à un caractère spécial, une arme ou une carte
* Un multiplicateur expérience temporaire.

### <a name="configuring-in-app-rewards"></a>Configuration récompenses dans l’application ###
La configuration d’une récompense dans l’application pour une prime est assez simple. Le propriétaire de la prime doit fournir un reward nom, une description de la récompense et une icône de récompense en plus d’une valeur de récompense. Cette valeur de la récompense est déterminée par le développeur et doit être autre chose que soit le jeu peut interpréter et traiter correctement ou que l’utilisateur peut entrer dans le cadre d’une expérience d’échange de récompense de titre spécifique.

Un exemple d’une valeur de récompense capable d’interpréter le jeu peut être un nombre à 5 chiffres ou une spéciale de chaîne qui le jeu ou service du jeu connaît des mappages à un élément dans le jeu particulier. Les développeurs veulent peuvent tirer parti du service de titre gérés stockage (TMS) pour le rendre facile d’ajouter de nouvelles valeurs de récompense au fil du temps le jeu comprendrez comment lire.

Un exemple d’une valeur de récompenser l’utilisateur doit envoyer peut être un code spécial ou une chaîne entrée par l’utilisateur une expérience d’échange dans le titre, au sein d’une application Compagnon, ou sur le site Web des développeurs.

### <a name="redeeming-in-app-rewards"></a>Échange les récompenses dans l’application ###
Une récompense dans l’application en vigueur lorsque l’utilisateur utilise la récompense dans le jeu. Titres doivent être conscients qu’un utilisateur a déverrouillé une prime est configurée avec une récompense dans l’application pour permettre le titre remettre correctement l’avantage lié à l’utilisateur. Pour ce faire, titres doivent effectuer le des opérations suivantes :

1. Interroger le service de primes lors de lancement de titre ou à la reprise de titre de suspension pour voir les primes déverrouillés sur lesquels les récompenses dans l’application et pour obtenir le code de récompense pour chacun. Cela doit toujours être fait pour vous assurer que vous interceptez les primes ont peut-être été déverrouillés tandis que le titre n’était pas en cours d’exécution ou sur une autre console.  

    Pour interroger, vous pouvez utiliser les URI de primes RESTful ou les API dans le Namespace Microsoft.Xbox.Services.Achievements.

2. Inscrivez-vous pour recevoir une notification lorsque l’une de vos primes est déverrouillée. Cela est facultatif, s’il est probablement souhaitable pour la plupart des titres. Notez que les titres ne recevra cette notification si le titre est réellement en cours d’exécution lorsque le déverrouillage se produit. Il s’agit d’une autre raison pour laquelle l’étape précédente important.

   Pour vous inscrire à une notification de succès, utilisez AchievementNotifier.GetTitleIdFilteredSource (méthode). Cette rubrique inclut un exemple de code qui illustre l’enregistrement.
