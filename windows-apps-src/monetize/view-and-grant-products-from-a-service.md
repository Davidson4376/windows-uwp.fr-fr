---
author: mcleanbyron
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: "Si vous disposez d’un catalogue d’applications et d’extensions, vous pouvez utiliser l’API de collection du WindowsStore et l’API d’achat du WindowsStore pour accéder aux informations de propriété de ces produits à partir de vos services."
title: "Gérer les droits sur les produits à partir d’un service"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows10, uwp, API de collection du WindowsStore, API d’achat du Windows Store, afficher des produits, octroyer des produits"
ms.openlocfilehash: 6ecc9d6014692cac52f5554f78a0773dfee3fb81
ms.sourcegitcommit: e7e8de39e963b73ba95cb34d8049e35e8d5eca61
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2017
---
# <a name="manage-product-entitlements-from-a-service"></a>Gérer les droits sur les produits à partir d’un service

Si vous disposez d’un catalogue d’applications et d’extensions, vous pouvez utiliser l’*API de collection du WindowsStore* et l’*API d’achat du WindowsStore* pour accéder aux informations de droit de ces produits à partir de vos services. Un *droit* représente le droit d'un client d’utiliser une application ou une extension qui est publiée par le biais du Windows Store.

Ces API sont constituées des méthodes REST, qui sont conçues pour être utilisées par les développeurs dont les catalogues d’extensions sont pris en charge par les services multiplateformes. Ces API vous permettent d’effectuer les actions suivantes:

-   API de collection du Windows Store: [demander des produits possédés par un utilisateur](query-for-products.md) et [signaler le traitement de la commande d’un produit consommable](report-consumable-products-as-fulfilled.md).
-   API d’achat du WindowsStore: [attribuer un produit gratuit à un utilisateur](grant-free-products.md), [recueillir les abonnements d’un utilisateur](get-subscriptions-for-a-user.md), et [modifier l’état de facturation de l’abonnement d’un utilisateur](change-the-billing-state-of-a-subscription-for-a-user.md).

