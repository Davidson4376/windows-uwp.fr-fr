---
author: PatrickFarley
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: Portail de périphériques pour mettre en service avec un certificat SSL personnalisé
description: À déterminer
ms.author: pafarley
ms.date: 07/11/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, portail de périphérique
ms.localizationpriority: medium
ms.openlocfilehash: 1192c200cd42ab28cc7e763c06fd8a5638aa3400
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2834488"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>Portail de périphériques pour mettre en service avec un certificat SSL personnalisé
Dans la mise à jour Windows 10: les créateurs de portail de périphériques Windows ajouté un moyen pour les administrateurs de périphérique installer un certificat personnalisé à utiliser dans les communications HTTPS. 

Alors que vous pouvez le faire sur votre propre ordinateur, cette fonctionnalité est principalement destinée aux entreprises ayant une infrastructure de certificats en place.  

Par exemple, une société peut avoir une autorité de certification (CA) qu’il utilise pour signer des certificats pour les sites Web intranet pris en charge via HTTPS. Cette fonctionnalité est l’acronyme de l’infrastructure. 

## <a name="overview"></a>Vue d’ensemble
Par défaut, appareil portail génère une autorité de certification racine auto-signé, puis l’utilise pour signer les certificats SSL pour chaque point de terminaison qu’il écoute sur. Cela inclut `localhost`, `127.0.0.1`, et `::1` (localhost IPv6).

Nom d’hôte du périphérique, sont également inclus (par exemple, `https://LivingRoomPC`) et chaque adresse IP de liaison locale attribué au périphérique (jusqu'à deux [IPv4, IPv6] par la carte réseau). Vous pouvez voir les adresses IP de liaison locale pour un périphérique en examinant l’outil de mise en réseau dans le portail de l’appareil. Ils peuvent commencer avec `10.` ou `192.` pour IPv4, ou `fe80:` pour IPv6. 

Dans l’installation par défaut, un avertissement de certificat peut-être apparaître dans votre navigateur en raison de l’autorité de certification racine non fiable. Plus précisément, le certificat SSL fourni par le portail de périphérique est signé par une autorité de certification du navigateur ou PC n’approuve pas racine. Cela peut être résolu en créant une autorité de certification racine approuvée.

## <a name="create-a-root-ca"></a>Créer une autorité de certification racine

Cela ne doit être effectuée que si votre société (ou accueil) ne possède pas configurer une infrastructure de certificats et ne doit être effectuée qu’une seule fois. Le script PowerShell suivant crée une autorité de certification appelée _WdpTestCA.cer_racine. L’installation de ce fichier pour les autorités de Certification racine de confiance de l’ordinateur local entraînera votre appareil approuver les certificats SSL qui sont signés par cette autorité de certification racine. Vous pouvez (et doivent) installer ce fichier .cer sur chaque ordinateur que vous souhaitez vous connecter au portail de périphériques Windows.  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

Une fois que cela est créé, vous pouvez utiliser le fichier _WdpTestCA.cer_ pour signer les certificats SSL. 

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>Créer un certificat SSL avec l’autorité de certification racine

Certificats SSL ont deux fonctions critiques: sécurisation de votre connexion par le biais de chiffrement et de vérifier que vous êtes en communication réellement avec l’adresse affichée dans la barre du navigateur (Bing.com, 192.168.1.37, etc.) et non un tiers malveillant.

Le script PowerShell suivant crée un certificat SSL pour le `localhost` point de terminaison. Chaque point de terminaison qui écoute périphérique portail a besoin de son propre certificat; Vous pouvez remplacer le `$IssuedTo` argument dans le script avec chacun des différents points de terminaison de votre appareil: le nom d’hôte, localhost et l’adresses IP.

```PowerShell
$IssuedTo = "localhost"
$Password = "PickAPassword"
$OutputPath = "c:\temp\"
$rootCA = Import-Certificate -FilePath C:\temp\WdpTestCA.cer -CertStoreLocation Cert:\CurrentUser\My\

# Create SSL cert signed by certificate authority
$IssuedToClean = $IssuedTo.Replace(":", "-").Replace(" ", "_")
$FilePath = $OutputPath + $IssuedToClean + ".pfx"
$Subject = "CN="+$IssuedTo
$cert = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -Subject $Subject -DnsName $IssuedTo -Signer $rootCA -HashAlgorithm "SHA512"
$certFile = Export-PfxCertificate -cert $cert -FilePath $FilePath -Password (ConvertTo-SecureString -String $Password -Force -AsPlainText)
```

Si vous disposez de plusieurs périphériques, vous pouvez réutiliser les fichiers .pfx localhost, mais vous devez toujours créer des certificats de nom d’hôte et les adresses IP pour chaque périphérique séparément.

Lorsque le groupe de fichiers .pfx est généré, vous devez les charger dans le portail de périphériques Windows. 

## <a name="provision-device-portal-with-the-certifications"></a>Portail de périphériques pour mettre en service avec le relatif

Pour chaque fichier .pfx que vous avez créé pour un périphérique, vous devez exécuter la commande suivante à partir d’une invite de commandes avec élévation de privilèges.

```
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx> 
```

Voir ci-dessous par exemple d’utilisation:
```
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

Une fois que vous avez installé les certificats, il vous suffit de redémarrer le service afin que les modifications prennent effet:

```
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> Les adresses IP peuvent changer au fil du temps.
De nombreux réseaux utilisent DHCP pour attribuer les adresses IP, afin que les périphériques ne reçois toujours la même adresse IP qu’ils avaient précédemment. Si vous avez créé un certificat pour une adresse IP sur un appareil et que l’adresse du périphérique a été modifié, portail de périphériques Windows générera un nouveau certificat utilisant le certificat auto-signé existant et arrête les à l’aide de celle que vous avez créé. Cela entraîne la page Avertissement cert s’affiche à nouveau dans votre navigateur. Pour cette raison, nous vous recommandons de connexion à vos périphériques par le biais de leurs noms d’hôte, que vous pouvez définir dans le portail de l’appareil. Il reste identique indépendamment des adresses IP.
