---
Description: Vous pouvez définir un dispositions attaché pour une utilisation avec des conteneurs tels que le contrôle ItemsRepeater.
title: AttachedLayout
label: AttachedLayout
template: detail.hbs
ms.date: 03/13/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6ff73b13acb5f5970bb79755b0bf5706fb12545a
ms.sourcegitcommit: c10d7843ccacb8529cb1f53948ee0077298a886d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58913989"
---
# <a name="attached-layouts"></a>Dispositions attachées

Un conteneur qui délègue sa logique de disposition à un autre objet (par exemple, le panneau de configuration) s’appuie sur l’objet de disposition attaché pour fournir le comportement de disposition pour ses enfants éléments.  Un modèle de disposition attaché offre une flexibilité pour une application de modifier la disposition des éléments lors de l’exécution, ou partager plus facilement les aspects de la disposition entre différentes parties de l’interface utilisateur (par exemple, les éléments dans les lignes d’une table qui semblent être alignées dans une colonne).

Dans cette rubrique, nous abordons ce qu’implique la création d’une disposition attachée (virtualisation et non virtualisation), les concepts et les classes que vous devrez comprendre, et les compromis que vous devrez prendre en compte lors du choix entre eux.

| **Obtenir la bibliothèque d’interface utilisateur Windows** |
| - |
| Ce contrôle est inclus dans le cadre de la bibliothèque d’interface utilisateur de Windows, un package NuGet qui contient les nouveaux contrôles et fonctionnalités de l’interface utilisateur pour les applications UWP. Pour plus d’informations, y compris les instructions d’installation, consultez le [vue d’ensemble de la bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **API importantes** :

> * [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
> * [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)
> * [Mise en page](/uwp/api/microsoft.ui.xaml.controls.layout)
>     * [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
>     * [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)
> * [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)
>     * [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)
>     * [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)
> * [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) (version préliminaire)

## <a name="key-concepts"></a>Concepts clés

Mise en nécessite que deux questions réponses pour chaque élément :

1. Ce que ***taille*** sera cet élément ?

2. Quel sera le ***position*** de cet élément être ?

Système de disposition du XAML, qui répond à ces questions, est brièvement couvert dans le cadre de la discussion de [les panneaux personnalisé](/windows/uwp/design/layout/custom-panels-overview).

### <a name="containers-and-context"></a>Conteneurs et contexte

De Conceptuellement, XAML [panneau](/uwp/api/windows.ui.xaml.controls.panel) remplit deux rôles importants dans le framework :

1. Il peut contenir des éléments enfants et présente la création de branche dans l’arborescence d’éléments.
2. Il s’applique une stratégie de mise en page spécifiques à ces enfants.

Pour cette raison, un panneau dans XAML a souvent été synonyme de disposition, mais techniquement parlant, plus que de disposition.

Le [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater) se comporte comme panneau mais, contrairement au panneau de configuration, elle n’expose pas une propriété Children qui permettrait l’ajout ou de suppression par programmation enfants UIElement.  Au lieu de cela, la durée de vie de ses enfants sont automatiquement gérées par l’infrastructure pour qu’elles correspondent à une collection d’éléments de données.  Bien qu’il n’est pas dérivé de panneau de configuration, il se comporte et est traité par l’infrastructure comme un panneau.

> [!NOTE]
> Le [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) est un conteneur, dérivé de panneau de configuration, qui délègue sa logique pour le fichier joint [disposition](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout) objet.  LayoutPanel est dans *aperçu* et est actuellement disponible uniquement dans le *la version préliminaire* supprime du package WinUI.

#### <a name="containers"></a>Conteneurs

Conceptuellement, [panneau](/uwp/api/windows.ui.xaml.controls.panel) est un conteneur d’éléments qui a également la capacité d’afficher les pixels pour un [arrière-plan](/uwp/api/windows.ui.xaml.controls.panel.background).  Panneaux de fournir un moyen d’encapsuler la logique de disposition commune dans un package facile à utiliser.

Le concept de **attaché disposition** fait la distinction entre les deux rôles de conteneur et de mise en page plus clair.  Si le conteneur délègue sa logique de disposition à un autre objet nous devrions appeler cet objet de la mise en page attaché comme indiqué dans l’extrait de code ci-dessous. Les conteneurs qui héritent de [FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement), telles que le LayoutPanel, exposer automatiquement les propriétés communes qui fournissent l’entrée au processus de mise en page du XAML (par exemple, hauteur et largeur).

```xaml
<LayoutPanel>
    <LayoutPanel.Layout>
        <UniformGridLayout/>
    </LayoutPanel.Layout>
    <Button Content="1"/>
    <Button Content="2"/>
    <Button Content="3"/>
</LayoutPanel>
```

Au cours du processus de mise en page, le conteneur s’appuie sur le fichier joint *UniformGridLayout* à mesurer et à réorganiser ses enfants.

#### <a name="per-container-state"></a>État par conteneur

Avec une disposition attachée, une seule instance de l’objet de mise en page peut être associée *nombreux* conteneurs, comme dans l’extrait de code ci-dessous ; par conséquent, il ne doit pas dépendre ou référencer directement le conteneur hôte.  Exemple :

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

Cette situation *ExampleLayout* devez considérer avec soin l’état qu’il utilise dans son calcul de disposition et où cet état est stocké pour ne pas affecter la disposition des éléments dans un panneau avec les autres.  Il est analogue à un panneau personnalisé dont la logique MeasureOverride et ArrangeOverride dépend des valeurs de ses *statique* propriétés.

#### <a name="layoutcontext"></a>LayoutContext

L’objectif de la [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext) consiste à traiter ces difficultés.  Il fournit la capacité d’interagir avec le conteneur hôte, par exemple pour récupérer des éléments enfants, sans introduire une dépendance directe entre les deux à la disposition attachée. Le contexte permet également la mise en page stocker n’importe quel état qu'elle nécessite qui peut-être être lié aux éléments d’enfants du conteneur.

Dispositions simples, non virtualisation souvent inutile maintenir un état, en rendant un problème. Une disposition plus complexe, telles que de la grille, toutefois, peut choisir maintenir l’état entre la mesure et d’organiser les appel afin d’éviter de recalculer une valeur.

Virtualisation de dispositions *souvent* devez conserver un état entre les deux la mesure et de réorganiser, ainsi qu’entre les passes de disposition itératif.

#### <a name="initializing-and-uninitializing-per-container-state"></a>Initialisation et désinitialisation état par conteneur

Lorsqu’une mise en page est attaché à un conteneur, son [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore) méthode est appelée et offre la possibilité d’initialiser un objet pour stocker l’état.

De même, lorsque la disposition est supprimée d’un conteneur, le [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore) méthode sera appelée.  Cela permet la mise en page pour nettoyer tout état qu’il avait associé à ce conteneur.

Objet d’état de la disposition peut être stockée avec et récupérée à partir du conteneur avec la [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) propriété dans le contexte.

### <a name="ui-virtualization"></a>Virtualisation de l’interface utilisateur

Virtualisation de l’interface utilisateur signifie que ce qui peut retarder la création d’un objet d’interface utilisateur jusqu'à ce que _quand il est nécessaire_.  Il est une optimisation des performances.  Pour déterminer les scénarios qui ne défilent pas _lorsque nécessaire_ peut reposer sur n’importe quel nombre d’éléments qui sont spécifiques à l’application.  Dans ce cas, les applications doivent prendre en compte à l’aide de la [x : Load](../../xaml-platform/x-load-attribute.md). Il ne nécessite pas de traitement spécial dans votre disposition.

Dans les scénarios en fonction du défilement telles qu’une liste, détermination _lorsque nécessaire_ est souvent basé sur « il sera visible pour l’utilisateur » qui dépend fortement où il a été placé au cours du processus de mise en page et nécessite des considérations spéciales.  Ce scénario est une priorité pour ce document.

> [!NOTE]
> Bien que ne pas abordé dans ce document, les mêmes fonctionnalités qu’activer la virtualisation de l’interface utilisateur dans les scénarios de défilement pouvait être appliquées dans les scénarios qui ne défilent pas.  Par exemple, un orientés données contrôle ToolBar qui gère la durée de vie des commandes, il présente et répond aux modifications dans l’espace disponible par recyclage / déplacement d’éléments entre une zone visible et un menu de dépassement de capacité.

## <a name="getting-started"></a>Prise en main

D’abord déterminer si la mise en page que vous créez doit prendre en charge la virtualisation de l’interface utilisateur.

**Quelques éléments à prendre en compte...**

1. Dispositions non-virtualisation sont plus faciles à créer. Si le nombre d’éléments ne sera pas toujours création d’une disposition non virtualisation est recommandé.
2. La plateforme fournit un ensemble de dispositions attachés qui fonctionnent avec le [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater#change-the-layout-of-items) et [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) pour couvrir les besoins courants.  Familiarisez-vous avec celles avant de décider de vous devez définir une disposition personnalisée.
3. Dispositions de virtualisation ont toujours des UC supplémentaire et coût/complexité/surcharge de mémoire par rapport à une disposition non virtualisation.  Une règle générale si les enfants la disposition devront gérer probablement être contenus dans une zone qui est de 3 fois la taille de la fenêtre d’affichage, puis peut-être gains d’une disposition de virtualisation. La taille de 3 x est abordée plus en détail plus loin dans ce document, mais il est dû à la nature asynchrone de défilement sur Windows et son impact sur la virtualisation.

> [!TIP]
> En tant que point de référence, les paramètres par défaut pour le [ListView](/uwp/api/windows.ui.xaml.controls.listview) (et [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)) sont que recyclage ne commence pas tant que le nombre d’éléments est suffisante pour saturer 3 fois la taille de la fenêtre d’affichage actuel.

**Choisissez votre type de base**

![hiérarchie des schémas attachés](images/xaml-attached-layout-hierarchy.png)

La base de [disposition](/uwp/api/microsoft.ui.xaml.controls.layout) type a deux types dérivés qui servent le point de départ pour la création d’une disposition attachée :

1. [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>Disposition non virtualisation

L’approche pour la création d’une disposition non virtualisation devrait sembler familière à toute personne qui a créé un [Panneau de configuration personnalisé](/windows/uwp/design/layout/custom-panels-overview).  Les mêmes concepts s’appliquent.  La principale différence est qu’un [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext) est utilisé pour accéder à la [enfants](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children) collection et la disposition peuvent choisir de stocker l’état.

1. Dériver du type de base [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout) (au lieu du panneau).
2. *(Facultatif)*  Définir des propriétés de dépendance que quand modifié sera invalide la disposition.
3. _(**New**/facultative)_ initialiser n’importe quel objet d’état requise par la mise en page dans le cadre de la [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore). Dissimulation avec le conteneur d’hôte à l’aide de la [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) fourni avec le contexte.
4. Remplacer le [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride) et appelez le [mesure](/uwp/api/windows.ui.xaml.uielement.measure) méthode sur tous les enfants.
5. Remplacer le [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride) et appelez le [organiser](/uwp/api/windows.ui.xaml.uielement.arrange) méthode sur tous les enfants.
6. *(**New**/facultative)* nettoyer tout état enregistré dans le cadre de la [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore).

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>Exemple : Une disposition de pile Simple (éléments de diverses tailles)

![MyStackLayout](images/xaml-attached-layout-mystacklayout.png)

Voici une disposition de pile de virtualisation non très basique des éléments de taille variables. Il ne dispose pas de toutes les propriétés pour ajuster le comportement de la disposition. L’implémentation ci-après illustre la façon dont la disposition s’appuie sur l’objet de contexte fourni par le conteneur pour :

1. Obtenir le nombre d’enfants, et
2. Accéder à chaque élément enfant d’un index.

```csharp
public class MyStackLayout : NonVirtualizingLayout
{
    protected override Size MeasureOverride(NonVirtualizingLayoutContext context, Size availableSize)
    {
        double extentHeight = 0.0;
        foreach (var element in context.Children)
        {
            element.Measure(availableSize);
            extentHeight += element.DesiredSize.Height;
        }

        return new Size(availableSize.Width, extentHeight);
    }

    protected override Size ArrangeOverride(NonVirtualizingLayoutContext context, Size finalSize)
    {
        double offset = 0.0;
        foreach (var element in context.Children)
        {
            element.Arrange(
                new Rect(0, offset, finalSize.Width, element.DesiredSize.Height));
            offset += element.DesiredSize.Height;
        }

        return finalSize;
    }
}
```

```xaml
 <LayoutPanel MaxWidth="196">
    <LayoutPanel.Layout>
        <local:MyStackLayout/>
    </LayoutPanel.Layout>

    <Button HorizontalAlignment="Stretch">1</Button>
    <Button HorizontalAlignment="Right">2</Button>
    <Button HorizontalAlignment="Center">3</Button>
    <Button>4</Button>

</LayoutPanel>
```

## <a name="virtualizing-layouts"></a>Virtualisation de dispositions

Similaire à une disposition non virtualisation, les étapes générales pour obtenir une présentation de virtualisation sont les mêmes.  La complexité est en grande partie dans la détermination des éléments tombent dans la fenêtre d’affichage et doivent être réalisés.

1. Dériver du type de base [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout).
2. (Facultatif) Définir vos propriétés de dépendance que quand modifié sera invalide la disposition.
3. Initialiser n’importe quel objet d’état qui sera requise par la mise en page dans le cadre de la [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore). Dissimulation avec le conteneur d’hôte à l’aide de la [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) fourni avec le contexte.
4. Remplacer le [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) et appelez le [mesure](/uwp/api/windows.ui.xaml.uielement.measure) méthode pour chaque enfant qui doit être mis en œuvre.
   1. Le [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) méthode est utilisée pour récupérer un UIElement qui a été préparé par l’infrastructure (par exemple, les liaisons de données appliquées).
5. Remplacer le [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) et appelez le [organiser](/uwp/api/windows.ui.xaml.uielement.arrange) méthode pour chaque enfant réalisé.
6. (Facultatif) Nettoyer un état enregistré dans le cadre de la [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore).

> [!TIP]
> La valeur retournée par la [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) est utilisé en tant que la taille du contenu virtualisé.

Il existe deux approches générales à prendre en compte lors de la création d’une disposition de virtualisation.  Choisissez une ou l’autre en grande partie dépend « comment vous détermine la taille d’un élément ».  Si son suffisant pour connaître l’index d’un élément dans le jeu de données ou les données proprement dites détermine sa taille éventuelle, nous considérons qu’il **dépendant des données**.  Il s’agit plus simples à créer.  Si, toutefois, la seule façon de déterminer la taille d’un élément consiste à créer et de mesurer l’interface utilisateur, puis nous pouvons dire qu’il est **dépendant du contenu**.  Il s’agit plus complexes.

### <a name="the-layout-process"></a>Le processus de disposition

Si vous créez un données ou la disposition du contenu dépendant, il est important de comprendre le processus de disposition et l’impact du défilement asynchrone des Windows.

Un (pointage) vue simplifiée des étapes effectuées par l’infrastructure de démarrage à l’affichage de l’interface utilisateur sur l’écran est que :

1. Il analyse le balisage.

2. Génère une arborescence d’éléments.

3. Effectue une passe de disposition.

4. Effectue une passe de rendu.

Avec la virtualisation de l’interface utilisateur, création des éléments qui seraient normalement effectuées à l’étape 2 est retardé ou est tronquée une fois son été déterminé que le contenu suffisant a été créé pour remplir la fenêtre d’affichage. Un conteneur de virtualisation (par exemple, ItemsRepeater) s’appuie sur sa disposition attachée au lecteur de ce processus. Il fournit la mise en page attachée avec une [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) qui met en évidence les informations supplémentaires qui a besoin d’une disposition de virtualisation.

**Le RealizationRect (par exemple, une fenêtre d’affichage)**

Le défilement sur Windows se produit asynchrone au thread d’interface utilisateur. Il n’est pas contrôlé par la mise en page de l’infrastructure.  Au lieu de cela, l’interaction et le déplacement se produit dans le compositeur du système. L’avantage de cette approche est que panoramique contenu peut toujours être effectuée à 60fps.  Le défi, toutefois, est que la « fenêtre d’affichage », comme indiqué par la mise en page sont légèrement obsolète par rapport à ce qui est réellement visible sur l’écran. Si un utilisateur fait défiler rapidement, ils peuvent dépasser la vitesse du thread d’interface utilisateur pour générer le nouveau contenu et « effectuer un panoramique sur noir ». Pour cette raison, il est souvent nécessaire pour obtenir une présentation de virtualisation générer une mémoire tampon supplémentaire d’éléments préparées suffisants pour remplir une zone supérieure à la fenêtre d’affichage. Lorsque sous une charge plus importante pendant le défilement de l’utilisateur est toujours présenté avec du contenu.

![Rect de réalisation](images/xaml-attached-layout-realizationrect.png)

Étant donné que la création d’élément est coûteuse, virtualisation de conteneurs (par exemple, [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)) fournit d’abord la mise en page attachée avec une [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) qui correspond à la fenêtre d’affichage. Périodes d’inactivité, le conteneur peut augmenter la mémoire tampon de contenu préparé en effectuant des appels répétés à la disposition à l’aide d’un type Rect. réalisation plus en plus volumineux Ce comportement est une optimisation des performances qui tente de trouver un équilibre entre les temps de démarrage rapide et une bonne expérience panoramique. La taille maximale de mémoire tampon qui génère le ItemsRepeater est contrôlée par son [VerticalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) et [HorizontalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) propriétés.

**Réutiliser des éléments (recyclage)**

La disposition est prévue pour dimensionner et positionner les éléments pour remplir le [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) chaque fois qu’il est exécuté. Par défaut le [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) recycle tous les éléments non utilisés à la fin de chaque phase de disposition.

Le [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) qui est passé à la disposition dans le cadre de la [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) et [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) fournit des informations supplémentaires une disposition de virtualisation doit. Certains aspects plus couramment utilisés, qu'il fournit sont la possibilité de :

1. Le nombre d’éléments dans les données de requête ([ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount)).
2. Récupérer un spécifique en utilisant la [GetItemAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat) (méthode).
3. Récupérer un [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) qui représente la fenêtre d’affichage et la mémoire tampon de la mise en page doit remplir avec réalise des éléments.
4. Demander le UIElement pour un élément spécifique avec le [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) (méthode).

Demande d’un élément d’un index donné entraîne cet élément soit marqué comme « en cours d’utilisation » pour cette étape de la disposition. Si l’élément n’existe pas déjà, puis il est réalisé et automatiquement préparé pour une utilisation (par exemple, gonfler l’arborescence de l’interface utilisateur définie dans un DataTemplate, traitement des liaisons de données, un etc..).  Sinon, il sera récupéré à partir d’un pool d’instances existantes.

À la fin de chaque passe de mesure, existants, réalisé élément qui n’a pas été marqué « en cours d’utilisation » est automatiquement considéré comme disponible pour une réutilisation, sauf si l’option à [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) a été utilisé lors de l’élément a été récupérée le [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) (méthode). Le framework déplace à un pool de recyclage automatiquement et met à disposition. Elles peuvent ensuite être extraites pour une utilisation par un autre conteneur. Le framework tente d’éviter ce problème lorsque cela est possible qu’il existe un coût associé à la ré-apparenter un élément.

Si une mise en page virtualisation sait au début de chaque mesure éléments ne sont plus tombent dans le rectangle de réalisation il peut optimiser sa réutilisation. Au lieu de compter sur le comportement du framework par défaut. La disposition peut titre préventif déplacer des éléments dans le pool de recyclage à l’aide de la [RecycleElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement) (méthode).  Appel de cette méthode avant de demander de nouveaux éléments provoque ces éléments existants être disponibles lors de la mise en page plus tard émet un [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) demande pour un index qui n’est pas déjà associé à un élément.

Le VirtualizingLayoutContext fournit deux propriétés supplémentaires conçues pour les auteurs de mise en page Création d’une disposition de contenu dépendant. Elles sont abordées plus en détail ultérieurement.

1. Un [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) qui fournit un texte facultatif _d’entrée_ à disposition.
2. Un [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) qui est facultatif _sortie_ de la disposition.

## <a name="data-dependent-virtualizing-layouts"></a>Dispositions de virtualisation dépendant des données

Une disposition de virtualisation est plus facile si vous savez ce que la taille de chaque élément doit être sans avoir à mesurer le contenu à afficher.  Dans ce document, nous allons faire simplement référence à cette catégorie de virtualiser des dispositions en tant que **mises en page de données** , car elles impliquent généralement l’inspection des données.  Selon les données d’une application peut choisir une représentation visuelle avec une taille connue - peut-être parce que sa partie des données ou a été précédemment déterminé par leur conception.

L’approche générale est pour la mise en page à :

1. Calculer une taille et la position de chaque élément.
2. Dans le cadre de la [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride):
   1. Utilisez le [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) pour déterminer quels éléments doivent apparaître dans la fenêtre d’affichage.
   2. Récupérer le UIElement que doit représenter l’élément avec la [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) (méthode).
   3. [Mesure](/uwp/api/windows.ui.xaml.uielement.measure) UIElement avec la taille précalculée.
3. Dans le cadre de la [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride), [organiser](/uwp/api/windows.ui.xaml.uielement.arrange) chacun mis en œuvre avec la position précalculée UIElement.

> [!NOTE]
> Une approche de mise en page de données est souvent incompatible avec _la virtualisation des données_.  Plus précisément, où les seules données chargement en mémoire sont que les données nécessaires pour compléter ce qui est visible à l’utilisateur.  La virtualisation des données n’est pas référence au chargement différé ou incrémentiel des données comme un utilisateur fait défiler vers le bas où ces données restent résidentes.  Au lieu de cela, il fait référence à lorsque les éléments sont publiés à partir de la mémoire comme ils sont trouvent en dehors de la vue.  Avoir une disposition de données qui inspecte chaque élément de données comme partie d’une disposition de données susceptibles d’empêcher la virtualisation des données à partir de travail comme prévu.  Une exception est une mise en page comme le UniformGridLayout qui suppose que tout a la même taille.

> [!TIP]
> Si vous créez un contrôle personnalisé pour une bibliothèque de contrôles qui sera utilisé par d’autres dans un large éventail de situations une disposition de données n’est peut-être pas une option pour vous.

### <a name="example-xbox-activity-feed-layout"></a>Exemple : Disposition de flux d’activité Xbox

L’interface utilisateur pour le flux d’activité Xbox utilise un modèle de répétition où chaque ligne comporte une vignette large, suivie de deux vignettes étroites qui est inversée sur la ligne suivante. Dans cette disposition, la taille de chaque élément est une fonction de la position dans le jeu de données et la taille connue pour les vignettes (affiner vs larges).

![Flux d’activité Xbox](images/xaml-attached-layout-activityfeedscreenshot.png)

Le code ci-dessous présente les un personnalisé virtualisation de l’interface utilisateur pour la flux d’activités peut être pour illustrer le général l’approche que vous pouvez prendre un **mise en page de données**.

<table>
<td>
    <p>Si vous avez le <strong style="font-weight: semi-bold">galerie de contrôles XAML</strong> application installée, cliquez ici pour ouvrir l’application et consultez le <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> en action avec cette mise en page de l’exemple.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

#### <a name="implementation"></a>Implémentation

```csharp
/// <summary>
///  This is a custom layout that displays elements in two different sizes
///  wide (w) and narrow (n). There are two types of rows 
///  odd rows - narrow narrow wide
///  even rows - wide narrow narrow
///  This pattern repeats.
/// </summary>

public class ActivityFeedLayout : VirtualizingLayout // STEP #1 Inherit from base attached layout
{
    // STEP #2 - Parameterize the layout
    #region Layout parameters

    // We'll cache copies of the dependency properties to avoid calling GetValue during layout since that
    // can be quite expensive due to the number of times we'd end up calling these.
    private double _rowSpacing;
    private double _colSpacing;
    private Size _minItemSize = Size.Empty;

    /// <summary>
    /// Gets or sets the size of the whitespace gutter to include between rows
    /// </summary>
    public double RowSpacing
    {
        get { return _rowSpacing; }
        set { SetValue(RowSpacingProperty, value); }
    }

    /// <summary>
    /// Gets or sets the size of the whitespace gutter to include between items on the same row
    /// </summary>
    public double ColumnSpacing
    {
        get { return _colSpacing; }
        set { SetValue(ColumnSpacingProperty, value); }
    }

    public Size MinItemSize
    {
        get { return _minItemSize; }
        set { SetValue(MinItemSizeProperty, value); }
    }

    public static readonly DependencyProperty RowSpacingProperty =
        DependencyProperty.Register(
            nameof(RowSpacing),
            typeof(double),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(0, OnPropertyChanged));

    public static readonly DependencyProperty ColumnSpacingProperty =
        DependencyProperty.Register(
            nameof(ColumnSpacing),
            typeof(double),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(0, OnPropertyChanged));

    public static readonly DependencyProperty MinItemSizeProperty =
        DependencyProperty.Register(
            nameof(MinItemSize),
            typeof(Size),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(Size.Empty, OnPropertyChanged));

    private static void OnPropertyChanged(DependencyObject obj, DependencyPropertyChangedEventArgs args)
    {
        var layout = obj as ActivityFeedLayout;
        if (args.Property == RowSpacingProperty)
        {
            layout._rowSpacing = (double)args.NewValue;
        }
        else if (args.Property == ColumnSpacingProperty)
        {
            layout._colSpacing = (double)args.NewValue;
        }
        else if (args.Property == MinItemSizeProperty)
        {
            layout._minItemSize = (Size)args.NewValue;
        }
        else
        {
            throw new InvalidOperationException("Don't know what you are talking about!");
        }

        layout.InvalidateMeasure();
    }

    #endregion

    #region Setup / teardown // STEP #3: Initialize state

    protected override void InitializeForContextCore(VirtualizingLayoutContext context)
    {
        base.InitializeForContextCore(context);

        var state = context.LayoutState as ActivityFeedLayoutState;
        if (state == null)
        {
            // Store any state we might need since (in theory) the layout could be in use by multiple
            // elements simultaneously
            // In reality for the Xbox Activity Feed there's probably only a single instance.
            context.LayoutState = new ActivityFeedLayoutState();
        }
    }

    protected override void UninitializeForContextCore(VirtualizingLayoutContext context)
    {
        base.UninitializeForContextCore(context);

        // clear any state
        context.LayoutState = null;
    }

    #endregion

    #region Layout // STEP #4,5 - Measure and Arrange

    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        if (this.MinItemSize == Size.Empty)
        {
            var firstElement = context.GetOrCreateElementAt(0);
            firstElement.Measure(new Size(double.PositiveInfinity, double.PositiveInfinity));

            // setting the member value directly to skip invalidating layout
            this._minItemSize = firstElement.DesiredSize;
        }

        // Determine which rows need to be realized.  We know every row will have the same height and
        // only contain 3 items.  Use that to determine the index for the first and last item that
        // will be within that realization rect.
        var firstRowIndex = Math.Max(
            (int)(context.RealizationRect.Y / (this.MinItemSize.Height + this.RowSpacing)) - 1,
            0);
        var lastRowIndex = Math.Min(
            (int)(context.RealizationRect.Bottom / (this.MinItemSize.Height + this.RowSpacing)) + 1,
            (int)(context.ItemCount / 3));

        // Determine which items will appear on those rows and what the rect will be for each item
        var state = context.LayoutState as ActivityFeedLayoutState;
        state.LayoutRects.Clear();

        // Save the index of the first realized item.  We'll use it as a starting point during arrange.
        state.FirstRealizedIndex = firstRowIndex * 3;

        // ideal item width that will expand/shrink to fill available space
        double desiredItemWidth = Math.Max(this.MinItemSize.Width, (availableSize.Width - this.ColumnSpacing * 3) / 4);

        // Foreach item between the first and last index,
        //     Call GetElementOrCreateElementAt which causes an element to either be realized or retrieved
        //       from a recycle pool
        //     Measure the element using an appropriate size
        //
        // Any element that was previously realized which we don't retrieve in this pass (via a call to
        // GetElementOrCreateAt) will be automatically cleared and set aside for later re-use.
        // Note: While this work fine, it does mean that more elements than are required may be
        // created because it isn't until after our MeasureOverride completes that the unused elements
        // will be recycled and available to use.  We could avoid this by choosing to track the first/last
        // index from the previous layout pass.  The diff between the previous range and current range
        // would represent the elements that we can pre-emptively make available for re-use by calling
        // context.RecycleElement(element).
        for (int rowIndex = firstRowIndex; rowIndex < lastRowIndex; rowIndex++)
        {
            int firstItemIndex = rowIndex * 3;
            var boundsForCurrentRow = CalculateLayoutBoundsForRow(rowIndex, desiredItemWidth);

            for (int columnIndex = 0; columnIndex < 3; columnIndex++)
            {
                var index = firstItemIndex + columnIndex;
                var rect = boundsForCurrentRow[index % 3];
                var container = context.GetOrCreateElementAt(index);

                container.Measure(
                    new Size(boundsForCurrentRow[columnIndex].Width, boundsForCurrentRow[columnIndex].Height));

                state.LayoutRects.Add(boundsForCurrentRow[columnIndex]);
            }
        }

        // Calculate and return the size of all the content (realized or not) by figuring out
        // what the bottom/right position of the last item would be.
        var extentHeight = ((int)(context.ItemCount / 3) - 1) * (this.MinItemSize.Height + this.RowSpacing) + this.MinItemSize.Height;

        // Report this as the desired size for the layout
        return new Size(desiredItemWidth * 4 + this.ColumnSpacing * 2, extentHeight);
    }

    protected override Size ArrangeOverride(VirtualizingLayoutContext context, Size finalSize)
    {
        // walk through the cache of containers and arrange
        var state = context.LayoutState as ActivityFeedLayoutState;
        var virtualContext = context as VirtualizingLayoutContext;
        int currentIndex = state.FirstRealizedIndex;

        foreach (var arrangeRect in state.LayoutRects)
        {
            var container = virtualContext.GetOrCreateElementAt(currentIndex);
            container.Arrange(arrangeRect);
            currentIndex++;
        }

        return finalSize;
    }

    #endregion
    #region Helper methods

    private Rect[] CalculateLayoutBoundsForRow(int rowIndex, double desiredItemWidth)
    {
        var boundsForRow = new Rect[3];

        var yoffset = rowIndex * (this.MinItemSize.Height + this.RowSpacing);
        boundsForRow[0].Y = boundsForRow[1].Y = boundsForRow[2].Y = yoffset;
        boundsForRow[0].Height = boundsForRow[1].Height = boundsForRow[2].Height = this.MinItemSize.Height;

        if (rowIndex % 2 == 0)
        {
            // Left tile (narrow)
            boundsForRow[0].X = 0;
            boundsForRow[0].Width = desiredItemWidth;
            // Middle tile (narrow)
            boundsForRow[1].X = boundsForRow[0].Right + this.ColumnSpacing;
            boundsForRow[1].Width = desiredItemWidth;
            // Right tile (wide)
            boundsForRow[2].X = boundsForRow[1].Right + this.ColumnSpacing;
            boundsForRow[2].Width = desiredItemWidth * 2 + this.ColumnSpacing;
        }
        else
        {
            // Left tile (wide)
            boundsForRow[0].X = 0;
            boundsForRow[0].Width = (desiredItemWidth * 2 + this.ColumnSpacing);
            // Middle tile (narrow)
            boundsForRow[1].X = boundsForRow[0].Right + this.ColumnSpacing;
            boundsForRow[1].Width = desiredItemWidth;
            // Right tile (narrow)
            boundsForRow[2].X = boundsForRow[1].Right + this.ColumnSpacing;
            boundsForRow[2].Width = desiredItemWidth;
        }

        return boundsForRow;
    }

    #endregion
}

internal class ActivityFeedLayoutState
{
    public int FirstRealizedIndex { get; set; }

    /// <summary>
    /// List of layout bounds for items starting with the
    /// FirstRealizedIndex.
    /// </summary>
    public List<Rect> LayoutRects
    {
        get
        {
            if (_layoutRects == null)
            {
                _layoutRects = new List<Rect>();
            }

            return _layoutRects;
        }
    }

    private List<Rect> _layoutRects;
}
```

### <a name="optional-managing-the-item-to-uielement-mapping"></a>(Facultatif) La gestion de l’élément de mappage de UIElement

Par défaut, le [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) gère un mappage entre les éléments réalisés et de l’index dans la source de données qu’ils représentent.  Une disposition peut choisir de gérer ce mappage lui-même en demandant toujours l’option à [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) lors de la récupération d’un élément via le [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) méthode ce qui empêche la valeur par défaut comportement de recyclage automatique.  Une disposition peut choisir pour ce faire, par exemple, s’il doit uniquement être utilisé lorsque le défilement est limité à une seule direction et les éléments qui que lui sera toujours contiguës (par exemple sachant que l’index de l’élément de premier et dernier est suffisant pour connaître tous les éléments qui doivent être rea lized).

#### <a name="example-xbox-activity-feed-measure"></a>Exemple : Mesure de flux d’activité Xbox

L’extrait de code ci-dessous illustre la logique supplémentaire qui peut être ajoutée à la MeasureOverride, dans l’exemple précédent, pour gérer le mappage.

```csharp
    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        //...

        // Determine which items will appear on those rows and what the rect will be for each item
        var state = context.LayoutState as ActivityFeedLayoutState;
        state.LayoutRects.Clear();

         // Recycle previously realized elements that we know we won't need so that they can be used to
        // fill in gaps without requiring us to realize additional elements.
        var newFirstRealizedIndex = firstRowIndex * 3;
        var newLastRealizedIndex = lastRowIndex * 3 + 3;
        for (int i = state.FirstRealizedIndex; i < newFirstRealizedIndex; i++)
        {
            context.RecycleElement(state.IndexToElementMap.Get(i));
            state.IndexToElementMap.Clear(i);
        }

        for (int i = state.LastRealizedIndex; i < newLastRealizedIndex; i++)
        {
            context.RecycleElement(context.IndexElementMap.Get(i));
            state.IndexToElementMap.Clear(i);
        }

        // ...

        // Foreach item between the first and last index,
        //     Call GetElementOrCreateElementAt which causes an element to either be realized or retrieved
        //       from a recycle pool
        //     Measure the element using an appropriate size
        //
        for (int rowIndex = firstRowIndex; rowIndex < lastRowIndex; rowIndex++)
        {
            int firstItemIndex = rowIndex * 3;
            var boundsForCurrentRow = CalculateLayoutBoundsForRow(rowIndex, desiredItemWidth);

            for (int columnIndex = 0; columnIndex < 3; columnIndex++)
            {
                var index = firstItemIndex + columnIndex;
                var rect = boundsForCurrentRow[index % 3];
                UIElement container = null;
                if (state.IndexToElementMap.Contains(index))
                {
                    container = state.IndexToElementMap.Get(index);
                }
                else
                {
                    container = context = context.GetOrCreateElementAt(index, ElementRealizationOptions.ForceCreate | ElementRealizationOptions.SuppressAutoRecycle);
                    state.IndexToElementMap.Add(index, container);
                }

                container.Measure(
                    new Size(boundsForCurrentRow[columnIndex].Width, boundsForCurrentRow[columnIndex].Height));

                state.LayoutRects.Add(boundsForCurrentRow[columnIndex]);
            }
        }

        // ...
   }

internal class ActivityFeedLayoutState
{
    // ...
    Dictionary<int, UIElement> IndexToElementMap { get; set; }
    // ...
}
```

## <a name="content-dependent-virtualizing-layouts"></a>Dispositions de virtualisation dépendant du contenu

Si vous devez tout d’abord mesurer le contenu de l’interface utilisateur d’un élément pour déterminer sa taille exacte, il se trouve un **disposition du contenu dépendant**.  Vous pouvez également considérer de celui-ci comme une mise en page dans laquelle chaque élément doit dimensionner plutôt que la mise en page indiquant l’élément de sa taille. Dispositions de virtualisation qui appartiennent à cette catégorie sont plus complexe.

> [!NOTE]
> Dispositions de contenu dépendant ne (ne doit pas) de rompre la virtualisation des données.

### <a name="estimations"></a>Estimations

Contenu dépendant des dispositions s’appuient sur l’estimation de deviner la taille du contenu non réalisée et la position du contenu réalisé. Comme ces estimations changent, cela entraînera le contenu réalisé décaler régulièrement des positions dans la zone de défilement. Cela peut entraîner une expérience utilisateur très frustrant et souffrent si n’est pas résolue. Les problèmes potentiels et les atténuations sont présentées ici.

> [!NOTE]
> Les dispositions de données que vous considérez chaque élément et connaître la taille exacte de tous les éléments, ou non et leurs positions peuvent éviter ces problèmes entièrement.

**Ancrage de défilement**

XAML fournit un mécanisme pour atténuer décale vers la fenêtre d’affichage soudaine en demandant le défilement de prise en charge des contrôles [défilement ancrage](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) en implémentant la [IScrollAnchorPovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) interface. Comme l’utilisateur manipule le contenu, le contrôle de défilement sélectionne en permanence un élément dans l’ensemble de candidats qui ont choisi de suivre. Si la position de l’élément d’ancrage équipes pendant la mise en page le contrôle de défilement déplace automatiquement sa fenêtre d’affichage pour maintenir la fenêtre d’affichage.

La valeur de la [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) fourni à la disposition peut refléter cet élément d’ancrage sélectionné actuellement choisi par le contrôle de défilement. Ou bien, si un développeur demande explicitement que de réaliser un élément pour un index avec la [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement) méthode sur le [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), alors cet index est transmis en tant que le [ RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) sur la prochaine phase de disposition. Cela permet la mise en page à préparer pour le scénario probable qu’un développeur se rend compte qu’un élément et demande ensuite qu’il placé dans la vue par le biais de la [StartBringIntoView](/uwp/api/windows.ui.xaml.uielement.startbringintoview) (méthode).

Le [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) est l’index de l’élément dans la source de données une mise en page de contenu dépendant doit positionner tout d’abord lors de l’estimation de la position de ses éléments. Il doit servir de point de départ pour le positionnement d’autres éléments réalisés.

**Impact sur les barres de défilement**

Même avec l’ancrage de défilement, si les estimations de la disposition beaucoup varient, peut-être en raison des variations importantes dans la taille du contenu, puis la position du curseur de défilement pour la barre de défilement peut-être apparaître pour naviguer.  Cela peut être dissonant pour un utilisateur si le curseur n’apparaît pas pour effectuer le suivi de la position du pointeur de la souris lorsqu’ils sont en le faisant glisser.

Le plus précis de la disposition peut être dans ses estimations, puis les moins susceptibles d’un utilisateur seront affiche le curseur de la barre de défilement moment du saut.

### <a name="layout-corrections"></a>Corrections de mise en page

Une disposition de contenu dépendant doit être prête à rationaliser son estimation à la réalité.  Par exemple, comme l’utilisateur fait défiler vers la partie supérieure du contenu et la disposition se rend compte très premier élément, il peut trouver que position prévue de l’élément par rapport à l’élément à partir duquel il est démarré entraînerait pas apparaître l’origine (x : 0 y:0). Lorsque cela se produit, la mise en page peut utiliser le [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) propriété pour définir la position, il est calculé comme l’origine de la nouvelle disposition.  Le résultat net est similaire à faire défiler ancrage dans lequel la fenêtre d’affichage du contrôle de défilement est automatiquement ajustée à compte pour la position du contenu comme indiqué par la mise en page.

![Correction de la LayoutOrigin](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>Fenêtres déconnecté

La taille retournée à partir de la mise en page [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) méthode représente la meilleure hypothèse à la taille du contenu qui peut changer avec chaque disposition successive.  Un utilisateur fait défiler la disposition sera réévaluée en permanence avec une mise à jour [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect).

Si un utilisateur fait glisser le curseur très rapidement son possible pour la fenêtre d’affichage, du point de vue de la disposition, apparaissent pour augmenter la taille d’accède où la position précédente ne chevauche pas la position actuelle.  Cela est dû à la nature asynchrone de défilement. Il est également possible pour une application qui consomme la disposition pour demander qu’un élément placé dans la vue pour un élément qui n’est pas actuellement réalisé et est estimé à présenter en dehors de la plage actuelle suivie par la mise en page.

Lors de la mise en page détecte son estimation est incorrecte et/ou voit un décalage vers la fenêtre d’affichage inattendu, il faut réorienter sa position de départ.  Les dispositions de virtualisation qui font partie des contrôles XAML sont développées en tant que contenu dépendant des dispositions qu’ils placent le moins de restrictions sur la nature du contenu qui s’affichera.


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>Exemple : Disposition de pile de virtualisation simple pour les éléments de taille Variable

L’exemple ci-dessous montre une disposition de pile simple pour éléments de taille variable qui :

* prend en charge la virtualisation de l’interface utilisateur,
* utilise des estimations de deviner la taille des éléments non réalisés,
* tient compte du potentiel discontinues fenêtre d’affichage équipes, et
* applique les corrections de mise en page pour prendre en compte ces équipes.

**Utilisation : Balisage**

```xaml
<ScrollViewer>

  <ItemsRepeater x:Name="repeater" >
    <ItemsRepeater.Layout>

      <local:VirtualizingStackLayout />

    </ItemsRepeater.Layout>
    <ItemsRepeater.ItemTemplate>
      <DataTemplate x:Key="item">
        <UserControl IsTabStop="True" UseSystemFocusVisuals="True" Margin="5">
          <StackPanel BorderThickness="1" Background="LightGray" Margin="5">
            <Image x:Name="recipeImage" Source="{Binding ImageUri}"  Width="100" Height="100"/>
              <TextBlock x:Name="recipeDescription"
                         Text="{Binding Description}"
                         TextWrapping="Wrap"
                         Margin="10" />
          </StackPanel>
        </UserControl>
      </DataTemplate>
    </ItemsRepeater.ItemTemplate>
  </ItemsRepeater>

</ScrollViewer>
```

**Code-behind : Main.cs**

```csharp
string _lorem = @"Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam laoreet erat vel massa rutrum, eget mollis massa vulputate. Vivamus semper augue leo, eget faucibus nulla mattis nec. Donec scelerisque lacus at dui ultricies, eget auctor ipsum placerat. Integer aliquet libero sed nisi eleifend, nec rutrum arcu lacinia. Sed a sem et ante gravida congue sit amet ut augue. Donec quis pellentesque urna, non finibus metus. Proin sed ornare tellus. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam laoreet erat vel massa rutrum, eget mollis massa vulputate. Vivamus semper augue leo, eget faucibus nulla mattis nec. Donec scelerisque lacus at dui ultricies, eget auctor ipsum placerat. Integer aliquet libero sed nisi eleifend, nec rutrum arcu lacinia. Sed a sem et ante gravida congue sit amet ut augue. Donec quis pellentesque urna, non finibus metus. Proin sed ornare tellus.";

var rnd = new Random();
var data = new ObservableCollection<Recipe>(Enumerable.Range(0, 300).Select(k =>
               new Recipe
               {
                   ImageUri = new Uri(string.Format("ms-appx:///Images/recipe{0}.png", k % 8 + 1)),
                   Description = k + " - " + _lorem.Substring(0, rnd.Next(50, 350))
               }));

repeater.ItemsSource = data;
```

**Code : VirtualizingStackLayout.cs**

```csharp
// This is a sample layout that stacks elements one after
// the other where each item can be of variable height. This is
// also a virtualizing layout - we measure and arrange only elements
// that are in the viewport. Not measuring/arranging all elements means
// that we do not have the complete picture and need to estimate sometimes.
// For example the size of the layout (extent) is an estimation based on the
// average heights we have seen so far. Also, if you drag the mouse thumb
// and yank it quickly, then we estimate what goes in the new viewport.

// The layout caches the bounds of everything that are in the current viewport.
// During measure, we might get a suggested anchor (or start index), we use that
// index to start and layout the rest of the items in the viewport relative to that
// index. Note that since we are estimating, we can end up with negative origin when
// the viewport is somewhere in the middle of the extent. This is achieved by setting the
// LayoutOrigin property on the context. Once this is set, future viewport will account
// for the origin.
public class VirtualizingStackLayout : VirtualizingLayout
{
    // Estimation state
    List<double> m_estimationBuffer = Enumerable.Repeat(0d, 100).ToList();
    int m_numItemsUsedForEstimation = 0;
    double m_totalHeightForEstimation = 0;

    // State to keep track of realized bounds
    int m_firstRealizedDataIndex = 0;
    List<Rect> m_realizedElementBounds = new List<Rect>();

    Rect m_lastExtent = new Rect();

    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        var viewport = context.RealizationRect;
        DebugTrace("MeasureOverride: Viewport " + viewport);

        // Remove bounds for elements that are now outside the viewport.
        // Proactive recycling elements means we can reuse it during this measure pass again.
        RemoveCachedBoundsOutsideViewport(viewport);

        // Find the index of the element to start laying out from - the anchor
        int startIndex = GetStartIndex(context, availableSize);

        // Measure and layout elements starting from the start index, forward and backward.
        Generate(context, availableSize, startIndex, forward:true);
        Generate(context, availableSize, startIndex, forward:false);

        // Estimate the extent size. Note that this can have a non 0 origin.
        m_lastExtent = EstimateExtent(context, availableSize);
        context.LayoutOrigin = new Point(m_lastExtent.X, m_lastExtent.Y);
        return new Size(m_lastExtent.Width, m_lastExtent.Height);
    }

    protected override Size ArrangeOverride(VirtualizingLayoutContext context, Size finalSize)
    {
        DebugTrace("ArrangeOverride: Viewport" + context.RealizationRect);
        for (int realizationIndex = 0; realizationIndex < m_realizedElementBounds.Count; realizationIndex++)
        {
            int currentDataIndex = m_firstRealizedDataIndex + realizationIndex;
            DebugTrace("Arranging " + currentDataIndex);

            // Arrange the child. If any alignment needs to be done, it
            // can be done here.
            var child = context.GetOrCreateElementAt(currentDataIndex);
            var arrangeBounds = m_realizedElementBounds[realizationIndex];
            arrangeBounds.X -= m_lastExtent.X;
            arrangeBounds.Y -= m_lastExtent.Y;
            child.Arrange(arrangeBounds);
        }

        return finalSize;
    }

    // The data collection has changed, since we are maintaining the bounds of elements
    // in the viewport, we will update the list to account for the collection change.
    protected override void OnItemsChangedCore(VirtualizingLayoutContext context, object source, NotifyCollectionChangedEventArgs args)
    {
        InvalidateMeasure();
        if (m_realizedElementBounds.Count > 0)
        {
            switch (args.Action)
            {
                case NotifyCollectionChangedAction.Add:
                    OnItemsAdded(args.NewStartingIndex, args.NewItems.Count);
                    break;
                case NotifyCollectionChangedAction.Replace:
                    OnItemsRemoved(args.OldStartingIndex, args.OldItems.Count);
                    OnItemsAdded(args.NewStartingIndex, args.NewItems.Count);
                    break;
                case NotifyCollectionChangedAction.Remove:
                    OnItemsRemoved(args.OldStartingIndex, args.OldItems.Count);
                    break;
                case NotifyCollectionChangedAction.Reset:
                    m_realizedElementBounds.Clear();
                    m_firstRealizedDataIndex = 0;
                    break;
                default:
                    throw new NotImplementedException();
            }
        }
    }

    // Figure out which index to use as the anchor and start laying out around it.
    private int GetStartIndex(VirtualizingLayoutContext context, Size availableSize)
    {
        int startDataIndex = -1;
        var recommendedAnchorIndex = context.RecommendedAnchorIndex;
        bool isSuggestedAnchorValid = recommendedAnchorIndex != -1;

        if (isSuggestedAnchorValid)
        {
            if (IsRealized(recommendedAnchorIndex))
            {
                startDataIndex = recommendedAnchorIndex;
            }
            else
            {
                ClearRealizedRange();
                startDataIndex = recommendedAnchorIndex;
            }
        }
        else
        {
            // Find the first realized element that is visible in the viewport.
            startDataIndex = GetFirstRealizedDataIndexInViewport(context.RealizationRect);
            if (startDataIndex < 0)
            {
                startDataIndex = EstimateIndexForViewport(context.RealizationRect, context.ItemCount);
                ClearRealizedRange();
            }
        }

        // We have an anchorIndex, realize and measure it and
        // figure out its bounds.
        if (startDataIndex != -1 & context.ItemCount > 0)
        {
            if (m_realizedElementBounds.Count == 0)
            {
                m_firstRealizedDataIndex = startDataIndex;
            }

            var newAnchor = EnsureRealized(startDataIndex);
            DebugTrace("Measuring start index " + startDataIndex);
            var desiredSize = MeasureElement(context, startDataIndex, availableSize);

            var bounds = new Rect(
                0,
                newAnchor ?
                    (m_totalHeightForEstimation / m_numItemsUsedForEstimation) * startDataIndex : GetCachedBoundsForDataIndex(startDataIndex).Y,
                availableSize.Width,
                desiredSize.Height);
            SetCachedBoundsForDataIndex(startDataIndex, bounds);
        }

        return startDataIndex;
    }


    private void Generate(VirtualizingLayoutContext context, Size availableSize, int anchorDataIndex, bool forward)
    {
        // Generate forward or backward from anchorIndex until we hit the end of the viewport
        int step = forward ? 1 : -1;
        int previousDataIndex = anchorDataIndex;
        int currentDataIndex = previousDataIndex + step;
        var viewport = context.RealizationRect;
        while (IsDataIndexValid(currentDataIndex, context.ItemCount) &&
            ShouldContinueFillingUpSpace(previousDataIndex, forward, viewport))
        {
            EnsureRealized(currentDataIndex);
            DebugTrace("Measuring " + currentDataIndex);
            var desiredSize = MeasureElement(context, currentDataIndex, availableSize);
            var previousBounds = GetCachedBoundsForDataIndex(previousDataIndex);
            Rect currentBounds = new Rect(0,
                                          forward ? previousBounds.Y + previousBounds.Height : previousBounds.Y - desiredSize.Height,
                                          availableSize.Width,
                                          desiredSize.Height);
            SetCachedBoundsForDataIndex(currentDataIndex, currentBounds);
            previousDataIndex = currentDataIndex;
            currentDataIndex += step;
        }
    }

    // Remove bounds that are outside the viewport, leaving one extra since our
    // generate stops after generating one extra to know that we are outside the
    // viewport.
    private void RemoveCachedBoundsOutsideViewport(Rect viewport)
    {
        int firstRealizedIndexInViewport = 0;
        while (firstRealizedIndexInViewport < m_realizedElementBounds.Count &&
               !Intersects(m_realizedElementBounds[firstRealizedIndexInViewport], viewport))
        {
            firstRealizedIndexInViewport++;
        }

        int lastRealizedIndexInViewport = m_realizedElementBounds.Count - 1;
        while (lastRealizedIndexInViewport >= 0 &&
            !Intersects(m_realizedElementBounds[lastRealizedIndexInViewport], viewport))
        {
            lastRealizedIndexInViewport--;
        }

        if (firstRealizedIndexInViewport > 0)
        {
            m_firstRealizedDataIndex += firstRealizedIndexInViewport;
            m_realizedElementBounds.RemoveRange(0, firstRealizedIndexInViewport);
        }

        if (lastRealizedIndexInViewport >= 0 && lastRealizedIndexInViewport < m_realizedElementBounds.Count - 2)
        {
            m_realizedElementBounds.RemoveRange(lastRealizedIndexInViewport + 2, m_realizedElementBounds.Count - lastRealizedIndexInViewport - 3);
        }
    }

    private bool Intersects(Rect bounds, Rect viewport)
    {
        return !(bounds.Bottom < viewport.Top ||
            bounds.Top > viewport.Bottom);
    }

    private bool ShouldContinueFillingUpSpace(int dataIndex, bool forward, Rect viewport)
    {
        var bounds = GetCachedBoundsForDataIndex(dataIndex);
        return forward ?
            bounds.Y < viewport.Bottom :
            bounds.Y > viewport.Top;
    }

    private bool IsDataIndexValid(int currentDataIndex, int itemCount)
    {
        return currentDataIndex >= 0 && currentDataIndex < itemCount;
    }

    private int EstimateIndexForViewport(Rect viewport, int dataCount)
    {
        double averageHeight = m_totalHeightForEstimation / m_numItemsUsedForEstimation;
        int estimatedIndex = (int)(viewport.Top / averageHeight);
        // clamp to an index within the collection
        estimatedIndex = Math.Max(0, Math.Min(estimatedIndex, dataCount));
        return estimatedIndex;
    }

    private int GetFirstRealizedDataIndexInViewport(Rect viewport)
    {
        int index = -1;
        if (m_realizedElementBounds.Count > 0)
        {
            for (int i = 0; i < m_realizedElementBounds.Count; i++)
            {
                if (m_realizedElementBounds[i].Y < viewport.Bottom &&
                   m_realizedElementBounds[i].Bottom > viewport.Top)
                {
                    index = m_firstRealizedDataIndex + i;
                    break;
                }
            }
        }

        return index;
    }

    private Size MeasureElement(VirtualizingLayoutContext context, int index, Size availableSize)
    {
        var child = context.GetOrCreateElementAt(index);
        child.Measure(availableSize);

        int estimationBufferIndex = index % m_estimationBuffer.Count;
        bool alreadyMeasured = m_estimationBuffer[estimationBufferIndex] != 0;
        if (!alreadyMeasured)
        {
            m_numItemsUsedForEstimation++;
        }

        m_totalHeightForEstimation -= m_estimationBuffer[estimationBufferIndex];
        m_totalHeightForEstimation += child.DesiredSize.Height;
        m_estimationBuffer[estimationBufferIndex] = child.DesiredSize.Height;

        return child.DesiredSize;
    }

    private bool EnsureRealized(int dataIndex)
    {
        if (!IsRealized(dataIndex))
        {
            int realizationIndex = RealizationIndex(dataIndex);
            Debug.Assert(dataIndex == m_firstRealizedDataIndex - 1 ||
                dataIndex == m_firstRealizedDataIndex + m_realizedElementBounds.Count ||
                m_realizedElementBounds.Count == 0);

            if (realizationIndex == -1)
            {
                m_realizedElementBounds.Insert(0, new Rect());
            }
            else
            {
                m_realizedElementBounds.Add(new Rect());
            }

            if (m_firstRealizedDataIndex > dataIndex)
            {
                m_firstRealizedDataIndex = dataIndex;
            }

            return true;
        }

        return false;
    }

    // Figure out the extent of the layout by getting the number of items remaining
    // above and below the realized elements and getting an estimation based on
    // average item heights seen so far.
    private Rect EstimateExtent(VirtualizingLayoutContext context, Size availableSize)
    {
        double averageHeight = m_totalHeightForEstimation / m_numItemsUsedForEstimation;

        Rect extent = new Rect(0, 0, availableSize.Width, context.ItemCount * averageHeight);

        if (context.ItemCount > 0 && m_realizedElementBounds.Count > 0)
        {
            extent.Y = m_firstRealizedDataIndex == 0 ?
                            m_realizedElementBounds[0].Y :
                            m_realizedElementBounds[0].Y - (m_firstRealizedDataIndex - 1) * averageHeight;

            int lastRealizedIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count;
            if (lastRealizedIndex == context.ItemCount - 1)
            {
                var lastBounds = m_realizedElementBounds[m_realizedElementBounds.Count - 1];
                extent.Y = lastBounds.Bottom;
            }
            else
            {
                var lastBounds = m_realizedElementBounds[m_realizedElementBounds.Count - 1];
                int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count;
                int numItemsAfterLastRealizedIndex = context.ItemCount - lastRealizedDataIndex;
                extent.Height = lastBounds.Bottom + numItemsAfterLastRealizedIndex * averageHeight - extent.Y;
            }
        }

        DebugTrace("Extent " + extent + " with average height " + averageHeight);
        return extent;
    }

    private bool IsRealized(int dataIndex)
    {
        int realizationIndex = dataIndex - m_firstRealizedDataIndex;
        return realizationIndex >= 0 && realizationIndex < m_realizedElementBounds.Count;
    }

    // Index in the m_realizedElementBounds collection
    private int RealizationIndex(int dataIndex)
    {
        return dataIndex - m_firstRealizedDataIndex;
    }

    private void OnItemsAdded(int index, int count)
    {
        // Using the old indexes here (before it was updated by the collection change)
        // if the insert data index is between the first and last realized data index, we need
        // to insert items.
        int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count - 1;
        int newStartingIndex = index;
        if (newStartingIndex > m_firstRealizedDataIndex &&
            newStartingIndex <= lastRealizedDataIndex)
        {
            // Inserted within the realized range
            int insertRangeStartIndex = newStartingIndex - m_firstRealizedDataIndex;
            for (int i = 0; i < count; i++)
            {
                // Insert null (sentinel) here instead of an element, that way we do not
                // end up creating a lot of elements only to be thrown out in the next layout.
                int insertRangeIndex = insertRangeStartIndex + i;
                int dataIndex = newStartingIndex + i;
                // This is to keep the contiguousness of the mapping
                m_realizedElementBounds.Insert(insertRangeIndex, new Rect());
            }
        }
        else if (index <= m_firstRealizedDataIndex)
        {
            // Items were inserted before the realized range.
            // We need to update m_firstRealizedDataIndex;
            m_firstRealizedDataIndex += count;
        }
    }

    private void OnItemsRemoved(int index, int count)
    {
        int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count - 1;
        int startIndex = Math.Max(m_firstRealizedDataIndex, index);
        int endIndex = Math.Min(lastRealizedDataIndex, index + count - 1);
        bool removeAffectsFirstRealizedDataIndex = (index <= m_firstRealizedDataIndex);

        if (endIndex >= startIndex)
        {
            ClearRealizedRange(RealizationIndex(startIndex), endIndex - startIndex + 1);
        }

        if (removeAffectsFirstRealizedDataIndex &&
            m_firstRealizedDataIndex != -1)
        {
            m_firstRealizedDataIndex -= count;
        }
    }

    private void ClearRealizedRange(int startRealizedIndex, int count)
    {
        m_realizedElementBounds.RemoveRange(startRealizedIndex, count);
        if (startRealizedIndex == 0)
        {
            m_firstRealizedDataIndex = m_realizedElementBounds.Count == 0 ? 0 : m_firstRealizedDataIndex + count;
        }
    }

    private void ClearRealizedRange()
    {
        m_realizedElementBounds.Clear();
        m_firstRealizedDataIndex = 0;
    }

    private Rect GetCachedBoundsForDataIndex(int dataIndex)
    {
        return m_realizedElementBounds[RealizationIndex(dataIndex)];
    }

    private void SetCachedBoundsForDataIndex(int dataIndex, Rect bounds)
    {
        m_realizedElementBounds[RealizationIndex(dataIndex)] = bounds;
    }

    private Rect GetCachedBoundsForRealizationIndex(int relativeIndex)
    {
        return m_realizedElementBounds[relativeIndex];
    }

    void DebugTrace(string message, params object[] args)
    {
        Debug.WriteLine(message, args);
    }
}
```

## <a name="related-articles"></a>Articles connexes

- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
