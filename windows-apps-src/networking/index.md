---
author: DelfCo
ms.assetid: 7bb9fd81-8ab5-4f8d-a854-ce285b0669a4
description: "Technologies d’accès aux services réseau et web."
title: "Réseau et services web"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: df62edbf63777a8fcec4ab08c1fb56ab76ea6624

---

# Réseau et services web

\[ Article mis à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Les technologies de réseau et services web suivantes sont disponibles pour les développeurs de plateforme Windows universelle (UWP).

| Rubrique                                                                                   | Description                                                                      |
|-----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| [Notions de base en matière de réseau](networking-basics.md)                                               | Ce que vous devez faire pour toute application réseau.                     |
| [Quelle technologie de réseau?](which-networking-technology.md)                          | Un aperçu rapide des technologies de réseau disponibles pour un développeur UWP, avec des suggestions sur la façon de choisir les technologies appropriées pour votre application.               |
| [Communications réseau en arrière-plan](network-communications-in-the-background.md) | Les applications utilisent les tâches en arrière-plan et deux mécanismes principaux pour maintenir les communications lorsqu’elles ne sont pas au premier plan: le broker de socket et les déclencheurs de canal de contrôle.                  |
| [Sockets](sockets.md)                                                                   | En tant que développeur d’applications UWP, vous pouvez utiliser tant [Windows.Networking.Sockets](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.networking.sockets.aspx) que [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms737523) pour communiquer avec d’autres appareils. Cette rubrique fournit des instructions détaillées sur l’utilisation de l’espace de noms Windows.Networking.Sockets pour les opérations réseau. |
| [WebSockets](websockets.md)                                                             | Les WebSockets fournissent un mécanisme de communication bidirectionnelle sécurisée et rapide entre un client et un serveur sur le web à l’aide du protocole HTTP(S).                 |
| [HttpClient](httpclient.md)                                                             | Utilisez l’API d’espace de noms [Windows.Web.Http](https://msdn.microsoft.com/library/windows/apps/dn279692) pour envoyer et recevoir des informations à l’aide des protocoles HTTP 2.0 et HTTP 1.1.             |
| [Flux RSS/Atom](web-feeds.md)                                                          | Récupérez ou créez le contenu web le plus actualisé et le plus populaire à l’aide de flux syndiqués générés conformément aux normes RSS et Atom via les fonctionnalités de l’espace de noms [Windows.Web.Syndication](https://msdn.microsoft.com/library/windows/apps/br243632).                   |
| [Transferts en arrière-plan](background-transfers.md)                                         | Utilisez l’API de transfert en arrière-plan pour copier des fichiers de manière fiable sur le réseau.           |
| [Marquage de connexions réseau avec l’identitéEDP](tagging_network_connections_with_edp_identity.md) | Cette rubrique vous explique comment générer un contexte de thread protégé avant de créer les connexions réseau dans un scénario de protection des données d’entreprise (EDP, Enterprise Data Protection). |



<!--HONumber=Jun16_HO4-->


