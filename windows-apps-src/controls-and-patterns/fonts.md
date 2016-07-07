---
author: Jwmsft
Description: "Suivez ces recommandations pour sélectionner des polices et spécifier leur taille et leur couleur."
title: Polices
ms.assetid: 1B8B90AD-CDC4-4997-ACDE-871C1E94A929
label: Fonts
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 268884c011caa18f3afb6d8b0d9cfda1ec27f51e

---

# Recommandations en matière de polices

**API importantes**

-   [**Propriété FontFamily**](https://msdn.microsoft.com/library/windows/apps/br209655)

Une utilisation appropriée des polices, aussi bien en termes de taille, d’épaisseur, de couleurs, d’interlettrage que d’espace, permet de conférer à votre application de plateforme Windows universelle (UWP) une apparence nette et sobre qui en facilite l’utilisation. Suivez ces recommandations pour sélectionner des polices et spécifier leur taille et leur couleur.

Si vous recherchez une liste d’icônes Segoe UI Symbol, voir [**Guidelines for Segoe UI Symbol icons**](segoe-ui-symbol-font.md).

## <span id="The_Windows_10_type_ramp"></span><span id="the_windows_10_type_ramp"></span><span id="THE_WINDOWS_10_TYPE_RAMP"></span>Gamme de types Windows10


La gamme de types établit une relation fondamentale entre les titres et le corps du texte et garantit une hiérarchie claire et compréhensible entre les différents niveaux. Les utilisateurs comprennent immédiatement où se trouvent les informations et comment analyser la page.

Voici la gamme que nous recommandons pour les applications UWP :

| Style du texte | Police | Épaisseur    | Taille (epx) | Interligne (epx) | Espacement des mots | Interlettrage (1/1 000 em) | Clé de style XAML          |
|------------|----------|-----------|------------|--------------------|--------------|----------------------|-------------------------|
| En-tête     | Segoe UI | Fine     | 46         | 56                 | 100 %         | 0                    | HeaderTextBlockStyle    |
| Sous-titre  | Segoe UI | Fine     | 34         | 40                 | 100 %         | 0                    | SubheaderTextBlockStyle |
| Titre      | Segoe UI | Semilight | 24         | 28                 | 100 %         | 0                    | TitleTextBlockStyle     |
| Sous-titre   | Segoe UI | Normal   | 20         | 24                 | 100 %         | 0                    | SubtitleTextBlockStyle  |
| Base       | Segoe UI | Semibold  | 15         | 20                 | 100 %         | 0                    | BaseTextBlockStyle      |
| Corps       | Segoe UI | Normal   | 15         | 20                 | 100 %         | 0                    | BodyTextBlockStyle      |
| Légende    | Segoe UI | Normal   | 12         | 14                 | 100 %         | 0                    | CaptionTextBlockStyle   |

 

## <span id="Recommended_fonts"></span><span id="recommended_fonts"></span><span id="RECOMMENDED_FONTS"></span>Polices recommandées


Vous n’êtes pas obligé d’utiliser la police SegoeUI pour tous les éléments. Vous pouvez utiliser d’autres polices dans certains cas de figure, notamment pour la lecture, ou pour l’affichage de textes rédigés dans une autre langue que l’anglais.

Voici la liste des polices systématiquement disponibles dans toutes les éditions de Windows 10 prenant en charge les applications UWP.

**Remarque** Si vous utilisez une police ne figurant pas dans cette liste, votre application peut déclencher un téléchargement automatique des données de police par le biais d’un service Microsoft. Ce téléchargement peut abaisser les performances et créer d’autres problèmes potentiels, en particulier pour les appareils mobiles. Notez également qu’un tel téléchargement peut utiliser une partie du forfait de données mobiles de l’utilisateur ou entraîner des frais liés à la consommation des données mobiles. L’interface utilisateur des applications UWP destinées aux appareils mobiles ne doit comprendre aucune police ne figurant pas dans cette liste.

 

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Famille de polices</th>
<th align="left">Styles</th>
<th align="left">Commentaire</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Arial</td>
<td align="left">Normal, Italique, Gras, Italique gras, Noir</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Calibri</td>
<td align="left">Normal, Italique, Gras, Italique gras, Maigre, Italique maigre</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Cambria</td>
<td align="left">Normal</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Cambria Math</td>
<td align="left">Normal</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Comique Sans MS</td>
<td align="left">Normal, Italique, Gras, Italique gras</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Courier New</td>
<td align="left">Normal, Italique, Gras, Italique gras</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Ebrima</td>
<td align="left">Normal, Gras</td>
<td align="left">Police d’interface utilisateur pour les scripts d’Afrique (éthiopien, n’ko, osmanya, tifinagh, vaï)</td>
</tr>
<tr class="even">
<td align="left">Gadugi</td>
<td align="left">Normal</td>
<td align="left">Police d’interface utilisateur pour les scripts d’Amérique du Nord (syllabiques canadiens, cherokee)</td>
</tr>
<tr class="odd">
<td align="left">Georgia</td>
<td align="left">Normal, Italique, Gras, Italique gras</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Javanese Text Normal Police de substitution pour le script javanais</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script javanais</td>
</tr>
<tr class="odd">
<td align="left">Leelawadee UI</td>
<td align="left">Normal, Semilight, Gras</td>
<td align="left">Police d’interface utilisateur pour les scripts d’Asie du Sud-Est (bugi, lao, khmer, thaï)</td>
</tr>
<tr class="even">
<td align="left">Lucida Console</td>
<td align="left">Normal</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Malgun Gothic</td>
<td align="left">Normal</td>
<td align="left">Police d’interface utilisateur pour le coréen</td>
</tr>
<tr class="even">
<td align="left">Microsoft Himalaya</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script tibétain</td>
</tr>
<tr class="odd">
<td align="left">Microsoft JhengHei</td>
<td align="left">Normal</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Microsoft JhengHei UI</td>
<td align="left">Normal</td>
<td align="left">Police d’interface utilisateur pour le chinois traditionnel</td>
</tr>
<tr class="odd">
<td align="left">Microsoft New Tai Lue</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script nouveau taï-lue</td>
</tr>
<tr class="even">
<td align="left">Microsoft PhagsPa</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script Phags-pa</td>
</tr>
<tr class="odd">
<td align="left">Microsoft Tai Le</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script Taï-le</td>
</tr>
<tr class="even">
<td align="left">Microsoft YaHei</td>
<td align="left">Normal</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Microsoft YaHei UI</td>
<td align="left">Normal</td>
<td align="left">Police d’interface utilisateur pour le chinois simplifié</td>
</tr>
<tr class="even">
<td align="left">Microsoft Yi Baiti</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script Yi</td>
</tr>
<tr class="odd">
<td align="left">Mongolian Baiti</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script mongolien</td>
</tr>
<tr class="even">
<td align="left">MV Boli</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script Tana</td>
</tr>
<tr class="odd">
<td align="left">Myanmar Text</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script du Myanmar</td>
</tr>
<tr class="even">
<td align="left">Nirmala UI</td>
<td align="left">Normal, Semilight, Gras</td>
<td align="left">Police d’interface utilisateur pour les scripts d’Asie du Sud (bangla, dévanâgarî, goudjrati, gurmukhi, kannada, malayalam, odia, ol tchiki, cinghalais, sora sompeng, tamoul, telugu)</td>
</tr>
<tr class="odd">
<td align="left">Segoe MDL2 Assets</td>
<td align="left">Normal</td>
<td align="left">Police d’interface utilisateur pour les icônes d’application</td>
</tr>
<tr class="even">
<td align="left">Segoe Print</td>
<td align="left">Normal</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Segoe UI</td>
<td align="left">Normal, Italique, Gras, Italique gras, Maigre, Semilight, Semibold, Noir</td>
<td align="left">Police d’interface utilisateur pour les scripts européens et du Moyen-Orient (arabe, arménien, cyrillique, géorgien, grec, hébreu, latin), et le script lisu</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Emoji</td>
<td align="left">Normal</td>
<td align="left">La version disponible sur Windows Phone inclut un contour blanc autour de chaque emoji pour vous assurer qu’il apparaîtra sur n’importe quelle couleur de l’arrière-plan. Elle est compatible métriquement avec la version fournie avec Windows.</td>
</tr>
<tr class="odd">
<td align="left">Segoe UI Historic</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour les scripts historiques</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour les symboles</td>
</tr>
<tr class="odd">
<td align="left">SimSun</td>
<td align="left">Normal</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Times New Roman</td>
<td align="left">Normal, Italique, Gras, Italique gras</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Trebuchet MS</td>
<td align="left">Normal, Italique, Gras, Italique gras</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Verdana</td>
<td align="left">Normal, Italique, Gras, Italique gras</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Webdings</td>
<td align="left">Normal</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Wingdings</td>
<td align="left">Normal</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Yu Gothic</td>
<td align="left">Moyen</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Yu Gothic UI</td>
<td align="left">Normal</td>
<td align="left">Police d’interface utilisateur pour le japonais</td>
</tr>
</tbody>
</table>

 

## <span id="related_topics"></span>Rubriques connexes

**Pour les concepteurs**
* [Étiquette (ou bloc de texte)](labels.md)
* [Icônes Segoe UI Symbol](segoe-ui-symbol-font.md) 
           **Pour les développeurs (XAML)**
* [Ressources de thème XAML](https://msdn.microsoft.com/library/windows/apps/mt187274)
* [Disposition d’une page d’application](https://msdn.microsoft.com/library/windows/apps/hh872191)
* [Icônes Segoe UI Symbol](segoe-ui-symbol-font.md)
* [**Propriété TextBlock.FontFamily**](https://msdn.microsoft.com/library/windows/apps/br209655)

**Exemples**
* [Exemple d’affichage de texte XAML](http://go.microsoft.com/fwlink/p/?linkid=238578)
* [Style CSS : personnalisation de votre application exemple](http://go.microsoft.com/fwlink/p/?linkid=231641)
* [Exemple de mappage de police de langue](http://go.microsoft.com/fwlink/p/?linkid=231603)
 

 







<!--HONumber=Jun16_HO4-->


