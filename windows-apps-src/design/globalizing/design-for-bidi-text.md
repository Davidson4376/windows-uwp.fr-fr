---
Description: Concevez votre application pour fournir la prise en charge du texte bidirectionnel (BiDi) afin que vous pouvez combiner le script à partir de gauche à droite (LTR) et de droite à gauche (DÀG) systèmes, qui contiennent généralement les différents types d’alphabets d’écriture.
title: Conception de votre application pour le texte bidirectionnel
template: detail.hbs
ms.date: 11/10/2017
ms.topic: article
keywords: Windows 10, uwp, globalisation, adaptabilité, localisation, DàG, GàD
ms.localizationpriority: medium
ms.openlocfilehash: 66a158a96fcab5391030f4517b6420ba4585bf04
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641124"
---
# <a name="design-your-app-for-bidirectional-text"></a>Conception de votre application pour le texte bidirectionnel

Concevez votre application pour fournir la prise en charge du texte bidirectionnel (BiDi) afin de pouvoir combiner les scripts de systèmes d'écriture de droite à gauche (DàG) et gauche à droite, qui contiennent généralement différents types d’alphabets.

Les systèmes d’écriture de droite à gauche, tels que ceux utilisés au Moyen-Orient, en Asie Centrale et du Sud et en Afrique, comportent des exigences de conception uniques. Ces systèmes d’écriture nécessitent la prise en charge du texte bidirectionnel (Bidi). La prise en charge du BiDi implique la capacité de saisir et d'afficher une disposition de texte de droite à gauche (DàG) ou de gauche à droite (GàD).

Un nombre total de neuf langues BiDi est inclus avec Windows.
- Deux langues intégralement localisées. L'arabe et l'hébreu.
- Sept packs linguistiques LIP pour les marchés émergeants. Le perse, l'ourdou, le dari, le kurde, le sindhi, le pendjabi et l'ouïghour.

Cette rubrique présente la philosophie de conception BiDi de Windows, quelques cas d'études et les considérations de conception BiDi.

## <a name="bidi-design-elements"></a>Éléments de conception BiDi

Dans Windows, quatre éléments influent sur les décisions de conception BiDi.

- **La mise en miroir de l'interface utilisateur (UI)**. Le flux de l'interface utilisateur (UI) permettent de présenter le contenu droite à gauche dans sa disposition native. La conception de l'interface utilisateur semble locale pour les marchés BiDi.
- **La cohérence de l’expérience utilisateur**. La conception semble naturelle dans la direction de droite à gauche. Les éléments de l'interface utilisateur partagent un sens de disposition cohérent et apparaissent de manière prévisible.
- **Optimisation de l'interaction tactile**. Similaires à l'interface utilisateur à interaction tactile dans l'interface utilisateur non mise en miroir, les éléments sont accessibles et naturels dans l'interaction tactile.
- **Prise en charge du texte de mixte**. La prise en charge du sens du texte permet une présentation textuelle très variée (texte en anglais sur des constructions BiDi, et vice versa).

## <a name="feature-design-overview"></a>Présentation de la conception intégrée

Windows prend en charge les quatre éléments de conception BiDi. Examinons quelques-unes des fonctionnalités majeures de Windows. Nous étudierons également le contexte dans lequel ils affectent votre application.

### <a name="navigate-in-the-direction-that-feels-natural"></a>La navigation du sens semble naturelle

Windows ajuste le sens de la grille typographique afin qu'elle soit disposée de droit à gauche. Ainsi, la première vignette de la grille est placée dans le coin supérieur droit et la dernière dans le coin inférieur gauche. Cela correspond au modèle de droite à gauche de publications imprimées telles que la documentation et magazines, où le modèle de lecture toujours démarre dans le coin supérieur droit et progresse vers la gauche.

![Menu Démarrer de BiDi](images/56283_BIDI_01_startscreen_resized.png)
![BiDi le menu Démarrer avec les icônes](images/56283_BIDI_02_startscreen_charm_resized.png)

Pour conserver un flux d'interface utilisateur cohérent, le contenu des vignettes garde une disposition de droite à gauche, ce qui signifie que le nom et le logo de l'application sont placés dans le coin inférieur droit de la vignette, quelle que soit la langue de l'interface utilisateur de l'application.

