---
description: Ce didacticiel montre comment ajouter des interfaces utilisateur en XAML UWP, créer des packages MSIX et intégrer d’autres composants modernes à votre application WPF.
title: Créer un package et déployer avec MSIX
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, îlots XAML
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 6f5c01b23f02bb9c116ddaaec698612aa539539d
ms.sourcegitcommit: e9dc2711f0a0758727468f7ccd0d0f0eee3363e3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69979351"
---
# <a name="part-5-package-and-deploy-with-msix"></a>Partie 5 : Créer un package et déployer avec MSIX

Il s’agit de la dernière partie d’un didacticiel qui montre comment moderniser un exemple d’application de bureau WPF nommée Contoso depenses. Pour obtenir une vue d’ensemble du didacticiel, des conditions préalables et des instructions pour télécharger l’exemple [d’application, consultez le didacticiel: Moderniser une application](modernize-wpf-tutorial.md)WPF. Cet article suppose que vous avez déjà effectué la [partie 4](modernize-wpf-tutorial-4.md).

Dans la [partie 4](modernize-wpf-tutorial-4.md) , vous avez appris que certaines API WinRT, y compris l’API notifications, nécessitent une identité de package avant de pouvoir être utilisées dans une application. Vous pouvez obtenir l’identité du package en empaquetant les dépenses de contoso à l’aide de [MSIX](https://docs.microsoft.com/windows/msix), le format d’empaquetage introduit dans Windows 10 pour empaqueter et déployer des applications Windows. MSIX offre des avantages pour les développeurs et les professionnels de l’informatique, notamment:

- Utilisation optimisée du réseau et espace de stockage.
- Terminez la désinstallation, grâce à un conteneur léger dans lequel l’application est exécutée. Les clés de Registre et les fichiers temporaires ne sont pas conservés sur le système.
- Découple les mises à jour du système d’exploitation des mises à jour et des personnalisations de l’application.
- Simplifie le processus d’installation, de mise à jour et de désinstallation.

Dans cette partie du didacticiel, vous allez apprendre à empaqueter l’application Contoso depenses dans un package MSIX.

## <a name="package-the-application"></a>Empaqueter l’application

Visual Studio 2019 offre un moyen simple d’empaqueter une application de bureau à l’aide du projet de packaging des applications Windows. 

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la solution **ContosoExpenses** et choisissez **Ajouter-> nouveau projet**.

    ![Ajouter un nouveau projet](images/wpf-modernize-tutorial/AddNewProject.png)

3. Dans la boîte de dialogue **Ajouter un nouveau projet** , recherchez `packaging`, choisissez le modèle projet de création de **packages d’applications Windows** dans la C# catégorie, puis cliquez sur **suivant**.

    ![Projet de création de packages d’application Windows](images/wpf-modernize-tutorial/WAP.png)

4. Nommez le nouveau `ContosoExpenses.Package` projet, puis cliquez sur **créer**.

5. Sélectionnez **Windows 10, version 1903 (10,0; Générez 18362)** pour la **version cible** et la **version minimale** , puis cliquez sur **OK**.

    Le projet **ContosoExpenses. Package** est ajouté à la solution **ContosoExpenses** . Ce projet comprend un [manifeste de package](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root), qui décrit l’application, ainsi que des ressources par défaut qui sont utilisées pour les éléments tels que l’icône dans le menu programmes et la vignette dans l’écran d’accueil. Toutefois, contrairement à un projet UWP, le projet de Packaging ne contient pas de code. Son objectif est d’empaqueter une application de bureau existante.

6. Dans le projet **ContosoExpenses. Package** , cliquez avec le bouton droit sur le nœud **applications** et choisissez **Ajouter une référence**. Ce nœud spécifie les applications de votre solution qui seront incluses dans le package.

6. Dans la liste des projets, sélectionnez **ContosoExpenses. Core** , puis cliquez sur **OK**.

7. Développez le nœud **applications** et vérifiez que le projet **ContosoExpense. Core** est référencé et mis en surbrillance en gras. Cela signifie qu’il sera utilisé comme point de départ pour le package.

8. Cliquez avec le bouton droit sur le projet **ContosoExpenses. Package** , puis choisissez **définir comme projet de démarrage**.

9. Dans Explorateur de solutions, cliquez avec le bouton droit sur le nœud de projet **ContosoExpenses. Package** , puis sélectionnez **modifier le fichier projet**.

10. Recherchez l’élément `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` dans le fichier.

11. Remplacez cet élément par le code XML suivant.

    ``` xml
    <ItemGroup>
        <SDKReference Include="Microsoft.VCLibs,Version=14.0">
        <TargetedSDKConfiguration Condition="'$(Configuration)'!='Debug'">Retail</TargetedSDKConfiguration>
        <TargetedSDKConfiguration Condition="'$(Configuration)'=='Debug'">Debug</TargetedSDKConfiguration>
        <TargetedSDKArchitecture>$(PlatformShortName)</TargetedSDKArchitecture>
        <Implicit>true</Implicit>
        </SDKReference>
    </ItemGroup>
    <Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />
    <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
        <ItemGroup>
        <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
        <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
        <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
            <SourceProject></SourceProject>
            <TargetPath Condition="'%(FileName)%(Extension)'=='resources.pri'">app_resources.pri</TargetPath>
        </_FilteredNonWapProjProjectOutput>
        </ItemGroup>
    </Target>
    ```

12. Enregistrez le fichier projet et fermez-le.

13. Appuyez sur **F5** pour démarrer l’application empaquetée dans le débogueur.

À ce stade, vous pouvez remarquer des modifications qui indiquent que l’application s’exécute maintenant en tant que package:

- L’icône dans la barre des tâches ou dans le menu Démarrer est désormais la ressource par défaut qui est incluse dans chaque **projet de packaging des applications Windows**.
- Si vous cliquez avec le bouton droit sur l’application **ContosoExpense. Package** répertoriée dans le menu Démarrer, vous remarquerez des options qui sont généralement réservées pour les applications téléchargées à partir du Microsoft Store, telles que les **paramètres d’application**, le **tarif et la révision** et le **partage** .

    ![ContosoExpenses dans le menu Démarrer](images/wpf-modernize-tutorial/StartMenu.png)

- Si vous souhaitez désinstaller l’application, cliquez avec le bouton droit sur **ContosoExpense. Package** dans le menu Démarrer,puis choisissez désinstaller. L’application est immédiatement supprimée, sans laisser de reste sur le système.

## <a name="test-the-notification"></a>Tester la notification

Maintenant que vous avez empaqueté l’application Contoso depenses avec MSIX, vous pouvez tester le scénario de notification qui ne fonctionnait pas à la fin de la [partie 4](modernize-wpf-tutorial-4.md).

1. Dans l’application Contoso Expenses, choisissez un employé dans la liste, puis cliquez sur le bouton **Ajouter une nouvelle dépense** . 
2. Complétez tous les champs du formulaire, puis cliquez sur **Enregistrer**.
3. Vérifiez que vous voyez une notification du système d’exploitation.

![Notification Toast](images/wpf-modernize-tutorial/ToastNotification.png)
