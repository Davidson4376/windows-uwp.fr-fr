---
author: joannaleecy
title: Ajouter des fonctionnalités de démonstration (RDX) de vente au détail à votre application
description: Préparer votre application pour le mode de démonstration commerciale, aidant à la promotion de votre application sur le sol de vente au détail.
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.author: joanlee
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, application de démonstration commerciale
ms.localizationpriority: medium
ms.openlocfilehash: 152c775c1b69bfd82d8969aed7e638f98646bdd7
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4500272"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>Ajouter des fonctionnalités de démonstration (RDX) de vente au détail à votre application

Inclure un mode de démonstration commerciale de votre application Windows afin que les clients qui tester le PC et appareils sur le sol vente peuvent aller à droite dans.

Lorsque les clients sont dans un magasin de vente au détail, ils s’attend à pouvoir tester les démonstrations du PC et appareils. Ils passent d’un bloc considérable du moment avec des applications par le biais de l' [expérience de démonstration commerciale (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience).

Vous pouvez configurer votre application pour proposer des expériences différentes tandis que dans les modes _normal_ ou de _vente au détail_ . Par exemple, si votre application commence par un processus d’installation, vous pourrez ignorer au-delà de celle-ci en mode commercial et préremplir l’application avec les paramètres par défaut et de données exemple afin qu’ils peuvent lancer directement.

Du point de vue de nos clients, il n'existe qu’une seule application. Pour aider les clients à faire la distinction entre les deux modes, nous recommandons que lorsque votre application est en mode commercial, elle affiche le mot «Commercial» met en évidence dans la barre de titre ou dans un emplacement approprié.

Outre les exigences de Microsoft Store pour les applications, les applications prenant en charge RDX doivent également être compatibles avec le programme d’installation RDX, le nettoyage et les processus de mise à jour pour garantir que les clients ont une expérience homogène et positive en magasin.

## <a name="design-principles"></a>Principes de conception

* **Afficher vos points forts**. Utilisez l’expérience de démonstration commerciale pour montrer forts de votre application. C’est probablement la première fois que votre client verra votre application, donc la montrer le sous son meilleur jour.

* **Affichez-les rapidement**. Les clients sont parfois impatients. Plus un utilisateur peut constater rapidement les points forts de votre application, mieux c’est.

* **Simplifiez le**. L’expérience de démonstration commerciale sert de révélateur gouverne de valeur de votre application.

* **Le focus sur l’expérience**. Donnez à l’utilisateur le temps d’assimiler votre contenu. S’il est essentiel de les amener rapidement à découvrir les points forts, il faut également aménager des pauses pour leur permettre de profiter pleinement de l’expérience.

## <a name="technical-requirements"></a>Exigences techniques

Applications prenant en charge RDX sont destinées à mettre le meilleur de votre application pour les clients au détail, elles doivent répondre aux exigences techniques et se conforment aux réglementations confidentialité du Microsoft Store pour toutes les applications de démonstration commerciale.

Cela peut être utilisé comme une liste de vérification pour vous aider à préparer le processus de validation et aussi clair que possible le processus de test. Notez que ces exigences doivent être satisfaites non seulement pendant le processus de validation, mais également pendant toute la durée de vie de l’application de démonstration commerciale tant qu’elle est exécutée sur les appareils de démonstration commerciale.

### <a name="critical-requirements"></a>Exigences critiques

Les applications prenant en charge RDX qui ne répondent pas à ces exigences critiques seront supprimées à partir de tous les appareils de démonstration commerciale dès que possible.

* **Ne demandez pas informations d’identification personnelle (informations d’identification personnelle)**. Cela inclut les informations de connexion, les informations de compte Microsoft ou contact détails.

* **Exempt d’erreurs de rencontrer**. Votre application doit s’exécuter sans erreur. En outre, aucune fenêtre ou notification d’erreur ne doit s’afficher lorsque les clients utilisent les appareils de démonstration commerciale. Erreurs impact négatif sur l’application elle-même, votre marque, la marque de l’appareil, marque du fabricant de l’appareil et marque de Microsoft.

