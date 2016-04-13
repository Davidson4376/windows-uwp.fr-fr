---
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
Si vous disposez d’un catalogue d’applications et de produits intégrés à l’application (PIA), vous pouvez utiliser l’API de collection du Windows Store et l’API d’achat du Windows Store pour accéder aux informations de propriété de ces produits à partir de vos services.
Afficher et octroyer des produits à partir d’un service
---

# Afficher et octroyer des produits à partir d’un service


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Si vous disposez d’un catalogue d’applications et de produits intégrés à l’application (PIA), vous pouvez utiliser l’*API de collection du Windows Store* et l’*API d’achat du Windows Store* pour accéder aux informations de propriété de ces produits à partir de vos services.

Ces API sont constituées des méthodes REST, qui sont conçues pour être utilisées par les développeurs dont les catalogues de PIA sont pris en charge par les services multiplateformes. Ces API vous permettent d’effectuer les actions suivantes :

-   API de collection du Windows Store : demander des applications ou des PIA possédés par un utilisateur donné ou signaler le traitement de la commande d’un produit consommable.
-   API d’achat du Windows Store : octroyer une application ou un PIA gratuits à un utilisateur donné.

## Utilisation de l’API de collection et de l’API d’achat du Windows Store


L’API de collection et l’API d’achat du Windows Store utilisent l’authentification Azure Active Directory (Azure AD) pour accéder aux informations de propriété client. Avant de pouvoir appeler ces API, vous devez appliquer des métadonnées Azure AD à votre application dans le tableau de bord du Centre de développement Windows et générer plusieurs jetons et clés d’accès requis. Les étapes suivantes décrivent le processus de bout en bout :

