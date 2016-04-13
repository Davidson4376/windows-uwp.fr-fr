---
Déclarer des tâches en arrière-plan dans le manifeste de l’application
Activez l’utilisation des tâches en arrière-plan en les déclarant comme extensions dans le manifeste de l’application.
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
---

# Déclarer des tâches en arrière-plan dans le manifeste de l’application


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**Schéma BackgroundTasks**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

Activez l’utilisation des tâches en arrière-plan en les déclarant comme extensions dans le manifeste de l’application.

Les tâches en arrière-plan doivent être déclarées dans le manifeste de l’application. Autrement, votre application n’est pas en mesure de les inscrire (une exception est levée). De plus, les tâches en arrière-plan doivent être déclarées dans le manifeste de l’application pour réussir la certification.

Cette rubrique suppose que vous avez créé une ou plusieurs classes de tâche en arrière-plan et que votre application inscrit chaque tâche en arrière-plan à exécuter en réponse à un déclencheur au minimum.

## Ajouter manuellement les extensions


Ouvrez le manifeste de l’application (Package.appxmanifest) et accédez à l’élément Application. Créez un élément Extensions (s’il n’en existe pas).

L’extrait de code suivant provient de l’[exemple de tâche en arrière-plan](http://go.microsoft.com/fwlink/p/?LinkId=618666) :

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

1.  Modifiez l’attribut EntryPoint de sorte que votre code utilise la même chaîne comme point d’entrée lors de l’inscription de votre tâche en arrière-plan (**namespace.classname**).

    Dans cet exemple, le point d’entrée est ExampleBackgroundTaskNameSpace.ExampleBackgroundTaskClassName :

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

    Cet extrait de code montre que des déclencheurs d’événements système et des notifications Push sont utilisés :

    ```xml
                <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
                  <BackgroundTasks>
                    <Task Type="systemEvent" />
                    <Task Type="pushNotification" />
                  </BackgroundTasks>
                </Extension>
                ```

    > **Note**  Normally, an app will run in a special process called "BackgroundTaskHost.exe". It is possible to add an Executable element to the Extension element, allowing the background task to run in the context of the app. Only use the Executable element with background tasks that require it, such as the [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).    

## Add Additional Background Task Extensions


Repeat step 2 for each additional background task class registered by your app.

The following example is the complete Application element from the [background task sample]( http://go.microsoft.com/fwlink/p/?linkid=227509). This shows the use of 2 background task classes with a total of 3 trigger types. Copy the Extensions section of this example, and modify it as needed, to declare background tasks in your application manifest.

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

## Rubriques connexes

* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Recommandations pour les tâches en arrière-plan](guidelines-for-background-tasks.md)

 

 





<!--HONumber=Mar16_HO1-->


