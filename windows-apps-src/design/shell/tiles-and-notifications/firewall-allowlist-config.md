---
author: mijacobs
Description: De nombreuses entreprises utilisent les pare-feux pour bloquer le trafic indésirable. Ce document explique comment autoriser le trafic WNS à passer à travers des pare-feu.
title: Ajout du trafic WNS à la Allowlist pare-feu
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.author: mijacobs
ms.date: 05/20/2019
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, WNS, service de notifications de windows, notification, windows, pare-feu, résolution des problèmes, l’adresse IP, le trafic, entreprise, réseau, IPv4, adresse IP virtuelle, nom de domaine complet, adresse IP publique
ms.localizationpriority: medium
ms.openlocfilehash: 23a9b11cd961e03217aba8ca3d1d988447a2f80b
ms.sourcegitcommit: b0edd3c09f931b9b62f9c2d17037fb58d826174f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/25/2019
ms.locfileid: "67349857"
---
# <a name="allowing-windows-notification-traffic-through-enterprise-firewalls"></a>Autoriser le trafic de Notification de Windows à travers des pare-feu d’entreprise

## <a name="background"></a>Arrière-plan
De nombreuses entreprises utilisent des pare-feu pour bloquer le trafic réseau indésirable ; Malheureusement, cela peut également bloquer les choses importantes comme les communications de Service de Notification Windows. Cela signifie que toutes les notifications envoyées via WNS seront supprimées. Pour éviter ce problème, les administrateurs réseau peuvent ajouter la liste des canaux WNS approuvées à leur liste d’exemption pour autoriser le trafic WNS à franchir le pare-feu. Vous trouverez ci-dessous plus d’informations sur la façon et les éléments à ajouter. 

> [!Note] 
À compter du 24/6/2019, les clients Windows **ne le faites pas** prise en charge des serveurs proxy, la connexion à WNS doit être une connexion directe.

## <a name="what-information-should-be-added-to-the-allowlist"></a>Quelles informations doivent être ajoutées à l’allowlist
Ci-dessous est une liste qui contient les noms de domaine complets, les adresses IP virtuelles, ainsi que les IP plages d’adresses utilisées par le Service de Notification Windows. 

> [!IMPORTANT]
> Il est vivement que vous autorisez la liste par nom de domaine complet, car il ne changera pas. Si vous autorisez la liste par nom de domaine complet, il est inutile qu’il autorise également les plages d’adresses IP.