#### <a name="bidi-tile"></a>Vignette BiDi

![Vignette BiDi](images/56284_BIDI_03_tile_callouts_withKey.png)

#### <a name="english-tile"></a>Vignette en anglais

![Vignette en anglais](images/56284_BIDI_03_tile_callouts_en-us.png)

### <a name="get-tile-notifications-that-read-correctly"></a>Obtenir des notifications par vignette à lecture aisée

Les vignettes prennent en charge une combinaison de texte. La zone de notification présente une flexibilité intégrée dans l'ajustement de l'alignement du texte en fonction de la langue de notification.  Lorsqu'une application envoi des notifications BiDi en arabe, en hébreu ou autre, le texte est aligné sur la droite. Lorsqu'une notification en anglais se présente, elle est alignée sur la gauche.

![Notifications par vignette](images/56285_BIDI_04_bidirectional_tiles_white.png)

### <a name="a-consistent-easy-to-touch-rtl-user-experience"></a>Une expérience utilisateur DàG cohérence à l'interaction tactile simple

Chaque élément de l'interface utilisateur Windows s'adapte à l'orientation DàG. Les icônes et menus volants sont placés sur le bord gauche de l'écran afin d'éviter qu'ils ne chevauchent les résultats de recherche ou qu'ils ne dégradent l'optimisation tactile. Ils sont simplement accessibles avec le pouce.

![Capture d’écran BiDi](images/56286_BIDI_05_search_flyout_resized.png)
![BiDi capture d’écran](images/56286_BIDI_06_print_flyout_resized.png)

![Capture d’écran BiDi](images/56286_BIDI_07_settings_flyout_resized.png)
![BiDi capture d’écran](images/56286_BIDI_08_app_bars_resized.png)

### <a name="text-input-in-any-direction"></a>Saisie de texte dans n’importe quelle direction

Windows propose un clavier visuel tactile net et clair. Pour les langues BiDi, la présence d'une touche de contrôle de sens du texte permet le basculement du sens de la saisie de texte selon les besoins.

![Clavier tactile pour les langues BiDi](images/56287_BIDI_09_keyboard_layout_resized.png)

### <a name="use-any-app-in-any-language"></a>Utiliser toutes applications dans toutes langues

Installez et utilisez vos applications favorites dans toutes langues. Les applications apparaissent et fonctionnement de la même manière qu'avec les versions non BiDi de Windows. Les éléments contenus dans ces applications sont toujours disposés de manière cohérente et prévisible.

![Application en anglais affichant du contenu BiDi](images/56288_BIDI_10_english_app_resized.png)

### <a name="display-parentheses-correctly"></a>Affichage correct des parenthèses

Avec l'introduction de l'algorithme de parenthèse BiDi (BPA), les paires de parenthèses sont toujours affichées correctement, quelles que soient les langues ou les propriétés d'alignement de texte.

#### <a name="incorrect-parentheses"></a>Parenthèses incorrectes

![Application BiDi avec des parenthèses incorrectes](images/56289_BIDI_11_parentheses_resized.png)

#### <a name="correct-parentheses"></a>Parenthèses correctes

![Application BiDi avec des parenthèses correctes](images/56289_BIDI_12_parentheses_fixed_resized.png)

### <a name="typography"></a>Typographie

Windows utilise la police d'interface utilisateur Segoe dans toutes les langues BiDi. Cette police est mise en force et mise à l'échelle pour l'interface utilisateur Windows.

![Police Segoe UI pour les langues BiDi](images/56290_BIDI_13_start_screen_segoe.png)
![police Segoe UI pour les langues BiDi](images/56290_BIDI_13_start_screen_segoe_arabic.png)

## <a name="case-study-1-a-bidi-music-app"></a>Étude de cas #1 : Une application de musique BiDi

### <a name="overview"></a>Vue d’ensemble

Les applications multimédia constituent un défi de conception très intéressant. En effet, on s'attend généralement à ce que les commandes média ait une disposition de gauche à droite, de manière similaire aux langues non BiDi.

![Commandes média de gauche à droite](images/56291_BIDI_1415_music_player_layouts_left-withcallouts.png)

