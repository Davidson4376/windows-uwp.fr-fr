---
ms.assetid: 8AC56AAF-8D8C-4193-A6B3-BB5D0669D994
description: Servez-vous des exemples de code Python présentés dans cette section pour en savoir plus sur l’utilisation de l’API de soumission au Microsoft Store.
title: "Exemple de code Python : soumissions d'applications, d'extensions et de versions d’évaluation"
ms.date: 07/10/2017
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, exemples de code, python
ms.localizationpriority: medium
ms.openlocfilehash: bc3959b4e26bd54542edc3f69666f6d97cddba26
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334827"
---
# <a name="python-sample-submissions-for-apps-add-ons-and-flights"></a>Exemple de code Python : soumissions d'applications, d'extensions et de versions d’évaluation

Cet article fournit des exemples de code Python qui décrivent comment utiliser l’[API de soumission au Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) pour les tâches suivantes :

* [Obtenir un jeton d’accès Azure AD](#token)
* [Créer un module complémentaire](#create-add-on)
* [Créer un package de vol](#create-package-flight)
* [Créer une soumission de l’application](#create-app-submission)
* [Créer une soumission de module complémentaire](#create-add-on-submission)
* [Créer une soumission de vol de package](#create-flight-submission)

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obtenir un jeton d’accès Azure AD

L’exemple suivant indique comment [obtenir un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) que vous pouvez utiliser pour appeler des méthodes dans l’API de soumission au Microsoft Store. Une fois le jeton obtenu, vous avez 60 minutes pour l’utiliser dans les appels à l’API de soumission au Microsoft Store avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en générer un autre.

[!code-python[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L1-L20)]

<span id="create-add-on" />

## <a name="create-an-add-on"></a>Créer une extension

L’exemple suivant indique comment [créer](create-an-add-on.md) et [supprimer](delete-an-add-on.md) une extension.

[!code-python[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L26-L52)]

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>Crée une version d’évaluation du package

L’exemple suivant indique comment [créer](create-a-flight.md) et [supprimer](delete-a-flight.md) une version d’évaluation du package.

[!code-python[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L58-L87)]

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Créer une soumission d’applications

L’exemple suivant indique comment utiliser plusieurs méthodes dans l’API de soumission au Microsoft Store afin de créer une soumission d’apps. Pour ce faire, le code crée une nouvelle soumission comme un clone de la dernière soumission publiée, puis met à jour et la présentation clonée à Partner Center est validée. Cet exemple effectue les tâches suivantes, entre autres :

1. Pour commencer, il [récupère les données de l’application indiquée](get-an-app.md).
2. Ensuite, elle [supprime la soumission en attente de l’application](delete-an-app-submission.md), s’il en existe une.
3. Cela fait, il [crée une soumission pour l’application](create-an-app-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Il modifie certains détails de cette soumission, puis charge un nouveau package associé à cette dernière dans le stockage Blob Azure.
5. Ensuite, il [mises à jour](update-an-app-submission.md) , puis [valide](commit-an-app-submission.md) la nouvelle soumission de partenaires.
6. Pour finir, elle [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-an-app-submission.md) jusqu’à ce que celle-ci soit validée.

[!code-python[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L93-L166)]

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Créer une soumission d’extension

L’exemple suivant indique comment utiliser plusieurs méthodes dans l’API de soumission au Microsoft Store afin de créer une soumission d’extension. Pour ce faire, le code crée une nouvelle soumission comme un clone de la dernière soumission publiée, puis met à jour et la présentation clonée à Partner Center est validée. Cet exemple effectue les tâches suivantes, entre autres :

1. Pour commencer, il [récupère les données de l’extension indiquée](get-an-add-on.md).
2. Ensuite, il [supprime la soumission en attente de l’extension](delete-an-add-on-submission.md), s’il en existe une.
3. Après cela, elle crée [une soumission pour l’extension](create-an-add-on-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Elle charge une archive ZIP contenant des icônes associées à la soumission dans le stockage d’objets blob Azure. Pour en savoir plus, consultez les instructions relatives au chargement d’une archive ZIP sur le stockage Blob Azure, dans la section [Créer une soumission d’extension](manage-add-on-submissions.md#create-an-add-on-submission).
5. Ensuite, il [mises à jour](update-an-add-on-submission.md) , puis [valide](commit-an-add-on-submission.md) la nouvelle soumission de partenaires.
6. Pour finir, elle [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-an-add-on-submission.md) jusqu’à ce que celle-ci soit validée.

[!code-python[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L172-L245)]

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Créer une soumission de version d’évaluation du package

L’exemple suivant indique comment utiliser plusieurs méthodes dans l’API de soumission au Microsoft Store afin de créer une soumission de version d’évaluation du package. Pour ce faire, le code crée une nouvelle soumission comme un clone de la dernière soumission publiée, puis met à jour et la présentation clonée à Partner Center est validée. Cet exemple effectue les tâches suivantes, entre autres :

1. Pour commencer, il [récupère les données de la version d’évaluation du package indiquée](get-a-flight.md).
2. Ensuite, il [supprime la soumission en attente de la version d’évaluation du package](delete-a-flight-submission.md), s’il en existe une.
3. Cela fait, il [crée une soumission pour la version d’évaluation du package](create-a-flight-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Elle charge un nouveau package associé à la soumission dans le stockage d’objets blob Azure. Pour en savoir plus, consultez les instructions relatives au chargement d’une archive ZIP sur le stockage Blob Azure, dans la section [Créer une soumission de version d’évaluation du package](manage-flight-submissions.md#create-a-package-flight-submission).
5. Ensuite, il [mises à jour](update-a-flight-submission.md) , puis [valide](commit-a-flight-submission.md) la nouvelle soumission de partenaires.
6. Pour finir, elle [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-a-flight-submission.md) jusqu’à ce que celle-ci soit validée.

[!code-python[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L251-L325)]

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
