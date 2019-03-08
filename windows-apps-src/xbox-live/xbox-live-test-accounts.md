---
title: Comptes de test de Xbox Live
description: Découvrez comment créer des comptes de test pour tester votre Xbox Live activé jeu pendant le développement.
ms.assetid: e8076233-c93c-4961-86ac-27ec74917ebc
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, compte de test
ms.localizationpriority: medium
ms.openlocfilehash: 14313b6121cabf82762b5e3e862c73a9d3ec05cc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594624"
---
# <a name="xbox-live-test-accounts"></a>Comptes de test de Xbox Live

Lorsque vous testez des fonctionnalités dans votre titre pendant le développement, il peut être utile de créer d’autres comptes Xbox Live.  Par exemple, vous pouvez vouloir un compte de frais avec aucun succès.  Ou vous souhaiterez peut-être rendre plusieurs comptes et les rendre des amis de l’autre pour tester des scénarios de réseaux sociaux.

Il peut prendre du temps pour créer plusieurs comptes MSA (Microsoft), donc un moyen simple pour créer plusieurs comptes de test à la fois est fourni.

Comptes de test ont également des autres avantages.  Ils peuvent se connecter à votre *Sandbox de développement*, tandis qu’un MSA régulière ne peut pas en raison de restrictions de sécurité.  Si vous ne savez pas un *Sandbox de développement* est, puis consultez [bacs à sable de Xbox Live](xbox-live-sandboxes.md)

## <a name="types-of-test-accounts"></a>Types de comptes de test

Il existe deux options de comptes de test.  MSAs régulières qui sont configurés pour fonctionner dans votre Sandbox de développement, ou les comptes de test qui fonctionnent uniquement dans un bac à sable de développement.

Si vous développez un titre avec le programme Creators, vous pouvez utiliser uniquement les comptes de service administrés régulières qui sont configurés pour votre Sandbox de développement.

Nous aborderons ci-dessous comment créer les deux types.

## <a name="provisioning-regular-msas"></a>Approvisionnement de comptes de service administrés régulières

Si vous avez un compte Xbox Live préexistant, un bon point de départ consisterait à mettre en service pour une utilisation avec votre Sandbox de développement.

Si vous n’avez compte Xbox Live existant ou requièrent des comptes de service administrés supplémentaires, vous pouvez créer certaines au [ https://account.microsoft.com/account ](https://account.microsoft.com/account).

## <a name="creating-test-accounts"></a>Création de comptes de Test

Si vous êtes un ID@Xbox développeur, puis vous pourrez également créer des comptes de test exclusivement pour une utilisation dans les bacs à sable votre développement.  Vous pouvez également créer plusieurs comptes de test à la fois.

Pour accéder à la page de gestion de compte de Test dans l’espace partenaires.
1. Accédez à [Partner Center](https://partner.microsoft.com/dashboard)
2. Cliquez sur l’icône d’engrenage dans la partie supérieure droite pour accéder à des paramètres de compte
3. Cliquez sur « Comptes de Test ».

Voir ci-dessous pour une capture d’écran indiquant où trouver cela

![](images/getting_started/devcenter_testaccount_nav.png)

Une fois que vous cliquez sur « Comptes de Test », vous verrez un résumé des existants comptes de test, le cas échéant.  Vous avez également la possibilité de créer des comptes de test.

![](images/getting_started/devcenter_testaccount_summary.png)

Vous pouvez cliquer sur « Nouveau compte de Test » et s’affiche avec un formulaire que vous pouvez utiliser pour créer des comptes de test.

![](images/getting_started/devcenter_testaccount_new.png)

Les comptes de test que vous créez seront préfixés avec le nom de votre sandbox de développement et auront automatiquement accès à votre sandbox de développement.
