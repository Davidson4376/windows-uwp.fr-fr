---
author: mcleanbyron
ms.assetid: c0450f7b-5c81-4d8c-92ef-2b1190d18af7
description: "Découvrez comment utiliser la classe AdControl pour afficher des bannières publicitaires dans une application Silverlight pour Windows Phone8.1 ou Windows Phone8.0."
title: AdControl dans Silverlight Windows Phone
ms.author: mcleans
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows10, uwp, annonces, publicité, AdControl, Silverlight, Windows Phone"
ms.openlocfilehash: f1582639757abfb6de156bf88ce8af71ba3eaacd
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/21/2017
---
# <a name="adcontrol-in-windows-phone-silverlight"></a>AdControl dans Silverlight Windows Phone

Cette procédure pas à pas montre comment utiliser la classe [AdControl](https://msdn.microsoft.com/library/windows/apps/hh524191.aspx) pour afficher des bannières publicitaires dans une application Silverlight pour Windows Phone8.1 ou Windows Phone8.0.

<span id="silverlight_support"/>
## <a name="advertising-support-for-windows-phone-8x-silverlight-projects"></a>Prise en charge des publicités pour les projets Silverlight WindowsPhone8.x

Certains scénarios de développement ne sont plus pris en charge dans les projets Silverlight Windows Phone8.x. Pour plus d’informations, voir le tableau suivant.

|  Version de la plateforme  |  Projets existants    |   Nouveaux projets  |
|-----------------|----------------|--------------|
| Silverlight Windows Phone8.0     |  Si vous avez un projet Silverlight Windows Phone8.0 existant qui utilise déjà un **AdControl** ou **AdMediatorControl** d’une précédente version de Microsoft Universal Ad Client SDK ou Microsoft Advertising SDK et que cette application est déjà publiée dans le Windows Store, vous pouvez modifier, puis régénérer le projet, puis vous pouvez déboguer ou tester vos modifications sur un appareil. Le débogage ou les tests du projet dans l’émulateur ne sont pas pris en charge.  |  Non pris en charge.  |
| Silverlight Windows Phone8.1    |  Si vous disposez d’un projet Silverlight Windows Phone8.1 qui utilise un **AdControl** ou **AdMediatorControl** d’un kit de développement logiciel (SDK) antérieur, vous pouvez modifier et régénérer le projet. Toutefois, pour déboguer ou tester l’application, vous devez l’exécuter dans l’émulateur et utiliser des [valeurs de mode de test](test-mode-values.md) pour les ID d’application et ID d’unité publicitaire. Le débogage ou les tests de l’application sur un appareil ne sont pas pris en charge.  |   Vous pouvez ajouter un **AdControl** ou **AdMediatorControl** à un nouveau projet Silverlight Windows Phone8.1. Toutefois, pour déboguer ou tester l’application, vous devez l’exécuter dans l’émulateur et utiliser des [valeurs de mode de test](test-mode-values.md) pour les ID d’application et ID d’unité publicitaire. Le débogage ou les tests de l’application sur un appareil ne sont pas pris en charge. |

## <a name="add-the-advertising-assemblies-to-your-project"></a>Ajouter les assemblys publicitaires à votre projet

Pour commencer, téléchargez et installez le package NuGet qui contient les assemblys publicitaires Microsoft pour Silverlight Windows Phone à votre projet.

1.  Ouvrez votre projet dans VisualStudio.

2.  Cliquez sur **Outils**, pointez sur **Gestionnaire de package NuGet**, puis cliquez sur **Console du Gestionnaire de package**.

3.  Dans la fenêtre **Console du Gestionnaire de package**, entrez l’une de ces commandes.

  * Si votre projet cible Windows Phone8.0, entrez cette commande.

      ```syntax
      Install-Package Microsoft.Advertising.WindowsPhone.SL80 -Version 6.2.40501.1
      ```

  * Si votre projet cible Windows Phone8.1, entrez cette commande.

      ```syntax
      Install-Package Microsoft.Advertising.WindowsPhone.SL81 -Version 8.1.50112
      ```

    Une fois la commande entrée, tous les assemblys publicitaires Microsoft nécessaires à Silverlight sont téléchargés dans votre projet local via des packages NuGet, et les références à ces assemblys sont ajoutées automatiquement à votre projet.

## <a name="code-your-app"></a>Coder votre application


1.  Ajoutez les fonctionnalités suivantes dans le nœud **Fonctionnalités** de votre fichier WMAppManifest.xml.

  ``` syntax
  <Capability Name="ID_CAP_IDENTITY_USER"/>
  <Capability Name="ID_CAP_MEDIALIB_PHOTO"/>
  <Capability Name="ID_CAP_PHONEDIALER"/>
  ```

  Pour cet exemple, le nœud **Fonctionnalités** ressemble à ceci:

  ``` syntax
  <Capabilities>
      <Capability Name="ID_CAP_NETWORKING"/>
      <Capability Name="ID_CAP_MEDIALIB_AUDIO"/>
      <Capability Name="ID_CAP_MEDIALIB_PLAYBACK"/>
      <Capability Name="ID_CAP_SENSORS"/>
      <Capability Name="ID_CAP_WEBBROWSERCOMPONENT"/>
      <Capability Name="ID_CAP_IDENTITY_USER"/>
      <Capability Name="ID_CAP_MEDIALIB_PHOTO"/>
      <Capability Name="ID_CAP_PHONEDIALER"/>
  </Capabilities>
  ```

2.  (Facultatif) Enregistrez votre projet. Cliquez sur l’icône **Enregistrer tout**, ou dans le menu **Fichier**, cliquez sur **Enregistrer tout**.

3.  Ajoutez la fonctionnalité **Internet (client et serveur)** dans le fichier Package.appxmanifest de votre projet. Dans l’**Explorateur de solutions**, double-cliquez sur **Package.appxmanifest**.

    ![wp81silverlightmarkup\-solutionexplorer\-packageappxmanifest](images/13-b98c2a1a-69c3-4018-be0a-6ce010e703e7.jpg)

    Dans l’**Éditeur**, cliquez sur l’onglet **Fonctionnalités**. Cochez la case **Internet (client et serveur)**.

4.  (Facultatif) Enregistrez votre projet. Cliquez sur l’icône **Enregistrer tout**, ou dans le menu **Fichier**, cliquez sur **Enregistrer tout**.

5.  Modifiez le balisage Silverlight du fichier MainPage.xaml pour inclure l’espace de noms **Microsoft.Advertising.Mobile.UI**.

  ``` xml
  xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
  ```

  L’en-tête de votre page contient alors le code suivant:

  ``` xml
  xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
  xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
  x:Class="PhoneApp1.MainPage"
  ```

6.  Dans la balise **Grid**, ajoutez le code suivant correspondant à la classe **AdControl**. Affectez les propriétés **ApplicationId** et **AdUnitId** aux valeurs de test indiquées dans [Valeurs du mode test](test-mode-values.md), puis attribuez aux propriétés **Height** et **Width** l’une des [tailles de bannières publicitaires prises en charge](supported-ad-sizes-for-banner-ads.md).

  > **Remarque**&nbsp;&nbsp;Avant de soumettre votre application, remplacez les valeurs de test **ApplicationId** et **AdUnitId** par des valeurs dynamiques.

  ``` xml
  <Grid x:Name="ContentPanel" Grid.Row="1">
      <UI:AdControl
          ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
          AdUnitId="10865270"
          HorizontalAlignment="Left"
          Height="80"
          VerticalAlignment="Top"
          Width="480"/>
  </Grid>
  ```

7.  Générez et exécutez votre projet. Vérifiez que votre application affiche une publicité, qui ressemble à ce qui suit:

  ![wp81silverlight\-emulatorwithad](images/13-8db1492f-ae1d-439b-9b78-bed8e22fe996.jpg)

## <a name="release-your-app-with-live-ads-using-dev-center"></a>Publier l’application avec des publicités dynamiques à l’aide du Centre de développement

1.  Dans le tableau de bord du Centre de développement, accédez à la page **Monétisation** &gt; **Monétiser avec des publicités** de votre application, puis [créez une unité publicitaire autonome](../publish/monetize-with-ads.md). Pour le type d’unité publicitaire, spécifiez **Bannière**. Prenez note de l’ID d’unité publicitaire et de l’ID de l’application.

2.  Dans votre code, remplacez les valeurs de test de l’unité publicitaire (**applicationId** et **adUnitId**) par les valeurs dynamiques que vous avez générées dans le Centre de développement.

3.  [Soumettez votre application](../publish/app-submissions.md) au Windows Store à l’aide du tableau de bord du Centre de développement.

4.  Passez en revue vos [rapports de performances des publicités](../publish/advertising-performance-report.md) dans le tableau de bord du Centre de développement.


 
