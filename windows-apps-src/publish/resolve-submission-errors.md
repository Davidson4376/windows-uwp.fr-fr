---
author: jnHs
Description: "Si vous rencontrez des erreurs après avoir envoyé votre application au Windows Store, vous devez les résoudre pour poursuivre le processus de certification."
title: "Résoudre les erreurs d’envoi"
ms.assetid: 68199E09-0C66-4EB4-BFE8-D2EEB139C4F3
translationtype: Human Translation
ms.sourcegitcommit: e295011db20d151f54e2581f8c67014b30261eb6
ms.openlocfilehash: 71f8ee9e2b8d1f892711ada30c58eefde250cca3

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

## Erreurs de nom/identité

Si vous voyez une erreur indiquant: **Le nom trouvé dans le package ne fait pas partie de vos noms d’application réservés. Veuillez réserver le nom de l’application et/ou mettre à jour votre package avec le nom d’application approprié pour cette langue**, il se peut que vous ayez saisi un nom incorrect dans votre package. Cette erreur peut également se produire si vous utilisez un nom d’application que vous n’avez pas réservé dans le Centre de développement. Vous pouvez généralement corriger cette erreur en procédant comme suit:

- Accédez à la page [Identité des applications](view-app-identity-details.md) pour votre application (sous **Gestion des applications**) pour vérifier si une identité est affectée à votre application. Si ce n’est pas le cas, une option vous permettra d’en créer une. Vous devez réserver un nom pour votre application afin de créer l’identité. Assurez-vous qu’il s’agit du nom que vous avez utilisé dans votre package.
- Si votre application possède déjà une identité, vous devrez peut-être quand même réserver le nom que vous voulez utiliser dans votre package. Sous **Gestion des applications**, cliquez sur [Gestion des noms d’application](manage-app-names.md). Entrez le nom que vous souhaitez utiliser, puis cliquez sur **Réserver le nom d’application**.

> **Important** Si le nom que vous voulez utiliser n’est pas disponible, il se peut qu’une autre application ait déjà réservé ce nom. Si votre application est déjà publiée sous ce nom, ou si vous pensez que vous avez le droit d’utiliser ce nom, [contactez le support](https://go.microsoft.com/fwlink/p/?LinkId=331509).  

 

 







<!--HONumber=Aug16_HO3-->


