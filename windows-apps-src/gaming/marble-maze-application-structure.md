---
author: eliotcowley
title: Structure de l’application Marble Maze
description: La structure d’une application de plateforme Windows universelle (UWP) en DirectX diffère de celle d’une application de bureau classique.
ms.assetid: 6080f0d3-478a-8bbe-d064-73fd3d432074
ms.author: elcowle
ms.date: 09/08/2017
ms.topic: article
keywords: windows 10, uwp, jeux, exemple, directx, structure
ms.localizationpriority: medium
ms.openlocfilehash: 1272200bf128443c82807aec9df5559f207819e1
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7287767"
---
# <a name="marble-maze-application-structure"></a>Structure de l’application Marble Maze




La structure d’une application de plateforme Windows universelle (UWP) en DirectX diffère de celle d’une application de bureau classique. Au lieu de fonctionner avec des types de handle tels que [HWND](https://msdn.microsoft.com/library/windows/desktop/aa383751) et des fonctions telles que [CreateWindow](https://msdn.microsoft.com/library/windows/desktop/ms632679), Windows Runtime fournit des interfaces comme [Windows::UI::Core::ICoreWindow](https://msdn.microsoft.com/library/windows/apps/br208296). Vous pouvez ainsi développer des applications pour UWP grâce à une approche orientée objet plus moderne. Cette section montre comment le code de l’application Marble Maze est structuré.

> [!NOTE]
> L’exemple de code correspondant à ce document est disponible dans [l’exemple de jeu Marble Maze en DirectX](http://go.microsoft.com/fwlink/?LinkId=624011).

 
## 
Ce document aborde certains points importants relatifs à la structure de votre code de jeu:

-   Dans la phase d’initialisation, configurez les composants d’exécution et de bibliothèque utilisés par votre jeu et les ressources spécifiques au jeu.
-   Les applications pour UWP doivent commencer à traiter les événements dans un délai de 5 secondes après le lancement. Ne chargez donc que les ressources nécessaires quand vous chargez votre application. Les jeux doivent charger une grande quantité de ressources en arrière-plan et afficher un écran de progression.
-   Dans la boucle de jeu, répondez aux événements Windows, lisez l’entrée utilisateur, mettez à jour les objets de scène, puis restituez la scène.
-   Utilisez les gestionnaires d’événements pour répondre aux événements fenêtre. (Ils remplacent les messages de fenêtre des applications de bureau Windows.)
-   Utilisez une machine à états pour contrôler le flux et l’ordre de la logique de jeu.

##  <a name="file-organization"></a>Organisation des fichiers


Vous pouvez réutiliser certains composants de Marble Maze dans n’importe quel jeu, avec peu ou pas de modifications. Pour votre propre jeu, vous pouvez adapter l’organisation et les idées présentées dans ces fichiers. Le tableau suivant décrit brièvement les fichiers de code source importants.

| Fichiers                                      | Description                                                                                                                                                                          |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| App.h, App.cpp               | Définit les classes **App** et **DirectXApplicationSource** qui encapsulent la vue (fenêtre, thread et événements) de l’application.                                                     |
| Audio.h, Audio.cpp                         | Définit la classe **Audio** qui gère les ressources audio.                                                                                                                          |
| BasicLoader.h, BasicLoader.cpp             | Définit la classe **BasicLoader** qui fournit les méthodes d’utilitaire permettant de charger des textures, du maillage et des nuanceurs.                                                                  |
| BasicMath.h                                | Définit les structures et les fonctions qui vous permettent d’utiliser des données vectorielles et matricielles, ainsi que des calculs. Plusieurs de ces fonctions sont compatibles avec les types de nuanceurs HLSL.                     |
| BasicReaderWriter.h, BasicReaderWriter.cpp | Définit la classe **BasicReaderWriter** qui utilise Windows Runtime pour lire et écrire les données d’un fichier dans une application UWP.                                                                    |
| BasicShapes.h, BasicShapes.cpp             | Définit la classe **BasicShapes** qui fournit des méthodes d’utilitaire pour créer des formes de base telles que les cubes et les sphères. (Ces fichiers ne sont pas utilisés par l’implémentation de Marble Maze). |                                                                                  |
| Camera.h, Camera.cpp                       | Définit la classe **Camera** qui fournit la position et l’orientation d’un appareil photo.                                                                                               |
| Collision.h, Collision.cpp                 | Gère les informations de collision entre la bille et les autres objets, comme le labyrinthe.                                                                                                       |
| DDSTextureLoader.h, DDSTextureLoader.cpp   | Définit la fonction **CreateDDSTextureFromMemory** qui charge les textures au format .dds à partir d’une mémoire tampon.                                                              |
| DirectXHelper.h             | Définit les fonctions d’assistance DirectX qui sont utiles à de nombreuses applications UWP DirectX.                                                                            |
| LoadScreen.h, LoadScreen.cpp               | Définit la classe **LoadScreen** qui affiche un écran de chargement pendant l’initialisation de l’application.                                                                                         |
| MarbleMazeMain.h, MarbleMazeMain.cpp               | Définit la classe **MarbleMazeMain** qui gère les ressources spécifiques au jeu et définit une grande partie de la logique de jeu.                                                                          |
| MediaStreamer.h, MediaStreamer.cpp         | Définit la classe **MediaStreamer** qui utilise Media Foundation pour aider le jeu à gérer les ressources audio.                                                                            |
| PersistentState.h, PersistentState.cpp     | Définit la classe **PersistentState** qui lit et écrit les types de données primitives depuis et vers un magasin de stockage.                                                                      |
| Physics.h, Physics.cpp                     | Définit la classe **Physics** qui implémente la simulation physique entre la bille et le labyrinthe.                                                                              |
| Primitives.h                               | Définit les types géométriques utilisés par le jeu.                                                                                                                                   |
| SampleOverlay.h, SampleOverlay.cpp         | Définit la classe **SampleOverlay** qui fournit les données et opérations d’interface utilisateur 2D communes.                                                                               |
| SDKMesh.h, SDKMesh.cpp                     | Définit la classe **SDKMesh** qui charge et restitue le maillage au format SDK Mesh (.sdkmesh).                                                                                |
| StepTimer.h               | Définit la classe **StepTimer** qui offre un moyen facile d’obtenir le nombre total d’heures et le nombre d’heures écoulées.
| UserInterface.h, UserInterface.cpp         | Définit la fonctionnalité liée à l’interface utilisateur, comme le système de menus et le tableau des scores.                                                                        |

 

##  <a name="design-time-versus-run-time-resource-formats"></a>Formats des ressources au moment de la conception et au moment de l’exécution


Si possible, utilisez les formats au moment de l’exécution à la place des formats au moment de la conception pour charger plus efficacement les ressources du jeu.

Vous devez utiliser les formats *au moment de la conception* quand vous concevez votre ressource. En général, les concepteurs 3D utilisent des formats au moment de la conception. Certains formats au moment de la conception sont également des formats texte que vous pouvez modifier dans n’importe quel éditeur de texte. Les formats au moment de la conception peuvent être constitués de commentaires et contenir plus d’informations que votre jeu n’en a besoin. Les formats *au moment de l’exécution* représentent le format binaire lu par votre jeu. Les formats au moment de l’exécution sont généralement plus compacts et plus efficaces à charger que les formats au moment de la conception correspondants. C’est pourquoi la plupart des jeux utilisent des composants au moment de l’exécution.

Bien que votre jeu puisse lire directement un format au moment de la conception, l’utilisation d’un format au moment de l’exécution distinct présente plusieurs avantages. Étant donné qu’ils sont souvent plus compacts, ils nécessitent moins d’espace disque et impliquent des temps de transfert réseau inférieurs. De plus, les formats au moment de l’exécution sont souvent représentés en tant que structures de données mappées en mémoire. Ils peuvent donc être chargés en mémoire beaucoup plus rapidement qu’un fichier texte XML, par exemple. Enfin, l’utilisateur final modifie plus difficilement les formats au moment de l’exécution car ils sont généralement codés en binaire.

Les nuanceurs HLSL sont un exemple de ressources qui utilisent différents formats au moment de la conception et au moment de l’exécution. Marble Maze utilise .hlsl comme format au moment de la conception et .cso comme format au moment de l’exécution. Un fichier .hlsl contient le code source du nuanceur ; un fichier .cso contient le code d’octet du nuanceur correspondant. Quand vous convertissez des fichiers .hlsl hors connexion et fournissez des fichiers .cso à votre jeu, vous n’avez pas besoin de convertir des fichiers sources HLSL en code d’octet pendant le chargement de celui-ci.

Pour les instructions, le projet Marble Maze inclut à la fois le format au moment de la conception et le format au moment de l’exécution pour de nombreuses ressources. Toutefois, vous n’avez à appliquer que les formats au moment de la conception dans le projet source de votre jeu, car vous pourrez toujours les convertir aux formats au moment de l’exécution quand vous en aurez besoin. Cette documentation indique comment convertir les formats au moment de la conception en formats au moment de l’exécution.

##  <a name="application-life-cycle"></a>Cycle de vie de l’application


Marble Maze suit le cycle de vie d’une application pour UWP classique. Pour plus d’informations sur le cycle de vie d’une application pour UWP, voir [Cycle de vie d’une application](https://msdn.microsoft.com/library/windows/apps/mt243287).

L’initialisation d’un jeu UWP entraîne généralement celle des composants d’exécution tels que Direct3D, Direct2D et de toutes les bibliothèques de données d’entrée, audio ou physiques qu’il utilise. Il charge également des ressources spécifiques au jeu qui sont nécessaires avant son démarrage. Cette initialisation se produit une seule fois pendant une session de jeu.

Après l’initialisation, les jeux exécutent généralement la *boucle de jeu*. Dans cette boucle, les jeux exécutent généralement quatre actions : le traitement des événements Windows, la collecte des données d’entrée, la mise à jour des objets de scène et le rendu de la scène. Quand le jeu met à jour la scène, il peut appliquer l’état d’entrée actuel aux objets de scène et simuler des événements physiques, comme les collisions d’objets. Le jeu peut également effectuer d’autres activités telles que la lecture d’effets sonores ou l’envoi de données sur le réseau. Quand le jeu restitue la scène, il capture l’état actuel de la scène et la dessine sur le périphérique d’affichage. Les sections suivantes décrivent ces activités de manière détaillée.

##  <a name="adding-to-the-template"></a>Ajout au modèle


Le modèle **Application DirectX 11 (Windows universel)** crée une fenêtre principale vers laquelle vous pouvez effectuer le rendu avec Direct3D. Le modèle comprend également la classe **DeviceResources** qui crée toutes les ressources d’appareil Direct3D nécessaires au rendu du contenu 3D dans une application UWP.

La classe **App** crée l’objet de classe **MarbleMazeMain**, démarre le chargement des ressources, effectue une boucle de mise à jour de la minuterie, puis appelle la méthode de rendu **MarbleMazeMain::Render** pour chaque image. Les méthodes **App::OnWindowSizeChanged**, **App::OnDpiChanged** et **App::OnOrientationChanged** appellent chacune la méthode **MarbleMazeMain::CreateWindowSizeDependentResources** et la méthode **App::Run** appelle les méthodes **MarbleMazeMain::Update** et **MarbleMazeMain::Render**.

L’exemple suivant montre l’emplacement où la méthode **App::SetWindow** crée l’objet de classe **MarbleMaze**. La classe **DeviceResources** est passée à la méthode afin de permettre l’utilisation des objets Direct3D pour le rendu.

```cpp
    m_main = std::unique_ptr<MarbleMazeMain>(new MarbleMazeMain(m_deviceResources));
```

La classe **App** démarre également le chargement des ressources différées du jeu. Pour plus d’informations, voir la section suivante.

En outre, la classe **App** définit les gestionnaires d’événements pour les événements [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow). Quand les gestionnaires de ces événements sont appelés, ils passent l’entrée à la classe **MarbleMazeMain**.

## <a name="loading-game-assets-in-the-background"></a>Chargement des composants du jeu en arrière-plan


Pour être sûr que votre jeu réponde aux événements fenêtre en moins de 5 secondes après son lancement, nous vous recommandons de charger les composants du jeu de façon asynchrone ou en arrière-plan. Quand les composants sont chargés en arrière-plan, votre jeu peut répondre aux événements de fenêtrage.

> [!NOTE]
> Vous pouvez également afficher le menu principal quand il est prêt et permettre aux autres composants de poursuivre leur chargement en arrière-plan. Si l’utilisateur sélectionne une option de menu avant que toutes les ressources soient chargées, vous pouvez indiquer le chargement en cours des ressources de scène en affichant, par exemple, une barre de progression.

 

Même si le jeu contient peu de composants, il est conseillé de les charger de façon asynchrone pour deux raisons. La première est qu’il est difficile de garantir que l’ensemble de vos composants seront rapidement chargés sur tous les périphériques et avec toutes les configurations. En outre, en incorporant le chargement asynchrone au préalable, votre code évolue à mesure que vous ajoutez des fonctionnalités.

Le chargement asynchrone des composants commence par la méthode **App::Load**. Cette méthode utilise la classe [task](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class) pour charger les composants du jeu en arrière-plan.

```cpp
    task<void>([=]()
    {
        m_main->LoadDeferredResources(true, false);
    });
```

La classe **MarbleMazeMain** définit l’indicateur *m\_deferredResourcesReady* pour spécifier que le chargement asynchrone est terminé. La méthode **MarbleMazeMain::LoadDeferredResources** charge les ressources du jeu, puis définit cet indicateur. Les phases de mise à jour (**MarbleMazeMain::Update**) et de rendu (**MarbleMazeMain::Render**) de l’application vérifient cet indicateur. Quand cet indicateur est défini, le jeu continue à s’exécuter normalement. S’il n’est pas encore défini, le jeu affiche l’écran de chargement.

Pour plus d’informations sur la programmation asynchrone dans les applications UWP, voir [Programmation asynchrone en C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

> [!TIP]
> Si vous écrivez le code de jeu qui fait partie d’une bibliothèque C++ WindowsRuntime (en d’autres termes, une DLL), prenez connaissance de la section [Création d’opérations asynchrones en C++ pour les applications UWP](https://docs.microsoft.com/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps) pour découvrir comment créer des opérations asynchrones pouvant être consommées par des applications et d’autres bibliothèques.

 

## <a name="the-game-loop"></a>Boucle de jeu


La méthode **App::Run** exécute la boucle de jeu principale (**MarbleMazeMain::Update**). Cette méthode est appelée pour chaque image.

Pour distinguer facilement le code de vue et de fenêtre du code spécifique au jeu, nous avons implémenté la méthode **App::Run** pour transférer la mise à jour et restituer les appels vers l’objet **MarbleMazeMain**.

L’exemple suivant montre la méthode **App::Run** qui inclut la boucle de jeu principale. La boucle de jeu met à jour les variables de durée totale et de durée d’image, puis met à jour et restitue la scène. Cela permet également de s’assurer que le rendu du contenu est effectué uniquement quand la fenêtre est visible.

```cpp
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->
                ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->
                ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }

    // The app is exiting so do the same thing as if the app were being suspended.
    m_main->OnSuspending();

#ifdef _DEBUG
    // Dump debug info when exiting.
    DumpD3DDebug();
#endif //_DEGBUG
}
```

## <a name="the-state-machine"></a>Machine à états


Les jeux contiennent généralement une *machine à états* (également appelée *machine à états finis* ou FSM (Finite State Machine) pour contrôler le flux et l’ordre de la logique de jeu. Une machine à états offre un nombre donné d’états et la capacité de passer d’un état à un autre. Une machine à états part généralement d’un état *initial*, passe par un ou plusieurs états *intermédiaires* et aboutit à un état *terminal*.

Une boucle de jeu utilise souvent une machine à états pour exécuter la logique spécifique à l’état de jeu actuel. Marble Maze définit l’énumération **GameState** qui définit chaque état potentiel du jeu.

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

Par exemple, l’état **MainMenu** définit que le menu principal s’affiche et que le jeu n’est pas actif. Inversement, l’état **InGameActive** définit que le jeu est actif et que le menu ne s’affiche pas. La classe **MarbleMazeMain** définit la variable membre **m\_gameState** afin qu’elle contienne l’état de jeu actif.

Les méthodes **MarbleMazeMain::Update** et **MarbleMazeMain::Render** utilisent les instructions switch pour exécuter la logique relative à l’état actuel. L’exemple suivant illustre une instruction switch dans le cas de la méthode **MarbleMazeMain::Update** (les détails sont supprimés pour représenter la structure).

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

Quand la logique ou le rendu du jeu dépend d’un état spécifique de jeu, nous le soulignons dans cette documentation.

## <a name="handling-app-and-window-events"></a>Gestion des événements d’application et de fenêtre


Windows Runtime fournit un système de gestion d’événements orienté objet qui vous permet de gérer plus facilement les messages Windows. Pour utiliser un événement dans une application, vous devez fournir un gestionnaire d’événements ou la méthode de gestion d’événements qui répond à l’événement. Vous devez également inscrire le gestionnaire d’événements avec la source de l’événement. Ce processus est souvent appelé connexion des événements.

### <a name="supporting-suspend-resume-and-restart"></a>Prise en charge de la suspension, la reprise et du redémarrage

Marble Maze est suspendu quand l’utilisateur quitte le jeu ou quand Windows entre dans un état de faible consommation d’énergie. Le jeu reprend quand l’utilisateur le place au premier plan ou quand Windows quitte un état de faible consommation d’énergie. En général, vous ne fermez pas les applications. Windows peut fermer l’application quand elle est à l’état suspendu et que des ressources sont nécessaires, comme la mémoire utilisée par l’application. Windows prévient quand une application est sur le point d’être suspendue ou de reprendre. En revanche, il ne signale pas si l’application est sur le point d’être fermée. Votre application doit donc pouvoir sauvegarder les données nécessaires (au moment où Windows signale que votre application est sur le point d’être suspendue) pour restaurer l’état de l’utilisateur actuel quand l’application est redémarrée. Si l’état de l’utilisateur de votre application est difficile à enregistrer, il sera alors nécessaire de procéder à son enregistrement régulier, même avant que votre application reçoive la notification de suspension. Marble Maze répond aux notifications de suspension et de reprise pour deux raisons :

1.  Quand l’application est suspendue, le jeu enregistre l’état de jeu actuel et met en pause la lecture audio. Quand l’application reprend, le jeu redémarre la lecture audio.
2.  Quand l’application est fermée puis redémarrée, le jeu reprend à l’état précédent.

Marble Maze effectue les tâches suivantes pour prendre en charge les fonctionnalités de suspension et de reprise :

-   Il enregistre son état dans un stockage persistant aux points clés dans le jeu, par exemple quand l’utilisateur atteint un point de contrôle.
-   Il répond aux notifications de suspension en enregistrant son état dans un stockage persistant.
-   Il répond aux notifications de reprise en chargeant son état à partir du stockage persistant. Il charge également l’état précédent au démarrage.

Pour prendre en charge les fonctionnalités de suspension et de reprise, Marble Maze définit la classe **PersistentState**. (Voir **PersistentState.h** et **PersistentState.cpp**). Cette classe utilise l’interface [Windows::Foundation::Collections::IPropertySet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IPropertySet) pour les propriétés de lecture et d’écriture. La classe **PersistentState** fournit des méthodes qui lisent et écrivent des types de données primitives (par exemple **bool**, **int**, **float**, [XMFLOAT3](https://msdn.microsoft.com/library/windows/desktop/ee419475) et [Platform::String](https://docs.microsoft.com/cpp/cppcx/platform-string-class)), depuis et vers un magasin de stockage.

```cpp
ref class PersistentState
{
internal:
    void Initialize(
        _In_ Windows::Foundation::Collections::IPropertySet^ settingsValues,
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

    DirectX::XMFLOAT3 LoadXMFLOAT3(
        Platform::String^ key, 
        DirectX::XMFLOAT3 defaultValue);

    Platform::String^ LoadString(
        Platform::String^ key, 
        Platform::String^ defaultValue);

private:
    Platform::String^ m_keyName;
    Windows::Foundation::Collections::IPropertySet^ m_settingsValues;
};
```

La classe **MarbleMazeMain** contient un objet **PersistentState**. Le constructeur **MarbleMazeMain** initialise cet objet et fournit le magasin de données d’application locales en tant que magasin de données de stockage.

```cpp
m_persistentState = ref new PersistentState();

m_persistentState->Initialize(
    Windows::Storage::ApplicationData::Current->LocalSettings->Values,
    "MarbleMaze");
```

Marble Maze enregistre son état quand la bille passe sur un point de contrôle ou sur l’objectif (dans la méthode **MarbleMazeMain::Update**) et quand la fenêtre perd le focus (dans la méthode **MarbleMazeMain::OnFocusChange**). Si le jeu contient un grand nombre de données d’état, nous vous recommandons d’enregistrer occasionnellement l’état dans un stockage persistant de la même façon, car vous ne disposez que de quelques secondes pour répondre à la notification de suspension. Par conséquent, quand votre application reçoit une notification de suspension, elle ne doit enregistrer que les données d’état qui ont changé.

Pour répondre aux notifications de suspension et de reprise, la classe **MarbleMazeMain** définit les méthodes **SaveState** et **LoadState**, qui sont appelées durant les phases de suspension et de reprise. La méthode **MarbleMazeMain::OnSuspending** gère l’événement de suspension et la méthode **MarbleMazeMain::OnResuming** gère l’événement de reprise.

La méthode **MarbleMazeMain::OnSuspending** enregistre l’état de jeu et suspend la lecture audio.

```cpp
void MarbleMazeMain::OnSuspending()
{
    SaveState();
    m_audio.SuspendAudio();
}
```

La méthode **MarbleMazeMain::SaveState** enregistre les valeurs d’état de jeu telles que la position et la vitesse actuelles de la bille, le point de contrôle le plus récent et le tableau des scores.

```cpp
void MarbleMazeMain::SaveState()
{
    m_persistentState->SaveXMFLOAT3(":Position", m_physics.GetPosition());
    m_persistentState->SaveXMFLOAT3(":Velocity", m_physics.GetVelocity());

    m_persistentState->SaveSingle(
        ":ElapsedTime", 
        m_inGameStopwatchTimer.GetElapsedTime());

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

        m_persistentState->SaveSingle(
            Platform::String::Concat(":ScoreTime", string), 
            iter->elapsedTime);

        m_persistentState->SaveString(
            Platform::String::Concat(":ScoreTag", string), 
            iter->tag);
    }
}
```

Quand le jeu reprend, seule la lecture audio est réactivée. Il ne doit pas charger l’état du stockage persistant car celui-ci est déjà chargé en mémoire.

Pour savoir comment le jeu est suspendu et la lecture audio reprend, voir le document [Ajout d’audio à l’exemple Marble Maze](adding-audio-to-the-marble-maze-sample.md).

Pour prendre en charge la reprise, le constructeur **MarbleMazeMain** qui est appelé au démarrage appelle la méthode **MarbleMazeMain::LoadState**. La méthode **MarbleMazeMain::LoadState** lit et applique l’état aux objets de jeu. Cette méthode définit également le jeu à l’état de pause si celui-ci a été mis en pause ou était actif au moment de la suspension. Le jeu est mis en pause afin que l’utilisateur ne soit pas surpris par une activité inattendue. Il se déplace également vers le menu principal s’il n’y avait aucune action de jeu en cours quand le jeu a été suspendu.

```cpp
void MarbleMazeMain::LoadState()
{
    XMFLOAT3 position = m_persistentState->LoadXMFLOAT3(
        ":Position", 
        m_physics.GetPosition());

    XMFLOAT3 velocity = m_persistentState->LoadXMFLOAT3(
        ":Velocity", 
        m_physics.GetVelocity());

    float elapsedTime = m_persistentState->LoadSingle(":ElapsedTime", 0.0f);

    int gameState = m_persistentState->LoadInt32(
        ":GameState", 
        static_cast<int>(m_gameState));

    int currentCheckpoint = m_persistentState->LoadInt32(
        ":Checkpoint", 
        static_cast<int>(m_currentCheckpoint));

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

        entry.elapsedTime = m_persistentState->LoadSingle(
            Platform::String::Concat(":ScoreTime", string), 
            0.0f);

        entry.tag = m_persistentState->LoadString(
            Platform::String::Concat(":ScoreTag", string), 
            L"");

        m_highScoreTable.AddScoreToTable(entry);
    }
}
```

> [!IMPORTANT]
> Marble Maze ne fait pas de distinction entre le démarrage à froid (autrement dit, le démarrage initial sans événement de suspension antérieur) et la reprise à partir de l’état suspendu. Il s’agit de la conception recommandée pour toutes les applications UWP.

Pour plus d’informations sur les données d’application, voir [Stocker et récupérer des paramètres et autres données d’application](https://msdn.microsoft.com/library/windows/apps/mt299098).

##  <a name="next-steps"></a>Étapes suivantes


Pour plus d’informations sur certaines pratiques importantes à garder à l’esprit quand vous utilisez des ressources visuelles, voir [Ajout de contenu visuel à l’exemple Marble Maze](adding-visual-content-to-the-marble-maze-sample.md).

## <a name="related-topics"></a>Rubriques connexes

* [Ajout de contenu visuel à l’exemple Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)
* [Notions de base de l’exemple Marble Maze](marble-maze-sample-fundamentals.md)
* [Développement de Marble Maze, jeu pour UW en C++ et DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




