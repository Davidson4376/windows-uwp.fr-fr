---
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: Utilisez l’API de soumission de Microsoft Store pour créer et gérer les envois pour les applications qui sont inscrits dans votre compte espace partenaires par programmation.
title: Créer et gérer des soumissions
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store
ms.localizationpriority: medium
ms.openlocfilehash: e8e7653174eaf31f11341a5c8e21b69ec54a6189
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371283"
---
# <a name="create-and-manage-submissions"></a>Créer et gérer des soumissions

Utilisez le *API de soumission de Microsoft Store* pour interroger par programmation et de créer des envois pour les applications, les modules complémentaires et package vols pour le compte espace partenaires de votre organisation. Cette API est utile si votre compte gère beaucoup d’applications ou d’extensions et que vous voulez automatiser et optimiser le processus de soumission de ces ressources. Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels en provenance de votre application ou service.

Les étapes suivantes décrivent le processus complet d’utilisation de l’API de soumission au Microsoft Store :

1.  Vérifiez que vous avez rempli toutes les [conditions préalables](#prerequisites).
3.  Avant d’appeler une méthode dans l’API de soumission au Microsoft Store, [procurez-vous un jeton d’accès Azure AD](#obtain-an-azure-ad-access-token). Une fois le jeton obtenu, vous avez 60 minutes pour l’utiliser dans les appels à l’API de soumission au Microsoft Store avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en générer un nouveau.
4.  [Appelez l’API de soumission au Microsoft Store](#call-the-windows-store-submission-api).

<span id="not_supported" />

> [!IMPORTANT]
> Si vous utilisez cette API pour créer une soumission pour une application, le vol de package ou le module complémentaire, veillez à apporter des modifications à l’envoi uniquement à l’aide de l’API, plutôt que dans l’espace partenaires. Si vous utilisez des partenaires pour modifier une soumission que vous avez créé à l’origine à l’aide de l’API, vous serez n’est plus en mesure de modifier ou de valider cette soumission à l’aide de l’API. Dans certains cas, la soumission non validée peut rester définie sur l'état d'erreur. Si cela se produit, vous devez supprimer la soumission et en créer une nouvelle.

> [!IMPORTANT]
> Vous ne pouvez pas utiliser cette API pour publier des soumissions pour [les achats en volume par le biais du Microsoft Store pour Entreprises et du Microsoft Store pour Éducation](../publish/organizational-licensing.md) ou pour publier des soumissions pour les [apps cœur de métier](../publish/distribute-lob-apps-to-enterprises.md) directement aux entreprises. Pour ces deux scénarios, vous devez utiliser publier la soumission de partenaires.

> [!NOTE]
> Cette API ne peut pas être utilisée avec des apps ou des extensions qui utilisent des mises à jour d'app obligatoires et des extensions consommables gérées par le Microsoft Store. Si vous utilisez l’API de soumission au Microsoft Store avec une app ou une extension qui utilise l’une de ces fonctionnalités, l’API retourne le code d’erreur 409. Dans ce cas, vous devez utiliser des partenaires pour gérer les envois de l’application ou le module complémentaire.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-submission-api"></a>Étape 1 : Remplir les conditions préalables pour l’utilisation de l’API de soumission de Microsoft Store

Avant d’écrire le code d’appel de l’API de soumission au Microsoft Store, vérifiez que vous remplissez bien les conditions préalables suivantes.

* Vous (ou votre organisation) devez disposer d’un annuaire Azure AD et d’une autorisation [Administrateur global](https://go.microsoft.com/fwlink/?LinkId=746654) pour l’annuaire. Si vous utilisez déjà Office 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’un annuaire Azure AD. Sinon, vous pouvez [créer un nouvelle application Azure AD dans partenaires](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sans aucun frais supplémentaire.

* Vous devez [associer une application Azure AD à votre compte espace partenaires](#associate-an-azure-ad-application-with-your-windows-partner-center-account) et obtenir votre client ID, ID de client et la clé. Vous avez besoin de ces valeurs pour obtenir un jeton d’accès Azure AD, qui vous servira dans les appels à l’API de soumission au Microsoft Store.

* Préparez votre app en vue de l’utiliser avec l’API de soumission au Microsoft Store :

  * Si votre application n’existe pas encore de partenaires, vous devez [créer votre application en réservant son nom dans le centre partenaires](https://docs.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name). Vous ne pouvez pas utiliser l’API de soumission de Microsoft Store pour créer une application dans le centre de partenaires ; Vous devez travailler dans le centre de partenaires pour la créer, et puis après que vous pouvez utiliser l’API pour accéder à l’application et créer par programmation des envois pour celui-ci. En revanche, vous pouvez vous servir de l’API pour créer des extensions et des versions d’évaluation de package par programmation avant de créer des soumissions pour ces ressources.

  * Avant de pouvoir créer une soumission pour une application donnée à l’aide de cette API, vous devez d’abord [créer une soumission de l’application dans le centre partenaires](https://docs.microsoft.com/windows/uwp/publish/app-submissions), y compris la réponse à la [âge des évaluations](https://docs.microsoft.com/windows/uwp/publish/age-ratings) questionnaire. Après quoi, vous pourrez créer des soumissions par programmation pour cette application à l’aide de l’API. Vous n’avez pas besoin de créer une soumission d’extension ou de version d’évaluation de package avant d’utiliser l’API pour ces types de soumission.

  * Si vous créez ou mettez à jour une soumission d’application et que vous devez inclure un package d’application, [préparez le package d’application](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements).

  * Si vous créez ou mettez à jour une soumission d’application et que vous devez inclure des captures d’écran ou des images à des fins de présentation sur le Store, [préparez les captures d’écran et les images de l’application](https://docs.microsoft.com/windows/uwp/publish/app-screenshots-and-images).

  * Si vous créez ou mettez à jour une soumission d’extension et que vous devez inclure une icône, [préparez le package l’icône](https://docs.microsoft.com/windows/uwp/publish/create-iap-descriptions).

<span id="associate-an-azure-ad-application-with-your-windows-partner-center-account" />

### <a name="how-to-associate-an-azure-ad-application-with-your-partner-center-account"></a>Comment associer une application Azure AD à votre compte espace partenaires

Avant de pouvoir utiliser l’API de soumission de Microsoft Store, vous devez associer une application Azure AD à votre compte espace partenaires, récupérer l’ID client et ID client pour l’application et générer une clé. L’application Azure AD est l’app ou le service à partir duquel vous allez appeler l’API de soumission au Microsoft Store. Vous avez besoin de l’ID de locataire, de l’ID client et de la clé pour obtenir le jeton d’accès Azure AD à transmettre à l’API.

> [!NOTE]
> Cette tâche ne doit être effectuée qu’une seule fois. Une fois que vous avez l’ID de locataire, l’ID client et la clé à disposition, vous pouvez les réutiliser chaque fois que vous avez besoin de créer un nouveau jeton d’accès Azure AD.

1.  Dans le centre de partenaires, [associer un compte espace partenaires de votre organisation avec un annuaire Azure AD de votre organisation](../publish/associate-azure-ad-with-partner-center.md).

2.  Ensuite, à partir de la **utilisateurs** page dans le **paramètres du compte** section de partenaires, [ajouter l’application Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) qui représente l’application ou le service que vous utiliserez pour soumissions d’accès pour votre compte espace partenaires. Assurez-vous d'attribuer à cette application le rôle **Manager**. Si l’application n’existe pas encore dans votre annuaire Azure AD, vous pouvez [créer une nouvelle application Azure AD dans le centre partenaires](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).  

3.  Revenez à la page **Utilisateurs**, cliquez sur le nom de votre application Azure AD pour accéder aux paramètres de l’application, puis notez les valeurs des champs **ID de locataire** et **ID client**.

4. Cliquez sur **Ajouter une clé**. Dans l’écran suivant, notez la valeur du champ **Clé**. Vous ne pourrez plus accéder à ces informations une fois que vous aurez quitté cette page. Pour plus d’informations, voir [Gérer les clés pour une application Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Étape 2 : Obtenir un jeton d’accès Azure AD

Avant d’appeler l’une des méthodes dans l’API de soumission au Microsoft Store, vous devez d’abord obtenir un jeton d’accès Azure AD pour le passer à l’en-tête **Autorisation** de chaque méthode de l’API. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez l’actualiser pour pouvoir continuer à l’utiliser dans d’autres appels à l’API.

Pour obtenir le jeton d’accès, suivez les instructions présentées dans l’article [Appels de service à service à l’aide des informations d’identification du client](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) pour envoyer une requête HTTP POST au point de terminaison ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Voici un exemple de requête.

```json
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Pour le *locataire\_id* valeur dans l’URI de la publication et la *client\_id* et *client\_secret* paramètres, spécifiez le locataire ID, ID de client et la clé de votre application que vous avez récupérées à partir du centre de partenaires dans la section précédente. Pour le paramètre *resource*, vous devez spécifier ```https://manage.devcenter.microsoft.com```.

Une fois votre jeton d’accès arrivé à expiration, vous pouvez l’actualiser en suivant les instructions fournies [ici](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

Pour voir des exemples d’utilisation de code C#, Java ou Python pour obtenir un jeton d’accès, consultez les [exemples de code](#code-examples) de l’API de soumission au Microsoft Store.

<span id="call-the-windows-store-submission-api">

## <a name="step-3-use-the-microsoft-store-submission-api"></a>Étape 3 : Utiliser l’API de soumission de Microsoft Store

À partir du moment où vous disposez d’un jeton d’accès Azure AD, vous pouvez appeler des méthodes dans l’API de soumission au Microsoft Store. L’API propose diverses méthodes qui sont regroupées dans des scénarios pour applications, extensions et versions d’essai de package. Pour créer ou mettre à jour des soumissions, il convient généralement d’appeler plusieurs méthodes de l’API de soumission au Microsoft Store dans un ordre spécifique. Pour plus d’informations sur chaque scénario et sur la syntaxe de chacune de ces méthodes, voir les articles indiqués dans le tableau suivant.

> [!NOTE]
> Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour appeler des méthodes dans l’API de soumission au Microsoft Store. Passé ce délai, il expire.

| Scénario       | Description                                                                 |
|---------------|----------------------------------------------------------------------|
| Applications |  Récupérer des données pour toutes les applications qui sont inscrits à votre compte espace partenaires et créer des envois pour les applications. Pour plus d’informations sur ces méthodes, voir les articles suivants : <ul><li>[Obtenir des données d’application](get-app-data.md)</li><li>[Gérer les envois d’application](manage-app-submissions.md)</li></ul> |
| Extensions | Obtient, crée ou supprime des extensions pour vos applications, puis obtient, crée ou supprime des soumissions pour les extensions. Pour plus d’informations sur ces méthodes, voir les articles suivants : <ul><li>[Gérer les modules complémentaires](manage-add-ons.md)</li><li>[Gérer les envois de module complémentaire](manage-add-on-submissions.md)</li></ul> |
| Versions d’évaluation de package | Obtient, crée ou supprime des versions d’évaluation de package pour vos applications, puis obtient, crée ou supprime des soumissions pour les versions d’évaluation de package. Pour plus d’informations sur ces méthodes, voir les articles suivants : <ul><li>[Gérer les vols de package](manage-flights.md)</li><li>[Gérer les envois de vol de package](manage-flight-submissions.md)</li></ul> |

<span id="code-samples"/>

## <a name="code-examples"></a>Exemples de code

Les articles suivants fournissent des exemples de code détaillés qui montrent comment utiliser l’API de soumission au Microsoft Store dans différents langages de programmation :

* [C#exemple : envois pour les applications, les modules complémentaires et les vols](csharp-code-examples-for-the-windows-store-submission-api.md)
* [C#exemple : soumission de l’application avec les options de jeu et codes de fin](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Exemple Java : envois pour les applications, les modules complémentaires et les vols](java-code-examples-for-the-windows-store-submission-api.md)
* [Exemple Java : soumission de l’application avec les options de jeu et codes de fin](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Exemple Python : envois pour les applications, les modules complémentaires et les vols](python-code-examples-for-the-windows-store-submission-api.md)
* [Exemple Python : soumission de l’application avec les options de jeu et codes de fin](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>module StoreBroker PowerShell

Au lieu d'appeler l'API de soumission au Microsoft Store directement, nous fournissons également un module PowerShell Open Source qui implémente une interface de ligne de commande sur l’API. Ce module est appelé [StoreBroker](https://aka.ms/storebroker). Vous pouvez utiliser ce module pour gérer les soumissions de votre app, de votre version et de vos modules complémentaires à partir de la ligne de commande, en lieu et place de l'appel direct de l'API de soumission au Microsoft Store. Sinon, vous pouvez simplement parcourir la source pour consulter des exemples supplémentaires d'appel de cette API. Le module StoreBroker est activement utilisé au sein de Microsoft en tant que vecteur principal de soumission de nombreuses applications internes dans le Windows Store.

Pour plus d’informations, consultez notre [page StoreBroker sur GitHub](https://aka.ms/storebroker).

## <a name="troubleshooting"></a>Résolution des problèmes

| Problème      | Résolution                                          |
|---------------|---------------------------------------------|
| Après avoir appelé l’API de soumission au Microsoft Store à partir de PowerShell, les données de réponse destinées à l’API sont altérées si vous les convertissez du format JSON en objet PowerShell à l’aide de l’applet de commande [ConvertFrom Json](https://technet.microsoft.com/library/hh849898.aspx) et les rétablissez ensuite au format JSON à l’aide de l’applet de commande [ConvertTo Json](https://technet.microsoft.com/library/hh849922.aspx). |  Par défaut, le paramètre *-Depth* de l’applet de commande [ConvertTo Json](https://technet.microsoft.com/library/hh849922.aspx) est défini à 2 niveaux d’objets, ce qui est trop superficiel pour la plupart des objets JSON retournées par l’API de soumission au Microsoft Store. Quand vous appelez l’applet de commande [ConvertTo Json](https://technet.microsoft.com/library/hh849922.aspx), attribuez au paramètre *-Depth* une valeur supérieure, par exemple 20. |

## <a name="additional-help"></a>Aide supplémentaire

Si vous avez des questions sur l’API de soumission au Microsoft Store ou si vous avez besoin d’aide pour gérer vos soumissions avec cette API, utilisez les ressources suivantes :

* Posez vos questions sur nos [forums](https://social.msdn.microsoft.com/Forums/windowsapps/home?forum=wpsubmit).
* Visitez notre [page de prise en charge](https://developer.microsoft.com/windows/support) et demander une des options de support assisté pour les partenaires. Si vous êtes invité à choisir un type de problème et une catégorie, choisissez respectivement **Soumission d’application et certification** et **Soumission d’une application**.  

## <a name="related-topics"></a>Rubriques connexes

* [Obtenir des données d’application](get-app-data.md)
* [Gérer les envois d’application](manage-app-submissions.md)
* [Gérer les modules complémentaires](manage-add-ons.md)
* [Gérer les envois de module complémentaire](manage-add-on-submissions.md)
* [Gérer les vols de package](manage-flights.md)
* [Gérer les envois de vol de package](manage-flight-submissions.md)
