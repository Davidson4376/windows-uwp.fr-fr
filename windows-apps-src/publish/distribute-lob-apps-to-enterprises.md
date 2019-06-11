---
Description: Vous pouvez publier des applications métiers directement à l’attention des entreprises pour une acquisition en volume par le biais de Microsoft Store pour Entreprises ou de Microsoft Store pour Éducation, sans mettre vos applications à la disposition générale dans le Windows Store.
title: Distribuer des applications métier aux entreprises
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, cœur de métier, métier, applications d’entreprise, store pour entreprises, store pour éducation, entreprise
ms.localizationpriority: medium
ms.openlocfilehash: d44d2b248ddc2bff6ffa91860621fe315fb22c47
ms.sourcegitcommit: 978df7dfd3813de51609b6a44aedcd402083a5fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66826152"
---
# <a name="distribute-lob-apps-to-enterprises"></a>Distribuer des applications métier aux entreprises


Vous pouvez publier des applications métiers directement à l’attention des entreprises pour une acquisition en volume par le biais de Microsoft Store pour Entreprises ou de Microsoft Store pour Éducation, sans mettre vos applications à la disposition générale dans le Windows Store.

> [!NOTE]
> Pour l’instant, seules les applications gratuites peuvent être distribuées de façon exclusive aux entreprises par le biais de Microsoft Store pour Entreprises ou de Microsoft Store pour Éducation. Si vous soumettez une application payante en tant qu’application métier, elle ne sera pas disponible pour l’entreprise. 

> [!IMPORTANT]
> Vous ne pouvez pas utiliser l'[API de soumission au Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) pour publier des applications métier directement à destination des entreprises. Toutes les soumissions pour les applications métier doivent être publiées via des partenaires.


## <a name="set-up-the-enterprise-association"></a>Configurer l’association d’entreprise

Lorsque vous envisagez de publier des applications métier exclusivement à l’attention d’une entreprise, la première étape consiste à établir l’association entre votre compte et le magasin privé de l’entreprise.

