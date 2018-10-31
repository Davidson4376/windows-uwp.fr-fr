---
author: QuinnRadich
Description: How to use thumbnail images to help users preview files in UWP apps.
title: Recommandations en matière d'images miniatures dans les applications UWP
label: Thumbnail images
template: detail.hbs
ms.author: quradic
ms.date: 01/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8499f64e265cfeaa0a21c111c1aec1e2d3adb8ff
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5819650"
---
# <a name="thumbnail-images"></a>Images miniatures

Ces recommandations décrivent comment utiliser des images miniatures pour permettre aux utilisateurs de prévisualiser des fichiers lorsqu’ils naviguent dans votre application UWP. 

> **API importantes**: [Énumération ThumbnailMode](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)

## <a name="should-my-app-include-thumbnails"></a>Mon application doit-elle inclure des vignettes?

Si votre application permet aux utilisateurs de parcourir des fichiers, vous pouvez afficher des images miniatures pour les aider à prévisualiser rapidement ces fichiers. 

Utilisez des vignettes dans les cas suivants: 
- Affichage d'aperçus pour de nombreux éléments d'une collection de galerie (comme des fichiers ou des dossiers). Par exemple, une galerie de photos doit utiliser des vignettes pour fournir aux utilisateurs un affichage de petite taille pour chaque image lorsqu’ils parcourent leurs fichiers de photos.

    ![galerie de vidéos](images/thumbnail-gallery.png)

