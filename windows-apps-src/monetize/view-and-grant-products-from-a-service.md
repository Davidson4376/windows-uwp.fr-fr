---
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: Si vous disposez d’un catalogue d’applications et d’extensions, vous pouvez utiliser l’API de collection du MicrosoftStore et l’API d’achat du MicrosoftStore pour accéder aux informations de propriété de ces produits à partir de vos services.
title: Gérer les droits sur les produits à partir d’un service
ms.date: 08/01/2018
ms.topic: article
keywords: windows10, uwp, API de collection du MicrosoftStore, API d’achat du Microsoft Store, afficher des produits, octroyer des produits
ms.localizationpriority: medium
ms.openlocfilehash: 68bcee02c07ea8c998927d558521084cb49e9e24
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8900519"
---
# <a name="manage-product-entitlements-from-a-service"></a>Gérer les droits sur les produits à partir d’un service

Si vous disposez d’un catalogue d’applications et d’extensions, vous pouvez utiliser l’*API de collection du MicrosoftStore* et l’*API d’achat du MicrosoftStore* pour accéder aux informations de droit de ces produits à partir de vos services. Un *droit* représente le droit d’un client d’utiliser une application ou une extension qui est publiée par le biais du MicrosoftStore.

Ces API sont constituées des méthodes REST, qui sont conçues pour être utilisées par les développeurs dont les catalogues d’extensions sont pris en charge par les services multiplateformes. Ces API vous permettent d’effectuer les actions suivantes:

-   API de collection du MicrosoftStore: [demander des produits possédés par un utilisateur](query-for-products.md) et [signaler le traitement de la commande d’un produit consommable](report-consumable-products-as-fulfilled.md).
-   API d’achat du MicrosoftStore: [attribuer un produit gratuit à un utilisateur](grant-free-products.md), [recueillir les abonnements d’un utilisateur](get-subscriptions-for-a-user.md) et [modifier l’état de facturation de l’abonnement d’un utilisateur](change-the-billing-state-of-a-subscription-for-a-user.md).

