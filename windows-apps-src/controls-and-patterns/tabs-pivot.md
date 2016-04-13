---
Les onglets et sélecteurs de vue permettent aux utilisateurs de naviguer entre les contenus fréquemment consultés.
Onglets et sélecteurs de vue
ms.assetid: 556BC70D-CF5D-4295-A655-D58163CC1824
Onglets et sélecteurs de vue
template: detail.hbs
---
# Onglets et sélecteurs de vue

Les onglets et sélecteurs de vue sont utilisés pour naviguer entre différentes catégories de contenu fréquemment consultées. Le modèle d’onglet/sélecteur de vue est constitué de plusieurs volets de contenu présentant les en-têtes de catégorie correspondants. Les en-têtes restent affichés à l’écran, et leur état de sélection est clairement indiqué. Les utilisateurs savent ainsi en permanence dans quelle catégorie ils se trouvent.
![Exemples d’onglets](images/HIGSecOne_Tabs.png)

Les onglets et sélecteurs de vue reposent en fait sur le même modèle et sont créés à l’aide du contrôle [**Pivot**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx). Les fonctionnalités de base du contrôle Pivot sont décrites plus loin dans cet article.

<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [**Classe Pivot**](https://msdn.microsoft.com/library/windows/apps/dn608241)

## Modèle d’onglet/sélecteur de vue

Lorsque vous créez une application avec le modèle d’onglet/sélecteur de vue, vous devez prendre en compte certaines variables de conception clés selon le jeu de fonctionnalités configurable du modèle.

- **Placement de l’en-tête.**   Les en-têtes peuvent être placés sur la partie supérieure ou inférieure de l’écran.
    
    **Remarque**&nbsp;&nbsp;Placer les en-têtes en bas de l’écran nécessite de recréer le modèle du contrôle Pivot.
- **Étiquettes d’en-tête.**  Les en-têtes peuvent être associés à une icône, à du texte ou aux deux.
- **Alignement de l’en-tête.**  Les en-têtes peuvent être alignés à gauche ou centrés.
- **Navigation de niveau supérieur ou inférieur.**  Les onglets/sélecteurs de vue peuvent être utilisés pour les deux niveaux de navigation et peuvent être empilés dans un modèle de niveau supérieur/niveau inférieur. En présence de deux niveaux d’onglets/sélecteurs de vue, les en-têtes de niveau supérieur et de niveau inférieur doivent présenter une différenciation visuelle suffisante, de sorte que les utilisateurs puissent clairement les distinguer.
- **Prise en charge des entrées tactiles**  Pour les appareils qui prennent en charge les entrées tactiles, vous pouvez utiliser un des deux ensembles d’interactions pour naviguer entre les catégories de contenu :
    1. Appuyez sur un en-tête d’onglet/sélecteur de vue pour accéder à cette catégorie, ou effectuez un mouvement de balayage sur la zone de contenu pour accéder à la catégorie adjacente.
    2. Appuyez sur un en-tête d’onglet/sélecteur de vue pour accéder à cette catégorie (aucun mouvement de balayage).

### Configurations de modèle

La disposition optimale du modèle d’onglet/sélecteur de vue dépend du scénario d’interaction et des périphériques sur lesquels votre application s’affiche. Le tableau suivant décrit certains scénarios et configurations de modèle clés.

Scénario d’interaction|Configuration recommandée
--------------------|-------------------------
Déplacement latéral entre 2 à 5 catégories de contenu d’affichage de liste et de grille de niveau supérieur sur un téléphone ou une phablette|Onglet/sélecteurs de vue : placés sur la partie supérieure de l’écran, centrés
|Étiquettes d’en-tête : icônes + texte
|Balayage sur la zone de contenu : activé
Déplacement entre une plage de catégories de contenu sur un téléphone ou une phablette, lorsque le balayage sur une zone de contenu n’est pas pratique pour la navigation|Onglet/sélecteurs de vue : placés sur la partie inférieure de l’écran, centrés
|Étiquettes d’en-tête : icônes + texte
|Balayage sur la zone de contenu : désactivé
Navigation de niveau supérieur avec une souris et un clavier|Onglet/sélecteurs de vue : placés sur la partie supérieure de l’écran, alignés à gauche
 *ou*|Étiquettes d’en-tête : texte uniquement
 Navigation de niveau page sur un appareil tactile|Balayage sur la zone de contenu : désactivé

## Exemples

Cette conception d’une application pour camions-restaurants illustre le placement d’en-têtes d’onglets/sélecteurs de vue sur la partie supérieure ou inférieure. Sur les appareils mobiles, le fait de les placer sur la partie inférieure de l’écran permet de les rendre plus accessibles.

![Exemple d’onglets sur un appareil mobile](images/uap_foodtruck_phone_320_tabsboth.png)

La conception de l’application pour camions-restaurants sur ordinateur de bureau/portable comporte des en-têtes avec texte uniquement. L’utilisation d’icônes avec du texte pour les en-têtes favorise le ciblage tactile. Cependant, avec une souris et un clavier, les en-têtes avec texte uniquement fonctionnent correctement.

![Exemple d’onglets sur ordinateur de bureau](images/uap_foodtruck_desktop_home_700.png)

## Créer un contrôle Pivot

Le modèle de navigation onglet/sélecteur de vue est conçu à l’aide du contrôle [**Pivot**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx). Le contrôle est fourni avec les fonctionnalités de base décrites dans cette section.

Ce code XAML crée un contrôle Pivot de base avec 3 sections de contenu.

```xaml
<Pivot x:Name="rootPivot" Title="PIVOT TITLE">
    <PivotItem Header="Pivot Item 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 1."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 2."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 3."/>
    </PivotItem>
</Pivot>
```

**Éléments du contrôle Pivot**

Le contrôle Pivot est un [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx). Il peut donc contenir une collection d’éléments de n’importe quel type. Tout élément que vous ajoutez au contrôle Pivot qui n’est pas explicitement un élément [**PivotItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx) est implicitement encapsulé dans un PivotItem. Un sélecteur de vue est souvent utilisé pour naviguer entre des pages de contenu. Il est donc courant de remplir la collection [**Items**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) directement avec des éléments d’interface utilisateur XAML. Vous pouvez également affecter à la propriété [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) une source de données. Les éléments liés dans la propriété ItemsSource peuvent être de n’importe quel type. Cependant, s’il ne s’agit pas explicitement d’éléments PivotItem, vous devez définir un [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) et un [**HeaderTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx) pour spécifier leur mode d’affichage.

Vous pouvez utiliser la propriété [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) pour obtenir ou définir l’élément actif du sélecteur de vue. Utilisez la propriété [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) pour obtenir ou définir l’index de l’élément actif. 

**En-têtes de sélecteur de vue**

Vous pouvez utiliser les propriétés [**LeftHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) et [**RightHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) pour ajouter d’autres contrôles à l’en-tête du sélecteur de vue. 

### Interaction avec le sélecteur de vue

Le contrôle offre les interactions d’entrée tactile suivantes :

-   Un appui sur un en-tête permet d’accéder au contenu de la section de cet en-tête.
-   Un mouvement de balayage sur un en-tête vers la gauche ou la droite permet d’accéder à la section/l’en-tête adjacent.
-   Un mouvement de balayage sur une section vers la gauche ou la droite permet d’accéder à la section/l’en-tête adjacent.

Le contrôle est disponible en deux modes :

**Stationnaire**

-   Les sélecteurs de vue sont stationnaires lorsque l’espace autorisé est suffisant pour contenir tous les en-têtes de sélecteur de vue.
-   Un appui sur une étiquette de sélecteur de vue permet d’accéder à la page correspondante, bien que le sélecteur de vue proprement dit ne bouge pas. Le sélecteur de vue actif est mis en surbrillance.

**Carrousel**

-   Les sélecteurs de vue sont présentés sous forme de carrousel lorsque l’espace autorisé n’est pas suffisant pour contenir tous les en-têtes de sélecteur de vue.
-   Un appui sur une étiquette de sélecteur de vue permet d’accéder à la page correspondante. L’étiquette du sélecteur de vue actif passe en première position par rotation.

Le contrôle possède des fonctionnalités de point d’arrêt intégrées, basées sur le nombre d’en-têtes et la longueur de chaîne des étiquettes.

## Recommandations

-   Définissez l’alignement des en-têtes d’onglet/sélecteur de vue selon la taille de l’écran. Pour les largeurs d’écran inférieures à 720 epx, l’alignement au centre est généralement plus efficace. Dans la plupart des cas, l’alignement à gauche est recommandé pour les largeurs d’écran supérieures à 720 epx.
-   Lors de la mise à l’échelle d’une fenêtre, dès lors que l’espace disponible est insuffisant pour contenir tous les en-têtes d’onglet/sélecteur de vue, commencez à déplacer les en-têtes vers la zone de débordement.
-   Les onglets/sélecteurs de vue peuvent être utilisés quelle que soit l’orientation de l’écran. Cependant, veillez à conserver le même nombre total d’en-têtes (visibles et masqués) dans les orientations paysage et portrait.
-   Évitez d’utiliser plus de 5 en-têtes en mode carrousel (rotation), afin de ne pas désorienter l’utilisateur.
-   Sur les appareils mobiles, le placement des onglets/sélecteurs de vue sur la partie inférieure de l’écran favorise l’accessibilité si le balayage est utilisé dans une autre partie de l’interface utilisateur. Ceci permet également d’alléger l’interface utilisateur.
-   Lorsque le clavier visuel est déployé, les en-têtes peuvent être retirés de l’écran afin de gagner de la place.

\[Cet article contient des informations propres aux applications de plateforme Windows universelle (UWP) et à Windows 10. Pour obtenir de l’aide concernant Windows 8.1, téléchargez le [document PDF de recommandations pour Windows 8.1](https://go.microsoft.com/fwlink/p/?linkid=258743) (en anglais).\]

## Rubriques connexes

[Informations de base relatives à la conception de la navigation](https://msdn.microsoft.com/library/windows/apps/dn958438)


<!--HONumber=Mar16_HO1-->


