---
description: La pratique de définition de l’interface utilisateur sous la forme de balisage XAML déclaratif convertit extrêmement bien entre WindowsPhone Silverlight aux applications de plateforme Windows universelle (UWP).
title: Portage d’interface vers UWP et WindowsPhone Silverlight XAML
ms.assetid: 49aade74-5dc6-46a5-89ef-316dbeabbebe
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 153d73a75b48d61cb490a903c6657c42638c6674
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8326211"
---
#  <a name="porting-windowsphone-silverlight-xaml-and-ui-to-uwp"></a>Portage d’interface vers UWP et WindowsPhone Silverlight XAML



Rubrique précédente : [Résolution des problèmes](wpsl-to-uwp-troubleshooting.md).

La pratique de définition de l’interface utilisateur sous la forme de balisage XAML déclaratif convertit extrêmement bien entre WindowsPhone Silverlight aux applications de plateforme Windows universelle (UWP). Vous allez découvrir que des sections importantes de votre balisage sont compatibles une fois que vous avez mis à jour les références de clés des ressources système, modifié certains noms de type d’élément et remplacé «clr-namespace» par «using». Une grande partie du code impératif de votre couche présentation (modèles d’affichage et code qui manipule les éléments d’interface utilisateur) peut également être portée directement.

## <a name="a-first-look-at-the-xaml-markup"></a>Découverte du balisage XAML

La rubrique précédente vous a indiqué comment copier vos XAML et code-behind fichiers dans votre nouveau projet Windows 10 Visual Studio. L’un des premiers problèmes que vous remarquerez peut-être dans le concepteur XAML de Visual Studio est que l’élément `PhoneApplicationPage` figurant à la racine de votre fichier XAML n’est pas valide pour un projet de plateforme Windows universelle (UWP). Dans la rubrique précédente, vous avez enregistré une copie des fichiers XAML générés par Visual Studio lors de la création du projet Windows 10. Si vous ouvrez cette version de MainPage.xaml, vous verrez que la racine est le type [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503), qui se trouve dans l’espace de noms [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716). Vous pouvez donc remplacer tous les éléments `<phone:PhoneApplicationPage>` par `<Page>` (n’oubliez pas la syntaxe des éléments de propriété) et supprimer la déclaration `xmlns:phone`.

Pour une approche plus générale à trouver le type UWP correspondant à un type WindowsPhone Silverlight, vous pouvez faire référence à des [mappages Namespace et des classes](wpsl-to-uwp-namespace-and-class-mappings.md).

## <a name="xaml-namespace-prefix-declarations"></a>Déclarations de préfixe d’espace de noms XAML


Si vous utilisez des instances de types personnalisés dans vos affichages (par exemple, une instance de modèle d’affichage ou un convertisseur de valeurs), vous aurez des déclarations de préfixe d’espace de noms XAML dans votre balisage XAML. La syntaxe diffère entre WindowsPhone Silverlight et UWP. En voici quelques exemples :

```xml
    xmlns:ContosoTradingCore="clr-namespace:ContosoTradingCore;assembly=ContosoTradingCore"
    xmlns:ContosoTradingLocal="clr-namespace:ContosoTradingLocal"
```

Remplacez « clr-namespace » par « using » et supprimez le jeton d’assembly et le point-virgule (l’assembly sera déduit). Le résultat ressemble à ceci :

```xml
    xmlns:ContosoTradingCore="using:ContosoTradingCore"
    xmlns:ContosoTradingLocal="using:ContosoTradingLocal"
```

Vous pouvez avoir une ressource dont le type est défini par le système :

```xml
    xmlns:System="clr-namespace:System;assembly=mscorlib"
    /* ... */
    <System:Double x:Key="FontSizeLarge">40</System:Double>
```

Dans UWP, omettez la déclaration de préfixe « System » et utilisez à la place le préfixe « x » (déjà utilisé) :

```xml
    <x:Double x:Key="FontSizeLarge">40</x:Double>
```

## <a name="imperative-code"></a>Code impératif


Vos modèles d’affichage sont un emplacement où le code impératif référence des types d’interface utilisateur. Un autre emplacement correspond aux fichiers code-behind qui manipulent directement des éléments d’interface utilisateur. Par exemple, vous pouvez constater qu’une ligne de code semblable à celle-ci n’est pas encore compilée:


