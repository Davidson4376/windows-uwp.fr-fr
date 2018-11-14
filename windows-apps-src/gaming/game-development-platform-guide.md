---
author: mtoepke
title: Technologies de jeu des applications pour la plateforme Windows universelle (UWP)
description: Ce guide décrit les technologies disponibles pour le développement de jeux UWP.
ms.assetid: bc4d4648-0d6e-efbb-7608-80bd09decd6e
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, jeux, technologie, directx
ms.localizationpriority: medium
ms.openlocfilehash: c6896bda0498483efb4d77e1fa2a6ef82e0f8789
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6181565"
---
# <a name="game-technologies-for-uwp-apps"></a>Technologies de jeu des applications UWP



Ce guide décrit les technologies disponibles pour le développement de jeux UWP (Universal Windows Platform).

##  <a name="benefits-of-windows10-for-game-development"></a>Avantages de Windows 10 pour le développement de jeux


Avec l’introduction de UWP dans Windows 10, vos titres Windows 10 sera en mesure de s’étendre sur toutes les plateformes Microsoft. Avec la migration gratuite à partir de versions antérieures de Windows, il existe un nombre croissant de clients de Windows 10. La combinaison de ces deux aspects signifie que vos titres Windows 10 pourront atteindre un nombre considérable de clients par le biais du Microsoft Store.

En outre, Windows 10 offre de nombreuses nouvelles fonctionnalités qui sont particulièrement intéressant de jeux:

-   Réduction de la pagination de la mémoire et de la taille globale du système de mémoire
-   Amélioration de la gestion de la mémoire graphique qui alloue et protège activement davantage de mémoire pour le jeu au premier plan

## <a name="uwp-games-with-c-and-directx"></a>Jeux UWP avec C++ et DirectX


Les jeux en temps réel exigeant des performances élevées doivent utiliser des API DirectX. DirectX est un ensemble d’API natives pour la création de jeux et d’applications multimédias qui exigent des performances élevées, tels les jeux 3D.

## <a name="development-environment"></a>Environnement de développement


Pour créer des jeux pour UWP, vous devez configurer votre environnement de développement en installant Visual Studio 2015 ou version ultérieure. Nous vous recommandons d’installer la dernière version de Visual Studio, vous donnant accès aux dernières mises à jour de sécurité et de développement. Visual Studio vous permet de créer des applications UWP et fournit des outils de développement de jeux:

-   Outils Visual Studio pour la programmation de jeux DX - Visual Studio offre des outils permettant de créer, modifier, prévisualiser et exporter des ressources d’image, de modèle et de nuanceur. Il existe également des outils que vous pouvez utiliser pour convertir des ressources au moment de la création et pour déboguer le code graphique DirectX. Pour plus d’informations, voir [Utiliser Visual Studio Tools pour la programmation de jeux](set-up-visual-studio-for-game-development.md).
-   Fonctionnalités de Graphics Diagnostics dans Visual Studio - Les outils de diagnostic de graphiques sont désormais disponibles dans Windows en tant que fonctionnalités facultatives. Les outils de diagnostic vous permettent d’effectuer des tâches de débogage graphique et d’analyse des frames graphiques, ainsi que de surveiller l’utilisation du processeur graphique (GPU) en temps réel. Pour plus d’informations, voir [Utiliser les fonctionnalités de diagnostic de graphiques de Visual Studio et du runtime DirectX](use-the-directx-runtime-and-visual-studio-graphics-diagnostic-features.md).

Pour plus d’informations, voir «Préparer votre environnement de [programmation de jeux DirectX](directx-programming.md) et Plateforme Windows universelle».

## <a name="getting-started-with-directx-game-project-templates"></a>Prise en main des modèles de projet de jeu DirectX


Après avoir configuré votre environnement de développement, vous pouvez utiliser l’un des modèles de projet DirectX associés pour créer votre jeu DirectX UWP. Visual Studio2015 comporte troismodèles disponibles pour la création de projets DirectX UWP: **Application DirectX11 (Windows universel)**, **Application DirectX12 (Windows universel)** et **Application DirectX11 et XAML (Windows universel)**. Pour plus d’informations, voir [Créer un projet de jeux DirectX et Plateforme Windows universelle à partir d’un modèle](user-interface.md).

## <a name="windows-10-apis"></a>API Windows 10