1.  [Configurer une application web dans Azure AD](#step-1:-configure-a-web-application-in-azure-ad). Cette application représente vos services inter-plateformes dans le contexte d’Azure AD.
2.  [Associer votre ID client Azure AD à votre application dans le tableau de bord du Centre de développement Windows](#step-2:-associate-your-azure-ad-client-id-with-your-application-in-the-windows-dev-denter-dashboard).
3.  Dans votre service, [générer des jetons d’accès Azure AD](#step-3:-retrieve-access-tokens-from-azure-ad) qui représentent votre identité d’éditeur.
4.  Dans le code côté client de votre application Windows, [générer une clé d’ID du Windows Store](#step-4:-generate-a-windows-store-id-key-from-client-side-code-in-your-app) qui représente l’identité de l’utilisateur actuel et retransmettre la clé d’ID du Windows Store à votre service.
5.  Lorsque vous disposez du jeton d’accès Azure AD et de la clé d’ID du Windows Store requis, [appelez l’API de collection et l’API d’achat du Windows Store à partir de votre service](#step-5:-call-the-windows-store-collection-api-or-purchase-api-from-your-service).

Les sections suivantes fournissent plus d’informations sur chacune de ces étapes.

### Étape 1 : Configurer une application web dans Azure AD

1.  Suivez les instructions de l’article [Intégration d’applications à Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722502) pour ajouter une application web à Azure AD.
    **Remarque** Dans la page **Parlez-nous de votre application**, veillez à bien choisir **Application web et/ou API web**. Cette opération est nécessaire pour vous permettre d’obtenir une clé (également appelée *clé secrète client*) pour votre application. Pour appeler l’API de collection ou d’achat du Windows Store, vous devez fournir une clé secrète client lorsque vous effectuez une demande de jeton d’accès auprès d’Azure AD au cours d’une étape ultérieure.
2.  Dans le [Portail de gestion Azure](http://manage.windowsazure.com/), accédez à **Active Directory**. Sélectionnez votre répertoire, cliquez sur l’onglet **Applications** en haut, puis sélectionnez votre application.
3.  Cliquez sur l’onglet **Configurer**. Dans cet onglet, obtenez l’ID client de votre application et demandez une clé (appelée *clé secrète client* dans les étapes suivantes).
4.  En bas de l’écran, cliquez sur **Gérer le manifeste**. Téléchargez le manifeste de votre application Azure AD et remplacez la section `"identifierUris"` par le texte suivant.

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    Ces chaînes représentent les audiences prises en charge par votre application. Dans une étape ultérieure, vous allez créer des jetons d’accès Azure AD qui seront associés à chacune de ces valeurs d’audience. Pour plus d’informations sur le téléchargement du manifeste de votre application, voir [Présentation du manifeste de l’application Azure Active Directory]( http://go.microsoft.com/fwlink/?LinkId=722500).

5.  Enregistrez le manifeste de votre application et chargez-le sur votre application dans le [Portail de gestion Azure](http://manage.windowsazure.com/).

### Étape 2 : Associer votre ID client Azure AD à votre application dans le tableau de bord du Centre de développement Windows

Les API de collection et d’achat du Windows Store permettent uniquement d’accéder aux informations de propriété d’un utilisateur relatives aux applications et aux PIA que vous avez associés à votre ID client Azure AD.

1.  Connectez-vous au [tableau de bord du Centre de développement Windows](https://dev.windows.com/overview) et sélectionnez votre application.
2.  Accédez à la page **Services** &gt; **Collections et achats de produits** et entrez votre ID client Azure AD dans l’un des champs disponibles.

### Étape 3 : Récupérer des jetons d’accès d’Azure AD

Pour pouvoir récupérer une clé d’ID du Windows Store ou appeler les API de collection ou d’achat du Windows Store, votre service doit demander trois jetons d’accès Azure AD qui représentent votre identité d’éditeur. Chacun de ces jetons d’accès est associé à un URI d’audience différent, et chaque jeton est utilisé avec un appel d’API différent. La durée de vie de chacun des jetons est de 60 minutes, et vous pouvez les actualiser une fois qu’ils sont arrivés à expiration.

Pour créer les jetons d’accès, utilisez l’API OAuth 2.0 dans votre service en suivant les instructions de la section [Appels de service à service à l’aide des informations d’identification du client](https://msdn.microsoft.com/library/azure/dn645543.aspx). Pour chaque jeton, spécifiez les données de paramètre suivantes :

-   Pour les paramètres *client\_id* et *client\_secret* , spécifiez l’ID client et la clé secrète client de votre application, obtenus à partir du [Portail de gestion Azure](http://manage.windowsazure.com/). Ces deux paramètres sont nécessaires pour générer un jeton d’accès disposant du niveau d’authentification requis par les API de collection ou d’achat du Windows Store.
-   Pour le paramètre *resource*, spécifiez l’un des URI d’ID d’application suivants (il s’agit des URI que vous avez précédemment ajoutés à la section `"identifierUris"` du manifeste de l’application). À la fin de ce processus, vous devez disposer de trois jetons d’accès, à chacun desquels l’un de ces URI d’ID d’application est associé.
    -   **https://onestore.microsoft.com/b2b/keys/create/collections** : dans une étape ultérieure, vous utiliserez le jeton d’accès que vous allez créer avec cet URI pour demander une clé d’ID du Windows Store que vous pouvez utiliser avec l’API de collection du Windows Store.
    -   **https://onestore.microsoft.com/b2b/keys/create/purchase** : dans une étape ultérieure, vous utiliserez le jeton d’accès que vous allez créer avec cet URI pour demander une clé d’ID du Windows Store que vous pouvez utiliser avec l’API d’achat du Windows Store.
    -   **https://onestore.microsoft.com** : dans une étape ultérieure, vous utiliserez le jeton d’accès que vous allez créer avec cet URI pour les appels directs des API de collection ou d’achat du Windows Store.

    **Important** Utilisez l’audience **https://onestore.microsoft.com** uniquement avec les jetons d’accès qui sont stockés en toute sécurité dans votre service. L’exposition des jetons d’accès avec cette audience en dehors de votre service peut rendre votre service vulnérable aux attaques par relecture.

Pour plus d’informations sur la structure d’un jeton d’accès, voir [Jeton et types de réclamations pris en charge](http://go.microsoft.com/fwlink/?LinkId=722501).

**Important** Vous devez créer des jetons d’accès Azure AD uniquement dans le contexte de votre service, et non dans votre application. Votre clé secrète client risque d’être compromise si elle est envoyée à votre application.

 

### Étape 4 : Générer une clé d’ID du Windows Store à partir du code côté client de votre application

Pour que vous puissiez appeler l’API de collection ou d’achat du Windows Store, votre service doit obtenir une clé d’ID du Windows Store. Il s’agit d’une clé d’authentification web JSON (JWT) qui représente l’identité de l’utilisateur dont vous souhaitez accéder aux informations de propriété. Pour plus d’informations sur les revendications dans cette clé, voir [Revendications dans une clé d’ID du Windows Store](#windowsstorekey).

La seule méthode actuelle permettant d’obtenir une clé d’ID du Windows Store consiste à appeler une API de plateforme Windows universelle (UWP) à partir du code côté client de votre application pour récupérer l’identité de l’utilisateur qui est actuellement connecté au Windows Store. Pour générer une clé d’ID du Windows Store :

1.  Transmettez l’un des jetons d’accès suivants de votre service à votre application cliente :
    -   Pour obtenir une clé d’ID du Windows Store utilisable avec l’API de collection du Windows Store, transmettez le jeton d’accès Azure AD que vous avez créé avec l’URI d’audience **https://onestore.microsoft.com/b2b/keys/create/collections**.
    -   Pour obtenir une clé d’ID du Windows Store utilisable avec l’API d’achat du Windows Store, transmettez le jeton d’accès Azure AD que vous avez créé avec l’URI d’audience **https://onestore.microsoft.com/b2b/keys/create/purchase**.

2.  Dans le code de votre application, appelez l’une des méthodes suivantes de la classe [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) pour récupérer une clé d’ID du Windows Store.

    -   [
            **GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) : appelez cette méthode si vous envisagez d’utiliser l’API de collection du Windows Store.
    -   [
            **GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675) : appelez cette méthode si vous envisagez d’utiliser l’API d’achat du Windows Store.

    Quelle que soit la méthode, transmettez votre jeton d’accès Azure AD au paramètre *serviceTicket*. Vous pouvez éventuellement transmettre un ID au paramètre *publisherUserId* qui identifie l’utilisateur actuel dans le contexte de vos services. Si vous gérez les identifiants utilisateur de vos services, vous pouvez utiliser ce paramètre pour mettre en corrélation ces identifiants avec les appels que vous effectuez pour les API de collection ou d’achat du Windows Store.

3.  Une fois que votre application a récupéré une clé d’ID du Windows Store, retransmettez-la à votre service.

**Remarque** Chaque clé d’ID du Windows Store est valide pendant 90 jours. Après l’expiration d’une clé, vous pouvez [la renouveler](renew-a-windows-store-id-key.md). Nous vous recommandons de renouveler vos clés d’ID du Windows Store plutôt que d’en créer d’autres.

 

### Étape 5 : Appeler l’API de collection ou d’achat du Windows Store à partir de votre service

Lorsque votre service dispose d’une clé d’ID du Windows Store qui lui permet d’accéder aux informations de propriété produit d’un utilisateur, votre service peut appeler l’API de collection ou d’achat du Windows Store. Suivez les instructions qui s’appliquent à votre scénario :

-   [Demander des produits](query-for-products.md)
-   [Signaler le traitement de la commande d’un produit consommable](report-consumable-products-as-fulfilled.md)
-   [Octroyer des produits gratuits](grant-free-products.md)

Pour chaque scénario, fournissez les informations suivantes à l’API :

-   Le jeton d’accès Azure AD que vous avez créé précédemment avec l’URI d’audience **https://onestore.microsoft.com**. Ce jeton représente votre identité d’éditeur. Transmettez ce jeton dans l’en-tête de requête.
-   La clé d’ID du Windows Store récupérée à partir de [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) ou de [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675) dans votre application. Cette clé représente l’identité de l’utilisateur dont vous souhaitez accéder aux informations de propriété.

## Revendications dans une clé d’ID du Windows Store


Une clé d’ID du Windows Store est une clé d’authentification web JSON (JWT) qui représente l’identité de l’utilisateur dont vous souhaitez accéder aux informations de propriété. Lorsqu’elle est décodée en Base64, une clé d’ID du Windows Store contient les revendications suivantes.

| Nom de la revendication                                                             | Description                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| iat                                                                    | Identifie l’heure à laquelle la clé a été émise. Cette revendication permet de déterminer l’âge du jeton. Cette valeur est exprimée en temps époque.                                                                                                                                                                                                                                       |
| iss                                                                    | Identifie l’émetteur. Cette revendication a la même valeur que la revendication *aud*.                                                                                                                                                                                                                                                                                                                      |
| aud                                                                    | Identifie l’audience. Doit prendre l’une des valeurs suivantes : **https://collections.mp.microsoft.com/v6.0/keys** ou **https://purchase.mp.microsoft.com/v6.0/keys**.                                                                                                                                                                                                                    |
| exp                                                                    | Identifie l’heure d’expiration à laquelle ou après laquelle la clé ne sera plus acceptée pour traiter quoique ce soit, excepté le renouvellement des clés. La valeur de cette revendication est exprimée en temps époque.                                                                                                                                                                                               |
| nbf                                                                    | Identifie l’heure à laquelle le jeton sera accepté pour le traitement. La valeur de cette revendication est exprimée en temps époque.                                                                                                                                                                                                                                                             |
| http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId   | ID client qui identifie le développeur.                                                                                                                                                                                                                                                                                                                                            |
| http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload    | Charge utile opaque (chiffrée et codée au format Base64) qui contient des informations destinées uniquement à être utilisées par les services du Windows Store.                                                                                                                                                                                                                                                     |
| http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId     | Identifiant utilisateur qui identifie l’utilisateur actuel dans le contexte de vos services. Il s’agit de la valeur que vous transmettez dans le paramètre facultatif *publisherUserId* de la méthode [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) ou [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675) lorsque vous créez la clé. |
| http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri | URI qui permet de renouveler la clé.                                                                                                                                                                                                                                                                                                                                              |

 

Voici un exemple d’en-tête de clé d’ID du Windows Store décodé.

```
{ 
    "typ":"JWT", 
    "alg":"RS256", 
    "x5t":"agA_pgJ7Twx_Ex2_rEeQ2o5fZ5g" 
} 
```

Voici un exemple de revendications de clé d’ID du Windows Store décodées.

```
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

## Rubriques connexes

* [Demander des produits](query-for-products.md)
* [Signaler le traitement de la commande d’un produit consommable](report-consumable-products-as-fulfilled.md)
* [Octroyer des produits gratuits](grant-free-products.md)
* [Renouveler une clé d’ID du Windows Store](renew-a-windows-store-id-key.md)
* [Intégration d’applications à Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722502)
* [Présentation du manifeste de l’application Azure Active Directory]( http://go.microsoft.com/fwlink/?LinkId=722500)
* [Jeton et types de réclamations pris en charge](http://go.microsoft.com/fwlink/?LinkId=722501)
 

 





<!--HONumber=Mar16_HO1-->


