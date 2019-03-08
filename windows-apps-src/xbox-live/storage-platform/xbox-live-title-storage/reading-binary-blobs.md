---
title: Lire un blob binaire
description: En savoir plus sur la lecture d’un blob binaire dans le stockage de titre Xbox Live.
ms.assetid: 9b8e0c35-0cea-4491-bf30-22fad224f11b
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, stockage de titre
ms.localizationpriority: medium
ms.openlocfilehash: e2a0be72142a3e11c7c680cfc998287f396c2afc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641654"
---
# <a name="reading-a-binary-blob-in-xbox-live-title-storage"></a>Lire un blob binaire dans le stockage de titre Xbox Live

1.  Envoyer une demande à l’aide de la *obtenir* méthode pour lire les données à partir du stockage de titre. Cet exemple utilise le stockage de titre global.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/userinfo.bin,binary
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive



-   L’utilisateur doit être dans la session de mise à jour.

-   STSTokenString est un espace réservé par souci de concision et doit être remplacé par le jeton retourné par la demande d’authentification.

#### <a name="reference"></a>Référence

**/global/scids/{scid}/data/{pathAndFileName},{type}**
