---
author: Xansky
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: Utilisez l’API de soumission au Microsoft Store pour créer et gérer des soumissions pour les applications qui sont enregistrées sur votre compte espace partenaires par programmation.
title: Créer et gérer des soumissions
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, API de soumission au MicrosoftStore
ms.localizationpriority: medium
ms.openlocfilehash: c91c7b42642df9a03aab1324f074799b63157e62
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7421187"
---
# <a name="create-and-manage-submissions"></a>Créer et gérer des soumissions


L' *API de soumission au Microsoft Store* permet d’interroger et de créer des soumissions d’applications, des modules complémentaires et des versions d’évaluation de package pour votre compte d’ou votre organisation l’espace partenaires par programmation. Cette API est utile si votre compte gère beaucoup d’applications ou d’extensions et que vous voulez automatiser et optimiser le processus de soumission de ces ressources. Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels en provenance de votre application ou service.

Les étapes suivantes décrivent le processus complet d’utilisation de l’API de soumission au MicrosoftStore:

1.  Vérifiez que vous avez rempli toutes les [conditions préalables](#prerequisites).
3.  Avant d’appeler une méthode dans l’API de soumission au MicrosoftStore, [procurez-vous un jeton d’accès Azure AD](#obtain-an-azure-ad-access-token). Une fois le jeton obtenu, vous avez 60minutes pour l’utiliser dans les appels à l’API de soumission au MicrosoftStore. Passé ce délai, le jeton n’est plus valable. Une fois le jeton arrivé à expiration, vous pouvez en générer un nouveau.
4.  [Appelez l’API de soumission au MicrosoftStore](#call-the-windows-store-submission-api).

<span id="not_supported" />

> [!IMPORTANT]
> Si vous utilisez cette API pour créer une soumission pour une application, version d’évaluation, ou un module complémentaire, veillez à apporter d’autres modifications à la soumission uniquement à l’aide de l’API, plutôt que dans l’espace partenaires. Si vous utilisez l’espace partenaires pour modifier une soumission que vous avez créé à l’origine à l’aide de l’API, vous ne serez n’est plus en mesure de modifier ou valider cette soumission à l’aide de l’API. Dans certains cas, la soumission non validée peut rester définie sur l'état d'erreur. Si cela se produit, vous devez supprimer la soumission et en créer une nouvelle.

> [!IMPORTANT]
> Vous ne pouvez pas utiliser cette API pour publier des soumissions pour [les achats en volume par le biais du MicrosoftStore pour Entreprises et du MicrosoftStore pour Éducation](../publish/organizational-licensing.md) ou pour publier des soumissions pour les [apps cœur de métier](../publish/distribute-lob-apps-to-enterprises.md) directement aux entreprises. Pour ces deux scénarios, vous devez utiliser publier la soumission dans l’espace partenaires.

> [!NOTE]
> Cette API ne peut pas être utilisée avec des apps ou des extensions qui utilisent des mises à jour d'app obligatoires et des extensions consommables gérées par le MicrosoftStore. Si vous utilisez l’API de soumission au MicrosoftStore avec une app ou une extension qui utilise l’une de ces fonctionnalités, l’API retourne le code d’erreur409. Dans ce cas, vous devez utiliser l’espace partenaires pour gérer les soumissions pour l’application ou l’extension.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-submission-api"></a>Étape 1: Remplir les conditions préalables pour utiliser l’API de soumission au MicrosoftStore

Avant d’écrire le code d’appel de l’API de soumission au MicrosoftStore, vérifiez que vous remplissez bien les conditions préalables suivantes.

* Vous (ou votre organisation) devez disposer d’un annuaire Azure AD et d’une autorisation [Administrateur global](http://go.microsoft.com/fwlink/?LinkId=746654) pour l’annuaire. Si vous utilisez déjà Office365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’un annuaire Azure AD. Dans le cas contraire, vous pouvez [créer un nouvel Azure AD dans l’espace partenaires](../publish/associate-azure-ad-with-dev-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) pour sans frais supplémentaires.

* Vous devez [associer une application Azure AD avec votre compte espace partenaires](#associate-an-azure-ad-application-with-your-windows-dev-center-account) et obtenez votre client, l’ID client et clé. Vous avez besoin de ces valeurs pour obtenir un jeton d’accès Azure AD, qui vous servira dans les appels à l’API de soumission au MicrosoftStore.

* Préparez votre app en vue de l’utiliser avec l’API de soumission au MicrosoftStore:

  * Si votre application n’existe pas encore dans l’espace partenaires, vous devez [créer votre application en réservant son nom dans l’espace partenaires](https://msdn.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name). Vous ne pouvez pas utiliser l’API de soumission au Microsoft Store pour créer une application dans l’espace partenaires; Vous devez travailler dans l’espace partenaires pour la créer et puis après que vous pouvez utiliser l’API pour accéder à l’application et créer par programmation des soumissions pour celle-ci. En revanche, vous pouvez vous servir de l’API pour créer des extensions et des versions d’évaluation de package par programmation avant de créer des soumissions pour ces ressources.

  * Avant de pouvoir créer une soumission pour une application donnée à l’aide de cette API, vous devez d’abord [créer une soumission pour l’application dans l’espace partenaires](https://msdn.microsoft.com/windows/uwp/publish/app-submissions), y compris répondre au questionnaire de [classification par âge](https://msdn.microsoft.com/windows/uwp/publish/age-ratings) . Après quoi, vous pourrez créer des soumissions par programmation pour cette application à l’aide de l’API. Vous n’avez pas besoin de créer une soumission d’extension ou de version d’évaluation de package avant d’utiliser l’API pour ces types de soumission.

  * Si vous créez ou mettez à jour une soumission d’application et que vous devez inclure un package d’application, [préparez le package d’application](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements).

  * Si vous créez ou mettez à jour une soumission d’application et que vous devez inclure des captures d’écran ou des images à des fins de présentation sur le Store, [préparez les captures d’écran et les images de l’application](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images).

  * Si vous créez ou mettez à jour une soumission d’extension et que vous devez inclure une icône, [préparez le package l’icône](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon).

<span id="associate-an-azure-ad-application-with-your-windows-dev-center-account" />

### <a name="how-to-associate-an-azure-ad-application-with-your-partner-center-account"></a>Comment associer une application Azure AD avec votre compte espace partenaires

Avant de pouvoir utiliser l’API de soumission au Microsoft Store, vous devez associer une application Azure AD avec votre compte espace partenaires, récupérer l’ID de locataire et ID de client pour l’application et générer une clé. L’application Azure AD est l’app ou le service à partir duquel vous allez appeler l’API de soumission au MicrosoftStore. Vous avez besoin de l’ID de locataire, de l’ID client et de la clé pour obtenir le jeton d’accès Azure AD à transmettre à l’API.

> [!NOTE]
> Cette tâche ne doit être effectuée qu’une seule fois. Une fois que vous avez l’ID de locataire, l’ID client et la clé à disposition, vous pouvez les réutiliser chaque fois que vous avez besoin de créer un nouveau jeton d’accès Azure AD.

1.  Dans l’espace partenaires, [Associez le compte de l’espace partenaires de votre organisation avec un annuaire Azure AD de votre organisation](../publish/associate-azure-ad-with-dev-center.md).

2.  Ensuite, à partir de la page **utilisateurs** dans la section **paramètres du compte** de l’espace partenaires, [Ajoutez l’application Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) qui représente l’application ou le service que vous allez utiliser pour accéder aux soumissions pour votre compte espace partenaires. Assurez-vous d'attribuer à cette application le rôle **Manager**. Si l’application n’existe pas encore dans votre répertoire Azure Active directory, vous pouvez [créer une nouvelle application Azure AD dans l’espace partenaires](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).  

3.  Revenez à la page **Utilisateurs**, cliquez sur le nom de votre application Azure AD pour accéder aux paramètres de l’application, puis notez les valeurs des champs **ID de locataire** et **ID client**.

4. Cliquez sur **Ajouter une clé**. Dans l’écran suivant, notez la valeur du champ **Clé**. Vous ne pourrez plus accéder à ces informations une fois que vous aurez quitté cette page. Pour plus d’informations, voir [Gérer les clés pour une application Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Étape2: Obtenir un jeton d’accès Azure AD

Avant d’appeler l’une des méthodes dans l’API de soumission au MicrosoftStore, vous devez d’abord obtenir un jeton d’accès Azure AD pour le passer à l’en-tête **Autorisation** de chaque méthode de l’API. Après avoir obtenu un jeton d’accès, vous disposez de 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez l’actualiser pour pouvoir continuer à l’utiliser dans d’autres appels à l’API.

Pour obtenir le jeton d’accès, suivez les instructions présentées dans l’article [Appels de service à service à l’aide des informations d’identification du client](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) pour envoyer une requête HTTP POST au point de terminaison ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Voici un exemple de requête.

```
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Pour la valeur *tenant\_id* dans l’URI POST et les paramètres *client\_id* et *client\_secret* , spécifiez l’ID de locataire, ID client et la clé pour votre application que vous avez récupérées à partir de l’espace partenaires dans la section précédente. Pour le paramètre *resource*, vous devez spécifier ```https://manage.devcenter.microsoft.com```.

Une fois votre jeton d’accès arrivé à expiration, vous pouvez l’actualiser en suivant les instructions fournies [ici](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

Pour voir des exemples d’utilisation de code C#, Java ou Python pour obtenir un jeton d’accès, consultez les [exemples de code](#code-examples) de l’API de soumission au MicrosoftStore.

<span id="call-the-windows-store-submission-api">

## <a name="step-3-use-the-microsoft-store-submission-api"></a>Étape3: Utiliser l’API de soumission au MicrosoftStore

À partir du moment où vous disposez d’un jeton d’accès Azure AD, vous pouvez appeler des méthodes dans l’API de soumission au MicrosoftStore. L’API propose diverses méthodes qui sont regroupées dans des scénarios pour apps, extensions et versions d’essai de package. Pour créer ou mettre à jour des soumissions, il convient généralement d’appeler plusieurs méthodes de l’API de soumission au MicrosoftStore dans un ordre spécifique. Pour plus d’informations sur chaque scénario et sur la syntaxe de chacune de ces méthodes, voir les articles indiqués dans le tableau suivant.

> [!NOTE]
> Après avoir obtenu un jeton d’accès, vous avez 60minutes pour appeler des méthodes dans l’API de soumission au MicrosoftStore. Passé ce délai, il expire.

| Scénario       | Description                                                                 |
|---------------|----------------------------------------------------------------------|
| Applications |  Récupérer les données de toutes les applications qui sont enregistrées sur votre compte espace partenaires et créer des soumissions pour les applications. Pour plus d’informations sur ces méthodes, voir les articles suivants: <ul><li>[Obtenir des données d’application](get-app-data.md)</li><li>[Gérer les soumissions d’applications](manage-app-submissions.md)</li></ul> |
| Extensions | Obtient, crée ou supprime des extensions pour vos applications, puis obtient, crée ou supprime des soumissions pour les extensions. Pour plus d’informations sur ces méthodes, voir les articles suivants: <ul><li>[Gérer les extensions](manage-add-ons.md)</li><li>[Gérer les soumissions d’extensions](manage-add-on-submissions.md)</li></ul> |
| Versions d’évaluation de package | Obtient, crée ou supprime des versions d’évaluation de package pour vos applications, puis obtient, crée ou supprime des soumissions pour les versions d’évaluation de package. Pour plus d’informations sur ces méthodes, voir les articles suivants: <ul><li>[Gérer les versions d’évaluation de package](manage-flights.md)</li><li>[Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md)</li></ul> |

<span id="code-samples"/>

## <a name="code-examples"></a>Exemples de code

Les articles suivants fournissent des exemples de code détaillés qui montrent comment utiliser l’API de soumission au MicrosoftStore dans différents langages de programmation:

* [Exemple de code C#: soumissions d'applications, d'extensions et de versions d’évaluation](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Exemple de code C#: soumission d’application avec options de jeu et bandes-annonces](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Exemple de code Java: soumissions d'applications, d'extensions et de versions d’évaluation](java-code-examples-for-the-windows-store-submission-api.md)
* [Exemple de code Java: soumission d’applications avec options de jeu et bandes-annonces](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Exemple de code Python: soumissions d'applications, d'extensions et de versions d’évaluation](python-code-examples-for-the-windows-store-submission-api.md)
* [Exemple Python: soumission d’apps avec des options de jeu et bandes-annonces](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>Module StoreBroker PowerShell

Au lieu d'appeler l'API de soumission au MicrosoftStore directement, nous fournissons également un module PowerShell OpenSource qui implémente une interface de ligne de commande sur l’API. Ce module est appelé [StoreBroker](https://aka.ms/storebroker). Vous pouvez utiliser ce module pour gérer les soumissions de votre app, de votre version et de vos modules complémentaires à partir de la ligne de commande, en lieu et place de l'appel direct de l'API de soumission au MicrosoftStore. Sinon, vous pouvez simplement parcourir la source pour consulter des exemples supplémentaires d'appel de cette API. Le module StoreBroker est activement utilisé au sein de Microsoft en tant que vecteur principal de soumission de nombreuses applications internes dans le WindowsStore.

Pour plus d'informations, consultez notre [page StoreBroker sur GitHub](https://aka.ms/storebroker).

## <a name="troubleshooting"></a>Résolution des problèmes

| Problème      | Résolution                                          |
|---------------|---------------------------------------------|
| Après avoir appelé l’API de soumission au MicrosoftStore à partir de PowerShell, les données de réponse destinées à l’API sont altérées si vous les convertissez du format JSON en objet PowerShell à l’aide de l’applet de commande [ConvertFrom Json](https://technet.microsoft.com/library/hh849898.aspx) et les rétablissez ensuite au format JSON à l’aide de l’applet de commande [ConvertTo Json](https://technet.microsoft.com/library/hh849922.aspx). |  Par défaut, le paramètre *-Depth* de l’applet de commande [ConvertTo Json](https://technet.microsoft.com/library/hh849922.aspx) est défini à 2niveaux d’objets, ce qui est trop superficiel pour la plupart des objets JSON retournées par l’API de soumission au MicrosoftStore. Quand vous appelez l’applet de commande [ConvertTo-Json](https://technet.microsoft.com/library/hh849922.aspx), attribuez au paramètre *-Depth* une valeur plus importante, par exemple 20. |

## <a name="additional-help"></a>Aide supplémentaire

Si vous avez des questions sur l’API de soumission au MicrosoftStore ou si vous avez besoin d’aide pour gérer vos soumissions avec cette API, utilisez les ressources suivantes:

* Posez vos questions sur nos [forums](https://social.msdn.microsoft.com/Forums/windowsapps/home?forum=wpsubmit).
* Visitez notre [page de support](https://developer.microsoft.com/windows/support) et demander une des options de support assisté pour l’espace partenaires. Si vous êtes invité à choisir un type de problème et une catégorie, choisissez respectivement **Soumission d’application et certification** et **Soumission d’une application**.  

## <a name="related-topics"></a>Rubriques connexes

* [Obtenir des données d’application](get-app-data.md)
* [Gérer les soumissions d’applications](manage-app-submissions.md)
* [Gérer les extensions](manage-add-ons.md)
* [Gérer les soumissions d’extensions](manage-add-on-submissions.md)
* [Gérer les versions d’évaluation de package](manage-flights.md)
* [Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md)
