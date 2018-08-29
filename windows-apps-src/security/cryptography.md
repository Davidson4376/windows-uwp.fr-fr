---
title: Chiffrement
description: L’article fournit une vue d’ensemble des fonctionnalités de chiffrement disponibles pour les applications de plateforme Windows universelle (UWP). Pour plus d’informations sur des tâches particulières, voir le tableau à la fin de cet article.
ms.assetid: 9C213036-47FD-4AA4-99E0-84006BE63F47
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, sécurité
ms.localizationpriority: medium
ms.openlocfilehash: aa01cc3d70db7a94667e944d1a1739e911f94b0c
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/29/2018
ms.locfileid: "2917479"
---
# <a name="cryptography"></a>Chiffrement




L’article fournit une vue d’ensemble des fonctionnalités de chiffrement disponibles pour les applications de plateforme Windows universelle (UWP). Pour plus d’informations sur des tâches particulières, voir le tableau à la fin de cet article.

## <a name="terminology"></a>Terminology


The following terminology is commonly used in cryptography and public key infrastructure (PKI).

| Term                        | Description                                                                                                                                                                                           |
|-----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Encryption                  | The process of transforming data by using a cryptographic algorithm and key. The transformed data can be recovered only by using the same algorithm and the same (symmetric) or related (public) key. |
| Decryption                  | The process of returning encrypted data to its original form.                                                                                                                                         |
| Plaintext                   | Originally referred to an unencrypted text message. Currently refers to any unencrypted data.                                                                                                         |
| Ciphertext                  | Originally referred to an encrypted, and therefore unreadable, text message. Currently refers to any encrypted data.                                                                                  |
| Hashing                     | The process of converting variable length data into a fixed length, typically smaller, value. By comparing hashes, you can obtain reasonable assurance that two or more data are the same.            |
| Signature                   | Encrypted hash of digital data typically used to authenticate the sender of the data or verify that the data was not tampered with during transmission.                                               |
| Algorithm                   | A step-by-step procedure for encrypting data.                                                                                                                                                         |
| Key                         | A random or pseudorandom number used as input to a cryptographic algorithm to encrypt and decrypt data.                                                                                               |
| Symmetric Key Cryptography  | Cryptography in which encryption and decryption use the same key. This is also known as secret key cryptography.                                                                                      |
| Asymmetric Key Cryptography | Cryptography in which encryption and decryption use a different but mathematically related key. This is also called public key cryptography.                                                          |
| Encoding                    | The process of encoding digital messages, including certificates, for transport across a network.                                                                                                     |
| Algorithm Provider          | A DLL that implements a cryptographic algorithm.                                                                                                                                                      |
| Key Storage Provider        | A container for storing key material. Currently, keys can be stored in software, smart cards, or the trusted platform module (TPM).                                                                   |
| X.509 Certificate           | A digital document, typically issued by a certification authority, to verify the identity of an individual, system, or entity to other interested parties.                                            |

 
## <a name="namespaces"></a>Namespaces

The following namespaces are available for use in apps.

### <a name="windowssecuritycryptography"></a>Windows.Security.Cryptography

Contains the CryptographicBuffer class and static methods that enable you to:

-   Convert data to and from strings
-   Convert data to and from byte arrays
-   Encode messages for network transport
-   Decode messages after transport

### <a name="windowssecuritycryptographycertificates"></a>Windows.Security.Cryptography.Certificates

Contains classes, interfaces, and enumeration types that enable you to:

-   Create a certificate request
-   Install a certificate response
-   Import a certificate in a PFX file
-   Specify and retrieve certificate request properties

### <a name="windowssecuritycryptographycore"></a>Windows.Security.Cryptography.Core

Contains classes and enumeration types that enable you to:

-   Encrypt and decrypt data
-   Hash data
-   Sign data and verify signatures
-   Create, import, and export keys
-   Work with asymmetric key algorithm providers
-   Work with symmetric key algorithm providers
-   Work with hash algorithm providers
-   Work with machine authentication code (MAC) algorithm providers
-   Work with key derivation algorithm providers

### <a name="windowssecuritycryptographydataprotection"></a>Windows.Security.Cryptography.DataProtection

Contains classes that enable you to:

-   Asynchronously encrypt and decrypt static data
-   Asynchronously encrypt and decrypt data streams

## <a name="crypto-and-pki-application-capabilities"></a>Crypto and PKI application capabilities


The simplified application programming interface available for apps enables the following cryptographic and public key infrastructure (PKI) capabilities.

### <a name="cryptography-support"></a>Cryptography support

