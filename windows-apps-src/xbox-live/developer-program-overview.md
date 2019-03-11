---
title: Vue d’ensemble du programme pour les développeurs
description: En savoir plus sur les programmes de développement différents disponibles pour utiliser Xbox Live.
ms.assetid: 1166308a-4079-41b4-8550-ce04b82b4f72
ms.date: 05/30/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox un, programme pour développeurs, créateurs
ms.localizationpriority: medium
ms.openlocfilehash: 05abd3f28328f4418f5a8a772049b3869b488ffc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603884"
---
# <a name="developer-program-overview"></a>Vue d’ensemble du programme pour les développeurs

Si vous souhaitez développer des titres de Xbox Live est activé, plusieurs options sont disponibles pour vous. Chaque offre différents niveaux d’investissement de temps de votre part, les fonctionnalités disponibles pour vous et les options de support.

## <a name="xbox-live-creators-program"></a>Programme Créateurs Xbox Live

Le programme Xbox Live Creators est un bon point de départ pour Xbox Live, si vous cherchez pour vous familiariser avec le développement Xbox Live. Aucun processus d’approbation à partir de Microsoft n’est requis participer au programme, et il existe une certification minimale en matière de publication.

Le programme Xbox Live Creators prend uniquement en charge la création de titres pour le [plateforme Windows universelle](https://msdn.microsoft.com/en-us/windows/uwp/get-started/universal-application-platform-guide) (UWP).  Ces titres créés en tant que jeux UWP s’exécutent sur des PC Windows 10 et consoles Xbox One.  Pour plus d’informations sur l’exécution des jeux UWP sur Xbox One, consultez [UWP sur Xbox One](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/index).  

Sur Xbox One, qui propose des joueurs une expérience organisée, les jeux publiés via le programme Xbox Live Creators seront vendus dans la nouvelle section de la Collection de créateurs du Microsoft Store sur Xbox. Cela offre un équilibre entre une plateforme ouverte, où tout le monde permet de développer et de livrer un jeu et une console d’expérience store organisé les joueurs ont appris à connaître et attendent. Sur Windows 10, votre titre paraîtra parmi tous les autres jeux Xbox Live dans le Microsoft Store.

### <a name="publishing-and-certification"></a>Publication et la certification
Vous devez être inscrit dans le [programme pour développeurs partenaires](https://developer.microsoft.com/store/register) pour libérer une partie dans le cadre du programme Xbox Live Creators. Il existe deux ensembles d’exigences que votre jeu doit suivre :

1. Intégrez Xbox Live connectez-vous et afficher l’identité de l’utilisateur (Gamertag, Gamerpic, etc.). Tous les autres services Xbox Live sont facultatifs.
2. Procédez à la norme [Microsoft Store stratégies](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx).

### <a name="supported-xbox-live-services"></a>Services pris en charge de Xbox Live
Titres activés sous le programme Xbox Live Creators peuvent utiliser Leaderboards, statistiques proposées, titre de stockage, stockage connecté et un ensemble limité de fonctionnalités de réseau social. Primes, mode multijoueur en ligne et les nombreuses fonctionnalités de réseau social sont **pas** pris en charge pour les titres de programme Xbox Live Creators.

Pour obtenir une liste complète des services pris en charge, consultez le [fonctionnalité Table](#feature-table).

### <a name="supported-third-party-game-development-engines"></a>Moteurs de développement de jeux tiers pris en charge
Les titres de programme Xbox Live Creators sont des jeux, UWP qui peuvent être générés avec un nombre de moteurs de jeux populaires. Microsoft fournit une documentation permettant d’intégrer les services Xbox Live à UWP jeux créés avec le [moteur de jeu Unity](https://unity.com). Vous pouvez trouver [documentation](get-started-with-creators/develop-creators-title-with-unity.md) détaillant l’intégration de Xbox Live avec des jeux Unity ici sur ce site, ainsi que télécharger et en savoir plus sur le Microsoft intégré [plug-in de Xbox Live Unity](https://github.com/Microsoft/xbox-live-unity-plugin).

Les titres de programme Xbox Live Creators peuvent également être générés avec les moteurs de jeu [construction (2 et 3)](https://www.scirra.com/construct2), et [GameMaker Studio 2](https://www.yoyogames.com/gamemaker). Les deux moteurs de jeux ont prise en charge Xbox Live, toutefois, que la prise en charge est gérée par le jeu moteurs créateurs et non par Microsoft. Pour plus d’informations sur la prise en charge pour l’ajout de Xbox Live à votre projet de construction ou GameMaker Studio 2, vous devez consulter la documentation de chaque moteurs de jeu respectivement.

[Apprenez à intégrer la Xbox Live dans votre projet de construction.](https://www.scirra.com/tutorials/9540/using-xbox-live-in-uwp-apps)

[Apprenez à intégrer la Xbox Live dans votre projet GameMaker Studio 2.](https://www.yoyogames.com/gamemaker/xblc)

Pour les autres moteurs de développement de jeux, tels que [MonoGame](https://www.monogame.net/) ou [Xenko](https://xenko.com/), que ne pas avoir intégrées fonctionnalités Xbox Live ou un plug-in, vous pouvez toujours utiliser API Xbox Live pour ajouter la Xbox Live à votre titre. Pour utiliser l'API Xbox Live de votre projet, vous pouvez soit ajouter des références aux fichiers binaires avec des packages NuGet ou ajouter la source de l’API. L'ajout de packages NuGet rend la compilation plus rapide, alors que l’ajout de la source facilite le débogage.

### <a name="support-and-feedback"></a>Support et commentaires
Réponses à des questions que vous pouvez avoir sur le [Forums MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev).  Vous pouvez également demander de se poser les questions de programmation [Stack Overflow](https://stackoverflow.com/questions/tagged/xbox-live) à l’aide de la balise « xbox live ».  L’équipe de Xbox Live s'implique dans la communauté et améliore en permanence nos API, outils et documentations en fonction des commentaires reçus ici.

Pour les développeurs dans le cadre du programme Xbox Live Creators, vous pouvez [soumettre une nouvelle idée](https://xbox.uservoice.com/forums/363186--new-ideas?category_id=196261) ou [voter sur une idée existante](https://xbox.uservoice.com/forums/251649?category_id=210838) à notre [Xbox User Voice](https://xbox.uservoice.com/forums/363186--new-ideas).

## <a name="idxbox"></a>ID@Xbox

Le programme Xbox Live Creators est très utile pour un grand nombre de jeux et les développeurs. Mais si vous souhaitez accéder à la pile complète Xbox Live, y compris le mode multijoueur en ligne, des primes et score de joueur, ou si vous souhaitez accéder à toute la puissance de la Xbox One gamme de périphériques à l’aide de kits de développement de matériel, le [ ID@Xbox ](https://www.xbox.com/en-US/developers/id) programme est pour vous.

Jeux dans le ID@Xbox programme doit être approuvée de concept et parcourez certification complet sur Xbox One et Windows 10, qui est un durée supérieure d’engagement de votre part.
ID@Xbox titres obtient placement dans la section principale du Store, par rapport à la Collection de créateurs, ce qui peut permettre une plus grande exposition aux clients.

Les développeurs dans le ID@Xbox programme également accéder à prise en charge du développement et d’assistance promotionnelle à partir de Microsoft, ainsi que le complément complet de livres blancs privés et développeur forums techniques. Vous pouvez continuer à utiliser [Forums MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev) ou poser des questions relatives de programmation sur [Stack Overflow](https://stackoverflow.com/questions/tagged/xbox-live) à l’aide de la balise « xbox live » si vous le souhaitez.

## <a name="microsoft-partners"></a>Partenaires Microsoft

Les développeurs qui travaillent avec un éditeur de jeu est un Microsoft Partner ont accès à l’ensemble complet des fonctionnalités Xbox Live et dédié représentants Microsoft pour faciliter votre développement, la certification et le processus de publication.

## <a name="feature-table"></a>Tableau des fonctionnalités

Le tableau ci-dessous illustre les fonctionnalités disponibles pour le programme Xbox Live Creators, et [ ID@Xbox ](https://www.xbox.com/en-US/developers/id) programmes.  

<table>

<tr>
<th>Zone fonctionnelle</th>
<th>Fonctionnalité</th>
<th>Description</th>
<th> ID@Xbox </th>
<th>Programme Créateurs Xbox Live</th>
</tr>

<tr>
<td rowspan="2" class="dev-program-feature-name">Identité</td>
<td>Connexion / inscription</td>
<td>Les joueurs pour se connecter à Xbox Live au sein de votre titre ou créer un compte Xbox Live, si nécessaire</td>
<td class="xbl-features-required">Obligatoire</td>
<td class="xbl-features-required">Obligatoire</td>
</tr>

<tr>
<td>Identité de l’utilisateur</td>
<td>Utiliser l’identité de Xbox Live en affichant le Gamertag, Gamerpic, etc.</td>
<td class="xbl-features-required">Obligatoire</td>
<td class="xbl-features-required">Obligatoire</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="13" class="dev-program-feature-name">Social</td>

<td>Présence de base</td>
<td>Afficher les chaînes de présence de base illustrant l’activité utilisateur dans un titre.  Par exemple : « Steve joue Minecraft »</td>
<td class="xbl-features-automatic">Automatique</td>
<td class="xbl-features-automatic">Automatique</td>
</tr>

<tr>
<td>Récemment lu</td>
<td>S’affichent dans les titres récemment lus dans l’application Xbox ou votre Xbox One</td>
<td class="xbl-features-automatic">Automatique</td>
<td class="xbl-features-automatic">Automatique</td>
</tr>

<tr>
<td>Flux d’activité</td>
<td>S’affichent dans l’activité de flux dans l’application Xbox ou votre Xbox One</td>
<td class="xbl-features-automatic">Automatique</td>
<td class="xbl-features-automatic">Automatique</td>
</tr>

<tr>
<td>Hub de jeux</td>
<td>Disposer d’un concentrateur de jeu associé à titre de votre affichage des statistiques, des vidéos et autres contenus dans un flux spécifique à votre titre</td>
<td class="xbl-features-automatic">Automatique</td>
<td class="xbl-features-automatic">Automatique</td>
</tr>

<tr>
<td>Clubs</td>
<td>Lecteurs peuvent utiliser l’application Xbox ou votre Xbox One pour créer des clubs qui peuvent être éventuellement associés à votre titre.</td>
<td class="xbl-features-automatic">Automatique</td>
<td class="xbl-features-automatic">Automatique</td>
</tr>

<tr>
<td>Recherchez le groupe (LFG)</td>
<td>LFG permet aux joueurs de trouver d’autres hors jeu pour planifier un jeu multijoueur.</td>
<td class="xbl-features-automatic">Automatique</td>
<td class="xbl-features-automatic">Automatique</td>
</tr>

<tr>
<td>GameDVR</td>
<td>Lecteurs peuvent capturer les vidéos de leurs sessions de jeu et partager ces derniers sur la flux d’activités.</td>
<td class="xbl-features-automatic">Automatique</td>
<td class="xbl-features-automatic">Automatique</td>
</tr>

<tr>
<td>Diffusion</td>
<td>Les joueurs peuvent diffusion en direct leur jeu par le biais de services tels que Mixer et Twitch de diffusion en continu</td>
<td class="xbl-features-automatic">Automatique</td>
<td class="xbl-features-automatic">Automatique</td>
</tr>

<tr>
<td>Présence enrichie</td>
<td>Affiche des informations détaillées sur les lecteurs dans votre titre.  Alors que la présence de base peut afficher « Utilisateur est de voiture de course Game », de présence enrichie vous permet de spécifier une chaîne plus détaillée tels que « utilisateur est conduite SuperCar dans RainyForest »</td>
<td class="xbl-features-required">Obligatoire</td>
<td class="xbl-features-notavailable">Non prise en charge</td>
</tr>

<tr>
<td>Amis</td>
<td>Récupérer la liste d’amis de l’utilisateur de connexion pour activer des scénarios dans le titre de votre société.</td>
<td class="xbl-features-required">Obligatoire</td>
<td class="xbl-features-limited">Facultatif / limité (uniquement les amis qui ont lu votre titre sont exposées)</td>
</tr>

<tr>
<td>Confidentialité</td>
<td>Autoriser les joueurs sur Muet ou de bloquer ou autres joueurs</td>
<td class="xbl-features-optional">Facultatif</td>
<td class="xbl-features-optional">Facultatif</td>
</tr>

<tr>
<td>Réputation</td>
<td>Lecteurs de gains ou perdent réputation via leur comportement. Comportement est utilisé dans Matchmaking et peut être utilisé par votre titre de manière personnalisée.</td>
<td class="xbl-features-optional">Facultatif</td>
<td class="xbl-features-notavailable">Non prise en charge</td>
</tr>

<tr>
<td>Gestionnaire sociale</td>
<td>Récupérer efficacement des informations sur le graphique social du lecteur</td>
<td class="xbl-features-optional">Facultatif</td>
<td class="xbl-features-limited">Facultatif / limité (uniquement les amis qui ont lu votre titre sont exposées)</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="4" class="dev-program-feature-name">Plateforme de données</td>

<td>Statistiques joueur</td>
<td>Charger des statistiques sur les joueurs qui peuvent être utilisées dans les tableaux de résultats.</td>
<td class="xbl-features-optional">Facultatif</td>
<td class="xbl-features-optional">Facultatif (plateforme de données 2017 uniquement)</td>
</tr>

<tr>
<td>Statistiques en vedette</td>
<td>Désigner certaines statistiques en tant que « Featured statistiques » qui s’affiche dans le Hub de jeu.</td>
<td class="xbl-features-required">Obligatoire</td>
<td class="xbl-features-optional">Facultatif (plateforme de données 2017 uniquement)</td>
</tr>

<tr>
<td>Classements</td>
<td>Récupérer et afficher les statistiques de manière trié à encourager la concurrence.</td>
<td class="xbl-features-optional">Facultatif</td>
<td class="xbl-features-optional">Facultatif (plateforme de données 2017 uniquement)</td>
</tr>

<tr>
<td>Primes avec un score de joueur</td>
<td>Désigner certaines statistiques en tant que « Featured statistiques » qui s’affiche dans le Hub de jeu.</td>
<td class="xbl-features-required">Obligatoire</td>
<td class="xbl-features-notavailable">Non prise en charge</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="1" class="dev-program-feature-name">Support</td>

<td>Recherche contextuelle</td>
<td>Annoter des clips GameDVR avec des mots clés pour le rendre plus facile pour les joueurs rechercher des éléments correspondant à ce qu’il souhaite regarder.</td>
<td class="xbl-features-optional">Facultatif</td>
<td class="xbl-features-notavailable">Non prise en charge</td>
</tr>


<tr class="dev-program-feature-start">
<td rowspan="2" class="dev-program-feature-name">Stockage</td>

<td>Stockage connecté</td>
<td>Itinérance enregistre jeux sur PC et Consoles une Xbox</td>
<td class="xbl-features-required">Obligatoire</td>
<td class="xbl-features-optional">Facultatif</td>
</tr>

<tr>
<td>Stockage de titres</td>
<td>Stockage cloud pour les grandes quantités de chaque utilisateur ou les données par titre.</td>
<td class="xbl-features-optional">Facultatif</td>
<td class="xbl-features-optional">Facultatif</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="6" class="dev-program-feature-name">Mode multijoueur en ligne</td>

<td>Multiplayer Session Directory (MPSD)</td>
<td>Stocke les informations relatives à une session multijoueur, telles que la liste des joueurs, état, etc.</td>
<td class="xbl-features-optional">Obligatoire</td>
<td class="xbl-features-notavailable">Non prise en charge</td>
</tr>

<tr>
<td>Matchmaking</td>
<td>Xbox Live peut correspondre à différents lecteurs ensemble d’une session multijoueur.</td>
<td class="xbl-features-optional">Facultatif</td>
<td class="xbl-features-notavailable">Non prise en charge</td>
</tr>

<tr>
<td>Arena</td>
<td>Les joueurs peuvent s’affronter contre l’autre style de tournoi.</td>
<td class="xbl-features-optional">Facultatif</td>
<td class="xbl-features-notavailable">Non prise en charge</td>
</tr>

<tr>
<td>Conversation de jeu</td>
<td>Conversation vocale pour les joueurs dans un jeu multijoueur</td>
<td class="xbl-features-optional">Facultatif</td>
<td class="xbl-features-notavailable">Non prise en charge</td>
</tr>

<tr>
<td>Calcul dynamique Xbox</td>
<td>Déployer des exécutables et des ressources de votre titre peut communiquer avec, pour décharger le calcul à partir du client.</td>
<td class="xbl-features-optional">Facultatif</td>
<td class="xbl-features-notavailable">Non prise en charge</td>
</tr>

</table>
