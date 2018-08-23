---
author: jnHs
Description: Set restrictions on how your app can be discovered and acquired, including whether people can find your app in the Store or see its Store listing at all.
title: Choisir les options de visibilité
ms.author: wdg-dev-content
ms.date: 08/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, visibilité, public privé, disponible, détectable
ms.localizationpriority: medium
ms.openlocfilehash: 07986353be41fcc9ef9dd9406fb0b30c4aa3d7f2
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2819879"
---
# <a name="choose-visibility-options"></a>Choisir les options de visibilité


La section **Visibilité** de la [page Tarification et disponibilité](set-app-pricing-and-availability.md) vous permet de définir des restrictions relatives aux modes de découverte et d’acquisition de votre application. Cela vous permet de spécifier si les personnes peuvent trouver votre application dans le Store ou voir sa description dans le Store.

La section Visibilité comporte deux sections distinctes: **Public** et **Détectabilité**. 

## <a name="audience"></a>Public

La section Public vous permet de spécifier si vous souhaitez restreindre la visibilité de votre soumission à un public spécifique que vous définissez.


### <a name="public-audience"></a>Public non privé

Par défaut, la description de votre application dans le Store sera visible par un **Public non privé**. Cela est approprié pour la plupart des soumissions, sauf si vous souhaitez restreindre l’affichage de la description de votre application à des personnes spécifiques. Vous pouvez également utiliser les options de la section [Détectabilité](#discoverability) pour limiter la détectabilité si vous le souhaitez.

> [!IMPORTANT]
> Si vous soumettez un produit avec cette option définie sur **Public non privé**, vous ne pourrez pas choisir **Public privé** dans le cadre d’une soumission ultérieure.


### <a name="private-audience"></a>Public privé

Si vous souhaitez que la description de votre application soit visible uniquement par des personnes sélectionnées, choisissez **Public privé**. Avec cette option, l’application ne sera pas détectable ni accessible aux personnes autres que celles faisant partie du ou des groupes spécifié(s). Cette option est souvent utilisée pour les [tests bêta](beta-testing-and-targeted-distribution.md), car elle vous permet de distribuer votre application aux testeurs sans que personne d’autre ne puisse l’obtenir, ni même voir sa description dans le Store (même si un utilisateur a entré l’URL de sa description dans le Store).

Lorsque vous choisissez **Public privé**, vous devez spécifier au moins un groupe de personnes qui doivent obtenir votre application. Vous pouvez choisir parmi un [groupe d’utilisateurs connus](create-known-user-groups.md) existant ou vous pouvez sélectionner **Créer un nouveau groupe** pour définir un nouveau groupe. Vous devrez saisir les adresses de messagerie associées au compte Microsoft de chaque personne que vous souhaitez inclure dans le groupe. Pour plus d’informations, consultez l’article [Créer des groupes d’utilisateurs connus](create-known-user-groups.md).

Une fois votre soumission publiée, les personnes figurant dans le groupe spécifié pourront afficher la description de l’application et télécharger cette dernière, dans la mesure où elles sont connectées avec le compte Microsoft associé à l’adresse de messagerie que vous avez indiquée et qu’elles exécutent Windows10, version1607 ou ultérieure (y compris XboxOne). Toutefois, les personnes qui ne font pas partie de votre public privé ne pourront pas afficher la description de l’application ou télécharger cette dernière, quelle que soit la version du système d’exploitation qu’elles exécutent. Vous pouvez publier des soumissions mises à jour pour le public privé, qui seront distribuées aux membres de ce public de la même façon qu’une mise à jour habituelle d’application (mais qui ne seront toujours pas disponibles aux personnes qui ne font pas partie de votre public privé, sauf si vous modifiez le public sélectionné). 

Si vous envisagez de rendre l’application disponible à un public non privé à une date et une heure données, vous pouvez cocher la case **Rendre ce produit public le** lors de la création de votre soumission. Entrez la date et l’heure (au formatUTC) auxquelles vous souhaitez que le produit devienne public. Gardez à l’esprit les points suivants:

- La date et l’heure que vous sélectionnez s’appliqueront à tous les marchés. Si vous souhaitez personnaliser la planification de la publication pour différents marchés, n’utilisez pas cette option. Au lieu de cela, créez une nouvelle soumission en modifiant votre paramètre sur **Public non privé**, puis utilisez les options [Planifier](configure-precise-release-scheduling.md) pour spécifier votre calendrier de publication.
- La date saisie pour **Rendre ce produit public le** ne s’applique pas au MicrosoftStore pour Entreprises et/ou au MicrosoftStore pour Éducation. Pour nous autoriser à proposer votre application à ces clients par le biais de la gestion de licences organisationnelles, vous devez créer une nouvelle soumission en sélectionnant **Public non privé** (et en activant la [gestion des licences organisationnelles](organizational-licensing.md)).
- Après la date et l’heure sélectionnées, toutes les soumissions utiliseront le paramètre **Public non privé**.

Si vous ne spécifiez pas une date et une heure pour rendre votre application disponible pour un public non privé, vous pourrez toujours le faire ultérieurement en créant une nouvelle soumission et en modifiant le paramètre **Public privé** en **Public non privé**. Lorsque vous procédez ainsi, n’oubliez pas que votre application peut être soumise à un processus de certification supplémentaire. Soyez donc prêt à résoudre tout nouveau problème de certification. 

Voici quelques éléments importants à prendre en considération lorsque vous choisissez de distribuer votre application à un public privé:
- Les personnes faisant partie de votre public privé pourront obtenir l’application en utilisant un lien spécifique vers la description de votre application dans le Store. Pour l’afficher, elles devront se connecter avec leur compte Microsoft. Ce lien est fourni lorsque vous sélectionnez **Public privé**. Vous pouvez également le trouver sur votre page [Identité des applications](view-app-identity-details.md) sous **URL si votre application n'est visible que pour certaines personnes (nécessite l'authentification)**. Veillez à transmettre ce lien à vos testeurs, et non l’URL habituelle vers votre description dans le Store.  
- À moins de choisir une option dans **Détectabilité** qui les en empêche, les personnes de votre public privé pourront trouver votre application en effectuant une recherche dans l’application MicrosoftStore. Toutefois, la description web de votre application ne sera pas détectable via la recherche, même pour les personnes faisant partie de ce public. 
- Vous ne pourrez pas [configurer les dates de publication dans la section Planifier](configure-precise-release-scheduling.md) de la **page Tarification et disponibilité**, dans la mesure où votre application ne sera pas publiée pour les clients ne faisant pas partie de votre public privé.
- Les autres sélections que vous effectuez seront appliquées aux personnes de ce public. Par exemple, si vous choisissez un prix autre que **Gratuit**, les personnes de votre public privé devront payer ce prix afin d’acquérir l’application. 
- Si vous souhaitez distribuer des packages différents à différentes personnes de votre public privé, une fois votre soumission initiale effectuée, vous pouvez utiliser les [versions d’évaluation de package](package-flights.md) pour distribuer des mises à jour de package différentes à des sous-ensembles de votre public privé. Vous pouvez créer des groupes d’utilisateurs connus supplémentaires pour définir les personnes susceptibles d’obtenir une version d’évaluation de package spécifique.
- Vous pouvez modifier l’appartenance au(x) groupe(s) d’utilisateurs connus dans votre public privé. Toutefois, n’oubliez pas que si vous supprimez une personne qui faisait partie du groupe et qui a précédemment téléchargé votre application, elle sera toujours en mesure d’utiliser l’application, mais elle ne pourra pas obtenir les mises à jour que vous fournissez (sauf si vous choisissez **Public non privé** ultérieurement).
- Votre application ne sera pas disponible par le biais du MicrosoftStore pour Entreprises et/ou MicrosoftStore pour Éducation, indépendamment de vos paramètres de gestion des licences organisationnelles, même pour les personnes de votre public privé.
- Alors que le Store s’assurera que votre application n’est visible et disponible que pour les personnes connectées avec un compte Microsoft que vous avez ajouté à votre public privé, nous ne pouvons pas empêcher ces personnes de partager des informations ou des captures d’écran en dehors de votre public privé. Si la confidentialité est une préoccupation majeure, veillez à ce que votre public privé n’inclue que des personnes de confiance qui ne partageront aucune information sur votre application avec d’autres utilisateurs.
- Veillez à informer vos testeurs sur la façon dont ils peuvent vous transmettre leurs commentaires. Vous ne souhaiterez sans doute pas qu’ils laissent des commentaires dans le Hub de commentaires, car tous les autres clients pourront voir leurs commentaires. Envisagez d’inclure un lien pour leur permettre d’envoyer un e-mail ou de fournir des commentaires de toute autre manière.
- Les avis rédigés par des individus de votre public privé vous seront disponibles pour une consultation. Toutefois, ces avis ne seront pas publiés dans la description de votre application dans le Store, même si votre soumission est ensuite définie sur **Public non privé**. Vous pouvez lire les avis rédigés par votre public privés en examinant le [Rapport sur les avis](reviews-report.md) dans le centre de développement, mais vous ne pouvez pas télécharger ces données ou utiliser l'[API d'analyse du MicrosoftStore](../monetize/access-analytics-data-using-windows-store-services.md) pour accéder à ces avis sur programmation.
- Lorsque vous modifiez une application de **Public privé** à **Public non privé**, la **Date de publication** indiquée dans la description dans le Store correspond à la date de sa première publication pour un public non privé.

