---
author: TylerMSFT
title: Créer et utiliser une extension d’application
description: Écrivez et hébergez des extensions d’applications de plateforme Windows universelle (UWP) qui vous permettent d’étendre votre application via des packages que les utilisateurs peuvent installer à partir du MicrosoftStore.
keywords: extension d’application, service d’application, arrière-plan
ms.author: twhitney
ms.date: 10/05/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c4c326dbafa719273c4535a42d58184c7ce360fe
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7286359"
---
# <a name="create-and-host-an-app-extension"></a>Créer et héberger une extension d’application

Cet article vous montre comment créer une extension d’applicationUWP et l’héberger dans une applicationUWP.

Cet article est accompagné d’un exemple de code:
- Téléchargez et décompressez [Exemple de code MathExtension](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip).
- Dans VisualStudio2017, ouvrez MathExtensionSample.sln. Définissez le type de build sur x86 (**Générer** > **Gestionnaire de configurations**, puis modifiez la **Plateforme** sur **x86** pour les deux projets).
- Déployez la solution: **Générer** > **Déployer la Solution**.

## <a name="introduction-to-app-extensions"></a>Présentation des extensions d’applications

Dans la plateforme Windows universelle (UWP), les extensions d’application fournissent des fonctionnalités similaires à celles des plug-ins, des macros complémentaires et des extensions sur d'autres plateformes. Les extensions MicrosoftEdge sont des extensions d’applicationUWP, par exemple. Les extensions d’applicationUWP ont été introduites dans Windows10 Édition anniversaire (version1607, build10.0.14393).

Les extensions d’applicationUWP sont des applicationsUWP qui possède une déclaration d’extension leur permettant de partager des événements de contenu et de déploiement avec une application hôte. Une application d’extension peut fournir plusieurs extensions.

