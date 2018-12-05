---
Description: Notifications Visualizer is a new Universal Windows Platform (UWP) app in the Store that helps developers design adaptive live tiles for Windows 10.
title: Notifications Visualizer
ms.assetid: FCBB7BB1-2C79-484B-8FFC-26FE1934EC1C
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e2bb5a450aebdf38f3d4f1a710f3537544dcddd6
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8692907"
---
# <a name="notifications-visualizer"></a>Notifications Visualizer

 


Notifications Visualizer est une nouvelle application de plateforme Windows universelle (UWP) [dans le Windows Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) qui permet aux développeurs de concevoir des vignettes dynamiques adaptatives et notifications toast interactives pour Windows 10.


## <a name="overview"></a>Vue d’ensemble

Notifications Visualizer fournit des aperçus visuels instantanés de votre vignette et notification toast lorsque vous modifiez la charge utile XML, comparables au mode Création/Éditeur XAML de VisualStudio. L’application contrôle également les erreurs, ce qui vous permet de créer une charge utile de vignette ou de notification toast valide.

Cette capture d’écran à partir de l’application montre la charge utile XML et la façon dont les tailles de vignette apparaissent sur un appareil sélectionné:

![Capture d’écran de l’éditeur d’application Notifications Visualizer avec le code et les vignettes](images/notif-visualizer-001.png)

 

Avec Notifications Visualizer, vous pouvez créer et tester des charges utiles de vignettes et de notifications toast adaptatives sans avoir à modifier et déployer votre propre application. Une fois que vous avez créé une charge utile avec des résultats visuels satisfaisants, vous pouvez l’intégrer dans votre application. Pour plus d’informations, voir [Envoyer une notification par vignette locale](sending-a-local-tile-notification.md) et [Envoyer un toast local](send-local-toast.md).

**Remarque**  simulation Notifications Visualizer des notifications toast et menu Démarrer de Windows n’est pas toujours tout à fait précise, et elle ne prend pas en charge certaines propriétés de charge utile avancées. Une fois la vignette ou le toast voulu obtenu, testez-le en épinglant la vignette ou en affichant le toast pour vérifier qu’il s’affiche comme vous le souhaitez.

 

## <a name="features"></a>Fonctionnalités

Notifications Visualizer est fourni avec quelques exemples de charges utiles pour montrer ce qu’il est possible de réaliser avec des vignettes dynamiques adaptatives et des notifications toast interactives et pour vous aider à démarrer. Vous pouvez tester les différentes options de texte, les gorupes/sous-groupes, les images d’arrière-plan, et vous pouvoir voir de quelle façon les vignettes s’adaptent aux différents écrans et appareils. Une fois les modifications effectuées, vous pouvez enregistrer votre charge utile mise à jour dans un fichier pour l’utiliser ultérieurement.

L’éditeur fournit des avertissements et des erreurs en temps réel. Par exemple, si votre charge utile est supérieure à 5Ko (limitation de la plateforme), Notifications Visualizer vous informe que votre charge utile dépasse cette limite. Vous êtes averti en cas de noms ou de valeurs d’attributs incorrects, ce qui vous permet de déboguer les problèmes visuels.

Vous pouvez contrôler les propriétés des vignettes comme le nom complet, la couleur, les logos, ShowName et la valeur de badge. Ces options vous aident à comprendre instantanément de quelle façon les propriétés des vignettes et les charges utiles de notification des vignettes interagissent, et quels sont les résultats produits.

Cette capture d’écran de l’application montre l’éditeur de vignettes:

![Capture d’écran de l’éditeur Notifications Visualizer avec les vignettes](images/notif-visualizer-004.png)

 

## <a name="related-topics"></a>Rubriques connexes

* [Obtenir Notifications Visualizer dans le WindowsStore](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)
* [Créer des vignettes adaptatives](create-adaptive-tiles.md)
* [Toasts interactifs](adaptive-interactive-toasts.md)