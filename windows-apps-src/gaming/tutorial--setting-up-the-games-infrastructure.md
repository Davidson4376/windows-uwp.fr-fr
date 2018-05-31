---
author: joannaleecy
title: Configurer le projet de jeu
description: La première étape de l’assemblage de votre jeu consiste à configurer un projet dans Microsoft VisualStudio de façon à réduire la quantité de travail nécessaire sur l’infrastructure de code.
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, jeux, configuration, directx
ms.localizationpriority: medium
ms.openlocfilehash: 6b1a993f16d2d9403f16c1f4cc15805b0c5f36a9
ms.sourcegitcommit: 842ddba19fa3c028ea43e7922011515dbeb34e9c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/05/2018
ms.locfileid: "1488913"
---
# <a name="set-up-the-game-project"></a>Configurer le projet de jeu

Cette rubrique décrit comment configurer un simple jeu UWP DirectX à l’aide des modèles dans Visual Studio. La première étape de l’assemblage de votre jeu consiste à configurer un projet dans Microsoft VisualStudio de façon à réduire la quantité de travail nécessaire sur l’infrastructure de code. Apprenez à gagner du temps de configuration en utilisant le modèle approprié et en configurant le projet spécifiquement pour le développement de jeux.

## <a name="objectives"></a>Objectifs

* Configurer un projet de jeu Direct3D dans Visual Studio à l’aide d’un modèle
* Comprendre le point d’entrée principal du jeu en examinant les fichiers sources **App**
* Examiner le fichier **package.appxmanifest** du projet
* Découvrir la prise en charge et les outils de développement de jeux inclus dans le projet

## <a name="how-to-set-up-the-game-project"></a>Comment configurer le projet de jeu

Si vous débutez dans le développement de la plateforme Windows universelle (UWP), nous vous recommandons d’utiliser des modèles dans Visual Studio pour configurer la structure du code de base.