```csharp
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

**BitmapImage** se trouve dans l’espace de noms **System.Windows.Media.Imaging** dans WindowsPhone Silverlight et un à l’aide de la directive dans le même fichier permet **BitmapImage** sans qualification d’espace de noms comme dans l’extrait de code ci-dessus. Dans ce cas, vous pouvez cliquer avec le bouton droit sur le nom de type (**BitmapImage**) dans Visual Studio et utiliser la commande **Résoudre** du menu contextuel pour ajouter une nouvelle directive d’espace de noms au fichier. Dans ce cas, l’espace de noms [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258) est ajouté et correspond à l’emplacement du type dans UWP. Vous pouvez supprimer la directive using **System.Windows.Media.Imaging**, et c’est tout ce que vous aurez à faire pour porter du code semblable à celui de l’extrait ci-dessus. Lorsque vous avez terminé, vous aurez supprimé tous les espaces de noms WindowsPhone Silverlight.

Dans des cas simples comme celui-ci, lorsque vous mappez les types d’un ancien espace de noms sur les mêmes types d’un nouvel espace de noms, vous pouvez utiliser la commande **Rechercher et remplacer** de Visual Studio pour apporter des modifications en bloc à votre code source. La commande **Résoudre** est un excellent moyen de découvrir le nouvel espace de noms d’un type. À titre d’exemple, vous pouvez remplacer toutes les instances «System.Windows» par «Windows.UI.Xaml». Cela portera essentiellement toutes les directives using et tous les noms de type complet qui font référence à cet espace de noms.

Une fois toutes les anciennes directives using supprimées et les nouvelles ajoutées, vous pouvez utiliser la commande **Organiser les instructions Using** de Visual Studio pour trier vos directives et supprimer celles qui ne sont pas utilisées.

La correction du code impératif est parfois aussi mineure que la modification d’un type de paramètre. Dans d’autres cas, vous devez utiliser les API UWP au lieu d’API .NET pour Windows Runtime 8.x applications. Pour identifier les API est prises en charge, utilisez le reste de ce guide de portage en combinaison avec [vue d’ensemble des applications .NET pour Windows Runtime 8.x](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx) et la [référence de Windows Runtime](https://msdn.microsoft.com/library/windows/apps/br211377).

Et si vous souhaitez simplement accéder à l’étape de construction de votre projet, vous pouvez commenter ou remplacer tout code non essentiel. Vous pouvez ensuite itérer un problème à la fois et consulter les rubriques suivantes de cette section (ainsi que la rubrique précédente : [Résolution des problèmes](wpsl-to-uwp-troubleshooting.md)), jusqu’à ce que les problèmes de génération et d’exécution soient supprimés et le portage terminé.

## <a name="adaptiveresponsive-ui"></a>Interface utilisateur adaptative/réactive

Dans la mesure où votre application Windows 10 peut s’exécuter sur une large gamme d’appareils, chacun avec son propre taille d’écran et la résolution, vous pouvez compléter la procédure minimale de portage de votre application en vous souhaiterez adapter votre interface utilisateur afin d’en optimiser l’aspect sur ces appareils. Vous pouvez utiliser la fonctionnalité adaptative Gestionnaire d’état visuel pour détecter dynamiquement la taille de la fenêtre et modifier la disposition en conséquence. Un exemple de procédure à suivre est décrit à la section [Interface utilisateur adaptative](wpsl-to-uwp-case-study-bookstore2.md) de la rubrique d’étude de cas Bookstore2.

## <a name="alarms-and-reminders"></a>Alarmes et rappels

Le code utilisant les classes **Alarm** ou **Reminder** doit être porté pour utiliser la classe [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) afin de créer et d’inscrire une tâche en arrière-plan et d’afficher un toast au moment approprié. Voir [Traitement en arrière-plan](wpsl-to-uwp-business-and-data.md) et [Toasts](#toasts).

## <a name="animation"></a>Animation

Solution de substitution préférée aux animations d’image clé et aux animations from/to, la bibliothèque d’animations UWP est disponible pour les applications UWP. Ces animations ont été conçues et ajustées pour que leur exécution soit parfaite et leur apparence remarquable, et pour que votre application soit aussi intégrée à Windows que les applications dites intégrées. Voir l’article [Démarrage rapide : animation de votre interface utilisateur avec des animations de la bibliothèque](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703).

Si vous utilisez des animations d’image clé ou des animations from/to dans vos applications UWP, vous souhaiterez peut-être comprendre la distinction entre les animations dépendantes et indépendantes introduites par la nouvelle plateforme. Voir [Optimiser les animations et le contenu multimédia](https://msdn.microsoft.com/library/windows/apps/mt204774). Les animations qui s’exécutent sur le thread d’interface utilisateur (celles qui animent les propriétés de disposition par exemple) sont appelées animations dépendantes. Si elles s’exécutent sur la nouvelle plateforme, elles n’ont aucun effet à moins que vous n’effectuiez l’une des deux opérations suivantes. Vous pouvez les recibler pour animer différentes propriétés telles que [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/br208980), les rendant alors indépendantes. Vous pouvez également définir `EnableDependentAnimation="True"` dans l’élément d’animation afin de confirmer votre intention d’exécuter une animation dont l’exécution parfaite ne peut pas être garantie. Si vous utilisez Blend pour VisualStudio pour créer des animations, cette propriété sera définie pour vous si nécessaire.

## <a name="back-button-handling"></a>Gestion du bouton Précédent

Dans une application Windows 10, vous pouvez utiliser une approche unique pour la gestion du bouton précédent, et il fonctionne sur tous les appareils. Sur les appareils mobiles, le bouton est fourni à votre intention sous la forme d’un bouton capacitif sur l’appareil ou d’un bouton dans l’interpréteur de commandes. Sur un appareil de bureau, vous ajoutez un bouton au chrome de votre application chaque fois que cette dernière permet la navigation vers l’arrière. Ceci est indiqué dans la barre de titre des applications avec fenêtres ou dans la barre des tâches en mode tablette. L’événement de bouton Précédent est un concept universel sur toutes les familles d’appareils, et les boutons implémentés dans le matériel ou dans le logiciel déclenchent le même événement [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596).

L’exemple ci-après fonctionne pour toutes les familles d’appareils et est adapté aux cas dans lesquels le même traitement s’applique à toutes les pages et où vous n’avez pas besoin de confirmer la navigation (par exemple, pour signaler les modifications non enregistrées).

```csharp
   // app.xaml.cs

    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        [...]

        Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
        rootFrame.Navigated += RootFrame_Navigated;
    }

    private void RootFrame_Navigated(object sender, NavigationEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        // Note: On device families that have no title bar, setting AppViewBackButtonVisibility can safely execute 
        // but it will have no effect. Such device families provide a back button UI for you.
        if (rootFrame.CanGoBack)
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Visible;
        }
        else
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Collapsed;
        }
    }

    private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        if (rootFrame.CanGoBack)
        {
            rootFrame.GoBack();
        }
    }
