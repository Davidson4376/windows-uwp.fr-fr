---
author: Xansky
ms.assetid: 4920D262-B810-409E-BA3A-F68AADF1B1BC
description: Servez-vous des exemples de code Java présentés dans cette section pour en apprendre un peu plus sur l’utilisation de l’API de soumission au MicrosoftStore.
title: "Exemple de code Java: soumissions d'applications, d'extensions et de versions d’évaluation"
ms.author: mhopkins
ms.date: 07/10/2017
ms.topic: article
keywords: windows10, uwp, API de soumission au MicrosoftStore, exemples de code, java
ms.localizationpriority: medium
ms.openlocfilehash: 4a0df9fe873ab7d7330e06a18bb1816df3157d7a
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7100049"
---
# <a name="java-sample-submissions-for-apps-add-ons-and-flights"></a>Exemple de code Java: soumissions d'applications, d'extensions et de versions d’évaluation

Cet article fournit des exemples de code Java qui décrivent comment utiliser l’[API de soumission au MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md) pour les tâches suivantes:

* [Obtenir un jeton d’accès AzureAD](#token)
* [Créer une extension](#create-add-on)
* [Créer une version d’évaluation du package](#create-package-flight)
* [Créer une soumission d’applications](#create-app-submission)
* [Créer une soumission d’extension](#create-add-on-submission)
* [Créer une soumission de version d’évaluation du package](#create-flight-submission)

Vous pouvez passer en revue chaque exemple pour en savoir plus sur la tâche qu’il illustre, ou vous pouvez générer tous les exemples de code de cet article dans une application console. Pour obtenir le listing du code complet, voir la section du [listing du code](java-code-examples-for-the-windows-store-submission-api.md#code-listing) à la fin de cet article.

## <a name="prerequisites"></a>Conditions préalables

Ces exemples utilisent les bibliothèques suivantes:

* [Apache Commons Logging1.2](http://commons.apache.org/proper/commons-logging) (commons-logging-1.2.jar).
* [Apache HttpComponents Core4.4.5 et Apache HttpComponents Client4.5.2](https://hc.apache.org/) (httpcore-4.4.5.jar et httpclient-4.5.2.jar).
* [JSR353 JSON Processing API1.0](https://mvnrepository.com/artifact/javax.json/javax.json-api/1.0) et [JSR353 JSON Processing Default Provider API1.0.4](https://mvnrepository.com/artifact/org.glassfish/javax.json/1.0.4) (javax.json-api-1.0.jar et javax.json-1.0.4.jar).

## <a name="main-program-and-imports"></a>Programme principal et importations

L’exemple suivant montre les instructions d’importation utilisées par tous les exemples de code et implémente un programme de ligne de commande qui appelle les autres exemples de méthode.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/MainExample.java#L1-L64)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obtenir un jeton d’accès Azure AD

L’exemple suivant indique comment [obtenir un jeton d’accès AzureAD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) que vous pouvez utiliser pour appeler des méthodes dans l’API de soumission au MicrosoftStore. Une fois le jeton obtenu, vous avez 60minutes pour l’utiliser dans les appels à l’API de soumission au MicrosoftStore avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en générer un nouveau.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L65-L95)]

<span id="create-add-on" />

## <a name="create-an-add-on"></a>Créer une extension

L’exemple suivant indique comment [créer](create-an-add-on.md) et [supprimer](delete-an-add-on.md) une extension.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L310-L345)]

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>Créer une version d’évaluation du package

L’exemple suivant indique comment [créer](create-a-flight.md) et [supprimer](delete-a-flight.md) une version d’évaluation du package.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L185-L221)]

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Créer une soumission d’applications

L’exemple suivant indique comment utiliser plusieurs méthodes dans l’API de soumission au MicrosoftStore afin de créer une soumission d’apps. Pour ce faire, le ```SubmitNewApplicationSubmission``` méthode crée une soumission en clonant la dernière soumission publiée, et puis met à jour et valide la soumission clonée à l’espace partenaires. Plus précisément, la méthode ```SubmitNewApplicationSubmission``` effectue les tâches suivantes:

1. Pour commencer, la méthode [récupère les données de l’application indiquée](get-an-app.md).
2. Ensuite, elle [supprime la soumission en attente de l’application](delete-an-app-submission.md), s’il en existe une.
3. Cela fait, il [crée une soumission pour l’application](create-an-app-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Il modifie certains détails de cette soumission, puis charge un nouveau package associé à cette dernière dans le stockage BlobAzure.
5. Ensuite, il [mises à jour](update-an-app-submission.md) et [valide](commit-an-app-submission.md) la nouvelle soumission dans l’espace partenaires.
6. Pour finir, il [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-an-app-submission.md), jusqu’à ce que sa validation aboutisse.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L97-L183)]

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Créer une soumission d’extension

L’exemple suivant indique comment utiliser plusieurs méthodes dans l’API de soumission au MicrosoftStore afin de créer une soumission d’extension. Pour ce faire, le ```SubmitNewInAppProductSubmission``` méthode crée une soumission en clonant la dernière soumission publiée, puis met à jour et valide la soumission clonée à l’espace partenaires. Plus précisément, la méthode ```SubmitNewInAppProductSubmission``` effectue les tâches suivantes:

1. Pour commencer, la méthode [récupère les données de l’extension indiquée](get-an-add-on.md).
2. Ensuite, elle [supprime la soumission en attente de l’extension](delete-an-add-on-submission.md), s’il en existe une.
3. Cela fait, il [crée une soumission pour l’extension](create-an-add-on-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Il charge une archiveZIP contenant des icônes associées à la soumission sur le stockage BlobAzure.
5. Ensuite, il [mises à jour](update-an-add-on-submission.md) et [valide](commit-an-add-on-submission.md) la nouvelle soumission dans l’espace partenaires.
6. Pour finir, il [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-an-add-on-submission.md) jusqu’à ce que sa validation aboutisse.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L347-L431)]

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Créer une soumission de version d’évaluation du package

L’exemple suivant indique comment utiliser plusieurs méthodes dans l’API de soumission au MicrosoftStore afin de créer une soumission de version d’évaluation du package. Pour ce faire, le ```SubmitNewFlightSubmission``` méthode crée une soumission en clonant la dernière soumission publiée, puis met à jour et valide la soumission clonée à l’espace partenaires. Plus précisément, la méthode ```SubmitNewFlightSubmission``` effectue les tâches suivantes:

1. Pour commencer, la méthode [récupère les données de la version d’évaluation du package indiquée](get-a-flight.md).
2. Ensuite, elle [supprime la soumission en attente de la version d’évaluation du package](delete-a-flight-submission.md), s’il en existe une.
3. Cela fait, il [crée une soumission pour la version d’évaluation du package](create-a-flight-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Il charge un nouveau package associé à la soumission sur le stockage BlobAzure.
5. Ensuite, il [mises à jour](update-a-flight-submission.md) et [valide](commit-a-flight-submission.md) la nouvelle soumission dans PartnerCenter.
6. Pour finir, elle [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-a-flight-submission.md) jusqu’à ce que celle-ci soit validée.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L223-L308)]

<span id="utilities" />

## <a name="utility-methods-to-upload-submission-files-and-handle-request-responses"></a>Méthodes d’utilitaire pour charger des fichiers de soumission et gérer les réponses aux demandes

Les méthodes d’utilitaire suivantes illustrent les tâches suivantes:

* Chargement d’une archive ZIP contenant de nouvelles ressources pour une soumission d’applications ou d’extension dans le stockage d’objets blob Azure. Pour plus d’informations sur le chargement d’une archive ZIP dans le stockage d’objets blob Azure pour les soumissions d’applications et d’extension, consultez les instructions correspondantes dans [Créer une soumission d’applications](manage-app-submissions.md#create-an-app-submission), [Créer une soumission d’extension](manage-add-on-submissions.md#create-an-add-on-submission) et [Créer une soumission de version d’évaluation du package](manage-flight-submissions.md#create-a-package-flight-submission).
* Gestion des réponses aux demandes.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L433-L490)]

<span id="code-listing" />

## <a name="complete-code-listing"></a>Listing du code complet

Le listing du code suivant contient tous les exemples précédents organisés dans un seul fichier source.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L1-L491)]

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)
