---
title: Création d’un composant Windows Runtime de base en C++ et appel de ce composant à partir de JavaScript ou C#
description: Cette procédure pas à pas indique comment créer une DLL de composant Windows Runtime de base qui peut être appelée à partir de JavaScript, C# ou Visual Basic.
ms.assetid: 764CD9C6-3565-4DFF-88D7-D92185C7E452
---

# Procédure pas à pas : création d’un composant Windows Runtime de base en C++ et appel de ce composant à partir de JavaScript ou C\#


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


\[Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.\]

Cette procédure pas à pas indique comment créer une DLL de composant Windows Runtime de base qui peut être appelée à partir de JavaScript, C\# ou Visual Basic. Avant d’entreprendre cette procédure pas à pas, vous devez maîtriser des concepts tels que l’interface binaire abstraite (ABI), les classes ref et les extensions des composants Visual C++ qui facilitent l’utilisation des classes ref. Pour plus d’informations, consultez les articles [Création de composants Windows Runtime en C++](creating-windows-runtime-components-in-cpp.md) et [Informations de référence sur le langage Visual C++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx).

## Création de la DLL du composant C++


Dans cet exemple, nous commençons par créer le projet du composant, mais vous pouvez très bien créer le projet JavaScript en premier. L’ordre n’a pas d’importance.

Notez que la classe principale du composant contient des exemples de définitions de propriété et de méthode, ainsi qu’une déclaration d’événement. Ces éléments sont fournis uniquement pour vous en montrer le fonctionnement. Ils ne sont pas obligatoires, et dans cet exemple, nous remplacerons l’ensemble du code généré par notre propre code.

## **Pour créer le projet du composant C++**

1.  Dans la barre de menus Visual Studio, choisissez **Fichier > Nouveau > Projet**.
2.  Dans le volet gauche de la boîte de dialogue **Nouveau projet**, développez **Visual C++**, puis sélectionnez le nœud des applications Windows universelles.
3.  Dans le volet central, sélectionnez **Composant Windows Runtime**, puis nommez le projet WinRT\_CPP.
4.  Cliquez sur le bouton **OK**.

## **Pour ajouter une classe activable au composant**

1.  Une classe activable est une classe que le code client peut créer à l’aide d’une expression **new** (**New** en Visual Basic ou **ref new** en C++). Dans votre composant, vous devez la déclarer sous la forme **public ref class sealed**. En fait, les fichiers Class1.h et .cpp disposent déjà d’une classe ref. Vous pouvez changer le nom, mais dans cet exemple, nous utiliserons celui par défaut : Class1. Vous pouvez définir des classes ref ou standard supplémentaires dans votre composant si nécessaire. Pour plus d’informations sur les classes ref, voir [Système de types (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx).

2.  Ajoutez ces directives \#include à Class1.h :

    ```cpp
             private void PrimesUnOrderedButton_Click_1(object sender, RoutedEventArgs e)
            {
                var nativeObject = new WinRT_CPP.Class1();

                StringBuilder sb = new StringBuilder();
                sb.Append("Primes found (unordered): ");
                PrimesUnOrderedResult.Text = sb.ToString();

                // primeFoundEvent is a user-defined event in nativeObject
                // It passes the results back to this thread as they are produced
                // and the event handler that we define here immediately displays them.
                nativeObject.primeFoundEvent += (n) =>
                {
                    sb.Append(n.ToString()).Append(" ");
                    PrimesUnOrderedResult.Text = sb.ToString();
                };

                // Call the async method.
                var asyncResult = nativeObject.GetPrimesUnordered(2, 100000);

                // Provide a handler for the Progress event that the asyncResult
                // object fires at regular intervals. This handler updates the progress bar.
                asyncResult.Progress += (asyncInfo, progress) =>
                    {
                        PrimesUnOrderedProgress.Value = progress;
                    };
            }

            private void Clear_Button_Click(object sender, RoutedEventArgs e)
            {
                PrimesOrderedProgress.Value = 0;
                PrimesUnOrderedProgress.Value = 0;
                PrimesUnOrderedResult.Text = "";
                PrimesOrderedResult.Text = "";
                Result1.Text = "";
            }
    ```

## Exécution de l’application


Sélectionnez le projet C\# ou le projet JavaScript en tant que projet de démarrage en ouvrant le menu contextuel du nœud de projet dans l’Explorateur de solutions et en sélectionnant **Définir comme projet de démarrage**. Appuyez ensuite sur F5 pour une exécution avec débogage ou sur Ctrl+F5 pour une exécution sans débogage.

## Examen de votre composant dans l’Explorateur d’objets (facultatif)


Dans l’Explorateur d’objets, vous pouvez examiner tous les types Windows Runtime définis dans les fichiers.winmd. Cela inclut les types de l’espace de noms Platform et de l’espace de noms par défaut. Toutefois, étant donné que les types de l’espace de noms Platform::Collections sont définis dans le fichier d’en-tête collections.h et non dans un fichier winmd, ils n’apparaissent pas dans l’Explorateur d’objets.

## **Pour examiner un composant**

1.  Dans la barre de menus, choisissez **Afficher, Explorateur d’objets** (Ctrl+Alt+J).
2.  Dans le volet gauche de l’Explorateur d’objets, développez le nœud WinRT\_CPP afin d’afficher les types et les méthodes définis dans votre composant.

## Conseils de débogage


Pour optimiser le débogage, téléchargez les symboles de débogage à partir des serveurs de symboles publics de Microsoft :

## **Pour télécharger les symboles de débogage**

1.  Dans la barre de menus, choisissez **Outils, Options**.
2.  Dans la boîte de dialogue **Options**, développez **Débogage** et sélectionnez **Symboles**.
3.  Sélectionnez **Serveurs de symboles Microsoft**, puis cliquez sur le bouton **OK**.

Le téléchargement des symboles peut prendre un certain temps la première fois. Pour accélérer les performances la prochaine fois que vous appuierez sur F5, spécifiez un répertoire local dans lequel mettre en cache les symboles.

Lorsque vous déboguez une solution JavaScript qui contient une DLL de composant, vous pouvez configurer le débogueur de manière à activer l’exécution pas à pas du script ou du code natif du composant, mais pas les deux à la fois. Pour changer ce paramètre, ouvrez le menu contextuel du nœud de projet JavaScript dans l’Explorateur de solutions, puis sélectionnez **Propriétés, Débogage, Type de débogueur**.

Veillez à sélectionner les fonctionnalités appropriées dans le concepteur de packages. Vous pouvez lancer le concepteur de packages en ouvrant le fichier Package.appxmanifest. Par exemple, si vous essayez d’accéder par programmation à des fichiers du dossier Images, veillez à activer la case à cocher **Bibliothèque d’images** dans le volet **Capacités** du concepteur de packages.

Si votre code JavaScript ne reconnaît pas les propriétés ou méthodes publiques du composant, assurez-vous que vous utilisez la casse mixte dans JavaScript. Par exemple, la méthode C++ `ComputeResult` doit être référencée sous la forme `computeResult` dans JavaScript.

Si vous supprimez un projet de composant Windows Runtime C++ dans une solution, vous devez également supprimer manuellement la référence de ce projet dans le projet JavaScript. Sinon, il ne sera plus possible d’effectuer d’opérations de débogage ou de génération. Si nécessaire, ajoutez ensuite une référence d’assembly à la DLL.

## Rubriques connexes

* [Création de composants Windows Runtime en C++](creating-windows-runtime-components-in-cpp.md)

<!--HONumber=Mar16_HO1-->
