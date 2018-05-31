---
author: mcleanbyron
ms.assetid: 9ca1f880-2ced-46b4-8ea7-aba43d2ff863
description: Découvrez les problèmes connus de la version actuelle du SDK Microsoft Advertising.
title: Problèmes connus et résolution des problèmes des publicités dans les applications
ms.author: mcleans
ms.date: 04/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, annonces, publicité, problèmes connus, résolution des problèmes
ms.localizationpriority: medium
ms.openlocfilehash: aaf2db68df9de3f397a0cbc677e18f4ed544cf4b
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1817530"
---
# <a name="known-issues-and-troubleshooting-for-ads-in-apps"></a>Problèmes connus et résolution des problèmes des publicités dans les applications

Cette rubrique répertorie les problèmes connus de la version actuelle du SDK Microsoft Advertising. Pour plus d'aide à la résolution des problèmes, consultez les rubriques suivantes.

* [Guide de résolution des problèmes pour HTML et JavaScript](html-and-javascript-troubleshooting-guide.md)
* [Guide de résolution des problèmes pour XAML et C#](xaml-and-c-troubleshooting-guide.md)

## <a name="adcontrol-interface-unknown-in-xaml"></a>Interface AdControl inconnue en XAML

Le balisageXAML d’un contrôle [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) peut afficher incorrectement une ligne courbe bleue impliquant que l’interface est inconnue. Ce problème se produit uniquement lors d’un ciblagex86, et peut être ignoré.

## <a name="lasterror-from-previous-ad-request"></a>Élément lastError de la demande de publicité précédente

S’il reste un élément **lastError** de la demande de publicité précédente, l’événement peut être déclenché deuxfois durant le prochain appel de publicité. Si la nouvelle demande de publicité est toujours effectuée et peut générer une publicité valide, ce comportement peut cependant prêter à confusion.

## <a name="interstitial-ads-and-navigation-buttons-on-phones"></a>Spots publicitaires et boutons de navigation sur les téléphones

Sur les téléphones (ou émulateurs) pourvus des boutons logiciels **Précédent**, **Démarrer** et **Rechercher** au lieu des boutons matériels, le compte à rebours minuterie et les boutons de clic publicitaire des spots publicitaires risquent d’être masqués.

## <a name="recently-created-ads-are-not-being-served-to-your-app"></a>Les publicités récemment créées ne sont pas fournies à votre application

Si vous avez créé une publicité récemment (moins d’un jour), elle peut ne pas être disponible immédiatement. Si le contenu éditorial de la publicité a été approuvé, cette publicité est fournie à l’application une fois que le serveur de publicités l’a traitée. Elle est alors disponible en stock.

## <a name="no-ads-are-shown-in-your-app"></a>Aucune publicité n’est affichée dans votre application

Plusieurs raisons peuvent provoquer le non-affichage des publicités, notamment des erreurs réseau. Autres raisons possibles:

* Vous avez sélectionné une unité publicitaire dans le Centre de développement Windows dont la taille est supérieure ou inférieure à la taille du **AdControl** dans le code de votre application.

* Les publicités ne s’affichent pas si vous utilisez une [valeur du mode test](set-up-ad-units-in-your-app.md#test-ad-units) pour votreID d’unité publicitaire lors de l’exécution d’une application dynamique.

* Si vous avez créé un ID d’unité publicitaire dans la dernière demi-heure, la publicité risque de ne pas s’afficher tant que les serveurs n’ont pas propagé les nouvelles données dans le système. Les ID existants qui affichaient des publicités précédemment doivent en afficher immédiatement.

Si vous pouvez voir des publicités de test dans l’application, c’est que votre code fonctionne et qu’il peut afficher des publicités. Si vous rencontrez des problèmes, contactez le [support produit](https://developer.microsoft.com/en-us/windows/support). Dans cette page, choisissez **Publicités intégrées à l’app**.

Vous pouvez également publier une question sur le [forum](http://go.microsoft.com/fwlink/p/?LinkId=401266).

## <a name="test-ads-are-showing-in-your-app-instead-of-live-ads"></a>Les publicités de test s’affichent dans votre application à la place des publicités dynamiques

Les publicités de test peuvent s’afficher même lorsque vous attendez des publicités dynamiques. Cela peut se produire dans les cas suivants:

* Notre plateforme publicitaire ne peut pas vérifier ni trouver l’ID d’application dynamique utilisé dans la boutique. Dans ce cas, lorsqu’une unité publicitaire est créée par un utilisateur, son état peut démarrer à dynamique (non-test), mais passer à l’état de test dans les 6heures qui suivent la première demande de publicité. Il revient à l’état dynamique en cas d’absence de demandes d’applications de test pendant 10jours.

* Les applications chargées indépendamment ou les applications qui sont exécutées dans l’émulateur n’affichent pas de publicités dynamiques.

Si une unité publicitaire dynamique fournit des publicités de test, l’état de l’unité publicitaire indique **Publicités de test actives et fournies** dans le Centre de développement Windows. Pour le moment, cela ne s’applique pas aux applications téléphoniques.


<span id="reference_errors"/>

## <a name="reference-errors-caused-by-targeting-any-cpu-in-your-project"></a>Erreurs de référence provoquées par le ciblage de TouteUC dans votre projet

Lorsque vous utilisez le SDK Microsoft Advertising, vous ne pouvez pas cibler **TouteUC** dans votre projet. Si votre projet cible la plateforme **TouteUC**, un message d’avertissement peut s’afficher après que vous avez ajouté une référence semblable à ce qui suit.

![referenceerror\-solutionexplorer](images/13-19629921-023c-42ec-b8f5-bc0b63d5a191.jpg)

Pour supprimer cet avertissement, mettez à jour votre projet pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Utilisez le **Gestionnaire de configurations** pour définir les cibles de plateforme pour déboguer et publier les configurations.

![configurationmanagerwin10](images/13-87074274-c10d-4dbd-9a06-453b7184f8de.png)

Lorsque vous créez vos packages d’application pour les soumettre au WindowsStore (comme illustré dans les images suivantes), veillez à inclure les architectures que vous souhaitez cibler. Vous pouvez choisir d’ignorerx64 si vous prévoyez d’exécuter des buildsx86 sur le système d’exploitationx64.

![projectstorecreateapppackages](images/13-a99b05a4-8917-4c53-822e-2548fadf828a.png)

![createapppackages](images/13-16280cb1-a838-42b9-9256-eac7f33f5603.png)

## <a name="z-order-in-javascripthtml-apps"></a>Ordre de plan dans les applications JavaScript/HTML

Les applications HTML/JavaScript ne doivent pas placer d’éléments dans la plage MAX-10 réservée de l’ordre de plan. La seule exception est une superposition d’interruptions, par exemple une notification d’appel entrant pour une application Skype.

<span id="bkmk-ui"/>

## <a name="do-not-use-borders"></a>Ne pas utiliser de bordures

La définition des propriétés associées aux bordures, héritées par la classe **AdControl** de sa classe parente entraîne le placement erroné de la publicité.

## <a name="more-information"></a>Plus d’informations

Pour plus d’informations sur les derniers problèmes connus et pour publier des questions liées au SDK Microsoft Advertising, visitez le [forum](http://go.microsoft.com/fwlink/p/?LinkId=401266).

 

 
