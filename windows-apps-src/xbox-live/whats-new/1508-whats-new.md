---
title: Nouveautés pour le Kit de développement Xbox Live - août 2015
description: Nouveautés pour le Kit de développement Xbox Live - août 2015
ms.assetid: a034867b-7cc0-4b97-89d5-3486e95a80b4
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a454b7339035475416934c2f921dae65283c340c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639814"
---
# <a name="whats-new-for-the-xbox-live-sdk---august-2015"></a>Nouveautés pour le Kit de développement Xbox Live - août 2015

Veuillez consulter la [What ' s New - juin 2015](1506-whats-new.md) article pour ce qui a été ajouté dans la version de juin 2015.

La version d’août de Xbox Live SDK inclut les mises à jour suivantes :

## <a name="os-and-tool-support"></a>Prise en charge du système d’exploitation et d’outil
Le Kit de développement Xbox Live prend désormais en charge Windows 10 RTM [Version 10.0.10240] et Visual Studio 2015 RTM [Version 14.0.23107.0].

## <a name="multiplayer-manager-winrt-apis"></a>Le Gestionnaire de mode multijoueur WinRT API
Gestionnaire de mode multijoueur (dans l’espace de noms expérimentale) prend désormais en charge WinRT APIs (en plus APIs C++)

## <a name="submit-batch-feedback-from-a-title"></a>Envoyer des commentaires de traitement par lots à partir d’un titre
Soumet un nombre d’éléments de commentaires à la fois à partir d’un titre.

## <a name="newupdated-documentation"></a>Documentation de mise à jour de nouveau /
Le package de Xbox Live SDK maintenant inclut un dossier « Docs », contient des références d’API mis à jour et le nouveau « Xbox Live programmation guide ».

Correctifs de bogues :

* Se bloquer lors de la suppression des abonnements dans le Service de l’activité en temps réel
* Se bloquer quand vous êtes connecté avec un compte invité
* Violation d’accès lorsque vous déconnectez le câble réseau
* Échecs de tunnel désormais donner à un code d’erreur dans les APIs C++
* Problème d’ETag avec TitleStorageService::DownloadBlobAsync
* Divers correctifs de bogues pour les exemples d’applications.
