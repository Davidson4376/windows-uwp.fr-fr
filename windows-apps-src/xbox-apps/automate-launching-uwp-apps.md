---
title: Automatisation du lancement des applications UWP Windows10
description: Les développeurs peuvent utiliser l’activation de protocole et de lancement pour automatiser le lancement de leurs jeux ou apps UWP pour les tests automatisés.
ms.localizationpriority: medium
ms.openlocfilehash: 123e2dfff909265673a711f480f5fe636590afa4
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7852123"
---
# <a name="automate-launching-windows-10-uwp-apps"></a>Automatisation du lancement des applications UWP Windows10

## <a name="introduction"></a>Introduction

Plusieurs options s’offrent aux développeurs qui souhaitent automatiser le lancement des applications de plateforme Windows universelle (UWP). Dans ce livre blanc, nous allons étudier les méthodes de lancement d’une application à l’aide de l’activation de protocole et de l’activation de lancement.

*L’activation de protocole* permet à une application de s’enregistrer en tant que gestionnaire pour un protocole donné. 

*L’activation de lancement* correspond au lancement normal de l’application, par exemple le lancement à partir de la vignette de l’application.

Pour chaque méthode d’activation, vous avez la possibilité d’utiliser la ligne de commande ou une application de lancement. Quelle que soit la méthode d’activation, si l’application est en cours d’exécution, l’activation amènera l’application au premier plan (ce qui la réactive) et fournira les nouveaux arguments d’activation. Cela offre la possibilité d’utiliser les commandes de l’activation pour fournir de nouveaux messages à l’application. Il est important de noter que le projet doit être compilé et déployé pour que la méthode d’activation exécute l’application récemment mise à jour. 

## <a name="protocol-activation"></a>Activation de protocole

Suivez ces étapes pour configurer l’activation de protocole pour les applications: 

1. Ouvrez le fichier **Package.appxmanifest** dans Visual Studio.
2. Sélectionnez l’onglet **Déclarations**.
3. Dans la liste déroulante **Déclarations disponibles**, sélectionnez **Protocole**, puis **Ajouter**.
4. Sous **Propriétés**, dans le champ **Nom**, saisissez un nom unique pour lancer l’application. 

    ![Activation de protocole](images/automate-uwp-apps-1.png)

5. Enregistrez le fichier et déployez le projet. 
6. Une fois le projet déployé, vous devez définir l’activation de protocole. 
7. Accédez à **Panneau de configuration\Tous les éléments du panneau de configuration\Programmes par défaut** et sélectionnez **Associer un type de fichier ou un protocole à un programme spécifique**. Faites défiler jusqu’à la section **Protocoles** pour voir si le protocole est répertorié. 

Maintenant que l’activation de protocole est configurée, vous avez deux options pour l’activation de l’application à l’aide du protocole (la ligne de commande ou une application de lancement). 

### <a name="command-line"></a>Ligne de commande

L’application peut être activée par protocole à l’aide de la ligne de commande incluant la commande start, suivie du nom de protocole défini précédemment, du signe deux-points («:») et des paramètres. Les paramètres peuvent être n’importe quelle chaîne arbitraire; toutefois, pour tirer parti des fonctionnalités d’URI (Uniform Resource Identifier), il est conseillé de respecter le format URI standard: 

  ```
  scheme://username:password@host:port/path.extension?query#fragment
  ```

