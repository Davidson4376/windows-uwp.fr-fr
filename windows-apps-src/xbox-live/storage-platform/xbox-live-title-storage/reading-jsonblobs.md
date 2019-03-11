---
title: Lecture d’un objet blob JSON
description: Découvrez comment lire un objet blob JSON dans le stockage de titre Xbox Live.
ms.assetid: 3697af16-d054-4835-af7f-7fee8c628345
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 30d2b9f6539e73df1c5ea5ae18b3d1a6ca061d60
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613484"
---
# <a name="reading-a-json-blob-in-xbox-live-title-storage"></a>Lecture d’un objet blob JSON dans le stockage de titre Xbox Live

1.  Envoyer une demande à l’aide de la *obtenir* méthode pour lire les données à partir du stockage de titre. Cet exemple utilise le stockage de titre global.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/surprise.json,json
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive

-   L’utilisateur doit être dans la session de mise à jour.

-   STSTokenString est un espace réservé par souci de concision et doit être remplacé par le jeton retourné par la demande d’authentification.

#### <a name="reference"></a>Référence

**/global/scids/{scid}/data/{pathAndFileName},{type}**
