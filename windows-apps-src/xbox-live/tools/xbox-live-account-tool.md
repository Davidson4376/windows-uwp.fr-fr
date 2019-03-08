---
title: Outil de compte Live Xbox
description: Découvrez comment utiliser l’outil de compte Xbox Live pour créer rapidement des comptes de test pour tester le titre de la Xbox Live est activé.
ms.assetid: ec5959f9-1c60-4aa4-94a6-5d8bdcf77a96
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, test, les comptes de test
ms.localizationpriority: medium
ms.openlocfilehash: dead4e62e41b7b597ba9a578ee8f174386529937
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632824"
---
# <a name="xbox-live-account-tool"></a>Outil de compte Live Xbox

## <a name="what-is-xbox-live-account-tool"></a>Nouveautés d’outil de compte Xbox Live ?
L’outil de compte Xbox Live est un outil conçu pour aider les développeurs de titre à configurer des comptes de développement existant pour les scénarios de jeu de test. Par exemple, vous pouvez utiliser l’outil de compte Xbox Live pour modifier l’identité d’un compte développement, ou ajouter rapidement des abonnés de 1000 à la liste d’amis d’un compte.

## <a name="what-can-i-do-with-xbox-live-account-tool"></a>Que puis-je faire avec l’outil de compte Xbox Live ?
Vous pouvez :
  1. Afficher les paramètres de profil d’un utilisateur, XUID et privilèges actives
  2. Ajouter une liste d’abonnés à graphique social d’un utilisateur, soit à partir d’un fichier texte ou un fichier csv de plateforme de développement Xbox
  3. Gérer la liste d’amis d’un utilisateur : favori, retirer des Favoris, bloquer et débloquer des utilisateurs que vous suivez et voir si elles vous suivent dans
  4. Modifier la réputation de l’utilisateur de votre développement (et voir immédiatement les valeurs brutes réputation stat)
  5. Modifier l’identité de l’utilisateur

## <a name="where-can-i-find-xbox-live-account-tool"></a>Où puis-je trouver outil de compte Xbox Live ?
Vous trouverez l’outil de compte Xbox Live en tant que partie du package à partir de Xbox Live Tools [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools).

## <a name="how-do-i-log-in"></a>Comment puis-je me connecter ?
Vous devez les informations d’identification de l’utilisateur que vous souhaitez gérer et spécifier le bac à sable correct. Assurez-vous que le compte de développement a accès pour le bac à sable, sinon la connexion peut échouer. L’outil a été conçu avec les comptes de développement à l’aide d’un bac à sable à l’esprit.

## <a name="can-i-use-a-retail-account-or-does-it-have-to-be-a-sandboxed-account"></a>Puis-je utiliser un compte de la vente au détail, ou qu’il ne doit être un compte de bac à sable ?
Vous pouvez certainement utiliser l’outil de compte Xbox Live pour gérer un compte de la vente au détail, mais pas toutes les fonctionnalités sont prises en charge. Par exemple, vous ne pouvez pas modifier la réputation de l’utilisateur de la vente au détail.

## <a name="how-do-i-change-a-dev-users-gamertag"></a>Évolution de l’identité de l’utilisateur d’un développement
Accédez à l’onglet « Identité » et entrez une identité. Identités doivent contenir uniquement des chiffres, des lettres et des espaces et peuvent uniquement être 15 caractères. Identités de compte de développement doivent commencer par un 2. Qu’une seule modification est actuellement pris en charge.

## <a name="how-do-i-see-my-block-list"></a>Comment voir ma liste de blocage ?
Accédez à l’onglet « People » et sélectionnez l’en-tête de colonne « Bloqué » pour trier par les utilisateurs qui sont actuellement bloqués.

## <a name="how-do-i-follow-a-large-group-of-users"></a>Comment pour suivre un grand groupe d’utilisateurs ?
Si vous avez une liste de XUIDs vous souhaitez suivre, copiez-les dans un fichier texte. Accédez à l’onglet « Suivre » et sélectionnez « Importer la liste » Choisir votre fichier. Les XUIDs doivent remplir dans l’affichage de liste. Cliquez sur « Valider les modifications » pour suivre les utilisateurs.

## <a name="how-do-i-block-someone"></a>Comment empêcher une personne ?
Accédez à l’onglet « People » et sélectionnez les utilisateurs que vous souhaitez bloquer. Appuyez sur le bouton « bloc » et ils amèneront sont bloqués par lots. Si vous constatez une erreur, il vous suffit de nouvelle tentative ultérieurement.

## <a name="how-do-i-change-my-dev-accounts-repuation"></a>Comment modifier réputation de le de mon compte de développement ?
Accédez à l’onglet « Réputation ». Sélectionnez les valeurs par défaut souhaité et appuyez sur le bouton « Valider les modifications » pour envoyer la demande. Vous verrez la statistique de réputation valeurs changent si la demande réussite.

## <a name="what-do-the-values-in-the-reputation-tab-mean"></a>Que signifient les valeurs dans l’onglet de réputation ?
Global réputation est calculée à partir de la sous-réputation de trois : FairPlay (conduite multijoueur), contenu généré par l’utilisateur (clips vidéo, etc.) et les communications (messages et la voix). Les valeurs brutes pour chaque catégorie peuvent compris entre 0 et 75, où la plus élevée signifie que réputation de l’utilisateur est une meilleure. Le OverallStatIsBad vous indique si l’utilisateur a « Éviter Me » réputation.

## <a name="whats-the-black-area-at-the-bottom"></a>Quel est le noir en bas ?
Pour vous tenir informé, nous avons pensé qu’il serait utile si la sortie de débogage est imprimée dans l’interface utilisateur. Vous dire ce que l’outil est jusqu'à et l’imprimer toutes les erreurs de zone qu’il s’exécute.

## <a name="wheres-my-gamerpic"></a>Où est mon gamerpic ?
Nous sommes conscients d’un bogue qui certains comptes de développement n’obtiennent pas une gamerpic généré automatiquement au moment de la création de compte.

## <a name="why-are-things-happening-so-slowly"></a>Pourquoi sont choses qui se produisent si lent ?
Pour les opérations par lot (comme l’ajout d’abonnés), l’outil effectue automatiquement des lots pour empêcher les tailles de demande énorme.

## <a name="how-do-i-run-xbox-live-account-tool"></a>Comment exécuter un outil de compte Xbox Live ?
Extrayez Xbox Live SDK dans un dossier et double-cliquez sur le fichier Tools\XboxLiveAccountTool.exe pour l’exécuter.

## <a name="i-have-a-feature-request-how-do-i-get-my-feature-incorporated"></a>J’ai une demande de fonctionnalité ! Comment obtenir ma fonctionnalité intégrée ?
Contactez votre représentant Microsoft avec des demandes de fonctionnalités et nous allons voir ce que nous pouvons faire.
