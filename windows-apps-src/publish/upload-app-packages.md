---
Description: Sur la page Packages, vous pouvez charger tous les fichiers de package (.xap, .appx, .appxupload et/ou .appxbundle) pour l’application que vous soumettez. Vous pouvez charger des packages pour tous les systèmes d’exploitation ciblés par votre application.
title: Charger les packages d’application
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
---

# Charger les packages d’application


Sur la page **Packages**, vous pouvez charger tous les fichiers de package (.xap, .appx, .appxupload et/ou .appxbundle) pour l’application que vous soumettez. Vous pouvez charger des packages pour tous les systèmes d’exploitation ciblés par votre application. Quand un client télécharge votre application, le Store passe en revue tous les packages disponibles de l’application, et propose automatiquement à chaque client le package le mieux adapté à son appareil.

Pour plus d’informations sur le contenu et sur la structure d’un package, voir [Exigences relatives au package de l’application](app-package-requirements.md). Vous devez également découvrir [comment les numéros de version peuvent avoir un impact sur les packages livrés à des clients spécifiques](package-version-numbering.md) et [comment les packages sont distribués à différents systèmes d’exploitation](guidance-for-app-package-management.md).

## Chargement de packages pour votre soumission


Pour charger des packages, faites-les glisser dans le champ de chargement, ou cliquez pour parcourir vos fichiers. La page **Packages** permet de charger des fichiers .xap, .appx, .appxupload et/ou .appxbundle.

> **Remarque** Pour Windows 10, vous devez toujours charger le fichier .appxupload, et non le fichier .appx ou .appxbundle. Pour plus d’informations sur l’empaquetage d’applications UWP pour le Windows Store, voir [Empaquetage d’applications Windows universelles pour Windows 10](../packaging/packaging-uwp-apps.md).

Si nous détectons des problèmes liés à vos packages lors de leur validation, vous devrez supprimer le package et résoudre le problème avant d’essayer de le charger à nouveau. Pour plus d’informations, voir [Résolution des erreurs de chargement de package](resolve-package-upload-errors.md).

Il se peut également que vous receviez des avertissements concernant des problèmes potentiels, sans que cela vous empêche de poursuivre votre soumission.

## Détails du package


Une fois vos packages correctement chargés, nous en dressons la liste en les groupant par système d'exploitation cible. Nous affichons le nom, la version et l'architecture du package. Cliquez sur **Détails** pour visualiser des informations complémentaires comme les langues prises en charge, les fonctionnalités de l'application et la taille de fichier de chaque package.

Si vous utilisez la [Médiation publicitaire Windows](../monetize/use-ad-mediation-to-maximize-revenue.md), vous disposez également d’un lien pour configurer la médiation publicitaire pour chaque package.

Si vous voulez supprimer un package de votre soumission, cliquez sur le lien **Supprimer** au bas de la section **Détails** de chaque package.

## Suppression des packages redondants


Si nous détectons qu’un ou plusieurs de vos packages sont redondants, nous affichons un avertissement vous recommandant de supprimer ces packages de votre soumission. Cette situation survient généralement quand, après avoir chargé des packages spécifiques, fournissez des packages de version supérieure prenant en charge le même ensemble de clients. Dans ce cas, aucun client n’obtient le package redondant, car vous proposez maintenant d’un meilleur package (version supérieure).

Si nous détectons la présence de packages redondants, nous vous offrons la possibilité de tous les supprimer automatiquement de cette soumission. Vous pouvez également supprimer des packages de la soumission individuellement.

## Packages avec Visual Studio Application Insights


Nous vous recommandons d’utiliser [Visual Studio Application Insights](http://go.microsoft.com/fwlink/?LinkId=615086) dans vos packages (ou de l’activer en cochant la case « Afficher la télémétrie dans le Centre de développement Windows » lors de la construction de votre package). Nous pourrons ainsi vous fournir les [informations détaillées sur la télémétrie de l’utilisation des applications](usage-report.md). Si vous ne configurez pas Application Insights dans Microsoft Visual Studio, lorsque nous détectons un package qui l’inclut, nous affichons un message confirmant qu’en soumettant votre package, vous acceptez d’activer la télémétrie de l’utilisation des applications pour votre compte de développeur. Vous pouvez désactiver la télémétrie de l’utilisation des applications à tout moment dans vos **Paramètres du compte**.

 

 




<!--HONumber=Mar16_HO1-->
