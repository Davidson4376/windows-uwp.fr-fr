---
author: mtoepke
title: Audio pour les jeux
description: Apprenez à développer et à incorporer de la musique et des sons dans votre jeu DirectX, et à traiter les signaux audio afin de créer des sons dynamiques et positionnels.
ms.assetid: ab29297a-9588-c79b-24c5-3b94b85e74a8
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, uwp, jeux, audio, directx
ms.localizationpriority: medium
ms.openlocfilehash: a0b0ae219ea7fd014b39eb8eb7a09049f7c632a2
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5709667"
---
# <a name="audio-for-games"></a>Audio pour les jeux



Apprenez à développer et à incorporer de la musique et des sons dans votre jeu DirectX, et à traiter les signaux audio afin de créer des sons dynamiques et positionnels.

Pour la programmation audio, nous vous recommandons d’utiliser la bibliothèque XAudio2 de DirectX, ce que nous faisons dans cette rubrique. XAudio2 est une bibliothèque audio de bas niveau qui procure une base pour le traitement et le mixage du signal pour les jeux, ainsi que la prise en charge de nombreux formats.

Vous pouvez également implémenter la lecture de sons simples et de musique avec [Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197). Microsoft Media Foundation est conçu pour la lecture de fichiers et de flux multimédias, autant audio que vidéo, mais il peut aussi être utilisé dans les jeux et est particulièrement utile dans les scènes cinématiques ou les composants non-interactifs de votre jeu.

## <a name="concepts-at-a-glance"></a>Aperçu rapide des concepts


Voici quelques concepts de programmation audio que nous utilisons dans cette section.

-   Le signal est à la programmation audio ce que le pixel est au graphisme : l’unité de base. Les processeurs de signal numérique (DSP, digital signal processor) qui les traitent sont comme les nuanceurs de pixels du son du jeu. Ils peuvent transformer les signaux, les combiner ou les filtrer. En programmant les DSP, vous pouvez modifier les effets audio et la musique de votre jeu avec le niveau de complexité de votre choix.
-   Les voix sont les composites sous-mixés de deux signaux, ou plus. Il existe 3 types d’objets vocaux XAudio2 : voix source, sous-mixée et de matriçage. Les voix source opèrent sur les données audio fournies par le client. Les voix source et sous-mixées envoient leur sortie vers une ou plusieurs voix sous-mixées ou de matriçage. Les voix sous-mixées et de matriçage mixent les signaux provenant de toutes les voix qui les alimentent et opèrent sur le résultat. Les voix de matriçage écrivent des données audio sur un périphérique audio.
-   La mixage est le processus consistant à combiner différentes voix discrètes, telles que les effets sonores et le son d’arrière-plan que l’on entend en fond sonore, en un seul flux. Le sous-mixage est le processus consistant à combiner différents signaux discrets, tels que les sons composant un bruit de moteur, et à créer une voix.
-   Formats audio. La musique et les effets audio peuvent être stockés dans une variété de formats numériques pour votre jeu. Il existe des formats non compressés, comme WAV, et des formats non compressés, comme MP3 et OGG. Plus un échantillon est compressé, moins la fidélité est bonne. En règle générale, plus le taux d’échantillonnage est bas, plus la perte de qualité audio est grande. La fidélité pouvant varier suivant les schémas de compression et les taux d’échantillonnage, il est recommandé d’expérimenter pour trouver ce qui fonctionne mieux pour votre jeu.
-   Taux d’échantillonnage et qualité. Les sons peuvent être échantillonnés à différents taux, et les sons échantillonnés à un faible taux offrent une fidélité moindre. Le taux d’échantillonnage de la qualité CD est de 44,1 Khz (44100 Hz). Si vous n’avez pas besoin d’une haute fidélité pour un son, vous pouvez choisir un taux d’échantillonnage inférieur. Les taux plus élevés conviennent aux applications audio professionnelles, mais vous n’en avez pas besoin tant que votre jeu n’exige pas un son à niveau de fidélité professionnel.
-   Émetteurs sonores (ou sources). Dans XAudio2, les émetteurs sonores sont des sources qui émettent un son, qu’il s’agisse d’un parasite de bruit de fond ou d’un morceau de rock joué par un jukebox dans une scène de votre jeu. Vous spécifiez les émetteurs à l’aide de coordonnées universelles.
-   Écouteurs de son. L’écouteur de son est souvent le joueur, voire une entité IA dans un jeu plus avancé, qui traite les sons reçus en provenance d’un écouteur. Vous pouvez sous-mixer ce son dans le flux audio pour le faire entendre au joueur, ou l’utiliser pour accompagner une action spécifique en cours de jeu, comme réveiller un garde IA marqué comme écouteur.

