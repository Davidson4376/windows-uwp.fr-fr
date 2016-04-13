---
Description: Windows 10 et les nouveaux outils de développement proposent les outils, les fonctionnalités et les expériences pris en charge par la  nouvelle plateforme Windows universelle (UWP).
title: Nouveautés pour les développeurs dans Windows 10, version finale - Juillet 2015
---

# Nouveautés pour les développeurs dans Windows 10, version finale : Juillet 2015


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132).\]

Windows 10 et les nouveaux outils de développement proposent les outils, les fonctionnalités et les expériences pris en charge par la  nouvelle plateforme Windows universelle (UWP). Après avoir [installé les outils et le Kit de développement logiciel](https://dev.windows.com/downloads) sur Windows 10, vous êtes prêt à [créer une nouvelle application Windows universelle](https://msdn.microsoft.com/library/windows/apps/bg124288) ou à découvrir comment utiliser votre [code d’application existant sur Windows](https://msdn.microsoft.com/library/windows/apps/mt238321).

Voici un aperçu, fonction par fonction, des nouveautés qui vous attendent dans la version finale de Windows 10.

## Dispositions adaptatives


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Plusieurs vues pour le contenu personnalisé</td>
<td align="left">XAML fournit une nouvelle prise en charge pour la définition de vues personnalisées (fichiers .xaml) partageant le même fichier de code. Ainsi, vous pouvez plus facilement créer et gérer différentes vues personnalisées pour une gamme d’appareils ou un scénario spécifiques. Si votre application présente du contenu d’interface utilisateur, une disposition ou des modèles de navigation radicalement différents en fonction des scénarios appliqués, générez plusieurs vues. Par exemple, vous pouvez utiliser un élément [<strong>Pivot</strong>](https://msdn.microsoft.com/library/windows/apps/dn608241) avec la navigation optimisée pour une utilisation à une main de votre application mobile, mais utiliser un élément [<strong>SplitView</strong>](https://msdn.microsoft.com/library/windows/apps/dn864360) avec un menu de navigation optimisée pour la souris sur votre application de bureau.</td>
</tr>
<tr class="even">
<td align="left">StateTriggers</td>
<td align="left">À l’aide de la nouvelle fonctionnalité [<strong>VisualState.StateTriggers</strong>](https://msdn.microsoft.com/library/windows/apps/dn890396), vous pouvez configurer de manière conditionnelle des propriétés basées sur la hauteur/la largeur de la fenêtre ou sur un déclencheur personnalisé. Auparavant, vous deviez traiter les événements [<strong>SizeChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br209055) de fenêtre dans le code et appeler [<strong>VisualStateManager.GoToState</strong>](https://msdn.microsoft.com/library/windows/apps/br209025).</td>
</tr>
<tr class="odd">
<td align="left">Setters</td>
<td align="left"><p>À l’aide de la nouvelle syntaxe [<strong>VisualState.Setters</strong>](https://msdn.microsoft.com/library/windows/apps/dn890395), vous pouvez utiliser une balise simplifiée pour définir les modifications de propriétés dans [<strong>VisualStateManager</strong>](https://msdn.microsoft.com/library/windows/apps/br209021). Auparavant, il vous fallait utiliser une table de montage séquentielle et créer des animations pour appliquer les modifications de propriétés, relatives par exemple à l’orientation d’un élément [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635), de Horizontal à Vertical. Dans les applications Windows universelles, vous pouvez utiliser cette syntaxe Setter simplifiée :</p>
<p><code>&lt;setter target=&quot;stackPanel1.Orientation&quot; value=&quot;Vertical&quot; /&gt;</code></p></td>
</tr>
</tbody>
</table>

 

## Fonctionnalités XAML


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Liaisons de données compilées (x: Bind)</td>
<td align="left"><p>Dans les applications Windows Universelles, vous pouvez utiliser le nouveau mécanisme de liaison basé sur compilateur, activé par la propriété x:Bind. Les liaisons basées sur compilateur étant fortement typées et traitées lors de la compilation, la procédure est accélérée et présente des erreurs de compilation en cas de défaut correspondance des types de liaison. Les liaisons étant traduites en code d’application compilé, vous pouvez désormais les déboguer en analysant le code dans Visual Studio, afin de diagnostiquer des problèmes spécifiques de liaison. Par ailleurs, vous pouvez utiliser la propriété x/Bind pour lier une méthode, comme ceci :</p>
<p><code>&lt;textblock text=&quot;{x:Bind Customer.Address.ToString()}&quot; /&gt;</code></p>
<p>Pour les scénarios de liaison typiques, utilisez x:Bind en lieu et place de Binding afin d’améliorer les performances et la maintenabilité.</p></td>
</tr>
<tr class="even">
<td align="left">Rendu incrémentiel déclaratif de listes (x:Phase)</td>
<td align="left"><p>Dans les applications Windows universelles, le nouvel attribut x:Phase vous permet de générer un rendu incrémentiel, ou progressif, des listes à l’aide de XAML en lieu et place du code. Lors d’un mouvement panoramique effectué sur de longues listes comportant des éléments complexes, votre application peut avoir des difficultés à afficher les éléments suffisamment rapidement. Le cas échéant, l’expérience de vos utilisateurs est affectée négativement. Grâce au rendu progressif, vous êtes en mesure de spécifier la priorité de rendu des composants d’un élément de liste, de manière à afficher uniquement les portions les plus importantes de l’élément de liste dans les scénarios de panoramique rapide. Dès lors, votre utilisateur jouit d’une expérience de panoramique plus agréable.</p>
<p>Dans Windows 8.1, vous pouviez traiter l’événement [<strong>ContainerContentChanging</strong>](https://msdn.microsoft.com/library/windows/apps/dn298914) et écrire du code pour paramétrer l’affichage progressif des éléments de liste. Dans les applications UWP, vous pouvez accomplir le rendu progressif de manière déclarative à l’aide de l’attribut x:Phase. Utilisé conjointement avec les liaisons compilées x:Bind, l’attribut x:Phase vous permet de spécifier simplement une priorité de rendu pour chaque élément lié dans un modèle de données. Lors du panoramique, la tâche de rendu des éléments est effectuée par tranche temporelle en fonction de la phase considérée, pour un rendu incrémentiel des éléments.</p></td>
</tr>
<tr class="odd">
<td align="left">Chargement différé des éléments d’interface utilisateur (x:DeferLoadingStrategy)</td>
<td align="left">Dans les applications Windows universelles, la nouvelle directive x:DeferLoadingStrategy prend en charge le chargement différé de portions de votre interface utilisateur. Cette configuration améliore les performances de démarrage et réduit l’utilisation de la mémoire de votre application. Par exemple, si l’interface utilisateur de votre application présente un élément de validation de données qui s’affiche uniquement lors de la saisie de données incorrectes, vous pouvez reporter au moment opportun le chargement de cet élément. Par la suite, les objets d’éléments ne sont pas créés lors du chargement de la page, mais en cas d’erreur de données. Ils doivent être ajoutés à l’arborescence visuelle de la page.</td>
</tr>
<tr class="even">
<td align="left">SplitView</td>
<td align="left">Le nouveau contrôle [<strong>SplitView</strong>](https://msdn.microsoft.com/library/windows/apps/dn864360) vous procure un moyen d’afficher et de masquer aisément du contenu temporaire. Il est généralement utilisé pour les scénarios de navigation de niveau supérieur de type « menu façon hamburger », dans lesquels le contenu de navigation est masqué, et apparaît au besoin suite à une action de l’utilisateur.</td>
</tr>
<tr class="odd">
<td align="left">RelativePanel</td>
<td align="left">[<strong>RelativePanel</strong>](https://msdn.microsoft.com/library/windows/apps/dn879546) est un nouveau panneau de disposition qui vous permet de positionner et d’aligner les objets enfants les uns par rapport aux autres ou vis-à-vis du panneau parent. Par exemple, vous pouvez définir une configuration suivant laquelle certains éléments textuels doivent toujours être positionnés à gauche du panneau et un élément Button toujours aligné sous le texte. Utilisez <strong>RelativePanel</strong> lors de la création d’interfaces utilisateur qui ne présentent pas de modèle linéaire clair qui invoquerait l’utilisation d’un élément [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635) ou d’un élément [<strong>Grid</strong>](https://msdn.microsoft.com/library/windows/apps/br242704).</td>
</tr>
<tr class="even">
<td align="left">CalendarView</td>
<td align="left">Le contrôle [<strong>CalendarView</strong>](https://msdn.microsoft.com/library/windows/apps/dn890052) facilite l’affichage et la sélection de date et de plages de dates à l’aide d’une vue personnalisable, par mois. <strong>CalendarView</strong> prend en charge les fonctions de dates minimales, maximales et grisées et limite ainsi la liste des dates pouvant être sélectionnées. Vous pouvez également définir des barres de densité personnalisées, à utiliser pour afficher le « remplissage » général du calendrier d’un jour donné.</td>
</tr>
<tr class="odd">
<td align="left">CalendarDatePicker</td>
<td align="left">[<strong>CalendarDatePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn950083) est un contrôle déroulant optimisé pour la sélection d’une date isolée dans un élément [<strong>CalendarView</strong>](https://msdn.microsoft.com/library/windows/apps/dn890052) lorsque des informations contextuelles, comme le jour de la semaine ou le taux de remplissage du calendrier présentent une importance notable. Il est similaire au contrôle [<strong>DatePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn298584) mais le contrôle <strong>DatePicker</strong> est quant à lui idéal pour la sélection d’une date précise, comme un anniversaire.</td>
</tr>
<tr class="even">
<td align="left">MediaTransportControls</td>
<td align="left">La nouvelle classe [<strong>MediaTransportControls</strong>](https://msdn.microsoft.com/library/windows/apps/dn831962) simplifie la personnalisation des contrôles de transport d’un élément [<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926). Dans Windows 8.1, vous pouviez activer les contrôles de transport intégrés de <strong>MediaElement</strong> ou créer vos propres contrôles de transport qui appelaient des méthodes <strong>MediaElement</strong>. Désormais, vous pouvez utiliser la fonctionnalité intégrée de <strong>MediaTransportControls</strong>, tout en personnalisant facilement l’apparence en fonction de votre application.</td>
</tr>
<tr class="odd">
<td align="left">Notifications de modifications de propriétés</td>
<td align="left"><p>Dans les applications Windows universelles, vous pouvez surveiller les modifications de propriétés sur [<strong>DependencyObject</strong>](https://msdn.microsoft.com/library/windows/apps/br242356), même pour les propriétés qui ne présentent aucun événement de modification correspondant.</p>
<p>La notification fonctionne comme un événement, mais elle est en réalité exposée comme un rappel. Le rappel traite l’argument sender comme un gestionnaire d’événements, sans traiter d’argument d’événement. À la place, seul l’identificateur de propriété est transmis. Avec ces informations, votre application peut définir un gestionnaire unique pour plusieurs notifications de propriétés. Pour plus d’informations, voir [<strong>RegisterPropertyChangedCallback</strong>](https://msdn.microsoft.com/library/windows/apps/dn831902) et [<strong>UnregisterPropertyChangedCallback</strong>](https://msdn.microsoft.com/library/windows/apps/dn831910).</p></td>
</tr>
<tr class="even">
<td align="left">Cartes</td>
<td align="left"><p>La classe [<strong>MapControl</strong>](https://msdn.microsoft.com/library/windows/apps/dn637004) a été mise à jour pour fournir des images aériennes 3D et des vues au niveau de la rue. Ces nouvelles fonctionnalités et une autre fonctionnalité de mappage antérieure sont désormais disponibles pour les applications Windows universelles. Ajouter du mappage à votre application à l’aide des API suivantes :</p>
<ul>
<li>Espace de noms [<strong>Windows.UI.Xaml.Controls.Maps</strong>](https://msdn.microsoft.com/library/windows/apps/dn610751) : affiche les cartes.</li>
<li>Espace de noms [<strong>Windows.Services.Maps</strong>](https://msdn.microsoft.com/library/windows/apps/dn636979) : recherche les emplacements et les itinéraires.</li>
</ul>
<p>Pour commencer à utiliser ces API dans une application Windows universelle dès aujourd’hui, solliciter une clé dans le [Bing Maps Developer Center](https://www.bingmapsportal.com/). Pour plus d’informations, voir [How to authenticate a Maps app](https://msdn.microsoft.com/library/windows/apps/xaml/dn741528). Autre nouveauté dans Windows 10, les utilisateurs de PC et de téléphones peuvent télécharger des cartes hors connexion à partir de l’application Paramètres. Lorsqu’elles sont disponibles, les cartes hors connexion sont utilisées par l’élément[<strong>MapControl</strong>](https://msdn.microsoft.com/library/windows/apps/dn637004), qui prend en charge l’affichage des cartes en cas d’indisponibilité d’accès Internet.</p></td>
</tr>
<tr class="odd">
<td align="left">Mappage de bouton d'entrée</td>
<td align="left">La classe [<strong>Windows.UI.Xaml.Input.KeyRoutedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/hh943072) dispose d’une nouvelle propriété [<strong>OriginalKey</strong>](https://msdn.microsoft.com/library/windows/apps/dn960168) qui, avec une mise à jour correspondante à [<strong>Windows.System.VirtualKey</strong>](https://msdn.microsoft.com/library/windows/apps/br241812), vous permet de récupérer le bouton d’entrée d’origine, non mappé qui est associé à l’événement d’entrée au clavier.</td>
</tr>
<tr class="even">
<td align="left">Entrée manuscrite</td>
<td align="left"><p>Il est désormais plus simple d’utiliser la fonctionnalité robuste d’entrée manuscrite dans les applications Windows Runtime à l’aide de C++, C#, ou Visual Basic, grâce au contrôle [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535) et aux classes sous-jacentes [<strong>InkPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn922011).</p>
<p>Le contrôle [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535) définit une zone de superposition dédiée à la rédaction et au rendu des entrées manuscrites. La fonctionnalité associée à ce contrôle (entrée, traitement et rendu) provient des classes [<strong>InkPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn922011), [<strong>InkStroke</strong>](https://msdn.microsoft.com/library/windows/apps/br208485), [<strong>InkRecognizer</strong>](https://msdn.microsoft.com/library/windows/apps/br208478) et [<strong>InkSynchronizer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903979).</p>
<p></p>
<div class="alert">
<strong>Important</strong> Ces classes ne sont pas prises en charge dans les applications Windows utilisant JavaScript.
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## Mises à jour des fonctionnalités XAML


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Mises à jour CommandBar et AppBar</td>
<td align="left"><p>Les contrôles [<strong>CommandBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn279427) et [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) ont été mis à jour pour présenter une API, un comportement et une expérience utilisateur uniformes sur les applications UWP, sur toutes les gammes d’appareils.</p>
<p>Le contrôle [<strong>CommandBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn279427) pour les applications Windows universelles a été amélioré pour présenter un sur-ensemble de fonctionnalité [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) et une plus grande flexibilité de son utilisation dans votre application. Vous devez utiliser <strong>CommandBar</strong> pour l’ensemble des nouvelles applications Windows universelles sur Windows 10. Dans un contrôle <strong>CommandBar</strong>, dans Windows 8.1, vous pourriez utiliser uniquement les contrôles ayant implémenté l’élément [<strong>ICommandBarElement</strong>](https://msdn.microsoft.com/library/windows/apps/dn251969), comme [<strong>AppBarButton</strong>](https://msdn.microsoft.com/library/windows/apps/dn279244). Dans les applications Windows universelle, vous pouvez désormais placer du contenu personnalisé dans le contrôle <strong>CommandBar</strong>, en plus des éléments <strong>AppBarButton</strong>s.</p>
<p>Le contrôle [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) a été mis à jour pour faciliter le déplacement de vos applications Windows 8.1 utilisant <strong>AppBar</strong> vers la plateforme Windows universelle. Le contrôle <strong>AppBar</strong> a été conçu pour être utilisé avec des applications en plein écran, et être sollicité à l’aide de mouvements latéraux. Les mises à jour du contrôle sont relatives aux problèmes associés aux applications fenêtrées et au manque de mouvements latéraux dans Windows 10.</p>
<p>Le contrôle masqué [<strong>AppBar.ClosedDisplayMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn633872), auparavant dédié aux appareils Windows Phone, est désormais pris en charge sur l’ensemble des gammes d’appareils. Dès lors, vous pouvez choisir différents niveaux d’indication pour les commandes. Par défaut, le contrôle [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) procure une indication minimale afin de garantir la cohérence lors des mises à niveau de vos applications Windows 8.1 vers les applications Windows universelles, dans les situations où les mouvements latéraux ne sont plus pris en charge sur la plateforme.</p>
<p><strong>Nouvelle API</strong> [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) <strong> :</strong>[<strong>Closing</strong>](https://msdn.microsoft.com/library/windows/apps/dn996483), [<strong>OnClosing</strong>](https://msdn.microsoft.com/library/windows/apps/dn996484), [<strong>Opening</strong>](https://msdn.microsoft.com/library/windows/apps/dn996486), [<strong>OnOpening</strong>](https://msdn.microsoft.com/library/windows/apps/dn996485), [<strong>TemplateSettings</strong>](https://msdn.microsoft.com/library/windows/apps/dn996487).</p>
<p><strong>Nouvelle API</strong> [<strong>CommandBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn279427) <strong> :</strong>[<strong>CommandBarOverflowPresenterStyle</strong>](https://msdn.microsoft.com/library/windows/apps/dn975227) et [<strong>CommandBarOverflowPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn975225).</p></td>
</tr>
<tr class="even">
<td align="left">Mises à jour GridView</td>
<td align="left">Avant Windows 10, l’orientation de disposition [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) par défaut était horizontale sur Windows et verticale sur Windows Phone. Dans les applications UWP, <strong>GridView</strong> utilise une disposition verticale par défaut sur l’ensemble des gammes d’appareils, afin de vous garantir une expérience par défaut cohérente.</td>
</tr>
<tr class="odd">
<td align="left">Propriété AreStickyGroupHeadersEnabled</td>
<td align="left">Lorsque vous affichez des données groupées dans [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) ou [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705), les en-têtes de groupe restent désormais visibles lorsque vous faites défiler la liste. Cela est important dans les grands ensembles de données, pour lesquels l’en-tête fournit du contexte relatif aux données affichées par l’utilisateur. Toutefois, dans les situations où les groupes comportent uniquement quelques éléments, vous souhaiterez peut-être voir les en-têtes sortir de l’écran avec les éléments. Vous pouvez définir la propriété [<strong>AreStickyGroupHeadersEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn932028) sur [<strong>ItemsStackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/dn298795) et sur [<strong>ItemsWrapGrid</strong>](https://msdn.microsoft.com/library/windows/apps/dn298849) pour contrôler ce comportement.</td>
</tr>
<tr class="even">
<td align="left">Méthode GroupHeaderContainerFromItemContainer</td>
<td align="left">Lorsque vous affichez les données groupées dans un élément [<strong>ItemsControl</strong>](https://msdn.microsoft.com/library/windows/apps/br242803), vous pouvez appeler la méthode [<strong>GroupHeaderContainerFromItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903984) afin d’obtenir une référence à l’en-tête parent du groupe. Par exemple, si un utilisateur supprime le dernier élément d’un groupe, vous pouvez obtenir une référence à l’en-tête de groupe et supprimer simultanément l’élément et l’en-tête de groupe.</td>
</tr>
<tr class="odd">
<td align="left">Événement ChoosingGroupHeaderContainer</td>
<td align="left">Le nouvel événement [<strong>ChoosingGroupHeaderContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn899082) sur [<strong>ListViewBase</strong>](https://msdn.microsoft.com/library/windows/apps/br242879) permet de définir l’état des en-têtes de groupe d’un élément [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) ou [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705). Par exemple, vous pouvez gérer cet événement pour définir la propriété [<strong>AutomationProperties.NameProperty</strong>](https://msdn.microsoft.com/library/windows/apps/br209099) sur l’en-tête de groupe afin de représenter le groupe dans des technologies d’assistance.</td>
</tr>
<tr class="even">
<td align="left">Événement ChoosingItemContainer</td>
<td align="left">Le nouvel événement [<strong>ChoosingItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903989) sur [<strong>ListViewBase</strong>](https://msdn.microsoft.com/library/windows/apps/br242879) vous octroie un contrôle supérieur sur la virtualisation de l’interface utilisateur dans un élément [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) ou [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705). Utilisez cet événement avec l’événement [<strong>ContainerContentChanging</strong>](https://msdn.microsoft.com/library/windows/apps/dn298914) afin de gérer votre propre file d’attente de conteneurs recyclés, dans laquelle puiser au besoin. Par exemple, si la source de données a été réinitialisée en raison du filtrage, vous pouvez rapidement mettre en correspondance un ensemble créé d’éléments visuels (ItemContainers) avec leurs données pour atteindre des performances supérieures.</td>
</tr>
<tr class="odd">
<td align="left">Virtualisation du défilement des listes</td>
<td align="left"><p>Les contrôles XAML [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) et [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) présentent un nouvel événement [<strong>ListViewBase.ChoosingItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903989) qui améliore les performances du contrôle en cas de modification dans la collecte de données.</p>
<p>Au lieu de réinitialiser entièrement la liste et d’activer une nouvelle lecture de l’animation d’entrée, le système gère désormais les éléments actuellement affichés, avec le focus et l’état de sélection. Les nouveaux éléments et les éléments supprimés de la fenêtre d’affichage entrent et sortent sans heurt de l’animation. Après une modification dans la collecte de données dans laquelle les conteneurs ne sont pas détruits, une application peut rapidement mettre des « anciens » éléments en correspondance avec leur précédent conteneur et passer le traitement approfondi des méthodes de remplacement de cycle de vie de conteneur. Seuls les « nouveaux » éléments sont traités et associés à des conteneurs recyclés ou nouveaux.</p></td>
</tr>
<tr class="even">
<td align="left">Méthode SelectRange et propriété SelectedRanges</td>
<td align="left">Dans les applications Windows universelles, les contrôles [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) et [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) prennent en charge la sélection d’éléments en fonction des plages d’index d’éléments, non pas des références d’objets d’éléments. Il s’agit d’un moyen plus efficace de décrire des sélections d’éléments, dans la mesure où il n’est pas nécessaire de créer des objets d’élément pour chaque élément sélectionné. Pour plus d’informations, voir [<strong>ListViewBase.SelectedRanges</strong>](https://msdn.microsoft.com/library/windows/apps/dn904244), [<strong>ListViewBase.SelectRange</strong>](https://msdn.microsoft.com/library/windows/apps/dn904245), et [<strong>ListViewBase.DeselectRange</strong>](https://msdn.microsoft.com/library/windows/apps/dn904241).</td>
</tr>
<tr class="odd">
<td align="left">Nouvelles API ListViewItemPresenter</td>
<td align="left">[<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) et [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) utilisent des présentateurs d’éléments pour fournir les éléments visuels par défaut pour la sélection et le focus. Dans les applications UWP, [<strong>ListViewItemPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn298500) et [<strong>GridViewItemPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn279298) présentent de nouvelles propriétés permettant de personnaliser les effets visuels des éléments de listes. Les nouvelles propriétés sont [<strong>CheckBoxBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn913905), [<strong>CheckMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn913923), [<strong>FocusSecondaryBorderBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn898370), [<strong>PointerOverForeground</strong>](https://msdn.microsoft.com/library/windows/apps/dn898372), [<strong>PressedBackground</strong>](https://msdn.microsoft.com/library/windows/apps/dn913931) et [<strong>SelectedPressedBackground</strong>](https://msdn.microsoft.com/library/windows/apps/dn913937).</td>
</tr>
<tr class="even">
<td align="left">Mises à jour SemanticZoom</td>
<td align="left"><p>Le contrôle [<strong>SemanticZoom</strong>](https://msdn.microsoft.com/library/windows/apps/hh702601) affiche désormais un comportement uniforme pour les applications UWP, sur l’ensemble des familles d’appareils.</p>
<p>L’action par défaut pour le basculement entre les zooms avant et arrière consiste à appuyer sur un en-tête de groupe dans le zoom avant. Il s’agit du comportement identifié sur Windows Phone 8.1, différent de celui respecté sur Windows 8.1. En effet, sur cette version, le basculement s’effectue à l’aide d’un pincement. Pour modifier l’affichage à l’aide de la fonctionnalité de zoom par pincement, définissez l’élément [<strong>ScrollViewer.ZoomMode</strong>](https://msdn.microsoft.com/library/windows/apps/br209601)=&quot;Enabled&quot; sur le contrôle interne de [<strong>SemanticZoom</strong>](https://msdn.microsoft.com/library/windows/apps/hh702601), [<strong>ScrollViewer</strong>](https://msdn.microsoft.com/library/windows/apps/br209527).</p>
<p>Sur les applications Windows universelles, le zoom arrière remplace le zoom avant. Il présente la même taille que la vue remplacée. S’il s’agit du comportement rencontré sur Windows 8.1, il est différent de celui de Windows Phone 8.1, où le zoom arrière prenait toute la largeur de l’écran et apparaissait par-dessus l’ensemble des autres contenus.</p></td>
</tr>
<tr class="odd">
<td align="left">Mises à jour DatePicker et TimePicker</td>
<td align="left">Les contrôles [<strong>DatePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn298584) et [<strong>TimePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn299280) présentent désormais une implémentation cohérente pour les applications Windows universelles, sur l’ensemble des familles d’appareils. Ils ont également une nouvelle apparence pour Windows 10. La portion contextuelle utilise désormais les contrôles [<strong>DatePickerFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn625013) et [<strong>TimePickerFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn608313) sur l’ensemble des appareils. Si ce comportement est identique à celui rencontré sur Windows Phone 8.1, il est différent de celui de Windows 8.1, qui tirait partie des contrôles [<strong>ComboBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209348). Les menus volants vous permettent de créer plus facilement des sélecteurs personnalisés de date et d’heure.</td>
</tr>
<tr class="even">
<td align="left">Nouvelles API ScrollViewer</td>
<td align="left">Le contrôle [<strong>ScrollViewer</strong>](https://msdn.microsoft.com/library/windows/apps/br209527) présente les nouveaux événements [<strong>DirectManipulationStarted</strong>](https://msdn.microsoft.com/library/windows/apps/dn858544) et [<strong>DirectManipulationCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn858543), utilisés pour signaler à votre application le démarrage et l’arrêt du défilement panoramique tactile. Vous pouvez gérer ces événements pour coordonner votre interface utilisateur avec ces actions utilisateur.</td>
</tr>
<tr class="odd">
<td align="left">Mises à jour MenuFlyout</td>
<td align="left">Dans les applications Windows universelles, vous trouverez de nouvelles API qui vous permettent de créer plus facilement des menus contextuels plus efficaces. La nouvelle méthode [<strong>MenuFlyout.ShowAt</strong>](https://msdn.microsoft.com/library/windows/apps/dn913174) vous permet de spécifier l’emplacement où vous voulez que le menu volant apparaisse par rapport à un autre élément. (Par ailleurs, votre contrôle [<strong>MenuFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn299030) peut même chevaucher les bordures de la fenêtre de votre application.) Utilisez la nouvelle classe [<strong>MenuFlyoutSubItem</strong>](https://msdn.microsoft.com/library/windows/apps/dn913170) afin de créer des menus en cascade.</td>
</tr>
<tr class="even">
<td align="left">Nouvelles propriétés Border pour ContentPresenter, Grid et StackPanel</td>
<td align="left">Les contrôles de conteneurs communs présentent de nouvelles propriétés de bordure. Grâce à ces dernières, vous pouvez tracer une bordure, sans ajouter aucun élément Border supplémentaire dans votre code XAML. [<strong>ContentPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/br209378), [<strong>Grid</strong>](https://msdn.microsoft.com/library/windows/apps/br242704) et [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635) présentent ces nouvelles propriétés : [<strong>BorderBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn960179), [<strong>BorderThickness</strong>](https://msdn.microsoft.com/library/windows/apps/dn960181), [<strong>CornerRadius</strong>](https://msdn.microsoft.com/library/windows/apps/dn960183) et [<strong>Padding</strong>](https://msdn.microsoft.com/library/windows/apps/dn960187).</td>
</tr>
<tr class="odd">
<td align="left">Nouvelles API textuelles sur ContentPresenter</td>
<td align="left">[<strong>ContentPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/br209378) possède de nouvelles API qui vous octroient un contrôle supérieur sur l’affichage du texte : [<strong>LineHeight</strong>](https://msdn.microsoft.com/library/windows/apps/dn903982), [<strong>LineStackingStrategy</strong>](https://msdn.microsoft.com/library/windows/apps/dn831933), [<strong>MaxLines</strong>](https://msdn.microsoft.com/library/windows/apps/dn831935) et [<strong>TextWrapping</strong>](https://msdn.microsoft.com/library/windows/apps/dn831937).</td>
</tr>
<tr class="even">
<td align="left">Visuels de focus système</td>
<td align="left">Les visuels de focus des contrôles XAML sont désormais créés par le système, non pas déclarés en tant qu’éléments XAML dans le modèle de contrôle. Les visuels de focus ne sont généralement pas nécessaires sur les appareils mobiles. De fait, en laissant le système les créer et les gérer au besoin, vous améliorez les performances applicatives. Si vous avez besoin d’un contrôle supérieur sur les visuels de focus, vous pouvez remplacer le comportement du système en fournissant un modèle de contrôle personnalisé les définissant. Pour plus d’informations, voir [<strong>UseSystemFocusVisuals</strong>](https://msdn.microsoft.com/library/windows/apps/dn899076) et [<strong>IsTemplateFocusTarget</strong>](https://msdn.microsoft.com/library/windows/apps/dn899074).</td>
</tr>
<tr class="odd">
<td align="left">PasswordBox.PasswordRevealMode</td>
<td align="left"><p>Dans les applications Windows universelles, la propriété [<strong>PasswordRevealMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn890867) remplace [<strong>IsPasswordRevealButtonEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/hh702579) afin d’offrir un comportement uniforme sur l’ensemble des familles d’appareils.</p>
<p></p>
<div class="alert">
<strong>Attention</strong> Avant Windows 10, le bouton d’affichage du mot de passe ne s’affichait pas par défaut, ce qui est le cas dans les applications Windows universelles. Si la sécurité de votre application nécessite le masquage permanent de votre mot de passe, veillez à définir [<strong>PasswordRevealMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn890867) sur Masqué.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">Control.IsTextScaleFactorEnabled</td>
<td align="left">La propriété [<strong>IsTextScaleFactorEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn624997) disponible auparavant sur Windows Phone 8.1 est désormais accessible sur les applications Windows universelles, sur l’ensemble des familles d’appareils.</td>
</tr>
<tr class="odd">
<td align="left">AutoSuggestBox</td>
<td align="left">Le contrôle [<strong>AutoSuggestBox</strong>](https://msdn.microsoft.com/library/windows/apps/dn633874) de Windows Phone 8.1 est désormais accessible pour les applications Windows universelles sur l’ensemble des familles d’appareils. Il est recommandé de l’utiliser en lieu et place de [<strong>SearchBox</strong>](https://msdn.microsoft.com/library/windows/apps/dn252771). <strong>AutoSuggestBox</strong> fournit des suggestions, relatives notamment aux types d’utilisateur. Ce contrôle est parfaitement compatible avec différents types d’entrées, comme la fonction tactile, le clavier et des éditeurs de méthode d’entrée. Il présente par ailleurs quelques nouveaux membres qui optimisent la fonctionnalité de la zone de recherche : la propriété [<strong>QueryIcon</strong>](https://msdn.microsoft.com/library/windows/apps/dn996494) et l’événement [<strong>QuerySubmitted</strong>](https://msdn.microsoft.com/library/windows/apps/dn996497).</td>
</tr>
<tr class="even">
<td align="left">ContentDialog</td>
<td align="left">Le contrôle [<strong>ContentDialog</strong>](https://msdn.microsoft.com/library/windows/apps/dn633972) de Windows Phone 8.1 est désormais disponible pour les applications Windows universelles, sur toutes les familles d’appareils. <strong>ContentDialog</strong> vous permet d’afficher une boîte de dialogue modale personnalisable qui fonctionne parfaitement sur l’ensemble des familles d’appareils.</td>
</tr>
<tr class="odd">
<td align="left">Pivot</td>
<td align="left">Le contrôle [<strong>Pivot</strong>](https://msdn.microsoft.com/library/windows/apps/dn608241) de Windows 8.1 est désormais disponible pour l’ensemble des applications Windows universelles, sur l’ensemble des familles d’appareils. Vous pouvez désormais utiliser le même contrôle <strong>Pivot</strong> pour vos applications exécutées sur les appareils mobiles et de bureau. <strong>Pivot</strong> offre un comportement adaptatif, en fonction de la taille de l’écran et du type d’entrée. Vous pouvez styliser un contrôle <strong>Pivot</strong> pour offrir une configuration à onglets, avec différentes vues des informations sur chaque élément pivot.</td>
</tr>
</tbody>
</table>

 

## Texte


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">API Windows Core Text</td>
<td align="left"><p>Le nouvel espace de noms [<strong>Windows.UI.Text.Core</strong>](https://msdn.microsoft.com/library/windows/apps/dn958238) comporte un système client-serveur qui centralise le traitement des entrées du clavier dans un serveur unique.</p>
<p>Vous pouvez l’utiliser pour manipuler la mémoire-tampon de modification de votre contrôle de saisie de texte personnalisé. Le serveur de saisie de texte garantit la synchronisation du contenu de votre contrôle de saisie de texte et du contenu de sa propre mémoire-tampon de modification, via un canal de communication asynchrone entre l’application et le serveur.</p></td>
</tr>
<tr class="even">
<td align="left">Icônes de vecteur</td>
<td align="left">L’élément [<strong>Glyphs</strong>](https://msdn.microsoft.com/library/windows/apps/br209921) présente les nouvelles propriétés [<strong>IsColorFontEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn900468) et [<strong>ColorFontPalleteIndex</strong>](https://msdn.microsoft.com/library/windows/apps/dn900466) pour la prise en charge des polices colorées. Désormais, vous pouvez utiliser un fichier de polices pour afficher les icônes basées sur les polices. Lorsque vous utilisez <strong>ColorFontPalleteIndex</strong> pour le basculement des palettes de couleur, une icône peut être affichée avec différents ensembles de couleurs, par exemple pour afficher une version activée et désactivée de l’icône.</td>
</tr>
<tr class="odd">
<td align="left">Événements de la fenêtre Éditeur de méthode d'entrée</td>
<td align="left">Parfois, les utilisateurs saisissent du texte dans un éditeur de méthode d’entrée qui s’affiche dans une fenêtre située juste en dessous d’une zone de saisie textuelle (généralement pour les langues d’Asie orientale). Vous pouvez utiliser l’événement [<strong>CandidateWindowBoundsChanged</strong>](https://msdn.microsoft.com/library/windows/apps/dn955144) et la propriété [<strong>DesiredCandidateWindowAlignment</strong>](https://msdn.microsoft.com/library/windows/apps/dn955145) sur [<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683) et [<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548) pour renforcer la compatibilité de l’interface utilisateur de votre application avec la fenêtre Éditeur de méthode d'entrée.</td>
</tr>
<tr class="even">
<td align="left">Événements de composition de texte</td>
<td align="left">[<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683) et [<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548) présentent de nouveaux événements conçus pour prévenir votre application de la composition de contenu textuel à l’aide de l’Éditeur de méthode d’entrée : [<strong>TextCompositionStarted</strong>](https://msdn.microsoft.com/library/windows/apps/dn891458), [<strong>TextCompositionEnded</strong>](https://msdn.microsoft.com/library/windows/apps/dn891457) et [<strong>TextCompositionChanged</strong>](https://msdn.microsoft.com/library/windows/apps/dn891456). Vous pouvez gérer ces événements afin de coordonner votre code d’application avec le processus de composition textuelle de l’Éditeur de méthode d'entrée. Par exemple, vous pouvez implémenter une fonctionnalité de saisie automatique inline pour les langues d’Asie orientale.</td>
</tr>
<tr class="odd">
<td align="left">Gestion améliorée du texte bidirectionnel</td>
<td align="left"><p>Les contrôles textuels XAML présentent une nouvelle API améliorant la gestion du texte bidirectionnel. Vous profiter d’un meilleur alignement du texte et d’une directionnalité de paragraphe supérieure dans de nombreuses langues d’entrée.</p>
<p>La valeur par défaut de la propriété [<strong>TextReadingOrder</strong>](https://msdn.microsoft.com/library/windows/apps/dn949133) ayant été changée en <strong>DetectFromContent</strong>, la détection de l’ordre de lecture est prise en charge par défaut. La propriété <strong>TextReadingOrder</strong> a également été ajoutée à [<strong>PasswordBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227519), [<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548) et [<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683).</p>
<p>Vous pouvez définir la propriété [<strong>TextAlignment</strong>](https://msdn.microsoft.com/library/windows/apps/br209703) des contrôles de texte sur la nouvelle valeur <strong>DetectFromContent</strong> afin de configurer la détection automatique de l’alignement du contenu.</p></td>
</tr>
<tr class="even">
<td align="left">Rendu du texte</td>
<td align="left">Dans Windows 10, le texte des applications XAML s’affiche, dans la plupart des situations, presque deux fois plus rapidement qu’avec Windows 8.1. Dans la plupart des cas, vos applications bénéficient de cette amélioration sans être par ailleurs affectée. Ces améliorations, qui accélèrent le rendu, réduisent également de 5 % en moyenne la consommation de mémoire des applications XAML.</td>
</tr>
</tbody>
</table>

 

## Modèle d’application


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Cortana</td>
<td align="left"><p>Complétez la fonctionnalité de base de <strong>Cortana</strong> avec des commandes vocales qui lancent et exécutent une action unique dans une application externe.</p>
<p>L’intégration des fonctionnalités de base de votre application et la fourniture d’un point d’entrée central pour que l’utilisateur accomplisse la plupart des tâches sans ouvrir directement votre application permet à <strong>Cortana</strong> de devenir un intermédiaire entre votre application et l’utilisateur. Dans de nombreux cas, cela permet à l’utilisateur de gagner beaucoup de temps et d’énergie.</p>
<p>Apprenez à [integrate your app into the Cortana canvas](https://msdn.microsoft.com/library/windows/apps/xaml/dn974230). Si vous êtes à court d’idées, vous pouvez vous référer aux recommandations de conception et en matière d’expérience utilisateur spécifiques à <strong>Cortana</strong> dans [Design basics for Universal Windows apps](https://dev.windows.com/design/design-basics).</p></td>
</tr>
<tr class="even">
<td align="left">Explorateur de fichiers</td>
<td align="left">Les nouvelles méthodes [<strong>Windows.System.Launcher.LaunchFolderAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn889616) prennent en charge le lancement de l’Explorateur de fichier et l’affichage du contenu sur un dossier que vous spécifiez.</td>
</tr>
<tr class="odd">
<td align="left">Stockage partagé</td>
<td align="left">Grâce à la nouvelle classe [<strong>Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager</strong>](https://msdn.microsoft.com/library/windows/apps/dn889985) et à ses méthodes, vous pouvez partager un fichier avec une autre application en plaçant un jeton de partage lorsque vous lancez l’autre application à l’aide de l’activation par URI. L’application cible utilise le jeton pour récupérer le fichier partagé par l’application source.</td>
</tr>
<tr class="even">
<td align="left">Paramètres</td>
<td align="left"><p>Affichez des pages intégrées de paramètres en appliquant le protocole ms-settings avec la méthode [<strong>LaunchUriAsync</strong>](https://msdn.microsoft.com/library/windows/apps/hh701476). Par exemple, le code suivant affiche la page des paramètres Wi-Fi.</p>
<p><code>bool result = await Launcher.LaunchUriAsync(new Uri(&quot;ms-settings://network/wifi&quot;));</code></p>
<p>Pour obtenir une liste des pages de paramètres pouvant être affichées, voir [How to display built-in settings pages by using the ms-settings protocol](https://msdn.microsoft.com/library/windows/apps/jj207014.aspx).</p></td>
</tr>
<tr class="odd">
<td align="left">Communication entre les applications</td>
<td align="left"><p>Les nouvelles API [app-to-app communication](https://msdn.microsoft.com/library/windows/apps/xaml/dn997827) de Windows 10 permettent aux applications Windows (ainsi qu’aux applications web Windows) de se lancer les unes les autres et d’échanger des données et des fichiers.</p>
<p>Grâce à ces nouvelles API, les tâches complexes qui, auparavant, auraient obligé l’utilisateur à lancer plusieurs applications peuvent désormais être gérées de manière transparente. Ainsi, votre application peut démarrer une application de réseau social pour choisir un contact, ou une application de validation pour effectuer un processus de paiement.</p></td>
</tr>
<tr class="even">
<td align="left">Services d’application</td>
<td align="left">Un service d’application permet à une application de fournir des services à d’autres applications dans Windows 10. Un service d’application prend la forme d’une tâche en arrière-plan. Les applications de premier plan peuvent appeler un service d’application dans une autre application afin d’exécuter les tâches en arrière-plan. Pour plus d’informations de référence sur l’API de service d’application, voir [<strong>Windows.ApplicationModel.AppService</strong>](https://msdn.microsoft.com/library/windows/apps/dn921731).</td>
</tr>
<tr class="odd">
<td align="left">Manifeste du package de l’application</td>
<td align="left"><p>Les mises à jour de la référence [package manifest schema](https://msdn.microsoft.com/library/windows/apps/br211474) pour Windows 10 incluent les ajouts, les suppressions et les modifications d’éléments.</p>
<p>Pour plus d’informations de référence sur l’ensemble des éléments, attributs et types du schéma, voir [Element Hierarchy](https://msdn.microsoft.com/library/windows/apps/dn934819).</p></td>
</tr>
</tbody>
</table>

 

## Appareils


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Microsoft Surface Hub</td>
<td align="left"><p>Le Microsoft Surface Hub est un puissant appareil de collaboration en équipe et une plateforme à grand écran pour les applications Windows universelles s’exécutant en mode natif à partir de Surface Hub ou de votre appareil connecté.</p>
<p>Développez vos propres applications en fonction de votre activité, et valorisez le grand écran, les fonctionnalités de saisie tactile et manuscrite tout en profitant du nombre important de matériel intégré, comme les appareils photo et les capteurs.</p>
<p>Pour bénéficier de recommandations de conception et sur l’expérience utilisateur relatives à Surface Hub, voir [Design basics for Universal Windows apps](https://dev.windows.com/design/design-basics). Ces documents fournissent des techniques de conception réactives pour les applications Windows universelles.</p>
<p>Pour plus de détails sur la prise en charge des applications partagées collectives, voir [<strong>SharedModeSettings</strong>](https://msdn.microsoft.com/library/windows/apps/dn949019).</p>
<p>Pour plus d’informations sur l’entrée manuscrite et pour la prise en charge d’une entrée manuscrite multipoint sur le nouveau contrôle [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535), voir [<strong>Windows.UI.Input.Inking</strong>](https://msdn.microsoft.com/library/windows/apps/br208524) et [<strong>Windows.UI.Input.Inking.Core</strong>](https://msdn.microsoft.com/library/windows/apps/dn958452).</p>
<p>Pour plus d’informations sur la gestion des entrées de capteurs, voir [Integrating devices, printers, and sensors](https://msdn.microsoft.com/library/windows/apps/br229563).</p></td>
</tr>
<tr class="even">
<td align="left">Emplacement</td>
<td align="left"><p>Windows 10 lance une nouvelle méthode pour solliciter l’autorisation des utilisateurs pour accéder à leur emplacement, [<strong>RequestAccessAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn859152).</p>
<p>L’utilisateur définit la confidentialité de ses données d’emplacement avec les <strong>paramètres de confidentialité d’emplacement</strong> de l’application <strong>Paramètres</strong>. Votre application peut accéder à l’emplacement de l’utilisateur dans les cas suivants uniquement :</p>
<ul>
<li><strong>Le paramètre Emplacement de cet appareil</strong> est activé (non applicable pour la version de Windows 10 pour les téléphones).</li>
<li>Le paramètre des services de localisation <strong>Emplacement</strong> est <strong>activé</strong>.</li>
<li>Sous <strong>Choisir les applications qui peuvent utiliser votre emplacement</strong>, votre application est <strong>activée</strong>.</li>
</ul>
<p>Il est important d’appeler [<strong>RequestAccessAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn859152) avant d’accéder à l’emplacement de l’utilisateur. À ce stade, votre application doit être au premier plan et l’élément <strong>RequestAccessAsync</strong> doit être appelé à partir du thread d’interface utilisateur. Jusqu’à ce que l’utilisateur l’y autorise, votre application ne peut pas accéder aux données d’emplacement.</p></td>
</tr>
<tr class="odd">
<td align="left">AllJoyn</td>
<td align="left"><p>L’espace de noms [<strong>Windows.Devices.AllJoyn</strong>](https://msdn.microsoft.com/library/windows/apps/dn894971) Windows Runtime introduit l’implémentation par Microsoft de la structure logicielle et des services open source AllJoyn. Grâce à ces API, votre application pour appareil Windows universel peut participer avec d’autres appareils aux scénarios AllJoyn, d’Internet des objets). Pour plus de détails sur les API AllJoyn C, téléchargez la documentation à l’adresse [The AllSeen Alliance](https://allseenalliance.org/).</p>
<p>Utilisez l’outil [AllJoynCodeGen tool](https://msdn.microsoft.com/library/windows/apps/dn913809) inclus dans cette version pour générer un composant Windows à utiliser pour activer les scénarios AllJoyn dans votre application pour appareil.</p>
<div class="alert">
<strong>Remarque</strong> Windows 10 IoT Core est désormais disponible pour une nouvelle classe de petits appareils. Ainsi, vous êtes en mesure de créer des appareils dédiés à l’Internet des objets à l’aide de Windows et de Visual Studio. Pour en savoir plus sur Windows IoT, accédez à l’adresse [WindowsOnDevices.com](http://www.windowsondevices.com/).
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">API d’impression sur appareils mobiles (XAML)</td>
<td align="left">Il existe un ensemble unique, unifié d’API que vous pouvez utiliser pour imprimer à partir de vos applications UWP basées sur XAML sur de nombreuses familles d’appareils, notamment sur les appareils mobiles. Vous pouvez désormais ajouter la fonctionnalité d’impression à votre application mobile en utilisant des API familières liées à l’impression des espaces de noms [<strong>Windows.Graphics.Printing</strong>](https://msdn.microsoft.com/library/windows/apps/br226489) et [<strong>Windows.UI.Xaml.Printing</strong>](https://msdn.microsoft.com/library/windows/apps/br243325).</td>
</tr>
<tr class="odd">
<td align="left">Batterie</td>
<td align="left"><p>Grâce aux API de batterie de l’espace de noms [<strong>Windows.Devices.Power</strong>](https://msdn.microsoft.com/library/windows/apps/dn895017), votre application peut récupérer des informations sur les batteries connectées à l’appareil exécutant votre application.</p>
<p>Créez un objet [<strong>Battery</strong>](https://msdn.microsoft.com/library/windows/apps/dn895004) pour représenter un contrôleur de batterie individuel ou un groupe de contrôleurs de l’ensemble des batteries (lors d’une création par [<strong>FromIdAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn895013) ou [<strong>AggregateBattery</strong>](https://msdn.microsoft.com/library/windows/apps/dn895011), respectivement).</p>
<p>Utilisez la méthode [<strong>GetReport</strong>](https://msdn.microsoft.com/library/windows/apps/dn895015) pour renvoyer un objet [<strong>BatteryReport</strong>](https://msdn.microsoft.com/library/windows/apps/dn895005) qui indique la charge, la capacité et le statut des batteries correspondantes.</p></td>
</tr>
<tr class="even">
<td align="left">Périphériques MIDI</td>
<td align="left"><p>Le nouvel espace de noms [<strong>Windows.Devices.Midi</strong>](https://msdn.microsoft.com/library/windows/apps/dn895002) vous permet de créer les éléments suivants :</p>
<ul>
<li>Applications pouvant communiquer avec des périphériques MIDI externes.</li>
<li>Applications et périphériques externes communiquant directement avec le synthétiseur logiciel Microsoft GS MIDI.</li>
<li>Scénarios dans lesquels plusieurs clients accèdent simultanément à un port MIDI.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Prise en charge de capteurs personnalisés</td>
<td align="left">L’espace de noms [<strong>Windows.Devices.Sensors.Custom</strong>](https://msdn.microsoft.com/library/windows/apps/dn895032) permet aux développeurs de matériel de définir de nouveaux types de capteurs personnalisés, comme les capteurs CO2.</td>
</tr>
<tr class="even">
<td align="left">Émulation de carte basée sur l’hôte</td>
<td align="left"><p>L’émulation de carte basée sur l’hôte prend en charge l’implémentation de services d’émulation de carte NFC hébergés dans le système d’exploitation, ainsi que la communication avec le terminal de lecteur externe, via la radio NFC.</p>
<p>Implémentez une tâche en arrière-plan afin d’émuler une carte à puce via NFC. Pour déclencher la tâche en arrière-plan, utilisez la classe [<strong>SmartCardTrigger</strong>](https://msdn.microsoft.com/library/windows/apps/dn624853).</p>
<p>Grâce à la valeur <strong>EmulatorHostApplicationActivated</strong> de l’énumération [<strong>SmartCardTriggerType</strong>](https://msdn.microsoft.com/library/windows/apps/dn608017), votre application est informée de la survenue d’un événement HCE.</p></td>
</tr>
</tbody>
</table>

 

## Graphismes


|                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DirectX              | DirectX 12 dans Windows 10 introduit la nouvelle version de Microsoft Direct3D, l’API graphique 3D au cœur de DirectX. Les [Graphismes Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn903821) prennent en charge l’efficacité et les performances d’une API de niveau inférieur, de type console. Direct3D 12 est plus rapide et plus efficace que jamais. Cette solution permet des scènes plus riches, un plus grand nombre d’objets, des effets plus complexes et une meilleure utilisation du matériel graphique moderne.                                                                                                                                                                                                                                                                                     |
| SoftwareBitmapSource | Dans les applications Windows universelles, vous pouvez utiliser le nouveau type [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) en tant que source d’image XAML. Dès lors, vous pouvez intégrer des images non chiffrées dans la structure XAML et de les afficher immédiatement à l’écran, en contournant le décodage d’image de la structure. Vous obtenez un rendu d’image bien plus rapide, par exemple avec les photos à faible décalage directement depuis la caméra, via des décodeurs d’images personnalisés, avec la capture d’images à partir des surfaces DirectX ou en créant des images en mémoire avant de les afficher directement dans le code XAML avec une charge à faibles latence et encombrement de mémoire.                                                                                                     |
| Caméra de perspective   | Dans les applications Windows universelles, XAML possède une nouvelle API Transform3D qui prend en charge les transformations de perspective sur une arborescence (ou une scène) XAML, qui modifient l’ensemble des éléments enfants XAML en fonction de cette transformation de scène (ou caméra). Auparavant, cette opération s’effectuait à l’aide de MatrixTransform et de fonctions mathématiques complexes, mais Transform3D simplifie grandement cet effet, tout en prenant en charge son animation. Pour plus d’informations, voir la propriété [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/dn906919), [**Transform3D**](https://msdn.microsoft.com/library/windows/apps/dn914748), [**CompositeTransform3D**](https://msdn.microsoft.com/library/windows/apps/dn914714) et [**PerspectiveTransform3D**](https://msdn.microsoft.com/library/windows/apps/dn914740). |

 

## Média


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Diffusion en continu en direct</td>
<td align="left">Vous pouvez utiliser la nouvelle classe [<strong>AdaptiveMediaSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn946912)afin d’ajouter des fonctionnalités de diffusion de vidéo en continu adaptative à vos applications. Pour initialiser l’objet, pointez-le vers un fichier de manifeste de diffusion en continu. Parmi les formats de manifeste pris en charge, citons la diffusion en continu en direct HTTP et la diffusion en flux adaptatif dynamique sur HTTP. Une fois que l’objet est lié à un élément multimédia XAML, la lecture adaptative démarre. Les propriétés du flux, comme les débits disponibles, minimum et maximum peuvent être interrogés et définis au besoin.</td>
</tr>
<tr class="even">
<td align="left">Le processeur XVP (Transcode Video Processor) de Media Foundation prend en charge Media Foundation Transforms (MFT).</td>
<td align="left"><p>Les applications Windows qui utilisent Media Foundation Transforms peuvent désormais utiliser le <strong>processeur XVP (Transcode Video Processor) de Media Foundation </strong> pour convertir, mettre à l’échelle et transformer des données vidéo brutes :</p>
<p>Le nouvel attribut [MF_XVP_CALLER_ALLOCATES_OUTPUT](https://msdn.microsoft.com/library/windows/desktop/dn803919) active la sortie sur des textures allouées à l’appelant, même en mode Microsoft DirectX Video Acceleration (DXVA).</p>
<p>La nouvelle interface [<strong>IMFVideoProcessorControl2</strong>](https://msdn.microsoft.com/library/windows/desktop/dn800741) permet à votre application d’activer les effets matériels, d’interroger les effets matériels pris en charge et de remplacer l’opération de rotation exécutée par le processeur vidéo.</p></td>
</tr>
<tr class="odd">
<td align="left">Transcodage</td>
<td align="left">Grâce à la nouvelle API [<strong>MediaProcessingTrigger</strong>](https://msdn.microsoft.com/library/windows/apps/dn806005), votre application peut exécuter le transcodage multimédia dans une tâche d’arrière-plan, de manière à ce que vos opérations de transcodage puissent être poursuivies, même lorsque votre application de premier plan a été arrêtée.</td>
</tr>
<tr class="even">
<td align="left">Événements de défaillance multimédia MediaElement</td>
<td align="left">Dans les applications Windows universelles, l’élément [<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) lit le contenu contenant plusieurs flux, même si une erreur est détectée dans le décodage d’un flux. Cela est valable si le contenu multimédia comporte au moins un flux valide. Par exemple, si le flux vidéo d’un contenu comportant un flux audio et un flux vidéo est mis en échec, l’élément <strong>MediaElement</strong> joue tout de même le flux audio. L’événement[<strong>PartialMediaFailureDetected</strong>](https://msdn.microsoft.com/library/windows/apps/dn889635) vous avertit que l’un des flux contenu dans un flux n’a pas pu être décodé. Il vous indique également le type de flux mis en échec, de manière à ce que vous puissiez placer cette information dans votre interface utilisateur. Si tous les flux d’un flux multimédia sont mis en échec, l’événement [<strong>MediaFailed</strong>](https://msdn.microsoft.com/library/windows/apps/br227393) est déclenché.</td>
</tr>
<tr class="odd">
<td align="left">Prise en charge de la diffusion vidéo en continu adaptative avec MediaElement</td>
<td align="left">L’élément [<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) présente la nouvelle méthode [<strong>SetPlaybackSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn899085) pour prendre en charge la diffusion en vidéo en continu adaptative. Utilisez cette méthode pour définir votre source multimédia sur un élément [<strong>AdaptiveMediaSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn946912).</td>
</tr>
<tr class="even">
<td align="left">Diffusion multimédia avec MediaElement et Image</td>
<td align="left">Les contrôles [<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) et [<strong>Image</strong>](https://msdn.microsoft.com/library/windows/apps/br242752) présentent la nouvelle méthode [<strong>GetAsCastingSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn920012). Vous pouvez utiliser cette méthode pour envoyer du contenu par programmation à partir de n'importe quel élément multimédia ou d’image à un éventail plus large de périphériques distants comme Miracast, Bluetooth et DLNA.Cette fonctionnalité est activée automatiquement lorsque vous définissez [<strong>AreTransportControlsEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn298977) sur true, sur un objet <strong>MediaElement</strong>.</td>
</tr>
<tr class="odd">
<td align="left">Contrôles de transport multimédia pour les applications de bureau</td>
<td align="left">Grâce à l’interface [<strong>ISystemMediaTransportControls</strong>](https://msdn.microsoft.com/library/windows/desktop/dn892299) et aux API associées, les applications peuvent interagir avec les contrôles intégrés de transport multimédia du système. Ainsi, les applications réagissent aux interactions des utilisateurs avec les boutons des contrôles de transport et mettent à jour l’affichage des contrôles de transport afin d’indiquer les métadonnées relatives au contenu multimédia actuellement lu.</td>
</tr>
<tr class="even">
<td align="left">Codage et décodage JPEG à accès aléatoire</td>
<td align="left">Les nouvelles méthodes WIC [<strong>IWICJpegFrameEncode</strong>](https://msdn.microsoft.com/library/windows/desktop/dn903864) et [<strong>IWICJpegFrameDecode</strong>](https://msdn.microsoft.com/library/windows/desktop/dn903834) prennent en charge le codage et le décodage des images JPEG. Vous pouvez également activer l’indexation des données d’image, qui fournit un accès aléatoire efficace aux images de grande taille, contre un plus grand encombrement mémoire.</td>
</tr>
<tr class="odd">
<td align="left">Superpositions pour compositions multimédias</td>
<td align="left">Les nouvelles API [<strong>MediaOverlay</strong>](https://msdn.microsoft.com/library/windows/apps/dn764793) et [<strong>MediaOverlayLayer</strong>](https://msdn.microsoft.com/library/windows/apps/dn764795) permettent d’ajouter facilement plusieurs couches de contenu multimédia statique ou dynamique à une composition multimédia. L’opacité, la position et le minutage peuvent être ajustés pour chacune des couches, et vous pouvez même implémenter votre propre compositeur personnalisé pour les couches d’entrée.</td>
</tr>
<tr class="even">
<td align="left">Nouvelle infrastructure d’effets</td>
<td align="left">L’espace de noms [<strong>Windows.Media.Effects</strong>](https://msdn.microsoft.com/library/windows/apps/dn278802) fournit une infrastructure simple et intuitive pour l’ajout d’effets aux flux audio et vidéo. L’infrastructure inclut des interfaces basiques que vous pouvez implémenter pour créer des effets audio et vidéo personnalisés, à insérer dans le pipeline multimédia.</td>
</tr>
</tbody>
</table>

 

## Mise en réseau


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Sockets</td>
<td align="left"><p>Les mises à jour de socket incluent :</p>
<ul>
<li><strong>Broker de socket.</strong> Le broker de socket peut établir et fermer les connexions de socket pour le compte d’une application, quel que soit l’état de cycle de vie de l’application. Dès lors, les applications et les services qu’elles fournissent sont davantage détectables. Par exemple, par le biais d’un broker de socket, un service Win32 peut toujours accepter les connexions entrantes de socket, même en dehors de l’état d’exécution.</li>
<li><strong>Amélioration du débit.</strong> Le débit des sockets a été optimisé pour les applications utilisant l’espace de noms [<strong>Windows.Networking.Sockets</strong>](https://msdn.microsoft.com/library/windows/apps/br226960).</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Tâches post-traitement du transfert en arrière-plan</td>
<td align="left">Les nouvelles API de l’espace de noms [<strong>Windows.Networking.BackgroundTransfer</strong>](https://msdn.microsoft.com/library/windows/apps/br207242) prennent en charge l’enregistrement de groupes de tâches de post-traitement. Votre application peut réagir immédiatement à la réussite ou à l’échec des transferts en arrière-plan, même si elle n’est pas au premier plan, au lieu d’attendre que l’utilisateur redémarre l’application.</td>
</tr>
<tr class="odd">
<td align="left">Prise en charge Bluetooth pour les publicités</td>
<td align="left">Grâce à l’espace de noms [<strong>Windows.Devices.Bluetooth.Advertisement</strong>](https://msdn.microsoft.com/library/windows/apps/dn894325), votre application peut envoyer, recevoir et filtrer les publicités Bluetooth LE.</td>
</tr>
<tr class="even">
<td align="left">Mise à jour de l’API Wi-Fi Direct</td>
<td align="left"><p>Le broker d’appareil est mis à jour, pour activer le couplage avec les appareils, sans quitter l’application. Les ajouts à l’espace de noms [<strong>Windows.Devices.WiFiDirect</strong>](https://msdn.microsoft.com/library/windows/apps/dn297687) permettent par ailleurs à un appareil de se rendre détectable par d’autres appareils et de détecter les notifications de connexion entrantes.</p>
<div class="alert">
<strong>Remarque</strong> Dans cette version, les améliorations de la fonctionnalité Wi-Fi Direct ne sont pas intégrées à l’expérience utilisateur, et elles prennent en charge uniquement le couplage de réinitialisation rapide. Par ailleurs, cette version prend une seule connexion active en charge.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left">Améliorations de la prise en charge JSON</td>
<td align="left">L’espace de noms [<strong>Windows.Data.Json</strong>](https://msdn.microsoft.com/library/windows/apps/br240639) prend désormais en charge plus efficacement les définitions standard existantes et l’expérience du développeur lors de la conversion des objets JSON au cours des sessions de débogage.</td>
</tr>
</tbody>
</table>

 

## Sécurité


|                             |                                                                      |
|-----------------------------|----------------------------------------------------------------------|
| Chiffrement ECC              | Les nouvelles API de l’espace de noms [**Windows.Security.Cryptography**](https://msdn.microsoft.com/library/windows/apps/br241404) prennent en charge le chiffrement à courbe elliptique (ECC), une implémentation de chiffrement à clé publique basée sur des courbes elliptiques sur des champs finis. ECC est plus complexe mathématiquement que RSA, fournit des clés de format plus réduit, réduit la consommation de mémoire et améliore les performances. Cette solution offre aux services et aux clients Microsoft une alternative aux clés RSA et aux paramètres de courbe approuvés par le NIST. |
| Microsoft Passport          | Microsoft Passport est une autre méthode d'authentification, qui remplace les mots de passe par un chiffrement asymétrique associé à un mouvement. Grâce aux classes de l’espace de noms [**Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089), comme [**KeyCredentialManager**](https://msdn.microsoft.com/library/windows/apps/dn973043), les développeurs peuvent créer des applications en toute simplicité à l’aide de Microsoft Passport, sans la complexité du chiffrement ou de la biométrique.  |
| Microsoft Passport for Work | Microsoft Passport for Work est une autre méthode de connexion à Windows à l’aide de votre compte Azure Active Directory qui n’utilise pas de mot de passe, de carte à puce ou de cartes à puce virtuelles. Vous pouvez décider de désactiver ou d’activer ce paramètre de stratégie. |
| Broker à jetons                | Le broker à jetons est une nouvelle infrastructure d’authentification qui simplifie pour les applications la procédure de connexion aux fournisseurs d’identité en ligne, comme Facebook. Les fonctionnalités telles que la gestion du nom d’utilisateur et du mot de passe du compte, associées à une interface utilisateur rationalisée, fournissent une expérience d’authentification considérablement améliorée aux utilisateurs. |

 

## Services système


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Alimentation</td>
<td align="left"><p>Il est désormais possible de signaler à votre application de bureau Windows l’activation et la désactivation de l’économiseur de batterie. En réagissant à l’évolution des conditions d’alimentation, votre application peut étendre l’autonomie de la batterie.</p>
<p>[<strong>GUID_POWER_SAVING_STATUS</strong>](https://msdn.microsoft.com/library/windows/desktop/hh448380) : utilisez cette nouvelle propriété GUID avec la fonction [<strong>PowerSettingRegisterNotification</strong>](https://msdn.microsoft.com/library/windows/desktop/hh769082) afin d’être informé de l’activation et de la désactivation de l’économiseur de batterie.</p>
<p>[<strong>SYSTEM_POWER_STATUS</strong>](https://msdn.microsoft.com/library/windows/desktop/aa373232) : cette structure a été mise à jour pour prendre en charge l’économiseur de batterie. Le quatrième membre, <strong>SystemStatusFlag</strong> (auparavant appelé Reserved1) indique si l’économiseur de batterie est activé. Utilisez la fonction [<strong>GetSystemPowerStatus</strong>](https://msdn.microsoft.com/library/windows/desktop/aa372693) afin de récupérer un pointeur dans cette structure.</p></td>
</tr>
<tr class="even">
<td align="left">Version</td>
<td align="left"><p>Vous pouvez utiliser les [Version Helper functions](https://msdn.microsoft.com/library/windows/desktop/dn424972) pour déterminer la version du système d’exploitation. Dans Windows 10, ces fonctions d’assistance incluent une nouvelle fonction, [<strong>IsWindows10OrGreater</strong>](https://msdn.microsoft.com/library/windows/desktop/dn905474). Pour déterminer la version du système, nous vous recommandons de préférer les fonctions d’assistance aux fonctions [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451) et [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439), obsolètes. Pour plus d’informations sur la procédure de récupération de la version du système, voir [Getting the System Version](https://msdn.microsoft.com/library/windows/desktop/ms724429).</p>
<p>Si vous utilisez la fonction obsolète [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451) ou [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439) pour obtenir les informations de version dans une structure [<strong>OSVERSIONINFOEX</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724833) ou [<strong>OSVERSIONINFO</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724834), notez que le numéro de version indiqué par ces structures augmente de 6.3 pour Windows 8.1 et Windows Server 2012 R2 à 10.0 pour Windows 10. Pour plus d’informations sur les numéros de versions du système d’exploitation, voir [Operating System Version](https://msdn.microsoft.com/library/windows/desktop/ms724832).</p>
<p>Par ailleurs, il vous faudra cibler spécifiquement Windows 8.1 ou Windows 10 dans votre application pour obtenir les informations de version appropriées avec la fonction [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451) ou [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439). Pour plus d’informations sur le ciblage de votre application pour ces versions de Windows, voir [Targeting your application for Windows](https://msdn.microsoft.com/library/windows/desktop/dn481241).</p></td>
</tr>
<tr class="odd">
<td align="left">Informations utilisateur</td>
<td align="left">Les nouvelles API de l’espace de noms [<strong>Windows.System</strong>](https://msdn.microsoft.com/library/windows/apps/br241814) permettent d’accéder facilement aux informations d’accès d’un utilisateur, comme son nom d’utilisateur et sa photo de profil. Elles rendent également possibles la réponse à des événements utilisateur, comme les connexions et les déconnexions.</td>
</tr>
<tr class="even">
<td align="left">Gestion de la mémoire et profilage</td>
<td align="left">La prise en charge des API de profilage de la mémoire de [<strong>Windows.System</strong>](https://msdn.microsoft.com/library/windows/apps/br241814) a été étendue à l’ensemble des plateformes, et leur fonctionnalité générale a été enrichie à l’aide de nouvelles classes et fonctions.</td>
</tr>
</tbody>
</table>

 

## Stockage


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">API de recherche de fichiers disponibles pour Windows Phone</td>
<td align="left"><p>Comme un éditeur, vous pouvez configurer votre application pour partager un dossier de stockage avec d’autres applications, publié en ajoutant des extensions au manifeste d’application. Appelez ensuite la méthode [<strong>Windows.Storage.ApplicationData.GetPublisherCacheFolder</strong>](https://msdn.microsoft.com/library/windows/apps/dn889607) pour obtenir l’emplacement de stockage partagé.</p>
<p>Le robuste modèle de sécurité des applications Windows Runtime empêche généralement les applications de partager des données entre elles. Néanmoins, il peut s’avérer utile pour des applications du même éditeur de partager des fichiers et des paramètres en fonction des utilisateurs.</p></td>
</tr>
</tbody>
</table>

 

## Outils


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Arborescence visuelle en direct dans Visual Studio</td>
<td align="left">Visual Studio dispose d’une nouvelle fonctionnalité d’arborescence visuelle en direct. Vous pouvez l'utiliser pendant le débogage pour comprendre rapidement l'état de l'arborescence visuelle de votre application et découvrir comment les propriétés de l’élément ont été définies. Elle vous permet également de modifier les valeurs des propriétés pendant l'exécution de votre application. Dès lors, vous pouvez effectuer des ajustements et des tests sans la relancer.</td>
</tr>
<tr class="even">
<td align="left">Journalisation du suivi</td>
<td align="left"><p>[
            TraceLogging](https://msdn.microsoft.com/library/windows/desktop/dn904636) est une nouvelle API de suivi des événements dédiée aux applications en mode utilisateurs et aux pilotes en mode noyau ; elle s’appuie sur [Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803) (ETW). Cette API fournit un moyen simple d’instrumenter du code et d’inclure des données structurées avec des événements, sans nécessiter de fichier XML distinct de manifeste d’instrumentation.</p>
<p>Les API WinRT, .NET et C/C++ TraceLogging satisfont différents publics de développeurs.</p></td>
</tr>
</tbody>
</table>

 

## Expérience utilisateur


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Reconnaissance vocale</td>
<td align="left">La reconnaissance vocale continue pour les scénarios de dictée de longue durée est désormais prise en charge par la plateforme Windows universelle. Pour plus d’informations sur l’activation de la dictée continue, consultez les documents sur l’interaction vocale.</td>
</tr>
<tr class="even">
<td align="left">Fonctionnalités de glisser-déplacer entre différentes plateformes applicatives</td>
<td align="left"><p>Les nouveaux espaces de noms [<strong>Windows.ApplicationModel.DataTransfer.DragDrop</strong>](https://msdn.microsoft.com/library/windows/apps/dn894216) apportent une fonctionnalité de glisser-déplacer aux applications Windows universelles. Auparavant, les scénarios communs de glisser-déplacer pour les programmes de bureau, comme par exemple le déplacement d’un document d’un dossier vers un message Outlook en tant que pièce jointe, n’étaient pas pris en charge avec les applications Windows universelles. À l’aide de ces nouvelles API, votre application donne les moyens aux utilisateurs de déplacer facilement des données entre différentes applications Windows universelles et le bureau.</p>
<p>Pour la prise en charge de la fonctionnalité glisser-déplacer entre les applications, ces nouvelles API ont été ajoutées au code XAML :</p>
<ul>
<li>[<strong>ListViewBase.DragItemsCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn831959)</li>
<li>[<strong>UIElement</strong>](https://msdn.microsoft.com/library/windows/apps/br208911) : [<strong>CanDrag</strong>](https://msdn.microsoft.com/library/windows/apps/dn903558), [<strong>DragStarting</strong>](https://msdn.microsoft.com/library/windows/apps/dn903560), [<strong>StartDragAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn903562), [<strong>DropCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn903561)</li>
<li>[<strong>DragOperationDeferral</strong>](https://msdn.microsoft.com/library/windows/apps/dn831917), [<strong>DragUI</strong>](https://msdn.microsoft.com/library/windows/apps/dn985714), [<strong>DragUIOverride</strong>](https://msdn.microsoft.com/library/windows/apps/dn985715)</li>
<li>[<strong>DragEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/br242372): [<strong>AcceptedOperation</strong>](https://msdn.microsoft.com/library/windows/apps/dn831912), [<strong>DataView</strong>](https://msdn.microsoft.com/library/windows/apps/dn831913), [<strong>DragUIOverride</strong>](https://msdn.microsoft.com/library/windows/apps/dn985710), [<strong>GetDeferral</strong>](https://msdn.microsoft.com/library/windows/apps/dn831914), [<strong>Modifiers</strong>](https://msdn.microsoft.com/library/windows/apps/dn831915)</li>
<li>[<strong>DragItemsCompletedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn831953), [<strong>DropCompletedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn903549), [<strong>DragStartingEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn903540)</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Barres de titre des fenêtres personnalisées</td>
<td align="left">Pour les applications UWP utilisées avec la famille d’appareils de bureau, vous pouvez désormais utiliser la classe [<strong>ApplicationViewTitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn906115) avec la propriété [<strong>ApplicationView.TitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn906131) et la méthode [<strong>Window.SetTitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn965560) afin de remplacer le contenu par défaut de la barre de titre Windows par votre propre contenu XAML personnalisé. Votre code XAML étant considéré comme un « chrome système », Windows traite des événements d’entrée en lieu et place de votre application. Dès lors, l’utilisateur peut toujours déplacer et redimensionner la fenêtre, même en cliquant sur le contenu de la barre de titre personnalisée.</td>
</tr>
</tbody>
</table>

 

## Web


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Internet Explorer</td>
<td align="left"><p>Internet Explorer lance le mode edge : un nouveau mode de document dynamique conçu pour une interopérabilité maximale avec d’autres navigateurs modernes et le contenu web contemporain. Ce mode expérimental est progressivement déployé sur un ensemble d’utilisateurs de Windows 10, sélectionné aléatoirement. Vous pouvez activer ou désactiver manuellement le mode Edge par le biais du nouveau mécanisme <strong>about:flags</strong> d’IE. Pour plus d’informations, voir :</p>
<ul>
<li>[
            Living on the Edge – our next step in helping the web just work](http://blogs.msdn.com/b/ie/archive/2014/11/11/living-on-the-edge-our-next-step-in-interoperability.aspx).</li>
<li>[
            The Internet Explorer for Windows 10 Developer Guide](https://dev.windows.com/microsoft-edge/).</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Navigation en mode Edge WebView</td>
<td align="left">Le contrôle [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) utilise le même moteur de rendu que le nouveau navigateur Edge. Vous bénéficiez du mode de rendu HTML le plus précis et le plus conforme aux normes.</td>
</tr>
<tr class="odd">
<td align="left">WebView hors thread</td>
<td align="left">Vous pouvez spécifier un contrôle [<strong>WebView.ExecutionMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn932034) afin d’activer le traitement et l’affichage de contenu web sur un thread séparé en arrière-plan. Cette action peut améliorer les performances dans certains scénarios spécifiques.</td>
</tr>
<tr class="even">
<td align="left">Événement WebView.UnsupportedUriSchemeIdentified</td>
<td align="left"><p>Le nouvel événement [<strong>WebView.UnsupportedUriSchemeIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn974400) vous permet de définir comment votre application traite un schéma d’URI non pris en charge. Vous pouvez gérer cet événement pour que votre application prenne en charge le traitement personnalisé des schémas d’URI non pris en charge.</p>
<p>Pour le contrôle HTML WebView, voir l’événement [<strong>MSWebViewUnsupportedUriSchemeIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn803906.aspx).</p></td>
</tr>
<tr class="odd">
<td align="left">Événement WebView.NewWindowRequested</td>
<td align="left"><p>Le nouvel événement [<strong>WebView.NewWindowRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn974397) vous permet de répondre lorsqu’un script d’un contrôle WebView sollicite une nouvelle fenêtre de navigateur.</p>
<p>Pour le contrôle HTML WebView, voir l’événement [<strong>MSWebViewNewWindowRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn806030).</p></td>
</tr>
<tr class="even">
<td align="left">Événement WebView.PermissionRequested</td>
<td align="left"><p>Grâce au nouvel événement [<strong>WebView.PermissionRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn974398), le contenu WebView peut valoriser de nouvelles API HTML5 riches qui nécessitent une autorisation spéciale de l’utilisateur, comme la géolocalisation.</p>
<p>Pour le contrôle HTML WebView, voir l’événement [<strong>MSWebViewPermissionRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn806030.aspx).</p></td>
</tr>
<tr class="odd">
<td align="left">Événement WebView.UnviewableContentIdentified</td>
<td align="left"><p>Le nouvel événement [<strong>WebView.UnviewableContentIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn299351) vous permet de répondre lorsque le contrôle [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) navigue vers le contenu hors connexion, comme un fichier PDF ou un document Office.</p>
<p>Pour les contrôles HTML WebView, voir l’événement [<strong>MSWebViewUnviewableContentIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn609716).</p></td>
</tr>
<tr class="even">
<td align="left">Méthode WebView.AddWebAllowedObject</td>
<td align="left"><p>Vous pouvez appeler la nouvelle méthode [<strong>WebView.AddWebAllowedObject</strong>](https://msdn.microsoft.com/library/windows/apps/dn903993) pour injecter un objet WinRT dans un code XAML [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702), puis appeler ses fonctions à partir d’un contenu Javascript approuvé hébergé dans ce contrôle <strong>WebView</strong>. Par exemple, le contenu web peut afficher les notifications système en demandant que son application parente appelle l’API WinRT [<strong>ToastNotificationManager</strong>](https://msdn.microsoft.com/library/windows/apps/br208642).</p>
<p>Pour le contrôle HTML [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702), voir la méthode [<strong>addWebAllowedObject</strong>](https://msdn.microsoft.com/library/windows/apps/dn926632).</p></td>
</tr>
<tr class="odd">
<td align="left">Méthode WebView.ClearTemporaryWebDataAync</td>
<td align="left">Lorsqu’un utilisateur interagit avec le contenu web à l’intérieur d’un code XAML [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702), le contrôle <strong>WebView</strong> met en cache les données associées à cette session utilisateur. Pour effacer ce cache, appelez la nouvelle méthode [<strong>ClearTemporaryWebDataAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn974394). Par exemple, vous pouvez effacer le cache lorsqu’un utilisateur se déconnecte de l’application pour qu’aucun autre utilisateur n’ait accès aux données de la session précédente.</td>
</tr>
</tbody>
</table>



<!--HONumber=Mar16_HO5-->


