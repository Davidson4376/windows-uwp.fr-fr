---
author: shawjohn
Description: "En plus de créer une campagne de publicité pour votre application qui s’exécute dans des applications Windows, vous pouvez également promouvoir votre application à l’aide d’autres canaux."
title: "Créer une campagne personnalisée de promotion d’applications"
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, personnalisé, application, promotion, campagne"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 7920e2ba2fd4222ba012a98751e35133b36ac334
ms.lasthandoff: 02/07/2017

---

# <a name="create-a-custom-app-promotion-campaign"></a>Créer une campagne personnalisée de promotion d’applications



En plus de créer une [campagne de publicité pour votre application](create-an-ad-campaign-for-your-app.md) qui s'exécute dans des applications Windows, vous pouvez également promouvoir votre application à l'aide d'autres canaux. Par exemple, vous pouvez promouvoir votre application en faisant appel à un fournisseur de commercialisation d’applications tiers, ou en publiant des liens vers votre application sur des médias sociaux. Ces activités sont appelées *campagnes personnalisées*.

Si vous lancez des campagnes personnalisées pour votre application, vous pouvez suivre les performances de chacune d’entre elles en créant pour chaque campagne personnalisée une URL d’application du Windows Store contenant un ID de campagne différent. Lorsqu’un client exécutant Windows 10 clique sur une URL contenant un ID de campagne, Microsoft associe le clic à la campagne personnalisée correspondante et vous rend ces données disponibles.