## <a name="discoverability"></a>Détectabilité

Les sélections effectuées dans la section **Détectabilité** indiquent comment les clients peuvent découvrir et acquérir votre application. 

> [!IMPORTANT]
> Si vous sélectionnez **Public privé**, vos sélections pour **Détectabilité** s’appliquent uniquement aux personnes de votre public privé. Les clients qui ne font pas partie des groupes que vous avez spécifiés ne seront pas en mesure d’obtenir l’application ni même de voir sa description. 


### <a name="make-this-product-available-and-discoverable-in-the-store"></a>Rendre ce produit accessible et détectable dans le Store

Il s’agit de l’option par défaut. Laissez cette option si vous souhaitez que votre application figurer dans le magasin pour les clients Rechercher par d’autres méthodes, notamment la recherche, l’exploration et inclusion dans les listes curated et/ou de via le lien direct de l’application. 

### <a name="make-this-product-available-but-not-discoverable-in-the-store"></a>Rendre ce produit accessible mais non détectable dans le Store

Lorsque vous sélectionnez cette option, votre application est introuvable dans le Store par les clients, que ce soit par recherche ou par navigation; la seule façon d’accéder à la description de votre application est d’utiliser un lien direct. 

> [!TIP]
> Si vous ne voulez pas que l’affichage de la description soit public, même avec un lien direct, choisissez **Public privé** dans la section **Public**, comme décrit ci-dessus.

