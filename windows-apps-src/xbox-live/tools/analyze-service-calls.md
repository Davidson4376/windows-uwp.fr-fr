---
title: Xbox Live Trace Analyzer
description: Découvrez comment utiliser l’Analyseur de Trace Xbox Live pour passer en revue les appels de service effectués par votre titre.
ms.assetid: b4490fae-d554-403d-bbbc-601af38af0ef
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, appels de service, test, analyseur de trace
ms.localizationpriority: medium
ms.openlocfilehash: fa8ca37842edfbeaab0063cd953f3a34358a82da
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623464"
---
# <a name="xbox-live-trace-analyzer"></a>Xbox Live Trace Analyzer

L’API de Services Xbox Live permet désormais aux développeurs de titre de capturer tous les appels de service et les analyser ensuite en mode hors connexion pour toutes les violations dans des modèles d’appel. Suivi des appels de service peut être activé à l’aide de nouvelles fonctionnalités disponibles dans l’outil de ligne de commande xbtrace, ou via l’activation de protocole pour des scénarios plus avancés. Activation du suivi directement à partir de code de titre des appels de service est également pris en charge. Vous trouverez l’outil d’analyse hors connexion, appelé l’Analyseur de Trace Xbox Live (XBLTraceAnalyzer.exe) en tant que partie du package à partir de Xbox Live Tools [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools).


## <a name="gather-logs-and-analyze-the-service-calls"></a>Collecter des journaux et analyser les appels de service

Les étapes suivantes sont nécessaires pour collecter les journaux qui contiennent l’enregistrement de vos appels de service et de les analyser à l’aide d’analyseur de Trace Xbox Live.

1.  Générer votre titre à l’aide de la version de la Xbox Live Services API qui est inclus dans le juillet 2015 ou une version plus récente du Kit de développeur Xbox (XDK).
2.  Modifier votre titre pour activer le suivi, comme décrit ci-dessous.
3.  Déployer votre titre.
4.  Lancer le titre et effectuer au moins un appel à Xbox Live Services afin d’initialiser l’API de Services Xbox Live.
5.  Démarrer le suivi au niveau du point dans votre titre, que vous souhaitez analyser.
6.  Arrêter le suivi.
7.  Exécutez l’outil Analyseur de Trace Xbox Live sur votre PC de développement et d’afficher la sortie.

## <a name="starting-and-stopping-tracing"></a>Démarrage et le suivi de l’arrêt

Il existe trois façons de démarrer et arrêter le suivi :

1.  Vous pouvez appeler un ensemble d’API de Services Xbox Live directement à partir de votre titre.
2.  Vous pouvez utiliser la *xbtrace* outil de ligne de commande.
3.  Vous pouvez utiliser l’activation du protocole via la *gestion des applications (xbapp.exe)* outil de ligne de commande.


### <a name="starting-and-stopping-tracing-directly-from-your-title"></a>Démarrage et suivi d’arrêt directement à partir de votre titre

Pour démarrer le suivi directement à partir de votre titre, vous devez procédez comme suit :

1.  Dans le `Microsoft::Xbox::Services::Experimental` espace de noms, définissez le `EnableServiceCallTracking` propriété de la `ServiceCallTrackerSettings` classe la valeur true.
2.  Appelez `StartServiceCallTracking()` pour démarrer le suivi des appels de service.
3.  Appelez `StopServiceCallTracking()` pour arrêter le suivi des appels de service.
4.  Une fois que le suivi est arrêté, copiez le fichier de suivi obtenu à partir du lecteur de travail de développeur sur la console à votre PC à l’aide *copie de fichiers (xbcp.exe)* ou le *Xbox un voisinage* de analyser en utilisant l’Analyseur de Trace Xbox Live.

### <a name="starting-and-stopping-tracing-by-using-the-xbtrace-command-line-tool"></a>Démarrage et le suivi de l’arrêt à l’aide de l’outil de ligne de commande xbtrace

La plus pratique et simple pour démarrer le suivi consiste à utiliser l’outil de ligne de commande xbtrace avec le type de trace xboxliveservices. Lorsque vous utilisez xbTrace, le fichier de trace qui en résulte est copié au PC pour vous.

Démarrage et arrêt des traces à l’aide de xbtrace s’appuie sur l’activation de protocole. Avant d’utiliser xbtrace pour démarrer et arrêter le suivi, vous devez initialiser l’activation de protocole en appelant le `RegisterForProtcolActivation` méthode sur le `ServiceCallTrackerSettings` classe.

L’exemple suivant montre comment démarrer et arrêter une trace de Xbox Live Services à l’aide de xbTrace :

    xbtrace start xboxliveservices
    xbtrace stop


N’oubliez pas que le titre doit être en cours d’exécution et l’activation de protocole doit être initialisée avant de pouvoir démarrer et arrêter le suivi avec xbtrace. Une fois que le suivi est arrêté, xbtrace copie le fichier de trace à votre PC de développement et le place dans un répertoire dont le nom inclut « xbtrace » et un horodatage. Le nom de ce répertoire peut être substitué à l’aide de \[etlfile\] option à xbtrace.

<a name="starting-and-stopping-tracing-by-using-protocol-activation"></a>Démarrage et le suivi de l’arrêt à l’aide de l’activation de protocole
----------------------------------------------------------
Le suivi peut également être contrôlé en utilisant les fonctionnalités d’activation de protocole de « lancement xbApp ». Vous devez connaître n ° titre de votre titre pour démarrer et arrêter le suivi par le biais de l’activation de protocole. Vous pouvez trouver votre id de titre dans le fichier manifeste de votre titre. Le suivi est contrôlé via les URI qui contiennent le paramètre de « serviceCallTracking ». Les exemples suivants montrent comment démarrer et arrêter le suivi pour un logiciel dont l’id de titre est 12345678 :

    xbapp launch "ms-xbl-12345678://serviceCallTracking?state=start"
    xbapp launch "ms-xbl-12345678://serviceCallTracking?state=stop"

Lorsque vous utilisez l’activation de protocole, le fichier de trace qui en résulte est stocké sur le disque de travail de développeur sur la console. Vous devez copier le fichier à votre PC à l’aide de xbcp ou la Xbox un voisinage. Le fichier n’est pas copié automatiquement sur le PC comme c’est lorsque vous utilisez xbtrace.

L’activation de protocole vous permet de définir les paramètres de suivi supplémentaires, telles que d’un niveau de détail. Quatre niveaux de détail sont prises en charge : quiet, diagnostic, détaillé et minimale. L’exemple suivant montre comment définir un niveau de détail :

    xbapp launch "ms-xbl-12345678://serviceCallTracking?verbosity=diagnostic"

## <a name="analyze-the-trace-file"></a>Analyser le fichier de trace

Une fois que le fichier de trace a été copié sur votre PC, vous pouvez utiliser l’outil Analyseur de Trace Xbox Live sur GNDP pour analyser l’utilisation de votre titre de Services de Xbox Live. Consultez la documentation fournie avec l’outil Analyseur de Trace Xbox Live Game Developer Network pour obtenir une description comment appeler l’outil et interpréter sa sortie. Vous pouvez également exécuter XBLTraceAnalyzer.exe avec l’option de ligne de commande ? ou-h pour afficher l’aide de la ligne de commande.