L’objet URI est capable d’analyser une chaîne URI dans ce format. Pour plus d’informations, voir [Classe URI (MSDN)](https://msdn.microsoft.com/library/windows/apps/windows.foundation.uri.aspx). 

Exemples:

  ```
  >start bingnews:
  >start myapplication:protocol-parameter
  >start myapplication://single-player/level3?godmode=1&ammo=200
  ```

L’activation de protocole par ligne de commande prend en charge les caractères Unicode jusqu'à un maximum de 2038caractères sur l’URI brut. 

### <a name="launcher-application"></a>Application de lancement

Pour le lancement, créez une application distincte qui prend en charge l’API WinRT. Le code C++ de lancement via l’activation de protocole dans un programme de lancement est présenté dans l’exemple suivant, où **PackageURI** est l’URI pour l’application avec n’importe quels arguments ; par exemple `myapplication:` ou `myapplication:protocol activation arguments`.

```
bool ProtocolLaunchURI(Platform::String^ URI)
{
       IAsyncOperation<bool>^ protocolLaunchAsyncOp;
       try
       {
              protocolLaunchAsyncOp = Windows::System::Launcher::LaunchUriAsync(ref new 
Uri(URI));
       }
       catch (Platform::Exception^ e)
       {
              Platform::String^ dbgStr = "ProtocolLaunchURI Exception Thrown: " 
+ e->ToString() + "\n";
              OutputDebugString(dbgStr->Data());
              return false;
       }

       concurrency::create_task(protocolLaunchAsyncOp).wait();

       if (protocolLaunchAsyncOp->Status == AsyncStatus::Completed)
       {
              bool LaunchResult = protocolLaunchAsyncOp->GetResults();
              Platform::String^ dbgStr = "ProtocolLaunchURI " + URI 
+ " completed. Launch result " + LaunchResult + "\n";
              OutputDebugString(dbgStr->Data());
              return LaunchResult;
       }
       else
       {
              Platform::String^ dbgStr = "ProtocolLaunchURI " + URI + " failed. Status:" 
+ protocolLaunchAsyncOp->Status.ToString() + " ErrorCode:" 
+ protocolLaunchAsyncOp->ErrorCode.ToString() + "\n";
              OutputDebugString(dbgStr->Data());
              return false;
       }
}
```
L’activation de protocole avec l’application de lancement présente les mêmes limites d’arguments que l’activation de protocole à l’aide de la ligne de commande. Les deux prennent en charge les caractères Unicode jusqu'à un maximum de 2038caractères sur l’URI brut. 

## <a name="launch-activation"></a>Activation de lancement

Vous pouvez également lancer l’application à l’aide de l’activation de lancement. Aucune configuration n’est requise, mais l’ID de modèle utilisateur de l’application UWP est nécessaire. L’ID de modèle utilisateur de l’application correspond au nom de la famille de packages suivi d’un point d’exclamation et de l’ID d’application. 

Pour obtenir le nom de la famille de packages, la meilleure solution consiste à suivre les étapes suivantes:

1. Ouvrez le fichier**Package.appxmanifest**.
2. Dans l’onglet **Packages**, entrez le **nom du package**.

    ![Activation de lancement](images/automate-uwp-apps-2.png)

3. Si le **nom de la famille de packages** n’apparaît pas dans la liste, ouvrez PowerShell et exécutez `>get-appxpackage MyPackageName` pour rechercher la propriété **PackageFamilyName**.

L’ID d’application se trouve dans le fichier **Package.appxmanifest** (ouvert dans la vue XML) sous l’élément `<Applications>`.

### <a name="command-line"></a>Ligne de commande

Un outil permettant d’effectuer l’activation de lancement d’une application UWP est installé avec le Kit de développement logiciel Windows10. Il peut être exécuté à partir de la ligne de commande et utilise l’ID de modèle utilisateur de l’application comme argument pour le lancement.

```
C:\Program Files (x86)\Windows Kits\10\App Certification Kit\microsoft.windows.softwarelogo.appxlauncher.exe <AUMID>
```

Voici à quoi cela devrait ressembler:

```
"C:\Program Files (x86)\Windows Kits\10\App Certification Kit\microsoft.windows.softwarelogo.appxlauncher.exe" MyPackageName_ph1m9x8skttmg!AppId
```

Cette option ne prend pas en charge les arguments de ligne de commande. 

### <a name="launcher-application"></a>Application de lancement

Pour le lancement, vous pouvez créer une application distincte qui prend en charge l’utilisation de COM. L’exemple suivant montre le code C++ pour le lancement avec l’activation de lancement dans un programme de lancement. Grâce à ce code, vous pouvez créer un objet **ApplicationActivationManager** et appeler **ActivateApplication** en passant l’ID de modèle utilisateur de l’application trouvé précédemment et les arguments. Pour plus d’informations sur les autres paramètres, voir [Méthode IApplicationActivationManager::ActivateApplication (MSDN)](https://msdn.microsoft.com/library/windows/desktop/hh706903(v=vs.85).aspx).

```
#include <ShObjIdl.h>
#include <atlbase.h>

HRESULT LaunchApp(LPCWSTR AUMID)
{
     HRESULT hr = CoInitializeEx(nullptr, COINIT_APARTMENTTHREADED);
     if (FAILED(hr))
     {
            wprintf(L"LaunchApp %s: Failed to init COM. hr = 0x%08lx \n", AUMID, hr);
     }
     {
            CComPtr<IApplicationActivationManager> AppActivationMgr = nullptr;
            if (SUCCEEDED(hr))
            {
                   hr = CoCreateInstance(CLSID_ApplicationActivationManager, nullptr,  
CLSCTX_LOCAL_SERVER, IID_PPV_ARGS(&AppActivationMgr));
                   if (FAILED(hr))
                   {
                         wprintf(L"LaunchApp %s: Failed to create Application Activation 
Manager. hr = 0x%08lx \n", AUMID, hr);
                   }
            }
            if (SUCCEEDED(hr))
            {
                   DWORD pid = 0;
                   hr = AppActivationMgr->ActivateApplication(AUMID, nullptr, AO_NONE, 
&pid);
                   if (FAILED(hr))
                   {
                         wprintf(L"LaunchApp %s: Failed to Activate App. hr = 0x%08lx 
\n", AUMID, hr);
                   }
            }
     }
     CoUninitialize();
     return hr;
}
```

Il est intéressant de noter que cette méthode prend en charge les arguments passés, contrairement à la méthode de lancement précédente (à l’aide de la ligne de commande).

## <a name="accepting-arguments"></a>Acceptation des arguments

Pour accepter des arguments passés lors de l’activation de l’application UWP, vous devez ajouter du code à l’application. Pour déterminer si une activation de protocole ou une activation de lancement s’est produite, remplacez l’événement **OnActivated** et vérifiez le type d’argument afin d’obtenir la chaîne brute ou les valeurs de l’objet URI avant analyse. 

L’exemple suivant explique comment obtenir la chaîne brute.

```
void OnActivated(IActivatedEventArgs^ args)
{
        // Check for launch activation
        if (args->Kind == ActivationKind::Launch)
        {
            auto launchArgs = static_cast<LaunchActivatedEventArgs^>(args); 
            Platform::String^ argval = launchArgs->Arguments;
            // Manipulate arguments …
        }

        // Check for protocol activation
        if (args->Kind == ActivationKind::Protocol)
        {
            auto protocolArgs = static_cast< ProtocolActivatedEventArgs^>(args);
            Platform::String^ argval = protocolArgs->Uri->ToString();
            // Manipulate arguments …
        }
}
```

## <a name="summary"></a>Résumé
En résumé, vous pouvez utiliser différentes méthodes pour lancer l’application UWP. Selon la configuration requise et les cas d’usage, certaines méthodes peuvent être mieux adaptées que d’autres. 

## <a name="see-also"></a>Voir également
- [UWP sur XboxOne](index.md)

