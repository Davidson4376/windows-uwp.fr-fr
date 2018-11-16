---
author: mtoepke
title: Optimiser la latence d’entrée pour les jeux UWP DirectX
description: La latence d’entrée peut avoir un impact important sur un jeu. Son optimisation peut rendre un jeu plus fluide.
ms.assetid: e18cd1a8-860f-95fb-098d-29bf424de0c0
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, jeux, directx, latence d’entrée
ms.localizationpriority: medium
ms.openlocfilehash: a2e92dc10dbcdc3a511c1b1a1271ae759cc03c60
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6844535"
---
#  <a name="optimize-input-latency-for-universal-windows-platform-uwp-directx-games"></a>Optimiser la latence d’entrée pour les jeux UWP DirectX



La latence d’entrée peut avoir un impact important sur un jeu. Son optimisation peut rendre un jeu plus fluide. De plus, une optimisation appropriée des événements d’entrée peut améliorer l’autonomie de la batterie. Apprenez à choisir les options de traitement appropriées de l’événement d’entrée CoreDispatcher pour vous assurer que votre jeu gère les entrées de façon aussi fluide que possible.

## <a name="input-latency"></a>Latence d’entrée


La latence d’entrée est le temps nécessaire au système pour répondre à une entrée utilisateur. La réponse représente souvent un changement par rapport à ce qui est affiché à l’écran ou à ce qui est entendu par le biais du retour audio.

Chaque événement d’entrée, qu’il provienne d’un pointeur tactile, d’un pointeur de souris ou d’un clavier, génère un message qui doit être traité par un gestionnaire d’événements. De nos jours, les numériseurs tactiles et les périphériques de jeu signalent les événements d’entrée à une fréquence d’au moins 100 Hz par pointeur. En d’autres termes, les applications peuvent recevoir 100 événements ou plus par seconde et par pointeur (ou frappe). Cette fréquence des mises à jour est amplifiée si plusieurs pointeurs s’exécutent en même temps, ou qu’un périphérique d’entrée d’une plus grande précision est utilisé (par exemple, une souris de jeu). La file d’attente de messages d’événements peut se remplir très rapidement.

Il est important de bien comprendre les exigences de votre jeu en matière de latence d’entrée afin que les événements soient traités du mieux possible selon la situation. Il n’y a pas de solution unique valable pour l’ensemble des jeux.

## <a name="power-efficiency"></a>Efficacité énergétique


Dans le contexte de la latence d’entrée, l’«efficacité énergétique» fait référence au taux d’utilisation du GPU par un jeu. Un jeu qui utilise moins de ressources GPU est plus économe en énergie et permet un accroissement de l’autonomie de la batterie. Cela est également vrai pour l’UC.

Si un jeu peut dessiner du contenu sur la totalité de l’écran en moins de 60 images par seconde (actuellement, il s’agit de la vitesse de rendu maximale sur la plupart des écrans) sans dégrader l’expérience de l’utilisateur, il sera plus économe en énergie en dessinant moins souvent. Certains jeux ne mettent à jour l’écran qu’en réponse à une entrée utilisateur. Ainsi, ces jeux ne dessinent pas le même contenu à plusieurs reprises à 60 images par seconde.

## <a name="choosing-what-to-optimize-for"></a>Choix en matière d’optimisation


Durant la conception d’une application DirectX, vous devez faire des choix. Est-ce que l’application doit afficher 60 images par seconde pour présenter une animation fluide, ou est-ce qu’elle doit seulement afficher le contenu en réponse à une entrée ? Faut-il qu’elle ait la latence d’entrée la plus basse possible ou peut-elle tolérer un léger retard ? Est-ce que mes utilisateurs s’attendent à ce que mon application soit économe en matière d’utilisation de la batterie ?

Les réponses à ces questions vous permettront probablement de situer votre application par rapport à l’un des scénarios suivants :