## <a name="design-considerations"></a>Considérations de conception


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
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415813">Introduction à XAudio2</a></p></td>
<td align="left"><p>Cette rubrique fournit la liste des fonctions de programmation audio prises en charge par XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415762">Prise en main de XAudio2</a></p></td>
<td align="left"><p>Cette rubrique fournit des informations sur les concepts clés de XAudio2, les versions XAudio2 et le format audio RIFF.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415692">Concepts courants de programmation audio</a></p></td>
<td align="left"><p>Cette rubrique fournit une vue d’ensemble des concepts audio courants qu’un développeur audio doit connaître.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415825">Voix XAudio2</a></p></td>
<td align="left"><p>Cette rubrique donne une vue d’ensemble des voix XAudio2 qui sont utilisées pour le prémixage, le traitement et le matriçage des données audio.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415745">Rappels XAudio2</a></p></td>
<td align="left"><p>Cette rubrique couvre les rappels XAudio 2 qui sont utilisés pour empêcher les ruptures dans la lecture audio.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415739">Graphiques audio XAudio2</a></p></td>
<td align="left"><p>Cette rubrique couvre les graphiques de traitement audio XAudio2 qui prennent en entrée une série de flux de données audio provenant du client, les traitent et délivrent le résultat final sur un périphérique audio.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415756">Effets audio XAudio2</a></p></td>
<td align="left"><p>Cette rubrique traite des effets audio XAudio2, qui prennent des données audio entrantes et effectuent une opération sur celles-ci (par exemple un effet de réverbération) avant de les transmettre.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415821">Diffusion de données audio en continu avec XAudio2</a></p></td>
<td align="left"><p>Cette rubrique couvre la diffusion audio en continu avec XAudio2.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415714">X3DAudio</a></p></td>
<td align="left"><p>Cette rubrique traite de X3DAudio, une interface API utilisée en conjonction avec XAudio2 pour créer l’illusion d’un son provenant d’un point donné dans l’espace 3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415899">Référence de programmation XAudio2</a></p></td>
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
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415779">Procédure: initialiser XAudio2</a></p></td>
<td align="left"><p>Découvrez comment initialiser XAudio2 pour la lecture audio en créant une instance du moteur XAudio2 et en créant une voix mastérisée.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415781">Procédure: charger des fichiers de données audio dans XAudio2</a></p></td>
<td align="left"><p>Apprenez comment remplir les structures nécessaires pour lire les données audio dans XAudio2.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415787">Procédure: lire un son avec XAudio2</a></p></td>
<td align="left"><p>Apprenez à lire des données audio précédemment chargées dans XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415794">Procédure: utiliser des voix prémixées</a></p></td>
<td align="left"><p>Apprenez à définir des groupes de voix pour créer en sortie une seule voix prémixée.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415769">Procédure: utiliser des rappels de voix source</a></p></td>
<td align="left"><p>Apprenez à utiliser les rappels de voix source XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415774">Procédure: utiliser des rappels de moteur</a></p></td>
<td align="left"><p>Apprenez à utiliser les rappels de moteur XAudio2.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415767">Procédure: créer un graphique de traitement audio de base</a></p></td>
<td align="left"><p>Apprenez à créer un graphique de traitement audio, construit à partir d’une seule voix mastérisée et d’une seule voix source.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415772">Procédure: Ajouter ou supprimer dynamiquement des voix d’un graphique audio</a></p></td>
<td align="left"><p>Apprenez à ajouter ou supprimer des voix prémixées dans un graphique qui a été créé en suivant les étapes décrites dans <a href="https://msdn.microsoft.com/library/windows/desktop/ee415767">Procédure: Créer un graphique de traitement audio de base</a>.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415789">Procédure: Créer une chaîne d’effets</a></p></td>
<td align="left"><p>Apprenez à appliquer une chaîne d’effets à une voix pour le traitement personnalisé des données audio de cette voix.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415730">Procédure: Créer un XAPO</a></p></td>
<td align="left"><p>Découvrez comment implémenter <a href="https://msdn.microsoft.com/library/windows/desktop/ee415893"><strong>IXAPO</strong></a> pour créer un objet de traitement audio XAudio2 (XAPO).</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415728">Procédure: Ajouter la prise en charge de paramètre d’exécution à un XAPO</a></p></td>
<td align="left"><p>Apprenez à ajouter la prise en charge de paramètre d’exécution à un XAPO en implémentant l’interface <a href="https://msdn.microsoft.com/library/windows/desktop/ee415896"><strong>IXAPOParameters</strong></a>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415733">Procédure: Utiliser un XAPO dans XAudio2</a></p></td>
<td align="left"><p>Apprenez à utiliser l’effet implémenté en tant que XAPO dans une chaîne d’effets XAudio2.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415723">Procédure: utiliser un XAPOFX dans XAudio2</a></p></td>
<td align="left"><p>Apprenez à utiliser les effets inclus dans XAPOFX dans une chaîne d’effets XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415791">Procédure: diffuser un son en continu à partir du disque</a></p></td>
<td align="left"><p>Apprenez à diffuser des données audio dans XAudio2 en créant un thread séparé pour lire un tampon audio et en utilisant des rappels pour contrôler ce thread.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415798">Procédure: intégrer X3DAudio avec XAudio2</a></p></td>
<td align="left"><p>Apprenez à utiliser X3DAudio pour fournir les valeurs de volume et de tonalité aux voix XAudio2 ainsi que les paramètres de l’effet de réverbération intégré XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415783">Procédure: regrouper des méthodes audio comme un ensemble d’opérations</a></p></td>
<td align="left"><p>Apprenez à utiliser les ensembles d’opérations XAudio2 pour appliquer un groupe d’appels de méthode en même temps.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415765">Débogage des problèmes audio dans XAudio2</a></p></td>
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
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ms696274">À propos de Media Foundation</a></p></td>
<td align="left"><p>Cette section contient des informations générales sur les API Media Foundation et les outils disponibles pour leur prise en charge.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee663601">Media Foundation: notions essentielles</a></p></td>
<td align="left"><p>Cette rubrique présente certains concepts que vous devez comprendre avant d’écrire une application Media Foundation.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ms696219">Architecture Media Foundation</a></p></td>
<td align="left"><p>Cette section décrit la conception générale de Microsoft Media Foundation, ainsi que les primitives et le pipeline de traitement multimédia qu’il utilise.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/dd317910">Capture audio/vidéo</a></p></td>
<td align="left"><p>Cette rubrique explique comment utiliser Microsoft Media Foundation pour effectuer la capture audio et vidéo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/dd317914">Lecture audio/vidéo</a></p></td>
<td align="left"><p>Cette rubrique décrit comment implémenter la lecture audio/vidéo dans votre application.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/dd757927">Formats multimédias pris en charge dans Media Foundation</a></p></td>
<td align="left"><p>Cette rubrique répertorie les formats multimédias que Microsoft Media Foundation prend en charge en mode natif. (Des tiers peuvent prendre en charge des formats supplémentaires en écrivant des plug-in personnalisés.)</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/dd318778">Codage et création de fichier</a></p></td>
<td align="left"><p>Cette rubrique explique comment utiliser Microsoft Media Foundation pour effectuer le codage audio et vidéo et créer des fichiers multimédias.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff819508">Codecs Windows Media</a></p></td>
<td align="left"><p>Cette rubrique décrit comment utiliser les fonctionnalités des codecs audio et vidéo Windows Media pour produire et consommer des flux de données compressées.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ms704847">Référence de programmation Media Foundation</a></p></td>
<td align="left"><p>Cette section contient des informations de référence sur les API Media Foundation.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/aa371827">Exemples du kit de développement logiciel Media Foundation</a></p></td>
<td align="left"><p>Cette section répertorie les exemples d’applications qui montrent comment utiliser Media Foundation.</p></td>
</tr>
</tbody>
</table>

 

