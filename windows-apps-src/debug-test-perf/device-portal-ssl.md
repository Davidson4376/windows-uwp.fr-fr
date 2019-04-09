---
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: Configurer le Portail d’appareil avec un certificat SSL personnalisé
description: À déterminer
ms.date: 4/8/2019
ms.topic: article
keywords: Windows 10, uwp, le portail de l’appareil
ms.localizationpriority: medium
ms.openlocfilehash: cbe813a58124b1cd80f352ae11e9dcff59b21da4
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244335"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>Configurer le Portail d’appareil avec un certificat SSL personnalisé

Dans la mise à jour Windows 10 Creators Update, Windows Device Portal a ajouté un moyen qui permet aux administrateurs de périphériques d'installer un certificat personnalisé pour l'utiliser dans les communications HTTPS.

Bien que vous puissiez le faire sur votre propre PC, cette fonctionnalité est principalement destinée aux entreprises qui ont déjà mis en place une infrastructure de certificat.  

Par exemple, une entreprise peut utiliser une autorité de confiance (AC) pour signer des certificats pour les sites Web intranet pris en charge via le protocole HTTPS. Cette fonctionnalité s'ajoute à cette infrastructure.

## <a name="overview"></a>Vue d'ensemble

Par défaut, Device Portal génère une autorité de confiance racine auto-signée, puis l'utilise pour signer des certificats SSL pour chaque point de terminaison détecté. Cela inclut `localhost`, `127.0.0.1` et `::1` (localhost IPv6).

Le nom d’hôte de l’appareil (par exemple, `https://LivingRoomPC`) et chaque adresse IP de liaison locale attribuée à l’appareil sont également inclus (jusqu'à deux [IPv4, IPv6] par carte réseau).
Vous pouvez voir les adresses IP de liaison locale d'un appareil en examinant l’outil Mise en réseau dans Device Portal. Elles commencent par `10.` ou `192.` pour IPv4, ou `fe80:` pour IPv6.

Dans la configuration par défaut, un avertissement de certificat peut apparaître dans votre navigateur en raison de l'AC racine non approuvée. Plus précisément, le certificat SSL fourni par Device Portal est signé par une AC racine que le navigateur ou le PC n’approuve pas. Cela peut être résolu en créant une AC racine approuvée.

## <a name="create-a-root-ca"></a>Créer une AC racine approuvée

Ce n'est possible que si votre société (ou domicile) ne dispose pas d'une infrastructure de certificat configurée et ne peut s'effectuer qu’une seule fois. Le script PowerShell suivant crée une AC racine appelée _WdpTestCA.cer_. Si vous installez ce fichier sur les autorités de certification racine de confiance de l’ordinateur local, votre appareil approuvera les certificats SSL signés par cette AC racine. Vous pouvez (et devez) installer ce fichier .cer sur chaque PC que vous souhaitez connecter à Windows Device Portal.  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

Une fois qu'il est créé, vous pouvez utiliser le fichier _WdpTestCA.cer_ pour signer des certificats SSL.

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>Créer un certificat SSL avec l’AC racine

Les certificats SSL ont deux fonctions essentielles : sécuriser votre connexion via le chiffrement et vérifier que vous communiquez vraiment avec l’adresse affichée dans la barre du navigateur (Bing.com, 192.168.1.37, etc.) et non avec un tiers malveillant.

Le script PowerShell suivant crée un certificat SSL pour le point de terminaison `localhost`. Chaque point de terminaison détecté par Device Portal nécessite son propre certificat. Vous pouvez remplacer l'argument `$IssuedTo` dans le script par chacun des différents points de terminaison de votre appareil : le nom d’hôte, localhost et les adresses IP.

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

Si vous avez plusieurs appareils, vous pouvez réutiliser les fichiers .pfx de localhost, mais vous devez toujours créer des certificats d'adresse IP et de nom d’hôte pour chaque périphérique séparément.

Lorsque le groupe des fichiers .pfx est généré, vous devez charger ces derniers dans Windows Device Portal.

## <a name="provision-device-portal-with-the-certifications"></a>Configurer Device Portal avec la ou les certifications

Pour chaque fichier .pfx créé pour un périphérique, vous devez exécuter la commande suivante à partir d’une invite de commandes avec élévation de privilèges.

```cmd
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx>
```

Voir l'exemple d’utilisation ci-dessous :

```cmd
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

Une fois que vous avez installé les certificats, il vous suffit de redémarrer le service pour que les modifications prennent effet :

```cmd
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> Les adresses IP peuvent changer au fil du temps.
De nombreux réseaux utilisent DHCP pour distribuer les adresses IP, si bien que les périphériques n’obtiennent pas toujours la même adresse IP que celles qu'ils avaient précédemment. Si vous avez créé un certificat pour une adresse IP sur un appareil et que l’adresse de cet appareil change, Windows Device Portal génère un nouveau certificat à l'aide du certificat auto-signé existant et arrête d'utiliser celui que vous avez créé. Cela fait réapparaître la page d’avertissement de certificat dans votre navigateur. Pour cette raison, nous vous recommandons de vous connecter à vos appareils par le biais de leurs noms d’hôte que vous pouvez définir dans Device Portal. Le nom d'hôte restera identique quelle que soit l'adresse IP.