1.  Rendu à la demande. Les jeux de cette catégorie ne doivent mettre à jour l’écran qu’en réponse à des types d’entrée spécifiques. L’efficacité énergétique est excellente, car l’application n’effectue pas de rendu répétitif d’images identiques. Par ailleurs, la latence d’entrée est faible, car l’application passe la majeure partie de son temps à attendre une entrée. Les jeux de société et les lecteurs de News sont des exemples d’applications qui peuvent correspondre à cette catégorie.
2.  Rendu à la demande avec animations de transition. Ce scénario est similaire au premier scénario, sauf que certains types d’entrée démarrent une animation qui ne dépend pas d’une entrée ultérieure de l’utilisateur. L’efficacité énergétique est bonne, car le jeu n’effectue pas de rendu répétitif d’images identiques. De plus, la latence d’entrée est faible quand le jeu n’est pas en cours d’animation. Les jeux interactifs pour enfants et les jeux de société où chaque déplacement est animé sont des exemples d’applications qui peuvent appartenir à cette catégorie.
3.  Rendu de 60 images par seconde. Dans ce scénario, le jeu met constamment l’écran à jour. L’efficacité énergétique est faible, car le jeu effectue le rendu du nombre maximal d’images pouvant être affichées. La latence d’entrée est élevée, car DirectX bloque le thread pendant la présentation du contenu. Ainsi, le thread ne peut pas envoyer plus d’images à l’écran qu’il ne peut en montrer à l’utilisateur. Les jeux de tir à la première personne, les jeux de stratégie en temps réel et les jeux basés sur la physique sont des exemples d’applications qui peuvent appartenir à cette catégorie.
4.  Rendu de 60 images par seconde avec la latence d’entrée la plus faible possible. Comme dans le scénario 3, l’application met constamment l’écran à jour, d’où une efficacité énergétique faible. Toutefois, il existe une différence dans ce cas précis : le jeu répond à l’entrée sur un thread distinct afin que le traitement de l’entrée ne soit pas bloqué par la présentation des images à l’écran. Les jeux multijoueurs en ligne, les jeux de combat ou les jeux de rythme/synchronisation peuvent appartenir à cette catégorie, car ils prennent en charge des entrées relatives aux déplacements dans des fenêtres d’événements très brèves.

## <a name="implementation"></a>Implémentation


La plupart des jeux DirectX sont basés sur ce que l’on appelle la boucle de jeu. L’algorithme de base consiste à effectuer les étapes suivantes jusqu’à ce que l’utilisateur quitte le jeu ou l’application :

1.  Traiter l’entrée
2.  Mettre à jour l’état du jeu
3.  Dessiner le contenu du jeu

Quand le contenu d’un jeu DirectX est rendu et qu’il est prêt à être présenté à l’écran, la boucle de jeu attend que le GPU soit prêt à recevoir une nouvelle image avant de se réactiver pour traiter une entrée.

Nous montrerons l’implémentation de la boucle de jeu pour chacun des scénarios mentionnés précédemment en itérant un simple jeu de puzzle. Les points de décision, les avantages et les compromis abordés durant chaque implémentation peuvent servir de repères pour vous aider à optimiser vos applications en atteignant une latence d’entrée faible et une efficacité énergétique élevée.

## <a name="scenario-1-render-on-demand"></a>Scénario 1 : rendu à la demande


La première itération du jeu de puzzle ne met à jour l’écran qu’au moment où l’utilisateur déplace une pièce de puzzle. Un utilisateur peut faire glisser une pièce de puzzle vers son emplacement ou l’ancrer à son emplacement en la sélectionnant, puis en appuyant sur la destination appropriée. Dans le second cas, la pièce de puzzle saute vers sa destination sans animations, ni effets d’aucune sorte.

Le code a une boucle de jeu à thread unique dans la méthode [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) qui utilise **CoreProcessEventsOption::ProcessOneAndAllPending**. L’utilisation de cette option envoie tous les événements actuellement disponibles en file d’attente. Si aucun événement n’est en attente, la boucle de jeu attend leur apparition.

``` syntax
void App::Run()
{
    
    while (!m_windowClosed)
    {
        // Wait for system events or input from the user.
        // ProcessOneAndAllPending will block the thread until events appear and are processed.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);

        // If any of the events processed resulted in a need to redraw the window contents, then we will re-render the
        // scene and present it to the display.
        if (m_updateWindow || m_state->StateChanged())
        {
            m_main->Render();
            m_deviceResources->Present();

            m_updateWindow = false;
            m_state->Validate();
        }
    }
}
```

## <a name="scenario-2-render-on-demand-with-transient-animations"></a>Scénario 2 : rendu à la demande avec animations de transition


Dans la deuxième itération, le jeu est modifié afin qu’au moment où l’utilisateur sélectionne une pièce de puzzle et appuie sur sa destination, la pièce s’anime à l’écran jusqu’à ce qu’elle atteigne sa destination.

Comme précédemment, le code a une boucle de jeu à thread unique qui utilise **ProcessOneAndAllPending** pour répartir les événements d’entrée en file d’attente. La différence est la suivante: durant une animation, la boucle utilise **CoreProcessEventsOption::ProcessAllIfPresent** afin de ne pas attendre de nouveaux événements d’entrée. Si aucun événement n’est en attente, [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) est immédiatement retourné et permet à l’application de présenter l’image suivante de l’animation. Quand l’animation est terminée, la boucle revient à **ProcessOneAndAllPending** pour limiter les mises à jour de l’écran.

