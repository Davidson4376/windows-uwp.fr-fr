---
title: Audio pour les jeux
description: Apprenez à développer et à incorporer de la musique et des sons dans votre jeu DirectX, et à traiter les signaux audio afin de créer des sons dynamiques et positionnels.
ms.assetid: ab29297a-9588-c79b-24c5-3b94b85e74a8
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, jeux, audio, directx
ms.localizationpriority: medium
ms.openlocfilehash: fa90b22e2661a748454231fea8838bb51b3c621c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367308"
---
# <a name="audio-for-games"></a>Audio pour les jeux



Apprenez à développer et à incorporer de la musique et des sons dans votre jeu DirectX, et à traiter les signaux audio afin de créer des sons dynamiques et positionnels.

Pour la programmation audio, nous vous recommandons d’utiliser la bibliothèque XAudio2 de DirectX, ce que nous faisons dans cette rubrique. XAudio2 est une bibliothèque audio de bas niveau qui procure une base pour le traitement et le mixage du signal pour les jeux, ainsi que la prise en charge de nombreux formats.

Vous pouvez également implémenter la lecture de sons simples et de musique avec [Microsoft Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk). Microsoft Media Foundation est conçu pour la lecture de fichiers et de flux multimédias, autant audio que vidéo, mais il peut aussi être utilisé dans les jeux et est particulièrement utile dans les scènes cinématiques ou les composants non-interactifs de votre jeu.

## <a name="concepts-at-a-glance"></a>Aperçu rapide des concepts


Voici quelques concepts de programmation audio que nous utilisons dans cette section.

-   Le signal est à la programmation audio ce que le pixel est au graphisme : l’unité de base. Les processeurs de signal numérique (DSP, digital signal processor) qui les traitent sont comme les nuanceurs de pixels du son du jeu. Ils peuvent transformer les signaux, les combiner ou les filtrer. En programmant les DSP, vous pouvez modifier les effets audio et la musique de votre jeu avec le niveau de complexité de votre choix.
-   Les voix sont les composites sous-mixés de deux signaux, ou plus. Il existe 3 types d’objets vocaux XAudio2 : voix source, sous-mixée et de matriçage. Les voix source opèrent sur les données audio fournies par le client. Les voix source et sous-mixées envoient leur sortie vers une ou plusieurs voix sous-mixées ou de matriçage. Les voix sous-mixées et de matriçage mixent les signaux provenant de toutes les voix qui les alimentent et opèrent sur le résultat. Les voix de matriçage écrivent des données audio sur un périphérique audio.
-   La mixage est le processus consistant à combiner différentes voix discrètes, telles que les effets sonores et le son d’arrière-plan que l’on entend en fond sonore, en un seul flux. Le sous-mixage est le processus consistant à combiner différents signaux discrets, tels que les sons composant un bruit de moteur, et à créer une voix.
-   Formats audio. La musique et les effets audio peuvent être stockés dans une variété de formats numériques pour votre jeu. Il existe des formats non compressés, comme WAV, et des formats non compressés, comme MP3 et OGG. Plus un échantillon est compressé, moins la fidélité est bonne. En règle générale, plus le taux d’échantillonnage est bas, plus la perte de qualité audio est grande. La fidélité pouvant varier suivant les schémas de compression et les taux d’échantillonnage, il est recommandé d’expérimenter pour trouver ce qui fonctionne mieux pour votre jeu.
-   Taux d’échantillonnage et qualité. Les sons peuvent être échantillonnés à différents taux, et les sons échantillonnés à un faible taux offrent une fidélité moindre. Le taux d’échantillonnage de la qualité CD est de 44,1 Khz (44100 Hz). Si vous n’avez pas besoin d’une haute fidélité pour un son, vous pouvez choisir un taux d’échantillonnage inférieur. Les taux plus élevés conviennent aux applications audio professionnelles, mais vous n’en avez pas besoin tant que votre jeu n’exige pas un son à niveau de fidélité professionnel.
-   Émetteurs sonores (ou sources). Dans XAudio2, les émetteurs sonores sont des sources qui émettent un son, qu’il s’agisse d’un parasite de bruit de fond ou d’un morceau de rock joué par un jukebox dans une scène de votre jeu. Vous spécifiez les émetteurs à l’aide de coordonnées universelles.
-   Écouteurs de son. L’écouteur de son est souvent le joueur, voire une entité IA dans un jeu plus avancé, qui traite les sons reçus en provenance d’un écouteur. Vous pouvez sous-mixer ce son dans le flux audio pour le faire entendre au joueur, ou l’utiliser pour accompagner une action spécifique en cours de jeu, comme réveiller un garde IA marqué comme écouteur.

