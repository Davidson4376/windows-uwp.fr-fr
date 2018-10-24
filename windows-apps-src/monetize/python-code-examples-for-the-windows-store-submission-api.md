---
author: Xansky
ms.assetid: 8AC56AAF-8D8C-4193-A6B3-BB5D0669D994
description: Servez-vous des exemples de code Python présentés dans cette section pour en savoir plus sur l’utilisation de l’API de soumission au MicrosoftStore.
title: "Exemple de code Python: soumissions d'applications, d'extensions et de versions d’évaluation"
ms.author: mhopkins
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, API de soumission au MicrosoftStore, exemples de code, python
ms.localizationpriority: medium
ms.openlocfilehash: 0dc6f9854ed3fabeb788b99297cd2b33cdc1b79b
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5470577"
---
# <a name="python-sample-submissions-for-apps-add-ons-and-flights"></a>Exemple de code Python: soumissions d'applications, d'extensions et de versions d’évaluation

Cet article fournit des exemples de code Python qui décrivent comment utiliser l’[API de soumission au MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md) pour les tâches suivantes:

* [Obtenir un jeton d’accès AzureAD](#token)
* [Créer une extension](#create-add-on)
* [Créer une version d’évaluation du package](#create-package-flight)
* [Créer une soumission d’applications](#create-app-submission)
* [Créer une soumission d’extension](#create-add-on-submission)
* [Créer une soumission de version d’évaluation du package](#create-flight-submission)

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obtenir un jeton d’accès AzureAD

L’exemple suivant indique comment [obtenir un jeton d’accès AzureAD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) que vous pouvez utiliser pour appeler des méthodes dans l’API de soumission au MicrosoftStore. Une fois le jeton obtenu, vous avez 60minutes pour l’utiliser dans les appels à l’API de soumission au MicrosoftStore avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en générer un autre.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L1-L20)]

<span id="create-add-on" />

## <a name="create-an-add-on"></a>Créer une extension

L’exemple suivant indique comment [créer](create-an-add-on.md) et [supprimer](delete-an-add-on.md) une extension.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L26-L52)]

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>Créer une version d’évaluation du package

L’exemple suivant indique comment [créer](create-a-flight.md) et [supprimer](delete-a-flight.md) une version d’évaluation du package.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L58-L87)]

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Créer une soumission d’applications

L’exemple suivant indique comment utiliser plusieurs méthodes dans l’API de soumission au MicrosoftStore afin de créer une soumission d’apps. Pour ce faire, le code crée une soumission en clonant la dernière soumission publiée, puis met à jour et valide la soumission clonée dans le Centre de développementWindows. Cet exemple effectue les tâches suivantes, entre autres:

1. Pour commencer, il [récupère les données de l’application indiquée](get-an-app.md).
2. Ensuite, il [supprime la soumission en attente de l’application](delete-an-app-submission.md), s’il en existe une.
3. Cela fait, il [crée une soumission pour l’application](create-an-app-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Il modifie certains détails de cette soumission, puis charge un nouveau package associé à cette dernière dans le stockage BlobAzure.
5. Ensuite, il [met à jour](update-an-app-submission.md) et [valide](commit-an-app-submission.md) la nouvelle soumission dans le Centre de développementWindows.
6. Pour finir, il [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-an-app-submission.md), jusqu’à ce que sa validation aboutisse.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L93-L166)]

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Créer une soumission d’extension

L’exemple suivant indique comment utiliser plusieurs méthodes dans l’API de soumission au MicrosoftStore afin de créer une soumission d’extension. Pour ce faire, le code crée une soumission en clonant la dernière soumission publiée, puis met à jour et valide la soumission clonée dans le Centre de développementWindows. Cet exemple effectue les tâches suivantes, entre autres:

1. Pour commencer, il [récupère les données de l’extension indiquée](get-an-add-on.md).
2. Ensuite, il [supprime la soumission en attente de l’extension](delete-an-add-on-submission.md), s’il en existe une.
3. Cela fait, il [crée une soumission pour l’extension](create-an-add-on-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Il charge une archiveZIP contenant des icônes associées à la soumission sur le stockage BlobAzure. Pour en savoir plus, consultez les instructions relatives au chargement d’une archiveZIP sur le stockage BlobAzure, dans la section [Créer une soumission d’extension](manage-add-on-submissions.md#create-an-add-on-submission).
5. Ensuite, il [met à jour](update-an-add-on-submission.md) et [valide](commit-an-add-on-submission.md) la nouvelle soumission dans le Centre de développementWindows.
6. Pour finir, il [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-an-add-on-submission.md) jusqu’à ce que sa validation aboutisse.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L172-L245)]

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Créer une soumission de version d’évaluation du package

L’exemple suivant indique comment utiliser plusieurs méthodes dans l’API de soumission au MicrosoftStore afin de créer une soumission de version d’évaluation du package. Pour ce faire, le code crée une soumission en clonant la dernière soumission publiée, puis met à jour et valide la soumission clonée dans le Centre de développementWindows. Cet exemple effectue les tâches suivantes, entre autres:

1. Pour commencer, il [récupère les données de la version d’évaluation du package indiquée](get-a-flight.md).
2. Ensuite, il [supprime la soumission en attente de la version d’évaluation du package](delete-a-flight-submission.md), s’il en existe une.
3. Cela fait, il [crée une soumission pour la version d’évaluation du package](create-a-flight-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Il charge un nouveau package associé à la soumission sur le stockage BlobAzure. Pour en savoir plus, consultez les instructions relatives au chargement d’une archiveZIP sur le stockage BlobAzure, dans la section [Créer une soumission de version d’évaluation du package](manage-flight-submissions.md#create-a-package-flight-submission).
5. Ensuite, il [met à jour](update-a-flight-submission.md) et [valide](commit-a-flight-submission.md) la nouvelle soumission dans le Centre de développementWindows.
6. Pour finir, il [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-a-flight-submission.md), jusqu’à ce que sa validation aboutisse.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L251-L325)]

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)
