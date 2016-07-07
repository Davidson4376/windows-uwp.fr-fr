---
author: jnHs
Description: "Le Windows Store applique certaines règles relatives aux numéros de version, qui fonctionnent différemment dans les différentes versions du système d’exploitation."
title: "Numérotation des versions de packages"
ms.assetid: DD7BAE5F-C2EE-44EE-8796-055D4BCB3152
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 9910ca5b5a3fd5d24643a1dfc6da3644d561c617

---

# Numérotation des versions de packages


Chaque package que vous fournissez doit avoir un numéro de version (fourni sous la forme d’une valeur dans l’attribut **Version** de l’élément **Package/Identity** dans le manifeste de l’application). Le Windows Store applique certaines règles relatives aux numéros de version, qui fonctionnent différemment dans les différentes versions du système d’exploitation.

> **Remarque** Cette rubrique fait référence aux « packages ». Toutefois, sauf indication contraire, les mêmes règles s’appliquent aux numéros de version des fichiers .appx et .appxbundle.

## Numérotation des versions pour les packages Windows10


Le numéro de version de tous vos packages Windows 10 doit toujours être supérieur à ceux des packages Windows 8, Windows 8.1 et/ou Windows Phone 8.1 que vous publiez (ou avez publié) pour la même application. (Pour plus d’informations, voir [Ajout de packages pour Windows 10 à une application publiée précédemment](guidance-for-app-package-management.md#adding-packages-for-windows-10-to-a-previously-published-app).)

> **Remarque** La dernière partie (quatrième) du numéro de version est réservée à l’utilisation du Windows Store et doit demeurer 0.

Lors du choix d’un package Windows10 à partir de votre soumission publiée, le Windows Store utilise toujours le package dont le numéro de version est le plus élevé et qui s’applique à l’appareil du client. Cela vous offre une plus grande souplesse et vous permet de contrôler les packages fournis aux clients sur des types spécifiques d’appareils. Il est important de noter que vous pouvez soumettre ces packages dans n’importe quel ordre ; vous n’êtes pas obligé de fournir des packages dont le numéro de version est supérieur avec chaque soumission ultérieure.

Vous pouvez même fournir plusieurs packages Windows 10 avec le même numéro de version. Toutefois, les packages qui partagent un même numéro de version ne peuvent pas avoir la même architecture, car l’identité complète que le Windows Store utilise pour chaque package doit être unique. Pour plus d’informations, voir [**Identity**](https://msdn.microsoft.com/library/windows/apps/br211441).

Si vous fournissez plusieurs packages Windows10 qui utilisent le même numéro de version, l’architecture (dans l’ordre x64, x86, ARM, neutre) permet de déterminer celui qui est de niveau supérieur lors du choix du package à fournir à un appareil donné. Lors du classement des ensembles d’applications qui utilisent la même version, le niveau d’architecture le plus élevé dans l’ensemble est pris en considération : un ensemble d’applications contenant un package x64 aura un classement plus élevé qu’un ensemble contenant uniquement un package x86.

Cela vous offre une grande souplesse pour faire évoluer votre application au fil du temps. Vous pouvez charger et soumettre de nouveaux packages utilisant des numéros de version inférieurs pour prendre en charge des appareils abordables qui ne sont pas encore pris en charge, ajouter des packages de version supérieure comprenant des dépendances plus strictes pour tirer parti de certaines fonctionnalités matérielles ou de système d’exploitation, ou ajouter des packages de version supérieure faisant office de mises à jour pour tout ou partie de vos clients existants.

L’exemple suivant montre comment gérer la numérotation des versions pour livrer les packages voulus à vos clients au gré de soumissions multiples.

### Exemple : évolution vers un package unique en plusieurs soumissions

Windows 10 vous permet d’écrire un seul code base qui s’exécute partout. Cela facilite considérablement le démarrage d’un nouveau projet interplateforme. Toutefois, pour diverses raisons, il peut sembler inopportun de fusionner plusieurs codes base existants pour créer immédiatement un seul projet.

Vous pouvez utiliser les règles de contrôle de version du package pour amener progressivement vos clients vers un package unique pour la famille d’appareils universelle, tout en envoyant un certain nombre de mises à jour intermédiaires pour des familles spécifiques d’appareils (y compris ceux qui tirent parti des API Windows 10). L’exemple suivant illustre la manière dont les mêmes règles sont appliquées de façon cohérente.

| Soumission | Contenu                                                  | Expérience utilisateur                                                                                                                                                                             |
|------------|-----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1          | - Version de package: 1.1.10.0 <br> - Famille d’appareils: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> - Version de package: 1.1.0.0 <br> - Famille d’appareils: Windows.Mobile, minVersion 10.0.10240.0     | - Les appareils sous Windows10 Desktop build 10.0.10240.0 et supérieure obtiendront le package 1.1.10.0 <br> - Les appareils sous Windows10 Mobile build 10.0.10240.0 et supérieure obtiendront le package 1.1.0.0 <br> - Les autres familles d’appareils ne peuvent acheter et installer l’application |
| 2          | - Version de package: 1.1.10.0 <br> - Famille d’appareils: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> - Version de package: 1.1.0.0 <br> - Famille d’appareils: Windows.Mobile, minVersion 10.0.10240.0 <br> <br> - Version de package: 1.0.0.0 <br> - Famille d’appareils: Windows.Universal, minVersion 10.0.10240.0    | - Les appareils sous Windows10 Desktop build 10.0.10240.0 et supérieure obtiendront le package 1.1.10.0 <br> - Les appareils sous Windows10 Mobile build 10.0.10240.0 et supérieure obtiendront le package 1.1.0.0 <br> - Les autres familles d’appareils (non de bureau, non mobiles) obtiendront le package 1.0.0.0 quand elles seront introduites <br> - Les appareils mobiles et de bureau sur lesquels l’application est déjà installée n’obtiendront aucune mise à jour (car ils ont déjà la meilleure version disponible: les versions 1.1.10.0 et 1.1.0.0 sont supérieures à la version 1.0.0.0) |
| 3          | - Version de package: 1.1.10.0 <br> - Famille d’appareils: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> - Version de package: 1.1.5.0 <br> - Famille d’appareils: Windows.Universal, minVersion 10.0.10250.0 <br> <br> - Version de package: 1.0.0.0 <br> - Famille d’appareils: Windows.Universal, minVersion 10.0.10240.0    | - Les appareils sous Windows10 Desktop build 10.0.10240.0 et supérieure obtiendront le package 1.1.10.0 <br> - Les appareils sous Windows10 Mobile build 10.0.10250.0 et supérieure obtiendront le package 1.1.5.0 <br> - Les appareils sous Windows10 Mobile build &gt;=10.0.10240.0 et &lt; 10.010250.0 obtiendront le package 1.1.0.0 
| 4          | - Version de package: 2.0.0.0 <br> - Famille d’appareils: Windows.Universal, minVersion 10.0.10240.0   | - Tous les clients sur toutes les familles d’appareils sous Windows10 build v10.0.10240.0 et supérieure obtiendront le package 2.0.0.0 | 

> **Remarque** Dans tous les cas, les appareils clients recevront le package associé au numéro de version le plus élevé possible pour lequel ils sont éligibles. Par exemple, dans la troisième soumission ci-dessus, tous les appareils de bureau recevront le package v1.1.10.0, même si leur système d’exploitation est de la version 10.0.10250.0 ou supérieure, et pourraient donc également accepter le package v1.1.5.0. Étant donné que le package 1.1.10.0 est celui dont le numéro de version est le plus élevé à leur disposition, ils obtiendront ce package.

### Utilisation de la numérotation des versions pour revenir à un package livré précédemment pour de nouvelles acquisitions

Si vous conservez des copies de vos fichiers de package Windows 10 antérieurs, vous serez en mesure de restaurer le package de votre application dans le Store à une version antérieure du package Windows 10 si vous venez à rencontrer des problèmes avec une version. Cela constitue un moyen temporaire de limiter l’interruption pour vos clients pendant que vous résolvez le problème.

Pour ce faire, créez une nouvelle soumission. Supprimez le package problématique et chargez l’ancien package que vous souhaitez fournir dans le Windows Store. Les clients qui ont déjà reçu le package que vous restaurez auront toujours le package problématique (car votre ancien package aura un numéro de version antérieur). Cela n’empêchera cependant aucun autre utilisateur d’acquérir le package problématique, tout en permettant à l’application de rester disponible dans le Windows Store.

Pour résoudre les problèmes des clients qui ont déjà reçu le package problématique, vous pouvez soumettre dès que possible un nouveau package Windows 10 associé à un numéro de version supérieur à celui du package incorrect. Une fois cette soumission certifiée, tous les clients seront mis à jour vers le nouveau package, car celui-ci aura un numéro de version supérieur.

## Numérotation des versions des packages pour Windows 8.1 (et versions antérieures) et Windows Phone 8.1

Pour les packages .appx qui ciblent Windows Phone 8.1, le numéro de version du package dans une nouvelle soumission doit toujours être supérieur à celui du package inclus dans la dernière soumission (ou toute soumission précédente).

Pour les packages .appx qui ciblent Windows 8 et Windows 8.1, la même règle s’applique par architecture : le numéro de version du package dans une nouvelle soumission doit toujours être supérieur à celui du package livré en dernier dans le Windows Store pour la même architecture.

De plus, le numéro de version des packages Windows 8.1 doit toujours être supérieur aux numéros de version de vos packages Windows 8 pour la même application. Autrement dit, le numéro de version d’un package Windows8 que vous soumettez doit être inférieur au numéro de version de tout package Windows8.1 que vous avez soumis pour la même application.

> **Remarque** Si vous avez également des packages Windows 10, leur numéro de version doit être supérieur aux numéros de version des packages Windows 8, Windows 8.1 et/ou Windows Phone 8.1 que vous publiez ou avez publiés. Pour plus d’informations, voir [Ajout de packages pour Windows 10 à une application publiée précédemment](guidance-for-app-package-management.md#adding-packages-for-windows-10-to-a-previously-published-app).

Voici quelques exemples de ce qui se passe dans les différents scénarios de mise à jour de numéro de version pour Windows 8 et Windows 8.1.

| Version de votre application dans le Store  | Version transférée | Une fois la nouvelle version présente dans le Windows Store, elle sera installée dans une nouvelle acquisition. | Une fois la nouvelle version présente dans le Windows Store, elle sera mise à jour si un client possède déjà l’application. |
|---------------------------------------------|-----------------------------|--------------------------------------------------------------------------------------------|----------|
| Rien                                     | x86, v1.0.0.0               | x86, v1.0.0.0 sur les ordinateurs x86 et x64                                                | Rien |
| x86, v1.0.0.0                               | x64, v1.0.0.0               | v1.0.0.0 pour l’architecture de l’ordinateur du client                                                   | Rien Les numéros de version sont les mêmes. |
| x86, v1.0.0.0 <br> x64, v1.0.0.0            | x64, v1.0.0.1               | v1.0.0.0 pour les clients possédant un ordinateur x86 <br> v1.0.0.1 pour les clients possédant un ordinateur x64                 | Rien pour les clients exécutant l’application sur un ordinateur x86. <br> Mise à jour de v1.0.0.0 vers v1.0.0.1 pour les clients exécutant l’application sur un ordinateur x64. <br> **Remarque** Si la version x86 de l’application s’exécute sur un ordinateur x64, l’application ne sera mise à jour vers la version x64 qu’à condition que le client effectue une désinstallation puis une réinstallation. |
| Rien                                     | neutre, v1.0.0.1           | neutre, v1.0.0.1 sur tous les ordinateurs                                                         | Rien |
| neutre, v1.0.0.1                           | x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | v1.0.0.0 pour l’architecture de l’ordinateur du client.          | Rien Les clients qui possèdent la version neutre v1.0.0.1 de l’application continuent de l’utiliser. |
| neutre, v1.0.0.1 <br> x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | v1.0.0.1 pour l’architecture de l’ordinateur du client. | Rien pour les clients qui exécutent la version neutre v1.0.0.1 de l’application. <br> Mise à jour de v1.0.0.0 vers v1.0.0.1 pour les clients exécutant la version v1.0.0.0 de l’application générée pour l’architecture de leur ordinateur. |
| x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | x86, v1.0.0.2 <br> x64, v1.0.0.2 <br> ARM, v1.0.0.2 | v1.0.0.2 pour l’architecture de l’ordinateur du client.  | Mise à jour de v1.0.0.1 vers v1.0.0.2 pour les clients exécutant la version v1.0.0.1 de l’application générée pour l’architecture de leur ordinateur. |
 
> **Remarque** À la différence des packages .appx, les numéros de version dans les packages .xap ne sont pas pris en compte lors de la détermination du package à fournir à un client donné. Pour mettre à jour un client d’un package .xap vers une version plus récente, veillez à supprimer l’ancien .xap dans la nouvelle soumission.



<!--HONumber=Jun16_HO4-->


