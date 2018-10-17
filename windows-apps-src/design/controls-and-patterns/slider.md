---
author: QuinnRadich
Description: Lets the user set a value in a given range.
title: Curseurs
ms.assetid: 7EC7EA33-BE7E-4FD5-B205-B8FA7B729ACC
label: Sliders
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: a284625bdfa76fd1fe41948f01e64e1bea3d2644
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4748923"
---
# <a name="sliders"></a>Curseurs

 

Un curseur est un contrôle qui permet à l’utilisateur d’effectuer une sélection parmi une plage de valeurs en déplaçant un contrôle curseur de position le long d’une ligne.

> **API importantes**: [classe Slider](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.slider.aspx), [propriété Value](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.value.aspx), [événement ValueChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.valuechanged.aspx)

![Contrôle de curseur](images/controls/slider.png)


## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Utilisez un curseur lorsque vous voulez donner la possibilité à l’utilisateur de spécifier des valeurs définies et contiguës (telles que le volume ou la luminosité) ou une plage de valeurs distinctes (comme les paramètres de résolution d’écran).

Un curseur représente un bon choix lorsque vous savez que les utilisateurs considèrent la valeur comme une quantité relative, et non comme une valeur numérique. Par exemple, dans le cas du volume, ils le régleront sur faible ou moyen, et pas sur 2 ou 5.

N’utilisez pas un curseur pour les paramètres binaires. Utilisez un [bouton bascule](toggles.md) à la place.

Voici les autres facteurs à prendre en compte lorsque vous hésitez à utiliser un curseur:

-   **Le paramètre s’apparente-t-il à une quantité relative?** Si ce n’est pas le cas, utilisez des [cases d’option](radio-button.md) ou une [zone de liste](lists.md).
-   **Le paramètre est-il une valeur numérique exacte connue?** Si tel est le cas, utilisez une [zone de texte](text-box.md) numérique.
-   **Un utilisateur bénéficiera-t-il de commentaires instantanés sur l’effet de toute modification apportée aux paramètres?** Si tel est le cas, utilisez un curseur. Par exemple, les utilisateurs choisissent plus facilement une couleur lorsqu’ils voient immédiatement le résultat des modifications qu’ils ont apportées aux valeurs de teinte, de saturation ou de luminosité.
-   **Le paramètre a-t-il une plage de quatre valeurs ou plus?** Si ce n’est pas le cas, utilisez des [cases d’option](radio-button.md).
-   **L’utilisateur peut-il changer la valeur?** Les curseurs sont pour l’interaction utilisateur. Pour les valeurs que l’utilisateur ne peut pas modifier, utilisez plutôt du texte en lecture seule.

Si vous devez choisir entre un curseur et une zone de texte numérique, utilisez plutôt une zone de texte numérique si:

-   l’espace de l’écran est réduit ;
-   l’utilisateur préfère probablement utiliser un clavier.

Utilisez un curseur si :

-   les utilisateurs bénéficieront d’un résultat instantané.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/Slider">ouvrir l’application et voir l'objet Curseur en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Un curseur pour contrôler le volume sur Windows Phone.

![Un curseur pour contrôler le volume sur Windows Phone](images/control-examples/slider-phone.png)

Un curseur pour changer la taille du texte dans les paramètres d’affichage de Windows.

![Un curseur pour changer la taille du texte dans les paramètres d’affichage de Windows](images/control-examples/slider-display-settings.png)

## <a name="create-a-slider"></a>Créer un curseur

Voici comment créer un curseur en XAML.

```xaml
<Slider x:Name="volumeSlider" Header="Volume" Width="200"
        ValueChanged="Slider_ValueChanged"/>
```

Voici comment créer un curseur en code.

```csharp
Slider volumeSlider = new Slider();
volumeSlider.Header = "Volume";
volumeSlider.Width = 200;
volumeSlider.ValueChanged += Slider_ValueChanged;

// Add the slider to a parent container in the visual tree.
stackPanel1.Children.Add(volumeSlider);
```

Vous obtenez et définissez la valeur du curseur à partir de la propriété [Value](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.value.aspx). En réponse aux changements de valeur, vous pouvez utiliser la liaison de données pour créer une liaison avec la propriété Value ou gérer l’événement [ValueChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.valuechanged.aspx).

```csharp
private void Slider_ValueChanged(object sender, RangeBaseValueChangedEventArgs e)
{
    Slider slider = sender as Slider;
    if (slider != null)
    {
        media.Volume = slider.Value;
    }
}
```

