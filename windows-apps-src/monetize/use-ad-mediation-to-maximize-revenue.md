---
author: mcleanbyron
ms.assetid: 772DEBF2-1578-4330-9C14-70BCC6F55005
description: Microsoft prend en charge la médiation publicitaire pour vous permettre d’optimiser vos revenus publicitaires des produits in-app par la médiation des demandes de bannières publicitaires provenant de plusieurs réseaux publicitaires.
title: Utiliser la médiation publicitaire pour optimiser les revenus
---

#  Utiliser la médiation publicitaire pour optimiser les revenus


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Microsoft prend en charge la médiation publicitaire pour vous permettre d’optimiser vos revenus publicitaires des produits in-app par la médiation des demandes de bannières publicitaires provenant de plusieurs réseaux publicitaires. Différents réseaux publicitaires peuvent avoir leurs propres points forts, certains ayant un coût supérieur par milliers de vues (eCPM) ou un taux de remplissage supérieur (pourcentage de publicités fournies lorsque votre application effectue une demande) dans certains marchés que d’autres. Avec un réseau publicitaire unique, vous risquez de vous retrouver avec des demandes de publicité non satisfaites, entraînant ainsi une perte de recettes potentielles. La médiation publicitaire vous permet d’optimiser la monétisation de vos publicités en vérifiant que vous avez toujours une publicité active.

La prise en charge de la médiation publicitaire est disponible via le [SDK d’engagement et de monétisation de la Boutique Microsoft](http://aka.ms/store-em-sdk). Une fois que vous avez installé le kit de développement logiciel (SDK) et ajouté le contrôle Ad Mediator à votre application, vous pouvez spécifier la fréquence à laquelle chaque réseau est utilisé en mettant à jour la configuration de la médiation dans le Centre de développement. Vous pouvez l’optimiser par marché. De cette façon, vous utilisez les réseaux publicitaires les plus efficaces dans les régions appropriées. Et vous pouvez apporter des modifications à l’utilisation de chaque réseau publicitaire sans avoir à publier à nouveau votre application.

## Prise en main de la médiation publicitaire


Procédez comme suit pour installer et configurer la médiation publicitaire dans votre application :

1.  Passez en revue la liste des réseaux publicitaires et des types de projets pris en charge par la médiation publicitaire, configurez les comptes avec les réseaux publicitaires utilisés, et suivez les instructions de chaque réseau pour intégrer une application. Pour en savoir plus, voir [Sélectionner et gérer vos réseaux publicitaires](select-and-manage-your-ad-networks.md).

2.  Installez le [Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft](http://aka.ms/store-em-sdk) avec Visual Studio 2015 ou Visual Studio 2013.

3.  Dans Visual Studio, ouvrez votre projet ou créez-en un. Ouvrez la page dans laquelle vous voulez héberger les publicités, et faites glisser un élément **AdMediatorControl** dans la page. Si vous utilisez Microsoft Advertising, ajustez la hauteur et la largeur du contrôle pour l’adapter aux tailles de publicité prises en charge. Pour plus d’informations, voir [Ajouter et utiliser le contrôle de médiation publicitaire](add-and-use-the-ad-mediator-control.md).

4.  Exécutez **Services connectés** pour choisir les réseaux publicitaires que vous voulez cibler et configurer les paramètres obligatoires pour chaque réseau. Pour plus d’informations, voir [Ajouter et utiliser le contrôle de médiation publicitaire](add-and-use-the-ad-mediator-control.md).

5.  Testez l’implémentation de la médiation publicitaire dans votre application. Pour en savoir plus, voir [Tester l’implémentation de votre médiation publicitaire](test-your-ad-mediation-implementation.md).

6.  Soumettez votre application au tableau de bord du Centre de développement Windows et configurez vos paramètres de médiation publicitaire dans celui-ci. Pour en savoir plus, voir [Soumettre votre application et configurer une médiation publicitaire](submit-your-app-and-configure-ad-mediation.md).

7.  Examinez vos rapports de médiation publicitaire dans le tableau de bord du Centre de développement. Pour en savoir plus, voir [Rapport de médiation publicitaire](https://msdn.microsoft.com/library/windows/apps/mt148521).

## Utilisation de Microsoft Advertising sans médiation publicitaire


Si vous ne voulez pas utiliser de médiation publicitaire ou si votre type de projet n’est actuellement pas pris en charge par celle-ci, vous pouvez toujours fournir des bannières publicitaires à partir de Microsoft sans utiliser de médiation publicitaire. Pour plus d’informations, voir [AdControl et XAML et .NET](https://msdn.microsoft.com/library/mt313186.aspx) et [AdControl en HTML 5 et JavaScript](https://msdn.microsoft.com/library/mt313130.aspx).

## Rubriques connexes

* [Sélectionner et gérer vos réseaux publicitaires](select-and-manage-your-ad-networks.md)
* [Ajouter et utiliser le contrôle de médiation publicitaire](add-and-use-the-ad-mediator-control.md)
* [Tester l’implémentation de votre médiation publicitaire](test-your-ad-mediation-implementation.md)
* [Soumettre votre application et configurer une médiation publicitaire](submit-your-app-and-configure-ad-mediation.md)
* [Résoudre les problèmes liés à la médiation publicitaire](troubleshoot-ad-mediation.md)
 

 


<!--HONumber=May16_HO2-->


