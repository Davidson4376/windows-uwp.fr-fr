---
author: mtoepke
title: Marble Maze application structure
description: The structure of a DirectX Universal Windows Platform (UWP) app differs from that of a traditional desktop application.
ms.assetid: 6080f0d3-478a-8bbe-d064-73fd3d432074
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 84dbe1730274bd39b1ba359c588bc68976a2d19e

---

# Marble Maze application structure


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


The structure of a DirectX Universal Windows Platform (UWP) app differs from that of a traditional desktop application. Instead of working with handle types such as **HWND** and functions such as **CreateWindow**, the Windows Runtime provides interfaces such as [**Windows::UI::Core::ICoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208296) so that you can develop UWP apps in a more modern, object-oriented manner. This section of the documentation shows how the Marble Maze application code is structured.

> **Note**   The sample code that corresponds to this document is found in the [DirectX Marble Maze game sample](http://go.microsoft.com/fwlink/?LinkId=624011).

 
## 
Here are some of the key points that this document discusses for when you structure your game code:

-   In the initialization phase, set up runtime and library components that your game uses. Also load game-specific resources.
-   UWP apps must start processing events within 5 seconds of launch. Therefore, load only essential resources when you load your app. Games should load large resources in the background and display a progress screen.
-   In the game loop, respond to Windows events, read user input, update scene objects, and render the scene.
-   Use event handlers to respond to window events. (These replace the window messages from desktop Windows applications.)
-   Use a state machine to control the flow and order of the game logic.

##  File organization


Some of the components in Marble Maze can be reused with any game with little or no modification. For your own game, you can adapt the organization and ideas that these files provide. The following table briefly describes the important source code files.

| Files                                      | Description                                                                                                                                                                          |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Audio.h, Audio.cpp                         | Defines the **Audio** class, which manages audio resources.                                                                                                                          |
| BasicLoader.h, BasicLoader.cpp             | Defines the **BasicLoader** class, which provides utility methods that help you load textures, meshes, and shaders.                                                                  |
| BasicMath.h                                | Defines structures and functions that help you work with vector and matrix data and computations. Many of these functions are compatible with HLSL shader types.                     |
| BasicReaderWriter.h, BasicReaderWriter.cpp | Defines the **BasicReaderWriter** class, which uses the Windows Runtime to read and write file data in a UWP app.                                                                    |
| BasicShapes.h, BasicShapes.cpp             | Defines the **BasicShapes** class, which provides utility methods for creating basic shapes such as cubes and spheres. (These files are not used by the Marble Maze implementation). |
| BasicTimer.h, BasicTimer.cpp               | Defines the **BasicTimer** class, which provides an easy way to get total and elapsed times.                                                                                         |
| Camera.h, Camera.cpp                       | Defines the **Camera** class, which provides the position and orientation of a camera.                                                                                               |
| Collision.h, Collision.cpp                 | Manages collision info between the marble and other objects, such as the maze.                                                                                                       |
| DDSTextureLoader.h, DDSTextureLoader.cpp   | Defines the **CreateDDSTextureFromMemory** function, which loads textures that are in .dds format from a memory buffer.                                                              |
| DirectXApp.h, DirectXApp.cpp               | Defines the **DirectXApp** and **DirectXAppSource** classes, which encapsulate the view (window, thread, and events) of the app.                                                     |
| DirectXBase.h, DirectXBase.cpp             | Defines the **DirectXBase** class, which provides infrastructure that is common to many DirectX UWP apps.                                                                            |
| DirectXSample.h                            | Defines utility functions that can be used by DirectX UWP apps.                                                                                                                      |
| LoadScreen.h, LoadScreen.cpp               | Defines the **LoadScreen** class, which displays a loading screen during app initialization.                                                                                         |
| MarbleMaze.h, MarbleMaze.cpp               | Defines the **MarbleMaze** class, which manages game-specific resources and defines much of the game logic.                                                                          |
| MediaStreamer.h, MediaStreamer.cpp         | Defines the **MediaStreamer** class, which uses Media Foundation to help the game manage audio resources.                                                                            |
| PersistentState.h, PersistentState.cpp     | Defines the **PersistentState** class, which reads and writes primitive data types from and to a backing store.                                                                      |
| Physics.h, Physics.cpp                     | Defines the **Physics** class, which implements the physics simulation between the marble and the maze.                                                                              |
| Primitives.h                               | Defines geometric types that are used by the game.                                                                                                                                   |
| SampleOverlay.h, SampleOverlay.cpp         | Defines the **SampleOverlay** class, which provides common 2-D and user-interface data and operations.                                                                               |
| SDKMesh.h, SDKMesh.cpp                     | Defines the **SDKMesh** class, which loads and renders meshes that are in SDK Mesh (.sdkmesh) format.                                                                                |
| UserInterface.h, UserInterface.cpp         | Defines functionality that's related to the user interface, such as the menu system and the high score table.                                                                        |

 

##  Design-time versus run-time resource formats


When you can, use run-time formats instead of design-time formats to more efficiently load game resources.

A *design-time* format is the format you use when you design your resource. Typically, 3-D designers work with design-time formats. Some design-time formats are also text-based so that you can modify them in any text-based editor. Design-time formats can be verbose and contain more information than your game requires. A *run-time* format is the binary format that is read by your game. Run-time formats are typically more compact and more efficient to load than the corresponding design-time formats. This is why the majority of games use run-time assets at run time.

Although your game can directly read a design-time format, there are several benefits to using a separate run-time format. Because run-time formats are often more compact, they require less disk space and require less time to transfer over a network. Also, run-time formats are often represented as memory-mapped data structures. Therefore, they can be loaded into memory much faster than, for example, an XML-based text file. Finally, because separate run-time formats are typically binary-encoded, they are more difficult for the end-user to modify.

HLSL shaders are one example of resources that use different design-time and run-time formats. Marble Maze uses .hlsl as the design-time format, and .cso as the run-time format. A .hlsl file holds source code for the shader; a .cso file holds the corresponding shader byte code. When you convert .hlsl files offline and provide .cso files with your game, you avoid the need to convert HLSL source files to byte code when your game loads.

For instructional reasons, the Marble Maze project includes both the design-time format and the run-time format for many resources, but you only have to maintain the design-time formats in the source project for your own game because you can convert them to run-time formats when you need them. This documentation shows how to convert the design-time formats to the run-time formats.

##  Application life cycle


Marble Maze follows the life cycle of a typical UWP app. For more info about the life cycle of a UWP app, see [App lifecycle](https://msdn.microsoft.com/library/windows/apps/mt243287).

When a UWP game initializes, it typically initializes runtime components such as Direct3D, Direct2D, and any input, audio, or physics libraries that it uses. It also loads game-specific resources that are required before the game begins. This initialization occurs one time during a game session.

After initialization, games typically run the *game loop*. In this loop, games typically perform four actions: process Windows events, collect input, update scene objects, and render the scene. When the game updates the scene, it can apply the current input state to the scene objects and simulate physical events, such as object collisions. The game can also perform other activities such as playing sound effects or sending data over the network. When the game renders the scene, it captures the current state of the scene and draws it to the display device. The following sections describe these activities in greater detail.

##  Adding to the template


The *DirectX 11 App (Universal Windows)* template creates a core window that you can render to with Direct3D. The template also includes the **DeviceResources** class that creates all of the Direct3D device resources needed for rendering 3D content in a UWP app. The **AppMain** class creates the **MarbleMaze** class object, starts the loading of resources, loops to update the timer, and calls the **MarbleMaze** render method each frame. The **CreateWindowSizeDependentResources**, Update, and Render methods for this class call the corresponding methods in the **MarbleMaze** class. The following example shows where the **AppMain** constructor creates the **MarbleMaze** class object. The device resources class is passed to the class so it can use the Direct3D objects for rendering.

```cpp
    m_marbleMaze = std::unique_ptr<MarbleMaze>(new MarbleMaze(m_deviceResources));
    m_marbleMaze->CreateWindowSizeDependentResources();
```

The **AppMain** class also starts loading the deferred resources for the game. See the next section for more detail. The **DirectXPage** constructor sets up the event handlers, creates the **DeviceResources** class, and creates the **AppMain** class.

When the handlers for these events are called, they pass the input to the **MarbleMaze** class.

## Loading game assets in the background


To ensure that your game can respond to window events within 5 seconds after it is launched, we recommend that you load your game assets asynchronously, or in the background. As assets load in the background, your game can respond to window events.

> **Note**  You can also display the main menu when it is ready, and allow the remaining assets to continue loading in the background. If the user selects an option from the menu before all resources are loaded, you can indicate that scene resources are continuing to load by displaying a progress bar, for example.

 

Even if your game contains relatively few game assets, it is good practice to load them asynchronously for two reasons. One reason is that it is difficult to guarantee that all of your resources will load quickly on all devices and all configurations. Also, by incorporating asynchronous loading early, your code is ready to scale as you add functionality.

Asynchronous asset loading begins with the **AppMain::Load** method. This method uses the [**task Class (Concurrency Runtime)**](https://msdn.microsoft.com/library/windows/apps/hh750113.aspx) class to load game assets in the background.

```cpp
    task<void>([=]()
    {
        m_marbleMaze->LoadDeferredResources();
    });

```

The **MarbleMaze** class defines the *m\_deferredResourcesReady* flag to indicate that asynchronous loading is complete. The **MarbleMaze::LoadDeferredResources** method loads the game resources and then sets this flag. The update (**MarbleMaze::Update**) and render (**MarbleMaze::Render**) phases of the app check this flag. When this flag is set, the game continues as normal. If the flag is not yet set, the game shows the loading screen.

For more information about asynchronous programming for UWP apps, see [Asynchronous programming in C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

>> > **Tip**   If you’re writing game code that is part of a Windows Runtime C++ Library (in other words, a DLL), consider whether to read [Creating Asynchronous Operations in C++ for Windows Store Apps](https://msdn.microsoft.com/library/windows/apps/hh750113.aspx) to learn how to create asynchronous operations that can be consumed by apps and other libraries.

 

## The game loop


The **DirectPage::OnRendering** method runs the main game loop. This method is called every frame.

To help separate view and window code from game-specific code, we implemented the **DirectXApp::Run** method to forward update and render calls to the **MarbleMaze** object. The **DirectPage::OnRendering** method also defines the game timer, which is used for animation and physics simulation.

The following example shows the **DirectPage::OnRendering** method, which includes the main game loop. The game loop updates the total time and frame time variables, and then updates and renders the scene. This also makes sure that content is only rendered when the window is visible.

```cpp

void DirectXPage::OnRendering(Object^ sender, Object^ args)
{
    if (m_windowVisible)
    {
        m_main->Update();

        if (m_main->Render())
        {
            m_deviceResources->Present();
        }
    }
}
```

## The state machine


Games typically contain a *state machine* (also known as a *finite state machine*, or FSM) to control the flow and order of the game logic. A state machine contains a given number of states and the ability to transition among them. A state machine typically starts from an *initial* state, transitions to one or more *intermediate* states, and possibly ends at a *terminal* state.

A game loop often uses a state machine so that it can perform the logic that is specific to the current game state. Marble Maze defines the **GameState** enumeration, which defines each possible state of the game.

```cpp
enum class GameState
{
    Initial,
    MainMenu,
    HighScoreDisplay,
    PreGameCountdown,
    InGameActive,
    InGamePaused,
    PostGameResults,
};
```

The **MainMenu** state, for example, defines that the main menu appears, and that the game is not active. Conversely, the **InGameActive** state defines that the game is active, and that the menu does not appear. The **MarbleMaze** class defines the **m\_gameState** member variable to hold the active game state.

The **MarbleMaze::Update** and **MarbleMaze::Render** methods use the switch statement to perform logic for the current state. The following example shows what this switch statement might look like for the **MarbleMaze::Update** method (details are removed to illustrate the structure).

```cpp
switch (m_gameState)
{
case GameState::MainMenu:
    // Do something with the main menu. 
    break;

case GameState::HighScoreDisplay:
    // Do something with the high-score table. 
    break;

case GameState::PostGameResults:
    // Do something with the game results. 
    break;

case GameState::InGamePaused:
    // Handle the paused state. 
    break;
}
```

When game logic or rendering depends on a specific game state, we emphasize it in this documentation.

## Handling app and window events


The Windows Runtime provides an object-oriented event-handling system so that you can more easily manage Windows messages. To consume an event in an application, you must provide an event handler, or event-handling method, that responds to the event. You must also register the event handler with the event source. This process is often referred to as event wiring.

### Supporting suspend, resume, and restart

Marble Maze is suspended when the user switches away from it or when Windows enters a low power state. The game is resumed when the user moves it to the foreground or when Windows comes out of a low power state. Generally, you don't close apps. Windows can terminate the app when it's in the suspended state and Windows requires the resources, such as memory, that the app is using. Windows notifies an app when it is about to be suspended or resumed, but it doesn't notify the app when it's about to be terminated. Therefore, your app must be able to save—at the point when Windows notifies your app that it is about to be suspended—any data that would be required to restore the current user state when the app is restarted. If your app has significant user state that is expensive to save, you may also need to save state regularly, even before your app receives the suspend notification. Marble Maze responds to suspend and resume notifications for two reasons:

1.  When the app is suspended, the game saves the current game state and pauses audio playback. When the app is resumed, the game resumes audio playback.
2.  When the app is closed and later restarted, the game resumes from its previous state.

Marble Maze performs the following tasks to support suspend and resume:

-   It saves its state to persistent storage at key points in the game, such as when the user reaches a checkpoint.
-   It responds to suspend notifications by saving its state to persistent storage.
-   It responds to resume notifications by loading its state from persistent storage. It also loads the previous state during startup.

To support suspend and resume, Marble Maze defines the **PersistentState** class. (See PersistentState.h and PersistentState.cpp). This class uses the [**Windows::Foundation::Collections::IPropertySet**](https://msdn.microsoft.com/library/windows/apps/br226054) interface to read and write properties. The **PersistentState** class provides methods that read and write primitive data types (such as **bool**, **int**, **float**, [**XMFLOAT3**](https://msdn.microsoft.com/library/windows/desktop/ee419475), and [**Platform::String**](https://msdn.microsoft.com/library/windows/apps/hh755812.aspx)), from and to a backing store.

```cpp
ref class PersistentState
{
public:
    void Initialize(
        _In_ Windows::Foundation::Collections::IPropertySet^ m_settingsValues,
        _In_ Platform::String^ key
        );

    void SaveBool(Platform::String^ key, bool value);
    void SaveInt32(Platform::String^ key, int value);
    void SaveSingle(Platform::String^ key, float value);
    void SaveXMFLOAT3(Platform::String^ key, DirectX::XMFLOAT3 value);
    void SaveString(Platform::String^ key, Platform::String^ string);

    bool LoadBool(Platform::String^ key, bool defaultValue);
    int  LoadInt32(Platform::String^ key, int defaultValue);
    float LoadSingle(Platform::String^ key, float defaultValue);
    DirectX::XMFLOAT3 LoadXMFLOAT3(Platform::String^ key, DirectX::XMFLOAT3 defaultValue);
    Platform::String^ LoadString(Platform::String^ key, Platform::String^ defaultValue);

private:
    Platform::String^ m_keyName;
    Windows::Foundation::Collections::IPropertySet^ m_settingsValues;
};
```

The **MarbleMaze** class holds a **PersistentState** object. The **MarbleMaze** constructor initializes this object and provides the local application data store as the backing data store.

```cpp
m_persistentState = ref new PersistentState();
m_persistentState->Initialize(ApplicationData::Current->LocalSettings->Values, "MarbleMaze");
```

Marble Maze saves its state when the marble passes over a checkpoint or the goal (in the **MarbleMaze::Update** method), and when the window loses focus (in the **MarbleMaze::OnFocusChange** method). If your game holds a large amount of state data, we recommend that you occasionally save state to persistent storage in a similar manner because you only have a few seconds to respond to the suspend notification. Therefore, when your app receives a suspend notification, it only has to save the state data that has changed.

To respond to suspend and resume notifications, the **DirectXPage** class defines the **SaveInternalState** and **LoadInternalState** methods that are called on suspend and resume. The **MarbleMaze::OnSuspending** method handles the suspend event and the **MarbleMaze::OnResuming** method handles the resume event.

The **MarbleMaze::OnSuspending** method saves game state and suspends audio.

```cpp
void MarbleMaze::OnSuspending()
{
    SaveState();
    m_audio.SuspendAudio();
}
```

The **MarbleMaze::SaveState** method saves game state values such as the current position and velocity of the marble, the most recent checkpoint, and the high-score table.

```cpp
void MarbleMaze::SaveState()
{
    m_persistentState->SaveXMFLOAT3(":Position", m_physics.GetPosition());
    m_persistentState->SaveXMFLOAT3(":Velocity", m_physics.GetVelocity());
    m_persistentState->SaveSingle(":ElapsedTime", m_inGameStopwatchTimer.GetElapsedTime());

    m_persistentState->SaveInt32(":GameState", static_cast<int>(m_gameState));
    m_persistentState->SaveInt32(":Checkpoint", static_cast<int>(m_currentCheckpoint));

    int i = 0; 
    HighScoreEntries entries = m_highScoreTable.GetEntries();
    const int bufferLength = 16;
    char16 str[bufferLength];

    m_persistentState->SaveInt32(":ScoreCount", static_cast<int>(entries.size()));
    for (auto iter = entries.begin(); iter != entries.end(); ++iter)
    {
        int len = swprintf_s(str, bufferLength, L"%d", i++);
        Platform::String^ string = ref new Platform::String(str, len);

        m_persistentState->SaveSingle(Platform::String::Concat(":ScoreTime", string), iter->elapsedTime);
        m_persistentState->SaveString(Platform::String::Concat(":ScoreTag", string), iter->tag);
    }
}
```

When the game resumes, it only has to resume audio. It doesn't have to load state from persistent storage because the state is already loaded in memory.

How the game suspends and resumes audio is explained in the document [Adding audio to the Marble Maze sample](adding-audio-to-the-marble-maze-sample.md).

To support restart, the **MarbleMaze::Initialize** method, which is called during startup, calls the **MarbleMaze::LoadState** method. The **MarbleMaze::LoadState** method reads and applies the state to the game objects. This method also sets the current game state to paused if the game was paused or active when it was suspended. We pause the game so that the user is not surprised by unexpected activity. It also moves to the main menu if the game was not in a gameplay state when it was suspended.

```cpp
void MarbleMaze::LoadState()
{
    XMFLOAT3 position = m_persistentState->LoadXMFLOAT3(":Position", m_physics.GetPosition());
    XMFLOAT3 velocity = m_persistentState->LoadXMFLOAT3(":Velocity", m_physics.GetVelocity());
    float elapsedTime = m_persistentState->LoadSingle(":ElapsedTime", 0.0f);

    int gameState = m_persistentState->LoadInt32(":GameState", static_cast<int>(m_gameState));
    int currentCheckpoint = m_persistentState->LoadInt32(":Checkpoint", static_cast<int>(m_currentCheckpoint));

    switch (static_cast<GameState>(gameState))
    {
    case GameState::Initial:
        break;

    case GameState::MainMenu:
    case GameState::HighScoreDisplay:
    case GameState::PreGameCountdown:
    case GameState::PostGameResults:
        SetGameState(GameState::MainMenu);
        break;

    case GameState::InGameActive:
    case GameState::InGamePaused:
        m_inGameStopwatchTimer.SetVisible(true);
        m_inGameStopwatchTimer.SetElapsedTime(elapsedTime);
        m_physics.SetPosition(position);
        m_physics.SetVelocity(velocity);
        m_currentCheckpoint = currentCheckpoint;
        SetGameState(GameState::InGamePaused);
        break;
    }

    int count = m_persistentState->LoadInt32(":ScoreCount", 0);

    const int bufferLength = 16;
    char16 str[bufferLength];

    for (int i = 0; i < count; i++)
    {
        HighScoreEntry entry;
        int len = swprintf_s(str, bufferLength, L"%d", i);
        Platform::String^ string = ref new Platform::String(str, len);

        entry.elapsedTime = m_persistentState->LoadSingle(Platform::String::Concat(":ScoreTime", string), 0.0f);
        entry.tag = m_persistentState->LoadString(Platform::String::Concat(":ScoreTag", string), L"");
        m_highScoreTable.AddScoreToTable(entry);
    }
}
```

> **Important**  Marble Maze doesn't distinguish between cold starting—that is, starting for the first time without a prior suspend event—and resuming from a suspended state. This is recommended design for all UWP apps.

 

For more examples that demonstrate how to store and retrieve settings and files from the local application data store, see [Quickstart: Local application data](https://msdn.microsoft.com/library/windows/apps/hh465118). For more info about application data, see [Store and retrieve settings and other app data](https://msdn.microsoft.com/library/windows/apps/mt299098).

##  Next steps


Read [Adding visual content to the Marble Maze sample](adding-visual-content-to-the-marble-maze-sample.md) for information about some of the key practices to keep in mind when you work with visual resources.

## Related topics

* [Adding visual content to the Marble Maze sample](adding-visual-content-to-the-marble-maze-sample.md)
* [Marble Maze sample fundamentals](marble-maze-sample-fundamentals.md)
* [Developing Marble Maze, a UWP game in C++ and DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 







<!--HONumber=Aug16_HO3-->


