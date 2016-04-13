---
Description: Si vous donnez aux clients la possibilité d’utiliser votre application gratuitement pendant une période d’évaluation, vous pouvez leur donner envie de mettre à niveau vers la version complète de votre application en excluant ou en limitant certaines fonctionnalités pendant la période d’évaluation.
title: Exclure ou limiter des fonctionnalités de la version d’évaluation
ms.assetid: 1B62318F-9EF5-432A-8593-F3E095CA7056
Mots-clés : évaluation gratuite
Mots-clés : période d’évaluation gratuite
Mots-clés : exemple de code d’évaluation gratuit
Mots-clés : exemple de code d’évaluation gratuite
---

# Exclure ou limiter des fonctionnalités de la version d’évaluation


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Si vous donnez aux clients la possibilité d’utiliser votre application gratuitement pendant une période d’évaluation, vous pouvez leur donner envie de mettre à niveau vers la version complète de votre application en excluant ou en limitant certaines fonctionnalités pendant la période d’évaluation. Choisissez les fonctionnalités à limiter avant de commencer à coder, puis faites en sorte que votre application ne les rende disponibles qu’à l’achat de la licence complète. Vous pouvez également activer certaines fonctionnalités, telles que des bannières ou des filigranes, qui ne s’afficheront que pendant la période d’évaluation, avant l’achat de votre application par un client.

Voyons comment ajouter cette prise en charge à votre application.

## Prérequis

Application Windows dans laquelle ajouter des fonctionnalités que les clients peuvent acheter

## Étape 1 : Sélectionnez les fonctionnalités que vous voulez activer ou désactiver durant la période d’évaluation.

L’état de licence actuel de votre application est stocké dans les propriétés de la classe [**LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/br225157). D’une manière générale, vous placez les fonctions qui dépendent de l’état de la licence dans un bloc conditionnel, comme nous le décrivons à l’étape suivante. Lorsque vous développez ces fonctionnalités, assurez-vous de pouvoir les implémenter de telle manière qu’elles fonctionnent avec tous les états de la licence.

Décidez également comment vous voulez gérer les modifications apportées à la licence de l’application lorsque celle-ci est en cours d’exécution. La version d’évaluation de votre application peut être entièrement fonctionnelle, mais comporter des bandeaux publicitaires intégrés à l’application, contrairement à la version payante. La version d’évaluation de votre application peut également être privée de certaines fonctionnalités ou afficher régulièrement des messages demandant à l’utilisateur de l’acheter.

Pensez au type d’application que vous développez et demandez-vous quelle est la meilleure stratégie d’évaluation ou d’expiration. Pour la version d’évaluation d’un jeu, une bonne stratégie est de limiter le contenu du jeu auquel un utilisateur peut avoir accès. Dans le cas de la version d’évaluation d’un utilitaire, vous pouvez envisager de définir une date d’expiration ou de limiter les fonctionnalités qu’un acheteur potentiel peut utiliser.

Pour la plupart des applications qui ne sont pas des jeux, la définition d’une date d’expiration est une bonne méthode, car cela permet aux utilisateurs d’avoir une idée correcte de l’application complète. Voici quelques scénarios d’expiration classiques et comment les gérer.

-   **La licence d’évaluation expire tandis que l’application est en cours d’exécution.**

    Si la version d’évaluation expire tandis que votre application est en cours d’exécution, cette dernière peut :

    -   Ne rien faire.
    -   Afficher un message à l’attention de votre client.
    -   Se fermer.
    -   Inviter votre client à acheter l’application.

    La pratique recommandée est d’afficher un message invitant l’utilisateur à acheter l’application, et si c’est le cas, de poursuivre l’exécution avec toutes les fonctionnalités activées. Sinon, fermez l’application ou redemandez régulièrement à l’utilisateur s’il souhaite acheter l’application.

-   **La licence d’évaluation expire avant le lancement de l’application.**

    Si la version d’évaluation expire avant que l’utilisateur ne lance l’application, cette dernière ne démarre pas. Au lieu de cela, une boîte de dialogue s’affiche pour donner à l’utilisateur la possibilité d’acheter votre application dans le Windows Store.

-   **Le client achète l’application tandis que celle-ci est en cours d’exécution.**

    Si le client achète votre application tandis que celle-ci est en cours d’exécution, voici quelques actions possibles.

    -   Ne rien faire et continuer en mode d’évaluation jusqu’au redémarrage de l’application.
    -   Les remercier de leur achat ou afficher un message.
    -   Activer de manière silencieuse les fonctionnalités qui sont disponibles avec une licence complète (ou désactiver les notifications réservées à la version d’évaluation).

Si vous voulez détecter la modification de la licence et exécuter une action quelconque dans votre application, vous devez ajouter un gestionnaire d’événements, comme décrit à l’étape suivante.
## Étape 2 : Initialisation des informations de licence

Lors de l’initialisation de votre application, recherchez l’objet [**LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/br225157) de votre application, comme indiqué dans cet exemple. Nous supposons que **licenseInformation** est une variable ou un champ global de type **LicenseInformation**.

Initialisez l’objet [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) ou [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) pour accéder aux informations de licence de l’application.

```CSharp
void initializeLicense()
{
    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    //   licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    licenseInformation = CurrentAppSimulator.LicenseInformation;
      
}
```

Ajoutez un gestionnaire d’événements afin de recevoir des notifications en cas de modification de la licence lorsque l’application est en cours d’exécution. La licence de l’application peut par exemple changer si la période d’évaluation arrive à expiration ou si le client achète l’application à travers un Windows Store.

