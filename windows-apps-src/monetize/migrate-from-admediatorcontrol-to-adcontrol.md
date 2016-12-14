---
author: mcleanbyron
ms.assetid: 9621641A-7462-425D-84CC-101877A738DA
description: "Découvrez comment migrer de la classe AdMediatorControl vers la classe AdControl dans vos applications UWP."
title: "Migration d’AdMediatorControl vers AdControl pour les applications UWP"
translationtype: Human Translation
ms.sourcegitcommit: 07baa54990ec31dc0cb9c289f9f0222754da9d7c
ms.openlocfilehash: 3abef943530cc756de117edccc5ab16e5f178604

---

# Migration d’AdMediatorControl vers AdControl pour les applications UWP

Les versions précédentes du Kit de développement logiciel (SDK) publicitaire Microsoft permettaient aux applications de la plateforme Windows universelle (UWP) d’afficher des bannières publicitaires à l’aide de la classe **AdMediatorControl**. Les développeurs pouvaient ainsi optimiser leurs revenus publicitaires en affichant des bannières publicitaires à partir de nos réseaux de partenaires (AOL et AppNexus), ainsi que d’AdDuplex. Le [SDK Microsoft Store Services](http://aka.ms/store-em-sdk) ne prend plus prend en charge la classe **AdMediatorControl**. Si vous avez une application qui utilise la classe **AdMediatorControl** d’une version précédente du SDK et que vous voulez migrer vers une application UWP qui utilise le [SDK Microsoft Store Services](http://aka.ms/store-em-sdk), suivez les instructions de cet article pour mettre à jour votre code de manière à utiliser la classe **AdControl** au lieu de la classe **AdMediatorControl**. Vous pouvez éventuellement configurer votre application pour qu’elle utilise AdDuplex pour la médiation publicitaire, selon une approche pondérée ou classée.

>**Remarque**  Les exemples de code fournis dans cet article sont donnés uniquement à des fins d’illustration. Vous devrez peut-être effectuer des ajustements pour adapter ces exemples de code à votre application.

## Prérequis

* Une application UWP qui utilise actuellement la classe AdMediatorControl et qui est publiée dans le Windows Store.
* Un ordinateur de développement avec Visual Studio 2015 et le [SDK Microsoft Store Services](http://aka.ms/store-em-sdk).
* Si vous souhaitez utiliser AdDuplex pour la médiation publicitaire, vous devez également disposer du [SDK AdDuplex Windows 10](https://visualstudiogallery.msdn.microsoft.com/6930860a-e64b-4b46-9d72-62d7fddda077) sur votre ordinateur de développement.

  >**Remarque**  Au lieu d’exécuter le programme d’installation du SDK AdDuplex à partir du lien ci-dessus, vous pouvez installer les bibliothèques AdDuplex pour votre projet d’application UWP dans Visual Studio 2015. Ouvrez votre projet d’application UWP dans Visual Studio 2015, puis cliquez sur **Projet** > **Gérer les packages NuGet**. Recherchez le package NuGet nommé **AdDuplexWin10**, puis installez-le.

## Étape 1 : récupérer vos ID d’application et d’unité publicitaire

Lorsque vous migrez votre code pour pouvoir utiliser la classe **AdControl**, vous devez connaître vos ID d’application et vos ID d’unité publicitaire. La meilleure façon d’obtenir les ID les plus récents consiste à les récupérer à partir de votre fichier de configuration de médiation.

1. Connectez-vous au tableau de bord du centre de développement Windows et cliquez sur l’application qui utilise actuellement **AdMediatorControl**.
2. Cliquez sur **Monétisation**, puis sur **Monétiser avec des publicités**.
3. Dans la section **Médiation publicitaire Windows**, cliquez sur le lien **Télécharger la configuration de médiation** et ouvrez le fichier AdMediator.config dans un éditeur de texte, par exemple le Bloc-notes.
4. Dans le fichier, repérez l’élément ```<AdAdapterInfo>``` contenant l’élément enfant ```<Name>MicrosoftAdvertising</Name>```. Cette section contient la configuration des publicités payantes Microsoft.
5. Dans cet élément ```<AdAdapterInfo>```, localisez l’élément ```<Property>``` qui contient les éléments ```<Key>``` ayant comme valeurs **WApplicationId** et **WAdUnitId**. Dans l’exemple ci-dessous, les valeurs des éléments ```<Value>``` ne sont données qu’à titre d’exemple.

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

## Étape 2 : mettre à jour votre code d’application

Maintenant que vous connaissez votre ID d’application et votre ID d’unité publicitaire, vous êtes prêt à mettre à jour le code de votre application pour utiliser la classe **AdControl** au lieu de la classe **AdMediatorControl**.

### Publicités payantes Microsoft uniquement

Si vous utilisez uniquement des publicités payantes Microsoft dans votre configuration de médiation publicitaire, procédez comme suit.

  >**Remarque**  Ces étapes supposent que la page d’application sur laquelle vous voulez afficher des publicités contient une grille vide nommée **myAdGrid**, par exemple : ```<Grid x:Name="myAdGrid"/>```. Dans ces étapes, vous allez créer et configurer les contrôles publicitaires entièrement dans votre code, plutôt qu’avec XAML.

1. Ouvrez votre application UWP dans Visual Studio.
2.  Dans la fenêtre **Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence...**.
Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour XAML** (version 10.0).
3. Dans **Gestionnaire de références**, cliquez sur OK.
4. Supprimez la déclaration **AdMediatorControl** de votre code XAML et supprimez le code qui utilise cet objet **AdMediatorControl**, y compris tous les gestionnaires d’événements associés.
5. Ouvrez le fichier de code de l’application **Page** sur laquelle vous voulez afficher des publicités.
6. Ajoutez, le cas échéant, l’instruction suivante en haut du fichier de code.

  ```csharp
  using Microsoft.Advertising.WinRT.UI;
  using Windows.System.Profile;
  ```
7. Ajoutez les déclarations de constantes suivantes à votre classe **Page**.

  ```csharp
  private const int AD_WIDTH = <tbd>;
  private const int AD_HEIGHT = <tbd>;
  private const string WAPPLICATIONID = "<tbd>";
  private const string WADUNITID = "<tbd>";
  private const string MAPPLICATIONID = "<tbd>";
  private const string MADUNITID = "<tbd>";
  ```
7. Pour chacune de ces déclarations de constantes, remplacez les valeurs ```<tbd>``` :
  * **AD_WIDTH** et **AD_HEIGHT** : assignez ces éléments à l’une des [tailles de bannières publicitaires prises en charge]( https://msdn.microsoft.com/windows/uwp/monetize/supported-ad-sizes-for-banner-ads).
  * **WAPPLICATIONID** et **WADUNITID** : affectez ces éléments aux valeurs **WApplicationId** et **WAdUnitId** pour les publicités payantes Microsoft que vous avez récupérées précédemment à partir du fichier de configuration de la médiation (ces valeurs s’appliquent à l’unité publicitaire non mobile correspondant aux publicités payantes).
  * **MAPPLICATIONID** et **MADUNITID** : affectez ces éléments aux valeurs **MApplicationId** et **MAdUnitId** pour les publicités payantes Microsoft que vous avez récupérées précédemment à partir du fichier de configuration de la médiation (ces valeurs s’appliquent à l’unité publicitaire mobile correspondant aux publicités payantes).

8. Ajoutez les déclarations de variables suivantes à votre classe **Page**.

  ```csharp
  // Declare an AdControl.
  private AdControl myAdControl = null;

  // Application ID and ad unit ID values for Microsoft advertising. By default,
  // assign these to non-mobile ad unit info.
  private string myAppId = WAPPLICATIONID;
  private string myAdUnitId = WADUNITID;
  ```
5. Ajoutez le code suivant à votre constructeur de classe **Page**, après l’appel à la méthode **InitializeComponent()**.

  ```csharp
  myAdGrid.Width = AD_WIDTH;
  myAdGrid.Height = AD_HEIGHT;

  // For mobile device families, use the mobile ad unit info.
  if ("Windows.Mobile" == AnalyticsInfo.VersionInfo.DeviceFamily)
  {
      myAppId = MAPPLICATIONID;
      myAdUnitId = MADUNITID;
  }

  // Initialize the AdControl.
  myAdControl = new AdControl();
  myAdControl.ApplicationId = myAppId;
  myAdControl.AdUnitId = myAdUnitId;
  myAdControl.Width = AD_WIDTH;
  myAdControl.Height = AD_HEIGHT;
  myAdControl.IsAutoRefreshEnabled = true;

  myAdGrid.Children.Add(myAdControl);
  ```  

### Publicités payantes Microsoft, publicités maison et AdDuplex

Si vous utilisez les publicités maison Microsoft, AdDuplex ou encore les publicités payantes Microsoft et que vous souhaitez continuer d’utiliser AdDuplex pour votre médiation publicitaire, suivez les étapes décrites dans cette section. Les exemples de code prennent en charge aussi bien AdDuplex que les publicités maison Microsoft. Si vous utilisez AdDuplex mais pas les publicités maison Microsoft (ou inversement), supprimez le code qui ne s’applique pas à votre scénario.

  >**Remarque**  Ces étapes supposent que la page d’application sur laquelle vous voulez afficher des publicités contient une grille vide nommée **myAdGrid**, par exemple : ```<Grid x:Name="myAdGrid"/>```. Dans ces étapes, vous allez créer et configurer les contrôles publicitaires entièrement dans votre code, plutôt qu’avec XAML.

1. Ouvrez votre application UWP dans Visual Studio.
2.  Dans la fenêtre **Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence...**.
Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour XAML** (version 10.0).
3. Dans **Gestionnaire de références**, cliquez sur OK.
4. Supprimez la déclaration **AdMediatorControl** de votre code XAML et supprimez le code qui utilise cet objet **AdMediatorControl**, y compris tous les gestionnaires d’événements associés.
5. Ouvrez le fichier de code de l’application **Page** sur laquelle vous voulez afficher des publicités.
6. Ajoutez, le cas échéant, les instructions suivantes en haut du fichier de code.

  ```csharp
  using Windows.System.UserProfile;
  using Microsoft.Advertising.WinRT.UI;
  using Windows.System.Profile;
  ```
7. Ajoutez les déclarations de constantes suivantes à votre classe **Page**.

  ```csharp
  private const int AD_WIDTH = <tbd>;
  private const int AD_HEIGHT = <tbd>;
  private const int HOUSE_AD_WEIGHT = <tbd>;
  private const int AD_REFRESH_SECONDS = 35;
  private const int MAX_ERRORS_PER_REFRESH = 3;
  private const string WAPPLICATIONID = "<tbd>";
  private const string WADUNITID_PAID = "<tbd>";
  private const string WADUNITID_HOUSE = "<tbd>";
  private const string MAPPLICATIONID = "<tbd>";
  private const string MADUNITID_PAID = "<tbd>";
  private const string MADUNITID_HOUSE = "<tbd>";
  private const string ADDUPLEX_APPKEY = "<tbd>";
  private const string ADDUPLEX_ADUNIT = "<tbd>";
  ```
4. Pour chacune des déclarations de constante affectées à ```<tbd>```, remplacez la valeur ```<tbd>``` par la valeur réelle de votre scénario.
  * **AD_WIDTH** et **AD_HEIGHT** : assignez ces éléments à l’une des [tailles de bannières publicitaires prises en charge]( https://msdn.microsoft.com/windows/uwp/monetize/supported-ad-sizes-for-banner-ads).
  * **HOUSE_AD_WEIGHT** : affectez cet élément à un entier compris entre 0 et 100 qui spécifie la valeur de poids que vous voulez appliquer aux publicités maison Microsoft par rapport aux publicités payantes Microsoft (où 0 indique que les publicités maison ne doivent jamais être affichées, et 100 que les publicités maison doivent toujours être affichées).
  * **WAPPLICATIONID** et **WADUNITID_PAID** : affectez ces éléments aux valeurs **WApplicationId** et **WAdUnitId** pour les publicités payantes Microsoft que vous avez récupérées précédemment à partir du fichier de configuration de la médiation (ces valeurs s’appliquent à l’unité publicitaire non mobile correspondant aux publicités payantes).
  * **WADUNITID_HOUSE** : affectez cet élément à la valeur **WAdUnitId** correspondant aux publicités maison que vous avez récupérées précédemment à partir du fichier de configuration de la médiation (cette valeur s’applique à l’unité publicitaire non mobile correspondant aux publicités maison).
  * **MAPPLICATIONID** et **MADUNITID_PAID** : affectez ces éléments aux valeurs **MApplicationId** et **MAdUnitId** pour les publicités payantes Microsoft que vous avez récupérées précédemment à partir du fichier de configuration de la médiation (ces valeurs s’appliquent à l’unité publicitaire mobile correspondant aux publicités payantes).
  * **MADUNITID_HOUSE** : affectez cet élément à la valeur **MAdUnitId** correspondant aux publicités maison que vous avez récupérées précédemment à partir du fichier de configuration de la médiation (cette valeur s’applique à l’unité publicitaire mobile correspondant aux publicités maison).
  * **ADDUPLEX_APPKEY** et **ADDUPLEX_ADUNIT** : affectez ces éléments aux valeurs de clé d’application AdDuplex et d’unité publicitaire que vous avez récupérées à partir du fichier de configuration de la médiation.
5. Ajoutez les déclarations de variables suivantes à votre classe **Page**.
  ```csharp
  // Dispatch timer to fire at each ad refresh interval.
  private DispatcherTimer myAdRefreshTimer = new DispatcherTimer();

  // Global variables used for mediation decisions.
  private Random randomGenerator = new Random();
  private int errorCountCurrentRefresh = 0;  // Prevents infinite redirects.
  private int adDuplexWeight = 0;            // Will be set by GetAdDuplexWeight().

  // Microsoft and AdDuplex controls for banner ads.
  private AdControl myMicrosoftBanner = null;
  private AdDuplex.AdControl myAdDuplexBanner = null;

  // Application ID and ad unit ID values for Microsoft advertising. By default,
  // assign these to non-mobile ad unit info.
  private string myMicrosoftAppId = WAPPLICATIONID;
  private string myMicrosoftPaidUnitId = WADUNITID_PAID;
  private string myMicrosoftHouseUnitId = WADUNITID_HOUSE;
  ```
5. Ajoutez le code suivant à votre constructeur de classe **Page**, après l’appel à la méthode **InitializeComponent()**.
  ```csharp
  myAdGrid.Width = AD_WIDTH;
  myAdGrid.Height = AD_HEIGHT;
  adDuplexWeight = GetAdDuplexWeight();
  RefreshBanner();

  // Start the timer to refresh the banner at the desired interval.
  myAdRefreshTimer.Interval = new TimeSpan(0, 0, AD_REFRESH_SECONDS);
  myAdRefreshTimer.Tick += myAdRefreshTimer_Tick;
  myAdRefreshTimer.Start();

  // For mobile device families, use the mobile ad unit info.
  if ("Windows.Mobile" == AnalyticsInfo.VersionInfo.DeviceFamily)
  {
      myMicrosoftAppId = MAPPLICATIONID;
      myMicrosoftPaidUnitId = MADUNITID_PAID;
      myMicrosoftHouseUnitId = MADUNITID_HOUSE;
  }
  ```
6. Pour finir, ajoutez les méthodes suivantes à votre classe **Page**. Ces méthodes instancient les objets **AdControl** Microsoft et **AdControl** AdDuplex, et utilisent un générateur de nombres aléatoire avec les valeurs de poids données pour actualiser les bannières publicitaires dans ces contrôles à des intervalles réguliers.
  ```csharp
  private int GetAdDuplexWeight()
  {
      // TODO: Change this logic to fit your needs.
      // This example uses Microsoft ads first in Canada and Mexico, then
      // AdDuplex as fallback. In France, AdDuplex is first. In other regions,
      // this example uses a weighted average approach, with 50% to AdDuplex.

      int returnValue = 0;
      switch (GlobalizationPreferences.HomeGeographicRegion)
      {
          case "CA":
          case "MX":
              returnValue = 0;
              break;
          case "FR":
              returnValue = 100;
              break;
          default:
              returnValue = 50;
              break;
      }
      return returnValue;
  }

  private void ActivateMicrosoftBanner()
  {
      // Return if you hit the error limit for this refresh interval.
      if (errorCountCurrentRefresh >= MAX_ERRORS_PER_REFRESH)
      {
          myAdGrid.Visibility = Visibility.Collapsed;
          return;
      }

      // Use random number generator and house ads weight to determine whether
      // to use paid ads or house ads. Paid is the default. You could alternatively
      // write a method similar to GetAdDuplexWeight and override by region.
      string myAdUnit = myMicrosoftPaidUnitId;
      int houseWeight = HOUSE_AD_WEIGHT;
      int randomInt = randomGenerator.Next(0, 100);
      if (randomInt < houseWeight)
      {
          myAdUnit = myMicrosoftHouseUnitId;
      }

      // Hide the AdDuplex control if it is showing.
      if (null != myAdDuplexBanner)
      {
          myAdDuplexBanner.Visibility = Visibility.Collapsed;
      }

      // Initialize or display the Microsoft control.
      if (null == myMicrosoftBanner)
      {
          myMicrosoftBanner = new AdControl();
          myMicrosoftBanner.ApplicationId = myMicrosoftAppId;
          myMicrosoftBanner.AdUnitId = myAdUnit;
          myMicrosoftBanner.Width = AD_WIDTH;
          myMicrosoftBanner.Height = AD_HEIGHT;
          myMicrosoftBanner.IsAutoRefreshEnabled = false;

          myMicrosoftBanner.AdRefreshed += myMicrosoftBanner_AdRefreshed;
          myMicrosoftBanner.ErrorOccurred += myMicrosoftBanner_ErrorOccurred;

          myAdGrid.Children.Add(myMicrosoftBanner);
      }
      else
      {
          myMicrosoftBanner.AdUnitId = myAdUnit;
          myMicrosoftBanner.Visibility = Visibility.Visible;
          myMicrosoftBanner.Refresh();
      }
  }

  private void ActivateAdDuplexBanner()
  {
      // Return if you hit the error limit for this refresh interval.
      if (errorCountCurrentRefresh >= MAX_ERRORS_PER_REFRESH)
      {
          myAdGrid.Visibility = Visibility.Collapsed;
          return;
      }

      // Hide the Microsoft control if it is showing.
      if (null != myMicrosoftBanner)
      {
          myMicrosoftBanner.Visibility = Visibility.Collapsed;
      }

      // Initialize or display the AdDuplex control.
      if (null == myAdDuplexBanner)
      {
          myAdDuplexBanner = new AdDuplex.AdControl();
          myAdDuplexBanner.AppKey = ADDUPLEX_APPKEY;
          myAdDuplexBanner.AdUnitId = ADDUPLEX_ADUNIT;
          myAdDuplexBanner.Width = AD_WIDTH;
          myAdDuplexBanner.Height = AD_HEIGHT;
          myAdDuplexBanner.RefreshInterval = AD_REFRESH_SECONDS;

          myAdDuplexBanner.AdLoaded += myAdDuplexBanner_AdLoaded;
          myAdDuplexBanner.AdCovered += myAdDuplexBanner_AdCovered;
          myAdDuplexBanner.AdLoadingError += myAdDuplexBanner_AdLoadingError;
          myAdDuplexBanner.NoAd += myAdDuplexBanner_NoAd;

          myAdGrid.Children.Add(myAdDuplexBanner);
      }
      myAdDuplexBanner.Visibility = Visibility.Visible;
  }

  private void myAdRefreshTimer_Tick(object sender, object e)
  {
      RefreshBanner();
  }

  private void RefreshBanner()
  {
      // Reset the error counter for this refresh interval and
      // make sure the ad grid is visible.
      errorCountCurrentRefresh = 0;
      myAdGrid.Visibility = Visibility.Visible;

      // Display ad from AdDuplex.
      if (100 == adDuplexWeight)
      {
          ActivateAdDuplexBanner();
      }
      // Display Microsoft ad.
      else if (0 == adDuplexWeight)
      {
          ActivateMicrosoftBanner();
      }
      // Use weighted approach.
      else
      {
          int randomInt = randomGenerator.Next(0, 100);
          if (randomInt < adDuplexWeight) ActivateAdDuplexBanner();
          else ActivateMicrosoftBanner();
      }
  }

  private void myMicrosoftBanner_AdRefreshed(object sender, RoutedEventArgs e)
  {
      // Add your code here as necessary.
  }

  private void myMicrosoftBanner_ErrorOccurred(object sender, AdErrorEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateAdDuplexBanner();
  }

  private void myAdDuplexBanner_AdLoaded(object sender, AdDuplex.Banners.Models.BannerAdLoadedEventArgs e)
  {
      // Add your code here as necessary.
  }

  private void myAdDuplexBanner_NoAd(object sender, AdDuplex.Common.Models.NoAdEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateMicrosoftBanner();
  }

  private void myAdDuplexBanner_AdLoadingError(object sender, AdDuplex.Common.Models.AdLoadingErrorEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateMicrosoftBanner();
  }

  private void myAdDuplexBanner_AdCovered(object sender,   AdDuplex.Banners.Core.AdCoveredEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateMicrosoftBanner();
  }
  ```



<!--HONumber=Sep16_HO1-->


