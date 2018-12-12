---
title: Présentation de certificats
description: Cet article traite de l’utilisation de certificats dans les applications de plateforme Windows universelle (UWP).
ms.assetid: 4EA2A9DF-BA6B-45FC-AC46-2C8FC085F90D
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, sécurité
ms.localizationpriority: medium
ms.openlocfilehash: 2ee96628fd90ec9eea998abf312c5da11bff3826
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8937414"
---
# <a name="intro-to-certificates"></a>Présentation de certificats




Cet article traite de l’utilisation de certificats dans les applications de plateforme Windows universelle (UWP). Les certificats numériques sont utilisés dans le chiffrement à clé publique pour lier une clé publique à une personne, un ordinateur ou une organisation. Les identités liées sont le plus souvent utilisées pour authentifier une entité auprès d’une autre. Par exemple, les certificats sont souvent utilisés pour authentifier un serveur Web auprès d’un utilisateur ou vice versa. Vous pouvez créer des demandes de certificat et installer ou importer des certificats émis. Vous pouvez aussi inscrire un certificat dans une hiérarchie de certificats.

### <a name="shared-certificate-stores"></a>Magasins de certificats partagés

Applications UWP utilisent le nouveau modèle d’application «isolationniste» introduit dans le package Windows8. Selon ce modèle, une application de s’exécuter dans une construction de système d’exploitation de bas niveau, appelée « conteneur d’application », qui empêche l’application d’accéder à des ressources ou des fichiers extérieurs, sauf autorisation explicite de le faire. Les sections suivantes décrivent les conséquences de ce modèle sur l’infrastructure à clé publique (PKI).

### <a name="certificate-storage-per-app-container"></a>Stockage de certificats par conteneur d’application

Les certificats qui sont destinés à être utilisés dans un conteneur d’application spécifique sont stockés dans des emplacements de conteneurs par utilisateur et par application. Une application s’exécutant dans un conteneur d’application ne dispose d’un accès en écriture que pour son propre magasin de certificats. Si l’application ajoute des certificats à n’importe quel de ses magasins, ces certificats ne peuvent pas être lus par d’autres applications. Si une application est désinstallée, tout certificat qui lui est spécifique est également supprimé. De même, une application ne dispose d’un accès en lecture que pour les magasins de certificats de l’ordinateur local autres que le magasin MY et REQUEST.

### <a name="cache"></a>Cache

Chaque conteneur d’application a un cache isolé dans lequel il peut stocker les certificats d’émetteur nécessaires pour la validation, les listes de révocation de certificats et les réponses OSCP (Online Certificate Status Protocol).

### <a name="shared-certificates-and-keys"></a>Certificats et clés partagés

Lorsqu’une carte à puce est insérée dans un lecteur, les certificats et clés contenus sur la carte sont propagés au magasin MY de l’utilisateur où ils peuvent être partagés par toute application de confiance totale que l’utilisateur exécute. Par défaut, toutefois, les conteneurs d’applications n’ont pas accès au magasin MY par utilisateur.

Pour résoudre ce problème et permettre à des groupes de principaux d’accéder à des groupes de ressources, le modèle d’isolation des conteneurs d’applications prend en charge le concept des fonctionnalités. Une fonctionnalité permet à un processus de conteneur d’applications d’accéder à une ressource spécifique. La fonctionnalité sharedUserCertificates octroie à un conteneur d’application un accès en lecture aux certificats et clés contenus dans le magasin MY de l’utilisateur et dans le magasin Racines de confiance de carte à puce. La fonctionnalité n’octroie pas d’accès en lecture au magasin REQUEST de l’utilisateur.

Vous devez spécifier la fonctionnalité sharedUserCertificates dans le manifeste, comme le montre l’exemple suivant.

```xml
<Capabilities>
    <Capability Name="sharedUserCertificates" />
</Capabilities>
```

## <a name="certificate-fields"></a>Champs Certificat


La norme de certificat de clé publique X.509 a été révisée au fil du temps. Chaque version successive de la structure de données a conservé les champs existants dans les versions précédentes et en a ajouté plusieurs, tel qu’illustré ci-dessous.

![certificat x.509 versions 1, 2 et 3](images/x509certificateversions.png)