>[!Note]
>Cet article fait partie d’une série de didacticiels basée sur un exemple de jeu. Vous pouvez obtenir le dernier code en accédant à [Exemple de jeu Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Cet exemple fait partie d’une vaste collection d’exemples de fonctionnalités UWP. Pour obtenir des instructions sur le téléchargement de l’exemple, consultez [Obtenir des exemples de la plateforme Windows universelle (UWP) à partir de GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

### <a name="use-directx-template-to-create-a-project"></a>Utiliser un modèle DirectX pour créer un projet

Un modèle Visual Studio est une collection de paramètres et de fichiers de code qui ciblent un type spécifique d’application selon la technologie et le langage préférés. Dans Microsoft Visual Studio 2017, vous trouverez de nombreux modèles qui peuvent considérablement simplifier le développement d’applications graphiques et de jeux. Si vous n’utilisez pas de modèle, vous devez développer vous-même une grande partie de l’infrastructure d’affichage et du rendu graphique de base, ce qui peut représenter une corvée pour un développeur de jeux débutant.

Le modèle utilisé pour ce didacticiel est intitulé **Application DirectX11 (Windows universel)**. 

Procédure de création d’un projet de jeu DirectX11 dans Visual Studio:
1.  Sélectionnez **Fichier...** &gt; **Nouveau**  &gt; **Projet...**
2.  Dans le volet gauche, sélectionnez **Installé** &gt; **Modèles** &gt; **Visual C++** &gt; **Windows universel**
3.  Dans le volet central, sélectionnez **Application DirectX11 (Windows universel)**
4.  Attribuez un nom à votre projet de jeu, puis cliquez sur **OK**.

![Capture d’écran qui montre comment sélectionner le modèle DirectX11 pour créer un projet de jeu](images/simple-dx-game-setup-new-project.png)

Ce modèle vous fournit l’infrastructure de base pour une application UWP utilisant DirectX avec C++. Cliquez sur la touche F5 pour générer et exécuter l’application. Regardez cet écran bleu poudre. Le modèle crée plusieurs fichiers de code contenant les fonctionnalités de base pour une applicationUWP utilisant DirectX avec C++.

## <a name="review-the-apps-main-entry-point-by-understanding-iframeworkview"></a>Passer en revue le point d’entrée principal de l’application en analysant IFrameworkView

La classe **App** hérite de la classe **IFrameworkView**.

### <a name="inspect-apph"></a>Examiner **App.h**.

Examinons rapidement les 5méthodes dans **App.h** &mdash; [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495), [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509), [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501), [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) et [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523) lors de l’implémentation de l’interface [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700469) qui définit un fournisseur d’affichage. Ces méthodes sont exécutées par le singleton de l’application qui est créé lors du lancement du jeu, chargent toutes les ressources de votre application et connectent les gestionnaires d’événements appropriés.

```cpp
    // Main entry point for our app. Connects the app with the Windows shell and handle application lifecycle events.
    ref class App sealed : public Windows::ApplicationModel::Core::IFrameworkView
    {
    public:
        App();

        // IFrameworkView Methods.
        virtual void Initialize(Windows::ApplicationModel::Core::CoreApplicationView^ applicationView);
        virtual void SetWindow(Windows::UI::Core::CoreWindow^ window);
        virtual void Load(Platform::String^ entryPoint);
        virtual void Run();
        virtual void Uninitialize();

    protected:
        ...
    };
```

### <a name="inspect-appcpp"></a>Examiner **App.cpp**

Voici la méthode **main** dans le fichier source **App.cpp**:

```cpp
//The main function is only used to initialize our IFrameworkView class.
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

Cette méthode crée une instance du fournisseur d’affichage Direct3D à partir de la fabrique de fournisseurs d’affichages (**Direct3DApplicationSource**, définie dans **App.h**), et la transmet au singleton de l’application en appelant ([**CoreApplication::Run**](https://msdn.microsoft.com/library/windows/apps/hh700469)). Cela signifie que le point de départ de votre jeu se trouve dans le corps de l’implémentation de la méthode [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505), **App::Run** dans ce cas précis. 

Faites défiler pour trouver la méthode **App::Run** dans **App.cpp**. Voici le code:

```cpp
//This method is called after the window becomes active.
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

Fonction de cette méthode: si la fenêtre de votre jeu n’est pas fermée, elle distribue tous les événements, met à jour le minuteur, restitue et présente les résultats de votre pipeline graphique. Nous aborderons ceci plus en détail dans [Définir l’infrastructure d’application UWP](tutorial--building-the-games-uwp-app-framework.md), [Infrastructure de renduI: présentation du rendu](tutorial--assembling-the-rendering-pipeline.md) et [Infrastructure de renduII: rendu de jeu](tutorial-game-rendering.md). À ce stade, vous devez avoir une idée de la structure de code de base d’un jeu UWP DirectX.

## <a name="review-and-update-the-packageappxmanifest-file"></a>Examiner et mettre à jour le fichier package.appxmanifest


Le modèle ne se résume pas aux seuls fichiers de code. Le fichier **Package.appxmanifest** contient des métadonnées relatives à votre projet qui sont utilisées pour la création de packages et le lancement de votre jeu, ainsi que pour l’envoi au MicrosoftStore. Il contient également des informations importantes utilisées par le système du joueur pour fournir l’accès aux ressources système nécessaires au fonctionnement du jeu.

Lancez le **concepteur de manifeste** en double-cliquant sur le fichier **Package.appxmanifest** dans l’**Explorateur de solutions**.

![Capture d’écran de l’éditeur de manifeste package.appx.](images/simple-dx-game-setup-app-manifest.png)

Pour plus d’informations sur le fichier **package.appxmanifest** et sur la création de packages, consultez [Concepteur de manifeste](https://msdn.microsoft.com/library/windows/apps/br230259.aspx). Pour le moment, examinez l’onglet **Fonctionnalités** et les options proposées.

![Capture d’écran avec les fonctionnalités par défaut d’une application Direct3D.](images/simple-dx-game-setup-capabilities.png)

Si vous ne sélectionnez pas les fonctionnalités utilisées par votre jeu, par exemple l’accès à **Internet** pour le tableau global des meilleurs scores, vous ne serez pas en mesure d’accéder aux fonctionnalités ou aux ressources correspondantes. Lorsque vous créez un jeu, veillez à sélectionner les fonctionnalités nécessaires au fonctionnement du jeu !

Examinons maintenant le reste des fichiers qui accompagnent le modèle **Application DirectX11 (Windows universel)**.

## <a name="review-the-included-libraries-and-headers"></a>Passer en revue les bibliothèques et en-têtes inclus

Il reste quelques fichiers que nous n’avons pas encore examinés. Ces fichiers offrent une prise en charge et des outils supplémentaires typiques des scénarios de développement de jeux Direct3D. Pour obtenir la liste complète des fichiers fournie avec le projet de jeu DirectX de base, consultez [Modèles de projet de jeu DirectX](user-interface.md#template-structure).

| Fichier source du modèle         | Dossier de fichiers            | Description |
|------------------------------|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DeviceResources.h/.cpp       | Common                 | Définit un objet de classe qui contrôle toutes les [ressources de l’appareil](tutorial--assembling-the-rendering-pipeline.md#resource) DirectX. Il inclut également une interface pour votre application qui détient DeviceResources pour être averti quand l’appareil est perdu ou créé.                                                |
| DirectXHelper.h              | Common                 | Implémente des méthodes, notamment **DX::ThrowIfFailed**, **ReadDataAsync** et **ConvertDipsToPixels. **DX::ThrowIfFailed** convertit les valeurs d’erreur HRESULT renvoyées par les API Win32 DirectX en exceptions WindowsRuntime. Utilisez cette méthode pour placer un point d’arrêt pour le débogage des erreurs DirectX. Pour plus d’informations, consultez [ThrowIfFailed](https://github.com/Microsoft/DirectXTK/wiki/ThrowIfFailed). **ReadDataAsync** lit dans un fichier binaire de façon asynchrone. **ConvertDipsToPixels** convertit une longueur exprimée en DIP (Device Independent Pixel) en longueur en pixels physiques. |
| StepTimer.h                  | Common                 | Définit un minuteur haute résolution utile pour les applications de rendu interactives ou les jeux.   |
| Sample3DSceneRenderer.h/.cpp | Content                | Définit un objet de classe pour instancier un pipeline de rendu de base. Il crée une implémentation de convertisseur de base qui connecte une chaîne d’échange Direct3D et une carte graphique à votre UWP avec DirectX.   |
| SampleFPSTextRenderer.h/.cpp | Content                | Définit un objet de classe pour afficher la valeur FPS (images par seconde) actuelle dans l’angle inférieur droit de l’écran avec Direct2D et DirectWrite.  |
| SamplePixelShader.hlsl       | Content                | Contient le code de langage HLSL (High-Level Shader Language) pour un nuanceur de pixels très simple.                                            |
| SampleVertexShader.hlsl      | Content                | Contient le code de langage HLSL (High-Level Shader Language) pour un nuanceur de sommets très simple.                                           |
| ShaderStructures.h           | Content                | Contient les structures de nuanceur qui peuvent être utilisées pour envoyer des matrices MVP et les données par sommet au nuanceur de sommets.  |
| pch.h/.cpp                   | Main                   | Contient tous les fichiers Include système Windows pour les API utilisées par une application Direct3D, notamment les API DirectX11.| 

### <a name="next-steps"></a>Étapes suivantes

À ce stade, vous avez appris comment créer un projet de jeu UWP DirectX à l’aide du modèle **Application DirectX11 (Windows universel)** et quelques composants et fichiers fournis par ce projet vous ont été présentés.

La section suivante est [Définition de l’infrastructure UWP du jeu](tutorial--building-the-games-uwp-app-framework.md). Nous allons examiner comment ce jeu utilise et étend un grand nombre des concepts et composants fournis par le modèle.

 

 