## <a name="recommendations"></a>Recommandations

-   Veillez à ce que la taille du contrôle permette à l’utilisateur de définir facilement la valeur souhaitée. Pour les paramètres ayant des valeurs distinctes, assurez-vous que l’utilisateur peut facilement sélectionner n’importe quelle valeur à l’aide de la souris. Assurez-vous que les extrémités du curseur restent toujours dans les limites de la zone d’affichage.
-   Fournissez un retour immédiat pendant ou juste après une sélection de l’utilisateur (lorsque cela est faisable). Par exemple, le contrôle du volume Windows émet un son pour indiquer le volume audio sélectionné.
-   Utilisez des étiquettes pour afficher les différentes valeurs de la plage. Exception : si le curseur est orienté à la verticale et que l’étiquette supérieure est Maximum, Haut, Plus ou un équivalent, vous n’êtes pas obligé d’ajouter les autres étiquettes, car la signification est évidente.
-   Si vous désactivez le curseur, désactivez aussi l’ensemble des étiquettes ou retours visuels associés.
-   Tenez compte de l’orientation du texte lorsque vous définissez la direction du flux et/ou l’orientation de votre curseur. Selon la langue, le script est lu de gauche à droite ou de droite à gauche.
-   N’utilisez pas un curseur comme indicateur de progression.
-   Ne modifiez pas la taille du curseur de défilement à partir de la taille par défaut.
-   Ne créez pas de curseur continu si la plage de valeurs est grande et s’il y a toutes les chances que l’utilisateur ne sélectionne qu’une seule des valeurs représentatives de la plage. Utilisez plutôt ces valeurs comme les seuls points d’ancrage autorisés. Par exemple, si la valeur exprimant une durée peut aller jusqu’à 1 mois mais que l’utilisateur a uniquement besoin de choisir entre 1 minute, 1 heure, 1 jour ou 1 mois, créez un curseur avec seulement 4 points d’ancrage.

## <a name="additional-usage-guidance"></a>Indications d’utilisation supplémentaires

### <a name="choosing-the-right-layout-horizontal-or-vertical"></a>Choix de la disposition appropriée : horizontale ou verticale

Vous pouvez orienter votre curseur à l’horizontale ou à la verticale. Utilisez ces recommandations pour déterminer la disposition à utiliser.

-   Utilisez une orientation naturelle. Par exemple, si le curseur représente une valeur du monde réelle qui est normalement affichée à la verticale (telle que la température), utilisez une orientation verticale.
-   Si le contrôle est utilisé pour effectuer une recherche dans une application multimédia, telle qu’une application vidéo, utilisez une orientation horizontale.
-   Lorsque vous utilisez un curseur dans une page où il est possible de faire un mouvement panoramique dans un sens (horizontal ou vertical), utilisez pour le curseur une orientation différente de celle du panoramique. Sinon, les utilisateurs peuvent balayer le curseur et modifier sa valeur par accident en essayant de faire défiler la page.
-   Si vous ne savez toujours pas quelle orientation utiliser, préférez celle qui s’adapte le mieux à la disposition de votre page.

### <a name="range-direction"></a>Direction de la plage

La direction de la plage est le sens dans lequel vous déplacez le curseur lorsque vous le faites glisser de sa valeur actuelle vers sa valeur maximale.

-   Pour les curseurs verticaux, placez la valeur la plus grande au sommet du curseur, quel que soit le sens de lecture. Par exemple, dans le cas d’un curseur de volume, placez toujours le paramètre de volume maximal en haut du curseur. Pour d’autres types de valeurs (comme les jours de la semaine), suivez la direction de lecture de la page.
-   Pour les curseurs horizontaux, placez la valeur la plus faible du côté gauche du curseur pour les pages qui se lisent de gauche à droite et à droite pour les pages qui se lisent de droite à gauche.
-   La seule exception à la recommandation précédente concerne les barres de recherche multimédias : placez toujours la valeur inférieure à gauche du curseur.

### <a name="steps-and-tick-marks"></a>Étapes et graduations

