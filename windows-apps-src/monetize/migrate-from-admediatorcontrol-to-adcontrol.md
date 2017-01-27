---
author: mcleanbyron
ms.assetid: 9621641A-7462-425D-84CC-101877A738DA
description: "Découvrez comment migrer de la classe AdMediatorControl vers la classe AdControl dans vos applications UWP."
title: "Migration d’AdMediatorControl vers AdControl pour les applications UWP"
translationtype: Human Translation
ms.sourcegitcommit: 2b5dbf872dd7aad48373f6a6df3dffbcbaee8090
ms.openlocfilehash: 6e7e833327dce4b49e44b7485908c8a507b217ef

---

# <a name="migrate-from-admediatorcontrol-to-adcontrol-for-uwp-apps"></a>Migration d’AdMediatorControl vers AdControl pour les applications UWP

Les versions précédentes du Kit de développement logiciel (SDK) publicitaire Microsoft permettaient aux applications de la plateforme Windows universelle (UWP) d’afficher des bannières publicitaires à l’aide de la classe **AdMediatorControl**. Les développeurs pouvaient ainsi optimiser leurs revenus publicitaires en affichant des bannières publicitaires à partir de nos réseaux de partenaires (AOL et AppNexus), ainsi que d’AdDuplex. Le [SDK Microsoft Store Services](http://aka.ms/store-em-sdk) ne prend plus prend en charge la classe **AdMediatorControl**. Si vous avez une application qui utilise la classe **AdMediatorControl** d’une version précédente du SDK et que vous voulez migrer vers une application UWP qui utilise le [SDK Microsoft Store Services](http://aka.ms/store-em-sdk), suivez les instructions de cet article pour mettre à jour votre code de manière à utiliser la classe **AdControl** au lieu de la classe **AdMediatorControl**. Vous pouvez éventuellement configurer votre application pour qu’elle utilise AdDuplex pour la médiation publicitaire, selon une approche pondérée ou classée.

>**Remarque**&nbsp;&nbsp;Les exemples de code fournis dans cet article sont donnés uniquement à des fins d’illustration. Vous devrez peut-être effectuer des ajustements pour adapter ces exemples de code à votre application.

## <a name="prerequisites"></a>Prérequis

* Une application UWP qui utilise actuellement la classe AdMediatorControl et qui est publiée dans le Windows Store.
* Un ordinateur de développement avec Visual Studio 2015 et le [SDK Microsoft Store Services](http://aka.ms/store-em-sdk).
* Si vous souhaitez utiliser AdDuplex pour la médiation publicitaire, vous devez également disposer du [SDK AdDuplex Windows 10](https://visualstudiogallery.msdn.microsoft.com/6930860a-e64b-4b46-9d72-62d7fddda077) sur votre ordinateur de développement.

  >**Remarque**&nbsp;&nbsp;Au lieu d’exécuter le programme d’installation du SDK AdDuplex à partir du lien ci-dessus, vous pouvez installer les bibliothèques AdDuplex pour votre projet d’application UWP dans Visual Studio 2015. Ouvrez votre projet d’application UWP dans Visual Studio 2015, puis cliquez sur **Projet** > **Gérer les packages NuGet**. Recherchez le package NuGet nommé **AdDuplexWin10**, puis installez-le.

## <a name="step-1-retrieve-your-application-ids-and-ad-unit-ids"></a>Étape 1 : récupérer vos ID d’application et d’unité publicitaire

Lorsque vous migrez votre code pour pouvoir utiliser la classe **AdControl**, vous devez connaître vos ID d’application et vos ID d’unité publicitaire. La meilleure façon d’obtenir les ID les plus récents consiste à les récupérer à partir de votre fichier de configuration de médiation.

1. Connectez-vous au tableau de bord du centre de développement Windows et cliquez sur l’application qui utilise actuellement **AdMediatorControl**.
2. Cliquez sur **Monétisation**, puis sur **Monétiser avec des publicités**.
3. Dans la section **Médiation publicitaire Windows**, cliquez sur le lien **Télécharger la configuration de médiation** et ouvrez le fichier AdMediator.config dans un éditeur de texte, par exemple le Bloc-notes.
4. Dans le fichier, repérez l’élément ```<AdAdapterInfo>``` contenant l’élément enfant ```<Name>MicrosoftAdvertising</Name>```. Cette section contient la configuration des publicités payantes Microsoft.
5. Dans cet élément ```<AdAdapterInfo>```, localisez l’élément ```<Property>``` qui contient les éléments ```<Key>``` ayant comme valeurs **WApplicationId** et **WAdUnitId**. Dans l’exemple ci-dessous, les valeurs des éléments ```<Value>``` ne sont données qu’à titre d’exemple.

  > [!div class="tabbedCodeSnippets"]
  ```xml
  <Metadata>
      <Property>
          <Key>WApplicationId</Key>
          <Value>364d4938-c0f5-4c3d-8aae-090206211dc9</Value>
      </Property>
      <Property>
          <Key>WAdUnitId</Key>
          <Value>301568</Value>
      </Property>
  </Metadata>
  ```

6. Copiez les deux valeurs dans ces éléments ```<Value>``` pour pouvoir les utiliser ultérieurement. Ces valeurs contiennent l’ID d’application et l’ID d’unité publicitaire pour les annonces non mobiles correspondant aux publicités payantes Microsoft.
5. Dans le même élément ```<AdAdapterInfo>```, localisez les éléments ```<Property>``` qui contiennent les éléments ```<Key>``` associés aux valeurs **MApplicationId** et **MAdUnitId**. Dans l’exemple ci-dessous, les valeurs des éléments ```<Value>``` ne sont données qu’à titre d’exemple.

  > [!div class="tabbedCodeSnippets"]
  ```xml
  <Metadata>
      <Property>
          <Key>MApplicationId</Key>
          <Value>e2cf8388-7018-4a11-8ab0de90f2a7a401</Value>
      </Property>
      <Property>
          <Key>MAdUnitId</Key>
          <Value>301056</Value>
      </Property>
  </Metadata>
  ```

6. Copiez les deux valeurs dans les éléments ```<Value>``` pour pouvoir les utiliser ultérieurement. Ces valeurs contiennent l’ID d’application et l’ID d’unité publicitaire pour les annonces mobiles correspondant aux publicités payantes Microsoft.
7. Si vous utilisez des [publicités maison](../publish/about-house-ads.md), repérez l’élément ```<AdAdapterInfo>``` contenant l’élément enfant ```<Name>MicrosoftAdvertisingHouse</Name>```. Dans cet élément, recherchez les éléments ```<Key>``` associés aux valeurs **MAdUnitId** et **WAdUnitId**, puis enregistrez les valeurs des éléments ```<Value>``` correspondants pour pouvoir les utiliser ultérieurement. Ces valeurs correspondent respectivement aux ID d’annonces mobiles et non mobiles pour les publicités maison Microsoft.
8. Si vous utilisez AdDuplex, recherchez l’élément ```<AdAdapterInfo>``` contenant l’élément enfant ```<Name>AdDuplex</Name>```. Dans cet élément, recherchez les éléments ```<Key>``` associés aux valeurs **AppKey** et **AdUnitId**, puis enregistrez les valeurs des éléments ```<Value>``` correspondants pour pouvoir les utiliser ultérieurement. Ces valeurs correspondent respectivement à votre clé d’application AdDuplex et à votre ID d’unité publicitaire.

## <a name="step-2-update-your-app-code"></a>Étape 2 : mettre à jour votre code d’application

Maintenant que vous connaissez votre ID d’application et votre ID d’unité publicitaire, vous êtes prêt à mettre à jour le code de votre application pour utiliser la classe **AdControl** au lieu de la classe **AdMediatorControl**.

### <a name="microsoft-paid-ads-only"></a>Publicités payantes Microsoft uniquement

Si vous utilisez uniquement des publicités payantes Microsoft dans votre configuration de médiation publicitaire, procédez comme suit.

  >**Remarque**&nbsp;&nbsp;Ces étapes supposent que la page d’application sur laquelle vous voulez afficher des publicités contient une grille vide nommée **myAdGrid**, par exemple : ```<Grid x:Name="myAdGrid"/>```. Dans ces étapes, vous allez créer et configurer les contrôles publicitaires entièrement dans votre code, plutôt qu’avec XAML.

1. Ouvrez votre application UWP dans Visual Studio.
2.  Dans la fenêtre **Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence...**.
Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour XAML** (version 10.0).
3. Dans **Gestionnaire de références**, cliquez sur OK.
4. Supprimez la déclaration **AdMediatorControl** de votre code XAML et supprimez le code qui utilise cet objet **AdMediatorControl**, y compris tous les gestionnaires d’événements associés.
5. Ouvrez le fichier de code de l’application **Page** sur laquelle vous voulez afficher des publicités.
6. Le cas échéant, ajoutez l’instruction suivante en haut du fichier de code.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[TrialVersion](./code/AdvertisingSamples/MigrateToAdControl/cs/MainPage.xaml.cs#Snippet1)]

7. Ajoutez les déclarations de constantes suivantes à votre classe **Page**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[TrialVersion](./code/AdvertisingSamples/MigrateToAdControl/cs/MainPage.xaml.cs#Snippet2)]

7. Pour chacune de ces déclarations de constantes, remplacez les valeurs comme indiqué ci-dessous :

  * **AD_WIDTH** et **AD_HEIGHT** : assignez ces éléments à l’une des [tailles de bannières publicitaires prises en charge]( https://msdn.microsoft.com/windows/uwp/monetize/supported-ad-sizes-for-banner-ads).
  * **WAPPLICATIONID** et **WADUNITID** : affectez ces éléments aux valeurs **WApplicationId** et **WAdUnitId** pour les publicités payantes Microsoft que vous avez récupérées précédemment à partir du fichier de configuration de la médiation (ces valeurs s’appliquent à l’unité publicitaire non mobile correspondant aux publicités payantes).
  * **MAPPLICATIONID** et **MADUNITID** : affectez ces éléments aux valeurs **MApplicationId** et **MAdUnitId** pour les publicités payantes Microsoft que vous avez récupérées précédemment à partir du fichier de configuration de la médiation (ces valeurs s’appliquent à l’unité publicitaire mobile correspondant aux publicités payantes).

8. Ajoutez les déclarations de variables suivantes à votre classe **Page**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/MainPage.xaml.cs#Snippet3)]

5. Ajoutez le code suivant à votre constructeur de classe **Page**, après l’appel à la méthode **InitializeComponent()**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/MainPage.xaml.cs#Snippet4)]

### <a name="microsoft-paid-ads-house-ads-and-adduplex"></a>Publicités payantes Microsoft, publicités maison et AdDuplex

Si vous utilisez les publicités maison Microsoft ou AdDuplex, ou encore les publicités payantes Microsoft, et que vous souhaitez continuer d’utiliser AdDuplex pour votre médiation publicitaire, suivez les étapes décrites dans cette section. Les exemples de code prennent en charge aussi bien AdDuplex que les publicités maison Microsoft. Si vous utilisez AdDuplex mais pas les publicités maison Microsoft (ou inversement), supprimez le code qui ne s’applique pas à votre scénario.

  >**Remarque**&nbsp;&nbsp;Ces étapes supposent que la page d’application sur laquelle vous voulez afficher des publicités contient une grille vide nommée **myAdGrid**, par exemple : ```<Grid x:Name="myAdGrid"/>```. Dans ces étapes, vous allez créer et configurer les contrôles publicitaires entièrement dans votre code, plutôt qu’avec XAML.

1. Ouvrez votre application UWP dans Visual Studio.
2.  Dans la fenêtre **Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence...**.
Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour XAML** (version 10.0).
3. Dans **Gestionnaire de références**, cliquez sur OK.
4. Supprimez la déclaration **AdMediatorControl** de votre code XAML et supprimez le code qui utilise cet objet **AdMediatorControl**, y compris tous les gestionnaires d’événements associés.
5. Ouvrez le fichier de code de l’application **Page** sur laquelle vous voulez afficher des publicités.
6. Le cas échéant, ajoutez les instructions suivantes en haut du fichier de code.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/ExamplePage1.xaml.cs#Snippet1)]

7. Ajoutez les déclarations de constantes suivantes à votre classe **Page**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/ExamplePage1.xaml.cs#Snippet2)]

4. Pour ces déclarations de constantes, remplacez les valeurs comme indiqué ci-dessous :

  * **AD_WIDTH** et **AD_HEIGHT** : assignez ces éléments à l’une des [tailles de bannières publicitaires prises en charge]( https://msdn.microsoft.com/windows/uwp/monetize/supported-ad-sizes-for-banner-ads).
  * **HOUSE_AD_WEIGHT** : affectez cet élément à un entier compris entre 0 et 100 qui spécifie la valeur de poids que vous voulez appliquer aux publicités maison Microsoft par rapport aux publicités payantes Microsoft (où 0 indique que les publicités maison ne doivent jamais être affichées, et 100 que les publicités maison doivent toujours être affichées).
  * **WAPPLICATIONID** et **WADUNITID_PAID** : affectez ces éléments aux valeurs **WApplicationId** et **WAdUnitId** pour les publicités payantes Microsoft que vous avez récupérées précédemment à partir du fichier de configuration de la médiation (ces valeurs s’appliquent à l’unité publicitaire non mobile correspondant aux publicités payantes).
  * **WADUNITID_HOUSE** : affectez cet élément à la valeur **WAdUnitId** correspondant aux publicités maison que vous avez récupérées précédemment à partir du fichier de configuration de la médiation (cette valeur s’applique à l’unité publicitaire non mobile correspondant aux publicités maison).
  * **MAPPLICATIONID** et **MADUNITID_PAID** : affectez ces éléments aux valeurs **MApplicationId** et **MAdUnitId** pour les publicités payantes Microsoft que vous avez récupérées précédemment à partir du fichier de configuration de la médiation (ces valeurs s’appliquent à l’unité publicitaire mobile correspondant aux publicités payantes).
  * **MADUNITID_HOUSE** : affectez cet élément à la valeur **MAdUnitId** correspondant aux publicités maison que vous avez récupérées précédemment à partir du fichier de configuration de la médiation (cette valeur s’applique à l’unité publicitaire mobile correspondant aux publicités maison).
  * **ADDUPLEX_APPKEY** et **ADDUPLEX_ADUNIT** : assignez ces éléments aux valeurs de clé d’application AdDuplex et d’ID d’unité publicitaire que vous avez récupérées à partir du fichier de configuration de la médiation.

  >**Remarque**&nbsp;&nbsp;Ne modifiez pas les valeurs **AD_REFRESH_SECONDS** et **MAX_ERRORS_PER_REFRESH** affichées dans l’exemple précédent.

5. Ajoutez les déclarations de variables suivantes à votre classe **Page**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/ExamplePage1.xaml.cs#Snippet3)]

5. Ajoutez le code suivant à votre constructeur de classe **Page**, après l’appel à la méthode **InitializeComponent()**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/ExamplePage1.xaml.cs#Snippet4)]

6. Pour finir, ajoutez les méthodes suivantes à votre classe **Page**. Ces méthodes instancient les objets **AdControl** Microsoft et **AdControl** AdDuplex, et utilisent un générateur de nombres aléatoire avec les valeurs de poids données pour actualiser les bannières publicitaires dans ces contrôles à intervalles réguliers.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/ExamplePage1.xaml.cs#Snippet5)]



<!--HONumber=Dec16_HO2-->