> [!IMPORTANT]
> Les plages d’adresses IP changera régulièrement ; pour cette raison, ils ne sont pas inclus dans cette page. Si vous souhaitez afficher la liste des plages d’adresses IP, vous pouvez télécharger le fichier à partir du centre de téléchargement : [Service de Notification Windows (WNS) adresse IP virtuelle et l’adresse IP plages](https://www.microsoft.com/download/details.aspx?id=44238). Veuillez vérifier back régulièrement pour vous assurer que vous disposez des informations les plus récentes. 


### <a name="fqdns-vips-and-ips"></a>Noms de domaine complets, les adresses IP virtuelles et les adresses IP
Chacun des éléments dans le document XML suivant est expliqué dans le tableau qui suit (dans [termes et les Notations](#terms-and-notations). Les plages d’adresses IP ont été intentionnellement écartés de ce document vous encourager à utiliser uniquement les noms de domaine complets, comme les noms de domaine complets reste constantes. Toutefois, vous pouvez télécharger le fichier XML contenant la liste complète à partir du centre de téléchargement : [Service de Notification Windows (WNS) adresse IP virtuelle et l’adresse IP plages](https://www.microsoft.com/download/details.aspx?id=44238). Adresses IP virtuelles nouvelles ou plages IP va être **efficace une semaine après leur chargement**.

```XML
<?xml version="1.0" encoding="UTF-8"?>
<WNSPublicIpAddresses xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <!-- This file contains the FQDNs, VIPs, and IP address ranges used by the Windows Notification Service. A new text file will be uploaded every time a new VIP or IP range is released in production.  Please copy the below information and perform the necessary changes on your site. Endpoints in CloudService nodes are used for cloud services to send notifications to WNS. Endpoints in Client nodes are used by devices to receive notifications from WNS. --> 
    <CloudServiceDNS>
    <DNS FQDN="*.notify.windows.com"/>
    </CloudServiceDNS>
    <ClientDNS>
        <DNS FQDN="*.wns.windows.com"/>
        <DNS FQDN="*.notify.live.net"/>
    </ClientDNS>
    <CloudServiceIPs>
        <IpRange Subnet=""/>
        <!-- See the file in Download Center for the complete list of IP ranges -->
    </CloudServiceIPs>
    <ClientIPsIPv4>
        <IpRange Subnet=""/>
        <!-- See the file in Download Center for the complete list of IP ranges -->
    </ClientIPsIPv4>
</WNSPublicIpAddresses>

```

### <a name="terms-and-notations"></a>Les notations et les termes du contrat
Vous trouverez ci-dessous des explications sur les notations et les éléments utilisés dans l’extrait de code XML ci-dessus.

| Terme | Explication |
|---|---|
| **Notation décimale à points (par exemple, 64.4.28.0/26)** | Notation décimale est un moyen de décrire la plage d’adresses IP. Par exemple, 64.4.28.0/26 signifie que les premiers bits 26 de 64.4.28.0 sont constants, tandis que les 6 derniers bits sont variables.  Dans ce cas, la plage IPv4 est 64.4.28.0 - 64.4.28.63. |
| **ClientDNS** | Voici les filtres de nom de domaine complet (FQDN) pour les périphériques client (PC Windows, postes de travail) recevoir des notifications de WNS. Il doivent être autorisés à travers le pare-feu pour que les clients WNS utiliser la fonctionnalité de WNS.  Il est recommandé pour la liste verte par les noms de domaine complets au lieu des plages d’adresses IP/VIP, car il ne changera jamais. |
| **ClientIPsIPv4** | Ce sont les adresses IPv4 des serveurs accédés par les appareils clients (Windows PC, postes de travail) de recevoir des notifications de WNS. |
| **CloudServiceDNS** | Voici les filtres de nom de domaine complet (FQDN) pour les serveurs WNS que votre service cloud parlera à envoyer notificatios à WNS. Il doivent être autorisés à travers le pare-feu pour que les services envoyer des notifications de WNS.  Il est recommandé pour la liste verte par les noms de domaine complets au lieu des plages d’adresses IP/VIP, car il ne changera jamais.|
| **CloudServiceIPs** | CloudServiceIPs sont les adresses IPv4 des serveurs utilisés pour les services cloud pour envoyer des notifications à WNS  |


## <a name="microsoft-push-notifications-service-mpns-public-ip-ranges"></a>Plages d’adresses IP publiques de services de Notifications Push Microsoft (MPNS)
Si vous utilisez le service de notification hérité, MPNS, les plages d’adresses IP dont vous aurez besoin pour ajouter à la liste verte sont disponibles à partir du centre de téléchargement : [Plages d’adresses IP publiques de services de Notifications Push Microsoft (MPNS)](https://www.microsoft.com/download/details.aspx?id=44535).


## <a name="related-topics"></a>Rubriques connexes

* [Démarrage rapide : Envoyer une notification push](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10))
* [Comment demander, créer et enregistrer un canal de notification](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10))
* [Comment intercepter des notifications pour les applications en cours d’exécution](https://docs.microsoft.com/previous-versions/windows/apps/jj709907(v=win.10))
* [Comment s’authentifier avec Windows Push Notification Service (WNS)](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10))
* [Envoyer des en-têtes de demande et de réponse de service de notification](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))
* [Instructions et liste de vérification pour les notifications push](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
 