```

Il existe également une approche unique pour toutes les familles d’appareils concernant la fermeture de l’application par programme.

```csharp
   Windows.UI.Xaml.Application.Current.Exit();
```

## <a name="binding-and-compiled-bindings-with-xbind"></a>Liaison et liaisons compilées avec {x: Bind}

La rubrique relative à la liaison inclut les aspects suivants :

-   Liaison d’un élément d’interface utilisateur aux « données » (autrement dit, aux propriétés et aux commandes d’un modèle d’affichage)
-   Liaison d’un élément d’interface utilisateur à un autre élément d’interface utilisateur
-   Écriture d’un modèle d’affichage observable (autrement dit, il déclenche des notifications en cas de modification d’une valeur de propriété et de la disponibilité d’une commande)

Tous ces aspects restent majoritairement pris en charge, mais il existe des différences relatives aux espaces de noms. Par exemple, **System.Windows.Data.Binding** correspond à [**Windows.UI.Xaml.Data.Binding**](https://msdn.microsoft.com/library/windows/apps/br209820), **System.ComponentModel.INotifyPropertyChanged** correspond à [**Windows.UI.Xaml.Data.INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/br209899) et **System.Collections.Specialized.INotifyPropertyChanged** correspond à [**Windows.UI.Xaml.Interop.INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/hh702001).

Barres de l’application WindowsPhone Silverlight et les boutons de barre d’application ne peut pas être liées comme ils le feraient dans une application UWP. Vous pouvez avoir du code impératif qui construit votre barre de l’application et ses boutons, les lie aux propriétés et aux chaînes localisées et gère leurs événements. Le cas échéant, vous pouvez maintenant porter ce code impératif en le remplaçant par un balisage déclaratif lié aux propriétés et aux commandes, avec des références de ressources statiques, ce qui renforce la sécurité et la maintenabilité de votre application de façon incrémentielle. Vous pouvez utiliser Visual Studio ou Blend pour Visual Studio pour lier et styliser les boutons de la barre de l’application UWP comme tout autre élément XAML. Notez que dans une application UWP, les noms de type que vous utilisez sont [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/dn279427) et [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244).

Les fonctionnalités associées aux liaisons des applications UWP présentent actuellement les limitations suivantes :

-   Il n’existe aucune prise en charge intégrée pour la validation de l’entrée des données et les interfaces [**IDataErrorInfo**](https://msdn.microsoft.com/library/system.componentmodel.idataerrorinfo.aspx) et [**INotifyDataErrorInfo**](https://msdn.microsoft.com/library/system.componentmodel.inotifydataerrorinfo.aspx).
-   La classe [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) n’inclut pas les propriétés de mise en forme étendues disponibles dans WindowsPhone Silverlight. Toutefois, vous pouvez implémenter [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903) pour produire une mise en forme personnalisée.
-   Les méthodes [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903) acceptent des chaînes de langage comme paramètres au lieu d’objets [**CultureInfo**](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx).
-   La classe [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/br209833) ne fournit pas de prise en charge intégrée pour le tri et le filtrage, et le regroupement fonctionne différemment. Pour plus d’informations, voir [Présentation détaillée de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt210946) et [Exemple de liaison de données](http://go.microsoft.com/fwlink/p/?linkid=226854).

Bien que les mêmes fonctionnalités de liaison sont toujours majoritairement prises en charge, Windows 10 offre la possibilité d’une nouvelle et mécanisme appelé de liaison plus performant liaisons compilées, qui utilisent l’extension de balisage {x: Bind}. Voir [Liaison de données : accroître les performances de votre application grâce aux nouvelles améliorations de la liaison de données XAML](http://channel9.msdn.com/Events/Build/2015/3-635) et [Exemple x:Bind](http://go.microsoft.com/fwlink/p/?linkid=619989) (en anglais).

## <a name="binding-an-image-to-a-view-model"></a>Liaison d’une propriété Image à un modèle d’affichage

Vous pouvez lier la propriété [**Image.Source**](https://msdn.microsoft.com/library/windows/apps/br242760) à toute propriété d’un modèle d’affichage de type [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/br210107). Voici une implémentation standard d’une telle propriété dans une application WindowsPhone Silverlight:

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

Dans une application UWP, vous utilisez le [schéma d’URI](https://msdn.microsoft.com/library/windows/apps/jj655406) ms-appx. Pour pouvoir conserver le reste de votre code identique, vous pouvez utiliser une surcharge différente du constructeur **System.Uri** pour placer le schéma d’URI ms-appx dans un URI de base et y ajouter le reste du chemin d’accès. Comme ceci:

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(new Uri("ms-appx://"), this.CoverImagePath));
```

De cette façon, le reste du modèle d’affichage, les valeurs de chemin d’accès dans la propriété de chemin d’accès à l’image et les liaisons dans le balisage XAML peuvent tous rester identiques.

