---
author: abbycar
title: Ajouter une interface utilisateur
description: Découvrez comment ajouter une superposition de l’interface utilisateur 2D à un jeu UWP DirectX.
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.author: abigailc
ms.date: 10/24/2017
ms.topic: article
keywords: windows10, uwp, jeux, interface utilisateur, directx
ms.localizationpriority: medium
ms.openlocfilehash: 9962cc9043bd650390721715ca73b2e85a219c25
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5691054"
---
# <a name="add-a-user-interface"></a>Ajouter une interface utilisateur


Maintenant que notre jeu possède ses visuels 3D en place, il est temps pour vous concentrer sur l’ajout de certains éléments 2D afin que le jeu peut fournir des commentaires sur l’état du jeu au joueur. Cela peut être obtenu en ajoutant de simples options de menu et composants d’affichage à tête haute sur le graphique 3D de pipeline sortie.

>[!Note]
>Si vous n’avez pas encore téléchargé le dernier code de jeu pour cet exemple, accédez à [Exemple de jeu Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Cet exemple fait partie d’une vaste collection d’exemples de fonctionnalités UWP. Pour obtenir des instructions sur le téléchargement de l’exemple, consultez [Obtenir des exemples de la plateforme Windows universelle (UWP) à partir de GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objectif

À l’aide de Direct2D, ajoutez un certain nombre de comportements et graphiques d’interface utilisateur à notre jeu UWP DirectX, y compris:
- Affichage à tête haute, y compris les rectangles du [contrôleur de déplacement / vue](tutorial--adding-controls.md) de frontière
- Menus de l’état du jeu


## <a name="the-user-interface-overlay"></a>Superposition de l’interface utilisateur


Bien qu’il existe de nombreuses façons d’afficher des éléments d’interface utilisateur et de texte dans un jeu DirectX, nous allons le focus sur l’utilisation de [Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx). Nous allons également utiliser [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) pour les éléments de texte.


Direct2D est qu'un ensemble d’API de dessin 2D utilisées pour dessiner des effets et des primitives en pixels. Lorsque vous commencez à avec Direct2D, il est préférable de simplifier les choses. Des comportements d’interface et dispositions complexes nécessitent du temps et une certaine planification. Si votre jeu nécessite une interface utilisateur complexe, comme celles de simulation ou de jeux de stratégie, envisagez plutôt d’utiliser XAML.

> [!NOTE]
> Pour plus d’informations sur le développement d’une interface utilisateur avec XAML dans un jeu UWP DirectX, voir [l’extension de l’exemple de jeu](tutorial-resources.md).

Direct2D n’est pas spécifiquement conçu pour les interfaces utilisateur ni les dispositions comme HTML et XAML. Il ne fournit pas les composants d’interface utilisateur tels que des listes, des zones ou des boutons. Il n’a pas également fournir des composants de disposition, div, les tables ou les grilles.


Pour cet exemple de jeu, nous avons deux composants principaux: l’interface utilisateur.
1. Un affichage à tête haute pour le score et les contrôles dans le jeu.
2. Une superposition utilisée pour afficher le texte de l’état du jeu et les options telles que des informations sur la suspension et au niveau des options de démarrage.

### <a name="using-direct2d-for-a-heads-up-display"></a>Utilisation de Direct2D pour l’affichage à tête haute

L’image suivante montre de l’affichage à tête haute dans le jeu. Il est clair, ce qui permet au joueur de se concentrer sur la navigation dans le monde en 3D et les cibles de tir et simple. Une interface ou un affichage à tête haute ne doit jamais compliquer la capacité du joueur à traiter et à réagir aux événements dans le jeu.

![Capture d’écran de la superposition du jeu](images/simple-dx-game-ui-overlay.png)

La superposition comprend les primitives de base suivantes.
- Texte [**DirectWrite**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038) dans le coin supérieur droit et informe le joueur du 
    - Coups réussis
    - Nombre de tirs qu’il a effectués
    - Temps restant à ce niveau
    - Numéro de niveau actuel 
- Deux intersection des segments de ligne utilisés pour former un réticule
- Deux rectangles dans les coins en bas pour le [contrôleur de déplacement / vue](tutorial--adding-controls.md) fonctionne. 


L’état d’affichage à tête haute de dans le jeu de la superposition est écrit dans la méthode [**GameHud::Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358) de la classe [**GameHud**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h) . Dans cette méthode, la superposition Direct2D qui représente notre interface utilisateur est mise à jour pour refléter les modifications du nombre de coups, temps restant et au niveau numéro.

Si le jeu a été initialisé, nous ajoutons `TotalHits()`, `TotalShots()`, et `TimeRemaining()` à un [**swprintf_s**](https://docs.microsoft.com/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l) de la mémoire tampon et spécifiez le format d’impression. Nous pouvons ensuite dessiner à l’aide de la méthode [**DrawText**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd742848) . Nous faire de même pour l’indicateur de niveau en cours, dessiner des nombres vide pour afficher des niveaux en cours comme ➀ et des nombres remplis comme ➊ pour afficher que le niveau spécifique s’est déroulée.


L’extrait de code suivant vous guide tout au long processus de la méthode **GameHud::Render** pour 
- Création d’une image Bitmap à l’aide [** ID2D1RenderTarget::DrawBitmap **](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371880)
- La section désactivant les zones de l’interface utilisateur en rectangles à l’aide de [ **D2D1::RectF**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368184)
- À l’aide de **DrawText** pour rendre les éléments de texte

```cpp
void GameHud::Render(_In_ Simple3DGame^ game)
{
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
    auto windowBounds = m_deviceResources->GetLogicalSize();

    if (m_showTitle)
    {
        d2dContext->DrawBitmap(
            m_logoBitmap.Get(),
            D2D1::RectF(
                GameConstants::Margin,
                GameConstants::Margin,
                m_logoSize.width + GameConstants::Margin,
                m_logoSize.height + GameConstants::Margin
                )
            );
        d2dContext->DrawTextLayout(
            Point2F(m_logoSize.width + 2.0f * GameConstants::Margin, GameConstants::Margin),
            m_titleHeaderLayout.Get(),
            m_textBrush.Get()
            );
        d2dContext->DrawTextLayout(
            Point2F(GameConstants::Margin, m_titleBodyVerticalOffset),
            m_titleBodyLayout.Get(),
            m_textBrush.Get()
            );
    }

    // Draw text for number of hits, total shots, and time remaining
    if (game != nullptr)
    {
        // This section is only used after the game state has been initialized.
        static const int bufferLength = 256;
        static char16 wsbuffer[bufferLength];
        int length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"Hits:\t%10d\nShots:\t%10d\nTime:\t%8.1f",
            game->TotalHits(),
            game->TotalShots(),
            game->TimeRemaining()
            );
        
        // Draw the upper right portion of the HUD displaying total hits, shots, and time remaining
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBody.Get(),
            D2D1::RectF(
                windowBounds.Width - GameConstants::HudRightOffset,
                GameConstants::HudTopOffset,
                windowBounds.Width,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize + GameConstants::Margin) * 3
                ),
            m_textBrush.Get()
            );

        // Using the unicode characters starting at 0x2780 ( ➀ ) for the consecutive levels of the game.
        // For completed levels start with 0x278A ( ➊ ) (This is 0x2780 + 10).
        uint32 levelCharacter[6];
        for (uint32 i = 0; i < 6; i++)
        {
            levelCharacter[i] = 0x2780 + i + ((static_cast<uint32>(game->LevelCompleted()) == i) ? 10 : 0);
        }
        length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"%lc %lc %lc %lc %lc %lc",
            levelCharacter[0],
            levelCharacter[1],
            levelCharacter[2],
            levelCharacter[3],
            levelCharacter[4],
            levelCharacter[5]
            );
        // Create a new rectangle and draw the current level info text inside
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBodySymbol.Get(),
            D2D1::RectF(
                windowBounds.Width - GameConstants::HudRightOffset,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize + GameConstants::Margin) * 3 + GameConstants::Margin,
                windowBounds.Width,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize+ GameConstants::Margin) * 4
                ),
            m_textBrush.Get()
            );

        if (game->IsActivePlay())
        {
            // Draw the move and fire rectangles
            // Draw the crosshairs
        }
    }
}
```

Interruption de la méthode de bas en outre, cet élément de la méthode dessine [**GameHud::Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358) notre déplacement et de tir rectangles avec [**ID2D1RenderTarget::DrawRectangle**](https://msdn.microsoft.com/library/windows/desktop/dd371902)et le réticule à l’aide de deux appels à [**ID2D1RenderTarget::DrawLine**](https://msdn.microsoft.com/library/windows/desktop/dd371895).

```cpp
        // Check if game is playing
        if (game->IsActivePlay())
        {
            // Draw a rectangle for the touch input for the move control.
            d2dContext->DrawRectangle(
                D2D1::RectF(
                    0.0f,
                    windowBounds.Height - GameConstants::TouchRectangleSize,
                    GameConstants::TouchRectangleSize,
                    windowBounds.Height
                    ),
                m_textBrush.Get()
                );
            // Draw a rectangle for the touch input of the fire control.
            d2dContext->DrawRectangle(
                D2D1::RectF(
                    windowBounds.Width - GameConstants::TouchRectangleSize,
                    windowBounds.Height - GameConstants::TouchRectangleSize,
                    windowBounds.Width,
                    windowBounds.Height
                    ),
                m_textBrush.Get()
                );

            // Draw two lines to form crosshairs
            d2dContext->DrawLine(
                D2D1::Point2F(windowBounds.Width / 2.0f - GameConstants::CrossHairHalfSize, windowBounds.Height / 2.0f),
                D2D1::Point2F(windowBounds.Width / 2.0f + GameConstants::CrossHairHalfSize, windowBounds.Height / 2.0f),
                m_textBrush.Get(),
                3.0f
                );
            d2dContext->DrawLine(
                D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f - GameConstants::CrossHairHalfSize),
                D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f + GameConstants::CrossHairHalfSize),
                m_textBrush.Get(),
                3.0f
                );
        }
```

Dans la méthode **GameHud::Render** nous stockons la taille logique de la fenêtre du jeu dans le `windowBounds` variable. Cet exemple utilise le [`GetLogicalSize`](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) méthode de la classe **DeviceResources** . 
```cpp
auto windowBounds = m_deviceResources->GetLogicalSize();
```

 Obtention de la taille de la fenêtre de jeu est essentielle pour la programmation de l’interface utilisateur. La taille de la fenêtre est fournie dans une mesure appelée DIP (device independent pixels), où un DIP est défini comme 1/96e de pouce. Direct2D met à l’échelle les unités de dessin en pixels réel lors du dessin, pour ce faire à l’aide de la Windows paramètre de points par pouce (PPP). De même, lorsque vous dessinez le texte à l’aide de [**DirectWrite**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038), vous spécifiez DIP au lieu des points de la taille de la police. Les DIP sont exprimés sous la forme de nombres à virgule flottante.

 

### <a name="displaying-game-state-info"></a>Afficher des informations d’état du jeu

Outre l’affichage à tête haute, l’exemple de jeu a une superposition qui représente les six États de jeu. Tous les États de fonctionnalité une primitive de grand rectangle noir avec du texte pour le joueur doit lire. Les rectangles du contrôleur de déplacement / vue et le réticule n’est pas tracé, car ils ne sont pas actifs dans ces États.

La superposition est créée à l’aide de la classe [**GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h) , ce qui nous permet de basculer en texte qui s’affiche pour s’aligner sur l’état du jeu.

![action de superposition et d’état](images/simple-dx-game-ui-finaloverlay.png)

La superposition est divisée en deux sections: **état** et **Action**. Le point **d’état** est détaillée vers le bas en rectangles de **titre** et de **corps** . La section **Action** a uniquement un rectangle. Chaque rectangle a un rôle différent.

-   `titleRectangle` contient le texte du titre.
-   `bodyRectangle` contient le texte de corps.
-   `actionRectangle` contient le texte indiquant au joueur d’entreprendre une action spécifique.

Le jeu possède six États qui peuvent être définies. L’état du jeu transmis à l’aide de la partie de **l’état** de la superposition. Les rectangles **d’état** sont mis à jour à l’aide d’un certain nombre de méthodes correspondant avec les états suivants.

- Chargement
- Statistiques de score de début/haute initiale
- Début de niveau
- Jeu en pause
- Fin du jeu
- Jeu a gagné


La partie de **l’Action** de la superposition est mise à jour à l’aide de la méthode [**GameInfoOverlay::SetAction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) , ce qui permet le texte d’action être défini sur l’une des opérations suivantes.
- «Appuyez pour jouer à nouveau …»
- «De niveau de chargement, veuillez patienter …»
- «Appuyez pour continuer …»
- None

> [!NOTE]
> Ces deux méthodes processus seront décrit plus en détail dans la section [représentant l’état du jeu](#representing-game-state) .

Champs de texte sont ajustées en fonction de la que se passe-t-il dans le jeu, **l’état** et la section **Action** .
Examinons comment nous initialiser et tracer la superposition pour ces six états.

### <a name="initializing-and-drawing-the-overlay"></a>Initialisation et tracé de la superposition

Les six États **d’état** ont en commun quelques points, rendre les ressources et les méthodes dont ils ont besoin très similaire.
    - Ils utilisent tous un rectangle noir au centre de l’écran que l’arrière-plan.
    - Le texte affiché est **titre** ou **corps de** texte.
    - Le texte utilise la police Segoe UI et est écrit au-dessus du rectangle noir. 


L’exemple de jeu dispose de quatre méthodes qui entrent en jeu lors de la création de la superposition.
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
Le constructeur de [**GameInfoOverlay::GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78) initialise la superposition, en conservant la surface de l’image bitmap que nous utiliserons pour afficher des informations au joueur sur. Le constructeur obtient une fabrique de l’objet [**ID2D1Device**](https://msdn.microsoft.com/library/windows/desktop/hh404478) transmis, qu’il utilise pour créer un [**ID2D1DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/hh404479) que l’objet de superposition proprement dit peut dessiner dans. [IDWriteFactory::CreateTextFormat](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368203) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>GameInfoOverlay::CreateDeviceDependentResources
[**GameInfoOverlay::CreateDeviceDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) est notre méthode pour créer des pinceaux qui sera utilisé pour dessiner notre texte. Pour ce faire, nous obtenir un objet [**ID2D1DeviceContext2**](https://msdn.microsoft.com/en-us/library/windows/desktop/dn890789) qui permet la création et de dessin de géométrie, ainsi que des fonctionnalités, telles que l’entrée manuscrite et dégradé de maillage rendu. Ensuite, nous créons une série de pinceaux en couleur à l’aide de [**ID2D1SolidColorBrush**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd372207) pour dessiner les éléments de l’interface utilisateur folling.
- Pinceau noir pour les arrière-plans du rectangle
- Pinceau blanc pour le texte d’état
- Pinceau orange pour le texte d’action

#### <a name="deviceresourcessetdpi"></a>DeviceResources::SetDpi
La méthode [**DeviceResources::SetDpi**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527) définit les points par pouce de la fenêtre. Cette méthode est appelée lorsque la résolution est modifiée et doit être réajustées qui se produit lorsque la fenêtre de jeu est redimensionnée. Après la mise à jour de la résolution, cette méthode appelle également[**DeviceResources::CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487) pour vous assurer que les ressources nécessaires sont recréés chaque fois que la fenêtre est redimensionnée.


#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources
La méthode [**GameInfoOverlay::CreateWindowsSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225) est où tous nos dessin a lieu. Voici les étapes de la méthode.
- Trois rectangles sont créés à la section désactiver le texte de l’interface utilisateur pour le texte de **titre**, le **corps**et **Action** .
    ```cpp 
    m_titleRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        GameInfoOverlayConstant::TopMargin,
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        GameInfoOverlayConstant::TopMargin + GameInfoOverlayConstant::TitleHeight
        );
    m_actionRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        overlaySize.height - (GameInfoOverlayConstant::ActionHeight + GameInfoOverlayConstant::BottomMargin),
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        overlaySize.height - GameInfoOverlayConstant::BottomMargin
        );
    m_bodyRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        m_titleRectangle.bottom + GameInfoOverlayConstant::Separator,
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        m_actionRectangle.top - GameInfoOverlayConstant::Separator
        );
    ```

- Une image Bitmap est créée nommée `m_levelBitmap`, à l’aide de **CreateBitmap**de compte tenu de la résolution en cours.
- `m_levelBitmap` est défini à l’aide de [**ID2D1DeviceContext::SetTarget**](https://msdn.microsoft.com/en-us/library/windows/desktop/hh404533)de cible de rendu de notre 2D.
- L’image Bitmap est désactivée avec chaque pixel apportée noir avec [**ID2D1RenderTarget::Clear**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371772).
- [**ID2D1RenderTarget::beginDraw**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371768) est appelée pour lancer le dessin. 
- **DrawText** est appelée pour dessiner le texte stocké dans `m_titleString`, `m_bodyString`, et `m_actionString` dans le rectangle appropriée à l’aide de **ID2D1SolidColorBrush**correspondante.
- [**ID2D1RenderTarget::EndDraw**](ID2D1RenderTarget::EndDraw) est appelé pour arrêter toutes les opérations de dessin sur `m_levelBitmap`.
- Une autre image Bitmap est créée à l’aide de **CreateBitmap** nommé `m_tooSmallBitmap` à utiliser comme un secours, en affichant uniquement si la configuration de l’affichage est trop petite pour le jeu.
- Répétez le processus pour le dessin sur `m_levelBitmap` pour `m_tooSmallBitmap`, cette fois uniquement le dessin la chaîne `Paused` dans le corps.




Il que nous suffit sont désormais six méthodes pour remplir le texte de nos États de six superposition!

### <a name="representing-game-state"></a>Représentant l’état du jeu


Chacun des États de six superposition dans le jeu a une méthode correspondante dans l’objet **GameInfoOverlay** . Ces méthodes tracent une variation de la superposition pour communiquer des informations explicites au joueur sur le jeu proprement dit. Cette communication est représentée par une chaîne de **titre** et de **corps** . Dans la mesure où l’exemple a déjà configuré les ressources et la disposition pour ces informations lorsqu’il a été initialisé et avec la méthode [**GameInfoOverlay::CreateDeviceDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) , il doit uniquement fournir la superposition des chaînes propres à l’état.

La partie de **l’état** de la superposition est définie avec un appel à l’une des méthodes suivantes.

État du jeu | État de définie la méthode | Champs d’état
:----- | :------- | :---------
Chargement | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**Title**</br>Chargement des ressources </br>**Body**</br> Imprime de manière incrémentielle «.» pour implique l’activité de chargement.
Statistiques de score de début/haute initiale | [GameInfoOverlay::SetGameStats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**Title**</br>Meilleur Score</br> **Body**</br> Niveaux terminé # </br>Total des Points #</br>Total captures de #
Début de niveau | [GameInfoOverlay::SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**Title**</br>Niveau #</br>**Body**</br>Description du niveau objectif.
Jeu en pause | [GameInfoOverlay::SetPause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**Title**</br>Jeu en pause</br>**Body**</br>None
Fin du jeu | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Title**</br>Fin du jeu</br> **Body**</br> Niveaux terminé # </br>Total des Points #</br>Total captures de #</br>Niveaux terminé #</br>Haute Score #
Jeu a gagné | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Title**</br>Vous avez GAGNÉ!</br> **Body**</br> Niveaux terminé # </br>Total des Points #</br>Total captures de #</br>Niveaux terminé #</br>Haute Score #




Avec la méthode [**GameInfoOverlay::CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134) , l’exemple déclaré trois zones rectangulaires qui correspondent à des zones spécifiques de la superposition.



En gardant ces zones à l’esprit, examinons l’une des méthodes propres à l’état, **GameInfoOverlay::SetGameStats**, et voyons comment la superposition est tracée.

```cpp
void GameInfoOverlay::SetGameStats(int maxLevel, int hitCount, int shotCount)
{
    int length;

    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    d2dContext->SetTarget(m_levelBitmap.Get());
    d2dContext->BeginDraw();
    d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    d2dContext->FillRectangle(&m_titleRectangle, m_backgroundBrush.Get());
    d2dContext->FillRectangle(&m_bodyRectangle, m_backgroundBrush.Get());
    m_titleString = "High Score";

    d2dContext->DrawText(
        m_titleString->Data(),
        m_titleString->Length(),
        m_textFormatTitle.Get(),
        m_titleRectangle,
        m_textBrush.Get()
        );
    length = swprintf_s(
        wsbuffer,
        bufferLength,
        L"Levels Completed %d\nTotal Points %d\nTotal Shots %d",
        maxLevel,
        hitCount,
        shotCount
        );
    m_bodyString = ref new Platform::String(wsbuffer, length);
    d2dContext->DrawText(
        m_bodyString->Data(),
        m_bodyString->Length(),
        m_textFormatBody.Get(),
        m_bodyRectangle,
        m_textBrush.Get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device.  All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call.   At that point, the sample will recreate the device
        // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        DX::ThrowIfFailed(hr);
    }
}
```

À l’aide du contexte de périphérique Direct2D que l’objet **GameInfoOverlay** initialisé, cette méthode remplit les rectangles de titre et de corps avec noir avec le pinceau d’arrière-plan. Elle écrit le texte pour la chaîne «Meilleur score» dans le rectangle de titre, ainsi qu’une chaîne contenant les informations mises à jour sur l’état du jeu dans le rectangle de corps en utilisant le pinceau de texte blanc.


Le rectangle d’action est mis à jour par un appel ultérieur à [**GameInfoOverlay::SetAction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) à partir d’une méthode sur l’objet **GameMain** , qui fournit les informations d’état du jeu nécessaires par **GameInfoOverlay::SetAction** pour déterminer le message approprié à le lecteur, par exemple, «Appuyez pour continuer».

La superposition pour tout état donné est choisie dans la méthode [**GameMain::SetGameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661) comme suit:

```cpp
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading();
        break;

    case GameInfoOverlayState::GameStats:
        m_uiControl->SetGameStats(
            m_game->HighScore().levelCompleted + 1,
            m_game->HighScore().totalHits,
            m_game->HighScore().totalShots
            );
        break;

    case GameInfoOverlayState::LevelStart:
        m_uiControl->SetLevelStart(
            m_game->LevelCompleted() + 1,
            m_game->CurrentLevel()->Objective(),
            m_game->CurrentLevel()->TimeLimit(),
            m_game->BonusTime()
            );
        break;

    case GameInfoOverlayState::GameOverCompleted:
        m_uiControl->SetGameOver(
            true,
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::GameOverExpired:
        m_uiControl->SetGameOver(
            false,
            m_game->LevelCompleted(),
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::Pause:
        m_uiControl->SetPause(
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->TimeRemaining()
            );
        break;
    }
}
```

Le jeu dispose maintenant d’un moyen de communiquer des informations de texte au joueur en fonction de l’état du jeu, et nous avons un moyen de commutation ce qui est affiché à leur tout au long du jeu.

### <a name="next-steps"></a>Étapes suivantes

Dans la rubrique suivante, [Ajout de contrôles](tutorial--adding-controls.md), nous étudions la façon dont le joueur interagit avec l’exemple de jeu et comment les entrées modifient l’état du jeu.



 




