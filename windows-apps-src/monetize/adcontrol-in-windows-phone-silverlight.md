---
author: mcleanbyron
ms.assetid: c0450f7b-5c81-4d8c-92ef-2b1190d18af7
description: "Découvrez comment utiliser la classe AdControl pour afficher des bannières publicitaires dans une application Silverlight pour Windows Phone8.1 ou Windows Phone8.0."
title: AdControl dans Silverlight Windows Phone
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 5a12badfb11cfd43c0833522d996da7df73b3d55


---

# AdControl dans Silverlight Windows Phone


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Cette procédure pas à pas montre comment utiliser la classe [AdControl](https://msdn.microsoft.com/library/windows/apps/hh524191.aspx) pour afficher des bannières publicitaires dans une application Silverlight pour Windows Phone8.1 ou Windows Phone8.0.

## Conditions préalables

*  Installez le [Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft](http://aka.ms/store-em-sdk) avec Visual Studio2015 ou Visual Studio2013.


## Ajouter les références d’assemblys publicitaires

Les assemblys Microsoft Advertising pour les projets Silverlight Windows Phone ne sont pas installés localement avec le Kit de développement logiciel (SDK) d’engagement et de monétisation de la BoutiqueMicrosoft. Avant de pouvoir mettre à jour votre code, vous devez d’abord utiliser **Services connectés** avec la prise en charge de la médiation publicitaire dans le Kit de développement logiciel (SDK) d’engagement et de monétisation de la BoutiqueMicrosoft pour télécharger ces assemblys et les référencer dans votre projet.

1.  Dans Visual Studio, cliquez sur **Projet**, puis sur **Ajouter un service connecté**.

2.  Dans la boîte de dialogue **Ajouter un service connecté**, cliquez sur **Ad Mediator**, puis sur **Configurer**.

3.  Cliquez sur **Sélectionner les réseaux publicitaires**, puis sélectionnez uniquement **Microsoft Advertising**.

    À ce stade, tous les assemblys Microsoft Advertising nécessaires pour Silverlight sont téléchargés vers votre projet local via des packages NuGet, et les références à ces assemblys sont ajoutées automatiquement à votre projet. Une référence à l’assembly de médiation publicitaire est également ajoutée à votre projet. Vous la supprimerez ultérieurement, car elle n’est pas nécessaire pour ce scénario.

4.  Dans la boîte de dialogue **Sélectionner les réseaux publicitaires**, cliquez sur **OK**. Cliquez de nouveau sur **OK** dans la page de confirmation **État de l’extraction** suivante, puis cliquez sur **Ajouter** pour fermer la boîte de dialogue **Ad Mediator**.

5.  Dans l’**Explorateur de solutions**, développez le nœud **Références**. Cliquez avec le bouton droit sur **Microsoft.AdMediator.WindowsPhone81SL.MicrosoftAdvertising**, puis cliquez sur **Supprimer**. Cette référence d’assembly n’est pas nécessaire pour ce scénario.

## Coder votre application


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

    ``` syntax
    xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
    ```

    L’en-tête de votre page contient alors le code suivant:

    ``` syntax
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
        x:Class="PhoneApp1.MainPage"
    ```

6.  Dans la balise **Grid**, ajoutez le code suivant correspondant à la classe **AdControl**. Affectez les propriétés **ApplicationId** et **AdUnitId** aux valeurs de test indiquées dans [Valeurs du mode test](test-mode-values.md), puis définissez les propriétés **Height** et **Width** sur l’une des [tailles de bannières publicitaires prises en charge](supported-ad-sizes-for-banner-ads.md).

    > **Remarque**  
    Vous allez remplacer les valeurs **ApplicationId** et **AdUnitId** de test par les valeurs dynamiques avant de soumettre votre application.

    ``` syntax
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

## Publier l’application avec des publicités dynamiques à l’aide du Centre de développement


1.  Dans le tableau de bord du Centre de développement, accédez à la page **Monétisation**&gt;**Monétiser avec des publicités** de votre application, puis [créez une unité Microsoft Advertising autonome](../publish/monetize-with-ads.md). Pour le type d’unité publicitaire, spécifiez **Bannière**. Prenez note de l’ID d’unité publicitaire et de l’ID de l’application.

2.  Dans votre code, remplacez les valeurs de test de l’unité publicitaire (**applicationId** et **adUnitId**) par les valeurs dynamiques que vous avez générées dans le Centre de développement.

3.  
            [Soumettez votre application](../publish/app-submissions.md) au Windows Store à l’aide du tableau de bord du Centre de développement.

4.  Passez en revue vos [rapports de performances des publicités](../publish/advertising-performance-report.md) dans le tableau de bord du Centre de développement.


 



<!--HONumber=Jun16_HO4-->