> [!NOTE]
> L’API de collection et l’API d’achat du WindowsStore utilisent l’authentification Azure Active Directory (AzureAD) pour accéder aux informations de propriété client. Pour utiliser ces API, vous (ou votre organisation) devez disposer d’un annuaire Azure AD et d’une autorisation [Administrateur global](http://go.microsoft.com/fwlink/?LinkId=746654) pour l’annuaire. Si vous utilisez déjà Office365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’un annuaire Azure AD.

## <a name="overview"></a>Vue d’ensemble

Les étapes suivantes décrivent le processus de bout en bout pour l’utilisation de l’API de collection et de l'API d’achat du Windows Store:

1.  [Configurer une application Web dans Azure AD](#step-1).
2.  [Associer votre ID client Azure AD à votre application dans le tableau de bord du Centre de développement Windows](#step-2).
3.  Dans votre service, [créer des jetons d’accès Azure AD](#step-3) qui représentent votre identité d’éditeur.
4.  Dans le code côté client de votre application Windows, [créer une clé d’ID du Windows Store](#step-4) qui représente l’identité de l’utilisateur actuel et retransmettre la clé d’ID du Windows Store à votre service.
5.  Lorsque vous disposez du jeton d’accès Azure AD et de la clé d’ID du Windows Store requis, [appelez l’API de collection et l’API d’achat du Windows Store à partir de votre service](#step-5).

Les sections suivantes fournissent plus d’informations sur chacune de ces étapes.

<span id="step-1"/>
## <a name="step-1-configure-a-web-application-in-azure-ad"></a>Étape1: configurer une application Web dans Azure AD

Avant de pouvoir utiliser l’API de collection ou d’achat du Windows Store, vous devez créer une application Azure AD Web, récupérer l’ID de locataire et l'ID de client pour l’application et générer une clé. L’application Azure AD est l’application ou le service à partir duquel vous allez appeler l’API de collection ou d'achat du Windows Store. Vous avez besoin de l’ID de locataire, de l’ID client et de la clé pour obtenir le jeton d’accès Azure AD à transmettre à l’API.

> [!NOTE]
> Les tâches de cette section ne doivent être accomplies qu’une seule fois. Une fois que vous mettez à jour votre manifeste d’application Azure AD et que vous connaissez votre ID de locataire, l’ID client et la clé secrète du client, vous pouvez réutiliser ces valeurs à chaque fois que vous devez créer un nouveau jeton d’accès Azure AD.

1.  Suivez les instructions de l’article [Intégration d’applications à Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722502) pour ajouter une application Web à Azure AD.
    > [!NOTE]
    > Dans la page **Parlez nous de votre application**, veillez à bien choisir **Application Web et/ou API Web**. Cette opération est nécessaire pour vous permettre de récupérer une clé (également appelée *clé secrète client*) pour votre application. Pour appeler l’API de collection ou d’achat du WindowsStore, vous devez fournir une clé secrète client lorsque vous effectuez une demande de jeton d’accès auprès d’Azure AD au cours d’une étape ultérieure.

2.  Dans le [Portail de gestion Azure](http://manage.windowsazure.com/), accédez à **Active Directory**. Sélectionnez votre répertoire, cliquez sur l’onglet **Applications** en haut, puis sélectionnez votre application.
3.  Cliquez sur l’onglet **Configurer**. Dans cet onglet, obtenez l’ID client de votre application et demandez une clé (appelée *clé secrète client* dans les étapes suivantes).
4.  En bas de l’écran, cliquez sur **Gérer le manifeste**. Téléchargez le manifeste de votre application Azure AD et remplacez la section `"identifierUris"` par le texte suivant.

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    Ces chaînes représentent les audiences prises en charge par votre application. Dans une étape ultérieure, vous allez créer des jetons d’accès Azure AD qui seront associés à chacune de ces valeurs d’audience. Pour plus d’informations sur le téléchargement du manifeste de votre application, voir [Présentation du manifeste de l’application Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722500).

5.  Enregistrez le manifeste de votre application et chargez-le sur votre application dans le [Portail de gestion Azure](http://manage.windowsazure.com/).

<span id="step-2"/>
## <a name="step-2-associate-your-azure-ad-client-id-with-your-app-in-windows-dev-center"></a>Étape2: associer votre ID client AzureAD à votre application dans le Centre de développement Windows

Avant de pouvoir utiliser l’API de collecte du Windows Store ou d'acheter l'API permettant de gérer une application ou une extension, vous devez associer votre ID client Azure AD à l'application (ou l'application contenant l'extension) dans le tableau de bord du centre de développement.

> [!NOTE]
> Cette tâche ne doit être effectuée qu’une seule fois.

1.  Connectez-vous au [tableau de bord du Centre de développement](https://dev.windows.com/overview) et sélectionnez votre application.
2.  Accédez à la page **Services** &gt; **Collections et achats de produits**, puis entrez votre ID client AzureAD dans l’un des champs disponibles.

<span id="step-3"/>
## <a name="step-3-create-azure-ad-access-tokens"></a>Étape3: créer des jetons d’accès Azure AD

Pour pouvoir récupérer une clé d’ID du Windows Store ou appeler les API de collection ou d’achat du Windows Store, votre service doit créer plusieurs jetons d’accès Azure AD différents qui représentent votre identité d’éditeur. Chaque jeton est utilisé avec une autre API. La durée de vie de chacun des jetons est de 60minutes, et vous pouvez les actualiser une fois qu’ils sont arrivés à expiration.

> [!IMPORTANT]
> Créez des jetons d’accès AzureAD uniquement dans le contexte de votre service, et non dans votre application. Votre clé secrète client risque d’être compromise si elle est envoyée à votre application.

<span id="access-tokens" />
### <a name="understanding-the-different-tokens-and-audience-uris"></a>Comprendre les différents jetons et les URI d’audience

Selon les méthodes que vous voulez appeler dans l’API de collection ou d’achat du Windows Store, vous devez créer deux ou trois jetons différents. Chaque jeton d’accès est associé à un URI d’audience différent (il s’agit des URI que vous avez ajoutés à la section `"identifierUris"` du manifeste d’application Azure AD).

  * Dans tous les cas, vous devez créer un jeton avec l'URI d’audience `https://onestore.microsoft.com`. Dans une étape ultérieure, vous allez passer ce jeton à l'en-tête **Autorisation** des méthodes de l’API de collection ou d’achat du Windows Store.
      > [!IMPORTANT]
      > Utilisez l’audience `https://onestore.microsoft.com` uniquement avec les jetons d’accès qui sont stockés en toute sécurité dans votre service. L’exposition des jetons d’accès avec cette audience en dehors de votre service peut rendre votre service vulnérable aux attaques par relecture.

  * Si vous voulez appeler une méthode dans l’API de collection du Windows Store pour [demander des produits appartenant à un utilisateur](query-for-products.md) ou [signaler le traitement de la commande d'un produit consommable](report-consumable-products-as-fulfilled.md), vous devez également créer un jeton avec l'URI d’audience `https://onestore.microsoft.com/b2b/keys/create/collections`. Dans une étape ultérieure, vous passerez ce jeton d’accès à une méthode de client dans le SDK Windows pour demander une clé d’ID du WindowsStore à utiliser avec l’API de collection du WindowsStore.

  * Si vous voulez appeler une méthode dans l’API d’achat de WindowsStore pour [attribuer un produit gratuit à un utilisateur](grant-free-products.md), [recueillir les abonnements d’un utilisateur](get-subscriptions-for-a-user.md), ou [modifier l’état de facturation de l’abonnement d’un utilisateur](change-the-billing-state-of-a-subscription-for-a-user.md), vous devez également créer un jeton avec l’URI d’audience `https://onestore.microsoft.com/b2b/keys/create/purchase`. Dans une étape ultérieure, vous passerez ce jeton à une méthode client dans leSDK Windows pour demander une clé d’ID du WindowsStore que vous pouvez utiliser avec l’API d’achat du WindowsStore.

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

* Pour les paramètres *client\_id* et *client\_secret*, spécifiez l’ID client et la clé secrète client de votre application, que vous avez récupérée à partir du [Portail de gestion Azure](http://manage.windowsazure.com). Ces deux paramètres sont nécessaires pour créer un jeton d’accès disposant du niveau d’authentification requis par les API de collection ou d’achat du WindowsStore.

* Pour le paramètre *ressource*, spécifiez l'un des URI d’audience répertoriés dans la [section précédente](#access-tokens), selon le type de jeton d’accès que vous créez.

Une fois votre jeton d’accès arrivé à expiration, vous pouvez l’actualiser en suivant les instructions fournies [ici](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens). Pour plus d’informations sur la structure d’un jeton d’accès, voir [Jeton et types de réclamations pris en charge](http://go.microsoft.com/fwlink/?LinkId=722501).

<span id="step-4"/>
## <a name="step-4-create-a-windows-store-id-key"></a>Étape4: créer une clé d’ID du Windows Store

Avant de pouvoir utiliser une quelconque méthode dans l'API de collection ou d'achat du Windows Store, votre application doit créer une clé d'ID Windows Store et l'envoyer à votre service. Il s’agit d’une clé d’authentification web JSON (JWT) qui représente l’identité de l’utilisateur dont vous souhaitez accéder aux informations de propriété. Pour plus d’informations sur les revendications dans cette clé, voir [Revendications dans une clé d’ID du Windows Store](#claims).

Actuellement, la seule façon de créer une clé d’ID du Windows Store est en appelant une API de plateforme Windows universelle (UWP) à partir du code client dans votre application. La clé générée représente l’identité de l’utilisateur actuellement connecté au Windows Store sur l’appareil.

> [!NOTE]
> Chaque clé d’ID du WindowsStore est valide pendant 90jours. Après l’expiration d’une clé, vous pouvez [la renouveler](renew-a-windows-store-id-key.md). Nous vous recommandons de renouveler vos clés d’ID du Windows Store plutôt que d’en créer d’autres.

<span />
### <a name="to-create-a-windows-store-id-key-for-the-windows-store-collection-api"></a>Pour créer une clé d’ID du Windows Store pour l’API de collection du Windows Store

Suivez ces étapes pour créer une clé d’ID du Windows Store que vous pouvez utiliser avec l’API de collection du Windows Store pour [demander des produits appartenant à un utilisateur](query-for-products.md) ou [signaler le traitement de la commande d'un produit consommable](report-consumable-products-as-fulfilled.md).

1.  Transmettez le jeton d’accès Azure AD que vous avez créé avec l'URI d’audience `https://onestore.microsoft.com/b2b/keys/create/collections` à partir de votre service à votre application cliente.

2.  Dans le code de votre application, appelez l’une de ces méthodes pour récupérer une clé d’ID du Windows Store:

  * Si votre application utilise la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) dans l'espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) pour gérer des achats in-app, utilisez la méthode [StoreContext.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomercollectionsidasync.aspx).

  * Si votre application utilise la classe [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) dans l'espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) pour gérer des achats in-app, utilisez la méthode [CurrentApp.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608674).

    Transmettez votre jeton d’accès Azure AD au paramètre *serviceTicket* de la méthode. Vous pouvez éventuellement transmettre un ID au paramètre *publisherUserId* qui identifie l’utilisateur actuel dans le contexte de vos services. Si vous gérez les identifiants utilisateur de vos services, vous pouvez utiliser ce paramètre pour mettre en corrélation ces identifiants avec les appels que vous effectuez pour les API de collection du WindowsStore.

3.  Une fois que votre application a créé une clé d’ID du WindowsStore, retransmettez-la à votre service.

<span />
### <a name="to-create-a-windows-store-id-key-for-the-windows-store-purchase-api"></a>Pour créer une clé d’ID du Windows Store pour l’API d’achat du Windows Store

Suivez ces étapes pour créer une clé d’ID du WindowsStore que vous pourrez utiliser avec l’API d’achat du WindowsStore pour [attribuer un produit gratuit à un utilisateur](grant-free-products.md), [recueillir les abonnements d’un utilisateur](get-subscriptions-for-a-user.md), ou [modifier l’état de facturation de l’abonnement d’un utilisateur](change-the-billing-state-of-a-subscription-for-a-user.md).

1.  Transmettez le jeton d’accès Azure AD que vous avez créé avec l'URI d’audience `https://onestore.microsoft.com/b2b/keys/create/purchase` à partir de votre service à votre application cliente.

2.  Dans le code de votre application, appelez l’une de ces méthodes pour récupérer une clé d’ID du Windows Store:

  * Si votre application utilise la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) dans l'espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) pour gérer des achats in-app, utilisez la méthode [StoreContext.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomerpurchaseidasync.aspx).

  * Si votre application utilise la classe [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) dans l'espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) pour gérer des achats in-app, utilisez la méthode [CurrentApp.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608675).

    Transmettez votre jeton d’accès Azure AD au paramètre *serviceTicket* de la méthode. Vous pouvez éventuellement transmettre un ID au paramètre *publisherUserId* qui identifie l’utilisateur actuel dans le contexte de vos services. Si vous gérez les identifiants utilisateur de vos services, vous pouvez utiliser ce paramètre pour mettre en corrélation ces identifiants avec les appels que vous effectuez pour les API d'achat du WindowsStore.

3.  Une fois que votre application a créé une clé d’ID du WindowsStore, retransmettez-la à votre service.

<span id="step-5"/>
## <a name="step-5-call-the-windows-store-collection-api-or-purchase-api-from-your-service"></a>Étape 5: Appeler l’API de collection ou d’achat du Windows Store à partir de votre service

Lorsque votre service dispose d’une clé d’ID du Windows Store qui lui permet d’accéder aux informations de propriété produit d’un utilisateur, votre service peut appeler l’API de collection ou d’achat du Windows Store en suivant les instructions ci-après:

* [Demander des produits](query-for-products.md)
* [Signaler le traitement de la commande d’un produit consommable](report-consumable-products-as-fulfilled.md)
* [Octroyer des produits gratuits](grant-free-products.md)
* [Recueillir les abonnements d’un utilisateur](get-subscriptions-for-a-user.md)
* [Modifier l’état de facturation de l’abonnement d’un utilisateur](change-the-billing-state-of-a-subscription-for-a-user.md)

Pour chaque scénario, fournissez les informations suivantes à l’API:

-   Le jeton d’accès AzureAD que vous avez créé précédemment avec l’URI d’audience `https://onestore.microsoft.com`. Ce jeton représente votre identité d’éditeur. Transmettez ce jeton dans l’en-tête de requête.
-   La clé d’ID du Windows Store récupérée à partir du code côté client dans votre application. Cette clé représente l’identité de l’utilisateur dont vous souhaitez accéder aux informations de propriété.

<span id="claims"/>
## <a name="claims-in-a-windows-store-id-key"></a>Revendications dans une clé d’ID du Windows Store

Une clé d’ID du Windows Store est une clé d’authentification web JSON (JWT) qui représente l’identité de l’utilisateur dont vous souhaitez accéder aux informations de propriété. Lorsqu’elle est décodée en Base64, une clé d’ID du Windows Store contient les revendications suivantes.

* `iat`:&nbsp;&nbsp;&nbsp;Identifie l’heure à laquelle la clé a été émise. Cette revendication permet de déterminer l’âge du jeton. Cette valeur est exprimée en temps époque.
* `iss`:&nbsp;&nbsp;&nbsp;Identifie l’émetteur. Cette revendication a la même valeur que la revendication `aud`.
* `aud`:&nbsp;&nbsp;&nbsp;Identifie l’audience. Doit être définie sur l’une des valeurs suivantes: `https://collections.mp.microsoft.com/v6.0/keys` ou `https://purchase.mp.microsoft.com/v6.0/keys`.
* `exp`:&nbsp;&nbsp;&nbsp;Identifie l’heure d’expiration à laquelle ou après laquelle la clé ne sera plus acceptée pour traiter quoique ce soit, excepté le renouvellement des clés. La valeur de cette revendication est exprimée en temps époque.
* `nbf`:&nbsp;&nbsp;&nbsp;Identifie l’heure à laquelle le jeton sera accepté pour le traitement. La valeur de cette revendication est exprimée en temps époque.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`:&nbsp;&nbsp;&nbsp;ID client qui identifie le développeur.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`:&nbsp;&nbsp;&nbsp;Charge utile opaque (chiffrée et codée au format Base64) qui contient des informations destinées uniquement à être utilisées par les services du WindowsStore.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`:&nbsp;&nbsp;&nbsp;Identifiant utilisateur qui identifie l’utilisateur actuel dans le contexte de vos services. Il s’agit de la valeur que vous transmettez dans le paramètre *publisherUserId* facultatif de la [méthode que vous utilisez pour créer la clé](#step-4).
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`:&nbsp;&nbsp;&nbsp;URI qui permet de renouveler la clé.

Voici un exemple d’en-tête de clé d’ID du Windows Store décodé.

```json
{
    "typ":"JWT",
    "alg":"RS256",
    "x5t":"agA_pgJ7Twx_Ex2_rEeQ2o5fZ5g"
}
```

Voici un exemple de revendications de clé d’ID du WindowsStore décodées.

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

## <a name="related-topics"></a>Rubriques connexes

* [Demander des produits](query-for-products.md)
* [Signaler le traitement de la commande d’un produit consommable](report-consumable-products-as-fulfilled.md)
* [Attribuer des produits gratuits](grant-free-products.md)
* [Recueillir les abonnements d’un utilisateur](get-subscriptions-for-a-user.md)
* [Modifier l’état de facturation de l’abonnement d’un utilisateur](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Renouveler une clé d’ID du WindowsStore](renew-a-windows-store-id-key.md)
* [Intégration d’applications à Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722502)
* [Présentation du manifeste de l’application Azure Active Directory]( http://go.microsoft.com/fwlink/?LinkId=722500)
* [Jeton et types de réclamations pris en charge](http://go.microsoft.com/fwlink/?LinkId=722501)
