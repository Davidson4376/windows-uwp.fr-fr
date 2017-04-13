---
author: DelfCo
Description: "La globalisation consiste à concevoir et développer votre application de sorte qu’elle adapte son comportement aux différents marchés du monde entier sans aucune modification ou personnalisation."
Search.SourceType: Video
title: Globalisation et localisation
ms.assetid: c0791eec-5bb8-4a13-8977-61d7d98e35ce
label: Intro
template: detail.hbs
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: f46a48e6f93dc152a76ab5791d722dbe757a90b6
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="globalization-and-localization"></a>Globalisation et localisation
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Windows est utilisé dans le monde entier, par des publics de diverses cultures, régions et langues. Un utilisateur peut parler une ou même plusieurs langues. Un utilisateur peut se trouver n'importe où dans le monde et peut parler une langue quelconque selon l’endroit où il se trouve. Pour étendre le marché potentiel de votre application, vous pouvez la rendre facilement adaptable grâce aux fonctionnalités de *globalisation* et de *localisation*.

**La globalisation** consiste à concevoir et développer votre application de sorte qu’elle adapte son comportement aux différents marchés du monde entier sans aucune modification ou personnalisation.

Vous pouvez par exemple:

-   Concevoir la disposition de votre application pour qu’elle s’adapte aux différentes longueurs de texte et tailles de police des autres langues dans les libellés et les chaînes de texte.
-   Récupérer du texte et des images liées à la culture à partir de ressources pouvant être adaptées à différents marchés locaux, au lieu de les coder en dur dans le code ou le balisage de votre application.
-   Utiliser les API de globalisation pour afficher les données associées à une mise en forme différente selon la région, notamment les valeurs numériques, les dates, les heures et les devises.

**La localisation** consiste à adapter votre application pour répondre aux besoins linguistiques, culturels et politiques d’un marché local spécifique.

Par exemple:

-   Traduire le texte et les libellés de l'application pour le nouveau marché et créer des ressources distinctes pour la langue utilisée sur ce marché.
-   Modifier si besoin les images liées à la culture, puis les placer dans des ressources distincts.

Regardez cette vidéo pour savoir brièvement comment préparer votre application à une utilisation dans le monde entier : [Introduction à la globalisation et à la localisation](https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-globalization-and-localization).

## <a name="articles"></a>Articles
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Article</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Pratiques conseillées et déconseillées](guidelines-and-checklist-for-globalizing-your-app.md)</p></td>
<td align="left"><p>Suivez ces meilleures pratiques en globalisant vos applications pour un public plus large, et en localisant vos applications pour un marché spécifique.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Utiliser des formats compatibles avec la globalisation](use-global-ready-formats.md)</p></td>
<td align="left"><p>Développez une application dans une perspective de globalisation en mettant correctement en forme les dates, les heures, les nombres et les devises.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Gérer la langue et la région](manage-language-and-region.md)</p></td>
<td align="left"><p>Contrôlez la façon dont Windows sélectionne les ressources de l’interface utilisateur et formate les éléments de l’interface utilisateur de l’application, en utilisant les différents paramètres de langue et de région fournis par Windows.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Utiliser des modèles de format des dates et heures](use-patterns-to-format-dates-and-times.md)</p></td>
<td align="left"><p>Utilisez l’API [<strong>Windows.Globalization.DateTimeFormatting</strong>](https://msdn.microsoft.com/library/windows/apps/br206859) avec des modèles personnalisés pour afficher les dates et les heures dans le format que vous désirez.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG](adjust-layout-and-fonts--and-support-rtl.md)</p></td>
<td align="left"><p>Développez votre application pour prendre en charge les dispositions et les polices de plusieurs langues, notamment le sens du flux de droite à gauche (DàG).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Préparer votre application en vue de sa localisation](prepare-your-app-for-localization.md)</p></td>
<td align="left"><p>Préparez votre application en vue de sa localisation pour d’autres marchés, langues ou régions.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Placer des chaînes d’interface utilisateur dans des ressources](put-ui-strings-into-resources.md)</p></td>
<td align="left"><p>Placez les ressources de type chaîne de votre interface utilisateur dans des fichiers de ressources. Vous pouvez ensuite référencer ces chaînes dans votre code ou votre balisage.</p></td>
</tr>
</tbody>
</table>

 

Consultez également la documentation initialement créée pour Windows 8.x, qui s’applique toujours aux applications de plateforme Windows universelle (UWP) et à Windows 10.

-   [Globalisation de votre application](https://msdn.microsoft.com/library/windows/apps/xaml/hh965328)
-   [Correspondance des langues](https://msdn.microsoft.com/library/windows/apps/xaml/jj673578.aspx)
-   [Valeurs de NumeralSystem](https://msdn.microsoft.com/library/windows/apps/xaml/jj236471.aspx)
-   [Polices internationales](https://msdn.microsoft.com/library/windows/apps/xaml/dn263115.aspx)
-   [Ressources et localisation des applications](https://msdn.microsoft.com/library/windows/apps/xaml/hh710212.aspx)

 

 



