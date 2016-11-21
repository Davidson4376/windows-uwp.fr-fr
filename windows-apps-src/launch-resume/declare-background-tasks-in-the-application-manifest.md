---
author: TylerMSFT
title: "Déclarer des tâches en arrière-plan dans le manifeste de l’application"
description: "Activez l’utilisation des tâches en arrière-plan en les déclarant comme extensions dans le manifeste de l’application."
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
translationtype: Human Translation
ms.sourcegitcommit: 7d1c160f8b725cd848bf8357325c6ca284b632ae
ms.openlocfilehash: b3518780600b9fe8f9be5af48eb5ee6022ec350f

---

# Déclarer des tâches en arrière-plan dans le manifeste de l’application


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**Schéma BackgroundTasks**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

Activez l’utilisation des tâches en arrière-plan en les déclarant comme extensions dans le manifeste de l’application.

> [!Important]
>  Cet article est spécifique aux tâches en arrière-plan hors processus. Les tâches en arrière-plan in-process ne sont pas déclarées dans le manifeste.

Les tâches en arrière-plan hors processus doivent être déclarées dans le manifeste de l’application. Autrement, votre application n’est pas en mesure de les inscrire (une exception est levée). De plus, les tâches en arrière-plan hors processus doivent être déclarées dans le manifeste de l’application pour réussir la certification.

Cette rubrique suppose que vous avez créé une ou plusieurs classes de tâche en arrière-plan et que votre application inscrit chaque tâche en arrière-plan à exécuter en réponse à un déclencheur au minimum.

## Ajouter manuellement les extensions


Ouvrez le manifeste de l’application (Package.appxmanifest) et accédez à l’élément Application. Créez un élément Extensions (s’il n’en existe pas).