Vous devez également choisir l’une des options suivantes pour spécifier comment votre application peut être obtenue:


>[!IMPORTANT]
> Chacune de ces options limite les versions des systèmes d’exploitation sur lesquelles les clients peuvent acquérir votre application. Lisez attentivement les descriptions pour vous assurer de savoir quelles versions des systèmes d’exploitation sont prises en charge. 

- **Lien direct uniquement: n’importe quel client ayant un lien direct vers la description du produit peut télécharger celui-ci, sauf sous Windows8.x.** Tout client parvenant à la description de votre application via un lien direct peut la télécharger sur des appareils exécutant Windows10 ou sur des appareils exécutant WindowsPhone8.1 ou version antérieure (mais non sur les appareils exécutant Windows8.x).
- **Arrêter l’acquisition: n’importe quel client ayant un lien direct peut voir la description du produit dans le Store, mais ne peut télécharger ce produit que s'il en est déjà propriétaire ou s'il possède un code promotionnel et utilise un appareil Windows10.** Même si un client dispose d’un lien direct, il ne peut pas télécharger l’application, sauf s'il possède un [code promotionnel](generate-promotional-codes.md) et utilise un appareil Windows10. Si un client possède un code promotionnel, il peut l’utiliser pour obtenir gratuitement votre application (sous Windows10 uniquement), même si vous ne l’offrez à aucun autre client. En dehors d'un code promotionnel, il n’existe aucun moyen pour qui que ce soit d’obtenir votre application.

> [!TIP]
> Si vous voulez cesser de proposer une application à de nouveaux clients, vous pouvez sélectionner **Rendre votre application indisponible** depuis sa page d'aperçu. Quelques heures après que vous avez confirmé vouloir la rendre indisponible, votre application disparaît du Store. Dès lors, aucun nouveau client ne peut plus y accéder (sauf s'il possède un [code promotionnel](generate-promotional-codes.md) et utilise un appareil Windows10). Cette action remplacera les sélections **Visibilité** de votre soumission. Pour rendre à nouveau l'application disponible pour de nouveaux clients (conformément à vos sélections de **Visibilité**), vous pouvez cliquer à tout moment sur **Rendre l'application disponible** depuis la page d'aperçu. Pour en savoir plus, consultez l’article [Suppression d’une application du Store](guidance-for-app-package-management.md#removing-an-app-from-the-store).




