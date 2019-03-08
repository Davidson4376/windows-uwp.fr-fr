---
title: Stockage d’un objet blob JSON
description: Découvrez comment stocker un objet blob JSON dans le stockage de titre Xbox Live.
ms.assetid: f424aca1-e671-4e31-84c6-098fda4a9558
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, stockage de titre
ms.localizationpriority: medium
ms.openlocfilehash: dabcd0634bb20bc6b82ae302c77338b5bb726a63
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595064"
---
# <a name="storing-a-json-blob-in-xbox-live-title-storage"></a>Stocker un objet blob JSON dans le stockage de titre Xbox Live

1.  Envoyer une demande à l’aide de la *PUT* méthode pour envoyer les données vers le stockage de titre.

        PUT https://titlestorage.xboxlive.com/json/users/xuid(1245111)/scids/{scid}/data/{pathAndFileName},json
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Content-Length: 240
        Connection: Keep-Alive



-   L’utilisateur doit être dans la session de mise à jour.

-   STSTokenString est un espace réservé par souci de concision et doit être remplacé par le jeton retourné par la demande d’authentification.

2.  Envoyez un objet JSON.

        {
            "startlevel":"1",
            "expression":"smile"
        }

#### <a name="reference"></a>Référence

**/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)**