> [!IMPORTANT]
> Ce processus d’association doit être lancé par l’entreprise et doit utiliser l’adresse de messagerie associée au compte Microsoft qui a été utilisé pour créer le compte de développeur. Pour plus d’informations, voir [Utilisation des applications métier](https://go.microsoft.com/fwlink/p/?LinkId=698846).

Lorsqu'une entreprise vous invite à publier des applications destinées à son utilisation exclusive, vous recevez un e-mail contenant un lien pour confirmer l'association. Vous pouvez également confirmer ces associations en accédant à la section **Associations d’entreprise** de vos **Paramètres de compte** (pour autant que vous êtes connecté avec le compte Microsoft qui a été utilisé pour ouvrir le compte de développeur).

Pour confirmer l'association, cliquez sur **Accepter**. Votre compte pourra désormais publier des applications destinées à une utilisation exclusive par l'entreprise.


## <a name="submit-lob-apps"></a>Soumettre des applications métiers

Lorsque vous êtes prêt à publier une application destinée à une utilisation exclusive par une entreprise, vous suivez un processus similaire au processus de soumission d'application. L’application est soumise au même [processus de certification](the-app-certification-process.md) et doit être conforme à l’ensemble des [stratégies du Microsoft Store](store-policies.md). Seuls quelques aspects du processus sont différents.


### <a name="visibility"></a>Visibilité

Une fois que vous avez configuré une association d’entreprise, chaque fois que vous soumettez une application, une zone de liste déroulante s’affiche dans la section **Visibilité** de la page **Tarification et disponibilité** de la soumission. La sélection par défaut est **Vente au détail**. Pour mettre l’application à la disposition exclusive d’une entreprise, vous devez choisir **Distribution d’applications métier**.

Une fois l’option **Distribution d’applications métier** sélectionnée, les options **Visibilité** habituelles sont remplacées par une liste des entreprises pour lesquelles vous pouvez publier des applications exclusives. Personne en dehors des entreprises que vous sélectionnez ne sera en mesure de visualiser ou télécharger l’application.

Vous devez sélectionner au moins une entreprise pour publier une application en tant qu’application cœur de métier.

<span id="organizational" />

### <a name="organizational-licensing"></a>Gestion des licences organisationnelles

Par défaut, la case **Proposer mon application aux organisations via le service de gestion de licences en volume (en ligne) du Store** est cochée lorsque vous soumettez une application. Lorsque vous publiez des applications métiers, cette case doit rester cochée afin que l’entreprise puisse acquérir votre application en volume. Personne ne pourra accéder à l’application, hormis les entreprises que vous avez sélectionnées dans la section **Distribution et visibilité**.

Si vous souhaitez mettre l’application à disposition de l’entreprise à l’aide de licences en mode hors connexion, vous pouvez également cocher la case **Permettre aux entreprises d’acheter des licences en mode hors connexion**.

Pour plus d’informations, voir [Options de gestion des licences organisationnelles](organizational-licensing.md).


### <a name="age-ratings"></a>Classification par âge

Pour les applications métier, la [classification par âge](age-ratings.md) du processus de soumission fonctionne comme pour les applications commerciales. Toutefois, une option supplémentaire vous permet d’indiquer manuellement la classification par âge de votre application dans le Windows Store au lieu de remplir le questionnaire ou d’importer un identificateur de classification IARC existant. Cette évaluation manuelle n’est utilisable qu’avec la distribution métier ; par conséquent, si vous redéfinissez le paramètre **Visibilité** de l’application sur **Vente au détail**, vous devrez répondre au questionnaire d’évaluation de l’âge avant de pouvoir publier la soumission.


## <a name="enterprise-deployment-of-lob-apps"></a>Déploiement d’applications métier dans les entreprises

Lorsque vous cliquez sur **Envoyer au Store**, le processus de certification de l'application s'exécute. À l’issue de ce processus, un administrateur de l’entreprise doit ajouter l’application à son magasin privé dans le portail Microsoft Store pour Entreprises ou Microsoft Store pour Éducation. L'entreprise peut alors déployer l'application à l'attention de ses utilisateurs.

> [!NOTE]
> Pour obtenir votre application métier, l’organisation doit se trouver dans un [marché pris en charge](https://technet.microsoft.com/itpro/windows/whats-new/windows-store-for-business-overview#supported-markets), et vous ne devez pas avoir [exclu ce marché](define-pricing-and-market-selection.md) lorsque vous avez soumis l’application. 

Pour plus d'informations, voir [Utilisation des applications métier](https://go.microsoft.com/fwlink/p/?LinkId=698846) et [Distribuer des applications à l'aide de votre magasin privé](https://go.microsoft.com/fwlink/p/?LinkId=698847).


## <a name="update-lob-apps"></a>Mettre à jour des applications métiers

Pour publier les mises à jour d'une application que vous avez déjà publiée en tant qu'application métier, il vous suffit de créer une soumission. Vous pouvez transférer de nouveaux packages ou apporter des modifications, puis cliquer sur **Envoyer au Store** pour mettre à disposition la version mise à jour. Veillez à ce que les sélections d’entreprises dans **Visibilité** restent les mêmes, sauf si vous souhaitez leur apporter des modifications, par exemple en sélectionnant une autre entreprise pouvant acquérir l’application ou en supprimant l’une des entreprises auxquelles vous l’avez déjà distribuée.

Si vous souhaitez ne plus offrir une application que vous avez déjà publiée en tant qu’application métier et que vous souhaitez empêcher toute nouvelle acquisition, vous devez créer une soumission. En premier lieu, vous devez modifier votre sélection sous **Visibilité** en choisissant **Vente au détail** au lieu de **Distribution d’applications métier**. Puis, dans la section [Détectabilité](choose-visibility-options.md#discoverability), choisissez **Rendre ce produit disponible mais non détectable dans le Store** avec l’option **Empêcher l’acquisition**.

Une fois le processus de certification appliqué à la soumission, l’application n’est plus disponible pour de nouvelles acquisitions (les personnes qui en disposent déjà pourront cependant continuer à l’utiliser).

> [!NOTE]
> Si vous redéfinissez une application sur **Vente au détail**, vous devrez répondre au [questionnaire d’évaluation de l’âge](age-ratings.md) si vous ne l’avez pas encore fait, même si l’application n’est pas disponible pour de nouvelles acquisitions.


## <a name="distribute-lob-apps-through-sideloading"></a>Distribuer des applications métiers par chargement indépendant

En mettant vos applications à la disposition d’une entreprise par le biais de Microsoft Store pour Entreprises ou de Microsoft Store pour Éducation, vous offrez l’assurance que ces applications ont été signées par le Windows Store et qu’elles sont conformes aux politiques standard du Windows Store.

Dans certains cas, les entreprises ne souhaitent pas leurs applications métier devant être soumis via des partenaires (par exemple, que pour des raisons de conformité ou pour les applications qui ont besoin de fonctionnalités supplémentaires). Ces entreprises peuvent alors déployer les applications directement sur des machines par chargement indépendant, sans utiliser Microsoft Store pour Entreprises ni Microsoft Store pour Éducation.

Pour plus d'informations, voir [Charger la version test d'applications métier dans Windows 10](https://go.microsoft.com/fwlink/p/?LinkId=623433).

 

 




