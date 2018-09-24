---
title: Clés de chiffrement
description: Cet article montre comment utiliser les fonctions de dérivation de clés standard, et chiffrer du contenu à l’aide de clés symétriques et asymétriques.
ms.assetid: F35BEBDF-28C5-4F91-A94E-F7D862B6ED59
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, sécurité
ms.localizationpriority: medium
ms.openlocfilehash: 24f41ac858e73041e5afb4db596ce52b7d9bf4d8
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2018
ms.locfileid: "4153020"
---
# <a name="cryptographic-keys"></a>Clés de chiffrement




Cet article montre comment utiliser les fonctions de dérivation de clés standard, et chiffrer du contenu à l’aide de clés symétriques et asymétriques. 

## <a name="symmetric-keys"></a>Clés symétriques


Le chiffrement à clé symétrique, également appelé chiffrement à clé secrète, exige que la clé utilisée pour le chiffrement le soit également pour le déchiffrement. Vous pouvez utiliser une classe [**SymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241537) pour spécifier un algorithme symétrique et créer ou importer une clé. Vous pouvez utiliser des méthodes statiques sur la classe [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) pour chiffrer et déchiffrer des données à l’aide de l’algorithme et de la clé.

Le chiffrement à clé symétrique fait généralement appel à des chiffrements par bloc et à des modes de chiffrement par bloc. Un chiffrement par bloc est une fonction de chiffrement symétrique qui opère sur des blocs de taille fixe. Si la longueur du message que vous souhaitez chiffrer est supérieure à la longueur du bloc, vous devez utiliser un mode de chiffrement par bloc. Un mode de chiffrement par bloc est une fonction de chiffrement symétrique générée à l’aide d’un chiffrement par bloc. Il chiffre le texte brut en une série de blocs de taille fixe. Les modes suivants sont pris en charge pour les applications :

-   Le mode ECB (Electronic Codebook) chiffre chaque bloc du message séparément. Il n’est pas considéré comme un mode de chiffrement sécurisé.
-   Le mode CBC (Cipher Block Chaining) utilise le bloc de texte chiffré précédent pour brouiller le bloc actif. Vous devez déterminer la valeur à utiliser pour le premier bloc. Cette valeur est appelée « vecteur d’initialisation » (VI).
-   Le mode CCM (Counter with CBC-MAC) associe le mode de chiffrement par bloc CBC à un code d’authentification de message (MAC).
-   Le mode GCM (Galois Counter Mode) associe le mode de chiffrement par compteur au mode d’authentification Galois.

Certains modes comme le mode CBC requièrent l’utilisation d’un vecteur d’initialisation (VI) pour le premier bloc de texte chiffré. Vous trouverez ci-après les vecteurs d’initialisation courants. Vous spécifiez le VI lorsque vous appelez [**CryptographicEngine.Encrypt**](https://msdn.microsoft.com/library/windows/apps/br241494). Dans la plupart des cas, il est important que le VI ne soit jamais réutilisé avec la même clé.

-   Le type fixe utilise le même VI pour tous les messages à chiffrer. Ce type divulgue des informations et son utilisation n’est pas recommandée.
-   Le type compteur incrémente le VI pour chaque bloc.
-   Le type aléatoire crée un VI pseudo-aléatoire. Vous pouvez utiliser [**CryptographicBuffer.GenerateRandom**](https://msdn.microsoft.com/library/windows/apps/br241392) pour créer le VI.
-   Le type généré par une valeur à usage unique utilise un numéro unique pour chaque message à chiffrer. En général, la valeur à usage unique est un identificateur de message ou de transaction modifié. Il n’est pas nécessaire de la garder secrète, mais elle ne doit jamais être réutilisée sous la même clé.

La plupart des modes requièrent que la longueur du texte brut soit un multiple exact de la taille du bloc. Cela implique généralement d’étoffer le texte brut afin d’obtenir la longueur appropriée.

Tandis que le chiffrement par bloc permet de chiffrer des blocs de données de taille fixe, le chiffrement par flux correspond à des fonctions de chiffrement symétrique qui combinent des bits de texte brut à un flux binaire pseudo-aléatoire (appelé séquence de clé) pour générer le texte chiffré. Certains modes de chiffrement par bloc comme le mode OFB (Output Feedback) et le mode CTR (Counter) transforment en effet un chiffrement par bloc en chiffrement par flux. Le chiffrement par flux réel tel que RC4, en revanche, fonctionne en général à des vitesses supérieures à celles que les modes de chiffrement par bloc sont capables d’atteindre.

L’exemple suivant montre comment utiliser la classe [**SymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241537) pour créer une clé symétrique et l’utiliser afin de chiffrer et de déchiffrer des données.

## <a name="asymmetric-keys"></a>Clés asymétriques


Le chiffrement à clé asymétrique, ou chiffrement à clé publique, utilise une clé publique et une clé privée pour chiffrer et déchiffrer les données. Bien qu’elles soient différentes, ces clés sont liées mathématiquement. Généralement, la clé privée est gardée secrète et sert à déchiffrer les données, alors que la clé publique est transmise aux parties concernées pour les besoins de chiffrement des données. Le chiffrement à clé asymétrique est également utile pour signer les données.

Le chiffrement asymétrique étant beaucoup plus lent que le chiffrement symétrique, il est rarement utilisé pour chiffrer de grandes quantités de données directement. Il est généralement plutôt utilisé selon les manières suivantes pour chiffrer des clés.

-   Alice nécessite que Bob ne lui envoie que des messages chiffrés.
-   Alice crée une paire de clés privées ou publiques, garde sa clé privée secrète et publie sa clé publique.
-   Bob veut envoyer un message à Alice.
-   Bob crée une clé symétrique.
-   Bob utilise une nouvelle clé symétrique pour chiffrer son message pour Alice.
-   Bob utilise la clé publique d’Alice pour chiffrer sa clé symétrique.
-   Bob envoie le message chiffré et la clé symétrique chiffrée à Alice (enveloppée).
-   Alice utilise sa clé privée (à partir de sa paire de clés privées ou publiques) pour déchiffrer la clé symétrique de Bob.
-   Alice utilise la clé symétrique de Bob pour déchiffrer le message.

Vous pouvez utiliser un objet [**AsymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241478) pour spécifier un algorithme asymétrique ou un algorithme de signature, pour créer ou importer une paire de clés éphémère ou pour importer la clé publique d’une paire de clés.

## <a name="deriving-keys"></a>Dérivation de clés


Il est souvent nécessaire de dériver des clés supplémentaires à partir d’un secret partagé. Vous pouvez utiliser la classe [**KeyDerivationAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241518) et l’une des méthodes spécialisées suivantes dans la classe [**KeyDerivationParameters**](https://msdn.microsoft.com/library/windows/apps/br241524) pour dériver des clés.

| Objet                                                                            | Description                                                                                                                                |
|-----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| [**BuildForPbkdf2**](https://msdn.microsoft.com/library/windows/apps/br241525)    | Crée un objet KeyDerivationParameters à utiliser dans la fonction2 de dérivation de clé reposant sur un mot de passe (PBKDF2).                                 |
| [**BuildForSP800108**](https://msdn.microsoft.com/library/windows/apps/br241526)  | Crée un objet KeyDerivationParameters à utiliser dans une fonction de dérivation de clé HMAC en mode compteur. |
| [**BuildForSP80056a**](https://msdn.microsoft.com/library/windows/apps/br241527)  | Crée un objet KeyDerivationParameters à utiliser dans une fonction de dérivation de clé SP800-56A.                                                 |

 
