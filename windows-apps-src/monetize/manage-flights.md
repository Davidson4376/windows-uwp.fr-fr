---
author: Xansky
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: Utilisez ces méthodes dans l’API de soumission au Microsoft Store pour gérer les versions d’évaluation de package pour les applications qui sont enregistrées sur votre compte espace partenaires.
title: Gérer les versions d’évaluation de package
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de soumission au MicrosoftStore, versions d'évaluation
ms.localizationpriority: medium
ms.openlocfilehash: 1f1300151d8b50a0a9e192c2090e9a3d72afa86e
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5988779"
---
# <a name="manage-package-flights"></a>Gérer les versions d’évaluation de package

Utilisez les méthodes suivantes dans l’API de soumission au MicrosoftStore pour gérer les versions d’évaluation de package de vos apps. Pour obtenir une présentation de l’API de soumission au MicrosoftStore, notamment les conditions préalables à l’utilisation de l’API, voir [Créer et gérer des soumissions à l’aide des services au MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md).

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
<td align="left"><a href="get-a-flight.md">Obtient une version d’évaluation du package</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights</td>
<td align="left"><a href="create-a-flight.md">Crée une version d’évaluation du package</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}</td>
<td align="left"><a href="delete-a-flight.md">Supprime une version d’évaluation du package</a></td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>Prérequis

Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au MicrosoftStore avant d’essayer d’utiliser l’une de ces méthodes.

## <a name="related-topics"></a>Rubriquesassociées

* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md)