## <a name="design-considerations"></a>Considérations relatives à la conception


Le son est un élément extrêmement important de la conception et du développement du jeu. De nombreux joueurs sont capables de se souvenir d’un jeu médiocre élevé au rang de légende pour sa bande son, un superbe travail sur les voix ou le mixage du son, ou simplement une production audio exceptionnelle. La musique et le son définissent la personnalité d’un jeu, et établissent le motif principal qui identifie le jeu et le distingue de ses concurrents. L’effort que vous consacrez à concevoir et à développer le profil audio de votre jeu sera bien employé.

Le son 3D positionnel peut ajouter un niveau d’immersion au-delà de ce qu’apportent les graphiques 3D. Si vous développez un jeu complexe qui simule un univers, ou qui exige un style cinématographique, envisagez d’employer des techniques audio positionnelles 3D pour faciliter l’immersion du joueur.

## <a name="directx-audio-development-roadmap"></a>Feuille de route de développement audio DirectX


### <a name="xaudio2-conceptual-resources"></a>Ressources conceptuelles XAudio2

XAudio2 est la bibliothèque de mixage audio pour DirectX, principalement destinée au développement de moteurs audio hautes performances pour les jeux. Pour les développeurs qui souhaitent ajouter des effets sonores et de la musique de fond dans leurs jeux modernes, XAudio2 propose un moteur de mixage et de graphique audio caractérisé par sa faible latence et sa prise en charge des tampons dynamiques, de la lecture synchrone et précise en échantillonnage, et de la conversion implicite du débit source.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Rubrique</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-introduction">Introduction à XAudio2</a></p></td>
<td align="left"><p>Cette rubrique fournit la liste des fonctions de programmation audio prises en charge par XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/getting-started">Prise en main XAudio2</a></p></td>
<td align="left"><p>Cette rubrique fournit des informations sur les concepts clés de XAudio2, les versions XAudio2 et le format audio RIFF.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/common-audio-concepts">Concepts de programmation courants Audio</a></p></td>
<td align="left"><p>Cette rubrique fournit une vue d’ensemble des concepts audio courants qu’un développeur audio doit connaître.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-voices">XAudio2 voix</a></p></td>
<td align="left"><p>Cette rubrique donne une vue d’ensemble des voix XAudio2 qui sont utilisées pour le prémixage, le traitement et le matriçage des données audio.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-callbacks">Rappels de XAudio2</a></p></td>
<td align="left"><p>Cette rubrique couvre les rappels XAudio 2 qui sont utilisés pour empêcher les ruptures dans la lecture audio.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/audio-graphs">Graphiques Audio XAudio2</a></p></td>
<td align="left"><p>Cette rubrique couvre les graphiques de traitement audio XAudio2 qui prennent en entrée une série de flux de données audio provenant du client, les traitent et délivrent le résultat final sur un périphérique audio.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-audio-effects">Effets Audio XAudio2</a></p></td>
<td align="left"><p>Cette rubrique traite des effets audio XAudio2, qui prennent des données audio entrantes et effectuent une opération sur celles-ci (par exemple un effet de réverbération) avant de les transmettre.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-streaming-audio-data">Diffusion en continu des données Audio avec XAudio2</a></p></td>
<td align="left"><p>Cette rubrique couvre la diffusion audio en continu avec XAudio2.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/x3daudio">X3DAudio</a></p></td>
<td align="left"><p>Cette rubrique traite de X3DAudio, une interface API utilisée en conjonction avec XAudio2 pour créer l’illusion d’un son provenant d’un point donné dans l’espace 3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/programming-reference">Référence de programmation XAudio2</a></p></td>
<td align="left"><p>Cette section contient la référence complète des API XAudio2.</p></td>
</tr>
</tbody>
</table>

 

