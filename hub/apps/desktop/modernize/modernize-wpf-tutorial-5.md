---
description: Ce didacticiel montre comment ajouter des interfaces utilisateur UWP XAML, créer des packages MSIX et incorporer d’autres composants modernes dans votre application WPF.
title: Empaqueter et déployer avec MSIX
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, uwp, WinForms, wpf, xaml (îles)
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: d11ef296b690297d33ebd5d366c2594f70b6d10b
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420098"
---
# <a name="part-5-package-and-deploy-with-msix"></a>Partie 5 : Empaqueter et déployer avec MSIX

Il s’agit de la dernière partie d’un didacticiel qui montre comment moderniser une application de bureau WPF exemple nommée dépenses de Contoso. Pour une vue d’ensemble du didacticiel, configuration requise et des instructions pour télécharger l’exemple d’application, consultez [didacticiel : Moderniser une application WPF](modernize-wpf-tutorial.md). Cet article suppose que vous avez déjà effectué [partie 4](modernize-wpf-tutorial-4.md).

Dans [partie 4](modernize-wpf-tutorial-4.md) vous avez appris que certaines API WinRT, y compris les notifications d’API, nécessite une identité du package avant de pouvoir être utilisés dans une application. Vous pouvez obtenir l’identité du package en plaçant des dépenses de Contoso à l’aide de [MSIX](https://docs.microsoft.com/windows/msix), le format d’empaquetage introduit dans Windows 10 pour empaqueter et déployer des applications de Windows. MSIX offre des avantages à la table à la fois pour les développeurs et professionnels de l’informatique, notamment :

- Espace de stockage et l’utilisation réseau optimisée.
- Effectuer le nettoyage désinstaller, grâce à un conteneur léger, où l’application est exécutée. Aucune des clés de Registre et les fichiers temporaires ne sont conservés sur le système.
- Dissocie les mises à jour du système d’exploitation à partir des mises à jour de l’application et les personnalisations.
- Simplifie l’installation, mettre à jour et les processus de désinstallation. 

Dans cette partie du didacticiel, vous apprendrez comment empaqueter l’application de dépenses de Contoso dans un package MSIX.

## <a name="package-the-application"></a>Empaqueter l’application

Visual Studio 2019 fournit un moyen simple pour empaqueter une application de bureau en utilisant le projet de Packaging d’Application Windows. 

1. Dans **l’Explorateur de solutions**, avec le bouton droit le **ContosoExpenses** solution et choisissez **Ajouter -> Nouveau projet**.

    ![Ajouter un nouveau projet](images/wpf-modernize-tutorial/AddNewProject.png)

3. Dans le **ajouter un nouveau projet** boîte de dialogue, recherchez `packaging`, choisissez le **projet d’empaquetage Windows Application** modèle de projet dans le C# catégorie, puis cliquez sur **suivant** .

    ![Projet de package d’Application Windows](images/wpf-modernize-tutorial/WAP.png)

4. Nommez le nouveau projet `ContosoExpenses.Package` et cliquez sur **créer**.

5. Sélectionnez **Windows 10, version 1903 (10.0 ; Build 18362)** à la fois pour le **version cible** et **version minimale** et cliquez sur **OK**.

    Le **ContosoExpenses.Package** projet est ajouté à la **ContosoExpenses** solution. Ce projet comprend un [manifeste du package](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root), qui décrit l’application et des ressources par défaut qui sont utilisés pour les éléments tels que l’icône dans le menu de programmes et de la vignette dans l’écran d’accueil. Toutefois, contrairement à un projet UWP, le projet d’empaquetage ne contient pas code. Son objectif consiste à empaqueter une application de bureau existante.

6. Dans le **ContosoExpenses.Package** de projet, cliquez sur le **Applications** nœud et choisissez **ajouter une référence**. Ce nœud spécifie les applications dans votre solution doit être incluses dans le package.

7. Dans la liste des projets, sélectionnez **ContosoExpenses.Core** et cliquez sur **OK**.

8. Développez le **Applications** nœud et vérifiez que le le **ContosoExpense.Core** projet est référencé et mis en surbrillance en gras. Cela signifie qu’il sera utilisé comme point de départ pour le package.

9. Cliquez sur le **ContosoExpenses.Package** projet, puis sélectionnez **définir comme projet de démarrage**.

10. Appuyez sur **F5** pour démarrer l’application empaquetée dans le débogueur.

À ce stade, vous pouvez remarquer des modifications qui indiquent que l’application est maintenant empaqueté en cours d’exécution en tant que :

- L’icône dans la barre des tâches ou dans le menu Démarrer est désormais la ressource par défaut qui est incluse dans chaque **projet d’empaquetage Windows Application**.
- Si vous cliquez sur le **ContosoExpense.Package** application répertoriée dans le menu Démarrer, vous remarquerez que les options qui sont généralement réservées pour les applications téléchargées depuis le Microsoft Store, telles que **paramètres de l’application**, **Taux et révision** et **partage**.

    ![ContosoExpenses dans le Menu Démarrer](images/wpf-modernize-tutorial/StartMenu.png)

- Si vous souhaitez désinstaller l’application, vous pouvez cliquer sur **ContosoExpense.Package** dans le menu Démarrer et choisissez **désinstallation**. L’application est immédiatement supprimée, sans quitter tout montant restant sur le système.

## <a name="test-the-notification"></a>Tester la notification

Maintenant que vous avez empaqueté l’application de dépenses de Contoso avec MSIX, vous pouvez tester le scénario de notification qui n’était pas de travail à la fin de [partie 4](modernize-wpf-tutorial-4.md).

1. Dans l’application de dépenses de Contoso, choisissez un employé dans la liste, puis sélectionnez le **ajouter nouvelle dépense** bouton. 
2. Renseignez tous les champs dans le formulaire et appuyez sur **enregistrer**.
3. Vérifiez que vous voyez une notification du système d’exploitation.

![Notification toast](images/wpf-modernize-tutorial/ToastNotification.png)
