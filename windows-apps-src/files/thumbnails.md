---
Description: Comment utiliser des images miniatures pour permettre aux utilisateurs d'afficher un aperçu des fichiers dans les applications UWP.
title: Recommandations en matière d'images miniatures dans les applications UWP
label: Thumbnail images
template: detail.hbs
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 15984e00b036bf44d6e4a7f60cb6435ea1add291
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63808715"
---
# <a name="thumbnail-images"></a>Images miniatures

Ces recommandations expliquent comment utiliser des images miniatures pour permettre aux utilisateurs d'afficher un aperçu des fichiers lorsqu'ils naviguent dans votre application UWP. 

**API importantes**

-   [**ThumbnailMode**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)

## <a name="should-my-app-include-thumbnails"></a>Mon application doit-elle inclure des miniatures ?

Si votre application permet aux utilisateurs de parcourir des fichiers, vous pouvez afficher des images miniatures pour les aider à prévisualiser rapidement ces fichiers. 

Utilisez des miniatures dans les cas suivants : 
- Affichage d'aperçus pour de nombreux éléments d'une collection de galerie (comme des fichiers ou des dossiers). Par exemple, une galerie de photos doit utiliser des miniatures pour fournir aux utilisateurs un affichage réduit de chaque image lorsqu'ils parcourent leurs fichiers de photos.

    ![Galerie de vidéos](images/thumbnail-gallery.png)

