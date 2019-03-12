---
ms.assetid: 7bb9fd81-8ab5-4f8d-a854-ce285b0669a4
description: Technologies d’accès aux réseaux et aux services web.
title: Réseau et services web
ms.date: 11/26/2017
ms.topic: article
keywords: "windows\_10, uwp"
ms.localizationpriority: medium
---
# <a name="networking-and-web-services"></a>Réseau et services web

Les technologies de réseau et services web suivantes sont disponibles pour les développeurs de plateforme Windows universelle (UWP).

| Rubrique | Description |
| - | - |
| [Notions de base relatives aux réseaux](networking-basics.md) | Ce que vous devez faire pour toute application réseau. |
| [Quelle technologie réseau ?](which-networking-technology.md) | Un aperçu rapide des technologies de réseau disponibles pour un développeur UWP, avec des suggestions sur la façon de choisir les technologies appropriées pour votre application. |
| [Communications réseau en arrière-plan](network-communications-in-the-background.md) | Pour qu’une application puisse poursuivre la communication quand elle n’est pas en arrière-plan, elle doit utiliser des tâches en arrière-plan et un répartiteur de socket ou des déclencheurs de chaîne de contrôle. |
| [Sockets](sockets.md) | Les sockets constituent une technologie de transfert de données de bas niveau sur laquelle repose l’implémentation d’un grand nombre de protocoles réseau. UWP propose des classes de socket TCP et UDP pour les applications client/serveur ou (P2P) pair à pair, qu’il s’agisse de connexions longue durée ou non établies. |
| [WebSockets](websockets.md) | Les WebSockets fournissent un mécanisme de communication bidirectionnelle sécurisée et rapide entre un client et un serveur sur le web, à l’aide du protocole HTTP(S) et avec une prise en charge des messages UTF-8 et binaires. |
| [HttpClient](httpclient.md) | Utilisez l’API d’espace de noms [Windows.Web.Http](https://msdn.microsoft.com/library/windows/apps/dn279692) pour envoyer et recevoir des informations à l’aide des protocoles HTTP 2.0 et HTTP 1.1. |
| [Flux RSS/Atom](web-feeds.md) | Récupérez ou créez le contenu web le plus actualisé et le plus populaire à l’aide de flux syndiqués générés conformément aux normes RSS et Atom via les fonctionnalités de l’espace de noms [Windows.Web.Syndication](https://msdn.microsoft.com/library/windows/apps/br243632). |
| [Transferts en arrière-plan](background-transfers.md) | Utilisez l’API de transfert en arrière-plan pour copier des fichiers de manière fiable sur le réseau. |
