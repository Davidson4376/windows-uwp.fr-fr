---
author: Jwmsft
Description: "Suivez ces recommandations pour sélectionner des polices et spécifier leur taille et leur couleur pour les applications UWP."
title: Polices pour les applications UWP
ms.assetid: 1B8B90AD-CDC4-4997-ACDE-871C1E94A929
label: Fonts
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: 0b25dc91a5ec82a83ae24a41854e9eeab8990128

---


# <a name="fonts-for-uwp-apps"></a>Polices pour les applications UWP

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Cet article répertorie les polices recommandées pour les applications UWP. Ces polices sont systématiquement disponibles dans toutes les éditions de Windows 10 prenant en charge les applications UWP.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Propriété FontFamily**](https://msdn.microsoft.com/library/windows/apps/br209655)</li>
</ul>
</div>

Le [guide typographique UWP](typography.md) recommande que les applications utilisent la police Segoe UI. Même si cette police est un excellent choix pour la plupart des applications, vous n’êtes pas contraint de l’utiliser pour tout. Vous pouvez utiliser d’autres polices dans certains cas de figure, notamment pour la lecture, ou pour l’affichage de textes rédigés dans certaines autres langues que l’anglais. 
 
## <a name="sans-serif-fonts"></a>Polices sans-serif

Les polices sans-serif sont un excellent choix pour les titres et les éléments d’interface utilisateur. 

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
<th align="left">Remarques</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left" style="font-family: Arial;">Arial</td>
<td align="left">Normal, Italique, Gras, Italique gras, Noir</td>
<td align="left">Prend en charge les scripts d’Europe et du Moyen-Orient (latin, grec, cyrillique, arabe, arménien et hébreu). L’épaisseur de police Noir prend uniquement en charge les scripts d’Europe.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Calibri;">Calibri</td>
<td align="left">Normal, Italique, Gras, Italique gras, Fin, Italique fin</td>
<td align="left">Prend en charge les scripts d’Europe et du Moyen-Orient (latin, grec, cyrillique, arabe et hébreu). Arabe disponible uniquement en écriture verticale.</td>
</tr>
<td style="font-family: Consolas;">Consolas</td>
<td>Normal, Italique, Gras, Italique gras</td>
<td>Police de largeur fixe qui prend en charge les scripts d’Europe (latin, grec et cyrillique).</td>
</tr>

<tr>
<td style="font-family: Segoe UI;">Segoe UI</td>
<td>Normal, Italique, Italique fin, Italique noir, Gras, Italique gras, Fin, Semi-fin, Semi-gras, Noir</td>
<td>Police d’interface utilisateur pour les scripts d’Europe et du Moyen-Orient (arabe, arménien, cyrillique, géorgien, grec, hébreu, latin) et le script lisu.</td>
</tr>

<tr class="odd">
<td>Segoe UI Historic</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour les scripts historiques</td>
</tr>

<tr class="even">
<td style="font-family: Selawik;">Selawik</td>
<td align="left">Normal, Semi-fin, Fin, Gras, Semi-gras</td>
<td align="left">Police open source métriquement compatible avec Segoe UI destinée aux applications sur d’autres plateformes qui ne veulent pas intégrer Segoe UI. [Obtenir Selawik sur GitHub.](https://github.com/Microsoft/Selawik)</td>
</tr>

<tr class="even">
<td style="font-family: Verdana;">Verdana</td>
<td align="left">Normal, Italique, Gras, Italique gras</td>
<td align="left">Prend en charge les scripts d’Europe (latin, grec, cyrillique et arménien).</td>
</tr>

</tbody>
</table>


## <a name="serif-fonts"></a>Polices serif

Les polices serif sont parfaites pour présenter de grandes quantités de texte. 

<table>
<thead>
<tr class="header">
<th align="left">Famille de polices</th>
<th align="left">Styles</th>
<th align="left">Remarques</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Cambria;">Cambria</td>
<td align="left">Normal</td>
<td align="left">Police serif qui prend en charge les scripts d’Europe (latin, grec et cyrillique).</td>
</tr>
<tr class="even">
<td style="font-family: Courier New;">Courier New</td>
<td align="left">Normal, Italique, Gras, Italique gras</td>
<td align="left">Police serif à largeur fixe qui prend en charge les scripts d’Europe et du Moyen-Orient (latin, grec, cyrillique, arabe, arménien et hébreu).</td>
</tr>
<tr class="odd">
<td style="font-family: Georgia;">Georgia</td>
<td align="left">Normal, Italique, Gras, Italique gras</td>
<td align="left">Prend en charge les scripts d’Europe (latin, grec et cyrillique).</td>
</tr>


<tr class="even">
<td style="font-family: Times New Roman;">Times New Roman</td>
<td align="left">Normal, Italique, Gras, Italique gras</td>
<td align="left">Police héritée qui prend en charge les scripts d’Europe (latin, grec, cyrillique, arabe, arménien, hébreu).</td>
</tr>

</tbody>
</table>

## <a name="symbols-and-icons"></a>Symboles et icônes


<table>
<thead>
<tr class="header">
<th align="left">Famille de polices</th>
<th align="left">Styles</th>
<th align="left">Remarques</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Segoe MDL2 Assets</td>
<td align="left">Normal</td>
<td align="left">Police d’interface utilisateur pour les icônes d’application. Pour plus d’informations, voir l’article [Recommandations en matière d’icônes Segoe MDL2](segoe-ui-symbol-font.md).</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Emoji</td>
<td align="left">Normal</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour les symboles</td>
</tr>
</tbody>
</table>



## <a name="fonts-for-non-latin-languages"></a>Polices pour les langues non latines

Même si beaucoup de ces polices proposent des caractères latins.

<table>
<thead>
<tr class="header">
<th align="left">Famille de polices</th>
<th align="left">Styles</th>
<th align="left">Remarques</th>
</tr>
</thead>
<tbody>

<tr class="odd">
<td style="font-family: Embrima;">Ebrima</td>
<td align="left">Normal, Gras</td>
<td align="left">Police d’interface utilisateur pour les scripts d’Afrique (éthiopien, n’ko, osmanya, tifinagh, vaï).</td>
</tr>
<tr class="even">
<td style="font-family: Gadugi;">Gadugi</td>
<td align="left">Normal, Gras</td>
<td align="left">Police d’interface utilisateur pour les scripts d’Amérique du Nord (syllabiques canadiens, cherokee).</td>
</tr>
<tr class="even">
<td align="left">Javanese Text Normal Police de substitution pour le script javanais</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script javanais</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Leelawadee UI;">Leelawadee UI</td>
<td align="left">Normal, Semi-fin, Gras</td>
<td align="left">Police d’interface utilisateur pour les scripts d’Asie du Sud-Est (bugi, lao, khmer, thaï).</td>
</tr>

<tr class="odd">
<td align="left" style="font-family: Malgun Gothic;">Malgun Gothic</td>
<td align="left">Normal</td>
<td align="left">Police d’interface utilisateur pour le coréen.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft Himalaya;">Microsoft Himalaya</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script tibétain.</td>
</tr>
<!--
<tr class="odd">
<td align="left" style="font-family: Microsoft JhengHei;">Microsoft JhengHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
-->
<tr class="even">
<td align="left" style="font-family: Microsoft JhengHei UI;">Microsoft JhengHei UI</td>
<td align="left">Normal, Gras, Fin</td>
<td align="left">Police d’interface utilisateur pour le chinois traditionnel.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft New Tai Lue;">Microsoft New Tai Lue</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script nouveau taï-lue.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft PhagsPa;">Microsoft PhagsPa</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script Phags-pa.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft Tai Le;">Microsoft Tai Le</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script Taï-le.</td>
</tr>
<!--
<tr class="even">
<td align="left" style="font-family: Microsoft YaHei;">Microsoft YaHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
-->
<tr class="odd">
<td align="left" style="font-family: Microsoft YaHei UI;">Microsoft YaHei UI</td>
<td align="left">Normal, Gras, Fin</td>
<td align="left">Police d’interface utilisateur pour le chinois simplifié.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft Yi Baiti;">Microsoft Yi Baiti</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script Yi.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Mongolian Baiti;">Mongolian Baiti</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script mongolien.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: MV Boli;">MV Boli</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script Tana.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Myanmar Text;">Myanmar Text</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script du Myanmar.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Nirmala UI;">Nirmala UI</td>
<td align="left">Normal, Semilight, Gras</td>
<td align="left">Police d’interface utilisateur pour les scripts d’Asie du Sud (bangla, dévanâgarî, goudjrati, gurmukhi, kannada, malayalam, odia, ol tchiki, cinghalais, sora sompeng, tamoul et telugu)</td>
</tr>

<tr class="odd">
<td align="left" style="font-family: SimSun;">SimSun</td>
<td align="left">Normal</td>
<td align="left">Police Chinese UI héritée. </td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Yu Gothic;">Yu Gothic</td>
<td align="left">Moyen</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left" style="font-family: Yu Gothic UI;">Yu Gothic UI</td>
<td align="left">Normal</td>
<td align="left">Police d’interface utilisateur pour le japonais.</td>
</tr>
</tbody>
</table>


## <a name="globalizinglocalizing-fonts"></a>Polices de globalisation/localisation
Utilisez les API de [mappage de police LanguageFont](https://msdn.microsoft.com/library/windows/apps/br206864) pour l’accès par programmation à la gamme de polices, à la taille, à l’épaisseur et au style recommandés pour une langue particulière. L’objet LanguageFont assure l’accès aux informations de police appropriées pour diverses catégories de contenu, notamment les en-têtes d’interface utilisateur, les notifications, le texte de corps et les polices de corps de document modifiables par l’utilisateur. Pour plus d’informations, voir [Ajuster la disposition et les polices pour prendre en charge la globalisation](https://msdn.microsoft.com/windows/uwp/globalizing/adjust-layout-and-fonts--and-support-rtl).


## <a name="get-the-samples"></a>Obtenir les exemples

* [Exemple de polices téléchargeables](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlCloudFontIntegration)
* [Exemple d’éléments de base d’interface utilisateur](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)
* [Exemple d’interligne avec DirectWrite](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/DWriteLineSpacingModes) 

## <a name="related-articles"></a>Articles connexes

* [Ajuster la disposition et les polices pour prendre en charge la globalisation](https://msdn.microsoft.com/windows/uwp/globalizing/adjust-layout-and-fonts--and-support-rtl)
* [Segoe MDL2](segoe-ui-symbol-font.md)
* [Contrôles de texte](../controls-and-patterns/text-controls.md)
* [Ressources de thème XAML](https://msdn.microsoft.com/library/windows/apps/mt187274)

 

 







<!--HONumber=Dec16_HO2-->


