---
title: Ajouter des fonctionnalités de démonstration (RDX) de vente au détail à votre application
description: Préparation de votre application pour le mode de démonstration de vente au détail, aidant à présenter votre application sur le lieu de vente au détail.
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp, application de démonstration commerciale
ms.localizationpriority: medium
ms.openlocfilehash: b66435dd7c94762874461b48e19e9a60224f287b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596754"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>Ajouter des fonctionnalités de démonstration (RDX) de vente au détail à votre application

Les clients qui essaient de PC et appareils sur le sol ventes peuvent se rendre directement dans inclut un mode de démonstration de vente au détail dans votre application Windows.

Lorsque les clients se trouvent dans un magasin de vente au détail, ils s’attendent à pouvoir essayer les démonstrations de PC et appareils. Ils passent souvent une partie considérable de leur temps à toucher à des applications via le [démonstration expérience (RDX) de la vente au détail](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience).

Vous pouvez configurer votre application pour fournir des expériences différentes lors dans _normal_ ou _retail_ modes. Par exemple, si votre application commence par un processus d’installation, vous pourrez ignorer au-delà de celle-ci en mode de vente au détail et préremplir l’application avec les paramètres par défaut et les données d’exemple, ils peuvent passer directement dans.

Du point de vue de nos clients, il n'existe qu’une seule application. Pour aider les clients à faire la distinction entre les deux modes, nous recommandons que pendant que votre application est en mode de vente au détail, il affiche le mot « Vente au détail » en premier plan dans la barre de titre ou dans un emplacement approprié.

Outre les exigences de Microsoft Store pour les applications, les applications prenant en charge RDX doivent également être compatibles avec la configuration RDX, le nettoyage et le processus de mise à jour afin de garantir que les clients aient une expérience positive de manière cohérente au niveau du magasin de vente au détail.

## <a name="design-principles"></a>Principes de conception

* **Afficher votre meilleur**. Utilisez l’expérience de démonstration de vente au détail pour showcase pourquoi votre application est absolument exceptionnel. Il s’agit probablement de la première fois votre client s’affiche votre application, par conséquent, leur montrer la pièce de meilleures !

* **Afficher rapide**. Les clients sont parfois impatients. Plus un utilisateur peut constater rapidement les points forts de votre application, mieux c’est.

* **Simplifier l’histoire**. L’expérience de démonstration de vente au détail est un présentation des avantages pour la valeur de votre application.

* **Vous concentrer sur l’expérience**. Donnez à l’utilisateur le temps d’assimiler votre contenu. S’il est essentiel de les amener rapidement à découvrir les points forts, il faut également aménager des pauses pour leur permettre de profiter pleinement de l’expérience.

## <a name="technical-requirements"></a>Spécifications techniques

Comme les applications prenant en charge RDX sont destinées à illustrer le meilleur de votre application aux clients de vente au détail, ils doivent répondre aux exigences techniques et respectez les réglementations de confidentialité ayant le Microsoft Store pour toutes les applications d’expérience de démonstration de vente au détail.

Cela peut être utilisé comme une liste de vérification pour vous aider à préparer pour le processus de validation et à assurer la clarté dans le processus de test. Notez que ces exigences doivent être satisfaites non seulement pendant le processus de validation, mais également pendant toute la durée de vie de l’application de démonstration commerciale tant qu’elle est exécutée sur les appareils de démonstration commerciale.

### <a name="critical-requirements"></a>Exigences critiques

Les applications prenant en charge les RDX qui ne répondent pas à ces exigences critiques seront supprimées de tous les appareils de démonstration de vente au détail dès que possible.

* **Ne pas demander d’informations d’identification personnelle (PII)**. Cela inclut les informations de connexion, les informations de compte Microsoft ou contact détails.

* **Expérience sans erreur**. Votre application doit s’exécuter sans erreur. En outre, aucune fenêtre ou notification d’erreur ne doit s’afficher lorsque les clients utilisent les appareils de démonstration commerciale. Erreurs reflètent avoir un impact négatif sur l’application elle-même, votre marque, marque de l’appareil, marque du fabricant du périphérique et les marque de Microsoft.

