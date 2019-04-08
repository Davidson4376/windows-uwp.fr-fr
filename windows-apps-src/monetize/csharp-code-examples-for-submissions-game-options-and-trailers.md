---
description: Servez-vous des exemples de code C# présentés dans cette section pour en savoir plus sur la soumission d'options de jeu et de bandes-annonces à l'aide de l’API de soumission au Microsoft Store.
title: 'Exemple de code C# : soumission d’applications avec options de jeu et bandes-annonces'
ms.date: 07/10/2017
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, exemples de code, options de jeu, bandes-annonces, listes avancées, C#
ms.localizationpriority: medium
ms.openlocfilehash: 277d455fe3387452a4afe91fd74e5c2099f76ce4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594074"
---
# <a name="c-sample-app-submission-with-game-options-and-trailers"></a>C\# exemple : soumission de l’application avec les options de jeu et codes de fin

Cet article fournit des exemples de code C# expliquant l’utilisation de l’[API de soumission au Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) pour les tâches suivantes :

* Obtenir un jeton d’accès Azure AD à utiliser avec l’API de soumission au Microsoft Store.
* Créer une soumission d’applications
* Configurer des données de description dans le Store pour la soumission d’applications, y compris des options de description avancées de [jeux](manage-app-submissions.md#gaming-options-object) et de [bandes-annonces](manage-app-submissions.md#trailer-object).
* Télécharger le fichier ZIP contenant les packages, les images de description et les fichiers de bande-annonce pour la soumission d’applications.
* Valider la soumission d’applications.

Vous pouvez passer en revue chaque exemple pour en savoir plus sur la tâche qu’elle illustre, ou vous pouvez générer tous les exemples de code de cet article dans une application console. Pour générer les exemples, créez une application console C# nommée **DevCenterApiSample** dans Visual Studio, copiez chaque exemple dans un fichier de code distinct dans le projet et générez le projet.

## <a name="prerequisites"></a>Conditions préalables

La configuration requise pour ces exemples est la suivante :

* Ajoutez une référence à l'assembly System.Web de votre projet.
* Installez le package NuGet [Newtonsoft.Json](https://www.newtonsoft.com/json) de Newtonsoft dans votre projet.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Créer une soumission d’applications

La catégorie ```CreateAndSubmitSubmissionExample``` définit une méthode ```Execute``` publique qui appelle d'autres méthodes d'exemple pour utiliser l'API de soumission au Microsoft Store afin de créer et de valider une soumission d'applications contenant des options de jeu et une bande-annonce. Adaptez ce code en fonction de vos besoins, comme suit :

* Affectez la variable ```tenantId``` à l’ID de locataire pour votre application et affectez les variables ```clientId``` et ```clientSecret``` à l'ID client et à la clé de votre application. Pour plus d’informations, consultez [comment associer une application Azure AD à votre compte espace partenaires](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)
* Affectez la variable ```applicationId``` à l'[ID Store](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous voulez créer une soumission.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/CreateAndSubmitSubmissionExample.cs#CreateAndSubmitSubmissionExample)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obtenir un jeton d’accès Azure AD

La classe ```DevCenterAccessTokenClient``` définit une méthode d’assistance qui utilise vos valeurs ```tenantId```, ```clientId``` et ```clientSecret``` pour créer un jeton d’accès Azure AD à utiliser avec l’API de soumission au Microsoft Store.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterAccessTokenClient.cs#DevCenterAccessTokenClient)]

<span id="utilities" />

## <a name="helper-methods-to-invoke-the-submission-api-and-upload-submission-files"></a>Méthodes d’assistance pour appeler l’API de soumission et charger les fichiers de soumission

La classe ```DevCenterClient``` définit des méthodes d’assistance qui appellent plusieurs méthodes dans l’API de soumission au Microsoft Store et chargent le fichier ZIP contenant les packages, les images de description et les fichiers de bande-annonce pour la soumission d’apps.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterClient.cs#DevCenterClient)]

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
