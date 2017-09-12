---
author: Karl-Bridge-Microsoft
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 010663320b4011d853c53bc4121f86a14bfbfe0c
ms.sourcegitcommit: a2908889b3566882c7494dc81fa9ece7d1d19580
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2017
---
# <a name="custom-keyboard-interactions"></a>Personnaliser les interactions clavier

Offrir des expériences d’interaction clavier complètes et cohérentes dans vos applications UWP et personnaliser les commandes des utilisateurs avancés de clavier et ceux qui possèdent des exigences liées au handicap et à l’accessibilité.

Dans cette rubrique, notre objectif principal est la prise en charge de la saisie au clavier grâce aux commandes personnalisées pour les applications UWP sur PC. Toutefois, une expérience de clavier bien conçue est importante pour prendre en charge les outils d’accessibilité tels que le Narrateur de Windows, à l’aide de claviers logiciels tels que le clavier tactile et le clavier visuel.

## <a name="providing-2d-directional-inner-navigation-a-namexyfocuskeyboardnavigation"></a>Mise à disposition de la navigation directionnelle interne en 2D <a name="xyfocuskeyboardnavigation">

Utilisez la propriété **XYFocusKeyboardNavigation** pour prendre en charge la navigation directionnelle interne en 2D des contrôles personnalisés et des groupes de contrôles à l’aide du clavier (touches de direction), du boîtier de commande Xbox (BMD et stick analogique gauche) et de la télécommande Xbox (BMD).

**Remarque** Nous appelons la région de navigation interne d’un contrôle ou d’un groupe de contrôle la *zone directionnelle*.

**XYFocusKeyboardNavigation** possède une valeur de type **XYFocusKeyboardNavigationMode** avec les valeurs possibles **Auto** (par défaut), **Activé** ou **Désactivé**.

Cette propriété n’affecte pas la navigation par onglets, elle affecte seulement la navigation interne des éléments enfants au sein de votre commande ou de votre groupe de contrôles. Les éléments enfants d’une zone directionnelle ne doivent pas être inclus dans la navigation par onglets.

### <a name="default-behavior"></a>Comportement par défaut

Le comportement de navigation directionnelle est basé sur l’ascendance des éléments, ou sur la hiérarchie d’héritage. Si tous les ancêtres sont en mode par défaut, ou configurés en mode **Auto**, le comportement de navigation directionnelle n’est pas pris en charge pour le clavier (le boîtier de commande et la télécommande prennent systématiquement en charge la navigation directionnelle, sauf si configurés de manière explicite en mode **Désactivé**).

### <a name="custom-behavior"></a>Personnaliser le comportement

Configurer cette propriété sur **Activé** maintient votre support de contrôle de navigation interne en 2D (à l’aide des touches de direction du clavier) sur chaque UIElement sous votre contrôle.

Lorsque vous utilisez les touches de direction du clavier, la navigation est limitée à la zone directionnelle (en appuyant sur la touche Tab, vous mettez le focus sur le prochain élément pouvant être désigné comme élément actif hors de la zone directionnelle).

**Remarque** Ce n’est pas le cas lorsque vous utilisez un boîtier de commande ou une télécommande, où la navigation perdure hors de la zone directionnelle vers la prochaine commande pouvant être désignée comme commande active.

Cette propriété affecte seulement la navigation interne avec les touches de direction. Elle n’affecte pas la navigation à l’aide de la touche Tab. Toutes les commandes conservent leur hiérarchie présumée d’ordre d’onglets.

L’image suivante montre trois boutons (A, B et C) contenus dans une zone directionnelle, ainsi qu’un quatrième bouton (D), hors de la zone directionnelle.

![zone directionnelle](images/keyboard/directional-area.png)

*Les touches de direction clavier peuvent déplacer le focus entre les boutons A, B et C, mais pas D*

L’exemple de code suivant montre comment la navigation est affectée lorsque le paramètre **XYFocusKeyboardNavigation** est activé. À l’aide de l’image précédente, A possède le focus initial et les boucles de la touche Tab, par le biais de toutes les commandes (A -&gt; B -&gt; C -&gt; D et à nouveau A), alors que les touches de direction sont limitées à la zone directionnelle.

```XAML
<StackPanel Orientation="Horizontal">
      <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
            <Button Content="A" />
            <Button Content="B" />
            <Button Content="C" />
      </StackPanel>
      <Button Content="D" />
</StackPanel>
```

#### <a name="override-directional-navigation"></a>Substituer la navigation directionnelle

Utilisez les propriétés XYFocusRight/XYFocusLeft/XYFocusTop/XYFocusDown pour substituer les comportements de navigation par défaut.

