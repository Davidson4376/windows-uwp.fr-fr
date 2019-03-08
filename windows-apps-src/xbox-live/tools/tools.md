---
title: Outils de développement pour Xbox Live
description: Découvrez les outils qui sont fournis pour vous aider à développer et tester votre Xbox Live activé titre.
ms.assetid: 380a29bf-41a7-4817-9c57-f48f2b824b52
ms.date: 6/13/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, outils, de réinitialisation du lecteur, l’Analyseur de trace, LTA, outil de compte live xbox, en direct
ms.localizationpriority: medium
ms.openlocfilehash: ad43ecbd3bfd266d4a237253380bca223302fe54
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623514"
---
# <a name="development-tools-for-xbox-live"></a>Outils de développement pour Xbox Live

Cette section traite des différents outils que vous pouvez utiliser pour vous aider à développer pour Xbox Live. La plupart des outils sont disponibles sur le [Xbox Live Developer Tools GitHub](https://github.com/Microsoft/xbox-live-developer-tools) dépôt. Vous pouvez également utiliser le [bibliothèque d’outils de développement](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools) pour créer vos propres outils personnalisés. Tous les outils de développement autonome peuvent être téléchargées sur [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools).

> [!NOTE]
> Les outils MatchSim et XboxLiveCompute inclus dans le téléchargement uniquement peuvent être utilisés que par les partenaires gérés, ou partenaires inscrits dans le [ ID@Xbox ](https://www.xbox.com/Developers/id) programme. Pour en savoir plus sur les programmes de développement disponibles, reportez-vous à la [vue d’ensemble du programme de développeurs](https://docs.microsoft.com/windows/uwp/xbox-live/developer-program-overview). 

## <a name="global-storage"></a>Stockage global
Stockage de titre global est utilisé pour stocker les données que tout le monde peut lire, tels que les mémos, des cartes, des défis ou des ressources de l’art. C’est un type de [titre stockage](../storage-platform/xbox-live-title-storage/xbox-live-title-storage.md). L’outil de stockage Global est utilisé pour gérer le stockage de titre global dans les bacs à sable de test. Données doivent toujours être publiées sur la vente au détail par le biais de partenaires ou Xbox Developer Portal (XDP). L’outil est disponible via la ligne de commande dans le cadre de la [outils de développement](https://aka.ms/xboxliveuwptools) zip. Outils personnalisés peuvent être créés avec le [bibliothèque d’outils de développement](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools).

## <a name="multiplayer-session-history-viewer"></a>Visionneuse de l’historique de Session multijoueurs
Visionneuse de l’historique de Session multijoueur vous donne la possibilité d’afficher une chronologie historique de toutes les modifications sur une session multijoueur l’historique de document (y compris les documents supprimés). À l’aide de cet outil vous donnera une bonne compréhension de ce qui se passe avec vos documents de session MPSD mesure qu’il change au fil du temps. Il est disponible en tant qu’outil autonome dans le [outils de développement](https://aka.ms/xboxliveuwptools) zip.

## <a name="player-data-reset"></a>Réinitialisation des données de lecteur
L’outil de réinitialisation des données de lecteur peut être utilisé pour réinitialiser les données d’un joueur dans les bacs à sable de test. Vous pouvez réinitialiser les données telles que ; primes, tableaux de résultats, statistiques et l’historique de titre. L’outil est disponible via la ligne de commande dans le cadre de la [outils de développement](https://aka.ms/xboxliveuwptools) zip. Outils personnalisés peuvent être créés avec le [bibliothèque d’outils de développement](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools).

## <a name="xbox-live-developer-account"></a>Compte de développeur Live Xbox
L’outil de compte de développeur Xbox Live est utilisé pour gérer l’authentification d’un compte de développeur. Il est nécessaire pour interagir avec d’autres outils de développement qui nécessitent des informations d’identification développeur, telles que la réinitialisation du lecteur et de stockage Global. L’outil est disponible via la ligne de commande dans le cadre de la [outils de développement](https://aka.ms/xboxliveuwptools) zip.

## <a name="xbox-live-trace-analyzer"></a>Xbox Live Trace Analyzer
À l’aide de [Xbox Live Trace analyseur](analyze-service-calls.md), vous pouvez capturer tous les appels de service et les analyser ensuite en mode hors connexion pour toutes les violations dans des modèles d’appel. Suivi des appels de service peut être activé à l’aide de l’outil de ligne de commande xbtrace ou via l’activation de protocole pour en savoir plus les scénarios avancé. Activation du suivi directement à partir de code de titre des appels de service est également pris en charge. L’outil est disponible via la ligne de commande dans le cadre de la [outils de développement](https://aka.ms/xboxliveuwptools) zip.

## <a name="xbox-live-account-tool"></a>Outil de compte Live Xbox  
Le [outil de compte Xbox Live](xbox-live-account-tool.md) est conçu pour vous aider à configurer des comptes de test existants pour les scénarios de jeu de test. Par exemple, vous pouvez utiliser l’outil de compte Xbox Live pour modifier le gamertag d’un compte, ou ajouter rapidement des abonnés de 1000 à la liste d’amis d’un compte. L’outil est disponible via la ligne de commande dans le cadre de la [outils de développement](https://aka.ms/xboxliveuwptools) zip.

## <a name="config-as-source"></a>Configuration en tant que Source
[Configuration en tant que Source](https://github.com/Microsoft/xbox-live-developer-tools/blob/master/CONFIGASSOURCE.md) est une suite d’outils développé par Microsoft pour prendre en compte les utilisateurs expérimentés, en fournissant des outils officiellement pris en charge et les API permettant d’intégrer à nos services de configuration. Ces services Xbox Live sont normalement configurés pour votre titre dans Partner Center, y compris des services allant des classements aux primes, aux services web et les parties de confiance. Pour de nombreux développeurs de jeux, à l’aide de partenaires est suffisant. Pour les utilisateurs expérimentés, dit, un désir d’intégrer des tâches de configuration courantes dans leur propre processus et les outils.  Configuration en tant que Source est destinée à prendre en charge ces scénarios en fournissant des outils de ligne de commande et les nouvelles API pour prendre en charge l’intégration personnalisée dans votre flux de travail existants et les pipelines. L’outil est disponible via la ligne de commande dans le cadre de la [outils de développement](https://aka.ms/xboxliveuwptools) zip.
