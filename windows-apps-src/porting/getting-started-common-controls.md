---
author: stevewhims
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
description: 'Prise en main : Contrôles courants'
title: 'Prise en main: Contrôles courants'
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bbb07da7fa28aed6e45c97d128f9bd04ca986fe7
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5572029"
---
# <a name="getting-started-common-controls"></a>Prise en main: Contrôles courants


## <a name="common-controls-list"></a>Liste des contrôles courants

Dans la section qui précède, vous utilisiez seulement deux contrôles : les boutons et les blocs de texte. La liste des contrôles à votre disposition est bien entendu beaucoup plus longue. Voici quelques contrôles communs que vous utiliserez dans vos applications et leurs équivalents dans iOS. Les contrôles iOS apparaissent dans l’ordre alphabétique, en regard des contrôles les plus similaires pour la plateforme Windows universelle (UWP).

L’intelligence des contrôles UWP est qu’ils sont capables de détecter le type d’appareil sur lequel ils s’exécutent, et de modifier leur apparence et leurs fonctionnalités en conséquence. Par exemple, si votre projet utilise le contrôle [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/br211681), il est capable de s’optimiser lui-même pour adapter son apparence et son comportement à l’appareil sur lequel il opère, qu’il s’agisse d’un ordinateur de bureau ou d’un téléphone. Vous n’avez rien à faire : les contrôles s’ajustent automatiquement au moment de l’exécution.