Étant donné que les extensions d’application sont simplement des applicationsUWP, elles peuvent également être des applications entièrement fonctionnelles, des extensions d’hôte, et fournir des extensions à d’autres applications (sans qu'il soit nécessaire de créer des packages d’application distincts).

Lorsque vous créez un hôte d’extension d’application, vous créez une opportunité de développer un écosystème autour de votre application, dans lequel d’autres développeurs peuvent améliorer votre application d’une manière inattendue ou pour laquelle vous n'aviez pas les ressources. Prenez les extensions MicrosoftOffice, les extensions VisualStudio, les extensions de navigateur, etc. Celles-ci créent des expériences plus riches pour les applications qui vont au-delà des fonctionnalités fournies. Les extensions peuvent ajouter de la valeur et de la longévité à votre application.

**Vue d'ensemble**

De manière générale, pour configurer une relation d’extension d’application, nous devons:

1. Déclarer une application comme hôte d’extension.
2. Déclarer une application comme extension.
3. Décider d’implémenter l’extension comme un service d’application, une tâche en arrière-plan ou toute une autre façon.
4. Définir le mode de communication des hôtes et de leurs extensions.
5. Utiliser l’API [Windows.ApplicationModel.AppExtensions](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppExtensions) dans l’application hôte pour accéder aux extensions.

Nous allons voir comment procéder en examinant l’[exemple de code MathExtension](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip) qui implémente une calculatrice hypothétique à laquelle vous pouvez ajouter de nouvelles fonctions à l’aide d’extensions. Dans MicrosoftVisual Studio2017, chargez **MathExtensionSample.sln** à partir de l’exemple de code.

![Exemple de code MathExtension](images/mathextensionhost-calctab.png)

## <a name="declare-an-app-to-be-an-extension-host"></a>Déclarer une application comme hôte d’extension

Une application s’identifie elle-même comme un hôte d’extension d’application par la déclaration de l'élément `<AppExtensionHost>` dans son fichier Package.appxmanifest. Voir le fichier **Package.appxmanifest** dans le projet **MathExtensionHost** pour voir comment procéder.

_Package.appxmanifest dans le projet MathExtensionHost_
```xml
<Package
  ...
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
  ...
    <Applications>
      <Application Id="App" ... >
        ...
        <Extensions>
            <uap3:Extension Category="windows.appExtensionHost">
                <uap3:AppExtensionHost>
                  <uap3:Name>com.microsoft.mathext</uap3:Name>
                </uap3:AppExtensionHost>
          </uap3:Extension>
        </Extensions>
      </Application>
    </Applications>
    ...
</Package>
```

Notez le `xmlns:uap3="http://..."` et la présence de `uap3` dans `IgnorableNamespaces`. Ces éléments sont nécessaires, car nous utilisons l’espace de noms uap3.

`<uap3:Extension Category="windows.appExtensionHost">` identifie cette application comme un hôte d’extension.

L'élément **Name** dans `<uap3:AppExtensionHost>` désigne le nom du _contrat d'extension_. Lorsqu’une extension spécifie le même nom de contrat d’extension, l’hôte sera en mesure de la trouver. Par convention, nous vous recommandons de créer le nom du contrat d’extension en utilisant le nom de votre application ou de votre éditeur, afin d'éviter tout conflit potentiel avec d’autres noms de contrat d’extension.

Vous pouvez définir plusieurs hôtes et plusieurs extensions dans la même application. Dans cet exemple, nous déclarons un hôte. L’extension est définie dans une autre application.

## <a name="declare-an-app-to-be-an-extension"></a>Déclarer une application comme extension

Une application s’identifie elle-même comme une extension d’application par la déclaration de l'élément `<uap3:AppExtension>` dans son fichier **Package.appxmanifest**. Ouvrez le fichier **Package.appxmanifest** dans le projet **MathExtension** pour voir comment procéder.

_Package.appxmanifest dans le projet MathExtension:_
```xml
<Package
  ...
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
  ...
    <Applications>
      <Application Id="App" ... >
        ...
        <Extensions>
          ...
          <uap3:Extension Category="windows.appExtension">
            <uap3:AppExtension Name="com.microsoft.mathext"
                               Id="power"
                               DisplayName="x^y"
                               Description="Exponent"
                               PublicFolder="Public">
              <uap3:Properties>
                <Service>com.microsoft.powservice</Service>
              </uap3:Properties>
              </uap3:AppExtension>
          </uap3:Extension>
        </Extensions>
      </Application>
    </Applications>
    ...
</Package>
```

De nouveau, notez la ligne `xmlns:uap3="http://..."` et la présence de `uap3` dans `IgnorableNamespaces`. Ces éléments sont nécessaires, car nous utilisons l’espace de noms `uap3`.

`<uap3:Extension Category="windows.appExtension">` identifie cette application comme une extension.

La signification des attributs `<uap3:AppExtension>` est la suivante:

|Attribut|Description|Requis|
|---------|-----------|:------:|
|**Name**|C’est le nom du contrat d’extension. Lorsqu’il correspond au nom **Name** déclaré dans un hôte, l’hôte sera en mesure de trouver cette extension.| :heavy_check_mark: |
|**ID**| Identifie de façon unique cette extension. Dans la mesure où il peut y avoir plusieurs extensions qui utilisent le même nom de contrat d’extension (imaginez une application de peinture qui prend en charge plusieurs extensions), vous pouvez utiliser l’ID pour les distinguer. Les hôtes d’extension d’application peuvent utiliser l’ID afin de déduire des informations à propos du type d’extension. Par exemple, vous pouvez avoir une extension conçue pour une application de bureau et une autre pour un appareil mobile, avec l’ID comme facteur de différenciation. Vous pouvez également utiliser l'élément **Properties**, décrit ci-dessous, à cette fin.| :heavy_check_mark: |
|**DisplayName**| Peut servir à partir de votre application hôte pour identifier l’extension pour l’utilisateur. Il peut être interrogé à partir du [nouveau système de gestion de ressources](https://docs.microsoft.com/windows/uwp/app-resources/using-mrt-for-converted-desktop-apps-and-games) (`ms-resource:TokenName`) et utiliser ce dernier à des fins de localisation. Le contenu localisé est chargé à partir du package d’extension d’application, et non de l’application hôte. | |
|**Description** | Peut servir à partir de votre application hôte pour décrire l’extension pour l’utilisateur. Il peut être interrogé à partir du [nouveau système de gestion de ressources](https://docs.microsoft.com/windows/uwp/app-resources/using-mrt-for-converted-desktop-apps-and-games) (`ms-resource:TokenName`) et utiliser ce dernier à des fins de localisation. Le contenu localisé est chargé à partir du package d’extension d’application, et non de l’application hôte. | |
|**PublicFolder**|Nom d’un dossier, relatif à la racine de package, où vous pouvez partager du contenu avec l’hôte d’extension. Par convention, le nom est «Public», mais vous pouvez utiliser n’importe quel nom qui correspond à un dossier dans votre extension.| :heavy_check_mark: |

`<uap3:Properties>` est un élément facultatif qui contient des métadonnées personnalisées que les hôtes peuvent lire lors de l’exécution. Dans l’exemple de code, l’extension est implémentée comme un service d’application, de sorte que l’hôte a besoin d’un moyen pour obtenir le nom de ce service d’application afin de pouvoir l’appeler. Le nom du service d’application est défini dans l'élément <Service>, que nous avons défini (nous aurions pu l'appeler comme nous le souhaitions). L’hôte dans l’exemple de code recherche cette propriété lors de l’exécution pour connaître le nom du service d’application.

## <a name="decide-how-you-will-implement-the-extension"></a>Décidez comment vous allez implémenter l’extension.

La [session Build2016 sur les extensions d’application](https://channel9.msdn.com/Events/Build/2016/B808) montre comment utiliser le dossier public qui est partagé entre l’hôte et les extensions. Dans cet exemple, l’extension est implémentée par un fichier Javascript qui est stocké dans le dossier public, que l'hôte appelle. Cette approche a l’avantage d’être légère, ne nécessite pas de compilation et prend en charge la création de la page d’accueil par défaut qui fournit des instructions pour l’extension et un lien vers la page MicrosoftStore de l’application hôte. Voir l’[exemple de code d’extension d’application Build2016](https://github.com/Microsoft/App-Extensibility-Sample) pour plus d’informations. Plus précisément, voir le projet **InvertImageExtension** et `InvokeLoad()` dans ExtensionManager.cs, dans le projet **ExtensibilitySample**.

Dans cet exemple, nous allons utiliser un service d’application pour implémenter l’extension. Les services d’application présentent les avantages suivants:

- Si l’extension se bloque, cela n'entraînera pas le blocage de l’application hôte, car l’application hôte s’exécute dans son propre processus.
- Vous pouvez utiliser le langage de votre choix pour implémenter le service. Il n'est pas nécessaire qu'il corresponde au langage utilisé pour implémenter l’application hôte.
- Le service d’application a accès à son propre conteneur d’application, qui peut avoir des capacités différentes de celles de l’hôte.
- Une isolation est fournie entre les données en service et l’application hôte.

### <a name="host-app-service-code"></a>Code de service de l’application hôte

Voici le code de l’hôte qui appelle le service d’application de l’extension:

_ExtensionManager.cs dans le projet MathExtensionHost_
```cs
public async Task<double> Invoke(ValueSet message)
{
    if (Loaded)
    {
        try
        {
            // make the app service call
            using (var connection = new AppServiceConnection())
            {
                // service name is defined in appxmanifest properties
                connection.AppServiceName = _serviceName;
                // package Family Name is provided by the extension
                connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;

                // open the app service connection
                AppServiceConnectionStatus status = await connection.OpenAsync();
                if (status != AppServiceConnectionStatus.Success)
                {
                    Debug.WriteLine("Failed App Service Connection");
                }
                else
                {
                    // Call the app service
                    AppServiceResponse response = await connection.SendMessageAsync(message);
                    if (response.Status == AppServiceResponseStatus.Success)
                    {
                        ValueSet answer = response.Message as ValueSet;
                        if (answer.ContainsKey("Result")) // When our app service returns "Result", it means it succeeded
                        {
                            return (double)answer["Result"];
                        }
                    }
                }
            }
        }
        catch (Exception)
        {
             Debug.WriteLine("Calling the App Service failed");
        }
    }
    return double.NaN; // indicates an error from the app service
}
```

Il s’agit d’un code classique pour appeler un service d’application. Pour plus d’informations sur la façon d’implémenter et d’appeler un service d’application, voir [Comment créer et utiliser un service d’application](how-to-create-and-consume-an-app-service.md).

Une chose à noter est la manière dont le nom du service d’application à appeler est déterminé. Étant donné que l’hôte n’a pas d’informations sur l’implémentation de l’extension, l’extension doit fournir le nom de son service d’application. Dans l’exemple de code, l’extension déclare le nom du service d’application dans son fichier, dans l'élément `<uap3:Properties>`:

_Package.appxmanifest dans le projet MathExtension_
```xml
    ...
    <uap3:Extension Category="windows.appExtension">
      <uap3:AppExtension ...>
        <uap3:Properties>
          <Service>com.microsoft.powservice</Service>
        </uap3:Properties>
        </uap3:AppExtension>
    </uap3:Extension>
```

Vous pouvez définir votre propre code XML dans l'élément `<uap3:Properties>`. Dans ce cas, nous définissons le nom du service d’application afin que l’hôte puisse l’utiliser lorsqu’il appelle l’extension.

Lorsque l’hôte charge une extension, du code tel que celui-ci extrait le nom du service à partir des propriétés définies dans le fichier Package.appxmanifest de l’extension:

_`Update()` dans ExtensionManager.cs, dans le projet MathExtensionHost_
```cs
...
var properties = await ext.GetExtensionPropertiesAsync() as PropertySet;

...
#region Update Properties
// update app service information
_serviceName = null;
if (_properties != null)
{
   if (_properties.ContainsKey("Service"))
   {
       PropertySet serviceProperty = _properties["Service"] as PropertySet;
       this._serviceName = serviceProperty["#text"].ToString();
   }
}
#endregion
```

Avec le nom du service d’application stocké dans `_serviceName`, l’hôte est en mesure de l’utiliser pour appeler le service d’application.

Appeler un service d’application requiert également le nom de famille du package qui contient le service d’application. Heureusement, l’API d’extension d’application fournit cette information qui est obtenue dans la ligne: `connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;`

### <a name="define-how-the-host-and-the-extension-will-communicate"></a>Définir le mode de communication de l’hôte et de l’extension

Les services d’application utilisent une classe [ValueSet](https://docs.microsoft.com/uwp/api/windows.foundation.collections.valueset) pour échanger des informations. En tant qu’auteur de l’hôte, vous devez élaborer un protocole flexible pour communiquer avec les extensions. Dans l’exemple de code, cela signifie prendre en compte les extensions qui peuvent avoir un ou plusieurs arguments dans le futur.

Dans cet exemple, le protocole pour les arguments est une classe **ValueSet** contenant les paires clé-valeur nommées «Arg» + le numéro de l’argument, par exemple `Arg1` et `Arg2`. L’hôte transmet tous les arguments dans la classe **ValueSet** et l’extension utilise ceux dont elle a besoin. Si l’extension est en mesure de calculer un résultat, alors l’hôte s'attend à ce que la classe **ValueSet** retournée par l’extension ait une clé nommée `Result` contenant la valeur du calcul. Si cette clé n’est pas présente, l’hôte suppose que l’extension n’a pas pu effectuer le calcul.

### <a name="extension-app-service-code"></a>Code de service de l’application d’extension

Dans l’exemple de code, le service d’application de l’extension n’est pas implémenté comme une tâche en arrière-plan. Au lieu de cela, il utilise le modèle de service d’application proc unique dans lequel le service d’application s’exécute dans le même processus que l’application d’extension qui l’héberge. Il s’agit toujours d’un processus différent de l’application hôte, offrant les avantages de la séparation de processus tout en bénéficiant de gains de performances en évitant les communications interprocessus entre le processus d’extension et le processus en arrière-plan qui implémente le service d’application. Voir [Convertir un service d’application pour qu’il s’exécute dans le même processus que son application hôte](convert-app-service-in-process.md) pour découvrir la différence entre un service d’application qui s’exécute en arrière-plan et un service d’application qui s'exécute dans le même processus.

Le système atteint `OnBackgroundActivate()` lorsque le service d’application est activé. Ce code définit les gestionnaires d’événements pour gérer l’appel au service d’application réel lorsqu’il se produit (`OnAppServiceRequestReceived()`) et traiter les événements de nettoyage, comme faire en sorte qu’un objet de différé gère une annulation ou un événement fermé.

_App.xaml.cs dans le projet MathExtension._
```cs
protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    base.OnBackgroundActivated(args);

    if ( _appServiceInitialized == false ) // Only need to setup the handlers once
    {
        _appServiceInitialized = true;

        IBackgroundTaskInstance taskInstance = args.TaskInstance;
        taskInstance.Canceled += OnAppServicesCanceled;

        AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
        _appServiceDeferral = taskInstance.GetDeferral();
        _appServiceConnection = appService.AppServiceConnection;
        _appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
        _appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
    }
}
```

Le code qui effectue le travail de l’extension se trouve dans `OnAppServiceRequestReceived()`. Cette fonction est appelée lorsque le service d’application est appelé pour effectuer un calcul. Elle extrait les valeurs nécessaires de la classe **ValueSet**. Si elle peut faire le calcul, elle place le résultat sous une clé nommée **Result**, dans la classe **ValueSet** qui est renvoyée à l’hôte. Il faut se rappeler que selon le protocole défini pour le mode de communication de cet hôte et de ses extensions, la présence d’une clé **Result** indique une réussite. Dans le cas contraire, c'est un échec.

_App.xaml.cs dans le projet MathExtension._
```cs
private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below (SendResponseAsync()) to respond to the message
    // and we don't want this call to get cancelled while we are waiting.
    AppServiceDeferral messageDeferral = args.GetDeferral();
    ValueSet message = args.Request.Message;
    ValueSet returnMessage = new ValueSet();

    double? arg1 = Convert.ToDouble(message["arg1"]);
    double? arg2 = Convert.ToDouble(message["arg2"]);
    if (arg1.HasValue && arg2.HasValue)
    {
        returnMessage.Add("Result", Math.Pow(arg1.Value, arg2.Value)); // For this sample, the presence of a "Result" key will mean the call succeeded
    }

    await args.Request.SendResponseAsync(returnMessage);
    messageDeferral.Complete();
}
```

## <a name="manage-extensions"></a>Gérer les extensions

Maintenant que nous avons vu comment implémenter la relation entre un hôte et ses extensions, voyons comment un hôte recherche les extensions installées sur le système et réagit à l’ajout et à la suppression de packages contenant des extensions.

Le MicrosoftStore propose des extensions sous forme de packages. La classe [AppExtensionCatalog](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions.appextensioncatalog) recherche les packages installés qui contiennent des extensions correspondant au nom du contrat d’extension de l’hôte et fournit des événements qui se déclenchent lorsqu’un package d’extension d’application pertinent pour l’hôte est installé ou supprimé.

Dans l’exemple de code, la classe `ExtensionManager` (définie dans **ExtensionManager.cs** dans le projet **MathExtensionHost**) encapsule la logique de chargement des extensions et de réponse aux installations et désinstallations de packages d’extension.

Le constructeur `ExtensionManager` utilise `AppExtensionCatalog` pour rechercher sur le système les extensions d’application qui ont le même nom de contrat d’extension que l’hôte:

_ExtensionManager.cs dans le projet MathExtensionHost._
```cs
public ExtensionManager(string extensionContractName)
{
   // catalog & contract
   ExtensionContractName = extensionContractName;
   _catalog = AppExtensionCatalog.Open(ExtensionContractName);
   ...
}
```

Lorsqu’un package d’extension est installé, `ExtensionManager` rassemble des informations sur les extensions dans le package qui ont le même nom de contrat d’extension que l’hôte. Une installation peut représenter une mise à jour, auquel les informations de l’extension affectée sont mises à jour. Lorsqu’un package d’extension est désinstallé, `ExtensionManager` supprime les informations sur les extensions affectées afin que l’utilisateur puisse identifier les extensions qui ne sont plus disponibles.

La classe `Extension` (définie dans **ExtensionManager.cs** dans le projet **MathExtensionHost**) a été créée pour l’exemple de code afin d’accéder à l’ID, à la description, au logo et aux informations spécifique à l'application d’une extension, par exemple si l’utilisateur a activé l’extension.

Dire que l’extension est chargée (voir `Load()` dans **ExtensionManager.cs**) signifie que l’état du package est correct et que nous avons obtenu son ID, son logo, sa description et son dossier public (que nous n’utilisons pas dans cet exemple, c'est juste pour indiquer comment vous l’obtenez). Le package d’extension lui-même n’est pas chargé.

Le concept de déchargement est utilisé pour suivre les extensions qui ne doivent plus être présentées à l’utilisateur.

`ExtensionManager` fournit une collection d’instances `Extension`, de sorte que les extensions, leurs noms, leurs descriptions et leurs logos peuvent être liés aux données de l’interface utilisateur. La page **ExtensionsTab** est liée à cette collection et fournit une interface utilisateur pour activer/désactiver et supprimer les extensions.

![Exemple d’interface utilisateur pour l’onglet Extensions](images/mathextensionhost-extensiontab.png)

 Lorsqu’une extension est supprimée, le système demande à l’utilisateur de confirmer la désinstallation du package qui contient l’extension (et éventuellement d’autres extensions). Si l’utilisateur accepte, le package est désinstallé et `ExtensionManager` supprime les extensions du package désinstallé de la liste des extensions disponibles pour l’application hôte.

 ![Désinstaller l’interface utilisateur](images/mathextensionhost-uninstall.png)

## <a name="debugging-app-extensions-and-hosts"></a>Débogage des extensions d’application et des hôtes

Souvent, l’hôte d’extension et l’extension ne font pas partie de la même solution. Dans ce cas, pour déboguer l’hôte et l’extension:

1. Chargez votre projet hôte dans une instance de VisualStudio.
2. Chargez votre extension dans une autre instance de VisualStudio.
3. Lancez votre application hôte dans le débogueur.
4. Lancez l’application d’extension dans le débogueur. (Si vous souhaitez déployer l’extension, plutôt que de la déboguer, afin de tester l’événement d’installation du package de l’hôte, sélectionnez **Générer &gt; Déployer la solution** à la place).

Maintenant, vous serez en mesure d’atteindre les points d’arrêt dans l’hôte et l’extension.
Si vous démarrez le débogage de l’application d’extension, une fenêtre vide apparaîtra pour l’application. Si vous ne souhaitez pas voir cette fenêtre vide, vous pouvez modifier les paramètres de débogage pour le projet d’extension afin de ne pas lancer l’application, mais plutôt de la déboguer au démarrage (cliquez avec le bouton droit sur le projet d’extension, **Propriétés** > **Débogage** > sélectionnez **Ne pas lancer, mais déboguer mon code au démarrage**). Vous devrez toujours démarrer le débogage (**F5**) du projet d’extension, mais il attendra jusqu'à ce que l’hôte active l'extension, ce qui fait que vos points d'arrêt dans l'extension seront atteints.

**Déboguer l’exemple de code**

Dans l’exemple de code, l’hôte et l’extension sont dans la même solution. Pour déboguer, procédez comme suit:

1. Vérifiez que **MathExtensionHost** est le projet de démarrage (cliquez avec le bouton droit sur le projet **MathExtensionHost**, puis sur **Définir comme projet de démarrage**).
2. Insérez un point d’arrêt sur `Invoke` dans ExtensionManager.cs, dans le projet **MathExtensionHost**.
3. Appuyez sur **F5** pour exécuter le projet **MathExtensionHost**.
4. Insérez un point d’arrêt sur `OnAppServiceRequestReceived` dans App.xaml.cs, dans le projet **MathExtension**.
5. Démarrez le débogage du projet **MathExtension** (cliquez avec le bouton droit sur le projet **MathExtension**, **Déboguer> Démarrer une nouvelle instance**), ce qui va le déployer et déclencher l’événement d'installation du package dans l’hôte.
6. Dans l'application **MathExtensionHost**, accédez à la page **Calcul**, puis cliquez sur **x^y** pour activer l’extension. Le point d’arrêt `Invoke()` est atteint en premier, et le service d’application de l’extension est appelé. Ensuite, la méthode `OnAppServiceRequestReceived()` dans l’extension est atteinte, et le service d’application calcule le résultat et le renvoie.

**Résolution des problèmes liés aux extensions implémentées comme un service d’application**

Si votre hôte d’extension a des difficultés pour se connecter au service d’application pour votre extension, vérifiez que l’attribut `<uap:AppService Name="...">` correspond au contenu que vous avez placé dans votre élément `<Service>`. S’ils ne correspondent pas, le nom du service fourni par votre extension à l’hôte ne correspondra pas au nom du service d’application que vous avez implémenté, et l’hôte ne sera pas en mesure d’activer votre extension.

_Package.appxmanifest dans le projet MathExtension:_
```xml
<Extensions>
   <uap:Extension Category="windows.appService">
     <uap:AppService Name="com.microsoft.sqrtservice" />      <!-- This must match the contents of <Service>...</Service> -->
   </uap:Extension>
   <uap3:Extension Category="windows.appExtension">
     <uap3:AppExtension Name="com.microsoft.mathext" Id="sqrt" DisplayName="Sqrt(x)" Description="Square root" PublicFolder="Public">
       <uap3:Properties>
         <Service>com.microsoft.powservice</Service>   <!-- this must match <uap:AppService Name=...> -->
       </uap3:Properties>
     </uap3:AppExtension>
   </uap3:Extension>
</Extensions>   
```

## <a name="a-checklist-of-basic-scenarios-to-test"></a>Liste de contrôle des scénarios de base à tester

Lorsque vous créez un hôte d’extension et que vous êtes prêt à tester la façon dont il prend en charge les extensions, voici quelques scénarios de base à essayer:

- Exécutez l’hôte, puis déployez une application d’extension.  
    - L’hôte détecte-t-il les nouvelles extensions qui sont présentes pendant son exécution?  
- Déployez l'application d’extension, puis déployez et exécutez l’hôte.
    - L’hôte détecte-t-il les extensions qui existaient déjà?  
- Exécutez l’hôte, puis supprimez l’application d’extension.
    - L’hôte détecte-t-il la suppression correctement?
- Exécutez l’hôte, puis mettez à jour l’application d’extension pour une version plus récente.
    - L’hôte détecte-t-il la modification et décharge-t-il les anciennes versions de l’extension correctement?  

**Scénarios avancés à tester:**

- Exécutez l’hôte, déplacez l’application d’extension vers un support amovible, puis retirez le support.
    - L’hôte détecte-t-il la modification de l’état du package et désactive-t-il l’extension?
- Exécutez l’hôte, puis corrompez l’application d’extension (en la rendant non valide, en la signant différemment, etc.).
    - L’hôte détecte-t-il l’extension falsifiée et la gère-t-il correctement?
- Exécutez l’hôte, puis déployez une application d’extension qui possède un contenu ou des propriétés non valides.
    - L’hôte détecte-t-il le contenu non valide et le gère-t-il correctement?

## <a name="design-considerations"></a>Considérations relatives à la conception

- Fournissez une interface utilisateur qui indique à l’utilisateur les extensions disponibles et lui permet de les activer/désactiver. Vous pouvez également envisager d'ajouter des glyphes pour les extensions, qui deviennent indisponibles lorsqu'un package passe hors ligne, etc.
- Dirigez l’utilisateur vers l’endroit où il peut obtenir des extensions. Votre page d’extension peut peut-être fournir une requête de recherche MicrosoftStore qui affiche la liste des extensions qui peuvent être utilisées avec votre application.
- Envisagez la façon d’avertir l’utilisateur de l’ajout et de la suppression des extensions. Vous pouvez créer une notification pour l’installation d'une nouvelle extension et inviter l’utilisateur à l’activer. Les extensions doivent être désactivées par défaut afin que les utilisateurs puissent les contrôler.

## <a name="how-app-extensions-differ-from-optional-packages"></a>En quoi les extensions d’application diffèrent des packages facultatifs

Les principales différences entre les [packages facultatifs](https://docs.microsoft.com/windows/uwp/packaging/optional-packages) et les extensions d’application sont un écosystème ouvert par rapport à un écosystème fermé, et un package dépendant par rapport à un package indépendant.

Les extensions d’application font partie d’un écosystème ouvert. Si votre application peut héberger des extensions d’application, n'importe qui peut écrire une extension pour votre hôte dans la mesure où elle se conforme à votre méthode de transmission/réception des informations à partir de l’extension. Cela diffère des packages facultatifs qui font partie d’un écosystème fermé où l’éditeur décide qui est autorisé à créer un package facultatif qui peut être utilisé avec l’application.

Les extensions d’application sont des packages indépendants et peuvent être des applications autonomes. Elles ne peuvent pas avoir une dépendance de déploiement sur une autre application.Les packages facultatifs requièrent le package principal et ne peuvent pas s’exécuter sans ce dernier.

Un pack d’extension pour un jeu serait parfaitement adapté pour un package facultatif, car il est étroitement lié au jeu, il ne peut pas s’exécuter indépendamment du jeu et vous pouvez ne pas vouloir que les kits d’extension soit créés par n’importe quel développeur de l’écosystème.

Si ce même jeu avait des extensions ou des thèmes d’interface utilisateur personnalisables, alors une extension d’application pourrait être un bon choix, car l’application fournissant l’extension pourrait s’exécuter seule et n'importe quel tiers pourrait en créer.

## <a name="remarks"></a>Remarques

Cette rubrique présente les extensions d’application. Les principaux points à prendre en compte sont la création de l’hôte et son marquage en tant que tel dans son fichier Package.appxmanifest, la création de l'extension et son marquage en tant que telle dans son fichier Package.appxmanifest, la détermination du mode d’implémentation de l’extension (comme un service d’application, une tâche en arrière-plan ou toute autre façon), la définition du mode de communication de l’hôte avec les extensions et l’utilisation de l’[API AppExtensions](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions) pour accéder et gérer les extensions.

## <a name="related-topics"></a>Rubriques associées

* [Présentation des extensions d’application](https://blogs.msdn.microsoft.com/appinstaller/2017/05/01/introduction-to-app-extensions/)
* [Session de la Build2016 sur les extensions d’application](https://channel9.msdn.com/Events/Build/2016/B808)
* [Exemple de code d’extension d’application de la Build2016](https://github.com/Microsoft/App-Extensibility-Sample)
* [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md)
* [Comment créer et utiliser un service d’application](how-to-create-and-consume-an-app-service.md).
* [Espace de noms AppExtensions](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)
* [Étendre votre application avec des services, des extensions et des packages](https://docs.microsoft.com/windows/uwp/launch-resume/extend-your-app-with-services-extensions-packages)