-   Utilisez des points d’ancrage si vous ne souhaitez pas que le curseur autorise des valeurs arbitraires entre la valeur minimale et la valeur maximale. Par exemple, si vous utilisez un curseur pour spécifier le nombre de tickets de cinéma à acheter, n’autorisez pas les valeurs à virgule flottante. Définissez un point d’ancrage de valeur1.
-   Si vous spécifiez des points d’ancrage, veillez à ce que le dernier point corresponde à la valeur maximale du curseur.
-   Utilisez la graduation lorsque vous voulez indiquer aux utilisateurs l’emplacement de valeurs importantes ou significatives. Par exemple, un curseur qui contrôle un zoom pourrait être pourvu de graduations pour 50 %, 100 % et 200 %.
-   Affichez des graduations lorsque les utilisateurs ont besoin de connaître la valeur approximative du paramètre.
-   Affichez des graduations et l’étiquette des valeurs lorsque les utilisateurs ont besoin de connaître la valeur exacte du réglage choisi, sans interagir avec le contrôle. Sinon, ils peuvent connaître la valeur exacte via l’info-bulle contenant la valeur.
-   Affichez systématiquement des graduations lorsque les points d’ancrage ne sont pas clairement identifiables. Par exemple, si le curseur a une largeur de 200 pixels et a 200 points d’ancrage, vous pouvez masquer les graduations, car les utilisateurs ne remarqueront pas le comportement d’ancrage. Mais s’il existe seulement 10 points d’ancrage, affichez les graduations.

### <a name="labels"></a>Étiquettes

-   **Étiquettes de curseur**

    L’étiquette du curseur indique à quoi sert le curseur.

    -   Utilisez une étiquette sans ponctuation à la fin (il s’agit de la convention pour toutes les étiquettes de contrôle).
    -   Placez les étiquettes au-dessus du curseur lorsque ce dernier se trouve dans un écran où la plupart des étiquettes sont placées au-dessus des contrôles.
    -   Placez les étiquettes sur le côté lorsque le curseur se trouve dans un écran où la plupart des étiquettes sont placées sur le côté des contrôles.
    -   Évitez de placer les étiquettes au-dessous du curseur, car les doigts de l’utilisateur risquent de masquer l’étiquette lorsque ce dernier touche le curseur.
-   **Étiquettes de plage**

    Les étiquettes de plage ou de remplissage décrivent les valeurs minimales et maximales du curseur.

    -   Étiquetez les deux extrémités de la plage du curseur, à moins que cela ne soit pas nécessaire avec une orientation verticale.
    -   Utilisez un seul mot, si possible, pour chaque étiquette.
    -   N’utilisez pas de ponctuation finale.
    -   Veillez à ce que ces étiquettes soient descriptives et balancées. Exemples : Maximum/Minimum, Plus/Moins, Bas/Haut, Faible/Fort.
-   **Étiquettes de valeur**

    Une étiquette de valeur affiche la valeur actuelle du curseur.

    -   Si vous nécessitez une étiquette de valeur, affichez-la sous le curseur.
    -   Centrez le texte relatif au contrôle et ajoutez les unités (telles que pixels).
    -   Étant donné que le contrôle de défilement du curseur est masqué pendant son déplacement, affichez la valeur actuelle d’une autre manière, avec une étiquette ou un autre élément visuel de votre choix. Vous pouvez définir une taille de texte afin d’afficher du texte dans la taille appropriée en dessous du curseur.

### <a name="appearance-and-interaction"></a>Apparence et interaction

Un curseur est constitué d’une piste et d’un contrôle de défilement. La piste a la forme d’une barre (sur laquelle peuvent éventuellement s’afficher différents styles de graduations). Cette barre représente la plage de valeurs proposées à l’utilisateur. Le contrôle de défilement est un sélecteur dont la position est réglable. L’utilisateur définit sa position en appuyant sur la piste ou en déplaçant le contrôle le long de cette piste.

Un curseur offre une large cible tactile. Pour garantir une accessibilité en mode tactile, un curseur ne doit pas être placé trop près du bord de l’écran.

Lors de la conception d’un curseur personnalisé, réfléchissez à la meilleure façon de présenter toutes les informations à l’utilisateur en encombrant le moins possible l’écran. Ajoutez une étiquette de valeur si vous souhaitez indiquer à l’utilisateur les unités utilisées pour l’aider à définir le paramètre. Trouvez une manière attrayante de représenter graphiquement ces valeurs. Prenons l’exemple d’un curseur contrôlant le volume. Ce curseur peut afficher une image de haut-parleur sans aucune onde sonore lorsque le volume est au plus bas et, à l’inverse, une image de haut-parleur avec des ondes sonores lorsque le volume est au plus haut.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-topics"></a>Rubriquesassociées
- [Boutons bascule](toggles.md)
- [Classe Slider](https://msdn.microsoft.com/library/windows/apps/br209614)