| Contrôle iOS (classe/protocole) | Contrôle UWP équivalent |
|------------------------------|--------------------------------------|
| Indicateur d’activité (**UIActivityIndicatorView**) | [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) <br/> Voir aussi [Démarrage rapide: ajout de contrôles de progression](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651) |
| Vue bannière publicitaire (**ADBannerView**) et vue déléguée bannière publicitaire (**ADBannerViewDelegate**) | [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) <br/> Voir aussi [Afficher des publicités dans votre application](../monetize/display-ads-in-your-app.md) |
| Bouton (UIButton) | [Button](https://msdn.microsoft.com/library/windows/apps/br209265) <br/> Voir aussi [Démarrage rapide: ajout de contrôles de bouton](https://msdn.microsoft.com/library/windows/apps/xaml/jj153346) |
| Sélecteur de dates (UIDatePicker) | [DatePicker](https://msdn.microsoft.com/library/windows/apps/br211681) |
| Vue image (UIImageView) | [Image](https://msdn.microsoft.com/library/windows/apps/br242752) <br/> Voir également [Image et ImageBrush](https://msdn.microsoft.com/library/windows/apps/mt280382) |
| Libellé (UILabel) | [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) <br/> Voir aussi [Démarrage rapide: affichage de texte](https://msdn.microsoft.com/library/windows/apps/xaml/hh700392) |
| Vue carte (MKMapView) et vue déléguée carte (MKMapViewDelegate) | Voir [Bing cartes pour les applications UWP](http://go.microsoft.com/fwlink/p/?LinkId=263496) |
| Contrôleur de navigation (UINavigationController) et contrôleur de navigation délégué (UINavigationControllerDelegate) | [Trame](https://msdn.microsoft.com/library/windows/apps/br242682) <br/> Voir aussi [Navigation](https://msdn.microsoft.com/library/windows/apps/mt187344) |
| Contrôle de page (UIPageControl) | [Page](https://msdn.microsoft.com/library/windows/apps/br227503) <br/> Voir aussi [Navigation](https://msdn.microsoft.com/library/windows/apps/mt187344) |
| Vue sélecteur (UIPickerView) et vue déléguée sélecteur (UIPickerViewDelegate) | [ComboBox](https://msdn.microsoft.com/library/windows/apps/br209348) <br/> Voir aussi [Ajout de zones de liste déroulante et de zones de liste](https://msdn.microsoft.com/library/windows/apps/xaml/hh780616) |
| Barre de progression (UIProgressView) | [ProgressBar](https://msdn.microsoft.com/library/windows/apps/br227529) <br/> Voir aussi [Démarrage rapide: ajout de contrôles de progression](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651) |
| Vue de défilement (UIScrollView) et vue déléguée de défilement (UIScrollViewDelegate) | [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/br209527) <br/>  Voir aussi [Exemple de zoom, panoramique et défilement XAML (Extensible Application Markup Language)](http://go.microsoft.com/fwlink/p/?LinkId=238577) |
| Barre de recherche (UISearchBar) et barre de recherche déléguée (UISearchBarDelegate) | Voir [Ajout d’une fonctionnalité de recherche à une application](https://msdn.microsoft.com/library/windows/apps/xaml/jj130767) <br/>  Voir aussi [Démarrage rapide : ajout d’une fonctionnalité de recherche à une application](https://msdn.microsoft.com/library/windows/apps/xaml/hh868180) |
| Contrôle segmenté (UISegmentedControl) | Aucun |
| Curseur (UISlider) | [Slider](https://msdn.microsoft.com/library/windows/apps/br209614) <br/>  Voir aussi [Comment ajouter un curseur](https://msdn.microsoft.com/library/windows/apps/xaml/hh868197) |
| Contrôleur de vue fractionnée (UISplitViewController) et contrôleur de vue fractionnée délégué (UISplitViewControllerDelegate) | Aucun |
| Switch (UISwitch) | [ToggleSwitch](https://msdn.microsoft.com/library/windows/apps/br209712) <br/>  Voir aussi [Comment ajouter un bouton bascule](https://msdn.microsoft.com/library/windows/apps/xaml/hh868198) |
| Contrôleur de barre d’onglets (UITabBarController) et contrôleur de barre d’onglets délégué (UITabBarControllerDelegate) | Aucun |
| Contrôleur de vue de table (UITableViewController), vue de table (UITableView), vue de table déléguée (UITableViewDelegate) et cellule de table (UITableViewCell) | [ListView](https://msdn.microsoft.com/library/windows/apps/br242878) <br/>  Voir aussi [Démarrage rapide: ajout des contrôles ListView et GridView](https://msdn.microsoft.com/library/windows/apps/xaml/hh780650) |
| Champ de texte (UITextField) et champ de texte délégué (UITextFieldDelegate) | [TextBox](https://msdn.microsoft.com/library/windows/apps/br209683) <br/>  Voir aussi [Afficher et modifier du texte](https://msdn.microsoft.com/library/windows/apps/mt280218) |
| Vue de texte (UITextView) et vue de texte déléguée (UITextViewDelegate) | [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) <br/>  Voir aussi [Démarrage rapide: affichage de texte](https://msdn.microsoft.com/library/windows/apps/xaml/hh700392) |
| Vue (UIView) et contrôleur de vue (UIViewController) | [Page](https://msdn.microsoft.com/library/windows/apps/br227503) <br/>  Voir aussi [Navigation](https://msdn.microsoft.com/library/windows/apps/mt187344) |
| Vue Web (UIWebView) et vue Web déléguée (UIWebViewDelegate) | [WebView](https://msdn.microsoft.com/library/windows/apps/br227702) <br/>  Voir aussi [Exemple de contrôle d’affichage Web XAML](http://go.microsoft.com/fwlink/p/?LinkId=238582) |
| Fenêtre (UIWindow) | [Trame](https://msdn.microsoft.com/library/windows/apps/br242682) <br/>  Voir aussi [Navigation](https://msdn.microsoft.com/library/windows/apps/mt187344) |

Pour obtenir d’autres contrôles, voir [Liste des contrôles](https://msdn.microsoft.com/library/windows/apps/mt185406).

**Remarque**pour obtenir la liste des contrôles pour les applications UWP en JavaScript et HTML, consultez la [liste des contrôles](https://msdn.microsoft.com/library/windows/apps/hh465453).

### <a name="next-step"></a>Étape suivante

[Prise en main: Navigation](getting-started-navigation.md)

## <a name="related-topics"></a>Rubriques connexes

* [build 2014: Le point sur l’interface utilisateur et les contrôles XAML](http://go.microsoft.com/fwlink/p/?LinkID=397897)
* [build 2014 : Développement d’applications à l’aide de l’infrastructure d’interface utilisateur XAML commune](http://go.microsoft.com/fwlink/p/?LinkID=397898)
* [build 2014 : Création d’applications convergées XAML à l’aide de Visual Studio](http://go.microsoft.com/fwlink/p/?LinkID=397876)