```CSharp
void InitializeLicense()
{
    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    //   licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    licenseInformation = CurrentAppSimulator.LicenseInformation;

    // Register for the license state change event.
     licenseInformation.LicenseChanged += new LicenseChangedEventHandler(licenseChangedEventHandler);

}

// ...

void licenseChangedEventHandler()
{
    ReloadLicense(); // code is in next steps
}
```

## Étape 3 : Coder les fonctionnalités dans des blocs conditionnels

Lorsque l’événement de changement de licence est déclenché, votre application doit appeler l’API de licence pour déterminer si l’état de la version d’évaluation a changé. Le code de cette étape indique comment structurer votre gestionnaire pour cet événement. À ce stade, si un utilisateur a acheté l’application, il est de bonne pratique de lui indiquer que l’état de sa licence a été modifié. Demandez à l’utilisateur de redémarrer l’application si vous avez conçu votre application ainsi. Rendez cependant cette transition aussi simple que possible.

Cet exemple illustre comment évaluer l’état de la licence d’une application afin de pouvoir activer ou désactiver une fonctionnalité particulière de votre application.

```CSharp
void ReloadLicense()
{
    if (licenseInformation.IsActive)
    {
         if (licenseInformation.IsTrial)
         {
             // Show the features that are available during trial only.
         }
         else
         {
             // Show the features that are available only with a full license.
         }
     }
     else
     {
         // A license is inactive only when there' s an error.
     }
}
```

## Étape 4 : Obtenir la date d’expiration de la version d’évaluation d’une application

Ajoutez le code permettant de déterminer la date d’expiration de la version d’évaluation de l’application.

Le code de cet exemple définit une fonction pour obtenir la date d’expiration de la licence pour la version d’évaluation de l’application. Si cette licence est toujours valide, faites afficher la date d’expiration avec le nombre de jours restants jusqu’à l’expiration de la période d’essai.

```CSharp
void DisplayTrialVersionExpirationTime()
{
    if (licenseInformation.IsActive)
    {
        if (licenseInformation.IsTrial)
        {
            var longDateFormat = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longdate");
                                                
            // Display the expiration date using the DateTimeFormatter. 
            // For example, longDateFormat.Format(licenseInformation.ExpirationDate)

            var daysRemaining = (licenseInformation.ExpirationDate - DateTime.Now).Days;

            // Let the user know the number of days remaining before the feature expires
        }
        else
        {
            // ...
        }
    }
    else
    {
       // ...
    }
}
```

## Étape 5 : Test des fonctionnalités à l’aide d’appels simulés à l’API de la licence

Vous pouvez maintenant tester votre application à l’aide d’appels simulés au serveur de licences. En JavaScript, C#, Visual Basic ou Visual C++, remplacez les références à [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) par [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) dans le code d’initialisation de l’application.

[
            **CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) obtient les informations de licence spécifiques au test à partir d’un fichier XML appelé « WindowsStoreProxy.xml », situé dans %userprofile%\AppData\local\packages\%%amp;lt;package name%%amp;gt;\LocalState\Microsoft\Windows Store\ApiData. Si ce chemin d’accès et ce fichier n’existent pas, vous devez les créer lors de l’installation ou de l’exécution. Si vous essayez d’accéder à la propriété [**CurrentAppSimulator.LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/hh779768) sans que le fichier WindowsStoreProxy.xml ne soit présent à cet emplacement spécifique, vous obtiendrez une erreur.

Cet exemple illustre l’ajout à votre application de code permettant de tester les divers états possibles de sa licence.

```CSharp
void appInit()
{
    // some app initialization functions

    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    //   licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    licenseInformation = CurrentAppSimulator.LicenseInformation;

    // other app initialization functions
}
```

Vous pouvez modifier le fichier WindowsStoreProxy.xml afin de changer les dates d’expiration simulées de votre application et de ses fonctionnalités. Testez l’ensemble de vos configurations d’expiration et de licence possibles afin de vous assurer que tout fonctionne comme vous le souhaitez.

## Étape 6 : Remplacement des méthodes d’API de licences simulées par l’API réelle

Après avoir testé votre application à l’aide du serveur de licences simulées et avant d’envoyer votre application à un Store pour certification, remplacez [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) par [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765), comme indiqué dans l’exemple de code suivant.

**Important** Votre application doit utiliser l’objet [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) quand vous la soumettez à un Windows Store. Dans le cas contraire, elle échoue à l’étape de certification.

```CSharp
void appInit()
{
    // some app initialization functions

    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    //   licenseInformation = CurrentAppSimulator.LicenseInformation;

    // other app initialization functions
}
```

## Étape 7 : Description du fonctionnement de la version d’évaluation à vos clients

Prenez soin d’expliquer à vos clients comment votre application se comportera pendant et après la période d’évaluation gratuite afin qu’ils ne soient pas surpris par le comportement de l’application.

Pour plus d’informations sur la description de votre application, voir [Création de descriptions d’applications](https://msdn.microsoft.com/library/windows/apps/mt148529).

## Rubriques connexes

* [Exemple du Windows Store (montre des versions d’évaluation et des achats in-app)](http://go.microsoft.com/fwlink/p/?LinkID=627610)
* [Définir la tarification et la disponibilité d’une application](https://msdn.microsoft.com/library/windows/apps/mt148548)
* [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)
* [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)
 

 






<!--HONumber=Mar16_HO1-->


