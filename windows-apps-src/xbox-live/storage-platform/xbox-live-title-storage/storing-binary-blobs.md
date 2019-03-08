---
title: Récupération d’un blob binaire
description: Découvrez comment stocker un objet blob binaire dans le stockage de titre Xbox Live.
ms.assetid: a0da36ef-5a5a-466d-80a8-e055ba7e4cdc
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, stockage de titre
ms.localizationpriority: medium
ms.openlocfilehash: 70e620f2e992dfdd240f71b5c73165f13d4ef5cb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660094"
---
# <a name="storing-a-binary-blob-in-xbox-live-title-storage"></a>Récupération d’un blob binaire dans le stockage de titre Xbox Live

1.  Envoyer une demande à l’aide de la sous méthode pour envoyer les données vers le stockage de titre.

        PUT https://titlestorage.xboxlive.com/trustedplatform/users/xuid(1245111)/scids/{scid}/data/lastturn.bin,binary              
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Content-Length: 40
        Connection: Keep-Alive


-   L’utilisateur doit être dans la session de mise à jour.

-   STSTokenString est un espace réservé par souci de concision et doit être remplacé par le jeton retourné par la demande d’authentification.

2.  Envoyer les données binaires. Dans la mesure où les données sont transférées via HTTP, les données doivent être limitées au jeu de caractères acceptable. Des informations telles que des images ou des données audio doivent être encodées. Vous pouvez sélectionner n’importe quelle méthode d’encodage qui génère les caractères compatibles HTTP.
d
```
  01EAEFBAD05903A4
  1EA2311656677DFF
  CF00
```

#### <a name="reference"></a>Référence

**/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}**
