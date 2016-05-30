---
author: jnHs
Description: Si vous rencontrez des erreurs après avoir envoyé votre application au Windows Store, vous devez les résoudre pour poursuivre le processus de certification.
title: Résoudre les erreurs d’envoi
ms.assetid: 68199E09-0C66-4EB4-BFE8-D2EEB139C4F3
---

# Résoudre les erreurs d’envoi


Si vous rencontrez des erreurs après avoir envoyé votre application au Windows Store, vous devez les résoudre pour poursuivre le [processus de certification](the-app-certification-process.md). Le message d’erreur indique la nature du problème et ce que vous devez faire pour le résoudre. Voici quelques informations supplémentaires pour vous aider à résoudre ces erreurs.

## Applications UWP


Si vous soumettez une application UWP, vous pouvez rencontrer une erreur au cours de prétraitement si votre fichier de package n'est pas un fichier .appxupload généré par Visual Studio pour le Windows Store. Lors de la création du fichier de package de votre application, veillez à suivre la procédure décrite dans la section [Création de packages d’applications Windows universels pour Windows 10](../packaging/packaging-uwp-apps.md) et à télécharger uniquement le fichier .appxupload sur la page [Packages](upload-app-packages.md) de la soumission (et non le fichier appx ou .appxbundle).

Si une erreur de compilation s'affiche, assurez-vous que vous êtes en mesure de générer correctement votre application en mode Release. Pour plus d'informations, voir [Erreurs du compilateur natif interne .NET](http://go.microsoft.com/fwlink/p/?LinkID=613098).

## Applications Windows Phone


L’**erreur 2001** peut se produire lorsque des problèmes sont détectés pendant le prétraitement des packages Windows Phone. Dans la plupart des cas, vous devez régénérer le package de votre application pour corriger l’erreur. Remplacez ensuite l'ancien package par le nouveau sur la page [Packages](upload-app-packages.md) de la soumission avant de cliquer de nouveau sur **Soumettre au Windows Store**.

Cette erreur peut avoir un certain nombre de causes. Passez en revue la liste ci-dessous pour identifier les problèmes qui peuvent s'appliquer à vos packages.

-   **Un ou plusieurs assemblys du package sont obscurcis de façon incorrecte :** utilisez un autre outil pour obscurcir le package ou supprimer l'obscurcissement. Le processus de compilation optimise les assemblys obscurcis mais il arrive que des assemblys soient obscurcis à l'aide d'un outil qui modifie le code MSIL d'une façon qui n'est pas prise en charge et qui provoque une erreur.
-   **La taille d'une ou plusieurs méthodes dans l'application dépasse 256 Ko d’IL :** refactorisez la méthode incriminée en fonctions plus petites. La taille du MSIL pour les méthodes dans un assembly peut être déterminée à l'aide de l'outil ILDASM.
-   **La validation de signature de nom fort a échoué pour un ou plusieurs assemblys :** cette erreur se produit généralement lorsque la signature de nom fort a été effectuée en utilisant une clé différente de celle prévue dans les métadonnées de l'assembly. Signez avec la clé correcte ou supprimez la signature de nom fort.
-   **Le package contient des assemblys en mode mixte (avec du code natif et managé) :** les assemblys en mode mixte ne sont pas pris en charge sur Windows Phone. Supprimez les assemblys en mode mixte du package et soumettez de nouveau l'application.
-   **Un assembly XAP ou appx/appxbundle Windows Phone 8.1 n'est pas valide :** assurez-vous que votre fichier .winmd dispose d'au moins un point d'entrée public. Vous pouvez utiliser n'importe quelle application de décompilation pour examiner le code et vérifier les points d'entrée publics si nécessaire.

Vous pouvez également rencontrer l’**erreur 1300** après avoir soumis votre application. Cette erreur se produit lorsqu’un ou plusieurs assemblys (ou la totalité du package) est déjà précompilé. Pour résoudre ce problème, recréez le package de l'application dans Microsoft Visual Studio, puis soumettez le package nouvellement généré.

 

 






<!--HONumber=May16_HO2-->


