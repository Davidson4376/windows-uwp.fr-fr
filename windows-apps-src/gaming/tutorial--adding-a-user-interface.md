---
title: Ajouter une interface utilisateur
description: Découvrez comment ajouter un segment de recouvrement interface utilisateur 2D à un jeu DirectX UWP.
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, jeux, interface utilisateur, directx
ms.localizationpriority: medium
ms.openlocfilehash: ef966901534302c505ddad37bd277d9141b512a1
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367863"
---
# <a name="add-a-user-interface"></a>Ajouter une interface utilisateur


Maintenant que notre jeu a ses visuels 3D en place, il est temps de se concentrer sur l’ajout de certains éléments 2D afin que le jeu puisse fournir des commentaires sur l’état du jeu à l’acteur. Cela est possible en ajoutant des options de menu simple et un affichage tête haute des composants sur les graphiques 3D pipeline de sortie.

>[!Note]
>Si vous n’avez pas encore téléchargé le dernier code de jeu pour cet exemple, accédez à [Exemple de jeu Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Cet exemple fait partie d’une vaste collection d’exemples de fonctionnalités UWP. Pour obtenir des instructions sur le téléchargement de l’exemple, consultez [Obtenir des exemples de la plateforme Windows universelle (UWP) à partir de GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objectif

À l’aide de Direct2D, ajoutez un nombre de graphiques d’interface utilisateur et comportements notre jeu UWP DirectX, avec notamment :
- Afficher de tête haute, y compris [déplacement apparence contrôleur](tutorial--adding-controls.md) frontière rectangles
- Menus de l’état du jeu


## <a name="the-user-interface-overlay"></a>Superposition de l’interface utilisateur


Bien qu’il existe de nombreuses façons d’afficher des éléments d’interface utilisateur et le texte dans un jeu DirectX, nous allons le focus sur l’utilisation de [Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal). Nous allons également utiliser [DirectWrite](https://docs.microsoft.com/windows/desktop/DirectWrite/direct-write-portal) pour les éléments de texte.


Direct2D est qu'un ensemble d’API de dessin 2D utilisé pour dessiner les effets et primitives en pixels. Quand vous commencez à avec Direct2D, il est préférable de simplifier les choses. Des comportements d’interface et dispositions complexes nécessitent du temps et une certaine planification. Si votre jeu requiert une interface utilisateur complexe, comme celles indiquées dans la simulation et jeux de stratégie, envisagez d’utiliser à la place de XAML.

> [!NOTE]
> Pour plus d’informations sur le développement d’une interface utilisateur avec XAML dans un jeu UWP DirectX, consultez [extension de l’exemple de jeu](tutorial-resources.md).

Direct2D n’est pas conçue spécifiquement pour les interfaces utilisateur ou des configurations telles que HTML et XAML. Il ne fournit pas les composants d’interface utilisateur comme des listes, des zones ou des boutons. Elle n’également fournir des composants de mise en page telles que les balises div, des tables ou des grilles.


Pour cet exemple de jeu, nous avons deux principaux composants d’interface utilisateur.
1. Un affichage tête haute pour le score et les contrôles dans le jeu.
2. Une superposition utilisée pour afficher le texte de l’état du jeu et des options telles que les informations de pause et au niveau des options de démarrage.

### <a name="using-direct2d-for-a-heads-up-display"></a>Utilisation de Direct2D pour l’affichage à tête haute

L’illustration suivante montre l’affichage tête haute de la partie de l’exemple. Il est simple et ne soit pas encombré, afin que le lecteur se concentrer sur la navigation dans le monde en 3D et le tir cibles. Une bonne interface ou un affichage tête haute ne doit jamais compliquer la capacité du lecteur pour traiter et réagir aux événements dans le jeu.

![Capture d’écran de la superposition du jeu](images/simple-dx-game-ui-overlay.png)

La superposition comprend les primitives de base suivantes.
- [**DirectWrite** ](https://docs.microsoft.com/windows/desktop/DirectWrite/direct-write-portal) texte dans le coin supérieur droit qui informe le lecteur de 
    - Correspondances réussies
    - Nombre de captures que le joueur a effectué
    - Temps restant dans le niveau
    - Numéro de niveau actuel 
- Deux segments de ligne utilisés pour former une croix d’intersection
- Deux rectangles aux angles bas pour le [déplacement apparence contrôleur](tutorial--adding-controls.md) les limites. 


L’état d’affichage de titres de la partie de la superposition est dessiné dans le [ **GameHud::Render** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358) méthode de la [ **GameHud** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h) classe. Dans cette méthode, la superposition de Direct2D qui représente notre interface utilisateur est mise à jour pour refléter les modifications dans le nombre d’accès, le nombre restant et de niveau de temps.

Si le jeu a été initialisé, nous ajoutons `TotalHits()`, `TotalShots()`, et `TimeRemaining()` à un [ **swprintf_s** ](https://docs.microsoft.com/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l) mettre en mémoire tampon et spécifier le format d’impression. Nous pouvons ensuite dessiner à l’aide de la [ **DrawText** ](https://docs.microsoft.com/windows/desktop/Direct2D/id2d1rendertarget-drawtext) (méthode). Nous avons faites de même pour l’indicateur de niveau, de dessin vide numéros pour afficher les niveaux inachevées comme ➀ et rempli comme ➊ pour montrer que le niveau spécifique a été effectué.


L’extrait de code suivant Guide à travers le **GameHud::Render** processus de la méthode pour 
- Création d’un Bitmap avec [** ID2D1RenderTarget::DrawBitmap **](https://docs.microsoft.com/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawbitmap(id2d1bitmap_constd2d1_rect_f__float_d2d1_bitmap_interpolation_mode_constd2d1_rect_f_))
- Sectionnement hors des zones d’interface utilisateur dans des rectangles à l’aide de [ **D2D1::RectF**](https://docs.microsoft.com/windows/desktop/api/dcommon/ns-dcommon-d2d_rect_f)
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

Avec rupture de la méthode vers le bas en outre, cette partie de la [ **GameHud::Render** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358) méthode dessine notre mouvement et déclencher des rectangles avec [ **ID2D1RenderTarget::DrawRectangle** ](https://docs.microsoft.com/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawrectangle(constd2d1_rect_f__id2d1brush_float_id2d1strokestyle))et en forme de croix à l’aide de deux appels à [ **ID2D1RenderTarget::DrawLine**](https://docs.microsoft.com/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawline).

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

Dans le **GameHud::Render** méthode que nous stockons la taille logique de la fenêtre du jeu dans le `windowBounds` variable. Cet exemple utilise le [ `GetLogicalSize` ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) méthode de la **DeviceResources** classe. 
```cpp
auto windowBounds = m_deviceResources->GetLogicalSize();
```

 Obtention de la taille de la fenêtre du jeu est essentielle pour la programmation de l’interface utilisateur. La taille de la fenêtre est fournie dans une mesure appelée DIP (device independent pixels), où une adresse DIP est définie en tant que 1/96 de pouce. Direct2D met à l’échelle les unités de dessin aux pixels réels lorsque le dessin se produit, cela à l’aide des Windows paramètre points par pouce (PPP). De même, lorsque vous dessinez à l’aide du texte [ **DirectWrite**](https://docs.microsoft.com/windows/desktop/DirectWrite/direct-write-portal), vous spécifiez des adresses IP dynamiques plutôt que des points pour la taille de la police. Les DIP sont exprimés sous la forme de nombres à virgule flottante.

 

### <a name="displaying-game-state-info"></a>Affichage des informations d’état du jeu

Outre l’affichage tête haute, l’exemple de jeu comporte un segment de recouvrement qui représente les six États de jeu. Tous les États de fonctionnalité une primitive de grand rectangle noir avec du texte à lire par le lecteur. Les rectangles de contrôleur de déplacement apparence et le réticule n’est pas dessiné, car ils ne sont pas actives dans ces États.

La superposition est créée à l’aide de la [ **GameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h) (classe), ce qui nous permet d’extraire le texte à afficher pour s’aligner avec l’état du jeu.

![état et l’action de superposition](images/simple-dx-game-ui-finaloverlay.png)

La superposition est divisée en deux sections : **État** et **Action**. Le **état** point est subdivisé **titre** et **corps** rectangles. Le **Action** section a uniquement un seul rectangle. Chaque rectangle a un objectif différent.

-   `titleRectangle` contient le texte du titre.
-   `bodyRectangle` contient le texte du corps.
-   `actionRectangle` contient le texte qui informe le lecteur pour entreprendre une action spécifique.

Le jeu a six États qui peuvent être définies. L’état du jeu acheminée à l’aide du **état** partie de la superposition. Le **état** rectangles sont mis à jour à l’aide d’un nombre de méthodes qui correspondent aux États suivants.

- Chargement
- Statistiques de score de début/haute initiale
- Début de niveau
- Jeu suspendu
- Fin du jeu
- Jeu a gagné


Le **Action** partie de la superposition est mis à jour à l’aide de la [ **GameInfoOverlay::SetAction** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) méthode, ce qui permet le texte d’action à définir sur l’une des opérations suivantes.
- « Appuyez pour lire à nouveau... »
- « Niveau de chargement, veuillez patienter... »
- « Appuyez pour continuer... »
- Aucune

> [!NOTE]
> Ces deux méthodes seront abordées plus en détail dans le [représentant l’état du jeu](#representing-game-state) section.

Selon le que se passe-t-il dans le jeu, le **état** et **Action** les champs de texte de section sont ajustées.
Examinons comment nous initialiser et dessiner la superposition pour ces six états.

### <a name="initializing-and-drawing-the-overlay"></a>Initialisation et tracé de la superposition

Les six **état** États ont quelques éléments en commun, de rendre les ressources et les méthodes dont ils ont besoin très similaire.
    - Elles utilisent toutes un rectangle noir dans le centre de l’écran que l’arrière-plan.
    - Le texte affiché est soit **titre** ou **corps** texte.
    - Le texte utilise la police Segoe UI et est dessiné sur le rectangle précédent. 


L’exemple de jeu comporte quatre méthodes qui entrent en jeu lors de la création de la superposition.
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
Le [ **GameInfoOverlay::GameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78) constructeur initialise la superposition, en conservant la surface de bitmap que nous allons utiliser pour afficher les informations sur le lecteur sur. Le constructeur obtient une fabrique à partir de la [ **ID2D1Device** ](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1device) objet passé, qu’il utilise pour créer un [ **ID2D1DeviceContext** ](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1devicecontext) que l’objet de segment de recouvrement lui-même peut dessiner à. [IDWriteFactory::CreateTextFormat](https://docs.microsoft.com/windows/desktop/api/dwrite/nf-dwrite-idwritefactory-createtextformat) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>GameInfoOverlay::CreateDeviceDependentResources
[**GameInfoOverlay::CreateDeviceDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) est notre méthode pour créer des pinceaux qui est utilisée pour dessiner le texte de notre. Pour ce faire, nous obtenons un [ **ID2D1DeviceContext2** ](https://docs.microsoft.com/windows/desktop/api/d2d1_3/nn-d2d1_3-id2d1devicecontext2) objet qui permet la création et le dessin de géométrie, ainsi que des fonctionnalités telles que l’encre et de dégradé de maillage rendu. Ensuite, nous créons une série de pinceaux de couleur à l’aide de [ **ID2D1SolidColorBrush** ](https://docs.microsoft.com/windows/desktop/api/d2d1/nn-d2d1-id2d1solidcolorbrush) pour dessiner les éléments d’interface utilisateur folling.
- Pinceau noir pour les arrière-plans de rectangle
- Pinceau blanc pour le texte d’état
- Orange pinceau pour le texte de l’action

#### <a name="deviceresourcessetdpi"></a>DeviceResources::SetDpi
Le [ **DeviceResources::SetDpi** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527) méthode définit les points par pouce de la fenêtre. Cette méthode est appelée lorsque la résolution est modifiée et doit être réajustées ce qui se produit lorsque la fenêtre du jeu est redimensionnée. Après la mise à jour la valeur PPP, cette méthode appelle également[**DeviceResources::CreateWindowSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487) pour rendre nécessaire que les ressources sont recréées chaque fois que la fenêtre est redimensionnée.


#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources
Le [ **GameInfoOverlay::CreateWindowsSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225) méthode est où tout le dessin notre a lieu. Voici les étapes de la méthode.
- Trois rectangles sont créés à la section désactiver le texte de l’interface utilisateur pour le **titre**, **corps**, et **Action** texte.
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

- Une image Bitmap est créée, appelée `m_levelBitmap`, tenant le compte à l’aide de la résolution actuelle **CreateBitmap**.
- `m_levelBitmap` est défini comme notre à l’aide de la cible de rendu 2D [ **ID2D1DeviceContext::SetTarget**](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nf-d2d1_1-id2d1devicecontext-settarget).
- La Bitmap est effacée avec chaque pixel apportée noir à l’aide de [ **ID2D1RenderTarget::Clear**](https://docs.microsoft.com/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-clear).
- [**ID2D1RenderTarget::beginDraw** ](https://docs.microsoft.com/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-begindraw) est appelée pour initialiser le dessin. 
- **DrawText** est appelé pour dessiner le texte stocké dans `m_titleString`, `m_bodyString`, et `m_actionString` dans le rectangle approperiate à l’aide de le correspondantes **ID2D1SolidColorBrush**.
- [**ID2D1RenderTarget::EndDraw** ](ID2D1RenderTarget::EndDraw) est appelée pour arrêter toutes les opérations de dessins sur `m_levelBitmap`.
- Une autre image Bitmap est créée à l’aide de **CreateBitmap** nommé `m_tooSmallBitmap` à utiliser comme solution de secours, en affichant uniquement si la configuration de l’affichage est trop petite pour le jeu.
- Répétez le processus de dessin sur `m_levelBitmap` pour `m_tooSmallBitmap`, cette fois uniquement la chaîne de dessin `Paused` dans le corps.




Maintenant, il que nous suffit sont six méthodes pour remplir le texte de notre États six superposition !

### <a name="representing-game-state"></a>Représentant l’état du jeu


Chacun des États de six superposition dans le jeu a une méthode correspondante la **GameInfoOverlay** objet. Ces méthodes tracent une variation de la superposition pour communiquer des informations explicites au joueur sur le jeu proprement dit. Cette communication est représentée par un **titre** et **corps** chaîne. Étant donné que l’exemple déjà configuré les ressources et la disposition pour ces informations lorsqu’il a été initialisé et avec le [ **GameInfoOverlay::CreateDeviceDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) (méthode), il doit uniquement fournir les chaînes spécifiques à l’état de la superposition.

Le **état** partie de la superposition est définie avec un appel à une des méthodes suivantes.

État du jeu | État défini (méthode) | Champs d’état
:----- | :------- | :---------
Chargement | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**Titre**</br>Chargement des ressources </br>**Body**</br> Imprime de façon incrémentielle ». « impliquer l’activité de chargement.
Statistiques de score de début/haute initiale | [GameInfoOverlay::SetGameStats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**Titre**</br>Score élevé</br> **Body**</br> Niveaux terminé # </br>Total de Points #</br>Total de captures de #
Début de niveau | [GameInfoOverlay::SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**Titre**</br>Niveau #</br>**Body**</br>Description du niveau objective.
Jeu suspendu | [GameInfoOverlay::SetPause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**Titre**</br>Jeu suspendu</br>**Body**</br>Aucune
Fin du jeu | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Titre**</br>Fin du jeu</br> **Body**</br> Niveaux terminé # </br>Total de Points #</br>Total de captures de #</br>Niveaux terminé #</br>Haute Score #
Jeu a gagné | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Titre**</br>Vous avez GAGNÉ !</br> **Body**</br> Niveaux terminé # </br>Total de Points #</br>Total de captures de #</br>Niveaux terminé #</br>Haute Score #




Avec le [ **GameInfoOverlay::CreateWindowSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134) (méthode), l’exemple déclaré trois zones rectangulaires qui correspondent à des régions spécifiques de la superposition.



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

En utilisant le contexte de périphérique Direct2D qui le **GameInfoOverlay** objet initialisé, cette méthode remplit les rectangles de titre et le corps de noir à l’aide du pinceau d’arrière-plan. Elle écrit le texte pour la chaîne « Meilleur score » dans le rectangle de titre, ainsi qu’une chaîne contenant les informations mises à jour sur l’état du jeu dans le rectangle de corps en utilisant le pinceau de texte blanc.


Le rectangle de l’action est mise à jour par un appel ultérieur à [ **GameInfoOverlay::SetAction** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) à partir d’une méthode sur le **GameMain** objet, qui fournit les informations d’état du jeu nécessaires par **GameInfoOverlay::SetAction** pour déterminer le bon message au lecteur, tel que « Appuyez pour continuer ».

Le segment de recouvrement pour n’importe quel état donné est choisi dans la [ **GameMain::SetGameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661) méthode comme suit :

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

Le jeu a maintenant un moyen pour communiquer des informations de texte à l’acteur en fonction de l’état du jeu, et nous avons un moyen de commutation de ce qui est affiché pour les tout au long du jeu.

### <a name="next-steps"></a>Étapes suivantes

Dans la rubrique suivante, [Ajout de contrôles](tutorial--adding-controls.md), nous étudions la façon dont le joueur interagit avec l’exemple de jeu et comment les entrées modifient l’état du jeu.



 




