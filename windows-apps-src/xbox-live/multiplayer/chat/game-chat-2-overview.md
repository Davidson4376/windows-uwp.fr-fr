---
title: Vue d’ensemble de jeu 2 Chat
description: Découvrez comment ajouter la communication vocale à votre jeu à l’aide de Xbox Live Game Chat 2, une version mise à jour de jeu Chat.
ms.date: 10/20/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, chat jeu, jeux chat 2, communication vocale
ms.localizationpriority: medium
ms.openlocfilehash: 9f013f8b80cc7bca367c3ef5cd2c0d1da86cc98c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615824"
---
# <a name="game-chat-2-overview"></a>Vue d’ensemble de la conversation jeu 2

Jeu Chat 2 vous permet d’ajouter facilement de communication de conversations vocales et de texte à votre application tout en respectant les paramètres de confidentialité de vos joueurs et en remplissant les conditions de Xbox pour jeux une Xbox et les applications de Hub relatives aux conversations vocales et de texte. Pour les lecteurs que vous ont activé la conversion de reconnaissance vocale ou synthèse vocale par le biais de la facilité d’accès - paramètres de la Transcription de conversation de jeu, jeu Chat 2 effectuera en toute transparence les traductions pour créer des messages texte représentant entrants audio de reconnaissance vocale et play chat synthétisées audio de reconnaissance vocale pour les messages sortants du texte de conversation, respectivement.

> [!NOTE]
> Si vous cherchez une référence d’API spécifique, vous pouvez le trouver dans téléchargeable Xbox Live API compilé (.chm) fichier d’aide HTML [ici](https://aka.ms/xboxliveuwpdocs).

- **Relations de communication** -2 de discuter de jeu vous donne un contrôle précis sur la façon dont vos joueurs peuvent communiquer avec chacun par d’autres utilisateurs. Au lieu de spécifier des équipes ou canaux, jeu Chat 2 nécessite que les relations explicites entre chaque paire d’utilisateurs soit définie. Relations de communication 2 Chat jeu prend en charge uni et la communication bidirectionnelle entre une paire quelconque de lecteurs. Relations de communication vocale et le texte peuvent être définies indépendamment les uns des autres.

- **Accessibilité** -jeu Chat 2 prend en charge la reconnaissance vocale et synthèse vocale. Lorsque l’option est activée, jeu Chat 2 respectera la préférence « Transcription de Chat Game » de vos joueurs et effectuera en toute transparence les traductions pour créer des messages texte représentant son vocale entrant et play synthétisé vocale audio pour le texte de conversation sortant de conversation messages, respectivement.

- **Xbox Live intégration** -jeu Chat 2 utilise les services Xbox Live pour garantir que les préférences de chaque joueur et les privilèges sont respectées.

- **Détection d’activité de voix** -jeu Chat 2 effectue la détection d’activité vocale pour déterminer quand les données audio incluent activité vocale.

- **Contrôle de Gain automatique** -2 de discuter de jeu effectue le contrôle de Gain automatique pour minimiser les variations dans la sortie du microphone d’un utilisateur.

- **Codecs** -2 de Chat jeu encode des données audio qui doivent être livrées aux instances distantes de l’application. Sur Xbox One, cet encodage et décodage sur l’extrémité de réception sont accéléré matérielle.

- `chat_manager::start/finish_processing_state_changes` -La paire de méthodes appelé par l’application pour chaque trame de l’interface utilisateur d’effectuer des opérations asynchrones, pour récupérer les résultats doivent être traitées dans le formulaire de `game_chat_state_change` structures, puis pour libérer les ressources associées, une fois.

- `chat_manager::start/finish_processing_data_frames` -La paire de méthodes permettant de brancher jeu Chat 2 dans la couche de transport de l’application. Ces méthodes sont appelées par l’application de chaque trame réseau pour récupérer et distribuer `game_chat_data_frame` objets aux instances de l’application sur des appareils distants, puis pour libérer les ressources associées, une fois.

- `chat_manager::process_incoming_data` -La méthode utilisée pour fournir des données à jeu Chat 2 qui a été remis au fil de la couche de transport de l’application à partir d’une instance distante de jeu Chat 2.

- **Manipulation Audio en temps réel** -2 de conversation jeu autorise l’application pour s’insérer dans le pipeline audio de conversation, afin qu’il peut inspecter ou manipuler des données audio de conversation. Consultez [manipulation audio en temps réel](real-time-audio-manipulation.md) pour plus d’informations.

L’application informe la bibliothèque des utilisateurs sur l’appareil local ou sur des périphériques distants qui sont censés chat ensemble. L’application configure ensuite les relations entre chaque utilisateur.

Une fois configuré, jeu Chat 2 interroge les données audio à partir de microphone(s) utilisateurs local, effectue la détection d’activité vocale et de contrôle automatique de gain, encode les données, puis expose les données audio à remettre aux instances distantes de jeu Chat 2 via `chat_manager::start/finish_processing_data_frames`. L’application doit fournir les données contenues dans le `game_chat_state_change` objet pour les instances distantes de jeu Chat 2 est spécifié dans le même objet. Lorsqu’il reçoit les données, les instances distantes de l’application doivent envoyer les données à leur propre instance de jeu Chat 2 via `chat_manager::process_incoming_data`. Jeu 2 Chat décode les données entrantes et effectue le rendu pour le périphérique audio de l’utilisateur local.

Jeu 2 Chat applique des spécifications de privilège et de confidentialité Xbox Live. Données audio ne seront pas générées par les utilisateurs qui n’ont pas des privilèges de communications et les données audio ne seront pas affichées à partir des utilisateurs qui sont bloqués par le biais de paramètres de confidentialité.

Pour commencer, consultez [2 de discuter de jeu à l’aide de](using-game-chat-2.md). Si vous utilisez C#, consultez [à l’aide de jeu discuter des Projections de WinRT 2](using-game-chat-2-winrt.md). Si vous disposez déjà d’une implémentation de jeu Chat, utilisez le [Guide de Migration 2 Chat jeu](game-chat-2-migration.md) pour mapper de Chat du jeu de concepts et modèles d’appel à jeu Chat 2 analogues.

Pour plus d’informations sur l’API 2 Chat de jeu, téléchargez la référence d’API Xbox : [XboxAPIs.zip](https://aka.ms/xboxliveuwpdocs)