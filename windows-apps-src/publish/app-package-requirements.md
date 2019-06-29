---
Description: Suivez ces instructions pour préparer les packages de votre application et les envoyer au Microsoft Store.
title: Exigences relatives aux packages d’application
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, exigences relatives aux packages, packages, format du package, version prise en charge, soumettre
ms.localizationpriority: medium
ms.openlocfilehash: 8502c477e3e1202ecf97c6081f4cd87b4d681081
ms.sourcegitcommit: 4aef8c01ba9321401d5729a1ec6d46452ee76faf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67468963"
---
# <a name="app-package-requirements"></a>Exigences relatives aux packages d’application

Suivez ces instructions pour préparer les packages de votre application et les envoyer au Microsoft Store.

## <a name="before-you-build-your-apps-package-for-the-microsoft-store"></a>Avant de générer le package de votre application pour le Microsoft Store

Veillez à [tester votre application à l’aide du Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md). Nous vous recommandons également de tester votre application sur différents types de matériels. Notez que votre application ne peut être installée et exécutée que sur des ordinateurs disposant de licences de développeur, tant que nous ne l’avons pas certifiée et rendue accessible sur le Microsoft Store.

## <a name="building-the-app-package-using-microsoft-visual-studio"></a>Génération du package d’application à l’aide de Microsoft Visual Studio

Si vous utilisez Visual Studio comme environnement de développement, vous disposez déjà d’outils intégrés pour créer rapidement et facilement un package d’application. Pour plus d’informations, consultez [Empaquetage d’applications](../packaging/index.md).

> [!NOTE]
> Vérifiez que tous vos noms de fichier utilisent le format ANSI. 

Quand vous créez votre package dans Visual Studio, vérifiez que vous êtes connecté au même compte que celui associé à votre compte de développeur. Les informations contenues dans certaines parties du manifeste du package font référence à votre compte. Ces informations sont automatiquement détectées et ajoutées. En l’absence des informations supplémentaires ajoutées au manifeste, vous pouvez rencontrer des échecs de chargement de package. 

Lorsque vous générez des packages UWP de votre application, Visual Studio peut créer un .msix ou fichier appx ou un fichier .msixupload ou .appxupload. Pour les applications UWP, nous vous recommandons de toujours télécharger le fichier .msixupload ou .appxupload dans le [Packages](upload-app-packages.md) page. Pour plus d’informations sur la création de packages d’applications UWP pour le Store, consultez l’article [Créer un package d’application UWP avec Visual Studio](../packaging/packaging-uwp-apps.md).

Il n’est pas nécessaire que les packages de votre application soient signés avec un certificat provenant d’une autorité de certification approuvée.


### <a name="app-bundles"></a>Ensembles d’applications

Pour les applications UWP, Visual Studio peut générer un lot d’applications (.msixbundle ou .appxbundle) pour réduire la taille de l’application que les utilisateurs téléchargent. Cela peut être utile si vous avez défini des ressources propres à une langue, plusieurs ressources de mise à l’échelle d’images ou encore des ressources qui s’appliquent à des versions spécifiques de Microsoft DirectX.

> [!NOTE]
> Un lot d’applications peut contenir vos packages pour toutes les architectures.

Avec un ensemble d’applications, un utilisateur ne télécharge que les fichiers pertinents, et non toutes les ressources disponibles. Pour plus d’informations sur les ensembles d’applications, consultez les articles [Création de packages d’application](../packaging/index.md) et [Créer un package d’application UWP avec Visual Studio](../packaging/packaging-uwp-apps.md).


## <a name="building-the-app-package-manually"></a>Génération manuelle du package d’application

Si vous n’utilisez pas Visual Studio pour créer votre package, vous devez [créer manuellement votre manifeste de package](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-create-a-package-manifest-manually).

Pour obtenir des détails complets et les conditions requises concernant le manifeste, consultez la documentation [Manifeste du package d’application](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest). Pour obtenir la certification, votre manifeste doit suivre le schéma du manifeste du package.

Votre manifeste doit inclure des informations spécifiques concernant votre compte et votre application. Pour trouver ces informations, consultez [Visualiser les détails d’identité d’application](view-app-identity-details.md) dans la section **Gestion de l’application** de la page de présentation de votre application, dans le tableau de bord.

> [!NOTE]
> Les valeurs dans le manifeste respectent la casse. Les espaces et autres symboles de ponctuation doivent également correspondre. Saisissez les valeurs correctement et vérifiez-les pour vous assurer qu’elles sont correctes.


Offres groupées d’applications (.msixbundle ou .appxbundle) utilisent un manifeste différent. Pour obtenir les détails et conditions requises concernant les manifestes d’offres groupées d’applications, consultez la documentation relative au [manifeste d’offre groupée](https://docs.microsoft.com/uwp/schemas/bundlemanifestschema/bundle-manifest). Notez que dans une .msixbundle ou .appxbundle, le manifeste de chaque package inclus doive utiliser les mêmes éléments et attributs, à l’exception de la **ProcessorArchitecture** attribut de la [identité](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) élément.

> [!TIP]
> Veillez à exécuter le [Kit de Certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) avant d’envoyer vos packages. Vous pouvez ainsi déterminer si votre manifeste présente des problèmes susceptibles de faire échouer la certification ou la soumission.


## <a name="package-format-requirements"></a>Exigences relatives au format des packages

Les packages de votre application doivent être conformes aux exigences ci-après.

| Propriété du package de l’application | Condition requise                                                          |
|----------------------|----------------------------------------------------------------------|
| Taille de package         | .msixbundle ou .appxbundle : Maximum de 25 Go par lot <br>packages .msix ou .appx ciblant Windows 10 : Maximum de 25 Go par package<br>.AppX packages ciblant Windows 8.1 : Maximum de 8 Go par package <br> .AppX packages ciblant Windows 8 : Nombre maximal de 2 Go par package <br> packages .appx ciblant Windows Phone 8.1 : 4 Go maximum par package <br> .xap packages : Nombre maximal de 1 Go par package                                                                           |
| Hachages de mappage de bloc     | Algorithme SHA2-256                                                   |

> [!IMPORTANT]
> À compter du 31 octobre 2018, produits nouvellement créé ne peut pas inclure les packages ciblant 8.x/Windows de Windows Phone 8.x ou une version antérieure. Pour plus d’informations, consultez ce [billet de blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="supported-versions"></a>Versions prises en charge

Pour les applications UWP, tous les packages doivent cibler une version de Windows 10 qui est prise en charge par le Store. Les versions prises en charge par votre package doivent être indiquées dans les attributs **MinVersion** et **MaxVersionTested** de l'élément [TargetDeviceFamily](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) du manifeste d'application.

Les versions actuellement prises en charge sont comprises entre les versions : 
- Minimum : 10.0.10240.0
- Maximum : 10.0.17763.1


## <a name="storemanifest-xml-file"></a>Fichier XML StoreManifest

Le fichier StoreManifest.xml est un fichier de configuration facultatif qui peut être inclus dans les packages d’application. Son objet est d’activer des fonctionnalités, par exemple en déclarant que votre application est une application pour périphériques du Microsoft Store ou en déclarant des critères dont dépend un package à appliquer à un appareil. Ces fonctionnalités ne sont pas couvertes par le manifeste du package. Si utilisé, StoreManifest.xml est envoyé avec le package d’application et doit être dans le dossier racine du projet principal de votre application. Pour plus d’informations, voir [Schéma StoreManifest](https://docs.microsoft.com/uwp/schemas/storemanifest/store-manifest-schema-portal).

 

 