``` syntax
void App::Run()
{

    while (!m_windowClosed)
    {
        // 2. Switch to a continuous rendering loop during the animation.
        if (m_state->Animating())
        {
            // Process any system events or input from the user that is currently queued.
            // ProcessAllIfPresent will not block the thread to wait for events. This is the desired behavior when
            // you are trying to present a smooth animation to the user.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_state->Update();
            m_main->Render();
            m_deviceResources->Present();
        }
        else
        {
            // Wait for system events or input from the user.
            // ProcessOneAndAllPending will block the thread until events appear and are processed.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);

            // If any of the events processed resulted in a need to redraw the window contents, then we will re-render the
            // scene and present it to the display.
            if (m_updateWindow || m_state->StateChanged())
            {
                m_main->Render();
                m_deviceResources->Present();

                m_updateWindow = false;
                m_state->Validate();
            }
        }
    }
}
```

Pour prendre en charge la transition entre **ProcessOneAndAllPending** et **ProcessAllIfPresent**, l’application doit suivre l’état afin de savoir si l’animation est en cours. Pour ce faire, dans l’application puzzle, ajoutez une nouvelle méthode qui peut être appelée durant la boucle de jeu sur la classe GameState. La branche d’animation de la boucle de jeu entraîne des mises à jour de l’état de l’animation via l’appel de la nouvelle méthode Update de GameState.

## <a name="scenario-3-render-60-frames-per-second"></a>Scénario 3 : rendu de 60 images par seconde


Dans la troisième itération, l’application affiche un minuteur qui indique à l’utilisateur le temps qu’il a passé sur le puzzle. Dans la mesure où elle affiche le temps écoulé à la milliseconde près, elle doit afficher 60 images par seconde pour garder l’affichage à jour.

Comme dans les scénarios 1 et 2, l’application comporte une boucle de jeu à thread unique. La différence avec ce scénario est la suivante: dans la mesure où le rendu est constant, l’application n’a plus besoin de suivre les modifications d’état du jeu comme dans les deux premiers scénarios. Ainsi, elle peut utiliser **ProcessAllIfPresent** par défaut pour le traitement des événements. Si aucun événement n’est en attente, **ProcessEvents** est immédiatement retourné et affiche ensuite l’image suivante.

``` syntax
void App::Run()
{

    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            // 3. Continuously render frames and process system events and input as they appear in the queue.
            // ProcessAllIfPresent will not block the thread to wait for events. This is the desired behavior when
            // trying to present smooth animations to the user.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_state->Update();
            m_main->Render();
            m_deviceResources->Present();
        }
        else
        {
            // 3. If the window isn't visible, there is no need to continuously render.
            // Process events as they appear until the window becomes visible again.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

Cette approche est la plus simple pour écrire un jeu, car il n’est pas nécessaire de suivre un état supplémentaire pour déterminer le moment où un affichage doit être effectué. Elle permet d’atteindre le rendu le plus rapide possible avec une réactivité raisonnable par rapport aux entrées en fonction d’un intervalle de minuteur.

Toutefois, cette facilité de développement a un prix. Le rendu à 60images par seconde consomme plus d’énergie que le rendu à la demande. Il est préférable d’utiliser **ProcessAllIfPresent** quand le jeu change, ce qui est affiché à chaque image. Cela entraîne également une augmentation de la latence d’entrée de 16,7ms, car l’application bloque désormais la boucle de jeu en fonction de l’intervalle de synchronisation de l’affichage au lieu de **ProcessEvents**. Certains événements d’entrée peuvent être annulés, car la file d’attente n’est traitée qu’une seule fois par image (60Hz).

## <a name="scenario-4-render-60-frames-per-second-and-achieve-the-lowest-possible-input-latency"></a>Scénario 4 : rendu de 60 images par seconde avec la latence d’entrée la plus basse possible


Certains jeux peuvent ignorer ou compenser l’augmentation de la latence d’entrée décrite au scénario 3. Toutefois, si une faible latence d’entrée est essentielle pour l’expérience du jeu et les sensations des joueurs, les jeux qui affichent 60 images par seconde doivent traiter les entrées sur un thread distinct.

La quatrième itération du jeu de puzzle se fonde sur le scénario 3 en séparant le traitement des entrées et le rendu graphique de la boucle de jeu en threads distincts. Avec des threads distincts, chaque entrée est assurée de ne jamais être retardée par la sortie graphique. Cependant, il en résulte un code plus complexe. Dans le scénario4, le thread d’entrée appelle [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) avec [**CoreProcessEventsOption::ProcessUntilQuit**](https://msdn.microsoft.com/library/windows/apps/br208217), qui attend les nouveaux événements et répartit tous les événements disponibles. Il garde ce comportement jusqu’à ce que la fenêtre soit fermée ou que le jeu appelle [**CoreWindow::Close**](https://msdn.microsoft.com/library/windows/apps/br208260).

``` syntax
void App::Run()
{
    // 4. Start a thread dedicated to rendering and dedicate the UI thread to input processing.
    m_main->StartRenderThread();

    // ProcessUntilQuit will block the thread and process events as they appear until the App terminates.
    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
}

