---
Description: Si vous rencontrez des erreurs après avoir envoyé votre application au Windows Store, vous devez les résoudre pour poursuivre le processus de certification.
title: Résoudre les erreurs d’envoi
ms.assetid: 68199E09-0C66-4EB4-BFE8-D2EEB139C4F3
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: df7c1bbbc77374b8afb4272e1d9618c8294a4b6e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591854"
---
# <a name="resolve-submission-errors"></a>Résoudre les erreurs d’envoi

Si vous rencontrez des erreurs après avoir envoyé votre application au Windows Store, vous devez les résoudre pour poursuivre le [processus de certification](the-app-certification-process.md). Le message d’erreur indique la nature du problème et ce que vous devez faire pour le résoudre. Voici quelques informations supplémentaires pour vous aider à résoudre ces erreurs.

## <a name="uwp-apps"></a>Applications UWP

Si vous soumettez une application UWP, vous pouvez voir une erreur pendant le prétraitement si votre fichier de package n’est pas un fichier .msixupload ou .appxupload généré par Visual Studio pour le Store. N’oubliez pas que vous suivez les étapes de [empaqueter une application UWP avec Visual Studio](../packaging/packaging-uwp-apps.md) lors de la création du fichier de package de votre application et uniquement le chargement du fichier .msixupload ou .appxupload sur le [Packages](upload-app-packages.md) page de l’envoi, .msix/appx ou non .msixbundle/appxbundle.

Si une erreur de compilation s'affiche, assurez-vous que vous êtes en mesure de générer correctement votre application en mode Release. Pour plus d'informations, voir [Erreurs du compilateur natif interne .NET](https://go.microsoft.com/fwlink/p/?LinkID=613098).

## <a name="desktop-application"></a>Application de bureau

Si vous planifiez de soumettre un package qui contient les fichiers binaires de Win32 et UWP, assurez-vous que vous créez ce package en utilisant le projet d’empaquetage de Windows qui est disponible dans Visual Studio 2017 Update 4. Si vous créez le package à l’aide d’un modèle de projet UWP, vous n’êtes peut-être pas en mesure d’envoyer qui l’empaqueter pour le Store ou le chargement de version test sur les autres ordinateurs. Même si le package a été publié avec succès, il peut se comporter de façon inattendue sur des PC de l’utilisateur. Pour plus d’informations, consultez [empaqueter une application à l’aide de Visual Studio (pont du bureau)]( https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-packaging-dot-net).

## <a name="windows-phone-8x-and-earlier"></a>Windows Phone 8.x et versions antérieures

> [!IMPORTANT]
> À compter du 31 octobre 2018, produits nouvellement créé ne peut pas inclure les packages ciblant Windows Phone 8.x ou une version antérieure. Pour plus d’informations, consultez ce [billet de blog](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97).

L’**erreur 2001** peut se produire lorsque des problèmes sont détectés pendant le prétraitement des packages Windows Phone. Dans la plupart des cas, vous devez régénérer le package de votre application pour corriger l’erreur. Remplacez ensuite l'ancien package par le nouveau sur la page [Packages](upload-app-packages.md) de la soumission avant de cliquer de nouveau sur **Soumettre au Windows Store**.

Cette erreur peut avoir un certain nombre de causes. Passez en revue la liste ci-dessous pour identifier les problèmes qui peuvent s'appliquer à vos packages.

-   **Un ou plusieurs assemblys dans le package sont masqués de manière incorrecte :** Utilisez un autre outil pour effectuer l’obscurcissement, ou supprimez l’obscurcissement. Le processus de compilation optimise les assemblys obscurcis mais il arrive que des assemblys soient obscurcis à l'aide d'un outil qui modifie le code MSIL d'une façon qui n'est pas prise en charge et qui provoque une erreur.
-   **La taille d’une ou plusieurs méthodes dans l’application dépasse 256 Ko de langage intermédiaire :** Refactorisez la méthode concernée en fonctions plus petites. La taille du MSIL pour les méthodes dans un assembly peut être déterminée à l'aide de l'outil ILDASM.
-   **La validation de signature de nom fort a échoué pour un ou plusieurs assemblys :** Cette erreur se produit généralement lorsque la signature de nom fort a été effectuée à l’aide d’une clé différente de celle attendue dans les métadonnées d’assembly. Signez avec la clé correcte ou supprimez la signature de nom fort.
-   **Le package contient les assemblys en mode mixte (avec le code managé et natif) :** Les assemblys en mode mixte ne sont pas pris en charge sur Windows Phone. Supprimez les assemblys en mode mixte du package et soumettez de nouveau l'application.
-   **Un Windows Phone 8.1 XAP ou un assembly d’appx/appxbundle n’est pas valide :** Vérifiez que votre fichier .winmd a au moins un public point d’entrée. Vous pouvez utiliser n'importe quelle application de décompilation pour examiner le code et vérifier les points d'entrée publics si nécessaire.

Vous pouvez également rencontrer l’**erreur 1300** après avoir soumis votre application. Cette erreur se produit lorsqu’un ou plusieurs assemblys (ou la totalité du package) est déjà précompilé. Pour résoudre ce problème, recréez le package de l'application dans Microsoft Visual Studio, puis soumettez le package nouvellement généré.

## <a name="nameidentity-errors"></a>Erreurs de nom/identité

Si vous voyez une erreur qui indique que **le nom se trouve dans le package n’est pas un de vos noms d’applications réservés. Merci de réserver le nom d’application et/ou mettre à jour votre package avec le nom de l’application appropriée pour ce langage**, il est possible que vous avez entré un nom incorrect dans votre package. Cette erreur peut également se produire si vous utilisez un nom d’application que vous n’avez pas réservé dans l’espace partenaires. Vous pouvez généralement corriger cette erreur en procédant comme suit :

- Accédez à la page [Identité des applications](view-app-identity-details.md) pour votre application (sous **Gestion des applications**) pour vérifier si une identité est affectée à votre application. Si ce n’est pas le cas, une option vous permettra d’en créer une. Vous devez réserver un nom pour votre application afin de créer l’identité. Assurez-vous qu’il s’agit du nom que vous avez utilisé dans votre package.
- Si votre application possède déjà une identité, vous devrez peut-être quand même réserver le nom que vous voulez utiliser dans votre package. Sous **Gestion des applications**, cliquez sur [Gestion des noms d’application](manage-app-names.md). Entrez le nom que vous souhaitez utiliser, puis cliquez sur **Réserver le nom d’application**.

> [!IMPORTANT]
>  Si le nom que vous voulez utiliser n’est pas disponible, il se peut qu’une autre application l'ait déjà réservé. Si votre application est déjà publiée sous ce nom, ou si vous pensez que vous avez le droit d’utiliser ce nom, [contactez le support](https://go.microsoft.com/fwlink/p/?LinkId=331509).  

 

 