L’extrait de code suivant provient de l’[exemple de tâche en arrière-plan](http://go.microsoft.com/fwlink/p/?LinkId=618666):

```xml
<Application Id="App"
   ...
   <Extensions>
     <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.SampleBackgroundTask">
       <BackgroundTasks>
         <Task Type="systemEvent" />
         <Task Type="timer" />
       </BackgroundTasks>
     </Extension>
     <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ServicingComplete">
       <BackgroundTasks>
         <Task Type="systemEvent"/>
       </BackgroundTasks>
     </Extension>
   </Extensions>
 </Application>
```

## Ajouter une extension de tâche en arrière-plan


Déclarez votre première tâche en arrière-plan.

Copiez ce code dans l’élément Extensions (vous ajouterez des attributs aux étapes suivantes).

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="">
      <BackgroundTasks>
        <Task Type="" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```

1.  Modifiez l’attribut EntryPoint afin que votre code utilise la même chaîne comme point d’entrée lors de l’inscription de votre tâche en arrière-plan (**namespace.classname**).

    Dans cet exemple, le point d’entrée est ExampleBackgroundTaskNameSpace.ExampleBackgroundTaskClassName:

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ExampleBackgroundTaskClassName">
       <BackgroundTasks>
         <Task Type="" />
       </BackgroundTasks>
    </Extension>
</Extensions>
```

2.  Modifiez la liste de l’attribut Task Type pour indiquer le type d’inscription de tâche utilisé avec cette tâche en arrière-plan. Si la tâche en arrière-plan est inscrite avec plusieurs types de déclencheur, ajoutez des éléments Task et des attributs Type supplémentaires pour chacun d’eux.

    **Remarque** Pensez impérativement à répertorier chaque type de déclencheur que vous utilisez, sans quoi la tâche en arrière-plan ne sera pas inscrite avec les types de déclencheur non déclarés (la méthode [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772) échouera et lèvera une exception).

    Cet extrait de code montre que des déclencheurs d’événements système et des notifications Push sont utilisés:

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
    <BackgroundTasks>
        <Task Type="systemEvent" />
        <Task Type="pushNotification" />
    </BackgroundTasks>
</Extension>
```


## Ajouter des extensions de tâche en arrière-plan supplémentaires

Répétez l’étape2 pour chaque classe de tâche en arrière-plan supplémentaire inscrite par votre application.

L’exemple suivant représente l’élément Application complet de l’[exemple de tâche en arrière-plan]( http://go.microsoft.com/fwlink/p/?linkid=227509). Il illustre l’utilisation de deuxclasses de tâche en arrière-plan avec au total troistypes de déclencheur. Copiez la section Extensions de cet exemple et modifiez-la si nécessaire pour déclarer des tâches en arrière-plan dans le manifeste de l’application.

```xml
<Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="BackgroundTask.App">
        <uap:VisualElements
          DisplayName="BackgroundTask"
          Square150x150Logo="Assets\StoreLogo-sdk.png"
          Square44x44Logo="Assets\SmallTile-sdk.png"
          Description="BackgroundTask"

          BackgroundColor="#00b2f0">
          <uap:LockScreen Notification="badgeAndTileText" BadgeLogo="Assets\smalltile-Windows-sdk.png" />
            <uap:SplashScreen Image="Assets\Splash-sdk.png" />
            <uap:DefaultTile DefaultSize="square150x150Logo" Wide310x150Logo="Assets\tile-sdk.png" >
                <uap:ShowNameOnTiles>
                    <uap:ShowOn Tile="square150x150Logo" />
                    <uap:ShowOn Tile="wide310x150Logo" />
                </uap:ShowNameOnTiles>
            </uap:DefaultTile>
        </uap:VisualElements>

      <Extensions>
        <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.SampleBackgroundTask">
          <BackgroundTasks>
            <Task Type="systemEvent" />
            <Task Type="timer" />
          </BackgroundTasks>
        </Extension>
        <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ServicingComplete">
          <BackgroundTasks>
            <Task Type="systemEvent"/>
          </BackgroundTasks>
        </Extension>
      </Extensions>
    </Application>
</Applications>
```

## Déclarer votre tâche en arrière-plan pour qu’elle s’exécute dans un autre processus

Cette nouvelle fonctionnalité de Windows10 version1507 vous permet d’exécuter votre tâche en arrière-plan dans un autre processus que BackgroundTaskHost.exe (le processus dans lequel les tâches en arrière-plan s’exécutent par défaut).  Vous avez deux options: exécuter la tâche dans le même processus que votre application au premier plan, ou exécuter la tâche dans une instance de BackgroundTaskHost.exe distincte des autres instances de tâches en arrière-plan de la même application.  

### Exécuter la tâche dans l’application au premier plan

Voici un exemple de code XML déclarant une tâche en arrière-plan qui s’exécute dans le même processus que l’application au premier plan. Notez l’attribut `Executable`:

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="ExecModelTestBackgroundTasks.ApplicationTriggerTask" Executable="$targetnametoken$.exe">
        <BackgroundTasks>
            <Task Type="systemEvent" />
        </BackgroundTasks>
    </Extension>
</Extensions>
```

> [!Note]
> Utilisez uniquement l’élément Executable avec les tâches en arrière-plan qui le requièrent, telles que [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).  

### Exécuter la tâche dans un processus hôte en arrière-plan différent

Voici un exemple de code XML déclarant une tâche en arrière-plan qui s’exécute dans un processus BackgroundTaskHost.exe distinct des autres instances de tâches en arrière-plan de la même application. Notez l’attribut `ResourceGroup`, qui définit quelles tâches en arrière-plan vont s’exécuter en même temps.

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.SessionConnectedTriggerTask" ResourceGroup="foo">
      <BackgroundTasks>
        <Task Type="systemEvent" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimeZoneTriggerTask" ResourceGroup="foo">
      <BackgroundTasks>
        <Task Type="systemEvent" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimerTriggerTask" ResourceGroup="bar">
      <BackgroundTasks>
        <Task Type="timer" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.ApplicationTriggerTask" ResourceGroup="bar">
      <BackgroundTasks>
        <Task Type="general" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.MaintenanceTriggerTask" ResourceGroup="foobar">
      <BackgroundTasks>
        <Task Type="general" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```


## Rubriques connexes


* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Recommandations pour les tâches en arrière-plan](guidelines-for-background-tasks.md)



<!--HONumber=Nov16_HO1-->


