---
title: Créer un certificat de signature de package
description: Créer et exporter un certificat pour la signature de packages d'applications à l'aide d'outils PowerShell.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 7bc2006f-fc5a-4ff6-b573-60933882caf8
ms.localizationpriority: medium
ms.openlocfilehash: 1b9a538dc36818c065e790170f693576650f5024
ms.sourcegitcommit: 34671182c26f5d0825c216a6cededc02b0059a9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67286929"
---
# <a name="create-a-certificate-for-package-signing"></a>Créer un certificat de signature de package


Cet article explique comment créer et exporter un certificat de signature de packages d'applications à l’aide des outils PowerShell. Il est recommandé d’utiliser Visual Studio pour les [packages d’applications UWP](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps), mais vous pouvez toujours intégrer manuellement dans le package une application du Windows Store si vous n’avez pas utilisé Visual Studio pour développer votre application.

> [!IMPORTANT] 
> Si vous avez utilisé Visual Studio pour développer votre application, nous vous recommandons d’utiliser l’Assistant Visual Studio pour importer un certificat et signer votre package d’application. Pour plus d’informations, voir [Créer un package d’application UWP avec Visual Studio](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

## <a name="prerequisites"></a>Prérequis

- **Une application empaquetée ou décompressée**  
Une application contenant un fichier AppxManifest.xml. Vous devrez référencer le fichier manifeste pendant la création d’un certificat qui sera utilisé pour signer le package d’applications final. Pour plus d’informations sur l'empaquetage manuel d'une application, voir [Créer un package d’application avec l’outil MakeAppx.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool).

- **Applets de commande Infrastructure à clé publique (PKI)**  
Vous avez besoin d’applets de commande PKI pour créer et exporter votre certificat de signature. Pour plus d’informations, voir [Applets de commande d'infrastructure à clé publique](https://docs.microsoft.com/powershell/module/pkiclient/).

## <a name="create-a-self-signed-certificate"></a>Create a self-signed certificate

Un certificat auto-signé est utile pour tester votre application avant que vous êtes prêt à publier sur le Store. Suivez les étapes décrites dans cette section pour créer un certificat auto-signé.

> [!NOTE]
> Les certificats auto-signés sont strictement pour le test. Lorsque vous êtes prêt à publier votre application dans le magasin ou à partir d’autres emplacements, basculez le certificat à une source digne de confiance. Pour ce faire peut échouer dans l’incapacité de votre application pouvoir être installés par vos clients.

### <a name="determine-the-subject-of-your-packaged-app"></a>Déterminer l’objet de votre application empaquetée  

Pour utiliser un certificat de signature de votre package d’applications, « l'objet » contenu dans le certificat **doit** correspondre à la section « Publisher » du manifeste de votre application.

Par exemple, la section « Identity » dans le fichier AppxManifest.xml de votre application doit ressembler à ceci :

```xml
  <Identity Name="Contoso.AssetTracker" 
    Version="1.0.0.0" 
    Publisher="CN=Contoso Software, O=Contoso Corporation, C=US"/>
```

L'entité « Publisher », dans ce cas, est « CN = Contoso Software, O = Contoso Corporation, C = US » ; c'est cette entité que vous devez utiliser pour la création de votre certificat.

### <a name="use-new-selfsignedcertificate-to-create-a-certificate"></a>Utiliser **New-SelfSignedCertificate** pour créer un certificat

Utilisez l'applet de commande PowerShell **New-SelfSignedCertificate** pour créer un certificat auto-signé. L'applet de commande **New-SelfSignedCertificate** comporte plusieurs paramètres de personnalisation, mais dans le cadre de cet article, nous allons créer un simple certificat qui fonctionne avec **SignTool**. Pour plus d’exemples et d’utilisations de cette applet de commande, voir [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/New-SelfSignedCertificate).

À partir du fichier AppxManifest.xml de l’exemple précédent, vous devez utiliser la syntaxe suivante pour créer un certificat. Dans une invite PowerShell avec élévation de privilèges :

```powershell
New-SelfSignedCertificate -Type Custom -Subject "CN=Contoso Software, O=Contoso Corporation, C=US" -KeyUsage DigitalSignature -FriendlyName "Your friendly name goes here" -CertStoreLocation "Cert:\CurrentUser\My" -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.3", "2.5.29.19={text}")
```

Notez les détails suivants sur certains des paramètres :

- **Utilisation de clé**: Ce paramètre définit ce que le certificat peut être utilisé pour. Pour un certificat d’auto-signature, ce paramètre doit être défini sur **DigitalSignature**.

- **Valeur TextExtension**: Ce paramètre inclut des paramètres pour les extensions suivantes :

  - Utilisation avancée de la clé (EKU) : Cette extension indique d’autres usages pour lequel la clé publique certifiée peut être utilisée. Pour un certificat d’auto-signature, ce paramètre doit inclure la chaîne d’extension **« 2.5.29.37={text}1.3.6.1.5.5.7.3.3 »** , ce qui indique que le certificat doit être utilisé pour la signature de code.

  - Contraintes de base : Cette extension indique si le certificat est une autorité de certification (CA). Pour un certificat d’auto-signature, ce paramètre doit inclure la chaîne d’extension **« 2.5.29.19={text} »** , ce qui indique que le certificat est une entité de fin (pas une autorité de certification).

Après avoir exécuté cette commande, le certificat sera ajouté au magasin de certificats local, tel qu’indiqué dans le paramètre « -CertStoreLocation ». Le résultat de la commande génère également l’empreinte du certificat.  

Vous pouvez afficher votre certificat dans une fenêtre PowerShell en utilisant les commandes suivantes :

```powershell
Set-Location Cert:\CurrentUser\My
Get-ChildItem | Format-Table Subject, FriendlyName, Thumbprint
```

Cela affichera tous les certificats contenus dans votre magasin local.

## <a name="export-a-certificate"></a>Exporter un certificat 

Pour exporter le certificat du magasin local dans un fichier Personal Information Exchange (PFX), utilisez l'applet de commande **Export-PfxCertificate**.

Lorsque vous utilisez **Export-PfxCertificate**, vous devez créer et utiliser un mot de passe ou utiliser le paramètre « -ProtectTo » pour spécifier les utilisateurs ou groupes qui peuvent accéder au fichier sans mot de passe. Notez qu’une erreur s’affiche si vous n’utilisez pas le paramètre « -Password » ou «-ProtectTo ».

### <a name="password-usage"></a>Utilisation du paramètre « Password »

```powershell
$pwd = ConvertTo-SecureString -String <Your Password> -Force -AsPlainText 
Export-PfxCertificate -cert "Cert:\CurrentUser\My\<Certificate Thumbprint>" -FilePath <FilePath>.pfx -Password $pwd
```

### <a name="protectto-usage"></a>Utilisation du paramètre « ProtectTo »

```powershell
Export-PfxCertificate -cert Cert:\CurrentUser\My\<Certificate Thumbprint> -FilePath <FilePath>.pfx -ProtectTo <Username or group name>
```

Après avoir créé et exporté votre certificat, vous êtes prêt à signer votre package d’applications avec **SignTool**. Pour l’étape suivante du processus d'empaquetage manuel, consultez [Signer un package d'application à l'aide de SignTool](https://docs.microsoft.com/windows/uwp/packaging/sign-app-package-using-signtool).

## <a name="security-considerations"></a>Considérations relatives à la sécurité

En ajoutant un certificat à des [magasins de certificats d'un ordinateur local](https://docs.microsoft.com/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores), vous affectez la fiabilité du certificat pour tous les utilisateurs de l’ordinateur. Il est recommandé de supprimer ces certificats lorsque vous n'en avez plus besoin, ce afin d'éviter qu'ils ne soient utilisés pour compromettre la fiabilité du système.
