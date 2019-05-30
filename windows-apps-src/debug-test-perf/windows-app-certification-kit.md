---
ms.assetid: 78D833B9-E528-4BCA-9C48-A757F17E6C22
title: Kit de certification des applications Windows
description: Pour maximiser les chances de publication sur le Microsoft Store ou de certification Windows de votre application, validez-la et testez-la sur votre ordinateur avant de l’envoyer pour certification. Cette rubrique explique comment installer et exécuter le Kit de certification des applications Windows.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, certification des applications
ms.localizationpriority: medium
ms.openlocfilehash: ea84d57fbfba9666a2553500dc94275bd8b4d209
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359750"
---
# <a name="windows-app-certification-kit"></a>Kit de certification des applications Windows



Pour obtenir votre application [certifiés Windows](https://msdn.microsoft.com/windows/desktop/jj134964.aspx) ou préparer pour [publication pour le Microsoft Store](https://docs.microsoft.com/windows/uwp/publish/app-submissions), vous devez valider et testez-le d’abord localement. Cette rubrique vous montre comment installer et exécuter le [Kit de Certification des applications Windows](https://go.microsoft.com/fwlink/p/?LinkID=309666) pour vérifier votre application est sécurisé et efficace.

## <a name="prerequisites"></a>Prérequis

Conditions préalables pour tester une application Windows universelle :

-   Vous devez installer et exécuter Windows 10.
-   Vous devez installer [version du Kit de Certification des applications Windows 10]( https://go.microsoft.com/fwlink/p/?LinkID=309666), qui est inclus dans le Kit de développement logiciel (SDK) Windows pour Windows 10.
-   Vous devez [activer votre appareil pour le développement](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).
-   Vous devez déployer l’application Windows que vous voulez tester sur votre ordinateur.

**Remarque à propos des mises à niveau sur place**

L’installation d’une nouvelle version du [Kit de certification des applications Windows]( https://go.microsoft.com/fwlink/p/?LinkID=309666) remplace toute version précédente du Kit qui est installée sur l’ordinateur.

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-interactively"></a>Validez votre application Windows de manière interactive à l’aide du Kit de certification des applications Windows

1.  À partir du menu **Démarrer**, recherchez **Applications**, **Kits Windows**, puis cliquez sur **Kit de certification des applications Windows**.

2.  Dans le Kit de certification des applications Windows, sélectionnez la catégorie de validation que vous souhaitez effectuer. Exemple : Si vous validez une application Windows, sélectionnez **valider une application Windows**.

    Vous pouvez accéder directement à l’application que vous testez ou choisir l’application dans une liste depuis l’interface utilisateur. Quand le Kit de certification des applications Windows est exécuté pour la première fois, l’interface utilisateur répertorie toutes les applications Windows que vous avez installées sur votre ordinateur. Pour toutes les exécutions ultérieures, l’interface utilisateur affiche les applications Windows les plus récentes que vous avez validées. Si l’application que vous voulez tester n’est pas répertoriée, vous pouvez cliquer sur **Mon application n’est pas répertoriée** pour obtenir une liste exhaustive de toutes les applications installées sur votre système.

3.  Une fois que vous avez entré ou sélectionné l’application que vous voulez tester, cliquez sur **Suivant**.

4.  À partir de l’écran suivant, vous verrez le flux de travail de test correspondant au type d’application que vous testez. Les tests grisés dans la liste ne sont pas applicables à votre environnement. Par exemple, si vous testez une application Windows 10 sous Windows 7, seuls les tests statiques s’appliqueront au flux de travail. Notez que le Microsoft Store peuvent s’appliquer tous les tests à partir de ce flux de travail. Sélectionnez les tests que vous souhaitez exécuter et cliquez sur **Suivant**.

    Le Kit de certification des applications Windows commence la validation de l’application.

5.  À l’invite après le test, entrez le chemin d’accès du dossier où vous voulez enregistrer le rapport de test.

    Le Kit de certification des applications Windows crée un fichier HTML, ainsi qu’un rapport XML et l’enregistre dans ce dossier.

6.  Ouvrez le fichier de rapport et examinez les résultats du test.

**Remarque**  si vous utilisez Visual Studio, vous pouvez exécuter le Kit de Certification des applications de Windows lorsque vous créez votre package d’application. Pour plus d’informations, voir [Création de packages d’application UWP](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

 

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-from-a-command-line"></a>Validez votre application Windows à l’aide du Kit de certification des applications Windows à partir d’une ligne de commande

**Important**  le Kit de Certification des applications Windows doit être exécuté dans le contexte d’une session utilisateur active.

1.  Dans la fenêtre de commande, accédez au répertoire contenant le Kit de certification des applications Windows.

    **Remarque**    le chemin d’accès par défaut est C:\\Program Files\\Windows Kits\\10\\Kit de Certification des applications\\.

2.  Entrez les commandes suivantes dans cet ordre pour tester une application qui est déjà installée sur votre ordinateur de test :

    `appcert.exe reset`

    `appcert.exe test -packagefullname [package full name] -reportoutputpath [report file name]`

    Vous pouvez également utiliser les commandes suivantes si l’application n’est pas installée. Le Kit de certification des applications Windows ouvre le package et applique le flux de travail de test approprié :

    `appcert.exe reset`

    `appcert.exe test -appxpackagepath [package path] -reportoutputpath [report file name]`

3.  Une fois le test terminé, ouvrez le fichier de rapport nommé `[report file name]` et examinez les résultats du test.

**Remarque**  le Kit de Certification des applications Windows peuvent être exécuté à partir d’un service, mais le service doit lancer le processus de kit au sein d’une session utilisateur active et ne peut pas être exécuté dans Session0.

**Remarque**    pour plus d’informations sur la ligne de commande du Kit de Certification des applications Windows, entrez la commande `appcert.exe /?`

## <a name="testing-with-a-low-power-computer"></a>Test avec un ordinateur à faible consommation d’énergie

Les seuils du test de performances du Kit de certification des applications Windows sont basés sur les performances d’un ordinateur à faible consommation d’énergie.

Les caractéristiques de l’ordinateur sur lequel le test est exécuté peuvent influencer les résultats du test. Pour déterminer si les performances de votre application répond à la [Microsoft Store stratégies](https://docs.microsoft.com/legal/windows/agreements/store-policies), nous vous recommandons de tester votre application sur un ordinateur de faible puissance, tel qu’un ordinateur en fonction du processeur Intel Atom avec une résolution d’écran de 1366 x 768 (ou version ultérieure) et un disque dur mécanique (par opposition à un disque dur SSD).

À mesure que les ordinateurs à faible consommation d’énergie évoluent, leurs caractéristiques de performances peuvent elles aussi varier. Reportez-vous à la plus récente [Microsoft Store stratégies](https://docs.microsoft.com/legal/windows/agreements/store-policies) et tester votre application avec la version la plus récente du Kit de Certification d’application Windows pour vous assurer que votre application est conforme avec les dernières exigences de performances.

## <a name="related-topics"></a>Rubriques connexes

* [Tests du Kit de Certification des applications Windows](windows-app-certification-kit-tests.md)
* [Politiques du Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies)
 

 




