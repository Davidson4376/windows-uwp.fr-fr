---
author: normesta
Description: This guide explains how to configure your Visual Studio Solution to edit, debug, and package desktop app for the Desktop Bridge.
Search.Product: eADQiWindows 10XVcnh
title: Créer un package d'application à l’aide de Visual Studio (Pont du bureau)
ms.author: normesta
ms.date: 08/30/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: d7ae77c499cb8398aa5557f0d422899fbe8b252d
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1816254"
---
# <a name="package-an-app-by-using-visual-studio-desktop-bridge"></a>Créer un package d'application à l’aide de Visual Studio (Pont du bureau)

Vous pouvez utiliser Visual Studio pour générer un package de votre application de bureau. Vous pouvez ensuite publier ce package sur le Store ou le charger de manière indépendante sur un ou plusieurs ordinateurs.

La dernière version de Visual Studio fournit une nouvelle version projet de création de packages qui élimine toutes les étapes manuelles autrefois nécessaires pour mettre en package votre application Il suffit d’ajouter un projet de création de packages, de référencer votre projet d'application de bureau, puis d'appuyer sur F5 pour déboguer votre application. Plus aucun ajustement n'est nécessaire. Cette nouvelle expérience plus fluide améliore considérable l'expérience de la version précédente de Visual Studio.

>[!IMPORTANT]
>Le Pont du bureau a été introduit dans Windows10, version1607 et peut être utilisé uniquement dans les projets qui ciblent la Mise à jour anniversaire Windows10 (version10.0; build 14393) ou une version ultérieure dans Visual Studio.

## <a name="first-consider-how-youll-distribute-your-app"></a>Commencez par examiner la méthode de distribution de votre application

Si vous prévoyez de publier votre application sur le [MicrosoftStore](https://www.microsoft.com/store/apps), commencez par remplir [ce formulaire](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge). Microsoft vous contactera pour démarrer le processus d’intégration. Dans le cadre de ce processus, vous devez réserver un nom dans le Store et obtenir les informations nécessaires pour créer le package de votre application.

En outre, assurez-vous de passer ce guide en revue avant de commencer à créer un package pour votre application: [Préparer le package d’une application (Pont du bureau)](desktop-to-uwp-prepare.md).

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

<iframe src="https://www.youtube.com/embed/fJkbYPyd08w" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="next-steps"></a>Étapes suivantes

**Trouvez des réponses à vos questions**

Des questions? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Transmettre des commentaires ou suggérer des fonctionnalités**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Exécuter, déboguer ou tester votre application**

Consultez [Exécuter, déboguer et tester une application de bureau mise en package (Pont du bureau)](desktop-to-uwp-debug.md)

**Améliorer votre application de bureau en ajoutant des API UWP**

Consultez [Améliorer votre application de bureau pour Windows10](desktop-to-uwp-enhance.md)

**Étendre votre application de bureau en ajoutant des projets UWP et des composants Windows Runtime**

Consultez [Étendre votre application de bureau avec des composants UWP modernes](desktop-to-uwp-extend.md).

**Distribuer votre application**

Consultez [Distribuer une application de bureau empaquetée (Pont du bureau)](desktop-to-uwp-distribute.md)
