---
author: GrantMeStrength
Description: Compare platform features between iOS, Android, and Windows 10.
Search.Product: eADQiWindows 10XVcnh
title: Mappage de concepts d’applications Windows pour développeurs iOS et Android
ms.author: jken
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: 082736c8-2ac3-41b3-b246-e705edc23f34
ms.localizationpriority: medium
ms.openlocfilehash: f7e211ebfa28421340e716c0176cab80a9511671
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7419316"
---
#<a name="windows-apps-concept-mapping-for-android-and-ios-developers"></a>Mappage de concepts d’applications Windows pour développeurs iOS et Android

Si vous êtes un développeur doté de compétences relatives aux systèmes d’exploitation Android ou iOS et au code et que vous souhaitez migrer vers Windows10 et la plateforme Windows universelle (UWP), cette ressource vous permettra de mapper les fonctionnalités de la plateforme, et vos connaissances, entre les trois plateformes.

Voir également le contenu de portage dans [Migrer d’iOS vers UWP](ios-to-uwp-root.md). Ce document est également disponible en [téléchargement](https://www.microsoft.com/download/details.aspx?id=52041).

## <a name="user-interface-ui"></a>Interface utilisateur (UI)


<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Plateforme Windows universelle Windows10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Langage de conception.</strong><br><br>Ensemble de conventions qui prévoient la manière dont les applications sur la plateforme doivent se présenter et se comporter.</td>
<td align="left">Les <strong>recommandations en matière de conception d’applications Android</strong> fournissent un langage visuel à suivre par les concepteurs et développeurs Android.</td>
<td align="left">Les <strong>recommandations en matière d’interface utilisateur</strong> fournissent des conseils aux concepteurs et développeurs iOS.</td>
<td align="left"><a href="https://dev.windows.com/design"><strong>Conception et interface utilisateur</strong></a> vous montre comment créer une application parfaitement adaptée à tous les appareils Windows10. Vous y trouverez des notions de base en matière de conception d’interface utilisateur (UI), des techniques de conception réactives et une liste complète de recommandations détaillées.<br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Langage de balisage d’interface utilisateur.</strong> <br><br>Langage de balisage effectuant le rendu et décrivant une interface utilisateur et ses composants. Chaque plateforme fournit un éditeur à des fins de modification visuelle et du balisage.<br/></td>
<td align="left">Modification des <strong>dispositions XML</strong> à l’aide d’<strong>AndroidStudio</strong> ou <strong>Eclipse</strong>.</td>
<td align="left">Modification de <strong>XIB</strong> et <strong>Storyboards</strong> à l’aide d’<strong>Interface Builder</strong> dans Xcode.</td>
<td align="left">Modification de <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt185595.aspx">XAML</a></strong> à l’aide de <strong><a href="https://www.visualstudio.com/">Microsoft VisualStudio</a></strong> et <strong><a href="https://msdn.microsoft.com/library/jj171012.aspx">Blend pour VisualStudio</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228259.aspx">Plateforme XAML</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228349.aspx">Créer une interface utilisateur avec XAML</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228350.aspx">Définir des dispositions avec XAML</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Contrôles d’interface utilisateur intégrés.</strong> <br><br>Éléments d’interface utilisateur réutilisables fournis par la plateforme, tels que des boutons, des contrôles de liste et des contrôles de texte.</td>
<td align="left">Classes <strong>view</strong> et <strong>view group</strong> prédéfinies désignées sous les termes widgets, dispositions, champs de texte, conteneurs, contrôles date/heure et contrôles d’expert.</td>
<td align="left"><strong>Vues</strong> et <strong>contrôles</strong> de la bibliothèque d’objets Xcode et répertoriés dans le catalogue d’interface utilisateur UIKit. Les vues comprennent les vues d’images, les vues de sélecteur et les vues de défilement. Les contrôles comprennent les boutons, les sélecteurs de dates et les champs de texte.</td>
<td align="left">La plateforme XAML propose un vaste ensemble de <strong>contrôles intégrés</strong>, tels que des boutons, des contrôles de liste, des panneaux, des contrôles de texte, des barres de commandes, des sélecteurs, du contenu multimédia et l’entrée manuscrite.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228345.aspx">Ajouter des contrôles et gérer les événements</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Gestion des événements de contrôle.</strong> <br><br>Définition de la logique qui s’exécute lorsque des événements sont déclenchés dans des contrôles d’interface utilisateur.</td>
<td align="left">Des <strong>gestionnaires d’événements</strong> et des <strong>détecteurs d’événements</strong> sont ajoutés dans le langage XML ou par programme.</td>
<td align="left">Les contrôles envoient des messages d’<strong>action</strong> aux <strong>cibles</strong>.</td>
<td align="left">Vous pouvez définir des méthodes pour gérer les événements d’un contrôle XAML dans un <strong>fichier code-behind</strong> attaché à la page XAML. Des <strong>gestionnaires d’événements</strong> sont toujours écrits dans le code. Toutefois, vous pouvez associer ces gestionnaires à des événements dans le balisage XAML ou le code.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228345.aspx">Ajouter des contrôles et gérer les événements</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt185584.aspx">Vue d’ensemble des événements et des événements routés</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Liaison de données.</strong> <br><br>Modèle de conception logicielle qui permet à l’interface utilisateur de votre application d’afficher des données et éventuellement de rester synchronisé avec celles-ci.</td>
<td align="left">Une <strong>bibliothèque de liaison de données</strong> est fournie, bien qu’elle soit toujours en version bêta.</td>
<td align="left">Il n’existe aucun système de liaison intégré sur iOS. Il est possible de se baser sur <strong>Key-value observing</strong> pour effectuer des liaisons de données par l’utilisation d’une bibliothèque tierce ou l’écriture de code supplémentaire. Les contrôles utilisent une approche délégué/rappel pour l’obtention des données.</td>
<td align="left">La plateforme UWP traite pour vous la <strong>liaison de données</strong>. Vous utilisez l’extension de balisage <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt204783.aspx">{x:Bind}</a></strong> pour tirer parti de la liaison de données hautes performances ou <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt204782.aspx">{Binding}</a></strong> pour exploiter d’autres fonctionnalités. Il suffit alors de configurer votre liaison pour choisir si la plateforme utilise une <strong>liaison à sens unique</strong> pour afficher les valeurs provenant d’une source de données dans votre interface utilisateur, ou si elle observe ces valeurs et met à jour votre interface utilisateur lors du passage en <strong>liaison bidirectionnelle</strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt210947.aspx">Liaison de données</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Automatisation de l’interface utilisateur.</strong> <br><br>Accès par programmation aux éléments d’interface utilisateur, rendant les applications accessibles aux produits de technologie d’assistance et permettant aux scripts automatisés d’interagir avec votre interface utilisateur.</td>
<td align="left">Les valeurs <strong>Text labels</strong>, <strong>contentDescription</strong> et <strong>hint</strong> permettent de garantir que les éléments d’interface utilisateur peuvent être trouvés par le biais de l’automatisation. Android Studio vous permet d’écrire des tests d’interface utilisateur à l’aide des infrastructures de test <strong>UI Automator</strong> et <strong>Espresso</strong>.</td>
<td align="left">L’<strong>instrument d’automatisation</strong> vous permet d’écrire des scripts de test d’interface utilisateur automatisés qui identifient les éléments à l’aide de paramètres d’<strong>accessibilité</strong> ou de la position de l’élément dans la <strong>hiérarchie d’éléments</strong>.</td>
<td align="left">Vous obtenez un accès par programmation aux éléments d’interface utilisateur intégrés dans UWP fournie avec <strong><a href="https://msdn.microsoft.com/library/windows/apps/ee684076.aspx">UI Automation</a></strong>.<br/>Les <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt297667.aspx">homologues d’automatisation personnalisés</a></strong> vous permettent de fournir la prise en charge de l’automatisation pour vos propres classes d’interface utilisateur personnalisées. Le <strong><a href="https://msdn.microsoft.com/library/dd286726.aspx#VerifyingCodeUsingCUITCreate">projet de test codé de l’interface utilisateur</a></strong> dans Visual Studio vous permet de tester automatiquement l’ensemble de votre application par le biais de l’interface utilisateur ou de tester l’interface utilisateur de manière isolée.</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Modification de l’apparence d’un contrôle.</strong> <br><br>Modification de la taille, de la couleur et d’autres attributs.</td>
<td align="left">Les contrôles possèdent des <strong>propriétés</strong> qui peuvent être modifiées à l’aide de l’outil de conception, dans le balisage XML ou par programmation.</td>
<td align="left">Les contrôles possèdent des <strong>attributs</strong> que vous pouvez modifier à l’aide de l’<strong>Attributes Inspector</strong> dans Interface Builder ou par programmation.</td>
<td align="left">Vous pouvez modifier les <strong>propriétés</strong> des contrôles dans le balisage XAML ou par programmation, à l’aide de Visual Studio et de Blend pour Visual Studio.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228345.aspx">Ajouter des contrôles et gérer les événements</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Styles visuels réutilisables.</strong> <br><br>Application de modifications visuelles à un certain nombre de contrôles, dans un format réutilisable.</td>
<td align="left">Les <strong>styles XML</strong> sont des ensembles de propriétés appliqués à un ou plusieurs contrôles.</td>
<td align="left">iOS ne prend pas en charge les styles visuels réutilisables fournis, toutefois le protocole UIAppearance permet à plusieurs contrôles de partager des attributs communs.</td>
<td align="left">Vous pouvez créer des <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx">styles</a></strong> réutilisables, qui peuvent être appliqués à plusieurs contrôles et stockés dans un <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.resourcedictionary.aspx">ResourceDictionary</a></strong> pour faciliter leur réutilisation.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465381.aspx">Démarrage rapide: application de styles aux contrôles (XAML)</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Modification de la structure visuelle des contrôles.</strong> <br><br>Personnalisation de la structure visuelle d’un contrôle au-delà de la simple modification des propriétés ou des attributs, par exemple, déplacement d’un texte de case à cocher sous cette dernière.</td>
<td align="left">Il n’existe aucune méthode de modification simple de la structure visuelle des contrôles dans Android.</td>
<td align="left">Il n’existe aucune méthode de modification simple de la structure visuelle des contrôles dans iOS.</td>
<td align="left">Pour personnaliser la structure visuelle d’un contrôle, vous pouvez copier et modifier son <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.controltemplate.aspx">modèle de contrôle</a></strong> dans le balisage XAML.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465374.aspx">Démarrage rapide: modèles de contrôle (XAML)</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Mouvements tactiles intégrés.</strong> <br><br>Prise en charge personnalisée de la fonctionnalité tactile par la gestion des événements de mouvement abstraits de niveau élevé, tels que l’appui et le double appui dans les vues et les contrôles.</td>
<td align="left">Les <strong>détecteurs de mouvement</strong> détectent les mouvements tactiles de base, notamment le défilement, l’appui prolongé, l’appui simple, le double appui et le glissement rapide.</td>
<td align="left">L’infrastructure UIKit fournit des <strong>outils de reconnaissance de mouvement</strong> qui détectent les mouvements tactiles, tels que l’appui, le pincement, le mouvement panoramique, le balayage, la rotation et l’appui prolongé.</td>
<td align="left">Les <strong>éléments d’interface utilisateur</strong> vous permettent de gérer les <strong>événements de mouvement statique</strong>, notamment l’appui simple, le double appui, l’appui prolongé du bouton droit, ainsi que les <strong>événements de mouvement de manipulation</strong>, notamment le glissement, le balayage, la rotation, le pincement et l’étirement. Les événements de mouvement sont des <strong>événements routés</strong> et peuvent être gérés par des objets parent contenant l’élément d’interface utilisateur enfant.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185617.aspx">Interactions tactiles</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185599.aspx#gestures__manipulations__and_interactions">Interactions utilisateur personnalisées (mouvements, manipulations et interactions)</a></td>
</tr>
</tbody>
</table>
<h2 id="navigation-and-app-structure">Navigation et structure de l’application</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Plateforme Windows universelle Windows10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Dispositions.</strong> <br><br>La disposition définit la structure de l’interface utilisateur.</td>
<td align="left">La disposition se compose de <strong>groupes d’affichages</strong>, tels que <strong>LinearLayout</strong> et <strong>RelativeLayout</strong> qui peuvent imbriquer d’autres groupes d’affichages ou vues.</td>
<td align="left">La disposition est constituée d’un <strong>UIViewController</strong> contenant des <strong>UIView</strong> qui peuvent être imbriqués.</td>
<td align="left">XAML fournissant un système de disposition flexible composé de <strong>classes de panneau de disposition</strong>, telles que <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.canvas.aspx">Canvas</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.grid.aspx">Grid</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.relativepanel.aspx">RelativePanel</a></strong> et <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx">StackPanel</a></strong> pour les dispositions statiques et réactives. Les <strong><a href="https://msdn.microsoft.com/library/ms171352.aspx">propriétés</a></strong> permettent de contrôler la taille et la position des éléments.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx">Définir des dispositions avec XAML</a><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Navigation parmi les homologues.</strong> <br><br>Présentation à l’utilisateur de méthodes de navigation entre les pages d’importance hiérarchique équivalente.</td>
<td align="left">Les <strong>onglets</strong>, <strong>vues à balayage</strong> et <strong>panneaux de navigation</strong> permettent une <strong>navigation latérale</strong>.</td>
<td align="left">Les <strong>contrôleurs de la barre d’onglets</strong>, <strong>les contrôleurs du mode fractionné</strong> et les <strong>contrôleurs d’affichage de page</strong> permettent de naviguer entre les vues de hiérarchie équivalente.</td>
<td align="left">Vous pouvez afficher une liste permanente de liens/onglets au-dessus du contenu à l’aide d’<strong><a href="https://msdn.microsoft.com/library/windows/apps/dn997788.aspx">onglets/de pivots</a></strong>. Le <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn997787.aspx">mode fractionné/volet de navigation</a></strong> vous permet d’afficher une liste de liens avec le contenu.<br/><br/><a href="https://msdn.microsoft.com/windows/uwp/layout/navigation-basics">Navigation</a><br/><br/><a href="https://msdn.microsoft.com/windows/uwp/layout/navigate-between-two-pages">Naviguer entre deux pages</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Navigation hiérarchique.</strong> <br><br>Navigation entre les pages parent et enfant d’une hiérarchie.</td>
<td align="left">Les <strong>listes</strong>, <strong>listes de quadrillage</strong>, <strong>boutons</strong> et autres contrôles permettent une <strong>navigation par descendants</strong> lorsqu’ils sont utilisés dans l’<strong>intention</strong> de charger d’autres <strong>activités</strong>.</td>
<td align="left">Les <strong>contrôleurs de navigation</strong> permettent aux utilisateurs de naviguer entre les niveaux d’une hiérarchie.</td>
<td align="left">Les <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn449149.aspx">concentrateurs</a></strong> permettent de présenter à l’utilisateur un aperçu du contenu qui peut être sélectionné pour accéder aux pages enfants. <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn997765.aspx">Maître/détails</a></strong> permet aux utilisateurs de faire une sélection parmi une liste de résumés d’éléments qui s’affichent en regard de la section correspondante relative aux détails.<br/><br/><a href="https://msdn.microsoft.com/windows/uwp/layout/navigation-basics">Navigation</a><br/><br/><a href="https://msdn.microsoft.com/windows/uwp/layout/navigate-between-two-pages">Naviguer entre deux pages</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Navigation via le bouton Précédent.</strong> <br><br>Retour à une application.</td>
<td align="left">Les boutons <strong>précédent</strong> et <strong>haut</strong> situés dans la barre d’actions permettent une navigation par <strong>ancêtres</strong> et <strong>temporelle</strong> à l’aide de la <strong>pile Back</strong>.</td>
<td align="left">Le <strong>contrôleur de navigation</strong> peut se voir ajouter un bouton Précédent.<br/></td>
<td align="left">Vous pouvez facilement gérer les appuis sur les boutons matériels ou logiciels à l’aide de la <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.frame.backstack.aspx">propriété pile Back</a></strong> qui permet aux utilisateurs de traverser l’<strong>historique de navigation</strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt465734.aspx">Navigation via le bouton Précédent</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Écran de démarrage.</strong> <br><br>Affiche d’une image au lancement de l’application, principalement utilisé pour la personnalisation.</td>
<td align="left">Les écrans de démarrage ne sont pas fournis par défaut et sont implémentés en modifiant l’arrière-plan du <strong>thème des premières activités</strong>.</td>
<td align="left">Les applications doivent disposer d’une <strong>image de lancement statique</strong> ou d’un <strong>fichier de lancement XIB/de table de montage séquentiel</strong>.</td>
<td align="left">Vous créez un écran de démarrage à l’aide d’une <strong>image</strong> et d’un arrière-plan coloré. <a href="https://msdn.microsoft.com/library/windows/apps/mt187309.aspx">Vous pouvez prolonger l’affichage de l’écran de démarrage</a>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187306.aspx">Ajouter un écran de démarrage</a></td>
</tr>
</tbody>
</table>
<h2 id="custom-inputs">Entrées personnalisées</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Plateforme Windows universelle Windows10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Voix.</strong> <br><br>Reconnaissance vocale pour la saisie vocale et fonctionnalités vocales supplémentaires.</td>
<td align="left">La saisie vocale peut être assurée par toute application implémentant une classe <strong>RecognizerIntent</strong>, tel que la <strong>recherche vocale Google</strong>. La classe <strong>SpeechRecognizer</strong> permet aux applications d’utiliser l’API de reconnaissance vocale de Google.</td>
<td align="left">Les applications peuvent utiliser la classe <strong>SFSpeechRecognizer</strong> pour implémenter la reconnaissance et la saisie vocales.</td>
<td align="left">Vous pouvez utiliser l’API de <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185615.aspx">reconnaissance vocale</a></strong> pour interagir avec votre application au premier plan. Vous pouvez utiliser des <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185598.aspx">interactions Cortana</a></strong> vocales pour lancer des applications au premier plan ou en arrière-plan et pour interagir avec des applications en arrière-plan.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185614.aspx">Interactions vocales</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Entrées utilisateur personnalisées.</strong> <br><br>Gestion du clavier, de la souris, du stylet et d’autres entrées.</td>
<td align="left">Prise en charge de l’interaction via la fonctionnalité <strong>tactile</strong>, le <strong>pavé tactile</strong>, le <strong>stylet</strong>, la <strong>souris</strong> et le <strong>clavier</strong>. Les mouvements et entrées sont considérés comme des mouvements tactiles, toutefois, il est possible de détecter plus d’informations concernant le <strong>périphérique d’entrée</strong>.</td>
<td align="left">La prise en charge de la fonctionnalité <strong>tactile</strong>, de l’<strong>Apple Pencil</strong> et du <strong>clavier</strong> est assurée.</td>
<td align="left">Vous découvrirez une prise en charge pour une large gamme d’interactions, notamment <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185617.aspx">tactiles</a></strong>, avec le<strong><a href="https://msdn.microsoft.com/library/windows/apps/mt187313.aspx"> pavé tactile</a></strong>, le <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt187311.aspx">stylet</a></strong> avec entrée manuscrite, la <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt187308.aspx">souris</a></strong> et le <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185607.aspx">clavier</a></strong>. Vos applications peuvent gérer les données sans avoir besoin de savoir quel périphérique d’entrée a été utilisé, et les données de périphérique d’entrée brutes sont accessibles si nécessaire.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt404610.aspx">Gérer les entrées du pointeur</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185599.aspx">Interactions utilisateur personnalisées</a></td>
</tr>
</tbody>
</table>
<h2 id="data">Données</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Plateforme Windows universelle Windows10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Données d’application locales.</strong> <br><br>Stockage local des paramètres et des fichiers associés à votre application.</td>
<td align="left">Les fichiers locaux peuvent être enregistrés à l’aide de <strong>openFileOutput</strong> et <strong>openFileInput</strong>. Les paramètres d’un <strong>fichier de préférences partagé</strong> sont accessibles à l’aide de <strong>getSharedPreferences</strong>.</td>
<td align="left">Les fichiers locaux peuvent être stockés dans le répertoire <strong>prise en charge de l’application</strong>, accessible via la classe <strong>NSFileManager</strong>. Les paramètres des fichiers de <strong>préférences</strong> sont accessibles via la classe <strong>NSUserDefaults</strong>.</td>
<td align="left">Les classes <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/br230562.aspx">Windows.Storage</a></strong> gèrent pour vous le stockage des données locales de manière uniformisée. Vous stockez les paramètres sous forme d’objet <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.storage.applicationdatacontainer.aspx">ApplicationDataContainer</a></strong>, accessible via la propriété <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.storage.applicationdata.localsettings.aspx">ApplicationData.LocalSettings</a></strong>. Vous stockez les fichiers sous forme d’objet <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefolder.aspx">StorageFolder</a></strong>, accessible via la propriété <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.storage.applicationdata.localfolder.aspx">ApplicationData.LocalFolder</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt299098.aspx">Stocker et récupérer des paramètres et autres données d’application</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Stockage de base de données local.</strong> <br><br>Stockage des données d’application dans une base de données relationnelle, avec ORM, le cas échéant.</td>
<td align="left">La base de données <strong>SQLite</strong> est fournie. Aucun ORM n’est intégré. Les requêtes SQL sont exécutées à l’aide de la classe <strong>SQLiteDatabase</strong>.</td>
<td align="left">La base de données <strong>SQLite</strong> est fournie. <strong>CoreData</strong> constitue l’infrastructure graphique d’objet intégrée qui peut être utilisée avec SQLite et peut proposer des fonctionnalités comparables à un ORM.</td>
<td align="left">Vous pouvez stocker des données à l’aide <strong>SQLite</strong>. <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592863.aspx">Entity Framework</a></strong> est un ORM intégré qui permet d’éviter d’avoir recours à l’écriture d’une grande quantité de code d’accès aux données et permet d’interroger facilement la base de données sans écrire de code SQL. Vous pouvez exécuter des requêtes SQL directement avec la <a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592864.aspx">bibliothèque SQLite</a>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592862.aspx">Accès aux données.</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Bibliothèques HTTP pour l’accès à REST.</strong> <br><br>Bibliothèques intégrées qui vous permettent de communiquer avec les services web et les serveurs web à l’aide du protocole HTTP(S).<br/></td>
<td align="left">Bibliothèques HTTP <strong>HttpURLConnection</strong> et <strong>Volley</strong>.</td>
<td align="left"><strong>NSURLSession</strong>, <strong>NSURLConnection</strong> et <strong>NSURLDownload</strong>.</td>
<td align="left">Vous pouvez utiliser l’API <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httpclient">HttpClient</a></strong> intégrée pour accéder à des fonctionnalités HTTP communes, y compris à GET, DELETE, PUT, POST, aux modèles d’authentification courants, à SSL, aux cookies et aux informations de progression.</td>
</tr>
<tr class="even">
<td align="left"><strong>Services de sauvegarde dans le cloud.</strong> <br><br>Services de sauvegarde fournis par la plate-forme pour les données d’application.</td>
<td align="left">Le <strong>gestionnaire de sauvegarde</strong> d’Android gère la sauvegarde des données d’application dans le <strong>service de sauvegarde Android</strong> de Google.</td>
<td align="left">La <strong>sauvegarde sur iCloud</strong> peut être configurée par un utilisateur pour gérer ses sauvegardes, y compris les données d’application. Les applications utilisant des <strong>données principales</strong> compatibles iCloud, le <strong>magasin clé-valeur iCloud</strong> et le <strong>stockage de documents iCloud</strong>.</td>
<td align="left">Les données d’application que vous stockez à l’aide des <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.aspx">API ApplicationData</a></strong> itinérantes <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.roamingfolder.aspx">RoamingFolder</a></strong> et <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.roamingsettings.aspx"><strong>RoamingSettings</strong></a>) seront automatiquement synchronisées sur le cloud et les autres appareils de l’utilisateur. La synchronisation s’effectue par le biais du compte Microsoft de l’utilisateur.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/hh465094.aspx">Recommandations en matière de données d’application itinérantes</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Téléchargements de fichiers sur HTTP.</strong> <br><br>Téléchargement de fichiers de petite et grande taille sur HTTP.</td>
<td align="left"><strong>URLConnection</strong> et <strong>HTTPURLConnection</strong> sont utilisés pour effectuer des téléchargements sur HTTP et FTP. Il est également possible d’utiliser le <strong>gestionnaire de téléchargement</strong> système à télécharger en arrière-plan.</td>
<td align="left"><strong>NSURLSession</strong> et <strong>NSURLConnection</strong> peuvent être utilisés pour télécharger des fichiers sur HTTP et FTP.</td>
<td align="left">L’<strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.networking.backgroundtransfer.aspx">API de transfert en arrière-plan</a></strong> vous permet de transférer de manière fiable des fichiers sur HTTP(S) et FTP en prenant en compte la suspension d’une application, la perte de connectivité et le réglage en fonction de l’autonomie de la batterie et de la connectivité. Vous pouvez également utiliser <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.web.http.httpclient.aspx">HttpClient</a></strong> qui est idéal pour les fichiers de plus petite taille.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280235.aspx">Quelle technologie de réseau?</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280377.aspx">Transferts en arrière-plan</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Sockets.</strong> <br><br>Création de datagrammes UDP et de sockets TCP de faible niveau pour communiquer avec d’autres appareils à l’aide de votre propre protocole.</td>
<td align="left">La classe <strong>Socket</strong> fournit des sockets TCP. La classe <strong>DatagramSocket</strong> fournit un socket UDP.</td>
<td align="left"><strong>NSStream</strong> et <strong>CFStream</strong> fournissent des sockets TCP. <strong>CFSocket</strong> fournit des sockets UDP.</td>
<td align="left">Vous pouvez utiliser la classe <strong><a href="https://msdn.microsoft.com/library/windows/apps/br241319">DatagramSocket</a></strong> pour communiquer à l’aide d’un socket de datagramme UDP et la classe <strong><a href="https://msdn.microsoft.com/library/windows/apps/br226882">StreamSocket</a></strong> pour communiquer sur TCP ou Bluetooth RFCOMM.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280233.aspx">Notions de base en matière de réseau</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280235.aspx">Quelle technologie de réseau?</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280234.aspx">Vue d’ensemble de sockets</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>WebSockets.</strong> <br><br>Assurer une communication bidirectionnelle entre un client et un serveur, ce qui permet le transfert de données en temps réel.</td>
<td align="left">Aucune bibliothèque WebSockets intégrée n’existe sur Android.</td>
<td align="left">Aucune bibliothèque WebSockets intégrée n’existe sur iOS.</td>
<td align="left">Des connexions sécurisées aux serveurs prenant en charge WebSockets peuvent être mises en place à l’aide de la classe <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.networking.sockets.messagewebsocket.aspx">MessageWebSocket</a></strong> pour les messages de plus petite taille avec des notifications de réception et <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.networking.sockets.streamwebsocket.aspx">StreamWebSocket</a></strong> pour les transferts de fichiers binaires de plus grande taille pouvant être lus en sections.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280233.aspx">Notions de base en matière de réseau</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280235.aspx">Quelle technologie de réseau?</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt186447.aspx">Vue d’ensemble de WebSockets</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Bibliothèques OAuth.</strong> <br><br>Bibliothèques OAuth permettant d’accéder à des fournisseurs OAuth tiers et à toute gestion de compte intégrée à la plateforme.</td>
<td align="left">Aucune bibliothèque OAuth générique n’est fournie. La classe <strong>GoogleAuthUtil</strong> est fournie pour l’authentification OAuth avec les Services Google Play.<br/></td>
<td align="left">Aucune bibliothèque OAuth générique n’est fournie. L’<strong>infrastructure des comptes</strong> permet d’accéder aux comptes d’utilisateurs déjà stockés sur l’appareil, tels que Facebook et Twitter.</td>
<td align="left">Le <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt270196.aspx">broker d’authentification Web</a></strong> de la bibliothèque OAuth générique permet de se connecter à des services de fournisseur d’identité tiers. Le <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt270189.aspx">stockage sécurisé des informations d’identification</a></strong> permet aux utilisateurs d’enregistrer leurs informations de connexion et de les utiliser sur plusieurs appareils. L’espace de noms <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn896755.aspx">Microsoft.Live</a></strong> vous permet d’accéder facilement à Live SDK OAuth pour l’accès aux services Microsoft.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt270184.aspx">Authentification et identité des utilisateurs</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/windows.security.authentication.web.aspx">Documentation sur l’API Windows.Security.Authentication.Web</a><br/><br/><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAuthenticationBroker">Exemple de code WebAuthenticationBroker</a></td>
</tr>
</tbody>
</table>
<h2 id="tooling">Outils</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Plateforme Windows universelle Windows10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>IDE.</strong> <br><br>Ensemble d’outils utilisé pour créer votre application.</td>
<td align="left"><strong>Android Studio</strong> et <strong>Eclipse</strong>, ainsi que Google incitent les développeurs à utiliser AndroidStudio.</td>
<td align="left"><strong>Xcode</strong></td>
<td align="left"><strong><a href="https://www.visualstudio.com/features/universal-windows-platform-vs.aspx">Visual Studio</a></strong> et <strong><a href="https://msdn.microsoft.com/library/jj171012.aspx">Blend pour Visual Studio</a></strong> disposent de tout ce dont vous avez besoin pour coder, concevoir, connecter, déboguer, analyser, optimiser et tester les applications UWP. Visual Studio propose également des <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt188754.aspx">émulateurs</a></strong> pour les appareils Windows 10 afin de pouvoir tester votre application sur une large gamme d’appareils émulés.<br/><br/><a href="https://dev.windows.com/downloads">Téléchargements et outils pour UWP</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Organisation du code.</strong> <br><br>Structure de dossiers de base d’une application, souvent créée à partir d’un modèle d’origine.</td>
<td align="left">Fichier <strong>AndroidManifest</strong>, dossier <strong>java</strong> contenant les fichiers sources, dossier <strong>res</strong> avec ressources comprenant des dispositions et des valeurs, scripts de création <strong>Gradle</strong> dans AndroidStudio et <strong>Ant</strong> dans Eclipse.</td>
<td align="left">Fichiers sources et <strong>fichiers de prise en charge</strong>, fichier <strong>Info.plist</strong>, <strong>Main.storyboard</strong> et <strong>LaunchScreen.storyboard</strong>. Les images sont stockées dans les <strong>bibliothèques de ressources</strong>.</td>
<td align="left">Votre application UWP contient des fichiers XAML et de code pour votre application intitulée Example.xaml et Example.xaml.cs, différentes images dans le <strong>dossier de ressources</strong>, une page de démarrage comme <strong>MainPage.xaml</strong> et <strong>MainPage.xaml.cs</strong> et un manifeste.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/dn765018.aspx">Créer une application Hello World</a></td>
</tr>
</tbody>
</table>
<h2 id="app-lifecycle">Cycle de vie de l’application</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Plateforme Windows universelle Windows10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Cycle de vie de l’application.</strong> <br><br>Gestion des événements lors du lancement, de la suspension, de la reprise et de la fermeture d’une application permettant d’enregistrer/de restaurer l’état de l’application et d’exécuter d’autres tâches.</td>
<td align="left">Chaque activité possède son propre <strong>cycle de vie d’activité</strong> avec des états de type <strong>reprise</strong>. <strong>Les rappels de cycle de vie</strong> tels que <strong>onResume</strong> sont implémentés dans vos <strong>classes d’activité</strong>.</td>
<td align="left">Le <strong>cycle de vie de l’application</strong> peut avoir des états de type <strong>suspendue</strong>. Les méthodes telles que <strong>applicationDidEnterBackground:</strong> sont implémentées dans l’<strong>objet délégué d’application</strong> pour exécuter du code lors d’un changement d’état.</td>
<td align="left">Votre application dispose des <strong>états d’exécution</strong> suivants : NotRunning, Activated, Running, Suspending, Suspended et Resuming.<br/><br/>Vous pouvez implémenter les méthodes de <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.aspx">classe d’application</a></strong> OnLaunched, OnActivated, Suspending ou Resuming dans votre application afin d’exécuter du code lors d’un changement d’état.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt243287.aspx">Cycle de vie de l’application</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Tâches en arrière-plan.</strong> <br><br>Tâches qui effectuent des opérations en arrière-plan et qui continuent à s’exécuter lorsque l’application n’est plus au premier plan.</td>
<td align="left">Les applications peuvent lancer des <strong>services</strong> qui effectuent des opérations en arrière-plan lorsque l’application n’est plus au premier plan. Les services ont leur propre <strong>cycle de vie</strong> et sont enregistrés dans le manifeste.</td>
<td align="left">L’<strong>exécution en arrière-plan</strong> est autorisée uniquement pour les types de tâche spécifiques.<br/><br/>Les applications déclarent les<strong> tâches en arrière-plan prises en charge</strong> dans le fichier Info.plist à l’aide de <strong>UIBackgroundModes</strong>.<br/><br/>Le système contrôle l’exécution des tâches en arrière-plan et leur durée.</td>
<td align="left">Vous pouvez créer une tâche en arrière-plan en implémentant l’interface <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.aspx">IBackgroundTask</a></strong> et en inscrivant la tâche dans le manifeste de l’application. Vous pouvez définir une tâche à déclencher avec un <a href="https://msdn.microsoft.com/library/windows/apps/mt186458.aspx"><strong>minuteur</strong></a>, un <a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.systemtriggertype.aspx"><strong>déclencheur système</strong></a> et un <a href="https://msdn.microsoft.com/library/windows/apps/mt185632.aspx"><strong>déclencheur de maintenance</strong></a>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt299103.aspx">Prendre en charge votre application avec des tâches en arrière-plan</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt299100.aspx">Créer et inscrire une tâche en arrière-plan</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187310.aspx">Recommandations en matière de tâches en arrière-plan</a></td>
</tr>
</tbody>
</table>
<h2 id="performance">Performances</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Plateforme Windows universelle Windows10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Meilleures pratiques en matière de performances</strong> <br><br>Recommandations en matière de création d’applications rapides, réactives, préservant l’autonomie de la batterie et offrant un démarrage rapide.</td>
<td align="left">Android fournit le guide de formation <strong>Meilleures pratiques en matière de performances</strong>.</td>
<td align="left">iOS fournit le document <strong>Vue d’ensemble des performances</strong>.</td>
<td align="left">Vous pouvez lire le <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt270266.aspx">guide des performances</a></strong> détaillé comprenant des sections traitant de sujets, tels que la définition d’objectifs en matière de performances, la mesure des performances, la gestion de la mémoire, la fluidité des animations, l’accès efficace au système de fichiers et les outils disponibles pour le profilage et les performances.</td>
</tr>
<tr class="even">
<td align="left"><strong>Optimisation de la vue pour une interface utilisateur réactive.</strong> <br><br>Amélioration des performances par l’optimisation des vues.</td>
<td align="left">L’optimisation des <strong>hiérarchies de disposition</strong> à l’aide de l’outil Visionneuse de la hiérarchie, la <strong>réutilisation des dispositions</strong> et le chargement des <strong>vues à la demande</strong>, sont des techniques qui permettent d’assurer la réactivité du thread et d’éviter les boîtes de dialogue &quot;L’application ne répond pas&quot; (<strong>ANR</strong>).<br/></td>
<td align="left">La résolution des problèmes liés à l’interface utilisateur avec le <strong>rendu hors écran</strong>, le <strong>mélange des couches</strong> et la <strong>rastérisation</strong> à l’aide de l’outil <strong>Core Animation</strong> assure la réactivité du thread de l’interface utilisateur.</td>
<td align="left">Vous pouvez facilement <strong>optimiser</strong> le <strong>balisage</strong> et les <strong>dispositions</strong> XAML en suivant quelques étapes simples. Les techniques incluent la réduction de la structure de la disposition, du nombre d’éléments et du surdessin. <br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185403.aspx">Assurer la réactivité du thread de l’interface utilisateur</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt204779.aspx">Optimiser votre balisage XAML</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt404609.aspx">Optimiser votre disposition XAML</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Threads.</strong> <br><br>Utilisation de threads pour assurer la <strong>réactivité de l’interface utilisateur</strong> et exécuter plusieurs <strong>tâches en parallèle</strong>.</td>
<td align="left">Les threads sont obtenus à l’aide des classes <strong>Runnable</strong>, <strong>Handler</strong>, <strong>ThreadPoolExecutor</strong> et <strong>AsyncTask</strong> de niveau supérieur.</td>
<td align="left">Les threads sont obtenus à l’aide des classes <strong>NSThread</strong>, <strong>Grand Central Dispatch</strong> et <strong>NSOperation</strong> de niveau supérieur.</td>
<td align="left">Vous pouvez travailler avec des threads en soumettant des <strong>éléments de travail</strong> au <strong>pool de threads</strong> avec <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.system.threading.threadpool.runasync.aspx">RunAsync</a></strong>. Vous pouvez utiliser un minuteur pour envoyer un élément de travail avec <strong><a href="https://msdn.microsoft.com/library/windows/apps/br230590.aspx">CreateTimer</a></strong> et créer un élément de travail qui se répète avec <strong><a href="https://msdn.microsoft.com/library/windows/apps/br230589.aspx">CreatePeriodicTimer</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187339.aspx">Envoyer un élément de travail au pool de threads</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187341.aspx">Utiliser un minuteur pour envoyer un élément de travail</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187338.aspx">Créer un élément de travail périodique</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187336.aspx">Meilleures pratiques pour l’utilisation du pool de threads</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Programmation asynchrone.</strong> <br><br>Éviter la complexité des threads en tirant parti des modèles de programmation asynchrones pour garantir la réactivité du thread de l’interface utilisateur.</td>
<td align="left">L’utilisation des <strong>threads est requise</strong> pour créer vos propres classes asynchrones. Certaines classes intégrées sont asynchrones.</td>
<td align="left">L’utilisation des <strong>threads est requise</strong> pour créer vos propres classes asynchrones. Certaines classes intégrées sont asynchrones.</td>
<td align="left">Vous pouvez utiliser les modèles asynchrones pour éviter le blocage du thread principal lorsque vous créez vos propres API, par exemple, à l’aide de <strong>async</strong> et <strong>await</strong> en C# et Visual Basic. Vous pouvez utiliser les API asynchrones intégrées qui se terminent par le mot <strong>Async</strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187335.aspx">Programmation asynchrone</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187337.aspx">Appeler des API asynchrones en C# ou Visual Basic</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Optimisation de l’affichage Liste.</strong> <br><br>Modèles intégrés permettant d’optimiser des listes de données, qui sont souvent peu performantes pour l’affichage de grandes quantités de données</td>
<td align="left">Le modèle de conception <strong>ViewHolder</strong> est utilisé pour éviter les multiples recherches de vues, et vous permet d’utiliser des éléments d’interface utilisateur réutilisables.</td>
<td align="left">Diverses optimisations peuvent être effectuées pour améliorer les performances de <strong>UITableView</strong>. Rien n’est intégré.</td>
<td align="left">Vous pouvez utiliser les contrôles <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx">ListView</a> et <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx">GridView</a> qui assurent la <strong>virtualisation de l’interface utilisateur</strong> offrant un mouvement panoramique fluide, une expérience de défilement expérience et un démarrage plus rapide. Vous pouvez également implémenter <a href="https://msdn.microsoft.com/library/windows/apps/system.collections.ilist.aspx">IList</a> et <a href="https://msdn.microsoft.com/library/windows/apps/system.collections.specialized.inotifycollectionchanged.aspx">INotifyCollectionChanged</a> dans votre source de données, pour <strong>virtualiser les données</strong> et améliorer les performances.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt204776.aspx">Optimisation des options d’interface ListView et GridView</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt574120.aspx">Virtualisation des données ListView et GridView</a></td>
</tr>
</tbody>
</table>
<h2 id="monetization">Monétisation</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Plateforme Windows universelle Windows10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Achats dans l’application.</strong> <br><br>Fonctionnalités de la plateforme permettant aux utilisateurs d’effectuer des achats dans vos applications.</td>
<td align="left">La <strong>facturation In-App</strong> est fournie par les services Google. Les produits sont ajoutés à la <strong>Console développeur de Google Play</strong>. Les achats dans l’application sont implémentés à l’aide de la <strong>bibliothèque de facturation Google Play</strong>.</td>
<td align="left">Les produits sont ajoutés à <strong>iTunes Connect</strong>. Les achats dans l’application sont implémentés à l’aide de l’infrastructure <strong>StoreKit</strong>.<br/><br/>Les produits sont achetés à l’aide de <strong>SKMutablePayment</strong> et <strong>SKPaymentQueue</strong>.</td>
<td align="left">Vous créez des achats de produits in-app pour votre application en les <a href="https://msdn.microsoft.com/library/windows/apps/mt148551.aspx">ajoutant à votre application et les soumettant au Store</a>. <br/><br/>Vous utilisez la <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx">classe CurrentApp</a></strong> pour définir des achats dans l’application. <br/><br/>Vous utilisez <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.requestproductpurchaseasync.aspx">CurrentApp.RequestProductPurchaseAsync</a></strong> pour afficher l’interface utilisateur qui permet aux clients d’acheter le produit.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219684.aspx">Activer les achats de produits in-app</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Achats de consommables dans l’application.</strong> <br><br>Produits intégrés à l’application pouvant être achetés, utilisés, puis de nouveau achetés.</td>
<td align="left">Les achats de consommables sont activés par un achat régulier, puis sa consommation à l’aide de <strong>consumePurchase</strong>, qui lui permet d’être acheté, utilisé, puis acheté de nouveau.</td>
<td align="left">Les produits consommables sont <strong>définis sous forme de produits consommables</strong> dans iTunes Connect.</td>
<td align="left">Vous pouvez prendre en charge des consommables en <a href="https://msdn.microsoft.com/library/windows/apps/mt148534.aspx">définissant leur type de produit sur Consommable lorsque vous les envoyez au</a> Store. Vous devez ensuite appeler <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync.aspx">CurrentApp.ReportConsumableFulfillmentAsync</a></strong> après l’achat d’un produit pour permettre au client d’y accéder.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219683.aspx">Activer les achats de consommables dans l’application</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Test des achats dans l’application.</strong> <br><br>Permet de tester le code d’achat dans l’application sans placer votre application dans le Store.</td>
<td align="left">Le <strong>bac à sable de facturation dans l’application</strong> est utilisé pour le test.</td>
<td align="left">Les <strong>comptes de testeur de bac à sable (sandbox)</strong> sont utilisés pour le test.</td>
<td align="left">Vous pouvez tester des achats dans l’application en utilisant simplement la classe <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx">CurrentAppSimulator</a></strong> au lieu de CurrentApp.<br/><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Essais gratuits.</strong> <br><br>Permet de limiter facilement le contenu ou de supprimer des publicités en fonction de la version d’évaluation d’une application.</td>
<td align="left">Google Play <strong>ne prend pas en charge officiellement les versions d’évaluation</strong>. Les essais gratuits ou la suppression des publicités s’obtient en créant un achat dans l’application et en prenant le chemin d’accès au code approprié lors de la confirmation de la réussite de l’achat.</td>
<td align="left">L’App Store <strong>ne prend pas en charge officiellement les essais gratuits d’applications</strong>. Les essais gratuits ou la suppression des publicités s’obtient en créant un achat dans l’application et en prenant le chemin d’accès au code approprié lors de la confirmation de la réussite de l’achat.</td>
<td align="left">Vous pouvez proposer une version d’évaluation gratuite de votre application à l’aide de l’<strong><a href="https://msdn.microsoft.com/library/windows/apps/mt148548.aspx">option Essai gratuit</a></strong> lors de l’envoi de votre application au Store. Vous pouvez ensuite utiliser <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.licenseinformation.istrial.aspx">LicenseInformation.IsTrial</a></strong> pour vérifier l’état d’essai de l’application et présenter, par conséquent, les différents chemins d’accès au code. Vous pouvez vous inscrire à l’<a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.licenseinformation.licensechanged">événement LicenseChanged</a> pour être averti lorsque l’utilisateur modifie l’état de l’essai lors de l’exécution de l’application.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219685.aspx">Exclure ou limiter des fonctionnalités de la version d’évaluation</a></td>
</tr>
</tbody>
</table>
<h2 id="adapting-to-multiple-platforms">Adaptation aux différentes plateformes</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Plateforme Windows universelle Windows10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Interface utilisateur adaptative: dispositions flexibles.</strong> <br><br>Prise en charge de différentes tailles d’écran avec une hauteur et une largeur flexibles.</td>
<td align="left">Des dispositions flexibles peuvent être obtenues à l’aide des valeurs <strong>wrap_content</strong> et <strong>match_parent</strong> des objets LinearLayout, ou en utilisant des objets RelativeLayout pour l’alignement.</td>
<td align="left">Des dispositions flexibles peuvent être obtenues à l’aide du <strong>modèle adaptatif</strong> avec des tables de montage séquentiel universelles, en utilisant la <strong>Disposition automatique</strong> avec des <strong>contraintes</strong> et des <strong>caractéristiques</strong> telles que horizontalSizeClass et displayScale qui sont appliquées aux contrôleurs d’affichage.</td>
<td align="left">Vous pouvez créer une disposition fluide à l’aide de <strong>propriétés</strong> et de <strong>panneaux de disposition</strong> avec une combinaison de dimensionnement fixe et dynamique.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx#layout_overview">Définir des dispositions avec XAML - Propriétés et panneaux de disposition</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/dn958435.aspx">Conception dynamique101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Interface utilisateur adaptative: dispositions adaptées.</strong> <br><br>Prise en charge de différentes tailles d’écran avec des dispositions ciblées distinctes.</td>
<td align="left">Le fait de fournir d’autres fichiers de disposition pour différentes configurations d’écran dans le répertoire de ressources utilisant des <strong>qualificateurs de configuration</strong> tels que <strong>small</strong>, <strong>large</strong>, <strong>ldpi</strong> et <strong>hdpi</strong> vous permet de cibler des dispositions personnalisées pour les écrans de différentes tailles et densités.</td>
<td align="left">Définissez une <strong>table de montage séquentiel pour iPhone et iPad distincte</strong> afin d’adapter les dispositions à différentes familles d’appareils dans une application universelle.</td>
<td align="left">Vous pouvez créer une disposition personnalisée en définissant <strong>différents fichiers de balisage XAML</strong> par famille d’appareils.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx#tailored_layouts">Définir des dispositions avec XAML - Dispositions personnalisées</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Interface utilisateur adaptative: dispositions dynamiques.</strong> <br><br>Réponse aux changements de taille d’écran, par exemple, à la rotation, ou à une modification de la taille d’une fenêtre.</td>
<td align="left">L’utilisation de dispositions flexibles avec <strong>LinearLayout</strong> et <strong>RelativeLayout</strong>, ou le fait de proposer d’autres fichiers de disposition pour différentes orientations offre des dispositions dynamiques.</td>
<td align="left">Lors du changement de la <strong>taille</strong> ou des <strong>caractéristiques</strong> d’une vue, les <strong>contraintes</strong> spécifiées dans les tables de montage séquentiel s’appliquent.</td>
<td align="left">Vous pouvez facilement réorganiser, repositionner, redimensionner, faire apparaître ou remplacer des sections de votre interface utilisateur lors de l’exécution en réponse aux changements de taille de fenêtre à l’aide de <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstate.aspx">VisualState</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager.aspx">VisualStateManager</a></strong> et <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.adaptivetrigger.aspx">AdaptiveTrigger</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx#visual_states_and_state_triggers">Définir des dispositions avec XAML - États visuels et déclencheurs d’état</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/dn958435.aspx">Conception dynamique101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Prise en charge de différentes fonctionnalités d’appareils.</strong> <br><br>Exploitation des fonctionnalités matérielles avancées tout en assurant la prise en charge des appareils qui n’en sont pas dotés.</td>
<td align="left">Le fait de tester des fonctionnalités d’appareil au moment de l’exécution à l’aide de <strong>PackageManager.hasSystemFeature</strong> vous permet de déterminer si un code spécifique d’un matériel peut s’exécuter.</td>
<td align="left">Il n’existe <strong>aucune vérification unique</strong> à effectuer lors de l’exécution pour tester les fonctionnalités d’un appareil. Chaque fonctionnalité est testée de manière spécifique pour déterminer si le code propre à un matériel peut être exécuté.</td>
<td align="left">Vous pouvez ajouter des <strong>Kits de développement logiciel (SDK) d’extension de plate-forme</strong> à votre package pour cibler les fonctionnalités supplémentaires trouvées dans différentes familles d’appareils : téléphone, ordinateur de bureau et IoT. Vous utilisez également l’<strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.aspx">API ApiInformation</a></strong> pour tester la présence de types et de membres lors de l’exécution. Vous ne pouvez appeler ces types et membres que s’ils sont présents.</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Prise en charge de différentes fonctionnalités d’appareils.</strong> <br><br>Exploitation des fonctionnalités matérielles avancées tout en assurant la prise en charge des appareils qui n’en sont pas dotés.</td>
<td align="left">La <strong>bibliothèque de prise en charge Android</strong> peut être fournie avec votre application afin de mettre à disposition quelques nouvelles API aux utilisateurs d’anciennes versions d’Android. Le test du niveau d’API lors de l’exécution peut être effectué à l’aide de <strong>Build.Version.SDK_INT</strong>.</td>
<td align="left">Les vérifications à l’exécution standard permettent de déterminer si des API sont disponibles, comme la méthode <strong>class</strong> qui vérifie l’existence d’une classe et <strong>respondsToSelector:</strong> qui recherche les méthodes sur les classes.</td>
<td align="left">Vous pouvez utiliser <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn949005.aspx">ApiInformation.IsApiContractPresent</a></strong> pour vérifier la présence d’un contrat API avec un nombre majeur et mineur spécifié. Vous utilisez également l’<strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.aspx">API ApiInformation</a></strong> pour tester la présence de types et de membres lors de l’exécution. Vous ne pouvez appeler ces types et membres que s’ils sont présents.</td>
</tr>
</tbody>
</table>
<h2 id="notifications">Notifications</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Plateforme Windows universelle Windows10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Vignettes et badges.</strong> <br><br>Présentation des mises à jour aux utilisateurs sur l’écran d’accueil.</td>
<td align="left">Les <strong>widgets d’application</strong> sont des vues de votre application qui peuvent être intégrées à l’écran d’accueil et qui peuvent recevoir des mises à jour périodiques. Il n’existe <strong>aucun système de badge</strong> sur Android. Il n’existe aucun système identique aux vignettes.</td>
  <td align="left"><strong>Widgets</strong> sur iOS, ils apparaissent dans le Centre de notifications et sont implémentés en tant qu'<strong>Extensions d’application</strong>. Vous pouvez également ajouter un <strong>badge</strong> à votre icône avec un nombre qui peut changer suite à des notifications locales ou distantes. Il n'existe aucun système de vignette.</td>
<td align="left">Votre application dispose d’une <strong>vignette</strong> qui peut être épinglée à l’écran de démarrage et permet d’afficher les textes, images et un <strong>badge</strong> de votre choix avec des glyphes et des chiffres. Vous pouvez mettre à jour le contenu des vignettes à partir de l’application par le biais de notifications Push ou selon un calendrier prédéfini. Les vignettes peuvent être adaptatives et varier en fonction de l’endroit où elles sont affichées.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt185605.aspx">Créer des vignettes</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt590880.aspx">Créer des vignettes adaptatives</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt187193.aspx">Choisir une méthode de remise de notification</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465403.aspx">Recommandations en matière de vignettes et de badges</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Affichage des notifications.</strong> <br><br>Types de notifications pouvant être affichées.</td>
<td align="left">Les notifications peuvent être affichées dans la <strong>zone de notification</strong> et le <strong>panneau de notification</strong>. Les <strong>notifications d’avertissement</strong> s’affichent dans une petite fenêtre flottante. La définition d’un <strong>PendingIntent</strong> permet d’ajouter des actions aux notifications.</td>
<td align="left">Les notifications contextuelles apparaissent sous forme de <strong>bannières</strong> ou d’<strong>alertes</strong>. Vous pouvez ajouter des boutons d’action personnalisés aux <strong>notifications interactives</strong> définis avec <strong>UIMutableUserNotificationAction</strong>.</td>
<td align="left">Vous pouvez créer des notifications contextuelles adaptatives appelées <strong>notifications toast</strong>. Vous pouvez définir des notifications toast en XML avec du contenu visuel, des <strong>actions</strong> qui peuvent être des boutons, ou des entrées et du son.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt631604.aspx">Notifications toast adaptatives et interactives</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt187193.aspx">Choisir une méthode de remise de notification</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465391.aspx">Recommandations en matière de notifications toast</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Planification de notifications locales.</strong> <br><br>Notifications locales envoyées par votre application à une heure prévue.</td>
<td align="left">Les notifications et actions sont définies à l’aide d’un <strong>NotificationCompat.Builder</strong> et peuvent être planifiées et gérées dans l’application à l’aide d’<strong>AlarmManager</strong> et de <strong>BroadcastReceiver</strong>.</td>
<td align="left">Des notifications locales sont créées à l'aide de l'élément <strong>UILocalNotification</strong>, et peuvent être planifiées avec <b> UILocalNotification.scheduleLocalNotification:<strong>. | Vous pouvez planifier une notification toast à l'aide de l'élément </strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.notifications.scheduledtoastnotification.aspx">ScheduledToastNotification</a><strong>. Vous pouvez envoyer une notification par vignette de votre application en utilisant la </strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.notifications.tilenotification.aspx">classe TileNotification</a><strong>, ou planifier une notification par vignette avec <a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.notifications.scheduledtilenotification.aspx">ScheduledTileNotification</a>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt631604.aspx">Notifications toast adaptatives et interactives</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt593299.aspx">Envoyer une notification par vignette locale</a> | | </strong>Envoi de notifications Push.</b> Une notification envoyée depuis un serveur de notification Push et potentiellement traitée dans l'application.</td>
<td align="left"><strong>Google Cloud Messaging</strong> prend en charge les notifications Push pour Android.</td>
</tr>
</tbody>
</table>
<h2 id="media-capture-and-rendering">Capture et rendu multimédia</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Plateforme Windows universelle Windows10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Capture multimédia.</strong> <br><br>Enregistrement d’un contenu audio et visuel.</td>
<td align="left">L’utilisation de <strong>intent</strong>, tel que MediaStore.ACTION_VIDEO_CAPTURE permet la capture multimédia avec une application de caméra existante. L’utilisation de la bibliothèque <strong>android.hardware.camera2</strong> ou <strong>camera</strong> permet d’implémenter une interface de caméra personnalisée. Les API <strong>MediaRecorder</strong> permettent de capturer des données audio.</td>
<td align="left"><strong>UIImagePickerController</strong> permet la capture de vidéos et de photos avec l’interface utilisateur du système. Les classes <strong>AVFoundation</strong> telles que <strong>AVCaptureSession</strong> permettent d’accéder directement à la caméra. <br/>La classe <strong>AVAudioRecorder</strong> permet d’enregistrer du son.</td>
<td align="left">Vous pouvez capturer des photos et des vidéos tout en utilisant l’interface utilisateur de la caméra intégrée avec la classe <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.capture.cameracaptureui.aspx">CameraCaptureUI</a></strong>. Vous pouvez interagir avec la caméra à un niveau faible et capturer du son à l’aide de classes dans <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.capture.aspx">Windows.Media.Capture</a></strong>, comme l’<strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.capture.mediacapture.aspx">API MediaCapture</a></strong>. <br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt282142.aspx">Capturer des photos et des vidéos à l’aide de CameraCaptureUI</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243896.aspx">Capturer des photos et des vidéos à l’aide de MediaCapture</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Lecture de contenu multimédia.</strong> <br><br>Lecture de fichiers audio et vidéo.</td>
<td align="left">Les classes <strong>MediaPlayer</strong> et <strong>AudioManager</strong> permettent de lire des fichiers audio et vidéo.</td>
<td align="left">L’<strong>infrastructure AVKit</strong>, <strong>AVAudioPlayer</strong> et l’<strong>infrastructure du lecteur multimédia</strong> permettent de lire des fichiers audio et vidéo.</td>
<td align="left">Vous pouvez utiliser les classes <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.core.mediasource.aspx">MediaSource</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaelement.aspx">MediaElement</a></strong> et <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.aspx">MediaPlayer</a></strong> pour lire du contenu audio et vidéo à partir de sources telles que des fichiers locaux et distants.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592657.aspx">Lecture de contenu multimédia avec MediaSource</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Modification multimédia.</strong> <br><br>Composition de nouveaux fichiers multimédias à partir d’enregistrements existants et application d’effets spéciaux.</td>
<td align="left">Les classes de faible niveau, telles que <strong>MediaCodec</strong>, <strong>MediaMuxer</strong> et <strong>android.media.effect</strong> peuvent être utilisées pour modifier du contenu.</td>
<td align="left">Les classes de l’infrastructure <strong>AV Foundation</strong>, telles que <strong>AVMutableComposition</strong>, <strong>AVMutableVideoComposition</strong> et <strong>AVMutableAudioMix</strong> peuvent être utilisées pour modifier du contenu.</td>
<td align="left">Vous pouvez utiliser les API <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.editing.aspx">Windows.Media.Editing</a></strong>, telles que <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.editing.mediacomposition.aspx">MediaComposition</a></strong> et <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.editing.mediaclip.aspx">MediaClip</a></strong> pour créer des compositions multimédias à partir de fichiers audio et vidéo. Vous pouvez ajouter des superpositions d’images et de vidéos, combiner des clips vidéo, ajouter du son en arrière-plan et appliquer des effets audio et vidéo.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt204792.aspx">Compositions multimédias et modification</a></td>
</tr>
</tbody>
</table>
<h2 id="sensors">Capteurs</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Plateforme Windows universelle Windows10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Capteurs.</strong> <br><br>Détection des propriétés environnementales, de position et de mouvement de l’appareil.</td>
<td align="left">L’<strong>infrastructure de capteur</strong> permet d’accéder aux capteurs matériels et logiciels avec des classes, telles que <strong>SensorManager</strong> et <strong>SensorEvent</strong>.</td>
<td align="left">L<strong>’infrastructure Core Motion</strong> permet d’accéder aux données des capteurs bruts et traités.</td>
<td align="left">Vous pouvez utiliser des classes dans <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.aspx">Windows.Devices.Sensors</a></strong> pour accéder aux mesures des capteurs et aux événements déclenchés lors de la réception de nouvelles données de lecture provenant de l’appareil.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt187358.aspx">Capteurs</a></td>
</tr>
</tbody>
</table>
<h2 id="location-and-mapping">Localisation et mappage</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Plateforme Windows universelle Windows10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Localisation.</strong> <br><br>Recherche de l’emplacement <strong>actuel</strong> de l’appareil et suivi des <strong>modifications</strong>.</td>
<td align="left">Les API de localisation des services Google Play offrent un accès de haut niveau au <strong>dernier emplacement connu</strong> avec le <strong>fournisseur de localisation fusionné</strong> à l’aide des méthodes <strong>getLastLocation</strong> et <strong>requestLocationUpdates</strong>. <strong>LocationManager</strong> offre un accès de bas niveau dans les bibliothèques Android.</td>
<td align="left">La classe <strong>CLLocationManager</strong> <strong>de Core Location</strong> permet de surveiller l’emplacement d’un appareil, avec <strong>startUpdatingLocation</strong> pour le service de localisation standard et <strong>startMonitoringSignificantLocationChanges</strong> pour le service de localisation en cas de <strong>changement notable</strong>.</td>
<td align="left">Vous pouvez suivre la localisation de l’appareil à l’aide des classes dans <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.aspx">Windows.Devices.Geolocation</a></strong>. Utilisez <strong><a href="https://msdn.microsoft.com/library/windows/apps/br225537.aspx">Geolocator.GetGeopositionAsync</a></strong> pour une lecture unique. Utilisez <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geolocator.positionchanged.aspx">Geolocator.PositionChanged</a></strong> pour procéder à une localisation régulière à l’aide d’un minuteur ou être informé lorsque l’emplacement a changé.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219698.aspx">Obtenir l’emplacement de l’utilisateur</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Affichage de cartes.</strong> <br><br>Affichage d’une <strong>carte interactive intégrée</strong> et ajout de <strong>centres d’intérêt</strong>.</td>
<td align="left">Les classes <strong>GoogleMap</strong>, <strong>MapFragment</strong> et <strong>MapView</strong> de l’<strong>API Google Maps Android</strong> permet d’intégrer des cartes dans des applications. Il est possible d’afficher des centres d’intérêt à l’aide de <strong>marqueurs</strong> et de la classe <strong>Marker</strong> personnalisable.</td>
<td align="left">Les cartes sont intégrées aux applications iOS à l’aide de la classe <strong>MKMapView</strong> de l’<strong>infrastructure MapKit</strong>. Des <strong>annotations</strong> peuvent être ajoutées aux applications pour afficher des centres d’intérêt à l’aide de classes d’objet, telles que <strong>MKPointAnnotation</strong> et de classes de vue, telles que <strong>MKPinAnnotationView</strong>.</td>
<td align="left">Vous pouvez intégrer des cartes dans vos applications à l’aide du contrôle XAML <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx">MapControl</a></strong> intégré qui fournit des vues 2D, 3D et Streeside. Vous pouvez ajouter des clics-infos, des images ou des formes aux centres d’intérêt à l’aide de classes, telles que <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapicon.aspx">MapIcon</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mappolygon.aspx">MapPolygon</a></strong> et <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mappolyline.aspx">MapPolyline</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219695.aspx">Afficher des cartes avec des vues 2D, 3D et Streetside</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219696.aspx">Afficher des centres d’intérêt sur une carte</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Géorepérage.</strong> <br><br>Surveillance de l’entrée et de la sortie d’une région géographique particulière.</td>
<td align="left">Les limites géographiques sont surveillées à l’aide des <strong>services de localisation</strong> du Kit de développement logiciel (SDK) des services Google Play.</td>
<td align="left">Les régions sont surveillées avec la classe <strong>CLCircularRegion</strong> et enregistrées à l’aide de <strong>CLLocationManager.startMonitoringForRegion:</strong>.</td>
<td align="left">Vous pouvez créer une limite géographique à l’aide de la classe <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geofencing.geofence.aspx">Geofence</a></strong> et définir les <strong>états surveillés</strong>, relatifs par exemple, à l’entrée ou à la sortie d’une région. Gérez les événements de limite géographique au premier plan avec la <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geofencing.geofencemonitor.aspx">classe GeofenceMonitor</a></strong>, et en arrière-plan avec <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.locationtrigger.aspx">LocationTrigger</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219702.aspx">Configurer une limite géographique</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Géocodage et géocodage inverse.</strong> <br><br>Conversion d’adresses en emplacements géographiques (géocodage) et inversement (géocodage inverse).<br/></td>
<td align="left">La classe <strong>Geocoder</strong> est utilisée pour le géocodage et le géocodage inverse.</td>
<td align="left">La classe <strong>CLGeocoder</strong> est utilisée pour le géocodage.</td>
<td align="left">Vous pouvez effectuer le géocodage à l’aide de la <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.aspx">classe MapLocationFinder</a></strong> dans <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.aspx">Windows.Services.Maps</a></strong>. Utilisez <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.findlocationsasync.aspx">FindLocationsAsync</a></strong> pour le géocodage et <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.findlocationsatasync.aspx">FindLocationsAtAsync</a></strong> pour le géocodage inverse.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219697.aspx">Effectuer un géocodage et un géocodage inverse</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Indications et itinéraires.</strong> <br><br>Itinéraires, indications et distances entre deux emplacements géographiques.</td>
<td align="left">Google propose l’<strong>API Google Maps Directions</strong> du service web qui peut être utilisée sur Android même si aucun Kit de développement logiciel n’est fourni.</td>
<td align="left">Map Kit fournit l’API <strong>MKDirections</strong> qui peut être utilisée pour récupérer des informations relatives à un itinéraire et des indications.</td>
<td align="left">Vous pouvez demander un itinéraire en voiture ou à pied avec la <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maproutefinder.aspx">MapRouteFinder</a></strong> de classe dans <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.aspx">Windows.Services.Maps</a></strong>. Les indications sont renvoyées sous forme d’instance <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maproute.aspx">MapRoute</a></strong> qui peut être facilement affichée sur un MapControl. Les itinéraires sont renvoyés à l’intérieur de l’objet <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maproutemaneuver.aspx">MapRouteManeuver</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219701.aspx">Afficher des itinéraires et indications sur une carte</a></td>
</tr>
</tbody>
</table>
<h2 id="app-to-app-communication">Communication entre les applications</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Plateforme Windows universelle Windows10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Appel d’une autre application.</strong> <br><br>Lancement d’une autre application et éventuellement partage de données, telles que des liens, du texte, des photos, des vidéos et des fichiers.</td>
<td align="left">Une <strong>intention implicite</strong> permet de lancer une autre application en définissant une <strong>action</strong> et des données facultatives dans une<strong> intention</strong> et en l’appelant avec <strong>startActivityForResult</strong>.<br/></td>
<td align="left">Les <strong>extensions d’application</strong> permettent d’accéder aux données d’une autre application. Les <strong>schémas d’URL</strong> permettent de transmettre une URL à une autre application.</td>
<td align="left">Vous pouvez lancer une autre application enregistrée pour un URI avec <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.system.launcher.launchuriasync.aspx">Launcher.LaunchUriAsync</a></strong>, ou <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.system.launcher.launchuriforresultsasync.aspx">Launcher.LaunchUriForResultsAsync</a></strong> pour obtenir des résultats et des données à partir de l’application lancée. Vous pouvez utiliser <strong><a href="https://msdn.microsoft.com/library/windows/apps/hh701471.aspx">Launcher.LaunchFileAsync</a></strong> pour transmettre un fichier à une autre application à gérer.<br/><br/>Vous pouvez utiliser un <strong>contrat de partage</strong> pour partager facilement des données entre des applications.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228340.aspx">Lancer l’application par défaut pour un URI</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt269386.aspx">Lancer une application pour obtenir des résultats</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt299102.aspx">Lancer l’application par défaut pour un fichier</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243293.aspx">Partager des données</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Autoriser l’appel de votre application.</strong> <br><br>Autorisation de votre application à répondre à une demande issue d’une autre application.</td>
<td align="left">Les applications enregistrent une <strong>activité de gestion d’intention</strong> avec un <strong>filtre d’intention</strong> pour répondre à une intention implicite à partir d’une autre application.</td>
<td align="left">La création d’un package d’une <strong>extension d’application</strong> permet de partager des données avec d’autres applications. Les applications peuvent enregistrer un <strong>schéma d’URL personnalisé</strong> à l’aide de la clé <strong>CFBundleURLTypes</strong> d’Info.plist.</td>
<td align="left">Vous pouvez enregistrer votre application comme gestionnaire par défaut d’un <strong>nom de schéma d’URI</strong> en enregistrant un <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.activationkind.aspx#Protocol">protocole</a></strong> dans le manifeste du package et en mettant à jour le gestionnaire d’événements <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onactivated.aspx">Application.OnActivated</a></strong>, qui renvoie éventuellement des résultats. De la même manière, vous pouvez enregistrer votre application comme gestionnaire par défaut pour certains types de fichiers en ajoutant une déclaration dans le manifeste du package et en gérant l’événement <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onfileactivated.aspx">Application.OnFileActivated</a></strong>.<br/><br/>Vous pouvez gérer les demandes de contrat de partage en enregistrant votre application en tant que cible de partage dans le manifeste et en gérant l’événement <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.application.onsharetargetactivated.aspx">Application.OnShareTargetActivated</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt269386.aspx">Lancer une application pour obtenir des résultats</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt269385.aspx">Gérer l’activation des fichiers</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243292.aspx">Recevoir des données</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Copier-coller.</strong> <br><br>Copie et collage de texte et d’autres contenus entre les applications.</td>
<td align="left">L’<strong>infrastructure du Presse-papiers</strong> peut servir à implémenter la fonction de copier-coller à l’aide des classes <strong>ClipboardManager</strong> et <strong>ClipData</strong>.</td>
<td align="left"><strong>UIPasteboard</strong>, <strong>UIMenuController</strong> et <strong>UIResponderStandardEditActions</strong> peuvent servir à implémenter la fonction de copier-coller.</td>
<td align="left">De nombreux contrôles XAML par défaut prennent déjà en charge la fonction de copier-coller. Vous pouvez implémenter la fonction de copier-coller vous-même à l’aide des classes <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx">DataPackage</a></strong> et <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.aspx">Clipboard</a></strong> dans <strong><a href="https://msdn.microsoft.com/library/windows/apps/br205967">Windows.ApplicationModel.DataTransfer</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243291.aspx">Copier et coller</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Glisser-déplacer.</strong> <br><br>Glisser-déplacer du contenu entre plusieurs applications.</td>
<td align="left">La fonction de glisser-déplacer peut être implémentée au sein d’une seule application à l’aide de l’<strong>infrastructure de glisser-déplacer Android</strong>.</td>
<td align="left">Aucune API de glisser-déplacer de haut niveau n’est fournie par iOS.</td>
<td align="left">Vous pouvez implémenter la fonction de glisser-déplacer dans votre application pour permettre de glisser-déplacer du contenu d’une application à une autre, du Bureau vers une application et inversement. La prise en charge de la fonction de glisser-déplacer au sein de la classe UIElement s’effectue à l’aide des propriétés <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx">AllowDrop</a></strong>, et <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx">CanDrag</a></strong> et des événements <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx">DragOver</a></strong> et <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx">Drop</a></strong>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt227651.aspx">Glisser-déplacer</a></td>
</tr>
</tbody>
</table>
<h2 id="software-design">Conception de logiciels</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Plateforme Windows universelle Windows10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Modèles de conception de logiciels.</strong> <br><br>Modèles recommandés ou bien adaptés pour la plateforme.</td>
<td align="left">Aucun modèle officiel n’a été recommandé ou fourni pour le développement Android, bien que la version bêta de l’infrastructure de liaison de données puisse permettre une utilisation plus généralisée du modèle <strong>Model-View-ViewModel (MVVM)</strong>. Un certain nombre d’articles et d’infrastructures tiers recommandent les approches <strong>Model-View-Presenter (MVP)</strong> et <strong>MVVM</strong>.</td>
<td align="left"><strong>Model-View-Controller (MVC)</strong> est un modèle commun utilisé avec iOS et intégré à la plateforme.</td>
<td align="left">Vous n’êtes pas limité à un modèle spécifique lorsque vous créez des logiciels pour UWP.<br/><br/>Vous pouvez utiliser le modèle de <a href="https://msdn.microsoft.com/library/windows/apps/mt210947.aspx">liaison de données</a> intégré pour garantir une séparation nette des problèmes liés aux données et de ceux liés à l’interface utilisateur et éviter d’avoir à coder les gestionnaires d’événements de l’interface utilisateur qui mettent ensuite à jour les valeurs des propriétés.<br/><br/>Vous pouvez étendre la liaison de données de manière à suivre le modèle <strong>Model-View-ViewModel (MVVM)</strong> soit en utilisant des bibliothèques MVVM tierces, telles que le <a href="https://mvvmlight.codeplex.com/">Kit de ressources MVVM Light</a>, ou en déployant votre propre liaison et en maintenant la logique en dehors de code-behind.<br/><br/><a href="https://msdn.microsoft.com/library/hh848246.aspx">Le modèle MVVM</a><br/><br/><a href="https://github.com/Windows-XAML/Template10/wiki">Modèles de projet Visual Studio Template10</a></td>
</tr>
</tbody>
</table>
