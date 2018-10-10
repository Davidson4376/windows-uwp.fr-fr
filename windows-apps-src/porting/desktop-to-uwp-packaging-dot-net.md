---
author: normesta
Description: This guide explains how to configure your Visual Studio Solution to edit, debug, and package desktop application.
Search.Product: eADQiWindows 10XVcnh
title: Empaqueter une application de bureau à l’aide de Visual Studio
ms.author: normesta
ms.date: 08/30/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: 2c9b7a30a50c26d2dbdaf6df04e85549addaf181
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4502803"
---
# <a name="package-a-desktop-application-by-using-visual-studio"></a>Empaqueter une application de bureau à l’aide de Visual Studio

Vous pouvez utiliser Visual Studio pour générer un package de votre application de bureau. Vous pouvez ensuite publier ce package sur le Store ou le charger de manière indépendante sur un ou plusieurs ordinateurs.

La dernière version de Visual Studio fournit une nouvelle version projet de création de packages qui élimine toutes les étapes manuelles autrefois nécessaires pour mettre en package votre application Il suffit d’ajouter un projet de création de packages, de référencer votre projet d'application de bureau, puis d'appuyer sur F5 pour déboguer votre application. Plus aucun ajustement n'est nécessaire. Cette nouvelle expérience plus fluide améliore considérable l'expérience de la version précédente de Visual Studio.

>[!IMPORTANT]
>La possibilité de créer un package d’application Windows pour votre application de bureau (dans le cas contraire, connu sous le pont du bureau, le nom a été introduit dans Windows 10, version 1607, et peut être utilisé uniquement dans les projets qui ciblent la mise à jour anniversaire Windows 10 (version 10.0; Build 14393) ou une version ultérieure dans Visual Studio.

## <a name="first-prepare-your-application"></a>Tout d'abord, préparez votre application

Consultez ce guide avant de commencer la création d’un package pour votre application: [préparer pour empaqueter une application de bureau](desktop-to-uwp-prepare.md).

<a id="new-packaging-project"/>

## <a name="create-a-package"></a>Créer un package

1. Dans Visual Studio, ouvrez la solution qui contient votre projet d'application du bureau.

2. Ajoutez un **Projet de création de package d'application Windows** à votre solution.

   Vous n’aurez pas à lui ajouter du code. Il vous sert seulement à générer un package. Nous allons désigner ce projet par l'expression «projet de création de package».

   ![Projet de création de package](images/desktop-to-uwp/packaging-project.png)

   >[!NOTE]
   >Ce projet apparaît uniquement dans la version 15.5 ou supérieure de VisualStudio2017.

3. Définissez la **Version cible** de ce projet sur la version de votre choix, mais veillez à définir la **Version minimum** sur **Mise à jour anniversaire Windows10**.

   ![Boîte de dialogue du sélecteur de version de création de package](images/desktop-to-uwp/packaging-version.png)

4. Dans le projet de mise en package, cliquez avec le bouton droit sur le dossier **Applications**, puis sélectionnez sur **Ajouter une référence**.

   ![Ajouter une référence de projet](images/desktop-to-uwp/add-project-reference.png)

5. Choisissez votre projet d'application de bureau, puis cliquez sur le bouton **OK**.

   ![Projet de bureau](images/desktop-to-uwp/reference-project.png)

   Vous pouvez inclure plusieurs applications de bureau à votre package, mais une seule d'entre elles démarre lorsque l'utilisateur sélectionne voter vignette d'application. Dans le nœud **Applications**, cliquez avec le bouton droit sur l'application qui doit démarrer lorsque les utilisateurs sélectionnent la vignette d'application, puis sélectionnez **Définir en tant que point d’entrée**.

   ![Définir le point d’entrée](images/desktop-to-uwp/entry-point-set.png)

6. Générez le projet de création de package pour vous assurer qu’aucune erreur n’apparaît.

7. Utilisez l'assistant [Créer des packages d'application](../packaging/packaging-uwp-apps.md) pour générer un fichier appxupload.

   Vous pouvez charger ce fichier directement sur le Store.

**Vidéo**

&nbsp;
> [!VIDEO https://www.youtube.com/embed/fJkbYPyd08w]

## <a name="next-steps"></a>Étapes suivantes

**Trouvez des réponses à vos questions**

Des questions? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Transmettre des commentaires ou suggérer des fonctionnalités**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Exécuter, déboguer ou tester votre application de bureau**

Voir [Exécuter, déboguer et tester une application de bureau empaquetée](desktop-to-uwp-debug.md)

**Améliorer votre application de bureau en ajoutant des API UWP**

Consultez [Améliorer votre application de bureau pour Windows10](desktop-to-uwp-enhance.md)

**Étendre votre application de bureau en ajoutant des projets UWP et les composants Windows Runtime**

Consultez [Étendre votre application de bureau avec des composants UWP modernes](desktop-to-uwp-extend.md).

**Distribuer votre application**

Voir [distribuer une application de bureau empaquetée](desktop-to-uwp-distribute.md)
