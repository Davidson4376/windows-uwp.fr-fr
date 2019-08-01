---
Description: Si vous rencontrez des erreurs après avoir envoyé votre application au Windows Store, vous devez les résoudre pour poursuivre le processus de certification.
title: Résoudre les erreurs d’envoi
ms.assetid: 68199E09-0C66-4EB4-BFE8-D2EEB139C4F3
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 63bb2ae4d79266a28cf92434e0018dbaedd6a73f
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682705"
---
# <a name="resolve-submission-errors"></a>Résoudre les erreurs d’envoi

Si vous rencontrez des erreurs après avoir envoyé votre application au Windows Store, vous devez les résoudre pour poursuivre le [processus de certification](the-app-certification-process.md). Le message d’erreur indique la nature du problème et ce que vous devez faire pour le résoudre. Voici quelques informations supplémentaires pour vous aider à résoudre ces erreurs.

## <a name="uwp-apps"></a>Applications UWP

Si vous soumettez une application UWP, vous pouvez voir une erreur lors du prétraitement si votre fichier de package n’est pas un fichier. msixupload ou. appxupload généré par Visual Studio pour le Windows Store. Veillez à suivre les étapes de la section empaqueter [une application UWP avec Visual Studio](/windows/msix/package/packaging-uwp-apps) lors de la création du fichier de package de votre application, et de télécharger uniquement le fichier. msixupload ou. appxupload sur la page [packages](upload-app-packages.md) de l’envoi, et non un fichier. msix/AppX ou. msixbundle/appxbundle .

Si une erreur de compilation s'affiche, assurez-vous que vous êtes en mesure de générer correctement votre application en mode Release. Pour plus d'informations, voir [Erreurs du compilateur natif interne .NET](https://go.microsoft.com/fwlink/p/?LinkID=613098).

## <a name="desktop-application"></a>Application de bureau

Si vous envisagez de soumettre un package qui contient à la fois des fichiers binaires Win32 et UWP, veillez à créer ce package à l’aide du projet de Packaging Windows disponible dans Visual Studio 2017 Update 4 et versions ultérieures. Si vous créez le package à l’aide d’un modèle de projet UWP, vous ne pourrez peut-être pas envoyer ce package au magasin ou l’chargement sur d’autres PC. Même si le package est correctement publié, il peut se comporter de manière inattendue sur le PC de l’utilisateur. Pour plus d’informations, consultez empaqueter [une application à l’aide de Visual Studio (Bridge Desktop)]( https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-packaging-dot-net).

## <a name="windows-phone-8x-and-earlier"></a>Windows Phone 8. x et versions antérieures

> [!IMPORTANT]
> Depuis le 31 octobre 2018, les nouveaux produits ne peuvent pas inclure des packages ciblant Windows Phone 8. x ou une version antérieure. Pour plus d’informations, consultez ce billet de [blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

L’**erreur 2001** peut se produire lorsque des problèmes sont détectés pendant le prétraitement des packages Windows Phone. Dans la plupart des cas, vous devez régénérer le package de votre application pour corriger l’erreur. Remplacez ensuite l'ancien package par le nouveau sur la page [Packages](upload-app-packages.md) de la soumission avant de cliquer de nouveau sur **Soumettre au Windows Store**.

Cette erreur peut avoir un certain nombre de causes. Passez en revue la liste ci-dessous pour identifier les problèmes qui peuvent s'appliquer à vos packages.

-   **Un ou plusieurs assemblys du package sont obscurcis de manière incorrecte:** Utilisez un autre outil pour effectuer l’obscurcissement, ou supprimez l’obscurcissement. Le processus de compilation optimise les assemblys obscurcis mais il arrive que des assemblys soient obscurcis à l'aide d'un outil qui modifie le code MSIL d'une façon qui n'est pas prise en charge et qui provoque une erreur.
-   **La taille d’une ou plusieurs méthodes de l’application dépasse 256 Ko de l’IL:** Refactorisez la méthode incriminée en fonctions plus petites. La taille du MSIL pour les méthodes dans un assembly peut être déterminée à l'aide de l'outil ILDASM.
-   **La validation de la signature de nom fort a échoué pour un ou plusieurs assemblys:** Cette erreur se produit généralement lorsque la signature de nom fort a été effectuée à l’aide d’une clé différente de celle attendue dans les métadonnées de l’assembly. Signez avec la clé correcte ou supprimez la signature de nom fort.
-   **Le package contient des assemblys en mode mixte (avec code managé et natif):** Les assemblys en mode mixte ne sont pas pris en charge sur les Windows Phone. Supprimez les assemblys en mode mixte du package et soumettez de nouveau l'application.
-   **Un assembly Windows Phone 8,1 XAP ou AppX/appxbundle n’est pas valide:** Assurez-vous que votre fichier. winmd possède au moins un point d’entrée public. Vous pouvez utiliser n'importe quelle application de décompilation pour examiner le code et vérifier les points d'entrée publics si nécessaire.

Vous pouvez également rencontrer l’**erreur 1300** après avoir soumis votre application. Cette erreur se produit lorsqu’un ou plusieurs assemblys (ou la totalité du package) est déjà précompilé. Pour résoudre ce problème, recréez le package de l'application dans Microsoft Visual Studio, puis soumettez le package nouvellement généré.

## <a name="nameidentity-errors"></a>Erreurs de nom/identité

Si vous voyez une erreur indiquant que **le nom trouvé dans le package ne correspond pas à l’un de vos noms d’application réservés. Veuillez réserver le nom de l’application et/ou mettre à jour votre package avec le nom d'** application correct pour cette langue. cela peut être dû au fait que vous avez entré un nom incorrect dans votre package. Cette erreur peut également se produire si vous utilisez un nom d’application que vous n’avez pas réservé dans l’espace partenaires. Vous pouvez généralement corriger cette erreur en procédant comme suit :

- Accédez à la page [Identité des applications](view-app-identity-details.md) pour votre application (sous **Gestion des applications**) pour vérifier si une identité est affectée à votre application. Si ce n’est pas le cas, une option vous permettra d’en créer une. Vous devez réserver un nom pour votre application afin de créer l’identité. Assurez-vous qu’il s’agit du nom que vous avez utilisé dans votre package.
- Si votre application possède déjà une identité, vous devrez peut-être quand même réserver le nom que vous voulez utiliser dans votre package. Sous **Gestion des applications**, cliquez sur [Gestion des noms d’application](manage-app-names.md). Entrez le nom que vous souhaitez utiliser, puis cliquez sur **Réserver le nom d’application**.

> [!IMPORTANT]
>  Si le nom que vous voulez utiliser n’est pas disponible, il se peut qu’une autre application l'ait déjà réservé. Si votre application est déjà publiée sous ce nom, ou si vous pensez que vous avez le droit d’utiliser ce nom, [contactez le support](https://go.microsoft.com/fwlink/p/?LinkId=331509).  

 

 




