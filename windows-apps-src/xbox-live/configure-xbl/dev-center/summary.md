---
title: Xbox Live Page Résumé
description: Décrit comment vous pouvez exploiter la vue Résumé Xbox Live
ms.assetid: ''
ms.date: 10/19/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, Xbox, jeux, uwp, windows 10, Xbox one, xbox live résumé, résumé, publier, historique de xbox live, barre de commandes, onglet Historique, table de résumé
ms.openlocfilehash: 289b472939c721e5bfb373d4de62ed800840bf57
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605984"
---
# <a name="the-xbox-live-configuration-summary-page"></a>La page Résumé configuration Xbox Live

Vous pouvez utiliser [partenaires](https://developer.microsoft.com/dashboard) pour configurer la Xbox Live pour votre titre. Avec les partenaires, vous serez en mesure de gérer la configuration du service pour chacun des bacs à sable de votre titre.
Pour configurer les aspects de Xbox Live de votre titre, accédez à la section Xbox Live pour votre titre, situé sous **Services** > **Xbox Live**. Dans cette page, vous trouverez un instantané de votre configuration actuelle sur le bac à sable sélectionné. Vous y trouverez également une description détaillée de ce qui a été configuré et publié dans le bac à sable également.

## <a name="sandbox-selector"></a>Sélecteur de bac à sable

 [Les bacs à sable](../../xbox-live-sandboxes.md) sont désormais un élément de navigation de niveau supérieur, vous pouvez basculer entre ou développez par sélection. L’interface utilisateur affiche les bacs à sable du titre en haut, sous forme d’onglets. Les informations affichées dans chaque onglet sont dans le contexte du sandbox associé.  

![Image de la commutation des onglets de bac à sable](../../images/summary/sandbox-tabs1.gif)

 Vous pouvez ajouter des bacs à sable supplémentaires en sélectionnant le « + » qui présente une boîte de dialogue où vous spécifiez quel bac à sable que vous souhaitez copier la configuration à partir d’et quel bac à sable voulez-vous copier la configuration.  

 ![Image de développement à un nouvel onglet de bac à sable](../../images/summary/sandbox-tabs2.gif)

## <a name="command-bar"></a>Barre de commandes

Comme indiqué plus haut la page affichée est toujours dans le contexte d’un bac à sable, par conséquent, la barre de commandes exposée juste en dessous montre toutes les actions que vous pouvez effectuer dans votre bac à sable donné. Les commandes disponibles sont :  

* **Exporter** -ce qui vous fournit un fichier zip qui contient tous les documents dans le bac à sable.
* **Importation** -vous permet de fournir un fichier zip contenant XBL valide qui une fois le chargement des documents seront disponible dans le bac à sable.
* **Certifier** -publie votre configuration actuelle dans le bac à sable de certification.  *Vous pouvez également utiliser le bouton Publier et modifier la destination vers le certificat pour effectuer cette opération.*
* **Historique** -ouvre un onglet qui affiche des informations sur le créateur de quoi et quand. Vous pouvez ouvrir cet onglet sur n’importe quelle page et il filtre pour les objets créés dans cette page.
* **Publier** -vous permet de choisir votre bac à sable de la source et de destination. Une fois la sélection effectuée, une validation sera exécutée ce qui vous permet de savoir si vous pouvez publier la configuration. Si, en sélectionnant la publication en définir votre configuration pour le bac à sable afin que vous pouvez tester cette configuration tout en utilisant le bac à sable approprié.  
  
  
![Image de la barre de commandes](../../images/summary/command-bar.png)  

## <a name="summary-table"></a>Table de résumé

L’interface utilisateur fournit désormais une restauration explicite de vos configurations différentes, permettant une à une vue d’ensemble de ce qui a été configuré, ce qui est facultatif, et ce qui est toujours requis avant la publication vers une version commerciale.  

* **Détail** – décrit ce qui a été configuré pour une fonctionnalité particulière dans le bac à sable en cours (Cela inclut les objets que vous avez créé, mais pas encore publié)
* **Depuis la dernière publication** : cela vous indiquera les nouvelles configurations que vous avez créé qui n’ont pas été publiées dans votre bac à sable pour le test
* **État** – vous informe que cette fonctionnalité est prête à être publié vers une version commerciale. Tout élément étiqueté « Pas prêt pour la vente au détail » doit être traité, sont des lignes marquées comme « Optional » à la discrétion du développeur.

*Auparavant, une fois que vous avez sélectionné le bouton « Test », validation exécuteriez et alors seulement vous sauriez si vous aviez un problème que vous avez besoin de corriger ; Cela simplifie ce processus et crée une meilleure interface utilisateur*  
  
![Image de la barre de commandes](../../images/summary/summary-table.png)  

## <a name="history-pane"></a>Volet Historique

Le volet de l’historique affiche les objets qui ont été créés dans le bac à sable et indique par qui et quand. Sur la page Résumé, le volet de l’historique affiche tous les objets créés et publier des actions effectuées sur le bac à sable. Toutefois, lorsque vous ouvrez ce volet sur une page spécifique, comme des primes, vous verrez uniquement l’historique de prime en ce qui vous permet de filtrer facilement votre recherche de l’historique.  

![Image du volet de l’historique](../../images/summary/history.png)  

## <a name="best-practices"></a>Meilleures pratiques

* Publiez vos modifications à votre sandbox après avoir apporté quelques modifications pour vérifier qu’ils sont en direct sur vos appareils et les comptes de test.
* Utilisez la nouvelle vue de l’onglet, la table de résumé et volet de l’historique pour vous aider à identifier rapidement ce qui est publiée là où.
* Dans les cas extrêmes où vous avez besoin pour effectuer des comparaisons XML entre les bacs à sable que vous pouvez utiliser la fonctionnalité d’exportation de ces deux bacs à sable pour obtenir les deux documents et les ouvrir puis avec un outil tel qu’au-delà de comparer.
* Exportation peut être utilisée pour obtenir une version locale des fichiers qui peuvent être validés sur votre propre contrôle de code source. De cette façon, si vous chaque perdre toute configuration, vous ne perdez pas il. Vous pouvez retirer votre configuration locale de votre contrôle de code source et le réimporter dans le bac à sable.