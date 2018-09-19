---
author: jnHs
Description: Follow these guidelines to prepare your app's packages for submission to the Microsoft Store.
title: Exigences relatives au package de l’application
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
ms.author: wdg-dev-content
ms.date: 05/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, exigences relatives aux packages, packages, format du package, version prise en charge, soumettre
ms.localizationpriority: medium
ms.openlocfilehash: d7d748f36dafd93066928f01f9aa42414f2ffc1f
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/19/2018
ms.locfileid: "4052213"
---
# <a name="app-package-requirements"></a>Exigences relatives au package de l’application

Suivez ces instructions pour préparer les packages de votre application et les envoyer au MicrosoftStore.

## <a name="before-you-build-your-apps-package-for-the-microsoft-store"></a>Avant de générer le package de votre application pour le MicrosoftStore

Veillez à [tester votre application à l’aide du Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md). Nous vous recommandons également de tester votre application sur différents types de matériels. Notez que votre application ne peut être installée et exécutée que sur des ordinateurs disposant de licences de développeur, tant que nous ne l’avons pas certifiée et rendue accessible sur le MicrosoftStore.

## <a name="building-the-app-package-using-microsoft-visual-studio"></a>Génération du package d’application à l’aide de Microsoft Visual Studio

Si vous utilisez Visual Studio comme environnement de développement, vous disposez déjà d’outils intégrés pour créer rapidement et facilement un package d’application. Pour plus d’informations, consultez [Empaquetage d’applications](../packaging/index.md).

> [!NOTE]
> Vérifiez que tous vos noms de fichier utilisent le format ANSI. 

Quand vous créez votre package dans Visual Studio, vérifiez que vous êtes connecté au même compte que celui associé à votre compte de développeur. Les informations contenues dans certaines parties du manifeste du package font référence à votre compte. Ces informations sont automatiquement détectées et ajoutées. En l’absence des informations supplémentaires ajoutées au manifeste, vous pouvez rencontrer des échecs de chargement de package. 

Quand vous générez les packages de votre application, Visual Studio peut créer un fichier .appx ou .appxupload (ou encore un fichier .xap pour Windows Phone 8.1 et versions antérieures). Pour les applications qui ciblent Windows 10, chargez toujours le fichier .appxupload dans la page [Packages](upload-app-packages.md). Pour plus d’informations sur la création de packages d’applicationsUWP pour leStore, consultez l’article [Créer un package d’application UWP avec VisualStudio](../packaging/packaging-uwp-apps.md).

Il n’est pas nécessaire que les packages de votre application soient signés avec un certificat provenant d’une autorité de certification approuvée.


### <a name="app-bundles"></a>Ensembles d’applications

Pour les applications qui ciblent Windows 10, Windows 8.1 et/ou Windows Phone 8.1, Visual Studio peut générer un ensemble d’applications (.appxbundle) pour réduire la taille de l’application téléchargée par les utilisateurs. Cela peut être utile si vous avez défini des ressources propres à une langue, plusieurs ressources de mise à l’échelle d’images ou encore des ressources qui s’appliquent à des versions spécifiques de Microsoft DirectX.

> [!NOTE]
> Un même ensemble d’applications peut contenir vos packages pour toutes les architectures. Vous ne devez soumettre qu’un seul ensemble par système d’exploitation ciblé.

Avec un ensemble d’applications, un utilisateur ne télécharge que les fichiers pertinents, et non toutes les ressources disponibles. Pour plus d’informations sur les ensembles d’applications, consultez les articles [Création de packages d’application](../packaging/index.md) et [Créer un package d’application UWP avec VisualStudio](../packaging/packaging-uwp-apps.md).


## <a name="building-the-app-package-manually"></a>Génération manuelle du package d’application

Si vous n’utilisez pas Visual Studio pour créer votre package, vous devez [créer manuellement votre manifeste de package](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-create-a-package-manifest-manually).

Pour obtenir des détails complets et les conditions requises concernant le manifeste, consultez la documentation [Manifeste du package d’application](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest). Pour obtenir la certification, votre manifeste doit suivre le schéma du manifeste du package.

Votre manifeste doit inclure des informations spécifiques concernant votre compte et votre application. Pour trouver ces informations, consultez [Visualiser les détails d’identité d’application](view-app-identity-details.md) dans la section **Gestion de l’application** de la page de présentation de votre application, dans le tableau de bord.

> [!NOTE]
> Les valeurs du manifeste respectent la casse. Les espaces et autres symboles de ponctuation doivent également correspondre. Saisissez les valeurs correctement et vérifiez-les pour vous assurer qu’elles sont correctes.


Les ensembles d’applications (.appxbundle) utilisent un manifeste différent. Pour connaître les détails et exigences concernant les manifestes d’ensemble d’applications, consultez la documentation relative au [manifeste d’offre groupée](https://docs.microsoft.com/uwp/schemas/bundlemanifestschema/bundle-manifest). Notez que dans un package .appxbundle, le .appxmanifest de chaque package inclus doit utiliser les mêmes éléments et attributs, à l’exception de l’attribut **ProcessorArchitecture** de l’élément [d’identité](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) .

> [!TIP]
> Veillez à exécuter le [Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) avant de soumettre vos packages. Vous pouvez ainsi déterminer si votre manifeste présente des problèmes susceptibles de faire échouer la certification ou la soumission.


## <a name="package-format-requirements"></a>Exigences relatives au format des packages

Les packages de votre application doivent être conformes aux exigences ci-après.

| Propriété du package de l’application | Exigence                                                          |
|----------------------|----------------------------------------------------------------------|
| Taille de package         | .appxbundle: 25Go maximum par offre groupée <br>Packages .appx ciblant Windows10: 25Go maximum par package<br>Packages .appx ciblant Windows8.1: 8Go maximum par package <br> Packages .appx ciblant Windows 8 : 2 Go maximum par package <br> Packages .appx ciblant Windows Phone 8.1 : 4 Go maximum par package <br> Packages .xap : 1 Go maximum par package                                                                           |
| Hachages de mappage de bloc     | Algorithme SHA2-256                                                   |


## <a name="supported-versions"></a>Versions prises en charge

Pour les applications UWP, tous les packages doivent cibler une version de Windows10 qui est prise en charge par le Store. Les versions prises en charge par votre package doivent être indiquées dans les attributs **MinVersion** et **MaxVersionTested** de l'élément [TargetDeviceFamily](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) du manifeste d'application.

Les versions actuellement prises en charge sont comprises entre les versions: 
- Minimum: 10.0.10240.0
- Maximum: 10.0.17134.0


## <a name="storemanifest-xml-file"></a>Fichier XML StoreManifest

Le fichier StoreManifest.xml est un fichier de configuration facultatif qui peut être inclus dans les packages d’application. Son objet est d’activer des fonctionnalités, par exemple en déclarant que votre application est une application pour périphériques du MicrosoftStore ou en déclarant des critères dont dépend un package à appliquer à un appareil. Ces fonctionnalités ne sont pas couvertes par le manifeste du package. Si vous utilisez, StoreManifest.xml est envoyé avec le package d’application et doit être dans le dossier racine du projet principal de votre application. Pour plus d’informations, voir [Schéma StoreManifest](https://docs.microsoft.com/uwp/schemas/storemanifest/store-manifest-schema-portal).

 

 