![Commandes média de droite à gauche](images/56291_BIDI_1415_music_player_layouts_right-withcallouts.png)

### <a name="establishing-ui-directionality"></a>Établissement de l’orientation de l’interface utilisateur

La conservation du flux de l'interface utilisateur de droite à gauche est importante pour une conception cohérente dans les marchés BiDi. Dans ce contexte, il est difficile d'ajouter des éléments d'un flux de gauche à droite. En effet, certains éléments de navigations tels que le bouton Précédent peuvent contredire l'orientation directionnelle du bouton Précédents des commandes audio.

![Page de piste d'une application de musique](images/56292_BIDI_16_app_layout_callouts_resized.png)

Cette application de musique conserve une grille à orientation de droite à gauche. Ainsi, l'application offre une expérience très naturelle aux utilisateurs utilisant déjà cette orientation dans la navigation au sein de l'interface utilisateur Windows. Le flux est conservé en garantissant que les éléments principaux ne sont pas simplement organisés de droite à gauche, mais également alignés de manière adéquate dans les titres de section afin de préserver le flux de l'interface utilisateur.

![Page d'album d'une application de musique](images/56292_BIDI_17_app_layout_callouts_resized.png)

### <a name="text-handling"></a>Gestion du texte

La biographie de l'artiste de la capture d'écran ci-dessous est alignée à gauche, alors que les autres zones de texte relatives à l'artiste, notamment le nom de l'album et des pistes, conservent un alignement à droite. Le champ de biographie est un élément de texte assez encombrant. Sa lecture est difficile lorsqu'il est aligné à droite. En effet, il n'est pas simple de suivre le cours des lignes tout en lisant un bloc de texte élargi. En général, tout élément de texte contenant plus de trois lignes et plus de cinq mot doit être associé à des exceptions d'alignement similaires dans lesquelles l'alignement du bloc de texte est opposé à celui de la disposition générale du sens de l'application.

La manipulation de l'alignement au sein de l'application peut paraître simple, mais elle présente souvent certaines limites dans le rendu des moteurs en termes de placement des caractères neutres dans les chaînes BiDi. Par exemple, la chaîne suivante peut s'afficher différemment selon l'alignement.

| | Chaîne en anglais (GàD) | Chaîne en hébreu (DàG) |
| -------------- | ------------------- | ------------------- |
| **Alignement à gauche** | Hello, World! | בוקר טוב! |
| **Alignement de la droite** | !Hello, World | !בוקר טוב |

Pour assurer que les informations de l'artiste s'affichent correctement dans l'application de musique, l'équipe de développement a séparé l'alignement des propriétés de disposition du texte. Autrement dit, les informations de l'artiste peuvent être alignés à droite dans la plupart des cas, mais l'alignement de la disposition de la chaîne est définie selon un traitement d'arrière-plan personnalisé. Le traitement d'arrière-plan détermine le meilleur paramètre de disposition directionnelle selon le contenu de la chaîne.

![Page d'artiste d'une application de musique](images/56292_BIDI_18_app_layout_callouts_resized.png)

Par exemple, sans traitement de disposition de chaîne personnalisé, le nom d'artiste « The Contoso Band. » deviendrait « .The Contoso Band ».

### <a name="specialized-string-direction-preprocessing"></a>Prétraitement de sens de chaîne spécialisé

Lorsque l'application contacte le serveur pour les métadonnées média, elle prétraite chaque chaîne avant de les afficher à l'utilisateur. Au cours de ce prétraitement, l'application procède également à une transformation afin que le sens du texte soit cohérent. Pour ce faire, elle vérifie la présence de caractères neutres aux extrémités de la chaîne. En outre, si l'orientation du texte de la chaîne est opposée à l'orientation de l'application définie par les paramètres linguistiques Windows, alors elle ajoute (et parfois en préfixe) les marqueurs d'orientation Unicode. La fonction de transformation ressemble à ceci.

```csharp
string NormalizeTextDirection(string data) 
{
    if (data.Length > 0) {
        var lastCharacterDirection = DetectCharacterDirection(data[data.Length - 1]);

        // If the last character has strong directionality (direction is not null), then the text direction for the string is already consistent.
        if (!lastCharacterDirection) {
            // If the last character has no directionality (neutral character, direction is null), then we may need to add a direction marker to
            // ensure that the last character doesn't inherit directionality from the outside context.
            var appTextDirection = GetAppTextDirection(); // checks the <html> element's "dir" attribute.
            var dataTextDirection = DetectStringDirection(data); // Run through the string until a non-neutral character is encountered,
                                                                 // which determines the text direction.

            if (appTextDirection != dataTextDirection) {
                // Add a direction marker only if the data text runs opposite to the directionality of the app as a whole,
                // which would cause the neutral characters at the ends to flip.
                var directionMarkerCharacter =
                    dataTextDirection == TextDirections.RightToLeft ?
                        UnicodeDirectionMarkers.RightToLeftDirectionMarker : // "\u200F"
                        UnicodeDirectionMarkers.LeftToRightDirectionMarker; // "\u200E"

                data += directionMarkerCharacter;

                // Prepend the direction marker if the data text begins with a neutral character.
                var firstCharacterDirection = DetectCharacterDirection(data[0]);
                if (!firstCharacterDirection) {
                    data = directionMarkerCharacter + data;
                }
            }
        }
    }

    return data;
}
```

Les caractères Unicode ajoutés n'ont aucune largeur. Ainsi, ils n'ont aucun impact sur l'espacement des chaînes. Ce code porte une potentielle pénalité de performance dans la mesure où l'orientation d'une chaîne nécessite d'être exécutée via la chaîne, jusqu'à ce qu'un caractère non neutre soit rencontré. Chaque caractère dont la neutralité est vérifiée est d'abord comparé à plusieurs gammes Unicode de la même manière. Cette vérification n'est donc pas sans importance.

## <a name="case-study-2-a-bidi-mail-app"></a>Étude de cas #2 : Une application de messagerie BiDi

### <a name="overview"></a>Vue d’ensemble

En ce qui concerne les exigences de disposition de l'interface utilisateur, un client courrier est très simple à concevoir. Par défaut, l'application Courrier dans Windows est mise en miroir. Du point de vue du traitement du texte, l'application courrier doit disposer d'un affichage de texte et de capacités de composition plus robustes pour s'adapter aux scénarios de texte combiné.

### <a name="establishing-ui-directionality"></a>Établissement de l’orientation de l’interface utilisateur

La disposition de l'interface utilisateur de l'application courrier est mise en miroir. Les trois volets ont été réorientés afin que le volet du dossier soit positionné sur le bord droit de l'écran, suivi par le volet de liste d'article de courrier à gauche, puis du volet de composition d'e-mail.

![Application de courrier mise en mémoire](images/56293_BIDI_19_icon_realignment_cropped_resized.png)

Les articles supplémentaires ont été réorientés pour correspondre au flux d'interface utilisateur général et à l'optimisation tactile. Ces articles concernent la barre de l'application et les icônes Composer, Répondre et Supprimer.

![Application de courrier mise en mémoire avec la barre de l'application](images/56294_BIDI_20_email_orientation_email_resized.png)

### <a name="text-handling"></a>Gestion du texte

#### <a name="ui"></a>Interface utilisateur

Le texte au sein de l'interface utilisateur est habituellement aligné à droite. Cela inclut le volet de dossier et le volet des articles. Le volet des articles se limite à deux lignes de texte (Adresse et Titre). Cet élément est important pour conserver l'alignement de droite à gauche sans introduire de bloc de texte qui serait difficile à lire lorsque l'orientation du contenu et opposée au flux d'orientation de l'interface utilisateur.

#### <a name="text-editing"></a>Modification de texte

La modification du texte nécessite la capacité de composer à la fois de droite à gauche et de gauche à droite. En outre, la disposition de composition doit être préservée par l'utilisation d'un format&mdash;tel qu'un texte riche&mdash;et ayant la capacité d'enregistrer les informations d'orientation.

![Application de courrier de gauche à droite](images/56294_BIDI_21_email_orientation_LtR_resized.png)

![Application de courrier de droite à gauche](images/56294_BIDI_22_email_orientation_RtL_resized.png)
