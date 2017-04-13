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
keywords: "Windows10, uwp, personnalisé, application, promotion, campagne"
ms.openlocfilehash: 1e56b1df4b5501ded5db799c3ad67f97c0e1cfcd
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="create-a-custom-app-promotion-campaign"></a>Créer une campagne personnalisée de promotion d’applications

En plus de créer une [campagne de publicité pour votre application](create-an-ad-campaign-for-your-app.md) qui s'exécute dans des applications Windows, vous pouvez également promouvoir votre application à l'aide d'autres canaux. Par exemple, vous pouvez promouvoir votre application en faisant appel à un fournisseur de commercialisation d’applications tiers, ou en publiant des liens vers votre application sur des médias sociaux. Ces activités sont appelées *campagnes personnalisées*.

Si vous lancez des campagnes personnalisées pour votre application, vous pouvez suivre les performances de chacune d’entre elles en créant pour chaque campagne personnalisée une URL d’application du Windows Store contenant un ID de campagne différent. Lorsqu’un client exécutant Windows 10 clique sur une URL contenant un ID de campagne, Microsoft associe le clic à la campagne personnalisée correspondante et vous rend ces données disponibles.

Il existe deux types principaux de données associées à des campagnes personnalisées : les vues de page de votre application et les *conversions*. Une conversion est une acquisition d’application qui se produit suite à un clic dans la page du Windows Store sur une URL de votre application promue via une campagne personnalisée. Pour plus d’informations sur les conversions, voir la section [Comprendre comment les acquisitions d’application sont éligibles en tant que conversions](#understanding-how-app-acquisitions-qualify-as-conversions) dans cette rubrique.

Pour récupérer les données de performances de campagne personnalisée de votre application, procédez comme suit:

* Vous pouvez afficher les données sur les vues de page et les conversions de votre application ou de votre module complémentaire dans le [rapport Canaux et conversions](channels-and-conversions-report.md) accessible via le tableau de bord du Centre de développement.
* Si votre application est une application de plateforme Windows universelle (UWP), elle peut utiliser les API du SDK Windows pour récupérer par programme l’ID de campagne personnalisée qui a abouti à une conversion.

> **Important**&nbsp;&nbsp;Ces données ne font l’objet d’un suivi que pour les clients exécutant Windows10. Les clients utilisant d’autres systèmes d’exploitation peuvent suivre le lien vers la description de votre application, mais les données sur les activités de ces clients ne seront pas incluses.

## <a name="example-custom-campaign-scenario"></a>Exemple de scénario de campagne personnalisée

Imaginez un développeur de jeu venant d’achever la création d’un jeu et désireux de promouvoir celui-ci auprès des joueurs de ses jeux existants. Il publie une annonce relative au lancement du nouveau de jeu sur sa page Facebook, incluant un lien vers la page du Windows Store où le jeu est disponible. Bon nombre de ses joueurs le suivant sur Twitter, il publie également un tweet d’annonce contenant le lien vers la page du Windows Store où le jeu est disponible.

Pour suivre le succès de chacun de ces canaux de promotion, le développeur crée deux variantes de l’URL de la page du Windows Store correspondant au jeu:

* L’URL publiée sur sa page Facebook inclut l’ID de campagne personnalisée `my-facebook-campaign`.
* L’URL publiée sur Twitter inclut l’ID de campagne personnalisée `my-twitter-campaign`.

Lorsque ses abonnés sur Facebook et Twitter cliquent sur l’URL, Microsoft suit chaque clic et l’associe à la campagne personnalisée correspondante. Les acquisitions éligibles du jeu et achats de modules complémentaires sont associés à la campagne personnalisée et déclarés comme autant de conversions.

<span id="conversions" />
## <a name="understanding-how-app-acquisitions-qualify-as-conversions"></a>Comprendre comment les acquisitions d’application sont éligibles en tant que conversions

Une *conversion* est une acquisition d’application qui se produit suite à un clic dans la page du Windows Store sur une URL de votre application promue via une campagne personnalisée. Il existe différents cas de figure pour qu’une conversion apparaisse dans le [rapport Canaux et conversions](channels-and-conversions-report.md) sur le tableau de bord du Centre de développement, et pour qu’elle [récupère par programme l’ID de campagne](#programmatically).

### <a name="qualifying-conversions-for-the-dashboard-report"></a>Conversions éligibles du rapport du tableau de bord

Les scénarios suivants permettent de faire figurer une conversion dans le [rapport Canaux et conversions](channels-and-conversions-report.md):

* Un client *disposant ou non d’un compte Microsoft reconnu* clique sur une URL d’application contenant un ID de campagne personnalisée, puis est redirigé vers la page de l’application dans le WindowsStore. Le même client acquiert l’application dans les 24heures après avoir cliqué pour la première fois sur l’URL du Windows Store avec l’ID de campagne personnalisée.

* Si le client acquiert l’application sur un ordinateur ou un appareil autre que celui qu’il a utilisé pour cliquer sur l’URL du WindowsStore avec l’ID de campagne personnalisée, la conversion sera prise en compte uniquement s’il possède un compte Microsoft.

> **Remarque**&nbsp;&nbsp;Pour les acquisitions d’application comptant comme conversions dans le cadre d’une campagne personnalisée, les achats d’extension dans cette application sont également comptabilisés comme conversions pour la même campagne personnalisée.

### <a name="qualifying-conversions-for-programmatically-retrieving-the-campaign-id"></a>Conversions éligibles pour récupérer par programme l’ID de campagne

Pour être éligible en tant que conversion lors de la récupération par programme de l’ID de campagne associé à l’application, les conditions suivantes doivent être réunies:

* Sur un appareil exécutant **Windows10, versions1607 ou ultérieures**: un client *avec ou sans compte Microsoft reconnu* clique sur une URL d’application qui contient un ID de campagne personnalisée avant d’être redirigé vers la page de l’application dans le WindowsStore. Le client acquiert l’application pendant la vue de page du WindowsStore, après avoir cliqué sur l’URL.

* Sur un appareil exécutant **Windows10, versions1511 ou antérieures**: un client *avec ou sans compte Microsoft reconnu* clique sur une URL d’application qui contient un ID de campagne personnalisée avant d’être redirigé vers la page de l’application dans le WindowsStore. Le client acquiert l’application pendant la vue de page du Windows Store, après avoir cliqué sur l’URL. Sur ces versions de Windows10, l’utilisateur doit disposer d’un compte Microsoft reconnu pour que l’acquisition soit éligible en tant que conversion lors de la récupération par programme de l’ID de campagne.

>**Remarque**&nbsp;&nbsp;Si le client quitte la page, puis y retourne (soit sur le même ordinateur ou appareil, soit sur un autre ordinateur ou appareil s’il dispose d’un compte Microsoft) dans les 24heures et acquiert l’application, cette action figure comme conversion dans le [rapport Canaux et conversions](channels-and-conversions-report.md). Cela ne s'applique pas toutefois si vous récupérez par programme l’ID de campagne.

## <a name="embed-a-custom-campaign-id-to-your-apps-windows-store-page-url"></a>Incorporer un ID de campagne personnalisée dans l’URL de la page de votre application dans le Windows Store

Pour créer une URL de page du Windows Store pour votre application avec un ID de campagne personnalisée :

1.  Créez une chaîne d’ID pour la campagne personnalisée. Cette chaîne peut contenir jusqu’à 100caractères, même s’il est recommandé de définir un ID de campagne court facilement identifiable.

  >**Remarque**&nbsp;&nbsp;La chaîne d’ID de campagne peut être visible pour d’autres développeurs dans un rapport Canaux et conversions. Cela peut se produire lorsqu’un client clique sur votre ID de campagne pour entrer dans le Windows Store, mais qu'il finit par acheter l'application d’un autre développeur au cours de la même session. Même si votre ID de campagne peut être visible par un autre développeur dans ce cas de figure, ce développeur verra uniquement le nombre de conversions obtenues pour leur propre application après un clic initial sur votre ID de campagne. Il ne verra aucune information concernant le nombre d’utilisateurs ayant acheté votre application en cliquant sur votre ID de campagne.

2.  Obtenez l’URL de la page du WindowsStore correspondant à votre application au format HTML ou de protocole.

    * Utilisez le format HTTP si vous souhaitez que les clients accèdent à la page de votre application dans le Windows Store à l’aide d’un navigateur (si l’application Windows Store est installée, cette URL ouvre également celle-ci en affichant la description de votre application). Cette URL est au format **`https://www.microsoft.com/store/apps/*your app name*/*your app ID*`**. Par exemple, l’URL HTTP pour Skype est `https://www.microsoft.com/store/apps/skype/9wzdncrfj364`. L’URL au format HTML est disponible dans la [**page Identité de l’application** du tableau de bord du Centre de développement](link-to-your-app.md). Notez que les URL au format HTTP permettent d’accéder au WindowsStore à l’aide d’un navigateur sur les ordinateurs et tablettes exécutant Windows7 et versions ultérieures, et les téléphones Windows Phone8 et versions ultérieures.

    * Utilisez le format de protocole si vous promouvez votre application à partir d’autres applications Windows s’exécutant sur un appareil ou un ordinateur sur lequel l’application Windows Store est installée, et souhaitez que les clients accèdent à la page de votre application dans l’application Windows Store. Cette URL est au format **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**. Par exemple, l’URL de protocole pour Skype est `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`.

3.  Ajouter la chaîne suivante à la fin de l’URL de votre application:

    * Pour une URL au format HTTP, ajoutez **`?cid=*my custom campaign ID*`**. Par exemple, si Skype introduisait un ID de campagne avec la valeur **custom\_campaign**, la nouvelle URL HTTP incluant l’ID de campagne serait : `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`.

    * Pour une URL au format de protocole, ajoutez **`&cid=*my custom campaign ID*`**. Par exemple, si Skype introduisait un ID de campagne avec la valeur **custom\_campaign**, la nouvelle URL de protocole incluant l’ID de campagne serait : `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`

<span id="programmatically" />
## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>Récupérer par programme l’ID de campagne personnalisée pour une application

Si votre application est une application UWP, vous pouvez récupérer par programme l’ID de campagne personnalisée qui lui est associé en utilisant les API du SDK Windows. Ces API rendent possibles de nombreux scénarios d’analyse et de monétisation. Par exemple, vous pouvez déterminer si l’utilisateur actuel a acquis votre application après l’avoir découverte via votre campagne Facebook, puis personnaliser l’expérience de l’application en conséquence. Si vous utilisez un fournisseur de commercialisation d’applications tiers, vous pouvez également lui renvoyer des données.

Ces API retournent une chaîne d’ID de campagne uniquement si le client clique sur votre URL contenant l’ID de campagne, est redirigé vers la page de votre application dans le WindowsStore, puis acquiert votre application sans quitter cette page. Si l’utilisateur quitte la page, puis y revient pour acquérir l’application, cela n’est pas [éligible en tant que conversion](#conversions) lors de l’utilisation de ces API.

Vous pouvez utiliser différentes API en fonction de la version de Windows10 ciblée par votre application:

* Windows10, versions1607 ou ultérieures: utilisez la classe [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) dans l’espace de noms **Windows.Services.Store**. Lorsque vous utilisez cette API, vous pouvez récupérer les ID de campagne personnalisée pour les [acquisitions éligibles](#conversions), que l’utilisateur dispose ou non d’un compte Microsoft reconnu.
* Windows10, versions1511 ou antérieures: utilisez la classe [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx) dans l’espace de noms **Windows.ApplicationModel.Store**. Lorsque vous utilisez cette API, vous ne pouvez récupérer les ID de campagne personnalisée pour les [acquisitions éligibles](#conversions) que si l’utilisateur dispose d’un compte Microsoft reconnu.

>**Remarque**&nbsp;&nbsp;Même si l’espace de noms **Windows.ApplicationModel.Store** est disponible dans toutes les versions de Windows10, nous vous recommandons d’utiliser les API dans l’espace de noms **Windows.Services.Store** si votre application cible Windows10, versions1607 ou ultérieures. Pour plus d’informations sur les différences entre ces espaces de noms, voir [Achats dans l’application et versions d’évaluation](../monetize/in-app-purchases-and-trials.md#choose-namespace). L’exemple de code suivant montre comment structurer votre code pour utiliser les deux API dans le même projet.

### <a name="code-example"></a>Exemple de code

L’exemple de code suivant montre comment récupérer l’ID de campagne personnalisée. Cet exemple utilise les deuxensembles d’API dans les espaces de noms **Windows.Services.Store** et **Windows.ApplicationModel.Store** à l’aide du [code adaptatif de version](../debug-test-perf/version-adaptive-code.md). En suivant ce processus, votre code peut s’exécuter sur toute version de Windows10. Pour utiliser ce code, la version du système d’exploitation cible de votre projet doit être **Windows10 Édition anniversaire (version10.0, build14394)** ou une version ultérieure, bien que la version minimale du système d’exploitation puisse être une version antérieure.

``` csharp
// This example assumes the code file has using statements for
// System.Linq, System.Threading.Tasks, Windows.Data.Json,
// and Windows.Services.Store.
public async Task<string> GetCampaignId()
{
    // Use APIs in the Windows.Services.Store namespace if they are available
    // (the app is running on a device with Windows 10, version 1607, or later).
    if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent(
         "Windows.Services.Store.StoreContext"))
    {
        StoreContext context = StoreContext.GetDefault();

        // Try to get the campaign ID for users with a recognized Microsoft account.
        StoreProductResult result = await context.GetStoreProductForCurrentAppAsync();
        if (result.Product != null)
        {
            StoreSku sku = result.Product.Skus.FirstOrDefault(s => s.IsInUserCollection);

            if (sku != null)
            {
                return sku.CollectionData.CampaignId;
            }
        }

        // Try to get the campaign ID from the license data for users without a
        // recognized Microsoft account.
        StoreAppLicense license = await context.GetAppLicenseAsync();
        JsonObject json = JsonObject.Parse(license.ExtendedJsonData);
        if (json.ContainsKey("customPolicyField1"))
        {
            return json["customPolicyField1"].GetString();
        }

        // No campaign ID was found.
        return String.Empty;
    }
    // Fall back to using APIs in the Windows.ApplicationModel.Store namespace instead
    // (the app is running on a device with Windows 10, version 1577, or earlier).
    else
    {
#if DEBUG
        return await Windows.ApplicationModel.Store.CurrentAppSimulator.GetAppPurchaseCampaignIdAsync();
#else
        return await Windows.ApplicationModel.Store.CurrentApp.GetAppPurchaseCampaignIdAsync() ;
#endif
    }
}
```

Ce code procède aux opérations suivantes:

1. Tout d’abord, il vérifie si la classe [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) de l’espace de noms **Windows.Services.Store** est disponible sur l’appareil actuel (cela signifie que l’appareil exécute Windows10, versions1607 ou ultérieures). Si tel est le cas, le code utilise cette classe.

2. Il tente ensuite d’obtenir l’ID de campagne personnalisée pour le cas où l’utilisateur actuel dispose d’un compte Microsoft reconnu. Pour ce faire, le code obtient un objet [**StoreSku**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreSku) qui représente la référence de l’application actuelle, puis il accède à la propriété [**CampaignId**](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata#Windows_Services_Store_StoreCollectionData_CampaignId) afin de récupérer l’ID de campagne, le cas échéant.

2. Le code tente ensuite de récupérer l’ID de campagne pour le cas où l’utilisateur actuel ne dispose pas d’un compte Microsoft reconnu. Dans ce cas, l’ID de campagne est incorporé à la licence de l’application. Le code récupère la licence à l’aide de la méthode [**GetAppLicenseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_GetAppLicenseAsync), puis il analyse le contenu JSON de la licence et recherche la valeur d’une clé nommée *customPolicyField1*. Cette valeur contient l’ID de campagne.

3. Si la classe [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) de l’espace de noms **Windows.Services.Store** n’est pas disponible, le code a recours à la méthode [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.getapppurchasecampaignidasync.aspx) dans l’espace de noms **Windows.ApplicationModel.Store** pour récupérer l’ID de campagne personnalisée (cet espace de noms est disponible dans toutes les versions de Windows10, y compris les versions1511 et antérieures). Lorsque vous utilisez cette méthode, notez que vous ne pouvez récupérer les ID de campagne personnalisée pour les [acquisitions éligibles](#conversions) que si l’utilisateur dispose d’un compte Microsoft reconnu.

  >**Remarque**&nbsp;&nbsp;L’espace de noms **Windows.ApplicationModel.Store** inclut [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator), une classe spéciale qui simule les opérations du WindowsStore pour tester votre code avant que vous ne soumettiez votre application au WindowsStore. Cette classe récupère les données [d’un fichier local nommé Windows.StoreProxy.xml](../monetize/in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#using-the-windowsstoreproxyxml-file-with-currentappsimulator). L’exemple de code précédent montre comment inclure les classes **CurrentApp** et **CurrentAppSimulator** dans le code de débogage et de non-débogage de votre projet. Pour tester ce code dans un environnement de débogage, ajoutez un élément **AppPurchaseCampaignId** dans le fichier WindowsStoreProxy.xml sur votre ordinateur de développement, comme illustré dans l’exemple suivant. Lorsque vous exécutez l’application, la méthode [**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator#Windows_ApplicationModel_Store_CurrentAppSimulator_GetAppPurchaseCampaignIdAsync) retourne toujours cette valeur.

  ``` xml
  <CurrentApp>
      ...
      <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
  </CurrentApp>
  ```

  >**Remarque**&nbsp;&nbsp;L’espace de noms **Windows.Services.Store** ne fournit pas une classe que vous pouvez utiliser pour simuler des informations de licence lors d’un test. En revanche, vous devez publier une application dans le WindowsStore et télécharger cette application sur votre appareil de développement pour utiliser sa licence à des fins de test. Pour plus d’informations, voir [Achats dans l’application et versions d’évaluation](../monetize/in-app-purchases-and-trials.md#testing).

## <a name="test-your-custom-campaign"></a>Tester votre campagne personnalisée

Avant de promouvoir une URL de campagne personnalisée, nous vous recommandons de tester celle-ci en procédant comme suit :

1.  Connectez-vous à votre compte Microsoft à l’aide de l’ordinateur ou de l’appareil que vous utilisez pour le test.
2.  Cliquez sur l’URL de votre campagne personnalisée. Assurez-vous que le Windows Store charge correctement la page de votre application, puis fermez l’application Windows Store ou la page du navigateur.
3.  Cliquez sur l’URL plusieurs fois pour fermer l’application Windows Store ou la page du navigateur après chaque visite de la page de votre application. Lors de l’une de ces visites de la page de votre application, procédez à l’acquisition de votre application pour générer une conversion. Comptez le nombre total de fois où vous avez cliqué sur l’URL.
4. Vérifiez si les vues de page escomptées et les conversions s’affichent dans le [rapport Canaux et conversions](channels-and-conversions-report.md), puis testez le code de votre application pour vérifier s’il peut récupérer l’ID de campagne.