Vous pouvez effectuer les tâches de chiffrement suivantes. Pour plus d’informations, voir l’espace de noms [**Windows.Security.Cryptography.Core**](https://msdn.microsoft.com/library/windows/apps/br241547).

-   Créer des clés symétriques
-   Perform symmetric encryption
-   Create asymmetric keys
-   Perform asymmetric encryption
-   Derive password based keys
-   Create message authentication codes (MACs)
-   Hash content
-   Digitally sign content

The SDK also provides a simplified interface for password-based data protection. Vous pouvez l’utiliser pour effectuer les tâches suivantes. Pour plus d’informations, voir l’espace de noms [**Windows.Security.Cryptography.DataProtection**](https://msdn.microsoft.com/library/windows/apps/br241585).

-   Protection asynchrone des données statiques
-   Asynchronous protection of a data stream

### <a name="encoding-support"></a>Prise en charge du codage

Une application permet de coder les données de chiffrement avant leur transfert via un réseau et de décoder les données provenant d’une source réseau. Pour plus d’informations, voir les méthodes statiques disponibles dans l’espace de noms [**Windows.Security.Cryptography**](https://msdn.microsoft.com/library/windows/apps/br241404).

### <a name="pki-support"></a>Prise en charge de l’infrastructure à clé publique (PKI)

Les applications pouvez effectuer les tâches PKI suivantes. Pour plus d’informations, voir l’espace de noms [**Windows.Security.Cryptography.Certificates**](https://msdn.microsoft.com/library/windows/apps/br241476).

-   Créer un certificat
-   Create a self-signed certificate
-   Install a certificate response
-   Import a certificate in PFX format
-   Use smart card certificates and keys (sharedUserCertificates capabilities set)
-   Use certificates from the user MY store (sharedUserCertificates capabilities set)

Additionally, you can use the manifest to perform the following actions:

-   Specify per application trusted root certificates
-   Specify per application peer trusted certificates
-   Explicitly disable inheritance from system trust
-   Specify the certificate selection criteria
    -   Hardware certificates only
    -   Certificates that chain through a specified set of issuers
    -   Sélectionner automatiquement un certificat dans le magasin d’applications

## <a name="detailed-articles"></a>Articles détaillés


Les articles suivants fournissent plus de détails sur la sécurité:

| Rubrique                                                                         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|-------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Certificats](certificates.md)                                               | Cet article traite de l’utilisation des certificats dans les applications UWP. Les certificats numériques sont utilisés dans le chiffrement à clé publique pour lier une clé publique à une personne, un ordinateur ou une organisation. Les identités liées sont le plus souvent utilisées pour authentifier une entité auprès d’une autre. Par exemple, les certificats sont souvent utilisés pour authentifier un serveur Web auprès d’un utilisateur ou vice versa. Vous pouvez créer des demandes de certificat et installer ou importer des certificats émis. Vous pouvez aussi inscrire un certificat dans une hiérarchie de certificats. |
| [Clés de chiffrement](cryptographic-keys.md)                                   | Cet article montre comment utiliser les fonctions de dérivation de clés standard, et chiffrer du contenu à l’aide de clés symétriques et asymétriques.                                                                                                                                                                                                                                                                                                                                                                             |
| [Protection des données](data-protection.md)                                         | Cet article explique comment utiliser la classe [DataProtectionProvider](https://msdn.microsoft.com/library/windows/apps/br241559) dans l’espace de noms [Windows.Security.Cryptography.DataProtection](https://msdn.microsoft.com/library/windows/apps/br241585) pour chiffrer et déchiffrer des données numériques dans une application UWP.                                                                                                                                                                                                                  |
| [Codes d’authentification des messages, hachages et signatures](macs-hashes-and-signatures.md)               | Cet article explique comment les codes d’authentification des messages, les hachages et les signatures peuvent être utilisés dans les applications UWP pour détecter une falsification des messages.                                                                                                                                                                                                                                                                                                                                                                                |
| [Restrictions à l’exportation liées à l’utilisation du chiffrement](export-restrictions-on-cryptography.md) | Utilisez ces informations pour déterminer si votre application emploie un type de chiffrement qui pourrait l’empêcher de figurer dans le Microsoft Store.                                                                                                                                                                                                                                                                                                                                                                                            |
| [Tâches courantes de chiffrement](common-cryptography-tasks.md)                     | Ces articles fournissent des exemples de code pour les tâches de chiffrement UWP courantes, telles que la création de nombres aléatoires, la comparaison de mémoires tampon, la conversion entre chaînes et données binaires, la copie de tableaux d’octets, et le codage/décodage de données.                                                                                                                                                                                                                                                                                    |

 