void JigsawPuzzleMain::StartRenderThread()
{
    // If the render thread is already running, then do not start another one.
    if (IsRendering())
    {
        return;
    }

    // Create a task that will be run on a background thread.
    auto workItemHandler = ref new WorkItemHandler([this](IAsyncAction^ action)
    {
        // Notify the swap chain that this app intends to render each frame faster
        // than the display's vertical refresh rate (typically 60 Hz). Apps that cannot
        // deliver frames this quickly should set this to 2.
        m_deviceResources->SetMaximumFrameLatency(1);

        // Calculate the updated frame and render once per vertical blanking interval.
        while (action->Status == AsyncStatus::Started)
        {
            // Execute any work items that have been queued by the input thread.
            ProcessPendingWork();

            // Take a snapshot of the current game state. This allows the renderers to work with a
            // set of values that won't be changed while the input thread continues to process events.
            m_state->SnapState();

            m_sceneRenderer->Render();
            m_deviceResources->Present();
        }

        // Ensure that all pending work items have been processed before terminating the thread.
        ProcessPendingWork();
    });

    // Run the task on a dedicated high priority background thread.
    m_renderLoopWorker = ThreadPool::RunAsync(workItemHandler, WorkItemPriority::High, WorkItemOptions::TimeSliced);
}
```

Le modèle **DirectX 11 et application XAML (Windows universel)** dans Studio2015 Visual Microsoft sépare la boucle de jeu en plusieurs threads de manière similaire. Il utilise l’objet [**Windows::UI::Core::CoreIndependentInputSource**](https://msdn.microsoft.com/library/windows/apps/dn298460) pour démarrer un thread dédié à la gestion des entrées et crée également un thread de rendu indépendant du thread d’interface utilisateur XAML. Pour plus de détails sur ces modèles, voir [Créer un projet de jeu de plateforme Windows universelle et DirectX à partir d’un modèle](user-interface.md).

## <a name="additional-ways-to-reduce-input-latency"></a>Autres façons de réduire la latence d’entrée


### <a name="use-waitable-swap-chains"></a>Utiliser des chaînes d’échange d’attente

Les jeux DirectX répondent aux entrées de l’utilisateur en mettant à jour ce que ce dernier voit à l’écran. L’affichage d’un écran de 60 Hz est rafraîchi toutes les 16,7 ms (1 seconde/60 images). La figure 1 illustre approximativement le cycle de vie et la réponse à un événement d’entrée par rapport au signal de rafraîchissement à 16,7 ms (VBlank) pour une application qui affiche 60 images par seconde :

Figure 1

![Figure 1: latence d’entrée dans DirectX ](images/input-latency1.png)

Dans Windows8.1, DXGI a introduit l’indicateur **DXGI\_SWAP\_CHAIN\_FLAG\_FRAME\_LATENCY\_WAITABLE\_OBJECT** pour la chaîne d’échange, ce qui permet aux applications de réduire facilement cette latence sans devoir implémenter des heuristiques afin de garder la file d’attente actuelle vide. Les chaînes d’échange créées avec cet indicateur sont appelées chaînes d’échange d’attente. La figure 2 illustre approximativement le cycle de vie et la réponse à un événement d’entrée durant l’utilisation de chaînes d’échange d’attente :

Figure 2

![Figure 2 : latence d’entrée dans une chaîne d’échange d’attente DirectX](images/input-latency2.png)

Ces schémas nous montrent que les jeux peuvent réduire la latence d’entrée de deuximages complètes s’ils sont capables d’afficher et de présenter chaque image dans la limite des 16,7ms définie par le taux de rafraîchissement de l’écran. L’exemple de jeu de puzzle utilise les chaînes d’échange d’attente et contrôle la limite de la file d’attente actuelle en appelant:` m_deviceResources->SetMaximumFrameLatency(1);`

 

 