### <a name="windows-runtime-xaml-media-types"></a>Types multimédias XAML de Windows Runtime

Si vous utilisez la technologie [interop XAML-DirectX](https://msdn.microsoft.com/library/windows/apps/hh825871), vous pouvez incorporer les API multimédias XAML WindowsRuntime dans vos applications UWP en C++ avec DirectX pour des scénarios de jeu plus simples.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Sujet</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br242926"><strong>Windows.UI.Xaml.Controls.MediaElement</strong></a></p></td>
<td align="left"><p>Élément XAML qui représente un objet contenant des données audio, vidéo ou les deux.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/mt203788">Audio, vidéo et appareil photo</a></p></td>
<td align="left"><p>Découvrez comment incorporer du contenu audio et vidéo de base dans votre application pour la plateforme Windows universelle (UWP).</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/mt187272">MediaElement</a></p></td>
<td align="left"><p>Découvrez comment lire un fichier multimédia stocké localement dans votre application UWP.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/mt187272">MediaElement</a></p></td>
<td align="left"><p>Découvrez comment diffuser un fichier multimédia avec une faible latence dans votre application pour UWP.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/mt282143">Diffusion multimédia</a></p></td>
<td align="left"><p>Découvrez comment utiliser le Contrat Lire sur pour diffuser du contenu multimédia sur un autre appareil à partir de votre application pour UWP.</p></td>
</tr>
</tbody>
</table>

 

## <a name="reference"></a>Référence


-   [Présentation de XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813)
-   [Guide de programmation XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415737)
-   [Vue d’ensemble de Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197)

 

## <a name="related-topics"></a>Rubriquesassociées


-   [Guide de programmation XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415737)

 

 




