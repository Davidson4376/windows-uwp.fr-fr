---
title: Lecture d’un objet blob de configuration
description: Découvrez comment lire un objet blob de configuration dans le stockage de titre Xbox Live.
ms.assetid: ee62d221-69b9-4f52-9b5d-5a44d04de548
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 71607f05f07ee4a98f73a19c28c85dc325041a10
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599394"
---
# <a name="reading-a-configuration-blob-in-xbox-live-title-storage"></a>Lecture d’un objet blob de configuration dans le stockage de titre Xbox Live

*Objets BLOB de configuration* sont des fichiers dans le stockage de titre Xbox Live qui contiennent des données de jeu. Les données sont des objets JSON qui incluent des nœuds virtuels qui peuvent être filtrés avant d’être remises au jeu. Pour plus d’informations sur les objets BLOB de configuration, consultez **titre stockage URI**.

### <a name="to-read-a-configuration-blob"></a>Pour lire un objet blob de configuration

1.  Envoyer une demande à l’aide de la sous méthode pour lire les données à partir du stockage de titre.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/config.json,config              
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive


-   L’utilisateur doit être dans la session de mise à jour.
-   STSTokenString est un espace réservé par souci de concision et doit être remplacé par le jeton retourné par la demande d’authentification.

#### <a name="reference"></a>Référence

**/ global/scids / {scid} /data/ {pathAndFileName}, {type}**
**obtenir (/ global/scids / {scid} /data/ {pathAndFileName}, {type})**
