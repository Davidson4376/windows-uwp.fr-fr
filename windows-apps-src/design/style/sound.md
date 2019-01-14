---
Description: Sound helps complete an application's user experience, and gives them that extra audio edge they need to match the feel of Windows across all platforms.
label: Sound
title: Son
template: detail.hbs
ms.assetid: 9fa77494-2525-4491-8f26-dc733b6a18f6
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
pm-contact: kisai
design-contact: mattben
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 5060012c90ec9cfef093021f44b39321f452e01c
ms.sourcegitcommit: 59f874b6667c3f639d8b0c7eeca886e71bf95614
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2019
ms.locfileid: "9004595"
---
# <a name="sound"></a>Son

![image hero](images/header-sound.svg)

Il existe de nombreuses manières d’utiliser le son pour améliorer votre application. Vous pouvez utiliser les sons pour compléter d’autres éléments de l’interface utilisateur, afin de permettre aux utilisateurs de reconnaître les événements par un son. Le son peut être un élément d’interface utilisateur efficace pour les personnes souffrant de handicaps visuels. Vous pouvez utiliser le son pour créer une atmosphère qui immerge totalement l’utilisateur; par exemple, vous pouvez lire une bande son fantaisiste en arrière-plan d’un jeu de puzzle ou utiliser des effets sonores menaçants pour un jeu d’horreur/de survie.

## <a name="sound-global-api"></a>API de son globale

UWP fournit un système audio aisément accessible qui vous permet de bénéficier d’une expérience audio immersive dans l’ensemble de votre application par le simple actionnement d’un commutateur.

[**ElementSoundPlayer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.elementsoundplayer) est un système audio intégré dans le code XAML qui, une fois activé, déclenche la lecture automatique de sons par tous les contrôles par défaut.
```C#
ElementSoundPlayer.State = ElementSoundPlayerState.On;
```
**ElementSoundPlayer** peut prendre trois états : **On**, **Off** et **Auto**.

S’il est défini sur **Off**, aucun son ne sera émis, quelle que soit la plateforme sur laquelle votre application s’exécute. Si le système audio est défini sur **On**, les sons de votre application seront émis sur chaque plateforme.

L’activation de l’objet ElementSoundPlayer entraîne également l’activation automatique de l’audio spatial (son 3D). Pour désactiver le son 3D (tout en gardant le son activé), désactivez la propriété **SpatialAudioMode** de l’objet ElementSoundPlayer: 

```C#
ElementSoundPlayer.SpatialAudioMode = ElementSpatialAudioMode.Off
```

La propriété **SpatialAudioMode** peut accepter les valeurs suivantes: 
- **Auto**: l’audio spatial est activé lorsque le son est activé. 
- **Off**: l’audio spatial est toujours désactivé, même si le son est activé.
- **On**: l’audio spatial est toujours émis.

