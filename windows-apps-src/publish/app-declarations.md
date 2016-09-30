---
author: jnHs
Description: "Vous pouvez fournir des informations complémentaires sur votre application dans la section Déclarations d’application de la page Propriétés de l’application pendant le processus de soumission."
title: "Déclarations d’application"
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: ee2e54494a868fbec8c4a8bd7bbb9bdcd6cbf9ae

---

# Déclarations d’application

Vous pouvez fournir des informations complémentaires sur votre application dans la section **Déclarations d'application** de la page **Propriétés de l'application** pendant le [processus de soumission](app-submissions.md). Avec ces déclarations, vous avez la garantie que votre application s'affiche correctement et qu'elle est proposée au groupe de clients approprié, ou vous pouvez indiquer le nombre de clients autorisés à utiliser votre application.

Les sections suivantes décrivent chaque déclaration et ce que vous avez besoin de prendre en compte au moment de déterminer si chaque déclaration s'applique à votre application.

## Cette application permet aux utilisateurs d'effectuer des achats, mais n'utilise pas le système de commerce du Windows Store.

La plupart des applications doivent laisser cette case désactivée, car les applications qui offrent des possibilités d’effectuer des achats in-app utilisent généralement l’API d’achat in-app Microsoft pour créer et [soumettre les produits in-app](iap-submissions.md). En vertu du [Contrat développeur d’applications](https://msdn.microsoft.com/library/windows/apps/hh694058), les applications créées et soumises avant le 29 juin 2015 peuvent continuer à offrir la fonctionnalité d’achat in-app sans utiliser le moteur de commerce de Microsoft, pour autant que la fonctionnalité d’achat soit conforme aux [Politiques du Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_8). Si cela s’applique à votre application, vous devez activer cette case. Sinon, laissez-la désactivée.

## Cette application a fait l'objet de tests pour voir si elle est conforme aux recommandations d'accessibilité.

En cochant cette case, vous rendez votre application détectable par les clients qui recherchent tout particulièrement des applications accessibles dans le Windows Store.

Cochez uniquement cette case si vous avez :

-   défini toutes les informations d'accessibilité relatives aux éléments de l'interface utilisateur, comme les noms accessibles ;
-   implémenté la navigation et l'utilisation à partir du clavier en tenant compte de l'ordre des onglets, de l'activation du clavier, de la navigation à l'aide des touches de direction et des raccourcis ;
-   vérifié la présence d'une expérience visuelle accessible respectant notamment un coefficient de contraste de texte de 4.5:1 et n'utilisant pas simplement la couleur pour transmettre des informations à l'utilisateur ;
-   utilisé des outils de test d'accessibilité, comme Inspect ou AccChecker, afin de vérifier votre application et de résoudre les erreurs de priorité élevée détectées par ces outils ;
-   vérifié les scénarios clés de votre application de bout en bout à l'aide de fonctionnalités et d'outils tels que le Narrateur, la Loupe, le clavier sur écran, le contraste élevé et la haute résolution.

Lorsque vous déclarez votre application comme étant accessible, vous certifiez qu'elle est accessible à tous les utilisateurs, y compris ceux souffrant de handicaps. Cela signifie, par exemple, que vous avez testé l'application avec le mode de contraste élevé et avec un lecteur d'écran. Vous avez également vérifié que l’interface utilisateur fonctionnait correctement avec un clavier, la Loupe et d’autres outils d’accessibilité.

Pour plus d’informations, voir [Accessibilité des applications Windows Runtime](https://msdn.microsoft.com/library/windows/apps/dn263101), [Test d’accessibilité](https://msdn.microsoft.com/library/windows/apps/mt297664) et [Accessibilité dans le Windows Store](https://msdn.microsoft.com/library/windows/apps/mt297663).

> **Important** Ne déclarez pas votre application comme accessible à moins de l’avoir spécifiquement conçue et testée pour des scénarios d’accessibilité. Si votre application est déclarée comme étant accessible, mais qu’elle ne prend pas réellement en charge l’accessibilité, vous allez probablement recevoir des commentaires négatifs de la part de la communauté.

## Les clients peuvent installer cette application sur d'autres disques ou dispositifs de stockage amovible.

Cette case, cochée par défaut, autorise les clients à installer votre application sur un support de stockage amovible tel qu'une carte SD ou un lecteur de volume tiers, comme un lecteur externe.

Si vous voulez empêcher les clients à installer votre application sur d'autres lecteurs ou dispositifs de stockage amovibles, désélectionnez cette case.

Notez qu’il n’existe aucune option permettant de restreindre l’installation d’une application sur un dispositif de stockage amovible.

> **Remarque** Pour Windows Phone 8.1, cela était auparavant mentionné dans le fichier StoreManifest.xml.

## Windows peut intégrer les données de cette application dans les sauvegardes automatiques sur OneDrive.

Cette case est cochée par défaut pour permettre l'insertion des données de votre application quand un client choisit de paramétrer Windows pour des sauvegardes automatiques sur OneDrive.

Si vous voulez empêcher l’insertion des données de votre application dans les sauvegardes automatiques, décochez cette case.

> **Remarque** Pour Windows Phone 8.1, cela était auparavant mentionné dans le fichier StoreManifest.xml.

 

 

 







<!--HONumber=Jun16_HO4-->


