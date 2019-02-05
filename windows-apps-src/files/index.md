---
ms.assetid: 1901c4c2-5161-435d-bc7b-f40c69cdb138
title: Fichiers, dossiers et bibliothèques
description: Découvrez la lecture et l’écriture de paramètres d’application, les sélecteurs de fichiers et de dossiers, et les emplacements de bac à sable «sandbox» tels que la bibliothèque de musique/vidéos.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 517302b7db1e7b65ca7a181a8c766ce1e71737ac
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/04/2019
ms.locfileid: "9044543"
---
 # <a name="files-folders-and-libraries"></a>Fichiers, dossiers et bibliothèques


Vous utilisez les API dans les espaces de noms [Windows.Storage](https://msdn.microsoft.com/library/windows/apps/br227346), [Windows.Storage.Streams](https://msdn.microsoft.com/library/windows/apps/br241791) et [Windows.Storage.Pickers](https://msdn.microsoft.com/library/windows/apps/br207928) pour lire et écrire du texte et d’autres formats de données dans des fichiers, et gérer les fichiers et dossiers. Cette section traite également de la lecture et de l’écriture de paramètres d’application, des sélecteurs de fichiers et de dossiers, et des emplacements bac à sable (sandbox) tels que la bibliothèque de musique/vidéos.

| Rubrique | Description  |
|-------|--------------|
| [Énumérer et interroger des fichiers et dossiers](quickstart-listing-files-and-folders.md) | Accédez aux fichiers et dossiers dans un dossier, une bibliothèque, un appareil ou un emplacement réseau. Vous pouvez également interroger les fichiers et dossiers situés dans un emplacement en créant des requêtes de fichiers et de dossiers. |
| [Créer, écrire et lire un fichier](quickstart-reading-and-writing-files.md) | Lisez et écrivez un fichier à l’aide d’un objet [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171). |
| [Obtenir les propriétés du fichier](quickstart-getting-file-properties.md) | Obtenez les propriétés (de niveau supérieur, de base et étendues) d’un fichier représenté par un objet [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171). |
| [Ouvrir des fichiers et dossiers à l’aide d’un sélecteur](quickstart-using-file-and-folder-pickers.md) | Accédez aux fichiers et dossiers en permettant à l’utilisateur d’interagir avec ceux-ci à l’aide d’un sélecteur. Vous pouvez utiliser [FolderPicker](https://msdn.microsoft.com/library/windows/apps/br207881) pour accéder à un dossier. |
| [Enregistrer un fichier avec un sélecteur](quickstart-save-a-file-with-a-picker.md) | Utilisez [FileSavePicker](https://msdn.microsoft.com/library/windows/apps/br207871) pour permettre aux utilisateurs de spécifier le nom et l’emplacement où ils souhaitent que votre application enregistre un fichier. |
| [Accès au contenu du Groupement résidentiel](quickstart-accessing-homegroup-content.md) | Accédez au contenu stocké dans le dossier Groupement résidentiel de l’utilisateur, qui contient des images, de la musique et des vidéos. |
| [Détermination de la disponibilité des fichiers Microsoft OneDrive](quickstart-determining-availability-of-microsoft-onedrive-files.md) | Déterminez si un fichier Microsoft OneDrive est disponible à l’aide de la propriété [StorageFile.isAvailable](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx). |
| [Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md) | Ajoutez les dossiers existants de musique, images ou vidéos dans les bibliothèques correspondantes. Vous pouvez également supprimer des dossiers de bibliothèques, obtenir la liste des dossiers d’une bibliothèque et découvrir des photos, de la musique et des vidéos. |
| [Suivre les fichiers et dossiers récemment utilisés](how-to-track-recently-used-files-and-folders.md) | Effectuez le suivi des fichiers auxquels l’utilisateur accède fréquemment en les ajoutant à la liste des éléments utilisés récemment de votre application. La plateforme gère les éléments récents pour vous, en les triant selon le critère du dernier accès, et en supprimant l’élément le plus ancien quand la limite de 25 éléments est atteinte. Toutes les applications ont leurs propres éléments utilisés récemment. |
| [Suivre les modifications de système de fichiers en arrière-plan](change-tracking-filesystem.md) | Suivre les modifications apportées au système de fichiers, même lorsque l’application n’est pas en cours d’exécution.|
| [Accéder à la carte SD](access-the-sd-card.md) | Vous pouvez stocker des données non essentielles et y accéder sur une carte microSD en option, plus particulièrement sur les appareils mobiles à faible coût dont le stockage interne est limité. |
| [Autorisations d’accès aux fichiers](file-access-permissions.md) | Les applications peuvent accéder à certains emplacements du système de fichiers par défaut. Les applications peuvent également accéder à des emplacements supplémentaires par le biais du sélecteur de fichiers ou par la déclaration de fonctionnalités. |
| [Accès rapide aux propriétés de fichier dans UWP](fast-file-properties.md) | Rassemblez efficacement une liste des fichiers et de leurs propriétés à partir d’une bibliothèque à utiliser dans une application UWP. |

## <a name="related-samples"></a>Exemples connexes
[Exemple d’énumération de dossier](https://go.microsoft.com/fwlink/p/?linkid=619993)

[Exemple d’accès aux fichiers](https://go.microsoft.com/fwlink/p/?linkid=619995)

[Exemple de sélecteur de fichiers](https://go.microsoft.com/fwlink/p/?linkid=619994)
 

 
