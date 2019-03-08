---
title: VerifyStringResult (JSON)
assetID: 272c688e-179e-c7e9-086b-e76d0d4bcb57
permalink: en-us/docs/xboxlive/rest/json-verifystringresult.html
description: " VerifyStringResult (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b01793222be80efccdca1f24f5226a2e9ff78064
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599484"
---
# <a name="verifystringresult-json"></a>VerifyStringResult (JSON)
Résultat des codes correspondant à chaque chaîne envoyée à [/system/strings/validate](../uri/stringserver/uri-systemstringsvalidate.md).
<a id="ID4ER"></a>


## <a name="verifystringresult"></a>VerifyStringResult

L’objet VerifyStringResult a la spécification suivante.

| Membre| Type| Description|
| --- | --- | --- |
| resultCode| entier non signé 32 bits| Obligatoire. HResult correspondant code soumis chaîne.|
| offendingString| chaîne| Obligatoire. Valeur de chaîne qui a provoqué une chaîne à être rejetés.|

<a id="ID4EXB"></a>


## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON


```json
{
    "verifyStringResult":
    [
        {"resultCode": "1", "offendingString": "badword"},
        {"resultCode": "0", "offendingString": ""},
        {"resultCode": "0", "offendingString": ""},
        {"resultCode": "0", "offendingString": ""},
    ]
}

```


#### <a name="common-hresult-values"></a>Valeurs HResult courantes

| Valeur| Nom de l’erreur|
| --- | --- | --- | --- | --- |
| 0| Réussite|
| 1| Chaîne offensant|
| 2| Chaîne trop longue|
| 3| Erreur inconnue|

<a id="ID4ELD"></a>


## <a name="see-also"></a>Voir également

<a id="ID4END"></a>


##### <a name="parent"></a>Parent

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)


<a id="ID4EXD"></a>


##### <a name="reference"></a>Référence

[POST (/system/strings/validate)](../uri/stringserver/uri-systemstringsvalidatepost.md)