> [!NOTE]
> L’API de collection et l’API d’achat du MicrosoftStore utilisent l’authentification Azure Active Directory (AzureAD) pour accéder aux informations de propriété client. Pour utiliser ces API, vous (ou votre organisation) devez disposer d’un annuaire Azure AD et d’une autorisation [Administrateur global](http://go.microsoft.com/fwlink/?LinkId=746654) pour l’annuaire. Si vous utilisez déjà Office365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’un annuaire Azure AD.

## <a name="overview"></a>Vue d’ensemble

Les étapes suivantes décrivent le processus de bout en bout pour l’utilisation de l’API de collection et de l'API d’achat du MicrosoftStore:

1.  [Configurer une application dans Azure AD](#step-1).
2.  [Associer votre ID d’application Azure AD avec votre application dans l’espace partenaires](#step-2).
3.  Dans votre service, [créez des jetons d’accès Azure AD](#step-3) qui représentent votre identité d’éditeur.
4.  Dans votre application Windows du client, [créer une clé d’ID du Microsoft Store](#step-4) qui représente l’identité de l’utilisateur actuel, puis transmettez cette clé revient à votre service.
5.  Lorsque vous disposez du jeton d’accès AzureAD et de la clé d’ID du MicrosoftStore requis, [appelez l’API de collection et l’API d’achat du MicrosoftStore à partir de votre service](#step-5).

Ce processus de bout en bout pour comprend deux composants logiciels qui effectuent différentes tâches:

* **Votre service**. Il s’agit d’une application qui s’exécute en toute sécurité dans le contexte de votre environnement d’entreprise, et il peut être implémenté à l’aide de n’importe quelle plateforme de développement que vous choisissez. Votre service est responsable de créer les jetons d’accès Azure AD pour le scénario et nécessaires pour appeler les URI dans le reste de la collection de Microsoft Store API et achat.
* **Votre application de Windows client**. Il s’agit de l’application pour laquelle vous souhaitez accéder et gérer les informations de droit de client (y compris les extensions pour l’application). Cette application est chargée de créer les clés d’ID du Microsoft Store que vous devez appeler l’API de collection de Microsoft Store et l’API d’achat de votre service.

<span id="step-1"/>

## <a name="step-1-configure-an-application-in-azure-ad"></a>Étape 1: Configurer une application dans Azure AD

Avant que vous pouvez utiliser l’API de collection de Microsoft Store ou d’achat, vous devez créer une application Azure AD Web, récupérer l’ID de locataire et ID d’application pour l’application et générer une clé. L’application Azure AD Web représente le service à partir duquel vous allez appeler l’API de collection de Microsoft Store ou d’achat. Vous devez le ID de locataire, ID d’application et la clé pour générer des jetons d’accès Azure AD que vous devez appeler l’API.

> [!NOTE]
> Les tâches de cette section ne doivent être accomplies qu’une seule fois. Une fois que vous mettez à jour votre manifeste d’application Azure AD et que vous avez votre ID de locataire, la clé secrète du client et d’ID d’application, vous pouvez réutiliser ces valeurs tout moment, vous devez créer un nouveau jeton d’accès Azure AD.

1.  Si vous n’avez pas déjà fait, suivez les instructions de [l’Intégration d’Applications à Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) pour inscrire un **application Web / API** application avec Azure AD.
    > [!NOTE]
    > Lorsque vous inscrivez votre application, vous devez choisir **application Web / API** en tant que type de l’application afin que vous pouvez récupérer une clé (également appelée une *clé secrète du client*) pour votre application. Pour appeler l’API de collection ou d’achat du MicrosoftStore, vous devez fournir une clé secrète client lorsque vous effectuez une demande de jeton d’accès auprès d’AzureAD au cours d’une étape ultérieure.

2.  Dans le [Portail de gestion Azure](https://portal.azure.com/), accédez à **Azure Active Directory**. Sélectionnez votre répertoire et cliquez sur **inscriptions** dans le volet de navigation de gauche, puis sélectionnez votre application.
3.  Vous êtes redirigé vers la page d’inscription principal de l’application. Sur cette page, copiez la valeur **d’ID d’Application** pour une utilisation ultérieure.
4.  Créer une clé dont vous aurez besoin ultérieurement (tous les appelée une *clé secrète du client*). Dans le volet gauche, cliquez sur **les paramètres** et les **clés**. Sur cette page, effectuez les étapes pour [créer une clé](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis). Copiez cette clé pour une utilisation ultérieure.
5.  Ajouter plusieurs URI d’audience nécessaire à votre [manifeste d’application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest). Dans le volet gauche, cliquez sur **le manifeste**. Cliquez sur **Modifier**, remplacez le `"identifierUris"` section par le texte suivant, puis cliquez sur **Enregistrer**.

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    Ces chaînes représentent les audiences prises en charge par votre application. Dans une étape ultérieure, vous allez créer des jetons d’accès Azure AD qui seront associés à chacune de ces valeurs d’audience.

<span id="step-2"/>

## <a name="step-2-associate-your-azure-ad-application-id-with-your-client-app-in-partner-center"></a>Étape 2: Associer votre ID d’application Azure AD à votre application cliente dans l’espace partenaires

Avant que vous pouvez utiliser l’API de collection de Microsoft Store ou d’achat pour configurer le propriétaire et les achats de votre application ou module complémentaire, vous devez associer votre ID d’application Azure AD à l’application (ou l’application qui contient l’extension) dans l’espace partenaires.

> [!NOTE]
> Cette tâche ne doit être effectuée qu’une seule fois.

1.  Se connecter à [L’espace partenaires](https://partner.microsoft.com/dashboard) et sélectionnez votre application.
2.  Accédez aux **Services** &gt; **produit collections et achats de** page et entrez votre ID d’application Azure AD dans l’un des champs **ID Client** disponibles.

<span id="step-3"/>

## <a name="step-3-create-azure-ad-access-tokens"></a>Étape3: créer des jetons d’accès AzureAD

Pour pouvoir récupérer une clé d’ID du MicrosoftStore ou appeler l’API de collection ou d’achat du MicrosoftStore, votre service doit créer plusieurs jetons d’accès AzureAD différents qui représentent votre identité d’éditeur. Chaque jeton est utilisé avec une autre API. La durée de vie de chacun des jetons est de 60minutes, et vous pouvez les actualiser une fois qu’ils sont arrivés à expiration.

> [!IMPORTANT]
> Créez des jetons d’accès AzureAD uniquement dans le contexte de votre service, et non dans votre application. Votre clé secrète client risque d’être compromise si elle est envoyée à votre application.

<span id="access-tokens" />

### <a name="understanding-the-different-tokens-and-audience-uris"></a>Comprendre les différents jetons et les URI d’audience

Selon les méthodes que vous voulez appeler dans l’API de collection ou d’achat du MicrosoftStore, vous devez créer deux ou trois jetons différents. Chaque jeton d’accès est associé à un URI d’audience différent (il s’agit des URI que vous avez ajoutés à la section `"identifierUris"` du manifeste d’application Azure AD).

  * Dans tous les cas, vous devez créer un jeton avec l'URI d’audience `https://onestore.microsoft.com`. Dans une étape ultérieure, vous allez passer ce jeton à l'en-tête **Autorisation** des méthodes de l’API de collection ou d’achat du MicrosoftStore.
      > [!IMPORTANT]
      > Utilisez l’audience `https://onestore.microsoft.com` uniquement avec les jetons d’accès qui sont stockés en toute sécurité dans votre service. L’exposition des jetons d’accès avec cette audience en dehors de votre service peut rendre votre service vulnérable aux attaques par relecture.

  * Si vous voulez appeler une méthode dans l’API de collection du MicrosoftStore pour [demander des produits appartenant à un utilisateur](query-for-products.md) ou [signaler le traitement de la commande d'un produit consommable](report-consumable-products-as-fulfilled.md), vous devez également créer un jeton avec l'URI d’audience `https://onestore.microsoft.com/b2b/keys/create/collections`. Dans une étape ultérieure, vous passerez ce jeton d’accès à une méthode de client dans le SDK Windows pour demander une clé d’ID du MicrosoftStore à utiliser avec l’API de collection du MicrosoftStore.

  * Si vous voulez appeler une méthode dans l’API d’achat de MicrosoftStore pour [attribuer un produit gratuit à un utilisateur](grant-free-products.md), [recueillir les abonnements d’un utilisateur](get-subscriptions-for-a-user.md) ou [modifier l’état de facturation de l’abonnement d’un utilisateur](change-the-billing-state-of-a-subscription-for-a-user.md), vous devez également créer un jeton avec l’URI d’audience `https://onestore.microsoft.com/b2b/keys/create/purchase`. Dans une étape ultérieure, vous passerez ce jeton à une méthode client dans le SDK Windows pour demander une clé d’ID du MicrosoftStore que vous pouvez utiliser avec l’API d’achat du MicrosoftStore.

<span />

### <a name="create-the-tokens"></a>Créer les jetons

Pour créer les jetons d’accès, utilisez l’API OAuth2.0 dans votre service en suivant les instructions de la section [Appels de service à service à l’aide des informations d’identification du client](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service) pour envoyer une requête HTTP POST au point de terminaison ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Voici un exemple de requête.

``` syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://onestore.microsoft.com
```

Pour chaque jeton, spécifiez les données de paramètre suivantes:

* Pour les paramètres *client\_id* et *client\_secret* , spécifiez l’ID d’application et la clé secrète du client pour votre application que vous avez récupérées à partir du [Portail de gestion Azure](http://manage.windowsazure.com). Ces deux paramètres sont nécessaires pour créer un jeton d’accès disposant du niveau d’authentification requis par l’API de collection ou d’achat du MicrosoftStore.

* Pour le paramètre *ressource*, spécifiez l'un des URI d’audience répertoriés dans la [section précédente](#access-tokens), selon le type de jeton d’accès que vous créez.

Une fois votre jeton d’accès arrivé à expiration, vous pouvez l’actualiser en suivant les instructions fournies [ici](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens). Pour plus d’informations sur la structure d’un jeton d’accès, voir [Jeton et types de réclamations pris en charge](http://go.microsoft.com/fwlink/?LinkId=722501).

<span id="step-4"/>

## <a name="step-4-create-a-microsoft-store-id-key"></a>Étape4: créer une clé d’ID du MicrosoftStore

Avant de pouvoir utiliser une quelconque méthode dans l’API de collection ou d’achat du MicrosoftStore, votre application doit créer une clé d’ID MicrosoftStore et l’envoyer à votre service. Il s’agit d’une clé d’authentification web JSON (JWT) qui représente l’identité de l’utilisateur pour lequel vous souhaitez accéder aux informations de propriété. Pour plus d’informations sur les revendications dans cette clé, voir [Revendications dans une clé d’ID du MicrosoftStore](#claims-in-a-microsoft-store-id-key).

Actuellement, la seule façon de créer une clé d’ID du MicrosoftStore est en appelant une API de plateforme Windows universelle (UWP) à partir du code client dans votre application. La clé générée représente l’identité de l’utilisateur actuellement connecté au MicrosoftStore sur l’appareil.

> [!NOTE]
> Chaque clé d’ID du MicrosoftStore est valide pendant 90jours. Après l’expiration d’une clé, vous pouvez [la renouveler](renew-a-windows-store-id-key.md). Nous vous recommandons de renouveler vos clés d’ID du MicrosoftStore plutôt que d’en créer d’autres.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-collection-api"></a>Pour créer une clé d’ID du MicrosoftStore pour l’API de collection du MicrosoftStore

Suivez ces étapes pour créer une clé d’ID du MicrosoftStore que vous pouvez utiliser avec l’API de collection du MicrosoftStore pour [demander des produits appartenant à un utilisateur](query-for-products.md) ou [signaler le traitement de la commande d'un produit consommable](report-consumable-products-as-fulfilled.md).

1.  Transmettez le jeton d’accès AzureAD associé à la valeur d’URI d’audience `https://onestore.microsoft.com/b2b/keys/create/collections` à partir de votre service à votre application cliente. Il s’agit d’un des jetons que vous avez créés à [l’étape3 précédente](#step-3).

2.  Dans le code de votre application, appelez l’une de ces méthodes pour récupérer une clé d’ID du MicrosoftStore:

  * Si votre application utilise la classe [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) dans l'espace de noms [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) pour gérer des achats in-app, utilisez la méthode [StoreContext.GetCustomerCollectionsIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomercollectionsidasync).

  * Si votre application utilise la classe [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) dans l'espace de noms [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) pour gérer des achats in-app, utilisez la méthode [CurrentApp.GetCustomerCollectionsIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomercollectionsidasync).

    Transmettez votre jeton d’accès Azure AD au paramètre *serviceTicket* de la méthode. Si vous gérez les identifiants d’utilisateur anonyme dans le contexte des services que vous gérez en tant que l’éditeur de l’application actuelle, vous pouvez également passer un ID d’utilisateur pour le paramètre *publisherUserId* pour associer l’utilisateur actuel avec la nouvelle clé d’ID du Microsoft Store (l’ID d’utilisateur sera em rodées dans la clé). Dans le cas contraire, si vous n’avez pas besoin d’associer un ID d’utilisateur avec la clé d’ID du Microsoft Store, vous pouvez passer n’importe quelle valeur de chaîne pour le paramètre *publisherUserId* .

3.  Une fois que votre application a créé une clé d’ID du MicrosoftStore, retransmettez-la à votre service.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-purchase-api"></a>Pour créer une clé d’ID du MicrosoftStore pour l’API d’achat du MicrosoftStore

Suivez ces étapes pour créer une clé d’ID du MicrosoftStore que vous pourrez utiliser avec l’API d’achat du MicrosoftStore pour [attribuer un produit gratuit à un utilisateur](grant-free-products.md), [recueillir les abonnements d’un utilisateur](get-subscriptions-for-a-user.md) ou [modifier l’état de facturation de l’abonnement d’un utilisateur](change-the-billing-state-of-a-subscription-for-a-user.md).

1.  Transmettez le jeton d’accès AzureAD associé à la valeur d’URI d’audience `https://onestore.microsoft.com/b2b/keys/create/purchase` à partir de votre service à votre application cliente. Il s’agit d’un des jetons que vous avez créés à [l’étape3 précédente](#step-3).

2.  Dans le code de votre application, appelez l’une de ces méthodes pour récupérer une clé d’ID du MicrosoftStore:

  * Si votre application utilise la classe [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) dans l'espace de noms [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) pour gérer des achats in-app, utilisez la méthode [StoreContext.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomerpurchaseidasync).

  * Si votre application utilise la classe [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) dans l'espace de noms [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) pour gérer des achats in-app, utilisez la méthode [CurrentApp.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomerpurchaseidasync).

    Transmettez votre jeton d’accès Azure AD au paramètre *serviceTicket* de la méthode. Si vous gérez les identifiants d’utilisateur anonyme dans le contexte des services que vous gérez en tant que l’éditeur de l’application actuelle, vous pouvez également passer un ID d’utilisateur pour le paramètre *publisherUserId* pour associer l’utilisateur actuel avec la nouvelle clé d’ID du Microsoft Store (l’ID d’utilisateur sera em rodées dans la clé). Dans le cas contraire, si vous n’avez pas besoin d’associer un ID d’utilisateur avec la clé d’ID du Microsoft Store, vous pouvez passer n’importe quelle valeur de chaîne pour le paramètre *publisherUserId* .

3.  Une fois que votre application a créé une clé d’ID du MicrosoftStore, retransmettez-la à votre service.

### <a name="diagram"></a>Diagramme

Le diagramme suivant illustre le processus de création d’une clé d’ID du Microsoft Store.

  ![Créer une clé d’ID du Windows Store](images/b2b-1.png)

<span id="step-5"/>

## <a name="step-5-call-the-microsoft-store-collection-api-or-purchase-api-from-your-service"></a>Étape 5: appeler l’API de collection ou d’achat du MicrosoftStore à partir de votre service

Lorsque votre service dispose d’une clé d’ID du MicrosoftStore qui lui permet d’accéder aux informations de propriété produit d’un utilisateur, votre service peut appeler l’API de collection ou d’achat du MicrosoftStore en suivant les instructions ci-après:

* [Demander des produits](query-for-products.md)
* [Signaler le traitement de la commande d’un produit consommable](report-consumable-products-as-fulfilled.md)
* [Octroyer des produits gratuits](grant-free-products.md)
* [Recueillir les abonnements d’un utilisateur](get-subscriptions-for-a-user.md)
* [Modifier l’état de facturation de l’abonnement d’un utilisateur](change-the-billing-state-of-a-subscription-for-a-user.md)

Pour chaque scénario, fournissez les informations suivantes à l’API:

-   Dans l’en-tête de la demande, transmettez le jeton d’accès AzureAD associé à la valeur d’URI d’audience `https://onestore.microsoft.com`. Il s’agit d’un des jetons que vous avez créés à [l’étape3 précédente](#step-3). Ce jeton représente votre identité d’éditeur.
-   Dans le corps de la demande, transmettez la clé d’ID du MicrosoftStore que vous avez récupérée à [l’étape4 précédente](#step-4) à partir du code côté client dans votre application. Cette clé représente l’identité de l’utilisateur pour lequel vous souhaitez accéder aux informations de propriété.

### <a name="diagram"></a>Diagramme

Le schéma suivant décrit le processus d’appeler une méthode dans l’API ou achat de l’API de collection de Microsoft Store à partir de votre service.

  ![Appeler des collections ou des API d’achat](images/b2b-2.png)

## <a name="claims-in-a-microsoft-store-id-key"></a>Revendications dans une clé d’ID du MicrosoftStore

Une clé d’ID du MicrosoftStore est une clé d’authentification web JSON (JWT) qui représente l’identité de l’utilisateur pour lequel vous souhaitez accéder aux informations de propriété. Lorsqu’elle est décodée en Base64, une clé d’ID du MicrosoftStore contient les revendications suivantes.

* `iat`:&nbsp;&nbsp;&nbsp;Identifie l’heure à laquelle la clé a été émise. Cette revendication permet de déterminer l’âge du jeton. Cette valeur est exprimée en temps époque.
* `iss`:&nbsp;&nbsp;&nbsp;Identifie l’émetteur. Cette revendication a la même valeur que la revendication `aud`.
* `aud`:&nbsp;&nbsp;&nbsp;Identifie l’audience. Doit être définie sur l’une des valeurs suivantes: `https://collections.mp.microsoft.com/v6.0/keys` ou `https://purchase.mp.microsoft.com/v6.0/keys`.
* `exp`:&nbsp;&nbsp;&nbsp;Identifie l’heure d’expiration à laquelle ou après laquelle la clé ne sera plus acceptée pour traiter quoique ce soit, excepté le renouvellement des clés. La valeur de cette revendication est exprimée en temps époque.
* `nbf`:&nbsp;&nbsp;&nbsp;Identifie l’heure à laquelle le jeton sera accepté pour le traitement. La valeur de cette revendication est exprimée en temps époque.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`:&nbsp;&nbsp;&nbsp;ID client qui identifie le développeur.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`:&nbsp;&nbsp;&nbsp;Charge utile opaque (chiffrée et codée au format Base64) qui contient des informations destinées uniquement à être utilisées par les services du MicrosoftStore.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`:&nbsp;&nbsp;&nbsp;Identifiant utilisateur qui identifie l’utilisateur actuel dans le contexte de vos services. Il s’agit de la valeur que vous transmettez dans le paramètre *publisherUserId* facultatif de la [méthode que vous utilisez pour créer la clé](#step-4).
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`:&nbsp;&nbsp;&nbsp;URI qui permet de renouveler la clé.

Voici un exemple d’en-tête de clé d’ID du MicrosoftStore décodé.

```json
{
    "typ":"JWT",
    "alg":"RS256",
    "x5t":"agA_pgJ7Twx_Ex2_rEeQ2o5fZ5g"
}
```

Voici un exemple de revendications de clé d’ID du MicrosoftStore décodées.

```json
{
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId": "1d5773695a3b44928227393bfef1e13d",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload": "ZdcOq0/N2rjytCRzCHSqnfczv3f0343wfSydx7hghfu0snWzMqyoAGy5DSJ5rMSsKoQFAccs1iNlwlGrX+/eIwh/VlUhLrncyP8c18mNAzAGK+lTAd2oiMQWRRAZxPwGrJrwiq2fTq5NOVDnQS9Za6/GdRjeiQrv6c0x+WNKxSQ7LV/uH1x+IEhYVtDu53GiXIwekltwaV6EkQGphYy7tbNsW2GqxgcoLLMUVOsQjI+FYBA3MdQpalV/aFN4UrJDkMWJBnmz3vrxBNGEApLWTS4Bd3cMswXsV9m+VhOEfnv+6PrL2jq8OZFoF3FUUpY8Fet2DfFr6xjZs3CBS1095J2yyNFWKBZxAXXNjn+zkvqqiVRjjkjNajhuaNKJk4MGHfk2rZiMy/aosyaEpCyncdisHVSx/S4JwIuxTnfnlY24vS0OXy7mFiZjjB8qL03cLsBXM4utCyXSIggb90GAx0+EFlVoJD7+ZKlm1M90xO/QSMDlrzFyuqcXXDBOnt7rPynPTrOZLVF+ODI5HhWEqArkVnc5MYnrZD06YEwClmTDkHQcxCvU+XUEvTbEk69qR2sfnuXV4cJRRWseUTfYoGyuxkQ2eWAAI1BXGxYECIaAnWF0W6ThweL5ZZDdadW9Ug5U3fZd4WxiDlB/EZ3aTy8kYXTW4Uo0adTkCmdLibw=",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId": "infusQMLaYCrgtC0d/SZWoPB4FqLEwHXgZFuMJ6TuTY=",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri": "https://collections.mp.microsoft.com/v6.0/b2b/keys/renew",
    "iat": 1442395542,
    "iss": "https://collections.mp.microsoft.com/v6.0/keys",
    "aud": "https://collections.mp.microsoft.com/v6.0/keys",
    "exp": 1450171541,
    "nbf": 1442391941
}
```

## <a name="related-topics"></a>Rubriquesassociées

* [Demander des produits](query-for-products.md)
* [Signaler le traitement de la commande d’un produit consommable](report-consumable-products-as-fulfilled.md)
* [Octroyer des produits gratuits](grant-free-products.md)
* [Recueillir les abonnements d’un utilisateur](get-subscriptions-for-a-user.md)
* [Modifier l’état de facturation de l’abonnement d’un utilisateur](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Renouveler une clé d’ID du MicrosoftStore](renew-a-windows-store-id-key.md)
* [Intégration d’applications à Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722502)
* [Présentation du manifeste de l’application Azure Active Directory]( http://go.microsoft.com/fwlink/?LinkId=722500)
* [Jeton et types de réclamations pris en charge](http://go.microsoft.com/fwlink/?LinkId=722501)