* **Les applications payé doivent proposer un mode d’évaluation**. Votre application doit être un libre ou inclure un [mode d’évaluation](https://msdn.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app). Les clients ne souhaitent pas payer pour une expérience en magasin.

### <a name="high-priority-requirements"></a>Exigences de priorité élevée

Les applications prenant en charge RDX qui ne répondent pas à ces exigences de priorité élevée doivent être examinées en vue immédiatement d’un correctif. En l’absence de correctif applicable, cette application sera probablement supprimée des appareils de démonstration commerciale.

* **Memorable expérience en mode hors connexion**. Votre application doit fournir une excellente expérience hors connexion dans la mesure environ 50 % des appareils sont hors connexion dans les magasins. Vous devez vous assurer que les clients qui interagissent avec votre application hors connexion vivent une expérience positive et significative.

* **Expérience de contenu mis à jour**. Votre application ne doit jamais être invite les mises à jour en ligne. Si les mises à jour sont nécessaires, ils doivent être effectuées en mode silencieux.

* **Aucune communication anonyme**. Dans la mesure où un client à l’aide d’un appareil de démonstration commerciale est un utilisateur anonyme, ils ne doivent pas être en mesure de messages ou partager du contenu à partir de l’appareil.

* **Créer des expériences cohérentes à l’aide du processus de nettoyage**. Chaque client doit vivre la même expérience lorsqu’il utilise un appareil de démonstration commerciale. Votre application doit utiliser le [processus de nettoyage](#clean-up-process) pour revenir à l’état par défaut même après chaque utilisation. Nous ne voulons pas le client suivant pour voir ce que le dernier client a. Cela inclut les scores, les réussites et les déverrouillages.

* **Contenu approprié par âge**. Tous les contenus d’application doivent être affecté une catégorie adolescents ou inférieure évaluation. Pour en savoir plus, voir [l’obtention de votre application est classifiée iarc](https://www.globalratings.com/for-developers.aspx) et [les classifications ESRB](https://www.esrb.org/ratings/ratings_guide.aspx).

### <a name="medium-priority-requirements"></a>Exigences de priorité moyenne

L’équipe commerciale Windows Store peut contacter directement les développeurs pour discuter de la manière de corriger ces problèmes.

* **Capacité à s’exécuter correctement sur une gamme d’appareils**. Les applications doivent s’exécuter correctement sur tous les appareils, y compris les appareils ayant des spécifications bas de gamme. Si l’application est installée sur les appareils qui ne satisfaisant pas les spécifications minimales requises, l’application doit clairement en informer l’utilisateur. La configuration minimale requise pour l’appareil doit être connue des clients afin qu’elle soit toujours exécutée avec des performances élevées.

* **Répondre aux exigences de taille application commerciale**. L’application doit être inférieure à 800Mo. Contactez l’équipe commerciale Windows Store directement pour en discuter si votre application prenant en charge RDX ne satisfait pas les exigences de taille.

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>RetailInfo API: Préparation de votre code pour le mode de démonstration

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
La propriété [**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) dans la classe utilitaire [**RetailInfo**](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo) , qui fait partie de l’espace de noms [Windows.System.Profile](https://docs.microsoft.com/uwp/api/windows.system.profile) dans le SDK Windows 10, est utilisée comme un indicateur booléen pour spécifier le chemin de code que votre application s’exécute sur - la normale de _ _mode ou le mode _commercial_ .

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

#### <a name="idl"></a>FICHIER IDL

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

Nettoyage commence deux minutes après qu’un acheteur s’arrête d’interagir avec l’appareil. La démonstration commerciale est lu, et Windows commence la réinitialisation des exemples de données dans les contacts, photos et d’autres applications. En fonction de l’appareil, cette opération peut prendre entre 1 à 5 minutes pour tout réinitialiser entièrement retour à la normale. Cela garantit que chaque client dans le magasin de vente au détail peut remonter à un appareil et avoir la même expérience lors de l’interaction avec l’appareil.

Étape 1: nettoyage
* Toutes les applications Win32 et du Windows Store sont fermées
* Tous les fichiers des dossiers connus comme __Images__, __Vidéos__, __Musique__, __Documents__, __Photos enregistrées__, __Pellicule__, __Bureau__ et __Téléchargements__ sont supprimés.
* Les états d’itinérance non structurés et structurés sont supprimés
* Les états locaux structurés sont supprimés

Étape 2: le programme d’installation
* Pour les appareils hors connexion: les dossiers restent vides
* Pour les appareils en ligne: les ressources de démonstration commerciale peuvent être transférées vers l’appareil à partir du MicrosoftStore

### <a name="store-data-across-user-sessions"></a>Stocker des données entre des sessions utilisateur

Pour stocker des données entre des sessions utilisateur, vous pouvez stocker des informations dans __ApplicationData.Current.TemporaryFolder__ comme le processus de nettoyage par défaut ne supprime pas automatiquement les données dans ce dossier. Notez que les informations stockées à l’aide de *LocalState* sont supprimées au cours du processus de nettoyage.

### <a name="customize-the-cleanup-process"></a>Personnaliser le processus de nettoyage

Pour personnaliser le processus de nettoyage, implémentez la `Microsoft-RetailDemo-Cleanup` service d’application dans votre application.

Scénarios exigeant une logique de nettoyage personnalisée inclut l’exécution d’une installation complète, le téléchargement et la mise en cache de données ou cherchant ne pas les données *LocalState* être supprimées.

Étape 1: Déclarez le service _Microsoft-RetailDemo-Cleanup_ dans le manifeste de votre application.
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

Étape 2: Implémentez votre logique de nettoyage personnalisée sous la fonction de cas _AppdataCleanup_ à l’aide de l’exemple de modèle ci-dessous.
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

* [Stocker et récupérer des données d’application](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)
* [Comment créer et utiliser un service d’application](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)
* [Localiser les contenus d’une application](https://msdn.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [Expérience de démonstration commerciale (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)
