---
author: mcleanbyron
ms.assetid: cf0d2709-21a1-4d56-9341-d4897e405f5d
description: "Découvrez comment intercepter les erreurs AdControl dans votre application."
title: "Gestion des erreurs dans la procédure pas à pas pour XAML/C#"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 7cb827b4269afb55f0326eec0a0ee25b93119eb0


---

# Gestion des erreurs dans la procédure pas à pas pour XAML/C#


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Cette rubrique montre comment intercepter les erreurs [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) dans votre application.

Ces exemples partent du principe que vous disposez d’une application XAML/C# qui contient un **AdControl**. Pour obtenir des instructions pas à pas qui montrent comment ajouter un **AdControl** à votre application, voir [AdControl en XAML et .NET](adcontrol-in-xaml-and--net.md). Pour un exemple de projet complet illustrant l’ajout de bannières publicitaires à une application XAML en C# et C++, voir [Exemples de publicité sur GitHub](http://aka.ms/githubads).

1.  Dans votre fichier MainPage.xaml, recherchez la définition du contrôle **AdControl**. Ce code se présente ainsi:

    ``` syntax
    <UI:AdControl
       ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
       AdUnitId="10865270"
       HorizontalAlignment="Left"
       Height="250"
       Margin="10,10,0,0"
       VerticalAlignment="Top"
       Width="300" />
    ```

2.   Après la propriété **Width**, mais avant la balise de fermeture, affectez le nom d’un gestionnaire d’événements d’erreur à l’événement [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.erroroccurred.aspx). Dans cette procédure pas à pas, le nom du gestionnaire d’événements d’erreur est **OnAdError**.

    ``` syntax
    ErrorOccurred="OnAdError"
    ```

    Le code XAML complet qui définit le contrôle **AdControl** se présente comme suit.

    ``` syntax
    <UI:AdControl
       ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
       AdUnitId="10865270"
       HorizontalAlignment="Left"
       Height="250"
       Margin="10,10,0,0"
       VerticalAlignment="Top"
       Width="300"
       ErrorOccurred="OnAdError"/>
    ```

2.  Pour générer une erreur lors de l’exécution, créez un second contrôle **AdControl** avec un ID d’application différent. Comme tous les objets **AdControl** d’une application doivent utiliser le même ID d’application, la création d’un objet **AdControl** supplémentaire doté d’un autre ID d’application lève une erreur.

    Définissez un second objet **AdControl** dans le fichier MainPage.xaml juste après le premier **AdControl**, puis définissez la propriété [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) sur zéro («0»).

    ``` syntax
    <UI:AdControl
      ApplicationId="0"
      AdUnitId="10865270"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,265,0,0"
      VerticalAlignment="Top"
      Width="300"
      ErrorOccurred="OnAdError" />
    ```

3.  Dans MainPage.xaml.cs, ajoutez le gestionnaire d’événements **OnAdError** suivant à la classe **MainPage**. Ce gestionnaire d’événements écrit les informations dans la fenêtre **Sortie** de VisualStudio.

    ``` syntax
    private void OnAdError(object sender, AdErrorEventArgs e)
    {
        System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.Error + " ErrorCode: " + e.ErrorCode.ToString());
    }
    ```

4.  Générez et exécutez le projet.

Après l’exécution de l’application, un message semblable à celui qui suit s’affiche dans la fenêtre **Sortie** de VisualStudio.

``` syntax
AdControl error (): MicrosoftAdvertising.Shared.AdException: all ad requests must use the same application ID within a single application (0, d25517cb-12d4-4699-8bdc-52040c712cab) ErrorCode: ClientConfiguration
```

## Rubriques connexes

* [Exemples de publicité sur GitHub](http://aka.ms/githubads)

 



<!--HONumber=Jun16_HO4-->


