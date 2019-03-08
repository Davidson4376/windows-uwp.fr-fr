---
title: Test de jeu Unity dans Visual Studio
description: Builds de la liste de contrôle pour un test réussi de Unity dans Visual Studio.
ms.date: 03/19/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, unity
ms.openlocfilehash: 4d5a1a5596beba2839e01ca5be6e6d2dbff0c148
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589874"
---
# <a name="test-your-unity-game-build-in-visual-studio"></a>Tester votre build de jeux Unity dans Visual Studio

Pour tester la fonctionnalité de Xbox Live de votre jeu Unity avec des données réelles, vous devez créer le jeu comme décrit dans la **Build et le projet de test** section de la [configurer Xbox Live dans l’article Unity](configure-xbox-live-in-unity.md). L’article suivant vous donnera une liste de contrôle d’éléments afin de garantir un test réussi de votre jeu Unity dans Visual Studio.

1. **Vérifiez que vous disposez d’un titre correctement configuré, associé à votre jeu Unity.**
    Si vous avez activé la Xbox Live via l’Assistant Association de Xbox Live, vous devez vous familiariser avec [partenaires](https://partner.microsoft.com/dashboard). Partenaires vous permet de configurer les paramètres de Xbox Live pour votre titre et doit être configuré correctement dans l’ordre pour votre titre communiquer avec Xbox Live. L’article [créer un nouveau titre de programme Xbox Live Creators et publier dans l’environnement de test](create-and-test-a-new-creators-title.md) vous accompagne tout au long du processus d’installation de partenaires. Si vous avez déjà configuré votre jeu via le **Assistant Configuration de la Xbox** dans Unity, vous pouvez passer à la section [Test Xbox Live de configuration de service dans votre jeu](create-and-test-a-new-creators-title.md#test-xbox-live-service-configuration-in-your-game). Tandis que dans l’espace partenaires, veillez à vérifier que les informations contenues dans votre configuration Xbox Live pour votre jeu Unity correspond à la configuration de partenaires à votre jeu.

2. **Vérifiez que votre titre a un Account(with gamertag) Microsoft autorisés qui peuvent se connectent à votre titre.**
    Sans un compte autorisé, vous ne serez pas en mesure de se complète signe dans lors du test de votre titre, ni vous pourrez utiliser d’autres fonctionnalités Xbox Live. S’assurer que vous avez un Account Microsoft et le gamertag autorisés lire [autoriser des comptes de Xbox Live pour effectuer des tests dans votre environnement](authorize-xbox-live-accounts.md).

3. **Assurez-vous que votre titre a été publiée pour le test.**
    Lorsque vous apportez des modifications à votre titre ces modifications doit être publié sur votre boîte de dialogue sable avant de pouvoir être utilisés. Assurez-vous que vous poussez le **Test** bouton Publier vos modifications à votre configuration.

    ![Publier pour l’image de test](../images/creators_udc/creators_udc_xboxlive_config_test.png)

    Le **Test** bouton se trouve dans [partenaires](https://partner.microsoft.com/dashboard) paramètres de service sous le Xbox Live titre de votre. Si vous avez apporté des modifications à votre configuration, telles que l’ajout d’un nouveau compte de test autorisé que vous souhaitez transmettre le **tester** bouton pour publier les nouveaux paramètres de configuration dans le service Xbox Live.

4. **Vérifiez que votre ordinateur est en mode développeur et que vous utilisez l’approprié Xbox Live bac à sable.**
    Lorsque votre titre est publiée pour la tester est publiée dans un environnement spécifique, appelé un bac à sable. Si votre PC de développement n’est pas configuré pour utiliser ce bac à sable vous ne serez pas en mesure de tester des fonctionnalités Xbox Live. Apprenez à vérifier et modifier le bac à sable de votre PC avec le [introduction de bacs à sable Xbox Live](xbox-live-sandboxes-creators.md).

5. **Assurez-vous que vous générez votre projet en tant qu’un x64 générer le ciblage de l’ordinateur local pour générer sur votre PC**

    ![paramètres de build](../images/unity/get-started-with-creators/vsBuildSettings.JPG)