Il existe deux types principaux de données associées à des campagnes personnalisées : les vues de page de votre application et les *conversions*. Une conversion est une acquisition d’application qui se produit suite à un clic dans la page du Windows Store sur une URL de votre application promue via une campagne personnalisée. Pour plus d’informations sur les conversions, voir la section [Comprendre comment les acquisitions d’application sont éligibles en tant que conversions](#understanding-how-app-acquisitions-qualify-as-conversions) dans cette rubrique.

Pour récupérer les données de performances de campagne personnalisée de votre application, procédez comme suit :

-   Si votre application est une application de plateforme Windows universelle (UWP), elle peut utiliser la méthode [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) pour récupérer par programme l’ID de campagne personnalisée qui a abouti à une conversion.
-   Vous pouvez afficher les données sur les vues de page et les conversions de votre application ou de votre module complémentaire dans le [rapport Canaux et conversions](channels-and-conversions-report.md) accessible via le tableau de bord du Centre de développement.

> **Important** Ces données ne font l’objet d’un suivi que pour les clients exécutant Windows 10. Les clients utilisant d’autres systèmes d’exploitation peuvent suivre le lien vers la description de votre application, mais les données sur les activités de ces clients ne seront pas incluses.

 

## <a name="example-custom-campaign-scenario"></a>Exemple de scénario de campagne personnalisée


Imaginez un développeur de jeu venant d’achever la création d’un jeu et désireux de promouvoir celui-ci auprès des joueurs de ses jeux existants. Il publie une annonce relative au lancement du nouveau de jeu sur sa page Facebook, incluant un lien vers la page du Windows Store où le jeu est disponible. Bon nombre de ses joueurs le suivant sur Twitter, il publie également un tweet d’annonce contenant le lien vers la page du Windows Store où le jeu est disponible.

Pour suivre le succès de chacun de ces canaux de promotion, le développeur crée deux variantes de l’URL de la page du Windows Store correspondant au jeu :

-   L’URL publiée sur sa page Facebook inclut l’ID de campagne personnalisée `my-facebook-campaign`.
-   L’URL publiée sur Twitter inclut l’ID de campagne personnalisée `my-twitter-campaign`.

Lorsque ses abonnés sur Facebook et Twitter cliquent sur l’URL, Microsoft suit chaque clic et l’associe à la campagne personnalisée correspondante. Les acquisitions éligibles du jeu et achats de modules complémentaires sont associés à la campagne personnalisée et déclarés comme autant de conversions.

## <a name="understanding-how-app-acquisitions-qualify-as-conversions"></a>Comprendre comment les acquisitions d’application sont éligibles en tant que conversions


Une *conversion* est une acquisition d’application qui se produit suite à un clic dans la page du Windows Store sur une URL de votre application promue via une campagne personnalisée. Il existe différents cas de figure pour qu’une conversion apparaisse dans le [rapport Canaux et conversions](channels-and-conversions-report.md) sur le tableau de bord du Centre de développement, et pour qu’elle [récupère par programme de l’ID de campagne](#programmatically).

Les scénarios suivants permettent de faire figurer une conversion dans le [rapport Canaux et conversions](channels-and-conversions-report.md) :

-   Un client disposant ou non d’un compte Microsoft reconnu clique sur une URL d’application contenant un ID de campagne personnalisée, et est redirigé vers la page de l’application dans le Windows Store. Le même client acquiert l’application dans les 24 heures après avoir cliqué pour la première fois sur l’URL du Windows Store avec l’ID de campagne personnalisée.
-   Si le client acquiert l’application sur un ordinateur ou un appareil autre que celui qu'il a utilisé pour cliquer sur l’URL du Windows Store avec l’ID de campagne personnalisée, la conversion sera uniquement être prise en compte si le client possède un compte Microsoft reconnu.

> **Remarque** Pour les acquisitions d’application comptant comme conversions dans le cadre d’une campagne personnalisée, les achats de module complémentaire dans cette application sont également comptabilisés comme conversions pour la même campagne personnalisée.
     

Pour être éligible en tant que conversion lors de la récupération par programme de l’ID de campagne associé à l’application, les conditions suivantes doivent être réunies :

-   Un client disposant ou non d’un compte Microsoft reconnu clique sur une URL d’application contenant un ID de campagne personnalisée, et est redirigé vers la page de l’application dans le Windows Store. Le client acquiert l’application pendant la vue de page du Windows Store, après avoir cliqué sur l’URL.
-   Si le client quitte la page puis y retourne (soit sur le même ordinateur ou appareil, soit sur un autre ordinateur ou appareil s’il dispose d’un compte Microsoft reconnu) dans les 24 heures et acquiert l’application, cette action figure comme une conversion sur le [rapport Canaux et conversions](channels-and-conversions-report.md). Cela ne s'applique pas toutefois si vous récupérez par programme l’ID de campagne.

## <a name="embed-a-custom-campaign-id-to-your-apps-windows-store-page-url"></a>Incorporer un ID de campagne personnalisée dans l’URL de la page de votre application dans le Windows Store


Pour créer une URL de page du Windows Store pour votre application avec un ID de campagne personnalisée :

1.  Créez une chaîne d’ID pour la campagne personnalisée. Cette chaîne peut contenir jusqu’à 100 caractères, même s’il est recommandé de définir un ID de campagne court facilement identifiable. Notez que la chaîne d’ID de campagne peut être visible à d’autres développeurs dans un rapport Canaux et conversions. Cela peut se produire lorsqu’un client clique sur votre ID de campagne pour entrer dans le Windows Store, mais qu'il finit par acheter l'application d’un autre développeur au cours de la même session. Même si votre ID de campagne peut être visible par un autre développeur dans ce cas de figure, ce développeur verra uniquement le nombre de conversions obtenues pour leur propre application après un clic initial sur votre ID de campagne. Il ne verra aucune information concernant le nombre d’utilisateurs ayant acheté votre application en cliquant sur votre ID de campagne.
2.  Obtenez l’URL de la page du Windows Store pour votre application au format HTML ou de protocole. L’URL au format HTML est disponible dans la page [**Identité de l’application** du tableau de bord du Centre de développement](link-to-your-app.md).
    -   Utilisez le format HTTP si vous souhaitez que les clients accèdent à la page de votre application dans le Windows Store à l’aide d’un navigateur (si l’application Windows Store est installée, cette URL ouvre également celle-ci en affichant la description de votre application). Cette URL est au format **`https://www.microsoft.com/store/apps/*your app name*/*your app ID*`**. Par exemple, l’URL HTTP pour Skype est `https://www.microsoft.com/store/apps/skype/9wzdncrfj364`. Notez que les URL au format HTTP permettent d’accéder au Windows Store à l’aide d’un navigateur sur les ordinateurs et tablettes exécutant Windows 7 et versions ultérieures, et les téléphones Windows Phone 8 et versions ultérieures.
    -   Utilisez le format de protocole si vous promouvez votre application à partir d’autres applications Windows s’exécutant sur un appareil ou un ordinateur sur lequel l’application Windows Store est installée, et souhaitez que les clients accèdent à la page de votre application dans l’application Windows Store. Cette URL est au format **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**. Par exemple, l’URL de protocole pour Skype est `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`.
3.  Ajouter la chaîne suivante à la fin de l’URL de votre application :
    -   Pour une URL au format HTTP, ajoutez **`?cid=*my custom campaign ID*`**. Par exemple, si Skype introduisait un ID de campagne avec la valeur **custom\_campaign**, la nouvelle URL HTTP incluant l’ID de campagne serait : `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`.
    -   Pour une URL au format de protocole, ajoutez **`&cid=*my custom campaign ID*`**. Par exemple, si Skype introduisait un ID de campagne avec la valeur **custom\_campaign**, la nouvelle URL de protocole incluant l’ID de campagne serait : `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`

## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>Récupérer par programme l’ID de campagne personnalisée pour une application


Si votre application est une application pour UWP, vous pouvez récupérer par programme l’ID de campagne personnalisée associé à votre application en utilisant la méthode [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445). Cette méthode rend possibles de nombreux scénarios d’analyse et de monétisation. Par exemple, vous pouvez déterminer si l’utilisateur actuel a acquis votre application après l’avoir découverte via votre campagne Facebook, puis personnaliser l’expérience de l’application en conséquence. En guise d’alternative, si vous utilisez un fournisseur de commercialisation d’applications tiers, vous pouvez lui renvoyer des données.

> **Remarque** La méthode [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) retourne une chaîne d’ID de campagne uniquement si le client clique sur votre URL contenant l’ID de campagne, est redirigé vers la page de votre application dans le Windows Store, puis acquiert votre application sans quitter cette page. Si l’utilisateur quitte la page, puis y revient pour acquérir l’application, cela n’est pas éligible en tant que conversion en cas d’utilisation de **GetAppPurchaseCampaignIdAsync**. Pour plus d’informations, voir la section [Comprendre les conversions](#conversions) dans cette rubrique.

 

L’exemple suivant montre comment utiliser la méthode [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) pour récupérer l’ID de campagne personnalisée pour l’application. Si l’application n’est pas acquise dans le cadre d’une conversion réussie, cette méthode retourne une chaîne vide.

``` csharp
string campaignId = await CurrentApp.GetAppPurchaseCampaignIdAsync();
```

``` cpp
HString campaignId;
HRESULT hr = CurrentApp::GetAppPurchaseCampaignIdAsync(campaignId.GetAddressOf());
```

La méthode [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) accède à des données du Windows Store. Lorsque vous utilisez cette méthode, suivez les recommandations suivantes :

-   Pour permettre l’exécution de cet appel de méthode, encapsulez celui-ci dans une opération asynchrone.
-   Si votre application n’est pas encore publiée dans le Windows Store lorsque vous testez votre campagne personnalisée, utilisez la méthode [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) de la classe [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) au lieu de la classe [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765). Procédez comme suit :
    -   Ajoutez un élément **AppPurchaseCampaignId** dans le fichier WindowsStoreProxy.xml, comme illustré dans l’exemple suivant. Affectez la valeur de l’élément à l’ID de la campagne personnalisée. Lorsque vous exécutez l’application, la méthode [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) retourne toujours cette valeur. Pour plus d’informations sur le fichier WindowsStoreProxy.xml, voir la documentation de [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766).

    ```        XML
    <CurrentApp>
        ....
        <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
    </CurrentApp>
    ```

    -   Pour basculer facilement de l’utilisation de [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) à celle de [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766), nous vous recommandons d’ajouter les instructions suivantes à votre code afin de définir un alias **Store**. Ensuite, qualifiez vos appels [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) avec l’alias **Store**.

    ```        CSharp
    #if DEBUG
            using Store = Windows.ApplicationMode.Store.CurrentAppSimulator;
            #else
            using Store = Windows.ApplicationMode.Store.CurrentApp;
            #endif   
    ```

## <a name="test-your-custom-campaign"></a>Tester votre campagne personnalisée


Avant de promouvoir une URL de campagne personnalisée, nous vous recommandons de tester celle-ci en procédant comme suit :

1.  Connectez-vous à votre compte Microsoft à l’aide de l’ordinateur ou de l’appareil que vous utilisez pour le test.
2.  Cliquez sur l’URL de votre campagne personnalisée. Assurez-vous que le Windows Store charge correctement la page de votre application, puis fermez l’application Windows Store ou la page du navigateur.
3.  Cliquez sur l’URL plusieurs fois pour fermer l’application Windows Store ou la page du navigateur après chaque visite de la page de votre application. Lors de l’une de ces visites de la page de votre application, procédez à l'acquisition de votre application pour générer une conversion. Comptez le nombre total de fois où vous avez cliqué sur l’URL.
4.  Si vous récupérez par programme l’ID de campagne personnalisée dans votre application, testez cette opération à l’aide de la méthode [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) de la classe [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) au lieu de la classe [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765).

 

 