* **Applications payantes doivent avoir un mode d’évaluation**. Votre application doit soit être gratuit ou inclure un [mode d’évaluation](https://msdn.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app). Les clients ne souhaitent pas payer pour une expérience en magasin.

### <a name="high-priority-requirements"></a>Exigences de haute priorité

Les applications prenant en charge les RDX qui ne répondent pas à ces exigences de haute priorité doivent être examinées pour un correctif immédiatement. En l’absence de correctif applicable, cette application sera probablement supprimée des appareils de démonstration commerciale.

* **Expérience hors connexion facile à mémoriser**. Votre application a besoin illustrer une excellente expérience hors connexion comme environ 50 % des appareils sont hors connexion à des emplacements de vente au détail. Vous devez vous assurer que les clients qui interagissent avec votre application hors connexion vivent une expérience positive et significative.

* **Mise à jour de contenu**. Votre application ne doit jamais être demande les mises à jour en ligne. Si les mises à jour sont nécessaires, elle doit s’exécuter en mode silencieux.

* **Aucune communication anonyme**. Car un client à l’aide d’un appareil de démonstration de vente au détail est un utilisateur anonyme, ils ne doivent pas être en mesure de message ou le partage de contenu à partir de l’appareil.

* **Fournir une expérience cohérente à l’aide du processus de nettoyage**. Chaque client doit vivre la même expérience lorsqu’il utilise un appareil de démonstration commerciale. Votre application doit utiliser [des processus de nettoyage](#cleanup-process) pour revenir à l’état par défaut après chaque utilisation. Nous ne voulons pas le client suivant pour voir ce que le client dernière abandonnée. Cela inclut les scores, les réussites et les déverrouillages.

* **Âge du contenu approprié**. Tout contenu de l’application doit être assigné un adolescent ou la catégorie de niveau inférieur. Pour plus d’informations, consultez [votre application évalués par IARC](https://www.globalratings.com/for-developers.aspx) et [des évaluations ESRB](https://www.esrb.org/ratings/ratings_guide.aspx).

### <a name="medium-priority-requirements"></a>Exigences de priorité moyenne

L’équipe commerciale Windows Store peut contacter directement les développeurs pour discuter de la manière de corriger ces problèmes.

* **Possibilité d’exécuter correctement sur une gamme d’appareils**. Applications doivent s’exécuter correctement sur tous les appareils, y compris les appareils avec des spécifications de bas de gamme. Si l’application est installée sur les appareils qui ne répondait pas aux spécifications minimales, l’application doit indiquer clairement à l’utilisateur à ce sujet. La configuration minimale requise pour l’appareil doit être connue des clients afin qu’elle soit toujours exécutée avec des performances élevées.

* **Répondre aux exigences de taille application retail store**. L’application doit être inférieure à 800 Mo. Contactez l’équipe de Windows Retail Store directement pour obtenir des informations supplémentaires si votre application prenant en charge les RDX ne remplit pas les exigences de taille.

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>RetailInfo API : Préparation de votre code pour le mode de démonstration

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
Le [ **IsDemoModeEnabled** ](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) propriété dans le [ **RetailInfo** ](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo) classe utilitaire, qui fait partie de la [ Windows.System.Profile](https://docs.microsoft.com/uwp/api/windows.system.profile) espace de noms dans le SDK Windows 10, est utilisé comme un indicateur booléen pour spécifier le chemin d’accès de code que votre application s’exécute sur - le _normal_ mode ou _retail_ mode.

``` csharp
using Windows.Storage;

StorageFolder folder = ApplicationData.Current.LocalFolder;

if (Windows.System.Profile.RetailInfo.IsDemoModeEnabled) 
{
    // Use the demo specific directory
    folder = await folder.GetFolderAsync(“demo”);
}

StorageFile file = await folder.GetFileAsync(“hello.txt”);
// Now read from file
```

``` cpp
using namespace Windows::Storage;

StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;

if (Windows::System::Profile::RetailInfo::IsDemoModeEnabled) 
{
    // Use the demo specific directory
    create_task(localFolder->GetFolderAsync(“demo”).then([this](StorageFolder^ demoFolder)
    {
        return demoFolder->GetFileAsync(“hello.txt”);
    }).then([this](task<StorageFile^> fileTask)
    {
        StorageFile^ file = fileTask.get();
    });
    // Do something with file
}
else
{
    create_task(localFolder->GetFileAsync(“hello.txt”).then([this](StorageFile^ file)
    {
        // Do something with file
    });
}
```

``` javascript
if (Windows.System.Profile.retailInfo.isDemoModeEnabled) {
    console.log(“Retail mode is enabled.”);
} else {
    Console.log(“Retail mode is not enabled.”);
}
```

### <a name="retailinfoproperties"></a>RetailInfo.Properties

Quand [**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) renvoie la valeur true, vous pouvez exécuter une requête concernant différentes propriétés de l’appareil à l’aide de [**RetailInfo.Properties**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.properties) pour créer une expérience de démonstration commerciale plus personnalisée. Ces propriétés incluent [**ManufacturerName**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.manufacturername), [**Screensize**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.screensize), [**Memory**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.memory) et ainsi de suite.

```csharp
using Windows.UI.Xaml.Controls;
using Windows.System.Profile

TextBlock priceText = new TextBlock();
priceText.Text = RetailInfo.Properties[KnownRetailInfo.Price];
// Assume infoPanel is a StackPanel declared in XAML
this.infoPanel.Children.Add(priceText);
```

```cpp
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::System::Profile;

TextBlock ^manufacturerText = ref new TextBlock();
manufacturerText.set_Text(RetailInfo::Properties[KnownRetailInfoProperties::Price]);
// Assume infoPanel is a StackPanel declared in XAML
this->infoPanel->Children->Add(manufacturerText);
```

```javascript
var pro = Windows.System.Profile;
console.log(pro.retailInfo.properties[pro.KnownRetailInfoProperties.price);
```

#### <a name="idl"></a>IDL

```
//  Copyright (c) Microsoft Corporation. All rights reserved.
//
//  WindowsRuntimeAPISet

import "oaidl.idl";
import "inspectable.idl";
import "Windows.Foundation.idl";
#include <sdkddkver.h>

namespace Windows.System.Profile
{
    runtimeclass RetailInfo;
    runtimeclass KnownRetailInfoProperties;

    [version(NTDDI_WINTHRESHOLD), uuid(0712C6B8-8B92-4F2A-8499-031F1798D6EF), exclusiveto(RetailInfo)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    interface IRetailInfoStatics : IInspectable
    {
        [propget] HRESULT IsDemoModeEnabled([out, retval] boolean *value);
        [propget] HRESULT Properties([out, retval, hasvariant] Windows.Foundation.Collections.IMapView<HSTRING, IInspectable *> **value);
    }

    [version(NTDDI_WINTHRESHOLD), uuid(50BA207B-33C4-4A5C-AD8A-CD39F0A9C2E9), exclusiveto(KnownRetailInfoProperties)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    interface IKnownRetailInfoPropertiesStatics : IInspectable
    {
        [propget] HRESULT RetailAccessCode([out, retval] HSTRING *value);
        [propget] HRESULT ManufacturerName([out, retval] HSTRING *value);
        [propget] HRESULT ModelName([out, retval] HSTRING *value);
        [propget] HRESULT DisplayModelName([out, retval] HSTRING *value);
        [propget] HRESULT Price([out, retval] HSTRING *value);
        [propget] HRESULT IsFeatured([out, retval] HSTRING *value);
        [propget] HRESULT FormFactor([out, retval] HSTRING *value);
        [propget] HRESULT ScreenSize([out, retval] HSTRING *value);
        [propget] HRESULT Weight([out, retval] HSTRING *value);
        [propget] HRESULT DisplayDescription([out, retval] HSTRING *value);
        [propget] HRESULT BatteryLifeDescription([out, retval] HSTRING *value);
        [propget] HRESULT ProcessorDescription([out, retval] HSTRING *value);
        [propget] HRESULT Memory([out, retval] HSTRING *value);
        [propget] HRESULT StorageDescription([out, retval] HSTRING *value);
        [propget] HRESULT GraphicsDescription([out, retval] HSTRING *value);
        [propget] HRESULT FrontCameraDescription([out, retval] HSTRING *value);
        [propget] HRESULT RearCameraDescription([out, retval] HSTRING *value);
        [propget] HRESULT HasNfc([out, retval] HSTRING *value);
        [propget] HRESULT HasSdSlot([out, retval] HSTRING *value);
        [propget] HRESULT HasOpticalDrive([out, retval] HSTRING *value);
        [propget] HRESULT IsOfficeInstalled([out, retval] HSTRING *value);
        [propget] HRESULT WindowsVersion([out, retval] HSTRING *value);
    }

    [version(NTDDI_WINTHRESHOLD), static(IRetailInfoStatics, NTDDI_WINTHRESHOLD)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone), static(IRetailInfoStatics, NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    [threading(both)]
    [marshaling_behavior(agile)]
    runtimeclass RetailInfo
    {
    }

    [version(NTDDI_WINTHRESHOLD), static(IKnownRetailInfoPropertiesStatics, NTDDI_WINTHRESHOLD)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone), static(IKnownRetailInfoPropertiesStatics, NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    [threading(both)]
    [marshaling_behavior(agile)]
    runtimeclass KnownRetailInfoProperties
    {
    }
}
```

## <a name="cleanup-process"></a>Processus de nettoyage

Nettoyage commence à deux minutes après le blocage d’un client interagissant avec l’appareil. La démonstration de la vente au détail est lue, et Windows commence à réinitialiser les exemples de données dans les contacts, des photos et des autres applications. En fonction de l’appareil, cette opération peut prendre entre 1-5 minutes tout réinitialiser entièrement à la normale. Cela garantit que chaque client dans le magasin de vente au détail permettre remonter à un appareil et avoir la même expérience lors de l’interaction avec l’appareil.

Étape 1 : Nettoyage
* Toutes les applications Win32 et du Windows Store sont fermées
* Tous les fichiers des dossiers connus comme __Images__, __Vidéos__, __Musique__, __Documents__, __Photos enregistrées__, __Pellicule__, __Bureau__ et __Téléchargements__ sont supprimés.
* Les états d’itinérance non structurés et structurés sont supprimés
* Les états locaux structurés sont supprimés

Étape 2 :  Configurer
* Pour les appareils hors connexion : Les dossiers sont vides
* Pour les appareils en ligne : Ressources de démonstration de vente au détail peuvent être transmises à l’appareil à partir du Microsoft Store

### <a name="store-data-across-user-sessions"></a>Store les données entre les sessions de l’utilisateur

Pour stocker des données entre les sessions de l’utilisateur, vous pouvez stocker des informations dans __ApplicationData.Current.TemporaryFolder__ en tant que la valeur par défaut des processus de nettoyage ne supprime pas automatiquement les données dans ce dossier. Notez que les informations stockées à l’aide *LocalState* est supprimé au cours du processus de nettoyage.

### <a name="customize-the-cleanup-process"></a>Personnaliser le processus de nettoyage

Pour personnaliser le processus de nettoyage, implémentez la `Microsoft-RetailDemo-Cleanup` service d’application dans votre application.

Les scénarios où une logique de nettoyage personnalisé est nécessaire, vous devez exécuter une installation complète, téléchargement et la mise en cache des données ou souhaitant ne pas *LocalState* données à supprimer.

Étape 1 : Déclarez le _-RetailDemo-nettoyage de Microsoft_ service dans votre manifeste d’application.
``` CSharp
  <Applications>
      <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MyCompany.MyApp.RDXCustomCleanupTask">
          <uap:AppService Name="Microsoft-RetailDemo-Cleanup" />
        </uap:Extension>
      </Extensions>
   </Application>
  </Applications>

```

Étape 2 : Implémenter votre logique de nettoyage personnalisé sous le _AppdataCleanup_ fonction cas à l’aide de l’exemple de modèle ci-dessous.
``` CSharp
using System;
using System.IO;
using System.Runtime.Serialization.Json;
using System.Threading;
using System.Threading.Tasks;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;
using Windows.Storage;

namespace MyCompany.MyApp
{
    public sealed class RDXCustomCleanupTask : IBackgroundTask
    {
        BackgroundTaskCancellationReason _cancelReason = BackgroundTaskCancellationReason.Abort;
        BackgroundTaskDeferral _deferral = null;
        IBackgroundTaskInstance _taskInstance = null;
        AppServiceConnection _appServiceConnection = null;

        const string MessageCommand = "Command";

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get the deferral object from the task instance, and take a reference to the taskInstance;
            _deferral = taskInstance.GetDeferral();
            _taskInstance = taskInstance;
            _taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

            AppServiceTriggerDetails appService = _taskInstance.TriggerDetails as AppServiceTriggerDetails;
            if ((appService != null) && (appService.Name == "Microsoft-RetailDemo-Cleanup"))
            {
                _appServiceConnection = appService.AppServiceConnection;
                _appServiceConnection.RequestReceived += _appServiceConnection_RequestReceived;
                _appServiceConnection.ServiceClosed += _appServiceConnection_ServiceClosed;
            }
            else
            {
                _deferral.Complete();
            }
        }

        void _appServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
        {
        }

        async void _appServiceConnection_RequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            //Get a deferral because we will be calling async code
            AppServiceDeferral requestDeferral = args.GetDeferral();
            string command = null;
            var returnData = new ValueSet();

            try
            {
                ValueSet message = args.Request.Message;
                if (message.ContainsKey(MessageCommand))
                {
                    command = message[MessageCommand] as string;
                }

                if (command != null)
                {
                    switch (command)
                    {
                        case "AppdataCleanup":
                            {
                                // Do custom clean up logic here
                                break;
                            }
                    }
                }
            }
            catch (Exception e)
            {
            }
            finally
            {
                requestDeferral.Complete();
                // Also release the task deferral since we only process one request per instance.
                _deferral.Complete();
            }
        }

        private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            _cancelReason = reason;
        }
    }
}
```

## <a name="related-links"></a>Liens connexes

* [Store et récupérer des données d’application](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)
* [Comment créer et consommer un service d’application](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)
* [Localisation de contenu de l’application](https://msdn.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [Expérience de démonstration de vente au détail (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)
