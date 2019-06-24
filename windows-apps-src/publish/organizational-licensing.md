---
Description: Vous pouvez indiquer si et comment votre application peut être proposée pour des achats en volume par le biais du Microsoft Store pour Entreprises et du Microsoft Store pour Éducation dans la section Gestion des licences organisationnelles d’une soumission d’application.
title: Options de gestion des licences organisationnelles
ms.assetid: 1EB139B0-67E7-4F66-AAEF-491B1E52E96F
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, store pour entreprises, store pour éducation, organisation, licence en volume, entreprise, store éducation, store entreprises, achat en volume, en gros
localizationpriority: high
ms.openlocfilehash: b7f0ecc1eb7faec39eaad925ccd6c9d501d39749
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63788399"
---
# <a name="organizational-licensing-options"></a>Options de gestion des licences organisationnelles


Vous pouvez indiquer si et comment votre application peut être proposée pour des achats en volume par le biais du Microsoft Store pour Entreprises et du Microsoft Store pour Éducation dans la section **Gestion des licences organisationnelles** de la page [Tarification et disponibilité](set-app-pricing-and-availability.md#organizational-licensing) d’une soumission d’application.

Ces paramètres vous permettent d’autoriser la mise de votre application à la disposition des organisations (entreprises et établissements d’enseignement) qui acquièrent et déploient plusieurs licences pour leurs utilisateurs, ce qui vous offre une possibilité d’élargir votre rayon d’action aux organisations qui utilisent Windows 10 sur différents types d’appareils, tels que PC, tablettes et téléphones.

Vous devrez également autoriser la gestion des licences organisationnelles pour toutes les [applications cœur de métier](distribute-lob-apps-to-enterprises.md) que vous publiez directement à l’attention des entreprises.

> [!NOTE]
> Les sélections pour chacune de vos applications sont configurées indépendamment les unes des autres. Vous pouvez modifier vos préférences concernant une application à tout moment en créant une nouvelle soumission. Vos modifications prennent effet dès la fin du [processus de certification](the-app-certification-process.md) de la soumission.

> [!IMPORTANT]
> Les soumissions qui utilisent l’[API de soumission au Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) ne seront pas disponibles sur le Microsoft Store pour Entreprises ou le Microsoft Store pour Éducation. Pour rendre votre application disponible pour les achats en volume par des organisations, vous devez créer et soumettre vos soumissions dans l’Espace partenaires.


## <a name="allowing-your-app-to-be-offered-to-organizations"></a>Autorisation de l’offre de votre application aux organisations

Par défaut, la case à cocher **Proposer mon application aux organisations via le service de gestion de licences et de distribution (en ligne) du Store** est activée. Cela signifie que vous souhaitez que votre application puisse être incluse dans les catalogues d’applications que nous mettons à la disposition des organisations pour l’acquisition en volume, avec des licences d’application gérées par le biais du système de gestion de licences en ligne du Windows Store.

> [!NOTE]
> Cela ne garantit pas que votre application sera mise à la disposition de toutes les organisations.

Si vous ne souhaitez pas nous autoriser à proposer votre application à l’acquisition en volume aux organisations, désactivez cette case à cocher. Notez que cette modification prend effet uniquement après l’achèvement du processus de certification. Si des organisations ont précédemment acquis des licences pour votre application, celles-ci restent valides, et les personnes disposant déjà de l’application peuvent continuer à l’utiliser.

> [!TIP]
> Pour publier des applications métier exclusivement à l’attention d’une organisation spécifique, vous pouvez configurer une association d’entreprise et autoriser l’organisation à ajouter les applications directement à son magasin privé. Pour plus d’informations, voir [Distribuer des applications métier aux entreprises](distribute-lob-apps-to-enterprises.md).


## <a name="allowing-disconnected-offline-licensing"></a>Autorisation de l’achat de licences en mode hors connexion

De nombreuses organisations ont besoin d’applications dont la licence peut être achetée hors connexion. Par exemple, certaines organisations doivent déployer des applications sur des appareils qui se connectent rarement ou jamais à Internet. Si vous souhaitez autoriser que votre application soit disponible pour ces clients, activez la case à cocher **Autoriser la gestion et la distribution des licences aux organisations en mode hors connexion**.

Notez que cette case est **désactivée** par défaut. Vous devez cocher cette case pour nous permettre de mettre votre application à la disposition d’organisations vérifiées qui l’installent à l’aide d’une gestion des licences par l’organisation (en mode hors connexion). Pour pouvoir installer des applications payantes pour leurs utilisateurs finaux de cette manière, les organisations doivent se soumettre à un processus de validation supplémentaire.

L’achat de licences en mode hors connexion permet aux organisations d’acquérir votre application en volume, puis de l’installer sans que chaque appareil doive contacter le système de gestion de licences du Windows Store. L'organisation peut télécharger le package de votre application en même temps qu'une licence lui permettant de l'installer sur des appareils (via ses propres outils de gestion ou en préchargeant l'application sur des images du système d'exploitation) sans informer le Store quand une licence particulière a été utilisée. L’activation de ce scénario accroît sensiblement la flexibilité de déploiement et peut augmenter considérablement l’attrait de votre application auprès de ces clients.

> [!IMPORTANT]
> La gestion de licences en mode hors connexion n’est pas prise en charge pour les packages .xap.

 
## <a name="paid-app-support"></a>Support de l’application payante

Les comptes de développeur situés dans certains marchés sont actuellement en mesure de proposer des applications payantes pour l’achat en volume sur le Microsoft Store pour Entreprises. 

> [!NOTE]
> Sur certains marchés, le prix affiché pour une application sur le Microsoft Store pour Entreprises ou le Microsoft Store pour Éducation peut différer de celui affiché pour les clients au détail sur le Microsoft Store, pour le même niveau de prix. Le paiement des revenus des achats d’entreprise fonctionne de la même façon que les achats grand public de votre application. Pour plus d’informations, voir [Rémunération](getting-paid-apps.md) et [Contrat du développeur de l’application](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement). Pour obtenir la liste des marchés dans lesquels le Microsoft Store pour Entreprises et le Microsoft Store pour Éducation sont disponibles, voir l’article [Vue d’ensemble de Microsoft Store pour Entreprises et Microsoft Store pour Éducation](https://technet.microsoft.com/itpro/windows/manage/windows-store-for-business-overview#supported-markets).

Si votre pays ou région ne sont pas répertoriés ci-dessous, vos applications payantes ne sont pour l’instant pas proposées dans Microsoft Store pour Entreprises ou dans Microsoft Store pour Éducation. Si tel est le cas, les choix de licences organisationnelles que vous effectuez pour vos applications payantes pourront être appliqués ultérieurement, à mesure que nous prendrons en charge les soumissions d’applications payantes pour de nouveaux marchés de compte de développeur à l’avenir.

À ce stade, les développeurs qui résident dans les pays et régions ci-après peuvent distribuer des applications payantes aux clients professionnels par le biais du Microsoft Store pour Entreprises et du Microsoft Store pour Éducation :

- Autriche
- Belgique
- Bulgarie
- Canada
- Croatie
- Chypre
- République tchèque
- Danemark
- Estonie
- Finlande
- France
- Allemagne
- Grèce
- Hongrie
- Irlande
- Île de Man
- Italie
- Lettonie
- Liechtenstein
- Lituanie
- Luxembourg
- Malte
- Monaco
- Pays-Bas
- Norvège
- Pologne
- Portugal
- Roumanie
- Slovaquie
- Slovénie
- Espagne
- Suède
- Suisse
- Royaume-Uni
- États-Unis
