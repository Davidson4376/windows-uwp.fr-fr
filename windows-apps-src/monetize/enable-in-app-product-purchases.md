---
Description: Que votre application soit gratuite ou non, vous pouvez vendre du contenu, d’autres applications ou de nouvelles fonctionnalités applicatives (par exemple le déverrouillage d’un nouveau niveau de jeu) directement dans l’application. Nous allons vous montrer comment activer ces produits dans votre application.
title: Activer les achats de produits dans l’application
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
mots-clés : offre dans l’application
mots-clés : achat dans l’application
mots-clés : produit in-app
mots-clés : comment prendre en charge les achats dans l’application
mots-clés : exemple de code d’un achat dans l’application
mots-clés : exemple de code d’une offre intégrée à l’application
---

# Activer les achats de produits dans l’application

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Que votre application soit gratuite ou non, vous pouvez vendre du contenu, d’autres applications ou de nouvelles fonctionnalités applicatives (par exemple le déverrouillage d’un nouveau niveau de jeu) directement dans l’application. Nous allons vous montrer comment activer ces produits dans votre application.

**Remarque** Les produits dans l’application ne peuvent pas être offerts à partir d’une version d’évaluation d’une application. Les clients qui utilisent une version d’évaluation de votre application peuvent uniquement acheter un produit in-app s’ils achètent une version complète de votre application.

## Prérequis