### <a name="xaudio2-how-to-resources"></a>Ressources XAudio2 « Procédure : »

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Rubrique</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--initialize-xaudio2">Procédure : Initialiser XAudio2</a></p></td>
<td align="left"><p>Découvrez comment initialiser XAudio2 pour la lecture audio en créant une instance du moteur XAudio2 et en créant une voix mastérisée.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--load-audio-data-files-in-xaudio2">Procédure : Charger des fichiers de données Audio dans XAudio2</a></p></td>
<td align="left"><p>Apprenez comment remplir les structures nécessaires pour lire les données audio dans XAudio2.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--play-a-sound-with-xaudio2">Procédure : Émettre un signal sonore avec XAudio2</a></p></td>
<td align="left"><p>Apprenez à lire des données audio précédemment chargées dans XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--use-submix-voices">Procédure : Utilisez la voix de mixage secondaire</a></p></td>
<td align="left"><p>Apprenez à définir des groupes de voix pour créer en sortie une seule voix prémixée.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--use-source-voice-callbacks">Procédure : Utiliser des rappels de voix Source</a></p></td>
<td align="left"><p>Apprenez à utiliser les rappels de voix source XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--use-engine-callbacks">Procédure : Utiliser des rappels de moteur</a></p></td>
<td align="left"><p>Apprenez à utiliser les rappels de moteur XAudio2.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--build-a-basic-audio-processing-graph">Procédure : Générer un graphique de base de traitement Audio</a></p></td>
<td align="left"><p>Apprenez à créer un graphique de traitement audio, construit à partir d’une seule voix mastérisée et d’une seule voix source.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--dynamically-add-or-remove-voices-from-an-audio-graph">Procédure : Dynamiquement ajouter ou supprimer des contacts à partir d’un graphique d’Audio</a></p></td>
<td align="left"><p>Découvrez comment ajouter ou supprimer les voix de mixage secondaire dans un graphique qui a été créé suivant les étapes de <a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--build-a-basic-audio-processing-graph">Comment : Générer un graphique de base de traitement Audio</a>.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--create-an-effect-chain">Procédure : Créer une chaîne d’effet</a></p></td>
<td align="left"><p>Apprenez à appliquer une chaîne d’effets à une voix pour le traitement personnalisé des données audio de cette voix.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--create-an-xapo">Procédure : Créer un XAPO</a></p></td>
<td align="left"><p>Découvrez comment implémenter <a href="https://docs.microsoft.com/windows/desktop/api/xapo/nn-xapo-ixapo"><strong>IXAPO</strong></a> pour créer un objet de traitement audio XAudio2 (XAPO).</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--add-run-time-parameter-support-to-an-xapo">Procédure : Ajouter la prise en charge du paramètre d’exécution à un XAPO</a></p></td>
<td align="left"><p>Apprenez à ajouter la prise en charge de paramètre d’exécution à un XAPO en implémentant l’interface <a href="https://docs.microsoft.com/windows/desktop/api/xapo/nn-xapo-ixapoparameters"><strong>IXAPOParameters</strong></a>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--use-an-xapo-in-xaudio2">Procédure : Utiliser un XAPO dans XAudio2</a></p></td>
<td align="left"><p>Apprenez à utiliser l’effet implémenté en tant que XAPO dans une chaîne d’effets XAudio2.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--use-xapofx-in-xaudio2">Procédure : Utiliser XAPOFX dans XAudio2</a></p></td>
<td align="left"><p>Apprenez à utiliser les effets inclus dans XAPOFX dans une chaîne d’effets XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--stream-a-sound-from-disk">Procédure : Stream un son à partir du disque</a></p></td>
<td align="left"><p>Apprenez à diffuser des données audio dans XAudio2 en créant un thread séparé pour lire un tampon audio et en utilisant des rappels pour contrôler ce thread.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--integrate-x3daudio-with-xaudio2">Procédure : Intégrer X3DAudio à XAudio2</a></p></td>
<td align="left"><p>Apprenez à utiliser X3DAudio pour fournir les valeurs de volume et de tonalité aux voix XAudio2 ainsi que les paramètres de l’effet de réverbération intégré XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--group-audio-methods-as-an-operation-set">Procédure : Groupe de méthodes Audio comme un jeu d’opération</a></p></td>
<td align="left"><p>Apprenez à utiliser les ensembles d’opérations XAudio2 pour appliquer un groupe d’appels de méthode en même temps.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/debugging-audio-glitches-in-xaudio2">Débogage des problèmes Audio dans XAudio2</a></p></td>
<td align="left"><p>Apprenez à définir le niveau de journalisation de débogage pour XAudio2.</p></td>
</tr>
</tbody>
</table>

 

### <a name="media-foundation-resources"></a>Ressources Media Foundation

