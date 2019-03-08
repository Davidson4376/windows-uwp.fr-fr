---
title: Résolution des problèmes de l’API des Services Xbox Live
description: Découvrez comment enregistrer des informations d’erreur supplémentaires lors de la résolution des problèmes avec l’API Live Xbox.
ms.assetid: 3827bba1-902f-4f2d-ad51-af09bd9354c4
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, dépannage, erreur, journaux
ms.localizationpriority: medium
ms.openlocfilehash: 67735d83e5ff301ee4434a6917fce814cabe8309
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621414"
---
# <a name="troubleshooting-the-xbox-live-apis"></a>Résolution des problèmes de l’API de Xbox Live

## <a name="code"></a>Code

Il est difficile de diagnostiquer un échec à l’aide d’uniquement l’erreur à partir de la couche API de Services Xbox Live. Informations d’erreur supplémentaires, telles que la journalisation de tous les appels RESTful, peut être disponible pour le serveur. Pour écouter ces données, raccorder l’enregistreur d’événements réponse et activer le suivi du débogage. Journalisation de réponse vous permet de voir le trafic et web service codes de réponse HTTP, qui est souvent aussi utile qu’une trace Fiddler.

### <a name="c"></a>C++

L’exemple de code suivant active la journalisation de réponse et définit le niveau d’erreur de débogage à détaillée (vous pouvez également définir le niveau d’erreur de débogage à l’erreur pour afficher uniquement des appels de trace ou Off pour désactiver le suivi). La sortie de débogage qui en résulte est envoyée vers le volet de sortie lors de l’exécution de votre projet dans Visual Studio.  

```cpp

        // Set up debug tracing to the Output window in Visual Studio.
            xbox::services::system::xbox_live_services_settings::get_singleton_instance()->set_diagnostics_trace_level(
                xbox_services_diagnostics_trace_level::verbose
                );
```

Vous pouvez également choisir de rediriger la sortie de débogage vers votre propre fichier journal comme suit :

```cpp

        // Set up debug tracing of the Xbox Live Services API traffic to the game UI.
        m_xboxLiveContext->Settings->EnableServiceCallRoutedEvents = true;
        m_xboxLiveContext->Settings->ServiceCallRouted += ref new
        Windows::Foundation::EventHandler<Microsoft::Xbox::Services::XboxServiceCallRoutedEventArgs^>(
            [=] ( Platform::Object^, Microsoft::Xbox::Services::XboxServiceCallRoutedEventArgs^ args )
            {
                gameUI->Log(L"[URL]: " + args->HttpMethod + " " + args->Url->AbsoluteUri);
                gameUI->Log(L"");
                gameUI->Log(L"[Response]: " + args->HttpStatus.ToString() + " " + args->ResponseBody);
            });

```