-   Application Windows dans laquelle ajouter des fonctionnalités que les clients peuvent acheter
-   Lorsque vous codez et testez de nouveaux produits in-app pour la première fois, vous devez utiliser l’objet [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) au lieu de l’objet [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765). Cela vous permet de vérifier votre logique de licence à l’aide d’appels simulés au serveur de licences au lieu d’appels au serveur Windows Live. Pour cela, vous devez personnaliser le fichier nommé « WindowsStoreProxy.xml » dans %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. Le simulateur Microsoft Visual Studio crée ce fichier quand vous exécutez votre application pour la première fois. Vous pouvez également charger un fichier personnalisé au moment de l’exécution. Pour plus d’informations, voir [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766).
-   Cette rubrique fait également référence à des exemples de code fournis dans l’[exemple du Windows Store](http://go.microsoft.com/fwlink/p/?LinkID=627610). Cet exemple représente un excellent moyen d’obtenir une expérience pratique avec les différentes options de monétisation fournies pour les applications UWP.

## Étape 1 : Initialisation des informations de licence de votre application

Lors de l’initialisation de votre application, obtenez l’objet [**LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/br225157) de votre application en initialisant l’élément [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) ou [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) pour activer les achats d’un produit in-app.

```CSharp
void AppInit()
{
    // some app initialization functions 

    // Get the license info
    // The next line is commented out for testing.
    // licenseInformation = CurrentApp.LicenseInformation;

    // The next line is commented out for production/release.       
    licenseInformation = CurrentAppSimulator.LicenseInformation;

    // other app initialization functions
}
```

## Étape 2 : Ajout des offres in-app à votre application

Pour chaque fonctionnalité que vous voulez rendre disponible par le biais d’un produit dans l’application, créez une offre et ajoutez-la à votre application.

**Important** Vous devez ajouter dans l’application tous les produits que vous voulez présenter à vos clients avant d’envoyer cette dernière au Windows Store. Pour ajouter ultérieurement de nouveaux produits dans l’application, vous devez mettre à jour votre application et soumettre à nouveau une nouvelle version.

1.  **Création d’un jeton d’offre dans l’application**

    Identifiez chaque produit dans l’application, via un jeton. Ce jeton est une chaîne que vous définissez et utilisez dans votre application, ainsi que dans le Windows Store, pour identifier un produit spécifique dans l’application. Donnez à votre jeton un nom unique et évocateur (dans l’application) afin de pouvoir rapidement identifier la fonctionnalité qu’il représente, lors de l’écriture du code. Voici quelques exemples de noms :

    -   « SpaceMissionLevel4 »
    -   « ContosoCloudSave »
    -   « RainbowThemePack »

2.  **Codage de la fonctionnalité dans un bloc conditionnel**

    Vous devez placer dans un bloc conditionnel le code de chaque fonctionnalité associée à un produit dans l’application. Ce bloc vérifie si le client possède une licence lui permettant d’utiliser cette fonctionnalité.

    Voici un exemple indiquant comment vous pouvez coder une fonctionnalité de produit nommée **featureName** dans le bloc conditionnel spécifique d’une licence. La chaîne **featureName** correspond au jeton qui identifie ce produit de manière unique au sein de l’application et qui permet de l’identifier dans le Windows Store.

    ```    CSharp
    if (licenseInformation.ProductLicenses["featureName"].IsActive) 
        {
            // the customer can access this feature
        } 
        else
        {
            // the customer can' t access this feature
        }
    ```

3.  **Ajout de l’interface utilisateur d’achat de cette fonctionnalité**

    Votre application doit également fournir à vos clients un moyen d’acheter le composant ou la fonctionnalité proposés par le produit dans l’application. En effet, ils ne peuvent pas les acheter par l’intermédiaire du Windows Store, de la même façon qu’ils ont acheté l’application complète.

    Voici comment vérifier si votre client possède déjà un produit dans l’application et, si tel n’est pas le cas, comment afficher la boîte de dialogue d’achat lui permettant de l’acheter. Remplacez le commentaire « Afficher la boîte de dialogue d’achat » par votre code personnalisé pour la boîte de dialogue d’achat (par exemple, une page présentant un bouton « Acheter cette application » convivial).

    ```    CSharp
    void BuyFeature1() {
            if (!licenseInformation.ProductLicenses["featureName"].IsActive)
            {
                try
                    {
                    // The customer doesn't own this feature, so 
                    // show the purchase dialog.
                                    
                    await CurrentAppSimulator.RequestProductPurchaseAsync("featureName", false);
                    //Check the license state to determine if the in-app purchase was successful.
                }
                catch (Exception)
                {
                    // The in-app purchase was not completed because 
                    // an error occurred.
                }
            } 
            else
            {
                // The customer already owns this feature.
            }
        }
    ```

## Étape 3 : Modification du code de test jusqu’aux appels finaux

C’est simple : remplacez chaque référence à [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) par une référence à [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) dans le code de votre application. Vous ne devez plus fournir le fichier WindowsStoreProxy.xml et vous pouvez par conséquent le supprimer du chemin d’accès de votre application (vous pourrez toutefois l’enregistrer pour référence lors de la configuration de l’offre in-app au cours de l’étape suivante).

## Étape 4 : Configuration dans le Windows Store de l’offre de produit in-app

Dans le tableau de bord du Centre de développement, définissez l’ID de produit, le type, le prix et les autres propriétés pour votre produit in-app. Veillez à effectuer les différents réglages en respectant la configuration définie dans le fichier WindowsStoreProxy.xml pendant le test. Pour plus d’informations, voir l’article [Soumissions d’IAP](https://msdn.microsoft.com/library/windows/apps/mt148551).

## Notes

Si vous envisagez de fournir à vos clients des options de produits in-app consommables (éléments pouvant être achetés, utilisés, puis rachetés si nécessaire), passez à la rubrique [Activer l’achat de produits in-app consommables](enable-consumable-in-app-product-purchases.md).

Si vous avez besoin de reçus pour vérifier que l’utilisateur a bien effectué un achat in-app, consultez la rubrique [Utiliser des reçus pour vérifier les achats de produits](use-receipts-to-verify-product-purchases.md).

## Rubriques connexes


* [Activer les achats de produits consommables in-app](enable-consumable-in-app-product-purchases.md)
* [Gérer un grand catalogue de produits in-app](manage-a-large-catalog-of-in-app-products.md)
* [Utiliser des reçus pour vérifier les achats de produits](use-receipts-to-verify-product-purchases.md)
* [Exemple du Windows Store (montre des versions d’évaluation et des achats in-app)](http://go.microsoft.com/fwlink/p/?LinkID=627610)
 

 






<!--HONumber=Mar16_HO1-->