Voici la même image que l’exemple précédent présentant les trois boutons (A, B et C) contenus dans une zone directionnelle, ainsi qu’un quatrième bouton (D) hors de cette zone.

![zone directionnelle](images/keyboard/directional-area.png)

*Les touches de direction clavier peuvent déplacer le focus entre les boutons A, B et C et jusqu’à D*

Cet exemple de code montre comment substituer le comportement de navigation par défaut de la flèche droite en l’autorisant à naviguer vers une commande hors de la zone directionnelle. Notez que la zone directionnelle ne peut pas être saisie une nouvelle fois à l’aide de la flèche gauche.

```XAML
<StackPanel Orientation="Horizontal">
      <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
            <Button Content="A" />
            <Button Content="B" />
            <Button Content="C" XYFocusRight="{x:Bind ButtonD}" />
      </StackPanel>
      <Button Content="D" x:Name="ButtonD"/>
</StackPanel>
```

Pour plus d’informations, consultez la section [Stratégies de Navigation XYFocus](#set-the-tab-navigation-behavior) plus loin dans cette rubrique.

#### <a name="restrict-navigation-with-disabled"></a>Limiter la navigation grâce à la commande Désactivé

Configurez **XYFocusKeyboardNavigation** sur **Désactivé** pour limiter la navigation à l’aide des touches de direction au sein d’une zone directionnelle.

**Remarque** Configurer cette propriété affecte la navigation clavier à la commande elle-même, juste les éléments enfants de la commande.

Dans l’exemple de code suivant, le parent StackPanel possède le paramètre **XYFocusKeyboardNavigation** défini sur **Activé** et l’élément enfant, C, possède le paramètre **XYFocusKeyboardNavigation** défini sur **Désactivé**. La navigation à l’aide des touches de direction est désactivée uniquement pour les éléments enfants de C.

```XAML
<StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
        <Button Content="A" />
        <Button Content="B" />
        <Button Content="C" XYFocusKeyboardNavigation="Disabled" >
            ...
        </Button>
</StackPanel>
```

#### <a name="use-nested-directional-areas"></a>Utiliser des zones directionnelles imbriquées

Vous pouvez avoir plusieurs niveaux de zones directionnelles imbriquées. Si tous les éléments parents possèdent le paramètre **XYFocusKeyboardNavigation** défini sur **Activé**, les limites de zone sont ignorées par la navigation à l’aide des touches de direction.

Voici une image qui montre trois boutons (A, B et C) contenus dans une zone directionnelle imbriquée, ainsi qu’un quatrième bouton (D) hors de la zone directionnelle.

![zone directionnelle imbriquée](images/keyboard/nested-directional-area.png)

*Les touches de direction clavier peuvent déplacer le focus entre les boutons A, B et C, mais pas D*

Cet exemple de code montre comment spécifier que les zones directionnelles imbriquées prennent en charge la navigation à l’aide des touches de direction à travers les limites de la région.

```XAML
<StackPanel Orientation="Horizontal">
        <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
            <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
                <Button Content="A" />
                <Button Content="B" />
            </StackPanel>
            <Button Content="C" />
        </StackPanel>
        <Button Content="D" />
 </StackPanel>
```

Voici une image montrant quatre boutons (A, B, C et D) où A et B sont contenus dans une zone directionnelle imbriquée et où C et D sont contenus dans une différente zone. Puisque l’élément parent possède le paramètre **XYFocusKeyboardNavigation** défini sur **Désactivé**, les limites de chaque zone imbriquée ne peuvent pas être franchies grâce aux touches de direction.

![zone non directionnelle](images/keyboard/non-directional-area.png)

*Les touches de direction clavier peuvent déplacer le focus entre les boutons A et B et entre les boutons C et D. Cependant, elles ne peuvent pas le déplacer entre les régions*

Cet exemple de code montre comment spécifier les zones directionnelles imbriquées qui ne prennent pas en charge la navigation à l’aide des touches de direction à travers les limites de la région.

```XAML
<StackPanel Orientation="Horizontal">
  <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
    <Button Content="A" />
    <Button Content="B" />
  </StackPanel>
  <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
    <Button Content="C" />
    <Button Content="D" />
  </StackPanel>
</StackPanel>
```

Voici un exemple plus complexe des zones directionnelles imbriquées dans lesquelles:

-   Si A a le focus, il est possible de naviguer uniquement vers E (et vice versa), car il y a une limite de zone non directionnelle rendant B, C et D inaccessible avec les touches de direction.
-   Si B a le focus, il est possible de naviguer uniquement vers C (et vice versa), car D se trouve hors de la zone directionnelle et la limite de la zone non directionnelle empêche l’accès à A et E
-   Si D a le focus, la touche Tab doit être utilisée pour naviguer entre les commandes, puisque la navigation à l’aide des touches de direction est impossible

![zone non directionnelle imbriquée](images/keyboard/nested-non-directional-area.png)

*Les touches de direction clavier peuvent déplacer le focus entre les boutons A et E et entre les boutons B et C. Cependant, elles ne peuvent pas le déplacer entre les autres régions*

Cet exemple de code montre comment spécifier les zones directionnelles imbriquées qui prennent en charge la navigation à l’aide des touches de direction complexe à travers les limites de la région.

```XAML
<StackPanel  Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
  <Button Content="A" />
    <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Disabled">
      <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
        <Button Content="B" />
        <Button Content="C" />
      </StackPanel>
        <Button Content="D" />
    </StackPanel>
  <Button Content="E" />
</StackPanel>
```

## <a name="set-the-tab-navigation-behavior-a-nametab-navigation"></a>Définir le comportement de navigation par onglets <a name="tab-navigation">

La propriété de l’UIElement.[TabFocusNavigation](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Controls.Control.TabNavigation) spécifie le comportement de navigation par onglets pour la totalité de son arbre d’objets (ou de sa zone directionnelle).

[TabFocusNavigation](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Controls.Control.TabNavigation) possède une valeur de type **TabFocusNavigationMode** avec les valeurs possibles de **Once**, **Cycle,** **ou Local** (par défaut).

Dans l’image suivante, en fonction de la navigation par onglets de la zone directionnelle, le focus est déplacé des manières suivantes:

-   Once: A, B1, C, A
-   Local: A, B1, B2, B3, B4, B5, C, A
-   Cycle: A, B1, B2, B3, B4, B5, B1, B2, B3, B4, B5, (boucle sur B)

![navigation par onglets](images/keyboard/tab-navigation.png)

*Comportement du focus basé sur le mode de navigation par onglets*

L’exemple de code suivant illustre l’utilisation de la valeur TabFocusNavigation en mode Once.

```XAML
<Button Content="X" Click="OnAClick"/>
<StackPanel Orientation="Horizontal" XYFocusNavigation ="Local"
   TabFocusNavigation ="Local">
   <Button Content="A" Click="OnAClick"/>
   <StackPanel Orientation="Horizontal" TabFocusNavigation ="Once">
        <Button Content="B" Click="OnBClick"/>
        <Button Content="C" Click="OnCClick"/>
        <Button Content="D" Click="OnDClick"/>
   </StackPanel>
   <Button Content="E" Click="OnBClick"/>
</StackPanel>
```

*La navigation par onglets lorsque le focus est sur X est: A, B, E, X*

#### <a name="about-tabfocusnavigation-and-tabindex"></a>À propos de TabFocusNavigation et TabIndex

La propriété UIElement.TabFocusNavigation a le même comportement que la propriété Control.TabNavigation, y compris la manière de fonctionner avec TabIndex.

Lorsqu’un contrôle n’a aucun TabIndex spécifié, l’infrastructure lui attribue une valeur d’index supérieure à la valeur d’index actuelle la plus élevée (et la priorité la plus basse). Cela résout l’ambiguïté en choisissant le premier élément dans l’arborescence d’éléments visuels. L’infrastructure résout les index de tabulation par étendue. Les enfants d’un contrôle sont considérés comme une étendue. Si l’un de ces enfants a des enfants, ceux-ci font partie d’une autre étendue.

Dans l’image suivante, en fonction de la navigation par onglets de la zone directionnelle et de l’index de tabulation des éléments, le focus est déplacé des manières suivantes:

-   Once: A, B3, C, A.
-   Local: A B3, B4, B5, B1, B2, C, A.
-   Cycle: A, B3, B4, B5, B1, B2, B3, B4, B5, B1, B2, (boucle sur B)

![index de tabulation](images/keyboard/tab-index.png)

*Comportement du focus basé sur le mode de navigation par onglets et sur les index de tabulation*

Notez la manière dont la zone directionnelle est considérée comme une étendue et comment la navigation dans le focus se déplace vers le contrôle avec en première ligne la priorité la plus élevée: B3. En réalité, il existe deux étendues: une pour A, zone directionnelle, et C. Il en existe également une autre pour la zone directionnelle. Comme la zone directionnelle n’est pas un TabStop, l’infrastructure bascule l’étendue afin de rechercher le meilleur candidat, puis de manière récursive par le biais de tous les éléments enfants.
