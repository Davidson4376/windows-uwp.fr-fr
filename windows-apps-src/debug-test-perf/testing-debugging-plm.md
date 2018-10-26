---
author: PatrickFarley
description: Outils et techniques pour le débogage et le test de fonctionnement de votre application avec la Gestion de la durée de vie des processus.
title: Outils de test et de débogage pour la Gestion de la durée de vie des processus (PLM)
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: 8ac6d127-3475-4512-896d-80d1e1d66ccd
ms.localizationpriority: medium
ms.openlocfilehash: 92d03ce30443f6efe8b19f4938b35d4040d7ea70
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5565892"
---
# <a name="testing-and-debugging-tools-for-process-lifetime-management-plm"></a>Outils de test et de débogage pour la Gestion de la durée de vie des processus (PLM)

L’une des principales différences entre les applications UWP et les applications de bureau traditionnelles est que les applications UWP se trouvent dans un conteneur d’application soumis à la Gestion de la durée de vie des processus (PLM). Les applications UWP peuvent être suspendues, reprises ou arrêtées par le service Runtime Broker sur l’ensemble des plateformes; des outils dédiés peuvent forcer ces transitions lorsque vous testez ou déboguez le code qui les gère.

## <a name="features-in-visual-studio-2015"></a>Fonctionnalités de VisualStudio2015

Le débogueur intégré à VisualStudio2015 peut vous aider à résoudre les problèmes potentiels pouvant survenir lors de l’utilisation des fonctionnalités propres à UWP. Vous pouvez forcer votre application à adapter différents états PLM à l’aide de la barre d’outils **Événements de cycle de vie**, qui devient visible lorsque vous exécutez et déboguez votre titre.

![Barre d’outils Événements de cycle de vie](images/gs-debug-uwp-apps-001.png)

## <a name="the-plmdebug-tool"></a>L’outil PLMDebug

PLMDebug.exe est un outil de ligne de commande qui vous permet de contrôler l’état PLM d’un package d’application; l’outil est inclus dans le SDKWindows. Après son installation, l’outil se trouve à l’emplacement *C:\Program Files (x86) \Windows Kits\10\Debuggers\x64* par défaut. 

PLMDebug vous permet également de désactiver les PLM pour un quelconque package d’application installé, ce qui est nécessaire pour certains débogueurs. La désactivation de la Gestion de la durée de vie des processus empêche le service Runtime Broker d’arrêter votre application avant que vous puissiez la déboguer. Pour désactiver la Gestion de la durée de vie des processus, utilisez le commutateur **/enableDebug**, suivi par le *nom complet du package* de votre application UWP (le nom court, le nom de famille ou l’AUMID du package ne fonctionneront pas):

```
plmdebug /enableDebug [PackageFullName]
```

Suite au déploiement de votre application UWP à partir de VisualStudio, le nom complet du package s’affiche dans la fenêtre Sortie. Vous pouvez également récupérer le nom complet du package en exécutant **Get-AppxPackage** dans une console PowerShell.

![Exécution de Get-AppxPackage](images/gs-debug-uwp-apps-003.png)

Vous pouvez éventuellement spécifier un chemin d’accès absolu à un débogueur qui se lance automatiquement lorsque votre package d’application est activé. Si vous souhaitez le faire à l’aide de VisualStudio, vous devez spécifier VSJITDebugger.exe en tant que débogueur. Toutefois, pour utiliser VSJITDebugger.exe, vous devez spécifier le commutateur «-p», ainsi que l’ID du processus (PID) de l’application UWP. Étant donné qu’il est impossible de connaître le PID de votre application UWP au préalable, ce scénario n’est pas réalisable pour l’opération standard.

Vous pouvez contourner cette limitation en écrivant un script ou un outil qui identifie le processus de votre jeu; l’interpréteur de commandes exécute ensuite VSJITDebugger.exe, en passant le PID de votre application UWP. L’exemple de code C# suivant illustre une approche simple pour y parvenir.

```
using System.Diagnostics;

namespace VSJITLauncher
{
    class Program
    {
        static void Main(string[] args)
        {
            // Name of UWP process, which can be retrieved via Task Manager.
            Process[] processes = Process.GetProcessesByName(args[0]);

            // Get PID of most recent instance
            // Note the highest PID is arbitrary. Windows may recycle or wrap the PID at any time.
            int highestId = 0;
            foreach (Process detectedProcess in processes)
            {
                if (detectedProcess.Id > highestId)
                    highestId = detectedProcess.Id;
            }

            // Launch VSJITDebugger.exe, which resides in C:\Windows\System32
            ProcessStartInfo startInfo = new ProcessStartInfo("vsjitdebugger.exe", "-p " + highestId);
            startInfo.UseShellExecute = true;

            Process process = new Process();
            process.StartInfo = startInfo;
            process.Start();
        }
    }
}
```

Exemple d’utilisation, conjointement avec PLMDebug:

```
plmdebug /enableDebug 279f7062-ce35-40e8-a69f-cc22c08e0bb8_1.0.0.0_x86__c6sq6kwgxxfcg "\"C:\VSJITLauncher.exe\" Game"
```
où `Game` est le nom du processus et `279f7062-ce35-40e8-a69f-cc22c08e0bb8_1.0.0.0_x86__c6sq6kwgxxfcg` est le nom complet du package de l’exemple de package d’application UWP.

Notez que chaque appel à **/enableDebug** doit ensuite être associé à un autre appel PLMDebug avec le commutateur **/disableDebug**. En outre, le chemin d’accès à un débogueur doit être absolu (les chemins d’accès relatifs ne sont pas pris en charge).

## <a name="related-topics"></a>Rubriques connexes
- [Déploiement et débogage des applications UWP](deploying-and-debugging-uwp-apps.md)
- [Débogage, tests et analyse des performances](index.md)