Certains de ces champs et extensions peuvent être spécifiés directement lorsque vous utilisez la classe [**CertificateRequestProperties**](https://msdn.microsoft.com/library/windows/apps/br212079) pour créer une demande de certificat. La plupart ne peuvent pas. Ces champs peuvent être complétés par l’autorité émettrice ou rester vides. Pour plus d’informations sur les champs, voir les sections suivantes :

### <a name="version-1-fields"></a>Champs version 1

| Champ | Description |
|-------|-------------|
| Version | Indique le numéro de version du certificat encodé. Actuellement les valeurs possibles pour ce champ sont 0, 1 ou 2. |
| Numéro de série | Contient un entier positif unique attribué au certificat par l’autorité de certification. |
| Algorithme de signature | Contient un identificateur d’objet (OID) qui indique l’algorithme utilisé par l’autorité de certification pour signer le certificat. Par exemple, 1.2.840.113549.1.1.5 indique un algorithme de hachage SHA-1 associé à l’algorithme de chiffrement RSA de RSA Laboratories. |
| Émetteur | Contient le nom unique X.500 de l’autorité de certification qui a créé et signé le certificat. |
| Validité | Indique l’intervalle de temps pendant lequel le certificat est valable. Les dates jusqu’à fin 2049 utilisent le format UTC (temps universel coordonné) (Heure de Greenwich) (yymmddhhmmssz). Les dates à partir du 1er janvier 2050 utilisent le format de temps généralisé (yyyymmddhhmmssz). |
| Sujet | Contient le nom unique X.500 de l’entité associée à la clé publique incluse dans le certificat. |
| Clé publique | Contient la clé publique et des informations sur l’algorithme associé. |

### <a name="version-2-fields"></a>Champs version 2

Un certificat X.509 version 2 contient les champs de base définis dans la version 1, ainsi que les champs supplémentaires suivants.

| Champ | Description |
|-------|-------------|
| Identificateur unique de l’émetteur | Contient une valeur unique pouvant être utilisée pour rendre le nom X.500 de l’autorité de certification non ambigu lorsqu’il est réutilisé par différentes entités au fil du temps. |
| Identificateur unique du sujet | Contient une valeur unique pouvant être utilisée pour rendre le nom X.500 du sujet du certificat non ambigu lorsqu’il est réutilisé par différentes entités au fil du temps. |

### <a name="version-3-extensions"></a>Extensions version 3

Un certificat X.509 version 3 contient les champs définis dans les versions 1 et 2 et ajoute des extensions de certificat.

| Champ  | Description |
|--------|-------------|
| Identificateur de clé de l’autorité | Identifie la clé publique de l’autorité de certification qui correspond à la clé privée de l’autorité de certification utilisée pour signer le certificat. |
| Contraintes de base | Indique si l’entité peut être utilisée en tant qu’autorité de certification et, le cas échéant, le nombre d’autorités de certification secondaires pouvant exister en dessous dans la chaîne de certificats. |
| Stratégies de certificat | Indique les stratégies sous lesquelles le certificat a été émis et les fins auxquelles il peut être utilisé. |
| Points de distribution de la liste de révocation de certificats | Contient l’URI de la liste de révocation de certificats de base. |
| Utilisation améliorée de la clé | Indique la façon dont la clé publique contenue dans le certificat peut être utilisée. |
| Autre nom de l’émetteur | Indique un ou plusieurs autres noms pour l’émetteur de la demande de certificat. |
| Utilisation de la clé | Indique des restrictions sur les opérations pouvant être effectuées par la clé publique contenue dans le certificat.|
| Contraintes de nom  | Indique l’espace de noms dans lequel tous les noms de sujets d’une hiérarchie de certificats doivent se trouver. L’extension n’est utilisée que dans un certificat d’autorité de certification. |
| Contraintes de stratégie | Contraint la validation du chemin d’accès en interdisant le mappage de stratégie ou en exigeant que tous les certificats de la hiérarchie contiennent un identificateur de stratégie acceptable. L’extension n’est utilisée que dans un certificat d’autorité de certification. |
| Mappages de stratégie | Indique les stratégies dans une autorité de certification secondaire qui correspondent aux stratégies dans l’autorité de certification émettrice. |
| Durée de l’utilisation de la clé privée | Indique une durée de validité pour la clé privée qui est différente de celle du certificat auquel la clé privée est associée. |
| Autre nom de l’objet | Indique une ou plusieurs autres formes de noms pour le sujet de la demande de certificat. Parmi les autres formes, citons : adresses de messagerie, noms DNS, adresses IP et URI. |
| Attributs d’annuaire du sujet | Véhicule des attributs d’identification tels que la nationalité du sujet du certificat. La valeur de l’extension est une séquence de paires OID-valeur. |
| Identificateur de clé du sujet | Fait la distinction entre plusieurs clés publiques détenues par le sujet du certificat. La valeur de l’extension est généralement un hachage SHA-1 de la clé. |