Windows 10 offre une collection complète d’API utiles pour le développement de jeux. Il existe des API pour quasiment tous les aspects des jeux : graphiques 3D, graphiques 2D, audio, entrée, ressources texte, interface utilisateur et réseau.

De nombreuses API sont liées au développement de jeux, mais tous les jeux ne doivent utiliser toutes les API. Par exemple, certains jeux utilisant uniquement des éléments graphiques 3D n’ont besoin que de Direct3D, d’autres utilisant uniquement des éléments graphiques 2D n’ont besoin que de Direct2D, et d’autres encore utilisent les deux. Le diagramme suivant présente les API liées au développement de jeux, regroupées par type de fonctionnalité.

![technologies de plateforme de jeu](images/gameplatformtechnologies.png)

-   Graphiques3D: Windows10 prend en charge deuxjeux d’API graphiques3D, Direct3D11 et [Direct3D12](https://msdn.microsoft.com/library/windows/desktop/dn899121). Ces API permettent de créer des éléments graphiques 2D et 3D. Les jeux d’API graphiques Direct3D 11 et Direct3D 12 ne sont pas utilisés ensemble, mais chacun peut être utilisé avec toute API du groupe d’interfaces utilisateur et d’éléments graphiques 2D. Pour plus d’informations sur l’utilisation des API graphiques dans votre jeu, voir [Graphismes3D de base pour jeux DirectX](an-introduction-to-3d-graphics-with-directx.md).

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Direct3D 12</td>
    <td align="left"><p>Direct3D 12 introduit la nouvelle version de Direct3D, l’API graphique 3D au cœur de DirectX. Cette version de Direct3D est conçue pour être plus rapide et plus efficace que les versions précédentes. La contrepartie de la vitesse accrue de l’API Direct3D12 est qu’étant de niveau inférieur, elle requiert que vous gériez vous-même vos ressources graphiques, et disposiez d’une plus vaste expérience en matière de programmation d’éléments graphiques pour tirer parti de cette vitesse accrue.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Utilisez Direct3D12 lorsque vous devez optimiser les performances de votre jeu et que celui-ci utilise le processeur de manière intensive.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la documentation concernant <a href="https://msdn.microsoft.com/library/windows/desktop/dn899121">Direct3D12</a>.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Direct3D 11</td>
    <td align="left"><p>Direct3D11 est la version précédente de Direct3D. Elle permet de créer des éléments graphiques3D avec un niveau d’abstraction matérielle supérieur à celui de Direct3D12.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Utilisez Direct3D 11 si vous avez du code Direct3D 11 existant, si votre jeu n’utilise pas le processeur de manière intensive, ou si voulez bénéficier de l’avantage de ne pas à avoir à gérer vous-même vos ressources.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la documentation concernant <a href="https://msdn.microsoft.com/library/windows/desktop/ff476080">Direct3D11</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Éléments graphiques et interfaces utilisateur 2D - API concernant les éléments graphiques 2D, tels que des textes et des interfaces utilisateur. Toutes les API pour éléments graphiques et interfaces utilisateur 2D sont facultatives.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Direct2D</td>
    <td align="left"><p>Direct2D est une API graphique 2D à accélération matérielle et en mode immédiat, qui offre des performances élevées et un rendu de grande qualité pour les éléments géométriques, bitmaps et textes 2D. L’API Direct2D repose sur Direct3D. Elle est conçue pour fonctionner correctement avec GDI, GDI+ et Direct3D.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Vous pouvez utiliser Direct2D soit à la place de Direct3D afin de produire des graphismes pour des jeux purement2D, tels des jeux à défilement horizontal ou de plateau, soit avec Direct3D pour simplifier la création d’éléments graphiques2D dans des jeux3D tels que des interfaces utilisateur ou des affichages tête haute.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la documentation concernant <a href="https://msdn.microsoft.com/library/windows/desktop/dd370990">Direct2D</a>.</p></td>
    </tr>
    <tr class="even">
    <td align="left">DirectWrite</td>
    <td align="left"><p>DirectWrite apporte des fonctionnalités supplémentaires de manipulation de texte. Il peut être utilisé avec Direct3D ou Direct2D afin de fournir une sortie de texte pour les interfaces utilisateur ou d’autres zones où du texte est requis. DirectWrite prend en charge la mesure, le dessin et le test de résultats de texte multiformat. DirectWrite gère le texte dans toutes les langues prises en charge pour les applications localisées et globales. DirectWrite fournit également une API de rendu de glyphe de bas niveau pour les développeurs désireux de réaliser leurs propres disposition et traitement d’Unicodeàglyphe.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p></p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la documentation concernant <a href="https://msdn.microsoft.com/library/windows/desktop/dd368038">DirectWrite</a>.</p></td>
    </tr>
    <tr class="odd">
    <td align="left">DirectComposition</td>
    <td align="left"><p>DirectComposition est un composant Windows qui permet de composer des bitmaps complexes avec des transformations, des effets et des animations. Les développeurs d’applications peuvent utiliser l’API DirectComposition pour créer des interfaces utilisateur visuellement attrayantes avec des transitions riches et fluides d’un élément visuel à l’autre.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>DirectComposition est conçue pour simplifier le processus de composition d’éléments visuels et de création de transitions animées. Si votre jeu nécessite des interfaces utilisateur complexes, vous pouvez utiliser DirectComposition pour simplifier la création et la gestion de l’interface utilisateur.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la documentation concernant <a href="https://msdn.microsoft.com/library/windows/desktop/hh437371">DirectComposition</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Audio - API concernant la lecture audio et l’application d’effets audio. Pour plus d’informations sur l’utilisation des API audio dans votre jeu, voir [Audio pour les jeux](working-with-audio-in-your-directx-game.md).

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">XAudio2</td>
    <td align="left"><p>XAudio2 est une API audio de bas niveau qui constitue le fondement du traitement et du mixage du signal. XAudio est conçue pour être très réactive aux moteurs audio de jeu tout en conservant la possibilité de créer des effets audio personnalisés et des chaînes complexes d’effets et de filtres audio.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Utilisez XAudio2 quand votre jeu doit lire des sons en n’occasionnant qu’un minimum de surcharge et de ralentissement.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la documentation concernant <a href="https://msdn.microsoft.com/library/windows/desktop/hh405049">XAudio2</a>.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Media Foundation</td>
    <td align="left"><p>Microsoft Media Foundation est conçue pour la lecture de fichiers multimédias et de flux audio et vidéo, mais peut également servir dans des jeux quand des fonctionnalités de niveau supérieur à XAudio2 sont requises et qu’une certaine surcharge supplémentaire est acceptable.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Media Foundation est particulièrement utile pour les scènes cinématographiques ou les composants non-interactifs de votre jeu. Media Foundation est également utile pour le décodage de fichiers audio avant leur lecture à l’aide de XAudio2.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la <a href="https://msdn.microsoft.com/library/windows/desktop/ms694197">vue d’ensemble de Microsoft Media Foundation</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Entrée - API concernant diverses sources d’entrée d’utilisateurs telles que le clavier, la souris, le boîtier de commande et autres.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">XInput</td>
    <td align="left"><p>L’API de contrôleur de jeu XInput permet aux applications de recevoir des entrées de contrôleurs de jeu.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Si votre jeu doit prendre en charge l’entrée par le biais du boîtier de commande et que vous avez du code XInput existant, vous pouvez continuer à utiliser XInput. XInput ayant été remplacé par Windows.Gaming.Input pour UWP, si vous écrivez un nouveau code d’entrée, vous devez utiliser Windows.Gaming.Input au lieu de XInput.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la documentation concernant <a href="https://msdn.microsoft.com/library/windows/desktop/hh405053">XInput</a>.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Windows.Gaming.Input</td>
    <td align="left"><p>L’API Windows.Gaming.Input remplaçant XInput offre les mêmes fonctionnalités que celle-ci, avec les avantages suivants :</p>
    <ul>
    <li>Moindre utilisation des ressources</li>
    <li>Moindre latence d’appel d’API pour récupérer l’entrée</li>
    <li>Possibilité d’utiliser plus de 4 boîtiers de commande à la fois</li>
    <li>Possibilité d’accéder à des fonctionnalités supplémentaires de boîtier de commande Xbox One, telles que les moteurs de vibration de déclencheur</li>
    <li>Possibilité d’être averti quand des contrôleurs se connectent/déconnectent via un événement au lieu d’une interrogation</li>
    <li>Possibilité d’attribuer une entrée à un utilisateur spécifique (Windows.System.User)</li>
    </ul>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Si votre jeu doit prendre en charge l’entrée de boîtier de commande et n’utilise pas de code XInput existant, ou si vous avez besoin d’un des avantages répertoriés ci-dessus, vous devez utiliser Windows.Gaming.Input.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la documentation concernant <a href="https://msdn.microsoft.com/library/windows/apps/dn707817">Windows.Gaming.Input</a>.</p></td>
    </tr>
    <tr class="odd">
    <td align="left">Windows.UI.Core.CoreWindow</td>
    <td align="left"><p>La classe Windows.UI.Core.CoreWindow fournit des événements pour suivre les actions d’activation et de déplacement du pointeur, ainsi que des touches Haut et Bas.</p>
    <p><strong>Quand utiliser</strong></p>
    <p>Utilisez des événements Windows.UI.Core.CoreWindows quand vous devez suivre les actions de la souris ou des touches dans votre jeu.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Pour plus d’informations sur l’utilisation de la souris ou du clavier dans votre jeu, voir <a href="https://docs.microsoft.com/windows/uwp/gaming/tutorial--adding-move-look-controls-to-your-directx-game">Contrôles de déplacement/vue pour les jeux</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Math - API concernant la simplification d’opérations mathématiques couramment utilisées.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">DirectXMath</td>
    <td align="left"><p>L’API DirectXMath fournit des types et fonctions C++ compatibles SIMD pour des opérations mathématiques courantes d’algèbre linéaire et de graphiques communes aux jeux.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>L’utilisation de DirectXMath est facultative. Elle simplifie les opérations mathématiques courantes.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la documentation concernant <a href="https://msdn.microsoft.com/library/windows/desktop/hh437833">DirectXMath</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Réseau - API concernant la communication avec d’autres ordinateurs et appareils par le biais d’Internet ou de réseaux privés.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Windows.Networking.Sockets</td>
    <td align="left"><p>L’espace de noms Windows.Networking.Sockets fournit des sockets TCP et UDP qui permettent une communication réseau fiable ou non fiable.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Utilisez Windows.Networking.Sockets si votre jeu doit communiquer avec d’autres ordinateurs ou appareils via le réseau.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir <a href="https://docs.microsoft.com/windows/uwp/gaming/work-with-networking-in-your-directx-game">Utiliser le réseau dans votre jeu</a>.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Windows.Web.HTTP</td>
    <td align="left"><p>L’espace de noms Windows.Web.HTTP fournit aux serveurs HTTP une connexion fiable qui peut être utilisée pour accéder à un site web.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Utilisez Windows.Web.HTTP quand votre jeu doit accéder à un site web pour récupérer ou stocker des informations.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir <a href="https://docs.microsoft.com/windows/uwp/gaming/work-with-networking-in-your-directx-game">Utiliser le réseau dans votre jeu</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Utilitaires de support - Bibliothèques qui s’appuient sur les API de Windows 10.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">Bibliothèque</th>
    <th align="left">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Kit de ressources DirectX</td>
    <td align="left"><p>Le Kit de ressources DirectX (DirectXTK) est une collection de classes d’assistance pour l’écriture de code DirectX11.x en C++.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Utilisez le Kit de ressources DirectX si vous êtes un développeur en C++ cherchant une alternative moderne au code hérité des utilitaires D3DX, ou un développeur en XNA Game Studio désireux de passer à C++ natif.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Consultez la page du projet Kit de ressources DirectX, <a href="https://github.com/Microsoft/DirectXTK">https://github.com/Microsoft/DirectXTK</a>.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Win2D</td>
    <td align="left"><p>Win2D est une API Windows Runtime facile à utiliser pour le rendu d’éléments graphiques 2D en mode immédiat.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Utilisez Win2D si vous êtes un développeur en C++ désireux de disposer d’un wrapper WinRT plus facile à utiliser pour Direct2D et DirectWrite, ou si vous êtes un développeur en C# souhaitant utiliser Direct2D et DirectWrite.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Consultez la page du projet Win2D, <a href="https://github.com/Microsoft/Win2D">https://github.com/Microsoft/Win2D</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

## <a name="xbox-live-services"></a>Services Xbox Live

Le [Programme créateurs Xbox Live](https://developer.microsoft.com/games/xbox/xboxlive/creator) permet aux développeurs d’intégrer Xbox Live dans ses jeux UWP et les publier sur Xbox One et Windows 10. Intégrez dans votre titre des fonctionnalités sociales de Xbox Live telles que la connexion, la présence, les classements, etc., avec un temps de développement minime. Les fonctionnalités sociales de Xbox Live visent à faire croître votre public de façon drastique en vous faisant connaître auprès de plus de 55millions de joueurs actifs.

Si vous souhaitez accéder à d’autres fonctionnalités Xbox Live, bénéficier d’une aide en termes de marketing et de développement et avoir la chance de figurer dans le Windows Store Xbox One principal, soumettez une demande de participation au programme [ID@Xbox](http://www.xbox.com/developers/id). Pour voir les fonctionnalités disponibles pour les programmes Créateurs Xbox Live et ID@Xbox, voir le [tableau des fonctionnalités](../xbox-live/developer-program-overview.md#feature-table).

Pour plus d’informations, accédez à [Ajout de Xbox Live à votre jeu](e2e.md#adding-xbox-live-to-your-game).

##  <a name="alternatives-to-writing-games-with-directx-and-uwp"></a>Alternatives à l’écriture de jeux avec DirectX et UWP


### <a name="uwp-games-without-directx"></a>Jeux UWP sans DirectX

Des jeux plus simples, sans exigences de performances minimales, tels des jeux de carte ou de plateau, peuvent être écrits sans DirectX et ne nécessitent pas nécessairement une écriture en C++. Ces types de jeux peuvent utiliser tout langage pris en charge par UWP, tel que C#, Visual Basic, C++ et HTML/JavaScript. Si votre jeu ne requiert pas une performance particulière ni un graphisme intensif, consultez l’[exemple de jeu tactile en JavaScript et HTML5](http://code.msdn.microsoft.com/windowsapps/JavaScript-and-HTML5-touch-d96f6031).

### <a name="game-engines"></a>Moteurs de jeu

Comme alternative à l’écriture de votre propre moteur de jeu à l’aide des API de développement de jeux Windows, de nombreux moteurs de jeu de haute qualité qui s’appuient sur les API de développement de jeux Windows sont disponibles pour le développement de jeux sur les plateformes Windows. Lorsque vous envisagez un moteur de jeu ou une bibliothèque de jeux, plusieurs options s’offrent à vous :

-   Moteur de jeu complet - Un moteur de jeu complet encapsule la plupart ou la totalité des API Windows 10 utiles lors de l’écriture d’un moteur de jeu à partir de rien, par exemple, en rapport avec les graphiques, l’audio, les entrées et l’utilisation du réseau. Les moteurs de jeu complets peuvent également offrir des fonctionnalités logiques de jeu, telles que l’intelligence artificielle et la recherche de chemin d’accès.
-   Moteur graphique - Les moteurs graphiques encapsulent les API graphiques Windows 10, gèrent les ressources graphiques, et prennent en charge une variété de formats de modèle et d’environnement.
-   Moteur audio - Les moteurs audio encapsulent les API audio Windows 10, gèrent les ressources audio et fournissent un traitement et des effets audio avancés.
-   Moteur réseau - Les moteurs réseau encapsulent les API de réseau Windows 10 pour l’ajout à votre jeu de la prise en charge du mode multijoueur de pair à pair ou basé sur un serveur, et peuvent inclure des fonctionnalités de réseau avancées pour prendre en charge un grand nombre de joueurs.
-   Intelligence artificielle et moteur de recherche de chemin d’accès - L’IA et les moteurs de recherche de chemin d’accès fournissent une infrastructure pour contrôler le comportement d’agents dans votre jeu.
-   Moteurs spéciaux - Divers moteurs supplémentaires existent pour la gestion de toute tâche de développement de jeu vous pourriez être amené à exécuter, telle que la création de systèmes de stock et d’arborescences de boîte de dialogue.

## <a name="submitting-a-game-to-the-store"></a>Soumission d’un jeu au Store


Lorsque vous êtes prêt à publier votre jeu, vous devez créer un compte de développeur et soumettre votre jeu au Microsoft Store.

Pour plus d’informations sur la soumission de votre jeu au Microsoft Store, voir [Envoi et publication du jeu](e2e.md#submitting-and-publishing-your-game).

 

 