- Affichage d'un aperçu pour un élément individuel d'une liste (comme un fichier spécifique). Par exemple, l'utilisateur peut souhaiter bénéficier d'informations supplémentaires sur un fichier, y compris une miniature de plus grande taille pour un meilleur aperçu, avant de décider d'ouvrir ou non le fichier. 

    ![Aperçu d'une vidéo](images/thumbnail-preview.png)

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées
- Spécifiez le [mode d'affichage des images miniatures](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode) (PicturesView, VideosView, DocumentsView, MusicView, ListView ou SingleItem) lorsque vous récupérez des miniatures. Cela garantit que les images miniatures sont optimisées pour afficher le type de fichiers que les utilisateurs souhaitent consulter. 
    - Utilisez le mode SingleItem pour récupérer la miniature d'un élément unique, quel que soit le type de fichier. Les autres modes d'affichage des images miniatures sont destinés à afficher les aperçus de plusieurs fichiers. 

- Affichez des images génériques d'espaces réservés à la place des miniatures lors du chargement de ces dernières. Le recours à des espaces réservés permet à votre application de sembler plus réactive, car les utilisateurs peuvent interagir avec les aperçus avant le chargement des miniatures. 

    Les images d'espaces réservés doivent être :
    * dédiées au type d'élément qu'elles remplacent. Par exemple, les dossiers, les images et les vidéos doivent disposer de leurs propres espaces réservés dédiés ; 
    * de la même taille et avoir les mêmes proportions que la miniature qu'elles remplacent ; 
    * affichées jusqu'au chargement de l'image miniature. 

- Utilisez des images d'espaces réservés avec des étiquettes de texte pour représenter les dossiers et les groupes de fichiers afin de les différencier des fichiers individuels.

- Si vous ne pouvez pas récupérer une miniature, affichez une image d'espace réservé. 

- Affichez des informations supplémentaires sur les fichiers lors de la prévisualisation de documents et de fichiers de musique. Les utilisateurs peuvent ainsi identifier les informations clés d'un fichier qui ne seraient peut-être pas aussi facilement accessibles à partir d'une image miniature seule. Par exemple, pour un fichier de musique, vous pouvez afficher le nom de l'artiste ainsi que la miniature de la pochette de l'album. 

- N'affichez aucune information supplémentaire pour les fichiers d'images et de vidéos. Dans la plupart des cas, une image miniature suffit pour les utilisateurs qui parcourent des images et des vidéos. 

## <a name="additional-usage-guidelines"></a>Recommandations d'utilisation supplémentaires
[Modes d'affichage recommandés pour les images miniatures](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode) et fonctionnalités associées :

<table>
<tr>
<th> Afficher des aperçus pour</th>
<th> Modes d'affichage des images miniatures </th>
<th> Fonctionnalités des images miniatures récupérées </th>
</tr>
<tr>
<td> Images<br /> Vidéos </td>
<td> PicturesView <br />VideosView </td>
<td> <b>Taille</b> : moyenne, de préférence au moins 190 (si la taille de l'image est de 190 x 130) <br />
<b>Proportions</b> : uniformes, rapport d'aspect d'environ 0,7 (190 x 130 si la taille est de 190) <br />
Images miniatures rognées pour les aperçus. <br /> 
Mode idéal pour aligner des images dans une grille en raison de l'uniformité des proportions.  </td>
</tr>
<tr>
<td> Documents<br />Musique </td>
<td> DocumentsView <br />MusicView <br /> ListView</td>
<td> <b>Taille</b> : petite, au moins 40 × 40 pixels de préférence <br />
<b>Proportions</b> :  uniformes, rapport d'aspect carré  <br />
Mode idéal pour afficher l'aperçu d'une pochette d'album en raison des proportions carrées. <br /> 
Les documents s'affichent de la même manière que dans la fenêtre d'un sélecteur de fichiers (recours aux mêmes icônes). </td>
</tr>
</tr>
<tr>
<td> Tout élément unique (quel que soit le type de fichier) </td>
<td> SingleItem </td>
<td> <b>Taille</b> : petite, au moins 40 × 40 pixels de préférence <br />
<b>Proportions</b> :  uniformes, rapport d'aspect carré  <br />
Mode idéal pour afficher l'aperçu d'une pochette d'album en raison des proportions carrées. <br /> 
Les documents s'affichent de la même manière que dans la fenêtre d'un sélecteur de fichiers (recours aux mêmes icônes). </td>
</tr>
</table>

Voici quelques exemples illustrant comment des images miniatures récupérées diffèrent selon le type de fichier et le mode d'affichage de la miniature :
<div class="mx-responsive-img">
<table>
<tr>
<th>Type d'élément</th>
<th>En cas de récupération à l'aide de : <ul><li>PicturesView <li>VideosView</ul></th>
<th>En cas de récupération à l'aide de : <ul><li>DocumentsView <li>MusicView <li>ListView</ul></th>
<th>En cas de récupération à l'aide de : <ul><li>SingleItem</ul></th>
<tr>
<tr>
<td>Picture</td>
<td>L'image miniature utilise les proportions d'origine du fichier. <br />
<img src="images/thumbnail-pic-picvidmode.png" alt="Picture thumbnail in picture or video mode"/></td>
<td>La miniature est rognée pour être dotée de proportions carrées. <br />
<img src="images/thumbnail-pic-doclistmusic-modes.png" alt="Picture thumbnail in documents, music, or list modes"/></td>
<td>L'image miniature utilise les proportions d'origine du fichier.<br />
<img src="images/thumbnail-pic-single-mode.png" alt="Picture thumbnail in single mode"/> </td>
</tr>
<tr>
<td>Video</td>
<td>La miniature est dotée d'une icône qui la différencie des images. <br />
<img src="images/thumbnail-vid-picvid-modes.png" alt="Video thumbnail in picture or video mode"/></td>
<td>La miniature est rognée pour être dotée de proportions carrées. <br />
<img src="images/thumbnail-vid-doclistmusic-modes.png" alt="Video thumbnail in documents, music, or list mode"/> </td>
<td>L'image miniature utilise les proportions d'origine du fichier. <br />
<img src="images/thumbnail-vid-single-mode.png" alt="Video thumbnail in single mode"/></td>
</tr>
<tr>
<td>Musique</td>
<td>La miniature est une icône sur un arrière-plan de taille appropriée. La couleur d'arrière-plan est déterminée par celle de la vignette de l'application. <br />
<img src="images/thumbnail-music-picvid-modes.png" alt="Music thumbnail in picture or video mode"/></td>
<td>Si le fichier est doté d'une pochette d'album, la miniature correspond à cette pochette.  <br />
<img src="images/thumbnail-music-doclistmusic-modes.png" alt="Music thumbnail in documents, music, or list mode"/> <br />
Sinon, la miniature est une icône sur un arrière-plan de taille appropriée.</td>
<td>Si le fichier est doté d'une pochette d'album, la miniature correspond à cette pochette, avec les proportions d'origine du fichier.  <br />
<img src="images/thumbnail-music-single-mode.png" alt="Music thumbnail in single mode"/> <br />
Sinon, la miniature est une icône. </td>
</tr>
<tr>
<td>Document</td>
<td>La miniature est une icône sur un arrière-plan de taille appropriée. La couleur d'arrière-plan est déterminée par celle de la vignette de l'application. <br />
<img src="images/thumbnail-docs-picvid-modes.png" alt="Document thumbnail in picture or video mode"/></td>
<td>La miniature est une icône sur un arrière-plan de taille appropriée. La couleur d'arrière-plan est déterminée par celle de la vignette de l'application. <br />
<img src="images/thumbnail-doc-doclistmusic-modes.png" alt="Document thumbnail in documents, music, or list mode"/></td>
<td>La miniature du document, le cas échéant. <br />
<img src="images/thumbnail-doc1-single-mode.png" alt="Document thumbnail in single mode"/><br />
Sinon, la miniature est une icône. <br />
<img src="images/thumbnail-doc2-single-mode.png" alt="Document thumbnail icon in single mode"/></td>
</tr>
<tr>
<td>Dossier</td>
<td>Si le dossier contient un fichier d'image, la miniature de l'image est utilisée.  <br />
<img src="images/thumbnail-dir-picvid-modes.png" alt="Folder thumbnail in picture or video mode"/> <br />
Sinon, aucune miniature n'est récupérée.</td>
<td>Aucune image miniature n'est récupérée.</td>
<td>La miniature est l'icône du dossier.<br />
<img src="images/thumbnail-dir-single-mode.png" alt="Folder icon thumbnail in single mode"/></td>
</tr>
<tr>
<td>Groupe de fichiers</td>
<td>Si le dossier contient un fichier d'image, la miniature de l'image est utilisée.<br />
<img src="images/thumbnail-grp-picvid-modes.png" alt="File group thumbnail in picture or video mode"/> <br /> Sinon, aucune miniature n'est récupérée. </td>
<td>S'il existe un fichier doté d'une pochette d'album parmi les fichiers du groupe, la miniature correspond à cette pochette. <br />
<img src="images/thumbnail-grp-doclistmusic-modes.png" alt="File group thumbnail in documents, music or list mode"/> <br />Sinon, aucune miniature n'est récupérée. </td>
<td>S'il existe un fichier doté d'une pochette d'album parmi les fichiers du groupe, la miniature correspond à cette pochette et utilise les proportions d'origine du fichier. <br />
<img src="images/thumbnail-grp1-single-mode.png" alt="File group thumbnail in picture or video mode"/> <br />Sinon, la miniature est une icône qui représente un groupe de fichiers. <br />
<img src="images/thumbnail-grp2-single-mode.png" alt="File group icon in single mode"/> 
</td>
</tr>
</table>
</div>

## <a name="related-topics"></a>Rubriques connexes
- [Énumération ThumbnailMode](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)
- [Classe StorageItemThumbnail](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.StorageItemThumbnail)
- [Classe StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)
- [Exemple de miniature de fichier et de dossier (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileThumbnails)
- [Affichage Liste et affichage Grille](../design/controls-and-patterns/lists.md)