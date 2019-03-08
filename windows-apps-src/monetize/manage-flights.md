---
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: Utilisez ces méthodes dans l’API de soumission de Microsoft Store pour gérer les vols de package pour les applications qui sont inscrits dans votre compte espace partenaires.
title: Gérer les versions d’évaluation de package
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, versions d'évaluation
ms.localizationpriority: medium
ms.openlocfilehash: 8678ee4d73f13e241a2c72d6dac532289af13ced
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601244"
---
# <a name="manage-package-flights"></a>Gérer les versions d’évaluation de package

Utilisez les méthodes suivantes dans l’API de soumission au Microsoft Store pour gérer les versions d’évaluation de package de vos apps. Pour obtenir une présentation de l’API de soumission au Microsoft Store, notamment les conditions préalables à l’utilisation de l’API, voir [Créer et gérer des soumissions à l’aide des services au Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

Ces méthodes peuvent uniquement être utilisées pour obtenir, créer ou supprimer des versions d’évaluation de package. Pour créer des soumissions pour des versions d’évaluation de package, voir les méthodes présentées dans l’article [Gérer les soumissions de version d’évaluation de package](manage-flight-submissions.md).

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Méthode</th>
<th align="left">URI</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}</td>
<td align="left"><a href="get-a-flight.md">Obtenir un vol de package</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights</td>
<td align="left"><a href="create-a-flight.md">Créer un package de vol</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}</td>
<td align="left"><a href="delete-a-flight.md">Supprimer un vol de package</a></td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>Conditions préalables

Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au Microsoft Store avant d’essayer d’utiliser l’une de ces méthodes.

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les envois de vol de package](manage-flight-submissions.md)
