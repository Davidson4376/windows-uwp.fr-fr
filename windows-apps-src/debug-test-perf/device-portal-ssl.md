---
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: Configurer le Portail d’appareil avec un certificat SSL personnalisé
description: À déterminer
ms.date: 07/11/2017
ms.topic: article
keywords: Windows 10, uwp, portal de l’appareil
ms.localizationpriority: medium
ms.openlocfilehash: faef15d523f56b6e45f77e0ccdbb2f5846f7a15a
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8212314"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>Configurer le Portail d’appareil avec un certificat SSL personnalisé
Dans Windows 10 Creators Update, Windows Device Portal ajouté un moyen pour les administrateurs d’appareil installer un certificat personnalisé pour une utilisation dans les communications HTTPS. 

Pendant que vous pouvez le faire sur votre propre PC, cette fonctionnalité est principalement destinée aux entreprises qui disposent déjà d’une infrastructure de certificat en place.  

Par exemple, une société peut avoir une autorité de certification (CA) qu’il utilise pour signer des certificats pour les sites Web intranet pris en charge via HTTPS. Cette fonctionnalité est l’abréviation d’infrastructure. 

## <a name="overview"></a>Vue d’ensemble
Par défaut, Device Portal génère une autorité de certification racine auto-signé et qui utilise ensuite pour signer des certificats SSL pour qu’il est à l’écoute sur chaque point de terminaison. Cela inclut les `localhost`, `127.0.0.1`, et `::1` (localhost IPv6).

Vous trouverez également des nom d’hôte de l’appareil (par exemple, `https://LivingRoomPC`) et chaque lien-local l’adresse IP attribuée à l’appareil (jusqu'à deux [IPv4, IPv6] par carte réseau). Vous pouvez voir les adresses IP de lien-local pour un appareil en fonction de l’outil de mise en réseau dans le portail d’appareil. Ils peuvent commencer avec `10.` ou `192.` pour IPv4, ou `fe80:` pour IPv6. 

Dans le programme d’installation par défaut, un avertissement concernant le certificat peut apparaître dans votre navigateur en raison de l’autorité de certification racine non approuvée. Plus précisément, le certificat SSL fourni par Device Portal est signé par une autorité de certification qui le navigateur ou le PC n’approuve racine. Cela peut être résolu en créant une nouvelle racine de confiance autorité de certification.

## <a name="create-a-root-ca"></a>Créer une autorité de certification racine

Cela doit uniquement être réalisée si votre société (ou accueil) n’a pas de configurer une infrastructure de certificat et ne doit être effectuée qu’une seule fois. Le script PowerShell suivant crée une autorité de certification appelée _WdpTestCA.cer_racine. L’installation de ce fichier pour les autorités de Certification racine de confiance de l’ordinateur local entraînera votre appareil pour approuver des certificats SSL qui sont signés par cette autorité de certification racine. Vous pouvez (et doivent) installer ce fichier .cer sur chaque PC auquel vous souhaitez vous connecter à Windows Device Portal.  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

Une fois que cela est créé, vous pouvez utiliser le fichier _WdpTestCA.cer_ pour signer des certificats SSL. 

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>Créer un certificat SSL avec l’autorité de certification racine

Certificats SSL ont deux fonctions essentielles: sécurisation de votre connexion par le biais de chiffrement et la vérification que vous communiquez réellement avec l’adresse affichée dans la barre du navigateur (Bing.com, 192.168.1.37, etc.) et non un tiers malveillant.

Le script PowerShell suivant crée un certificat SSL pour le `localhost` point de terminaison. Chaque point de terminaison qui écoute Device Portal a besoin de son propre certificat; Vous pouvez remplacer le `$IssuedTo` argument dans le script avec chacune des différents points de terminaison de votre appareil: le nom d’hôte, hôte local et l’adresses IP.

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

Si vous avez plusieurs appareils, vous pouvez réutiliser les fichiers .pfx localhost, mais vous devez toujours créer des certificats d’adresse et le nom d’hôte IP pour chaque périphérique séparément.

Lorsque l’ensemble de fichiers .pfx est générée, vous devez charger dans Windows Device Portal. 

## <a name="provision-device-portal-with-the-certifications"></a>Configurer le portail d’appareil avec la relatif

Pour chaque fichier .pfx que vous avez créé pour un appareil, vous devez exécuter la commande suivante à partir d’une invite de commandes avec élévation de privilèges.

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
De nombreux réseaux utilisent DHCP pour fournir les adresses IP, afin que les appareils n’obtient pas toujours la même adresse IP qu’ils avaient précédemment. Si vous avez créé un certificat pour une adresse IP sur un appareil et que l’adresse de l’appareil a changé, Windows Device Portal génère un nouveau certificat utilisant le certificat auto-signé existant, et arrête à l’aide de celui que vous avez créé. Cela entraînera la page d’avertissement de certificat s’affiche dans votre navigateur à nouveau. Pour cette raison, nous vous recommandons de se connecter à vos appareils par le biais de leurs noms d’hôtes, que vous pouvez définir dans Device Portal. Elles restent les mêmes, quel que soit les adresses IP.
