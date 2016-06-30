---
author: drewbatgit
ms.assetid: 0fc12d26-f1cf-4da7-b5a7-735a5074b74a
description: "Cette section fournit des informations sur la création d’une application Windows universelle permettant de capturer, de lire ou de modifier des photos, vidéos ou fichiers audio."
title: "Audio, vidéo et appareil photo"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 8480eb181b6af26eae5950a1f1bf040d0cb5db99

---

# Audio, vidéo et appareil photo

\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Cette section fournit des informations sur la création d’une application Windows universelle permettant de capturer, de lire ou de modifier des photos, vidéos ou fichiers audio.
 
| Rubrique                                                                                             | Description                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Capturer des photos et des vidéos à l’aide de CameraCaptureUI](capture-photos-and-video-with-cameracaptureui.md) | Cet article décrit comment utiliser la classe [CameraCaptureUI](capture-photos-and-video-with-cameracaptureui.md) afin de capturer des photos ou des vidéos à l’aide de l’interface utilisateur de l’appareil photo intégré à Windows.                                                                                                            |
| [Capturer des photos et des vidéos à l’aide de MediaCapture](capture-photos-and-video-with-mediacapture.md)       | Cet article décrit les étapes permettant de capturer des photos et des vidéos à l’aide de l’API [MediaCapture](https://msdn.microsoft.com/library/windows/apps/br241124), y compris celles pour initialiser et arrêter MediaCapture, ainsi que pour gérer les modifications relatives à l’orientation de l’appareil.                                  |
| [Détecter les visages dans des images ou des vidéos](detect-and-track-faces-in-an-image.md)                         | Cette rubrique vous montre comment utiliser [FaceTracker](https://msdn.microsoft.com/library/windows/apps/dn974150), qui est optimisé pour suivre les visages au fil du temps dans une séquence d’images vidéo.                                                                                                               |
| [Compositions multimédias et modification](media-compositions-and-editing.md)                               | Cet article vous montre comment utiliser les API de l’espace de noms [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565) pour développer rapidement des applications qui permettent aux utilisateurs de créer des compositions multimédias à partir de fichiers sources audio et vidéo.                                    |
                                                                                                                                        | [Effets vidéo personnalisés](custom-video-effects.md)                               | Cet article explique comment créer un composant Windows Runtime implémentant l’interface IBasicVideoEffect pour créer des effets personnalisés de flux vidéo.                                                                                                                                |
| [Imagerie](imaging.md)                                                                             | Cet article explique comment charger et enregistrer des fichiers image à l’aide de l’objet [SoftwareBitmap](https://msdn.microsoft.com/library/windows/apps/dn887358) pour représenter des images bitmap.                                                                                                                     |
| [Propriétés d’informations des appareils audio](audio-device-information-properties.md)                                                                             | Cet article répertorie les propriétés d’informations liées aux appareils audio                                                                                                                      |
| [Transcoder des fichiers multimédias](transcode-media-files.md)                                                 | Vous pouvez utiliser les API [Windows.Media.Transcoding](https://msdn.microsoft.com/library/windows/apps/br207105) pour transcoder des fichiers vidéo d’un format vers un autre.                                                                                                                                |
| [Traiter des fichiers multimédias en arrière-plan](process-media-files-in-the-background.md)                 | Cet article vous montre comment utiliser [MediaProcessingTrigger](https://msdn.microsoft.com/library/windows/apps/dn806005) et une tâche en arrière-plan pour traiter des fichiers multimédias en arrière-plan.                                                                                             |
| [Lecture de média avec MediaSource](media-playback-with-mediasource.md)                             | La classe [MediaSource](https://msdn.microsoft.com/library/windows/apps/dn930905) offre une méthode courante de référencement et de lecture de média à partir de différentes sources telles que des fichiers locaux ou à distance et elle présente un modèle commun d’accès aux données multimédias, quel que soit le format du média sous-jacent.  |
| [Diffusion en continu adaptative](adaptive-streaming.md)                                                       | Cet article décrit comment doter une application de plateforme Windows universelle (UWP) d’une fonctionnalité de lecture de contenu multimédia à diffusion en continu adaptative. Cette fonctionnalité prend actuellement en charge la lecture de contenu Http Live Streaming (HLS) et Dynamic Streaming over HTTP (DASH).                                          |
| [Contenu audio en arrière-plan](background-audio.md)                                                           | Cet article décrit comment créer des applications UWP permettant de lire du contenu audio en arrière-plan.                                                                                                                                                                                                               |
| [Contrôles de transport de média système](system-media-transport-controls.md)                             | La classe [SystemMediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn278677) permet à votre application d’utiliser les contrôles de transport de média système intégrés à Windows et de mettre à jour les métadonnées affichées par les contrôles concernant le média lu actuellement par votre application. |
| [Diffusion multimédia](media-casting.md)                                                                 | Cet article vous montre comment procéder à une diffusion multimédia sur des appareils distants à partir d’une application Windows universelle.                                                                                                                                                                                                       |
| [Graphiques audio](audio-graphs.md)                                                                   | Cet article montre comment utiliser les API dans l’espace de noms [Windows.Media.Audio](https://msdn.microsoft.com/library/windows/apps/dn914341) pour créer des graphiques pour le routage audio, le mixage et les scénarios de traitement audio.                                                                            |
| [MIDI](midi.md)                                                                                   | Cet article vous montre comment énumérer des appareils MIDI (Musical Instrument Digital Interface) et envoyer et recevoir des messages MIDI à partir d’une application Windows universelle.                                                                                                                                   |
| [Lampe torche indépendante de l’appareil photo](camera-independent-flashlight.md)                                 | Cet article montre comment accéder à la lampe d’un appareil et comment l’utiliser, le cas échéant. La fonctionnalité de lampe est gérée indépendamment des fonctionnalités de flash et d’appareil photo.                                                                                                                 |
| [Codecs pris en charge](supported-codecs.md)                                                           | Cet article répertorie les codecs et formats audio et vidéo pris en charge dans les applications UWP.                                                                                                                                                                                                                  |
| [Gestion des droits numériques par PlayReady](playready-client-sdk.md)                                                          | Cette rubrique explique comment ajouter du contenu multimédia PlayReady protégé à votre application de plateforme Windows universelle (UWP).                                                                                                                                                                                |
| [Extension EME (Encrypted Media Extension) PlayReady](playready-encrypted-media-extension.md)                     | Cette section explique comment modifier votre application web PlayReady pour prendre en charge les modifications apportées entre la version Windows 8.1 précédente et la version Windows 10.                                                                                                                                       |

 

 

 







<!--HONumber=Jun16_HO4-->


