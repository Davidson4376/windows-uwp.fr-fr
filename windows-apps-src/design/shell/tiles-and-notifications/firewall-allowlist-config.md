---
author: mijacobs
Description: De nombreuses entreprises utilisent des pare-feu pour bloquer le trafic indésirable. Ce document décrit comment autoriser le trafic WNS à traverser des pare-feu.
title: Ajout du trafic WNS au pare-feu Allowlist
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.author: mijacobs
ms.date: 05/20/2019
ms.topic: article
keywords: Windows 10, UWP, WNS, service de notification Windows, notification, Windows, pare-feu, dépannage, IP, trafic, entreprise, réseau, IPv4, VIP, nom de domaine complet, adresse IP publique
ms.localizationpriority: medium
ms.openlocfilehash: 1f8a72eec46971fa27a4bd0dec112430f2eb3535
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867311"
---
# <a name="allowing-windows-notification-traffic-through-enterprise-firewalls"></a>Autorisation du trafic de notification Windows via les pare-feu d’entreprise

## <a name="background"></a>Présentation
De nombreuses entreprises utilisent des pare-feu pour bloquer le trafic réseau indésirable. Malheureusement, cela peut également bloquer des éléments importants tels que les communications du service de notification Windows. Cela signifie que toutes les notifications envoyées via WNS seront supprimées. Pour éviter cela, les administrateurs réseau peuvent ajouter la liste des canaux WNS approuvés à leur liste d’exemptions pour permettre au trafic WNS de traverser le pare-feu. Vous trouverez ci-dessous plus d’informations sur la façon et les éléments à ajouter. 

> [!Note] 
À partir de 6/24/2019, les clients Windows **ne prennent pas** en charge les proxies, la connexion à WNS doit être une connexion directe.

## <a name="what-information-should-be-added-to-the-allowlist"></a>Les informations qui doivent être ajoutées à allowlist
Vous trouverez ci-dessous une liste contenant les noms de domaine complets, les adresses IP virtuelles et les plages d’adresses IP utilisées par le service de notification Windows. 

> [!IMPORTANT]
> Nous vous suggérons vivement d’autoriser la liste par nom de domaine complet (FQDN), car ceux-ci ne seront pas modifiés. Si vous autorisez la liste par nom de domaine complet, vous n’avez pas besoin d’autoriser également les plages d’adresses IP.

> [!IMPORTANT]
> Les plages d’adresses IP seront modifiées périodiquement. pour cette raison, elles ne sont pas incluses dans cette page. Si vous souhaitez afficher la liste des plages d’adresses IP, vous pouvez télécharger le fichier à partir du centre de téléchargement: [Adresses IP virtuelles Windows notification service (WNS) et plages d’adresses IP](https://www.microsoft.com/download/details.aspx?id=44238). Vérifiez régulièrement pour vous assurer que vous disposez des informations les plus récentes. 


### <a name="fqdns-vips-and-ips"></a>Noms de domaine complets, adresses IP virtuelles et adresses IP
Chacun des éléments du document XML suivant est expliqué dans le tableau qui le suit (en [termes et notations](#terms-and-notations). Les plages d’adresses IP ont été intentionnellement omises dans ce document pour vous inciter à utiliser uniquement les noms de domaine complets, car les noms de domaine complets restent constants. Toutefois, vous pouvez télécharger le fichier XML contenant la liste complète à partir du centre de téléchargement: [Adresses IP virtuelles Windows notification service (WNS) et plages d’adresses IP](https://www.microsoft.com/download/details.aspx?id=44238). Les nouvelles adresses IP virtuelles ou les plages d’adresses IP seront **effectives une semaine après leur téléchargement**.

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

### <a name="terms-and-notations"></a>Termes et notations
Vous trouverez ci-dessous des explications sur les notations et les éléments utilisés dans l’extrait de code XML ci-dessus.

| Terme | Explication |
|---|---|
| **Point-notation décimale (par exemple, 64.4.28.0/26)** | La notation à points décimaux est un moyen de décrire la plage d’adresses IP. Par exemple, 64.4.28.0/26 signifie que les 26 premiers bits de 64.4.28.0 sont constants, tandis que les 6 derniers bits sont variables.  Dans ce cas, la plage IPv4 est 64.4.28.0-64.4.28.63. |
| **ClientDNS** | Il s’agit des filtres de nom de domaine complet (FQDN) pour les périphériques clients (PC Windows, ordinateurs de bureau) recevant des notifications de WNS. Elles doivent être autorisées par le biais du pare-feu pour permettre aux clients WNS d’utiliser la fonctionnalité WNS.  Il est recommandé d’autoriser la liste des noms de domaine complets au lieu des plages IP/VIP, puisque celles-ci ne seront jamais modifiées. |
| **ClientIPsIPv4** | Il s’agit des adresses IPv4 des serveurs auxquels les périphériques clients accèdent (ordinateurs Windows, ordinateurs de bureau) recevant des notifications de WNS. |
| **CloudServiceDNS** | Il s’agit des filtres de nom de domaine complet (FQDN) pour les serveurs WNS auxquels votre service Cloud doit communiquer pour envoyer des notificatios à WNS. Celles-ci doivent être autorisées par le biais du pare-feu pour permettre aux services d’envoyer des notifications WNS.  Il est recommandé d’autoriser la liste des noms de domaine complets au lieu des plages IP/VIP, puisque celles-ci ne seront jamais modifiées.|
| **CloudServiceIPs** | CloudServiceIPs sont les adresses IPv4 des serveurs utilisés pour les services Cloud pour envoyer des notifications à WNS  |


## <a name="microsoft-push-notifications-service-mpns-public-ip-ranges"></a>Plages d’adresses IP publiques du service de notifications push Microsoft (MPNS)
Si vous utilisez le service de notification hérité, MPNS, les plages d’adresses IP que vous devrez ajouter à la liste verte sont disponibles dans le centre de téléchargement: [Plages d’adresses IP publiques du service de notifications push Microsoft (MPNS)](https://www.microsoft.com/download/details.aspx?id=44535).


## <a name="related-topics"></a>Rubriques connexes

* [Démarrage rapide : Envoi d’une notification push](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10))
* [Comment demander, créer et enregistrer un canal de notification](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10))
* [Comment intercepter des notifications pour les applications en cours d’exécution](https://docs.microsoft.com/previous-versions/windows/apps/jj709907(v=win.10))
* [Comment s’authentifier avec le service de notifications push Windows (WNS)](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10))
* [En-têtes de demande et de réponse du service de notification push](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))
* [Instructions et liste de vérification pour les notifications push](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
 