Pour plus d’informations sur l’audio spatial et la façon dont XAML le gère, voir [AudioGraph - Audio spatial](/windows/uwp/audio-video-camera/audio-graphs#spatial-audio).

### <a name="sound-for-tv-and-xbox"></a>Son pour Xbox et télévision

Le son constitue un aspect essentiel de l’expérience d’interface à 3 mètres (ou « 10-foot ») et, par défaut, le système **ElementSoundPlayer** présente l’état **Auto**, ce qui signifie que vous n’entendrez le son que si votre application est exécutée sur Xbox.
Pour plus d’informations sur la conception pour Xbox et télévision, voir [Conception pour Xbox et télévision](http://go.microsoft.com/fwlink/?LinkId=760736).

## <a name="sound-volume-override"></a>Substitution du volume sonore

Tous les sons au sein de l’application peuvent être atténués avec le contrôle **Volume**. En revanche, le volume des sons de l’application ne peut pas être *plus élevé que le volume système*.

Pour définir le niveau de volume de l’application, appelez la méthode suivante:
```C#
ElementSoundPlayer.Volume = 0.5;
```
où le volume maximal (par rapport au volume système) est de 1.0, et le volume minimal de 0.0 (essentiellement silencieux).

## <a name="control-level-state"></a>État du niveau d’un contrôle

Si le son par défaut d’un contrôle n’est pas souhaité, il peut être désactivé. Cette opération est effectuée par le biais de l’utilisation de l’élément **ElementSoundMode** sur le contrôle.

L’élément **ElementSoundMode** peut prendre deux états : **Off** et **Default**. Lorsqu’il n’est pas défini, il présente la valeur **Default**. S’il est défini sur **Off**, chaque son lu par le contrôle sera désactivé, *sauf pour le focus*.

```XAML
<Button Name="ButtonName" Content="More Info" ElementSoundMode="Off"/>
```

```C#
ButtonName.ElementSoundState = ElementSoundMode.Off;
```

## <a name="is-this-the-right-sound"></a>Est-ce le son approprié?

Lorsque vous créez un contrôle personnalisé ou que vous modifiez le son d’un contrôle existant, il est important de bien comprendre les utilisations de tous les sons fournis par le système.

Chaque son se rapporte à une interaction utilisateur de base spécifique, et bien que vous puissiez personnaliser les sons afin qu’ils soient émis pour n’importe quelle interaction, cette section illustre les scénarios qui nécessitent l’utilisation de sons pour garantir une expérience cohérence dans l’ensemble des applications UWP.

### <a name="invoking-an-element"></a>Invocation d’un élément

Le son le plus courant déclenché par un contrôle dans notre système actuel est le son **Invoke**. Ce son est émis lorsqu’un utilisateur invoque un contrôle par le biais d’un appui, d’un clic, d’une sélection de la touche Entrée ou de la barre d’espace ou d’une pression de la touche «A» d’un boîtier de commande.

En règle générale, ce son est uniquement émis quand un utilisateur cible explicitement un contrôle simple ou une partie d’un contrôle par l’intermédiaire d’un [périphérique d’entrée](../input/index.md).

<Clip audio SelectButtonClick.mp3 ici>

Pour lire ce son à partir d’un événement de contrôle quelconque, il vous suffit d’appeler la méthode Play à partir du système **ElementSoundPlayer** et de transmettre l’élément **ElementSound.Invoke** :
```C#
ElementSoundPlayer.Play(ElementSoundKind.Invoke);
```

### <a name="showing--hiding-content"></a>Affichage et masquage de contenu

Le code XAML offre de nombreux menus volants, boîtes de dialogue et interfaces utilisateur révocables, et toute action déclenchant l’une de ces superpositions doit appeler un son **Show** ou **Hide**.

Lorsqu’une fenêtre de contenu superposée s’affiche, le son **Show** doit être appelé :

<Clip audio OverlayIn.mp3 ici>

```C#
ElementSoundPlayer.Play(ElementSoundKind.Show);
```
À l’inverse, quand une fenêtre de contenu superposée se ferme (ou fait l’objet d’un abandon interactif), il convient d’appeler le son **Hide** :

<Clip audio OverlayOut.mp3 ici>

```C#
ElementSoundPlayer.Play(ElementSoundKind.Hide);
```
### <a name="navigation-within-a-page"></a>Navigation dans une page

Lors de la navigation entre les panneaux ou les vues d’une page de l’application (voir [onglets et sélecteurs de vue](../controls-and-patterns/tabs-pivot.md)), il est généralement un mouvement bidirectionnel. Cela signifie que vous pouvez accéder à la vue/au panneau suivants ou précédents sans quitter la page de l’application sur laquelle vous vous trouvez.

L’expérience audio associée à ce concept de navigation est représentée par les sons **MovePrevious** et **MoveNext**.

Lors de l’accès à une vue/un panneau considérés comme l’*élément suivant* d’une liste, appelez la méthode :

<Clip audio PageTransitionRight.mp3 ici>

```C#
ElementSoundPlayer.Play(ElementSoundKind.MoveNext);
```
Lors de l’accès à une vue/un panneau considérés comme l’*élément précédent* d’une liste, appelez la méthode :

<Clip audio PageTransitionLeft.mp3 ici>

```C#
ElementSoundPlayer.Play(ElementSoundKind.MovePrevious);
```
### <a name="back-navigation"></a>Navigation vers l’arrière

Lors de l’accès à la page précédente d’une application à partir d’une page donnée, le son **GoBack** doit être appelé :

<Clip audio BackButtonClick.mp3 ici>

```C#
ElementSoundPlayer.Play(ElementSoundKind.GoBack);
```
### <a name="focusing-on-an-element"></a>Placement du focus sur un élément

Le son **Focus** est le seul son implicite dans notre système. Cela signifie que lorsqu’un utilisateur n’interagit directement avec aucun élément, il entend quand même un son.

Le placement du focus se produit quand un utilisateur parcourt une application, que ce soit au moyen d’un boîtier de commande, d’un clavier, d’une télécommande ou de Kinect. En règle générale, le son **Focus** *n’est pas émis lors des événements PointerEntered ou de pointage avec la souris*.

Pour configurer un contrôle afin qu’il lise le son **Focus** lorsqu’il reçoit le focus, appelez la méthode :

<Clip audio ElementFocus1.mp3 ici>

```C#
ElementSoundPlayer.Play(ElementSoundKind.Focus);
```
### <a name="cycling-focus-sounds"></a>Émission successive de différents sons de focus

En complément de l’appel de l’élément **ElementSound.Focus**, le système audio émet successivement par défaut 4 sons différents lors de chaque déclencheur de navigation. Cela signifie que le système n’émettra pas deux fois de suite les deux mêmes sons de focus.

Cette fonctionnalité de lecture successive est destinée à empêcher que les sons de focus ne deviennent monotones ou gênants pour l’utilisateur; les sons de focus sont ceux qui seront émis le plus souvent, et doivent donc se révéler les plus subtils.

## <a name="related-articles"></a>Articles connexes

* [Conception pour Xbox et TV](http://go.microsoft.com/fwlink/?LinkId=760736)
* [Documentation de la classe ElementSoundPlayer](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.elementsoundplayer)
