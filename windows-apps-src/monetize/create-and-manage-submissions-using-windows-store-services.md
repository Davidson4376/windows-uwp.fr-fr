---
author: mcleanbyron
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: "L’API de soumission du Windows Store permet de créer et de gérer par programmation les soumissions d’application inscrites dans le compte du Centre de développement Windows."
title: "Créer et gérer des soumissions"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, API de soumission du Windows Store
ms.openlocfilehash: ca8bb623d06da0001b1b0751a5ac1ccc310bbd84
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="create-and-manage-submissions"></a>Créer et gérer des soumissions


L’*API de soumission du Windows Store* vous permet d’interroger et de créer par programmation des soumissions pour des applications, des extensions (également connue sous le nom PIA, produit in-app) et des versions d’évaluation de package pour votre compte Centre de développement Windows ou celui de votre organisation. Cette API est utile si votre compte gère beaucoup d’applications ou d’extensions et que vous voulez automatiser et optimiser le processus de soumission de ces ressources. Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels en provenance de votre application ou service.

Les étapes suivantes décrivent le processus complet d’utilisation de l’API de soumission du Windows Store:

1.  Vérifiez que vous avez rempli toutes les [conditions préalables](#prerequisites).
3.  Avant d’appeler une méthode dans l’API de soumission du Windows Store, [procurez-vous un jeton d’accès Azure AD](#obtain-an-azure-ad-access-token). Une fois le jeton obtenu, vous avez 60minutes pour l’utiliser dans les appels à l’API de soumission du Windows Store avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en générer un nouveau.
4.  [Appelez l’API de soumission du Windows Store](#call-the-windows-store-submission-api).


<span id="not_supported" />
>**Remarques importantes**

> * Cette API ne peut être utilisée que pour les comptes du Centre de développement Windows qui en ont l’autorisation. L’octroi de cette autorisation se fait en plusieurs étapes. Elle est accordée aux comptes de développeur, et tous les comptes n’en bénéficient pas pour le moment. Pour demander un accès anticipé, connectez-vous au tableau de bord du Centre de développement, cliquez sur **Commentaires** au bas du tableau de bord, sélectionnez **API de soumission** dans la zone de commentaires, puis soumettez votre demande. Vous recevrez un message électronique dès que cette autorisation sera accordée à votre compte.
<br/><br/>
>* Si vous utilisez cette API pour créer une soumission pour une application, une version d'évaluation d'un package ou un module complémentaire, assurez-vous d'utiliser exclusivement l'API pour apporter d'autres modifications à la soumission, sans passer par le tableau de bord du Centre de développement. Si vous passez par le tableau de bord pour modifier une soumission initialement créée via l'API, vous ne pourrez plus modifier ou valider cette soumission à l'aide de l'API. Dans certains cas, la soumission non validée peut rester définie sur l'état d'erreur. Si cela se produit, vous devez supprimer la soumission et en créer une nouvelle.
<br/><br/>
> * Cette API ne peut pas être utilisée avec les applications ou les extensions qui utilisent certaines des fonctionnalités introduites au tableau de bord du Centre de développement en août 2016, comme notamment les mises à jour d’applications obligatoires et les extensions consommables gérées par le Store. Si vous utilisez l’API de soumission du Windows Store avec une application ou une extension qui utilise l’une de ces fonctionnalités, l’API retourne le code d’erreur409. Dans ce cas, vous devez utiliser le tableau de bord pour gérer les soumissions pour l’application ou l’extension.


<span id="prerequisites" />
## <a name="step-1-complete-prerequisites-for-using-the-windows-store-submission-api"></a>Étape 1: Remplir les conditions préalables pour utiliser l’API de soumission du Windows Store

Avant d’écrire le code d’appel de l’API de soumission du Windows Store, vérifiez que vous remplissez bien les conditions préalables suivantes.

* Vous (ou votre organisation) devez disposer d’un annuaire Azure AD et d’une autorisation [Administrateur global](http://go.microsoft.com/fwlink/?LinkId=746654) pour l’annuaire. Si vous utilisez déjà Office365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’un annuaire Azure AD. Dans le cas contraire, vous pouvez [créer un annuaire Azure AD dans le Centre de développement](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users) sans frais supplémentaires.

* Vous devez [associer une application Azure AD à votre compte du Centre de développement Windows](#associate-an-azure-ad-application-with-your-windows-dev-center-account) et obtenir vos ID de locataire, ID client et clé. Vous avez besoin de ces valeurs pour obtenir un jeton d’accès Azure AD, qui vous servira dans les appels à l’API de soumission du Windows Store.

* Préparez votre application en vue de l’utiliser avec l’API de soumission du Windows Store:

  * Si elle n’existe pas encore dans le Centre de développement, [créez votre application dans le tableau de bord du Centre de développement](https://msdn.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name). Vous ne pouvez pas utiliser l’API de soumission du Windows Store pour créer une application dans le Centre de développement; vous devez d’abord la créer dans tableau de bord et ensuite utiliser l’API pour accéder à l’application et créer des soumissions pour celle-ci par programmation. En revanche, vous pouvez vous servir de l’API pour créer des extensions et des versions d’évaluation de package par programmation avant de créer des soumissions pour ces ressources.

  * Avant de pouvoir créer une soumission pour une application donnée à l’aide de cette API, vous devez d’abord [créer une soumission pour l’application dans le tableau de bord du Centre de développement](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) en répondant notamment aux questions de [classification par âge](https://msdn.microsoft.com/windows/uwp/publish/age-ratings). Après quoi, vous pourrez créer des soumissions par programmation pour cette application à l’aide de l’API. Vous n’avez pas besoin de créer une soumission d’extension ou de version d’évaluation de package avant d’utiliser l’API pour ces types de soumission.

  * Si vous créez ou mettez à jour une soumission d’application et que vous devez inclure un package d’application, [préparez le package d’application](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements).

  * Si vous créez ou mettez à jour une soumission d’application et que vous devez inclure des captures d’écran ou des images à des fins de présentation sur le Store, [préparez les captures d’écran et les images de l’application](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images).

  * Si vous créez ou mettez à jour une soumission d’extension et que vous devez inclure une icône, [préparez le package l’icône](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon).

<span id="associate-an-azure-ad-application-with-your-windows-dev-center-account" />
### <a name="how-to-associate-an-azure-ad-application-with-your-windows-dev-center-account"></a>Comment associer une application Azure AD à votre compte du Centre de développement Windows

Avant de pouvoir utiliser l’API de soumission du Windows Store, vous devez associer une application Azure AD à votre compte du Centre de développement, récupérer l’ID de locataire et l’ID client pour l’application et générer une clé. L’application Azure AD est l’application ou le service à partir duquel vous allez appeler l’API de soumission du Windows Store. Vous avez besoin de l’ID de locataire, de l’ID client et de la clé pour obtenir le jeton d’accès Azure AD à transmettre à l’API.

>**Remarque**&nbsp;&nbsp;Vous n’avez besoin d’effectuer cette tâche qu’une seule fois. Une fois que vous avez l’ID de locataire, l’ID client et la clé à disposition, vous pouvez les réutiliser chaque fois que vous avez besoin de créer un nouveau jeton d’accès Azure AD.

1.  Dans le Centre de développement, accédez à vos **Paramètres du compte**, cliquez sur **Gérer les utilisateurs**, puis associez le compte du Centre de développement de votre organisation à son annuaire Azure AD. Pour obtenir des instructions détaillées, voir [Gérer les utilisateurs de comptes](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

2.  Dans la page **Gérer les utilisateurs**, cliquez sur **Ajouter des applications Azure AD**, ajoutez l’application Azure AD qui représente l’application ou le service que vous allez utiliser pour accéder aux soumissions pour votre compte du Centre de développement, puis attribuez-lui le rôle **Gestionnaire**. Si cette application existe déjà dans votre annuaire AzureAD, vous pouvez la sélectionner dans la page **Ajouter des applications Azure AD** pour l’ajouter à votre compte du Centre de développement. Sinon, vous pouvez créer une application Azure AD dans la page **Ajouter des applications Azure AD**. Pour plus d’informations, voir [Ajouter et gérer des applications Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

3.  Revenez à la page **Gérer les utilisateurs**, cliquez sur le nom de votre application Azure AD pour accéder aux paramètres de l’application, puis notez les valeurs des champs **ID de locataire** et **ID client**.

4. Cliquez sur **Ajouter une clé**. Dans l’écran suivant, notez la valeur du champ **Clé**. Vous ne pourrez plus accéder à ces informations une fois que vous aurez quitté cette page. Pour en savoir plus, consultez les informations sur la gestion des clés dans l’article [Ajouter et gérer des applications Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

<span id="obtain-an-azure-ad-access-token" />
## <a name="step-2-obtain-an-azure-ad-access-token"></a>Étape2: Obtenir un jeton d’accès Azure AD

Avant d’appeler l’une des méthodes dans l’API de soumission du Windows Store, vous devez d’abord obtenir un jeton d’accès Azure AD pour le passer à l’en-tête **Autorisation** de chaque méthode de l’API. Après avoir obtenu un jeton d’accès, vous disposez de 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez l’actualiser pour pouvoir continuer à l’utiliser dans d’autres appels à l’API.

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

Pour la valeur *tenant\_id* dans l’URI POST et les paramètres *client\_id* et *client\_secret*, spécifiez l’ID de locataire, l’ID client et la clé pour votre application que vous avez récupérés à partir du Centre de développement à l’étape précédente. Pour le paramètre *resource*, vous devez spécifier ```https://manage.devcenter.microsoft.com```.

Une fois votre jeton d’accès arrivé à expiration, vous pouvez l’actualiser en suivant les instructions fournies [ici](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

Pour voir des exemples d’utilisation de code C#, Java ou Python pour obtenir un jeton d’accès, consultez les [exemples de code](#code-examples) de l’API de soumission du Windows Store.

<span id="call-the-windows-store-submission-api">
## <a name="step-3-use-the-windows-store-submission-api"></a>Étape3: Utiliser l’API de soumission du Windows Store

À partir du moment où vous disposez d’un jeton d’accès Azure AD, vous pouvez appeler des méthodes dans l’API de soumission du Windows Store. L’API propose diverses méthodes qui sont regroupées dans des scénarios pour applications, extensions et versions d’essai de package. Pour créer ou mettre à jour des soumissions, il convient généralement d’appeler plusieurs méthodes de l’API de soumission du Windows Store dans un ordre spécifique. Pour plus d’informations sur chaque scénario et sur la syntaxe de chacune de ces méthodes, voir les articles indiqués dans le tableau suivant.

>**Remarque**&nbsp;&nbsp;Après avoir obtenu un jeton d’accès, vous avez 60minutes pour appeler des méthodes dans l’API de soumission du Windows Store. Passé ce délai, il expire.

| Scénario       | Description                                                                 |
|---------------|----------------------------------------------------------------------|
| Applications |  Récupère des données pour toutes les applications inscrites dans votre compte du Centre de développement Windows et crée des soumissions pour les applications. Pour plus d’informations sur ces méthodes, voir les articles suivants: <ul><li>[Obtenir des données d’application](get-app-data.md)</li><li>[Gérer les soumissions d’applications](manage-app-submissions.md)</li></ul> |
| Extensions | Obtient, crée ou supprime des extensions pour vos applications, puis obtient, crée ou supprime des soumissions pour les extensions. Pour plus d’informations sur ces méthodes, voir les articles suivants: <ul><li>[Gérer les extensions](manage-add-ons.md)</li><li>[Gérer les soumissions d’extensions](manage-add-on-submissions.md)</li></ul> |
| Versions d’évaluation de package | Obtient, crée ou supprime des versions d’évaluation de package pour vos applications, puis obtient, crée ou supprime des soumissions pour les versions d’évaluation de package. Pour plus d’informations sur ces méthodes, voir les articles suivants: <ul><li>[Gérer les versions d’évaluation de package](manage-flights.md)</li><li>[Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md)</li></ul> |

<span id="code-samples"/>
## <a name="code-examples"></a>Exemples de code

Les articles suivants fournissent des exemples de code détaillés qui montrent comment utiliser l’API de soumission du Windows Store dans différents langages de programmation:

* [Exemples de code C#](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Exemples de code Java](java-code-examples-for-the-windows-store-submission-api.md)
* [Exemples de code Python](python-code-examples-for-the-windows-store-submission-api.md)

>**Remarque**&nbsp;&nbsp;En complément des exemples de code répertoriés ci-dessus, nous fournissons également un module PowerShell OpenSource qui implémente une interface de ligne de commande sur l'API de soumission du WindowsStore. Ce module est appelé [StoreBroker](https://aka.ms/storebroker). Vous pouvez utiliser ce module pour gérer les soumissions de votre application, de votre version et de vos modules complémentaires à partir de la ligne de commande, en lieu et place de l'appel direct de l'API de soumission du WindowsStore. Sinon, vous pouvez simplement parcourir la source pour consulter des exemples supplémentaires d'appel de cette API. Le module StoreBroker est activement utilisé au sein de Microsoft en tant que vecteur principal de soumission de nombreuses applications internes dans le WindowsStore. Pour plus d'informations, consultez notre [page StoreBroker sur GitHub](https://aka.ms/storebroker).

## <a name="troubleshooting"></a>Résolution des problèmes

| Problème      | Résolution                                          |
|---------------|---------------------------------------------|
| Après avoir appelé l’API de soumission du Windows Store à partir de PowerShell, les données de réponse destinées à l’API sont altérées si vous les convertissez du format JSON en objet PowerShell à l’aide de l’applet de commande [ConvertFrom Json](https://technet.microsoft.com/library/hh849898.aspx) et les rétablissez ensuite au format JSON à l’aide de l’applet de commande [ConvertTo Json](https://technet.microsoft.com/library/hh849922.aspx). |  Par défaut, le paramètre *-Depth* de l’applet de commande [ConvertTo Json](https://technet.microsoft.com/library/hh849922.aspx) est défini à 2niveaux d’objets, ce qui est trop superficiel pour la plupart des objets JSON retournées par l’API de soumission du Windows Store. Quand vous appelez l’applet de commande [ConvertTo Json](https://technet.microsoft.com/library/hh849922.aspx), attribuez au paramètre *-Depth* une valeur supérieure, par exemple 20. |

## <a name="additional-help"></a>Aide supplémentaire

Si vous avez des questions sur l’API de soumission du Windows Store ou si vous avez besoin d’aide pour gérer vos soumissions avec cette API, utilisez les ressources suivantes:

* Posez vos questions sur nos [forums](https://social.msdn.microsoft.com/Forums/windowsapps/home?forum=wpsubmit).
* Consultez notre [page de support](https://developer.microsoft.com/windows/support) et choisissez l’une des options de support assisté pour le tableau de bord du Centre de développement. Si vous êtes invité à choisir un type de problème et une catégorie, choisissez respectivement **Soumission d’application et certification** et **Soumission d’une application**.  

## <a name="related-topics"></a>Rubriques connexes

* [Obtenir des données d’application](get-app-data.md)
* [Gérer les soumissions d’applications](manage-app-submissions.md)
* [Gérer les extensions](manage-add-ons.md)
* [Gérer les soumissions d’extensions](manage-add-on-submissions.md)
* [Gérer les versions d’évaluation de package](manage-flights.md)
* [Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md)