## <a name="controls-and-control-stylestemplates"></a>Contrôles et styles/modèles de contrôle

Les applications WindowsPhone Silverlight utilisent les contrôles définis dans l’espace de noms **Microsoft.Phone.Controls** et l’espace de noms **System.Windows.Controls** . Les applications UWP XAML utilisent les contrôles définis dans l’espace de noms [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716). L’architecture et la conception des contrôles XAML dans UWP sont pratiquement le même que les contrôles de WindowsPhone Silverlight. Toutefois, certaines modifications ont été apportées pour améliorer l’ensemble des contrôles disponibles et les unifier avec les applications Windows. En voici des exemples spécifiques.

| Nom du contrôle | Modification |
|--------------|--------|
| ApplicationBar | Propriété [Page.TopAppBar](https://msdn.microsoft.com/library/windows/apps/hh702575). |
| ApplicationBarIconButton | L’équivalent UWP est la propriété [Glyph](https://msdn.microsoft.com/library/windows/apps/dn279538). PrimaryCommands est la propriété de contenu du contrôle CommandBar. L’analyseurXAML interprète le code xml interne d’un élément comme la valeur de sa propriété de contenu. |
| ApplicationBarMenuItem | L’équivalent UWP est la propriété [AppBarButton.Label](https://msdn.microsoft.com/library/windows/apps/dn279261) définie sur le texte de l’élément de menu. |
| ContextMenu (dans le kit de ressources pour Windows Phone) | Dans le cas d’un menu volant à sélection unique, utilisez [Flyout](https://msdn.microsoft.com/library/windows/apps/dn279496). |
| ControlTiltEffect.TiltEffect class | Des animations de la bibliothèque d’animations UWP sont intégrées aux styles par défaut des contrôles courants. Voir [Animation des actions de pointeur](https://msdn.microsoft.com/library/windows/apps/xaml/jj649432). |
| LongListSelector avec des données groupées | Les fonctions LongListSelector de Silverlight WindowsPhone de deux façons, qui peuvent être utilisées conjointement. Tout d’abord, il peut afficher les données groupées en fonction d’une clé, par exemple la liste des noms groupés en fonction de leur lettre initiale. Ensuite, il peut «zoomer» entre deux affichages sémantiques: la liste groupée des éléments (par exemple, les noms) et la liste des clés de groupe proprement dites (par exemple, les lettres initiales). Avec la plateforme UWP, vous pouvez afficher les données groupées à l’aide des contrôles ListView et GridView (voir les [Recommandations en matière d’affichage Liste et Grille](https://msdn.microsoft.com/library/windows/apps/mt186889)). |
| LongListSelector avec des données mises à plat | Pour des raisons de performances, dans le cas de très longues listes, nous vous recommandons de LongListSelector au lieu d’un WindowsPhone Silverlight zone de liste même pour les données non groupées mises à plat. Dans une application UWP, les contrôles [GridView](https://msdn.microsoft.com/library/windows/apps/br242705) sont privilégiés pour les longues listes d’éléments, que les données soient ou non prêtes pour le regroupement. |
| Panorama | Le contrôle Panorama de Silverlight WindowsPhone mappe vers les [recommandations en matière de contrôles hub dans les applications Windows Runtime 8.x](https://msdn.microsoft.com/library/windows/apps/dn449149) et les instructions pour le contrôle hub. <br/> Notez qu’un contrôle Panorama exécute une boucle entre la dernière section et la première, et que son image d’arrière-plan se déplace en parallaxe par rapport aux sections. Les sections de [Hub](https://msdn.microsoft.com/library/windows/apps/dn251843) n’exécutent aucune boucle, et l’effet parallaxe n’est pas utilisé. |
| Pivot | L’équivalent UWP du contrôle Pivot de Silverlight WindowsPhone est [Windows.UI.Xaml.Controls.Pivot](https://msdn.microsoft.com/library/windows/apps/dn608241). Il est disponible pour toutes les familles d’appareils. |

**Remarque**  l’état visuel PointerOver est adapté styles/modèles personnalisés dans les applications Windows 10, mais non dans les applications WindowsPhone Silverlight. Il existe autres raisons pour lesquelles vos styles/modèles personnalisés existants peuvent ne pas appropriés pour les applications Windows 10, y compris les clés de ressources système vous utilisez, les modifications apportées aux jeux d’états visuels utilisés et des améliorations de performances apportées aux styles par défaut Windows 10 / modèles. Nous vous recommandons modifiez une nouvelle copie d’un modèle de contrôle par défaut pour Windows 10 et réappliquez votre personnalisation de style et le modèle de.

Pour plus d’informations sur les contrôles UWP, voir [Contrôles par fonction](https://msdn.microsoft.com/library/windows/apps/mt185405), [Liste des contrôles](https://msdn.microsoft.com/library/windows/apps/mt185406) et [Recommandations relatives aux contrôles](https://msdn.microsoft.com/library/windows/apps/dn611856).

##  <a name="design-language-in-windows10"></a>Langage de conception dans Windows 10

Il existe certaines différences de langage de conception entre les applications WindowsPhone Silverlight et les applications Windows 10. Pour plus de détails, voir [Conception](http://dev.windows.com/design). Malgré les changements en matière de langage, nos principes de conception restent cohérents : être attentif aux détails, mais toujours viser la simplicité en se concentrant sur le contenu sans superflu, en réduisant à tout prix les éléments visuels et en restant authentique en matière de domaine numérique ; utiliser la hiérarchie visuelle, en particulier avec la typographie ; concevoir à l’aide d’une grille et donner vie à vos expériences grâce à des animations fluides.

## <a name="localization-and-globalization"></a>Localisation et globalisation

Pour les chaînes localisées, vous pouvez réutiliser le fichier .resx de votre projet WindowsPhone Silverlight dans votre projet d’application UWP. Copiez le fichier, ajoutez-le au projet et renommez-le Resources.resw pour que le mécanisme de recherche le trouve par défaut. Définissez **Action de génération** sur **PRIResource** et **Copier dans le répertoire de sortie** sur **Ne pas copier**. Vous pouvez ensuite utiliser les chaînes dans le balisage en spécifiant l’attribut **x:Uid** dans vos éléments XAML. Voir [Démarrage rapide : utilisation de ressources de chaîne](https://msdn.microsoft.com/library/windows/apps/xaml/hh965329).

Les applications WindowsPhone Silverlight utilisent la classe **CultureInfo** pour aider à globaliser une application. Les applications UWP utilisent MRT (Modern Resource Technology), qui permet le chargement dynamique des ressources d’application (localisation, échelle et thème) lors de l’exécution et dans l’aire de conception de Visual Studio. Pour plus d’informations, voir [Recommandations relatives aux fichiers, aux données et à la globalisation](https://msdn.microsoft.com/library/windows/apps/dn611859).

La rubrique [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/br206071) décrit comment charger des ressources propres à la famille d’appareils en fonction du facteur de sélection de ressources de cette dernière.

## <a name="media-and-graphics"></a>Média et graphismes

Au moment où vous lisez la section relative au média et aux graphismes d’UWP, gardez à l’esprit que les principes de conception de Windows favorisent la réduction drastique des éléments superflus, notamment l’encombrement et la complexité graphiques. La conception de Windows se caractérise par des éléments visuels, une typographie et un mouvement nets et clairs. Si votre application suit ces mêmes principes, elle ressemblera davantage aux applications intégrées.

WindowsPhone Silverlight a un type **RadialGradientBrush** qui n’est pas présent dans UWP, bien que les autres types de [**pinceau**](/uwp/api/Windows.UI.Xaml.Media.Brush) sont. Dans certains cas, vous serez en mesure d’obtenir un effet similaire avec une image bitmap. Notez que vous pouvez [créer un pinceau dégradé radial](https://msdn.microsoft.com/library/windows/desktop/dd756679) avec Direct2D dans une application UWP [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) et XAML en C++.

WindowsPhone Silverlight a la propriété **System.Windows.UIElement.OpacityMask** , mais cette propriété n’est pas un membre de type UWP [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) . Dans certains cas, vous serez en mesure d’obtenir un effet similaire avec une image bitmap. Vous pouvez également [créer un masque d’opacité](https://msdn.microsoft.com/library/windows/desktop/ee329947) avec Direct2D dans une application UWP [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) et XAML en C++. Néanmoins, un cas d’utilisation courante de la propriété **OpacityMask** consiste à utiliser une seule image bitmap qui s’adapte aux thèmes clair et foncé. Pour les graphiques vectoriels, vous pouvez utiliser les pinceaux système thématiques (par exemple, les graphiques en secteurs illustrés ci-dessous). Pour rendre une image bitmap thématique (par exemple, les coches illustrées ci-dessous), vous devez utiliser une approche différente.

![Image bitmap thématique](images/wpsl-to-uwp-case-studies/wpsl-to-uwp-theme-aware-bitmap.png)

Dans une application WindowsPhone Silverlight, la technique consiste à utiliser un masque alpha (sous la forme d’une image bitmap) comme **OpacityMask** pour un **Rectangle** rempli avec le pinceau de premier plan:

```xml
    <Rectangle Fill="{StaticResource PhoneForegroundBrush}" Width="26" Height="26">
        <Rectangle.OpacityMask>
            <ImageBrush ImageSource="/Assets/wpsl_check.png"/>
        </Rectangle.OpacityMask>
    </Rectangle>
```

La façon la plus simple de porter celui-ci vers une application UWP consiste à utiliser un contrôle [**BitmapIcon**](https://msdn.microsoft.com/library/windows/apps/dn279306), comme ceci :

```xml
    <BitmapIcon UriSource="Assets/winrt_check.png" Width="21" Height="21"/>
```

Ici, winrt\_check.png est un masque alpha sous la forme d’une image bitmap comme l’est wpsl\_check.png et peut très bien être le même fichier. Toutefois, il peut être judicieux de fournir différentes tailles de winrt\_check.png à utiliser pour différents facteurs d’échelle. Pour plus d’informations à ce sujet et pour obtenir une explication des modifications apportées aux valeurs **Width** et **Height**, voir la section [Pixels d’affichage ou effectifs, distance d’affichage et facteurs d’échelle](#view-or-effective-pixels-viewing-distance-and-scale-factors) dans cette rubrique.

Une approche plus générale, qui est appropriée en cas de différences entre les thèmes clair et foncé d’une image bitmap, consiste à utiliser deux composants d’image: l’un avec un premier plan foncé (pour le thème clair) et l’autre avec un premier plan clair (pour le thème foncé). Pour plus de détails sur la façon de cet ensemble de ressources d’image bitmap, voir [personnaliser vos ressources pour la langue, échelle et d’autres qualificateurs](../app-resources/tailor-resources-lang-scale-contrast.md). Une fois qu’un ensemble de fichiers image a été correctement nommé, vous pouvez le désigner dans le résumé, à l’aide de son nom racine, comme ceci:

```xml
    <Image Source="Assets/winrt_check.png" Stretch="None"/>
```

Dans WindowsPhone Silverlight, la propriété **UIElement.Clip** peut être n’importe quelle forme que vous pouvez exprimer avec une **géométrie** et êtes généralement sérialisée dans le balisage XAML en **StreamGeometry** mini langage. Dans UWP, le type de la propriété [**Clip**](https://msdn.microsoft.com/library/windows/apps/br208919) est [**RectangleGeometry**](https://msdn.microsoft.com/library/windows/apps/br210259), de sorte que vous pouvez uniquement détourer une zone rectangulaire. Permettre à un rectangle d’être défini à l’aide du minilangage serait trop permissif. Ainsi, pour porter une zone de détourage dans le balisage, remplacez la syntaxe de l’attribut **Clip** par la syntaxe d’élément de propriété, comme ceci :

```xml
    <UIElement.Clip>
        <RectangleGeometry Rect="10 10 50 50"/>
    </UIElement.Clip>
```

Notez que vous pouvez [utiliser une géométrie arbitraire sous forme de masque dans une couche](https://msdn.microsoft.com/library/windows/desktop/dd756654) avec Direct2D dans une application UWP [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) et XAML en C++.

## <a name="navigation"></a>Navigation

Lorsque vous naviguez vers une page dans une application WindowsPhone Silverlight, vous utilisez un schéma d’adressage URI Uniform Resource Identifier ():

```csharp
    NavigationService.Navigate(new Uri("/AnotherPage.xaml", UriKind.Relative)/*, navigationState*/);
```

Dans une application UWP, vous appelez la méthode [**Frame.Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) et spécifiez le type de la page de destination (tel que défini par l’attribut **x:Class** de définition du balisage XAML de la page) :


```csharp
    // In a page:
    this.Frame.Navigate(typeof(AnotherPage)/*, parameter*/);

    // In a view model, perhaps inside an ICommand implementation:
    var rootFrame = Windows.UI.Xaml.Window.Current.Content as Windows.UI.Xaml.Controls.Frame;
    rootFrame.Navigate(typeof(AnotherPage)/*, parameter*/);
```

Définissez la page de démarrage pour une application WindowsPhone Silverlight dans WMAppManifest.xml:

```xml
    <DefaultTask Name="_default" NavigationPage="MainPage.xaml" />
```

Dans une application UWP, utilisez du code impératif pour définir la page de démarrage. Voici du code d’App.xaml.cs indiquant comment procéder :

```csharp
    if (!rootFrame.Navigate(typeof(MainPage), e.Arguments))
```

Le mappage d’URI et la navigation par fragment sont des techniques de navigation d’URI. Par conséquent, ils ne sont pas applicables à la navigation UWP, qui ne repose pas sur les URI. Le mappage d’URI existe en réponse à la nature faiblement typée de l’identification d’une page cible avec une chaîne d’URI, ce qui conduit à des problèmes de fragilité et de maintenabilité si la page est déplacée dans un autre dossier et donc dans un autre chemin d’accès relatif. Les applications UWP utilisent la navigation basée sur le type, qui est fortement typée et vérifiée par compilateur, et qui ne présente pas le problème que le mappage d’URI résout. Le cas d’utilisation de la navigation par fragment consiste à transmettre un contexte à la page cible pour que la page puisse entraîner le défilement d’un fragment particulier de son contenu dans l’affichage, ou l’affichage de ce fragment sous une autre forme. Le même résultat peut être atteint en transmettant un paramètre de navigation lorsque vous appelez la méthode [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694).

Pour plus d’informations, voir [Navigation](https://msdn.microsoft.com/library/windows/apps/mt187344).

## <a name="resource-key-reference"></a>Référence aux clés de ressources

Le langage de conception a évolué pour Windows 10 et, par conséquent, certains styles système ont été modifiés, et de nombreuses clés de ressources système ont été supprimées ou renommées. L’éditeur de balisage XAML de Visual Studio met en surbrillance les références aux clés de ressources qui ne peuvent pas être résolues. Par exemple, il souligne une référence à la clé de style `PhoneTextNormalStyle` d’une ligne ondulée rouge. Si ce n’est pas corrigé, l’application s’arrête immédiatement lorsque vous essayez de la déployer vers l’émulateur ou l’appareil. Il est donc important de veiller à l’exactitude du balisage XAML. Et vous allez découvrir que Visual Studio est un formidable outil pour intercepter ces problèmes.

Voir également la section [Texte](#text) ci-dessous.

## <a name="status-bar-system-tray"></a>Barre d’état (zone de notification)

La barre d’état système (définie dans le balisage XAML avec `shell:SystemTray.IsVisible`) est désormais appelée barre d’état. Elle s’affiche par défaut. Vous pouvez contrôler sa visibilité dans le code impératif en appelant les méthodes [**Windows.UI.ViewManagement.StatusBar.ShowAsync**](https://msdn.microsoft.com/library/windows/apps/dn610343) et [**HideAsync**](https://msdn.microsoft.com/library/windows/apps/dn610339).

## <a name="text"></a>Texte

Le texte (ou la typographie) constitue un aspect important d’une application UWP et, pendant le portage, il vous sera peut-être utile de revoir les conceptions visuelles de vos vues afin de les harmoniser avec le nouveau langage de conception. Utilisez ces illustrations pour identifier les styles  **TextBlock** système d’UWP disponibles. Recherchez ceux qui correspondre aux styles de WindowsPhone Silverlight que vous avez utilisé. Vous pouvez également créer vos propres styles universels et copier les propriétés des styles système WindowsPhone Silverlight.

![Styles TextBlock système pour les applications Windows10](images/label-uwp10stylegallery.png)

Styles TextBlock système pour les applications Windows 10

Dans une application WindowsPhone Silverlight, la famille de polices par défaut est Segoe WP. Dans une application Windows 10, la famille de polices par défaut est Segoe UI. Par conséquent, les métriques de police dans votre application peuvent être différentes. Si vous souhaitez reproduire l’aspect de votre texte WindowsPhone Silverlight, vous pouvez définir vos propres métriques à l’aide des propriétés telles que [**LineHeight**](https://msdn.microsoft.com/library/windows/apps/br209671) et [**LineStackingStrategy**](https://msdn.microsoft.com/library/windows/apps/br244362). Pour plus d’informations, voir [Recommandations en matière de polices](https://msdn.microsoft.com/library/windows/apps/hh700394.aspx) et [Concevoir des applications UWP](http://dev.windows.com/design).

## <a name="theme-changes"></a>Modifications de thème

Pour une application WindowsPhone Silverlight, le thème par défaut est sombre par défaut. Pour les appareils Windows 10, le thème par défaut a changé, mais vous pouvez contrôler le thème utilisé en déclarant un thème demandé dans App.xaml. Par exemple, pour utiliser un thème sombre sur tous les appareils, ajoutez `RequestedTheme="Dark"` à l’élément racine Application.

## <a name="tiles"></a>Vignettes

Vignettes pour les applications UWP ont des comportements semblables aux vignettes dynamiques pour les applications WindowsPhone Silverlight, il existe certaines différences. Par exemple, le code qui appelle la méthode **Microsoft.Phone.Shell.ShellTile.Create** pour créer des vignettes secondaires doit être porté pour appeler [**SecondaryTile.RequestCreateAsync**](https://msdn.microsoft.com/library/windows/apps/br230606). Voici un exemple avant et après, tout d’abord la version WindowsPhone Silverlight:


```csharp
    var tileData = new IconicTileData()
    {
        Title = this.selectedBookSku.Title,
        WideContent1 = this.selectedBookSku.Title,
        WideContent2 = this.selectedBookSku.Author,
        SmallIconImage = this.SmallIconImageAsUri,
        IconImage = this.IconImageAsUri
    };

    ShellTile.Create(this.selectedBookSku.NavigationUri, tileData, true);
```

Et son équivalent UWP:

```csharp
    var tile = new SecondaryTile(
        this.selectedBookSku.Title.Replace(" ", string.Empty),
        this.selectedBookSku.Title,
        this.selectedBookSku.ArgumentString,
        this.IconImageAsUri,
        TileSize.Square150x150);

    await tile.RequestCreateAsync();
```

Le code qui met à jour une vignette avec la méthode **Microsoft.Phone.Shell.ShellTile.Update** ou la classe **Microsoft.Phone.Shell.ShellTileSchedule**, doit être porté pour utiliser les classes [**TileUpdateManager**](https://msdn.microsoft.com/library/windows/apps/br208622), [**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628), [**TileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616) ou [**ScheduledTileNotification**](https://msdn.microsoft.com/library/windows/apps/hh701637).

Pour plus d’informations sur les vignettes, les toasts, les badges, les bannières et les notifications, voir [Création de vignettes](https://msdn.microsoft.com/library/windows/apps/xaml/hh868260) et [Utilisation de vignettes, de badges et de notifications toast](https://msdn.microsoft.com/library/windows/apps/xaml/hh868259). Pour obtenir des informations spécifiques sur les différentes tailles des composants visuels utilisés pour les vignettes UWP, voir [Composants visuels des vignettes et des toasts](https://msdn.microsoft.com/library/windows/apps/hh781198).

## <a name="toasts"></a>Toasts

Le code qui affiche un toast avec la classe **Microsoft.Phone.Shell.ShellToast** doit être porté pour utiliser les classes [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642), [**ToastNotifier**](https://msdn.microsoft.com/library/windows/apps/br208653), [**ToastNotification**](https://msdn.microsoft.com/library/windows/apps/br208641) ou [**ScheduledToastNotification**](https://msdn.microsoft.com/library/windows/apps/br208607). Notez que sur les appareils mobiles, le terme grand public correspondant à «toast» est «bannière».

Voir [Utilisation de vignettes, de badges et de notifications toast](https://msdn.microsoft.com/library/windows/apps/xaml/hh868259).

## <a name="view-or-effective-pixels-viewing-distance-and-scale-factors"></a>Pixels d’affichage ou effectifs, distance d’affichage et facteurs d’échelle

Les applications WindowsPhone Silverlight et les applications Windows 10 diffèrent abstraire la taille et la disposition des éléments d’interface utilisateur la taille physique réelle et la résolution d’appareils. Pour ce faire, une application WindowsPhone Silverlight utilise des pixels d’affichage. Avec Windows 10, le concept de pixels d’affichage a été affiné en pixels effectifs. Voici une explication de ce terme, sa signification et la valeur supplémentaire qu’il offre.

Le terme « résolution » fait référence à la mesure de la densité des pixels et non, comme on le pense souvent, au nombre de pixels. La « résolution effective » est la façon dont les pixels physiques qui composent une image ou un glyphe apparaissent à l’œil, étant donné les différences liées à la distance de visualisation et à la taille des pixels physiques sur l’appareil (la densité de pixels étant l’inverse de la taille des pixels physiques). La résolution effective est une bonne unité de mesure pour créer une expérience, car elle est centrée sur l’utilisateur. La compréhension de tous ces facteurs et le contrôle de la taille des éléments d’interface utilisateur vous permettent d’optimiser l’expérience utilisateur.

Pour une application WindowsPhone Silverlight, tous les écrans de téléphone sont largeur exacte de 480 pixels d’affichage, sans exception, quel que soit le nombre de pixels physique a de l’écran, ni sa densité de pixels ou sa taille physique. Cela signifie qu’un élément **d’Image** avec `Width="48"` exactement à un dixième de la largeur de l’écran de tout téléphone pouvant exécuter l’application WindowsPhone Silverlight.

Pour une application Windows 10, il est fixe *pas* le cas que tous les appareils sont certains nombre de pixels effectifs. Ceci est probablement évident, étant donné le large éventail d’appareils sur lesquels une application UWP peut s’exécuter. Les différents appareils utilisés présentent une largeur variable (en pixels effectifs). Celle-ci est de 320 epx sur les plus petits d’entre eux, de 1 024 epx sur les écrans de taille modeste et nettement plus élevée sur d’autres. Il vous suffit de continuer à utiliser les éléments à dimensionnement automatique et les panneaux à disposition dynamique que vous utilisez depuis toujours. Dans certains cas, il se peut que vous définissiez une taille fixe pour les propriétés de vos éléments d’interface utilisateur dans le balisage XAML. Un facteur d’échelle est automatiquement affecté à votre application, en fonction de l’appareil sur lequel elle s’exécute et des paramètres d’affichage définis par l’utilisateur. Ce facteur permet aux éléments à taille fixe de l’interface utilisateur de conserver la même taille (approximativement) sur les écrans de différentes tailles de l’utilisateur, pour les opérations tactiles ou pour la lecture. Et avec la disposition dynamique, votre interface utilisateur ne sera pas seulement mise à l’échelle sur différents appareils : elle s’efforcera d’adapter la quantité de contenu appropriée à l’espace disponible.

Étant donné que 480 était auparavant une largeur fixe pixels d’affichage pour un téléphone présentaient et cette valeur est désormais généralement plus petites en pixels effectifs, une règle de base consiste à multiplier toute dimension indiquée dans votre balisage d’application WindowsPhone Silverlight par un facteur de 0,8.

Pour que votre application offre une expérience optimale sur tous les écrans, nous vous recommandons de créer chaque ressource bitmap dans différentes tailles, chacune étant adaptée à un facteur d’échelle spécifique. Fournir des ressources aux échelles 100%, 200% et 400% (dans cet ordre de priorité) produit d’excellents résultats dans la plupart des cas à tous les facteurs d’échelle intermédiaires.

**Remarque**If, pour une raison quelconque, vous ne pouvez pas créer de ressources dans plusieurs tailles, créez des ressources à l’échelle 100 %. Dans Microsoft Visual Studio, le modèle de projet par défaut pour les applications UWP fournit des ressources de personnalisation (vignettes et logos) dans une seule taille, mais elles ne sont pas à l’échelle 100%. Lorsque vous créez des ressources pour votre propre application, suivez les recommandations de cette section, fournissez des tailles 100 %, 200 % et 400 %, et utilisez des packs de ressources.

Si vous disposez d’illustrations complexes, vous serez peut-être amené à fournir vos ressources dans un plus grand nombre de tailles. Si vous débutez avec une image vectorielle, il est relativement aisé de générer des ressources de haute qualité à n’importe quel facteur d’échelle.

Nous vous déconseillons que vous essayez de prendre en charge tous les facteurs d’échelle, mais la liste complète des facteurs d’échelle pour les applications Windows 10 est 100 %, 125 %, 150 %, 200 %, 250 %, 300 % et 400 %. Si vous fournissez ces facteurs, le Windows Store sélectionne les ressources de taille appropriée pour chaque appareil, et seules ces ressources sont téléchargées. Le Windows Store sélectionne les ressources à télécharger en fonction de la résolution de l’appareil.

Pour en savoir plus, voir [Conception réactive 101 pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/dn958435).

## <a name="window-size"></a>Taille de la fenêtre

Dans votre application UWP, vous pouvez spécifier une taille minimale (largeur et hauteur) avec le code impératif. La taille minimale par défaut est 500 x 320 epx (plus petite taille minimale acceptée). La plus grande taille minimale acceptée est de 500 x 500 epx.

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

Rubrique suivante : [Portage pour le modèle d’E/S, d’appareil et d’application](wpsl-to-uwp-input-and-sensors.md).

## <a name="related-topics"></a>Rubriques connexes

* [Mappages des espaces de noms et des classes](wpsl-to-uwp-namespace-and-class-mappings.md)