Media Foundation (MF) est une plate-forme de médias pour la diffusion audio et la lecture vidéo. Media Foundation (MF) est une plate-forme de médias pour la diffusion audio et la lecture vidéo. Vous pouvez utiliser les API de Media Foundation pour diffuser en continu des flux audio et vidéo encodés et compressés avec une variété d’algorithmes.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Rubrique</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/about-the-media-foundation-sdk">À propos de Media Foundation</a></p></td>
<td align="left"><p>Cette section contient des informations générales sur les API Media Foundation et les outils disponibles pour leur prise en charge.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/media-foundation-programming--essential-concepts">Media Foundation : Concepts essentiels</a></p></td>
<td align="left"><p>Cette rubrique présente certains concepts que vous devez comprendre avant d’écrire une application Media Foundation.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/media-foundation-architecture">Architecture de Media Foundation</a></p></td>
<td align="left"><p>Cette section décrit la conception générale de Microsoft Media Foundation, ainsi que les primitives et le pipeline de traitement multimédia qu’il utilise.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/audio-video-capture">Capture audio/vidéo</a></p></td>
<td align="left"><p>Cette rubrique explique comment utiliser Microsoft Media Foundation pour effectuer la capture audio et vidéo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/audio-video-playback">Lecture audio/vidéo</a></p></td>
<td align="left"><p>Cette rubrique décrit comment implémenter la lecture audio/vidéo dans votre application.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/supported-media-formats-in-media-foundation">Prise en charge des Formats multimédias dans Media Foundation</a></p></td>
<td align="left"><p>Cette rubrique répertorie les formats multimédias que Microsoft Media Foundation prend en charge en mode natif. (Des tiers peuvent prendre en charge des formats supplémentaires en écrivant des plug-in personnalisés.)</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/encoding-and-file-authoring">Encodage et création de fichiers</a></p></td>
<td align="left"><p>Cette rubrique explique comment utiliser Microsoft Media Foundation pour effectuer le codage audio et vidéo et créer des fichiers multimédias.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/windows-media-codecs">Codecs Windows Media</a></p></td>
<td align="left"><p>Cette rubrique décrit comment utiliser les fonctionnalités des codecs audio et vidéo Windows Media pour produire et consommer des flux de données compressées.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/media-foundation-programming-reference">Référence de programmation de Media Foundation</a></p></td>
<td align="left"><p>Cette section contient des informations de référence sur les API Media Foundation.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/media-foundation-sdk-samples">Exemples de kit de développement logiciel Media Foundation</a></p></td>
<td align="left"><p>Cette section répertorie les exemples d’applications qui montrent comment utiliser Media Foundation.</p></td>
</tr>
</tbody>
</table>

 

### <a name="windows-runtime-xaml-media-types"></a>Types multimédias XAML de Windows Runtime

Si vous utilisez la technologie [interop XAML-DirectX](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10)), vous pouvez incorporer les API multimédias XAML Windows Runtime dans vos applications UWP en C++ avec DirectX pour des scénarios de jeu plus simples.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Rubrique</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement"><strong>Windows.UI.Xaml.Controls.MediaElement</strong></a></p></td>
<td align="left"><p>Élément XAML qui représente un objet contenant des données audio, vidéo ou les deux.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/uwp/audio-video-camera/index">Audio, vidéo et appareil photo</a></p></td>
<td align="left"><p>Découvrez comment incorporer du contenu audio et vidéo de base dans votre application pour la plateforme Windows universelle (UWP).</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/media-playback">MediaElement</a></p></td>
<td align="left"><p>Découvrez comment lire un fichier multimédia stocké localement dans votre application UWP.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/media-playback">MediaElement</a></p></td>
<td align="left"><p>Découvrez comment diffuser un fichier multimédia avec une faible latence dans votre application pour UWP.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/uwp/audio-video-camera/media-casting">Conversion de média</a></p></td>
<td align="left"><p>Découvrez comment utiliser le Contrat Lire sur pour diffuser du contenu multimédia sur un autre appareil à partir de votre application pour UWP.</p></td>
</tr>
</tbody>
</table>

 

## <a name="reference"></a>Référence


-   [XAudio2 Introduction](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-introduction)
-   [Guide de programmation XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/programming-guide)
-   [Vue d’ensemble de Microsoft Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk)

 

## <a name="related-topics"></a>Rubriques connexes


-   [Guide de programmation XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/programming-guide)

 

 