- Affichage d'un aperçu pour un élément individuel d'une liste (comme un fichier spécifique). Par exemple, l’utilisateur peut voir plus d’informations sur un fichier, y compris une plus grande vignette pour un meilleur aperçu, avant de décider d'ouvrir ou non le fichier. 

    ![aperçu de vidéo](images/thumbnail-preview.png)

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées
- Spécifiez le [mode d'affichage des vignettes](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode) (PicturesView, VideosView, DocumentsView, MusicView, ListView ou SingleItem) lorsque vous récupérez des vignettes. Cela garantit l'optimisation des images miniatures pour afficher le type de fichiers que les utilisateurs souhaitent consulter. 
    - Utilisez le mode SingleItem pour récupérer une vignette d'un élément unique, quel que soit le type de fichier. Les autres modes d'affichage de vignettes sont destinés à afficher les aperçus de multiples fichiers. 

- Affichez des images génériques d’espaces réservés à la place de vignettes lors du chargement de ces dernières. Le recours à des espaces réservés permet à votre application de sembler plus réactive, car les utilisateurs peuvent interagir avec les aperçus avant le chargement des vignettes. 

    Les images d’espaces réservés doivent être:
    * Dédiées au type d’élément qu'elles remplacent. Par exemple, les dossiers, les images et les vidéos doivent disposer de leurs propres espaces réservés dédiés. 
    * De la même taille et avoir les mêmes proportions que l’image miniature qu'elles remplacent. 
    * Affichées jusqu'au chargement de l’image miniature. 

- Utilisez des images d’espaces réservés avec des libellés pour représenter les dossiers et les groupes de fichiers afin de les différencier des fichiers individuels.

- Si vous ne pouvez pas récupérer une vignette, affichez une image d’espace réservé. 

- Affichez des informations supplémentaires sur les fichiers lors de la fourniture d'aperçus pour des fichiers de documents et de musique. Les utilisateurs peuvent ainsi identifier les informations clés sur un fichier susceptible de ne pas être immédiatement disponible à partir d’une image miniature uniquement. Par exemple, pour un fichier de musique, vous pourriez afficher le nom de l’artiste, ainsi que la vignette de la pochette de l'album. 

- N'affichez aucune information supplémentaire sur les fichiers pour les fichiers d'images et de vidéos. Dans la plupart des cas, une image miniature suffit pour les utilisateurs qui parcourent des images et des vidéos. 

## <a name="additional-usage-guidelines"></a>Recommandations d’utilisation supplémentaires
[Modes d'affichage des vignettes](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode) recommandés et leurs fonctionnalités:

<table>
<tr>
<th> Afficher des aperçus pour</th>
<th> Modes d'affichage des vignettes </th>
<th> Fonctionnalités des images miniatures récupérées </th>
</tr>
<tr>
<td> Images<br /> Vidéos </td>
<td> PicturesView <br />VideosView </td>
<td> <b>Taille</b>: moyenne, de préférence au moins190 (si la taille de l’image est 190x130) <br />
<b>Proportions</b>: uniformes, rapport d'aspect d’environ0,7 (190x130 si la taille est190) <br />
Vignettes rognées pour les aperçus. <br /> 
Mode idéal pour aligner des images dans une grille en raison des proportions uniformes.  </td>
</tr>
<tr>
<td> Documents<br />Musique </td>
<td> DocumentsView <br />MusicView <br /> ListView</td>
<td> <b>Taille</b>: petite, de préférence au moins 40×40pixels <br />
<b>Proportions</b>: uniformes, rapport d'aspect carré  <br />
Mode idéal pour afficher un aperçu de pochette d’album en raison des proportions carrées. <br /> 
Les documents s’affichent de la même manière que dans une fenêtre de sélecteur de fichiers (recours aux mêmes icônes). </td>
</tr>
</tr>
<tr>
<td> Tout élément unique (quel que soit le type de fichier) </td>
<td> SingleItem </td>
<td> <b>Taille</b>: petite, de préférence au moins 40×40pixels <br />
<b>Proportions</b>: uniformes, rapport d'aspect carré  <br />
Mode idéal pour afficher un aperçu de pochette d’album en raison des proportions carrées. <br /> 
Les documents s’affichent de la même manière que dans une fenêtre de sélecteur de fichiers (recours aux mêmes icônes). </td>
</tr>
</table>

Voici quelques exemples illustrant comment des images miniatures récupérées diffèrent selon le type de fichier et le mode d'affichage des vignettes:
<div class="mx-responsive-img">
<table>
<tr>
<th>Type d'élément</th>
<th>En cas de récupération à l'aide de: <ul><li>PicturesView <li>VideosView</ul></th>
<th>En cas de récupération à l'aide de: <ul><li>DocumentsView <li>MusicView <li>ListView</ul></th>
<th>En cas de récupération à l'aide de: <ul><li>SingleItem</ul></th>
<tr>
<tr>
<td>Image</td>
<td>L’image miniature utilise les proportions d’origine du fichier. <br />
<img src="images/thumbnail-pic-picvidmode.png" alt="Picture thumbnail in picture or video mode"/></td>
<td>La vignette est rognée pour être dotée de proportions carrées. <br />
<img src="images/thumbnail-pic-doclistmusic-modes.png" alt="Picture thumbnail in documents, music, or list modes"/></td>
<td>L’image miniature utilise les proportions d’origine du fichier.<br />
<img src="images/thumbnail-pic-single-mode.png" alt="Picture thumbnail in single mode"/> </td>
</tr>
<tr>
<td>Vidéo</td>
<td>La vignette est dotée d'une icône qui la différencie des images. <br />
<img src="images/thumbnail-vid-picvid-modes.png" alt="Video thumbnail in picture or video mode"/></td>
<td>La vignette est rognée pour être dotée de proportions carrées. <br />
<img src="images/thumbnail-vid-doclistmusic-modes.png" alt="Video thumbnail in documents, music, or list mode"/> </td>
<td>L’image miniature utilise les proportions d’origine du fichier. <br />
<img src="images/thumbnail-vid-single-mode.png" alt="Video thumbnail in single mode"/></td>
</tr>
<tr>
<td>Musique</td>
<td>La vignette est une icône sur un arrière-plan de taille appropriée. La couleur d'arrière-plan est déterminée par celle de la vignette de l'application. <br />
<img src="images/thumbnail-music-picvid-modes.png" alt="Music thumbnail in picture or video mode"/></td>
<td>Si le fichier est doté d'une pochette d’album, la vignette est cette pochette.  <br />
<img src="images/thumbnail-music-doclistmusic-modes.png" alt="Music thumbnail in documents, music, or list mode"/> <br />
Sinon, la vignette est une icône sur un arrière-plan de taille appropriée.</td>
<td>Si le fichier est doté d'une pochette d’album, la vignette est cette pochette, avec les proportions d’origine du fichier.  <br />
<img src="images/thumbnail-music-single-mode.png" alt="Music thumbnail in single mode"/> <br />
Sinon, la vignette est une icône. </td>
</tr>
<tr>
<td>Document</td>
<td>La vignette est une icône sur un arrière-plan de taille appropriée. La couleur d'arrière-plan est déterminée par celle de la vignette de l'application. <br />
<img src="images/thumbnail-docs-picvid-modes.png" alt="Document thumbnail in picture or video mode"/></td>
<td>La vignette est une icône sur un arrière-plan de taille appropriée. La couleur d'arrière-plan est déterminée par celle de la vignette de l'application. <br />
<img src="images/thumbnail-doc-doclistmusic-modes.png" alt="Document thumbnail in documents, music, or list mode"/></td>
<td>La vignette du document, le cas échéant. <br />
<img src="images/thumbnail-doc1-single-mode.png" alt="Document thumbnail in single mode"/><br />
Sinon, la vignette est une icône. <br />
<img src="images/thumbnail-doc2-single-mode.png" alt="Document thumbnail icon in single mode"/></td>
</tr>
<tr>
<td>Dossier</td>
<td>Si le dossier contient un fichier d'image, la vignette de l’image est utilisée.  <br />
<img src="images/thumbnail-dir-picvid-modes.png" alt="Folder thumbnail in picture or video mode"/> <br />
Sinon, aucune vignette n’est récupérée.</td>
<td>Aucune image miniature n’est récupérée.</td>
<td>La vignette est l'icône du dossier.<br />
<img src="images/thumbnail-dir-single-mode.png" alt="Folder icon thumbnail in single mode"/></td>
</tr>
<tr>
<td>Groupe de fichiers</td>
<td>Si le dossier contient un fichier d'image, la vignette de l’image est utilisée.<br />
<img src="images/thumbnail-grp-picvid-modes.png" alt="File group thumbnail in picture or video mode"/> <br /> Sinon, aucune vignette n’est récupérée. </td>
<td>S’il existe un fichier doté d'une pochette d’album parmi les fichiers du groupe, la vignette est cette pochette. <br />
<img src="images/thumbnail-grp-doclistmusic-modes.png" alt="File group thumbnail in documents, music or list mode"/> <br />Sinon, aucune vignette n’est récupérée. </td>
<td>S’il existe un fichier doté d'une pochette d’album parmi les fichiers du groupe, la vignette est cette pochette et utilise les proportions d'origine du fichier. <br />
<img src="images/thumbnail-grp1-single-mode.png" alt="File group thumbnail in picture or video mode"/> <br />Sinon, la vignette est une icône qui représente un groupe de fichiers. <br />
<img src="images/thumbnail-grp2-single-mode.png" alt="File group icon in single mode"/> 
</td>
</tr>
</table>
</div>

## <a name="related-topics"></a>Rubriquesassociées
- [Énumération ThumbnailMode](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)
- [Classe StorageItemThumbnail](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.StorageItemThumbnail)
- [Classe StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)
- [Exemple de vignette de fichier et de dossier (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileThumbnails)
- [Affichage Liste et affichage Grille](../design/controls-and-patterns/lists.md)