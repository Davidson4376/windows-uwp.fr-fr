---
title: Présentation des outils XboxOne
description: Outil Accueil du développeur propre à XboxOne, utilisant Windows Device Portal.
ms.date: 10/04/2017
ms.topic: article
keywords: windows10, uwp, xboxone, outils
ms.assetid: 6eaf376f-0d7c-49de-ad78-38e689b43658
ms.localizationpriority: medium
ms.openlocfilehash: ed106095d83ed0c6e055d22a1a0cf229380cff71
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8197581"
---
# <a name="introduction-to-xbox-one-tools"></a>Présentation des outils Xbox One

Cette section décrit le mode d'accès au portail d'appareil Xbox via l'application Accueil du développeur.

## <a name="dev-home"></a>Accueil du développeur

Accueil du développeur permet d’expérimenter des outils dans le Kit de développement Xbox One qui sont destinés à améliorer la productivité des développeurs. L’outil Accueil du développeur offre des fonctionnalités vous permettant de gérer et de configurer votre kit de développement.

Accueil du développeur est l'application par défaut qui est ouverte lorsque votre console démarre en mode Développeur. Vous pouvez également ouvrir Accueil du développeur en sélectionnant la vignette **Accueil du développeur** sur l’écran d’accueil. Si cette vignette est absente, la console n’est pas en mode développeur.

Pour plus d’informations concernant Dev Home, consultez [Accueil du développeur sur la Console (DevHome)](dev-home.md).

## <a name="xbox-device-portal"></a>Portail d'appareil Xbox
Le portail d'appareil Xbox est un outil de gestion des périphériques sur navigateur. Il vous permet d'ajouter des jeux et applications, d'ajouter des comptes test XboxLive, de modifier des bacs à sable, etc.

Pour activer le portail d'appareil Xbox sur votre console XboxOne:

1. Sélectionnez la vignette **Accueil du développeur** sur l’écran d’accueil.

  ![Sélection de la vignette Accueil du développeur](images/introduction-to-xbox-one-tools-1.png)

2. Dans Accueil du développeur, naviguez vers l'onglet **Accueil** et, dans la section **Accès à distance**, sélectionnez **Paramètres d'accès à distance**.

  ![Outil Gestion à distance](images/introduction-to-xbox-one-tools-2.png)

3. Cochez la case **Activer le portail d'appareil Xbox**.

4. Sous **Authentification**, cochez la case **Nécessiter l'authentification pour accéder à cette console à distance depuis des outils web ou PC**.

5. Renseignez les champs **Nom d'utilisateur** et __Mot de passe__, puis sélectionnez **Enregistrer**. Ces identifiants sont utilisés pour authentifier l’accès à votre kit de développement à partir d’un navigateur.

6. Sélectionnez **Fermer**, puis sur l'onglet **Accueil**, prenez note de l'URL répertoriée dans l'outil **Accès à distance**.

7. Saisissez l'URL dans votre navigateur. Vous recevrez un avertissement concernant le certificat fourni, semblable à la capture d’écran ci-dessous. En effet, le certificat de sécurité signé par votre console XboxOne n’est pas considéré comme un éditeur approuvé bien connu. Sur Edge, cliquez sur **Détails**, puis sur **Accéder à la page Web** pour accéder au portail d’appareilXbox.

    ![Avertissement concernant le certificat de sécurité](images/introduction-to-xbox-one-tools-3.png)

8. Connectez-vous avec les identifiants que vous avez configurés.

## <a name="xbox-dev-mode-companion"></a>Compagnon du mode de développement Xbox
Le Compagnon du mode de développement Xbox est un outil qui vous permet de travailler sur votre console sans quitter votre PC. L’application vous permet d’afficher l’écran de la console et d’y envoyer des données. Pour plus d’informations, voir [Compagnon du mode de développement Xbox](xbox-dev-mode-companion.md).

## <a name="see-also"></a>Voir également
- [Utilisation de Fiddler avec XboxOne lors du développement pour UWP](uwp-fiddler.md)
- [Vue d’ensemble de Windows Device Portal](../debug-test-perf/device-portal.md)
- [UWP sur XboxOne](index.md)