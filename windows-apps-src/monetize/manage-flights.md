---
author: mcleanbyron
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: "Utilisez ces méthodes dans l’API de soumission du Windows Store pour gérer les versions d’évaluation de package pour les applications inscrites dans votre compte du Centre de développement Windows."
title: "Gérer les versions d’évaluation de package"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de soumission du Windows Store, versions d&quot;évaluation"
ms.openlocfilehash: b560c2c12dc2fd7984287d039d20c121698aef85
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="manage-package-flights"></a>Gérer les versions d’évaluation de package

Utilisez les méthodes suivantes dans l’API de soumission du WindowsStore pour gérer les versions d’évaluation de package de vos applications. Pour obtenir une présentation de l’API de soumission du WindowsStore, notamment les conditions préalables à l’utilisation de l’API, voir [Créer et gérer des soumissions à l’aide des services du WindowsStore](create-and-manage-submissions-using-windows-store-services.md).

>**Remarque**&nbsp;&nbsp;Ces méthodes ne peuvent être utilisées que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. L’octroi de cette autorisation s’effectue en plusieurs étapes. Elle est accordée aux comptes de développeur, et tous les comptes n’en bénéficient pas pour le moment. Pour demander un accès anticipé, connectez-vous au tableau de bord du Centre de développement, cliquez sur **Commentaires** au bas du tableau de bord, sélectionnez **API de soumission** dans la zone de commentaires, puis soumettez votre demande. Vous recevrez un message électronique dès que cette autorisation sera accordée à votre compte.

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
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}```</td>
<td align="left">[Obtient une version d’évaluation du package](get-a-flight.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights```</td>
<td align="left">[Crée une version d’évaluation du package](create-a-flight.md)</td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}```</td>
<td align="left">[Supprime une version d’évaluation du package](delete-a-flight.md)</td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>Prérequis

Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission du WindowsStore avant d’essayer d’utiliser l’une de ces méthodes.

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md)
