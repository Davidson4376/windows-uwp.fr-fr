---
title: Bacs à sable Xbox Live avancés
description: Découvrez comment isoler efficacement le contenu en utilisant les bacs à sable Xbox Live comme un partenaire géré ou un ID@Xbox membre.
ms.assetid: bd8a2c51-2434-4cfe-8601-76b08321a658
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b4c54a4bf3ab665ded7bfaa45d3b6be9346c384d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599004"
---
# <a name="advanced-xbox-live-sandboxes"></a>Bacs à sable Xbox Live avancés

> **Remarque** cet article expliquent l’utilisation avancée de bacs à sable et est principalement applicable aux studios de jeu volumineux qui ont plusieurs équipes et les exigences relatives aux autorisations complexes.  Si vous êtes partie du programme Xbox Live Creators ou un ID@Xbox développeur, il est recommandé d’examiner le [Xbox Live bacs à sable Intro](../xbox-live-sandboxes.md)

La Xbox Live *bac à sable* fournit un ensemble de l’environnement privé pour le développement. Ce document explique quelles sont les bacs à sable, pourquoi elles existent, comment elles s’appliquent aux serveurs de publication, et leur impact sur les équipes Xbox internes. L’audience pour ce document est éditeurs qui créent des contenu Xbox One et utilisent les bacs à sable.

## <a name="summary"></a>Résumé

Dans le Xbox Live, il est uniquement un environnement de production unique dans lequel réside la version préliminaire (de développement et de la version bêta), de certification et de contenu de la vente au détail tous les.

Isolation du contenu est le moyen pour vous assurer qu’il n’y a aucune fuite de contenu de serveur de publication en production. Fondamentalement, l’isolation contenue garantit que n’importe quel utilisateur principal, le périphérique ou le titre qui demande l’autorisation d’accéder à une ressource (un titre ou un service) a été autorisé à accéder à la ressource. Avec l’isolation du contenu, les partitions sont organisées dans les bacs à sable où sont stockées les données de titre ou service. En d’autres termes, les stratégies d’autorisation sont définis dans l’étendue d’un bac à sable.

Les bacs à sable sont un moyen de partitionner les données de production. Avec les services d’ère Xbox 360, PartnerNet et ProductionNet sont deux environnements distincts. Avec les services d’ère Xbox One, contient un environnement de production unique *n* des environnements virtuels distincts, où chaque environnement virtuel est appelée un bac à sable. Étant donné qu’un environnement de production unique pour tout le contenu, bacs à sable sont des environnements virtuels véritablement uniques où les données générées dans un environnement ne peut pas traverser vers un autre.

La figure suivante montre un environnement de production unique dans lequel les éditeurs peuvent créer leurs bacs à sable de développement privé. Comptes de développement autorisés uniquement ou les kits de développement sont autorisés à accéder à ces bacs à sable.

Figure 1. Les bacs à sable dans un environnement de production.

![](../images/sandboxes/sandboxes_image1.png)

Tout comme PublisherA a son sandbox de développement, autres éditeurs ont leurs propres bacs à sable de développement. Le même ID de titre peut se trouver dans les bacs à sable différents mais les données générées pour l’ID de titre distinctes entre les bacs à sable.

Il existe deux bacs à sable système peuvent uniquement être remplies par Microsoft : CERT et vente au détail. Comme leur nom le suggère, le bac à sable CERT concerne les titres qui sont en cours de mise en production, alors que le bac à sable de vente au détail est le bac à sable représentant dollars réels qui est accessible par tous les utilisateurs de la vente au détail et les appareils avant la certification.

Tandis qu’un ID de titre était anciennement unique dans Xbox Live, maintenant un ID de titre et d’un ID de bac à sable est unique. Il en va de même pour l’ID de produit et d’autres espaces d’ID qui ont été une fois traitées comme uniques. Ils doivent maintenant être associés avec un ID de bac à sable. Toutes les données de Xbox Live seront principalement partitionnées par l’ID de bac à sable dans tout le système.

## <a name="initial-setup-for-a-title"></a>Programme d’installation initial pour un titre

Un titre est né dans le portail de développement Xbox (XDP) ou les partenaires. Il reçoit un ID de titre et un ID de produit et une configuration de service ID (SCID).

Dans ce nouveau monde, un titre ou un produit sur son propre ne signifie rien à Xbox Live. Étant donné que nous devons prendre en charge la vente au détail simultanée et l’utilisation de développement d’un titre, nous devons prendre en charge *instanciation* de titres afin de libérer et conserver les distinctions nécessaires. Une instance d’un titre réside dans un bac à sable et c’est là qu’interviennent les bacs à sable.

Pour créer un titre, un serveur de publication crée un groupe de produits, spécifie le genre pour le groupe de produits et crée ensuite des produits individuels qu’il contient. (Pour plus d’informations, consultez la documentation XDP). Le diagramme suivant illustre les relations entre un groupe de produits, un produit, une instance de produit et un bac à sable.

Figure 2. Les relations entre un groupe de produits, un produit, une instance de produit et un bac à sable.

![](../images/sandboxes/sandboxes_image2.png)

<a name="product-instances"></a>Instances de produit
-----------------
Un *instance product* est une projection de titre, produit et la configuration des données dans un bac à sable spécifique. Ces données sont décrite dans les trois domaines suivants : service de configuration, les métadonnées de catalogue et les fichiers binaires.

### <a name="service-configuration"></a>Configuration du service

> Définitions de configuration de service (événements, statistiques, primes, etc.). La configuration du service est définie au niveau de l’instance de produit.

### <a name="catalog-metadata"></a>Métadonnées de catalogue

> Les métadonnées qui se trouvent dans le catalogue, y compris le texte de vente, des composants des illustrations, des informations de disponibilité et à l’offre, le Gestionnaire de licences de données et bien plus encore.

### <a name="binary"></a>Binary

> Un fichier binaire peut être représenté de deux manières :

1.  Métadonnées uniquement à activer le chargement indépendant. Cela inclut l’ID de contenu, les informations de version et des informations de licence.

2.  Binaire complet propagées sur un CDN et téléchargeable pour un client.

<a name="getting-the-access-right"></a>Obtention de l’accès à droite
------------------------
Il existe deux types d’accès au contenu de votre Xbox One :

*L’accès au moment du design*: accès à partir d’un PC via l’outil XDP — permet de travailler sur vos produits à télécharger, organiser et travailler avec du contenu, de configuration et de métadonnées, mais ne pas les autoriser à exécuter ou lire des instances de vos produits.

*Accès d’exécution*: accès à partir d’une console Xbox, permet à vos développeurs, testeurs, réviseurs et finit par vos clients pour exécuter et lire des instances de produit.

**Remarque** afin d’être disponible pour l’accès de l’exécution, une instance de produit doit être placée dans un bac à sable. Une fois qu’une build est placée dans un bac à sable, les utilisateurs XDP ou devkit les appareils qui ont été accordés l’accès à ce sandbox peuvent exécuter l’instance. Pour ce faire, ils se connectent à Xbox One via une console Xbox, en utilisant l’une de leurs comptes de développement, des comptes spéciaux cette fonction comme virtuels des utilisateurs pour l’accès d’exécution.

Lorsque nous parlons de bacs à sable, nous parlons généralement accès d’exécution au contenu qui s’exécute sur Xbox Live. Pour accéder à un service sur le Xbox Live, un ID de titre est obligatoire. Une fois un **appxmanifest** contient un ID de titre, la console envoie l’ID de titre à Xbox Live. Les services de sécurité Xbox Live ne fournira pas revenir un jeton valide, sauf si l’entité de sécurité (périphérique ou utilisateur) a octroyé l’accès au titre.

Ce processus de validation est le cœur de l’isolation du contenu. Lorsqu’ils sont affichés à un niveau très élevé :

-   Un groupe principal peut contenir des ID d’utilisateur Xbox (XUIDs), ID d’appareil, ID de titre ou ID de service.

-   Un bac à sable peut contenir des ID de titre, des codes de produit ou des ID (SCIDs) de configuration de service.

-   Un groupe principal est un accès à un bac à sable.

Par conséquent, pour un utilisateur ou un appareil accéder à un titre de la version préliminaire dans un bac à sable, l’accès doit être accordé via XDP tout d’abord.

Figure 3. Un modèle pour la configuration d’un accès via XDP.

![](../images/sandboxes/sandboxes_image3.png)

L’efficacité de l’isolation du contenu est basé sur le fait que votre organisation possède les processus suivants :

-   Création de vos comptes d’utilisateur XDP, les comptes de développement que chaque utilisateur utilisera pour se connecter pour l’accès de l’exécution et les groupes d’utilisateurs dans lequel chaque utilisateur peut appartenir.

-   Création de groupes d’appareils de confiance consoles.

-   Spécification pour chacun de vos bacs à sable de développement précisément quels groupes d’utilisateurs et les groupes d’appareils ont accès aux instances de produit qu’il contient.

Un exemple de cette configuration est illustré dans la figure ci-dessous.

Figure 4. Informations d’identification d’un utilisateur non autorisé n’accéder pour le bac à sable, telle que les informations d’identification ordinaire d’un compte d’utilisateur XDP autorisé. Uniquement les informations d’identification du compte de développement détenue par le compte d’utilisateur XDP autorisé de réussissent à l’accès d’exécution pour le bac à sable et toutes les instances de produit qu’il contient.

![](../images/sandboxes/sandboxes_image4.png)

### <a name="dev-accounts-setup"></a>Paramètres des comptes de développement

Les comptes de développement Xbox One sont simplement standard comptes Microsoft (MSA) avec des règles spéciales pour les. Ils sont utilisés dans le Xbox Live pour le développement. Un compte de développement :

-   Doivent être créés à partir de XDP ou partenaires.

-   Le rôle est attribué développeur externe lorsque créées par les éditeurs.

-   Est lié au compte XDP ou compte espace partenaires qui a créé le compte de développement.

-   Peut uniquement se connecter à des kits de développement. Connexion d’accès est refusée à un compte de développement sur les périphériques de vente au détail.

-   Peuvent acheter des abonnement Xbox Live Gold développeur ou autres abonnements gratuitement afin de tester.

### <a name="user-group-setup"></a>Paramètres de groupe d’utilisateurs

Un groupe d’utilisateurs, le premier type de groupe principal, est une collection d’utilisateurs XDP. Lorsque les utilisateurs XDP sont ajoutés aux groupes d’utilisateurs, leurs comptes de développement de flux avec ces utilisateurs XDP.

Par conséquent, lorsqu’un groupe d’utilisateurs est affecté à un bac à sable, les comptes de développement associée XDP aux utilisateurs dans la mesure où un groupe d’utilisateurs sont ajoutés à des groupes de principal appropriés et les principaux groupes d’obtiennent une configuration de stratégie avec la ressource principale définie sauvegarde le bac à sable.

**Remarque** les groupes d’utilisateurs qui sont créées pour accéder aux bacs à sable sont les groupes d’utilisateurs qui sont utilisées pour empêcher l’accès aux données de configuration dans XDP pour les produits et les groupes de produits.

### <a name="device-setup"></a>Installation d’appareil

Un appareil est également ajouté à un groupe principal. Un appareil peut uniquement être utilisé comme un kit de développement si un droit est acheté via le Store de développeur de jeu et de l’appareil est configuré pour être un kit de développement. Une fois un appareil est configuré comme un kit de développement, l’appareil s’affiche dans la liste des appareils qui peuvent être ajoutés aux groupes de volumes.

### <a name="device-group-setup"></a>Installation de groupe de l’appareil

Un groupe d’appareils, le deuxième type de groupe principal, peut également obtenir l’accès aux bacs à sable. Le programme d’installation est similaire à la configuration du groupe utilisateur détaillée plus haut.

## <a name="sandboxes"></a>Sandboxes

<a name="what-is-a-sandbox"></a>Qu’est un bac à sable ?
------------------
Définir simplement, *un bac à sable est un moyen de partitionner les données en production*.

<a name="why-do-we-need-sandboxes"></a>Pourquoi avons-nous besoin les bacs à sable ?
-------------------------
Tout comme les utilisateurs et périphériques accéder aux titres, titres, accéder aux services. Nous présentons un concept de « groupe title » où ensembles de titres ont accès aux ressources du service.

Étant donné qu’un environnement de production unique pour Xbox One pour tout le contenu (version préliminaire et vente au détail), plusieurs instances (pré-version/vente au détail) d’un titre est empêchés d’exploitation sur les mêmes instances de ressources.

<a name="what-is-in-a-sandbox"></a>Nouveautés dans un bac à sable
---------------------
Un bac à sable contient une instance de produit pour chaque titre est ajouté pour le bac à sable.

<a name="what-is-a-sandbox-id"></a>Qu’est un ID de bac à sable ?
---------------------
Un ID de bac à sable est une unité de partitionnement de données pour un titre, un produit ou une configuration de service. Plusieurs titres peuvent exister dans le même bac à sable, qui est un composant requis pour qu’ils partagent des données de configuration de service.

Un ID de bac à sable (respecte la casse) est une chaîne au format suivant : &lt;PublisherMoniker&gt;.*n*. Un exemple d’ID bac à sable, XLDP.5, est expliqué ci-dessous :

-   Le *moniker du serveur de publication* est unique pour tous les serveurs de publication. Par conséquent, « XLPD » représente le moniker du serveur de publication pour ce serveur de publication particulière. Un moniker de serveur de publication est créé quand un serveur de publication est « activé » dans XDP par le Gestionnaire de compte de développeur.

-   Le chiffre *« n »* identifie le numéro de bac à sable. Dans ce cas, « 5 » est le bac à sable sixième créé pour ce serveur de publication.

Lorsque les données du titre se déplacent via les services, les services Xbox utilisent l’ID de bac à sable pour identifier de manière unique le « environnement » pour les données qui sont générées.

<a name="what-data-is-sandboxed"></a>Quelles sont les données seront sable ?
-----------------------
Le diagramme ci-dessous illustre les données utilisateur et le titre sont sable.

![](../images/sandboxes/sandboxes_image5.png)

<a name="global-override-sandbox"></a>Bac à sable de remplacement global
-----------------------
Un développeur définit l’ID de bac à sable sur son kit de développement et définit donc le bac à sable que le kit de développement s’exécute dans ; Cela est également appelé le bac à sable de remplacement global. Par conséquent, toutes les demandes adressées aux services Xbox Live (par exemple, achievements, matchmaking, gestion des licences, EDS, etc.) à partir de titres (applications de shell et régulière) dans le développement kit sont effectuées dans ce bac à sable.

Le bac à sable de remplacement global implique également que seul le contenu ingéré dans le bac à sable de remplacement global est visible lorsque parcouru.

<a name="types-of-sandboxes"></a>Types de bac à sable
-------------------------------------------------------------------------------------------------------------------------------------------------------
Il existe deux catégories différentes de bac à sable. Ces catégories sont définies comme suit :

-   *Les bacs à sable de serveur de publication*. Les éditeurs ont accès à leurs bacs à sable de développement. Il peuvent ressembler XLDP.0, XLDP.1, XLDP.2, XLDP.3, etc. Il s’agit où les serveurs de publication place leurs instances de produit du titre. Accès à ces bacs à sable est contrôlé pour les utilisateurs/appareils que le serveurs de publication accorde l’accès à

-   *Les bacs à sable Microsoft*. Il s’agit des bacs à sable intégrés : Vente au détail et CERT uniquement Microsoft est autorisé à publier sur ces bacs à sable protégés.

<a name="cert-sandbox"></a>Bac à sable CERT
------------
Si un titre est prêt pour la disponibilité générale, il doit parcourir tout d’abord de certification. Le bac à sable CERT est un bac à sable contrôlés par Microsoft uniquement les personnes de certification ayant accès à. Les éditeurs peuvent voir quel contenu ils propre traverse certification.

Toutes les instances de produit qui échouent en certification peuvent être renvoyés à un bac à sable de développement à être débogué et résolu par les éditeurs à l’aide de XDP.

<a name="retail-sandbox"></a>Bac à sable de la vente au détail
--------------
Le bac à sable de vente au détail constitue la destination finale pour tout le contenu qui est créé pour Xbox One.

Après qu’un titre est écoulé certification, il est ajouté à la sandbox de vente au détail. Seul le contenu signé vert est autorisé à s’exécuter dans le bac à sable de vente au détail. Ceci a une implication importante contrôlée par le serveur de publication des versions bêta sont également effectuées dans le bac à sable de vente au détail. Données générées dans le bac à sable de vente au détail représentent des données de production réels.

Notez que l’accès au contenu est que le bac à sable de vente au détail est toujours contrôlable via l’isolation de contenu.

Par exemple, contrôlée par le serveur de publication des versions bêta sont exécutés dans le bac à sable de la vente au détail, où le serveur de publication choisit les principaux groupes d’obtiennent accéder aux titres de jeu ressources bêta définie par le serveur de publication. Les données de service générées par les titres de la version bêta sont prod réelle des données et continue d’exister une fois que le titre est à la disposition générale.

<a name="cross-sandbox-data-interaction"></a>Interaction des données de cross-bac à sable
------------------------------
Par définition, un bac à sable est un conteneur qui restreint le partage de données. Par conséquent, l’interaction des données de cross-bac à sable n’est pas possible.

## <a name="organizing-your-sandboxes"></a>Organiser votre bacs à sable

Cette section fournit un exemple de la façon dont un serveur de publication peut organiser les bacs à sable. Un serveur de publication doit comprendre l’utilisation de bacs à sable pour organiser les données.

**Remarque** les exemples ci-dessous montrent uniquement la gestion de l’accès d’exécution avec l’isolation du contenu.

### <a name="scenario-1-two-titles-one-sandbox"></a>Scénario 1 : Deux titres, un bac à sable

La structure de base pour un serveur de publication peut être :

-   Deux titres qui sont accessibles à tous les utilisateurs et appareils détenus par le serveur de publication pour le moment du design et de runtime.

-   Instance d’un produit par titre.

Dans ce cas, le serveur de publication doit simplement un bac à sable unique pour tout le contenu en version préliminaire.

Le diagramme ci-dessous montre un groupe d’utilisateurs. Le serveur de publication peut choisir d’utiliser un groupe d’appareils au lieu d’un groupe d’utilisateurs, si cela s’avère plus facile. En outre, ce groupe d’utilisateurs a accès exécution et au moment du design pour le bac à sable XLDP.1 et les titres dans ce bac à sable.

![](../images/sandboxes/sandboxes_image6.png)

### <a name="scenario-2-one-title-different-teams"></a>Scénario 2 : Un titre, différentes équipes

Dans ce modèle, les spécifications sont :

-   Un titre.

-   Équipe de développement fonctionne sur les builds quotidiennes.

-   Équipe l’assurance qualité exécute LKGs hebdomadaire.

-   Équipe de développement doit déboguer LKGs hebdomadaire en cas de bogues.

-   Le service financier doit accéder à des cartes de prix et d’autres métadonnées liées à la version de catalogue d’un titre.

La figure ci-dessous montre que TitleX a deux instances de produit : PI-1 et 2 de PI. Une instance de produit doit se trouver dans un bac à sable et deux instances de produit du même titre ne peut pas être dans le même bac à sable. Par conséquent, TitleX-PI-1 est dans un bac à sable XLDP.1 et TitleX-PI-2 est dans un bac à sable XLDP.2.

Le groupe d’utilisateurs de développement a accès à ces deux bacs à sable, alors que le groupe d’utilisateurs de questions et réponses a accès à uniquement bac à sable XLDP.2.

En outre, l’utilisateur de finance (groupe C) a accès au moment du design à TitleX. Étant donné que le groupe d’utilisateurs de finance est généralement pas de débogage de l’exécution d’un titre, ils sont séparés et répartis.

**Remarque** , quelle que soit l’organisation, un utilisateur XDP peut appartenir à plusieurs groupes de l’utilisateur.

![](../images/sandboxes/sandboxes_image7.png)

### <a name="scenario-3-two-titles-completely-separate"></a>Scénario 3 : Deux titres, totalement distinctes

Dans cet exemple, les exigences changent un peu :

-   Titres des deux.

-   Accès à chaque titre doit être limité à un certain ensemble de personnes.

-   Instance d’un produit par titre.

-   Un groupe d’utilisateurs administrateur qui a besoin d’accéder aux données de configuration XDP au moment du design pour les titres. Les individus dans ce groupe sont tous les administrateurs du serveur de publication et peuvent contrôler toutes les données qui sont publiées dans le catalogue (métadonnées de catalogue, finance, marketing, envoi de la certification, etc.).

Dans ce modèle, le serveur de publication a choisi de conserver les deux titres complètement séparées et donc attribué ces deux titres dans deux bacs à sable différents. Le serveur de publication a également choisi pour créer un groupe d’utilisateurs administrateur distincts et affecter l’accès à ces deux produits.

![](../images/sandboxes/sandboxes_image8.png)

### <a name="scenario-4-anyway-you-like-it"></a>Scénario 4 : Malgré tout vous le souhaitez

En raison du nombre de connexions et pour conserver la formulation courte, nous avons choisi d’afficher uniquement le bac à sable connexions au moment de l’exécution. Rien ne vous empêche d’ajouter d’autres autorisations d’accès d’au moment du design.

Dans cet exemple, la configuration requise est :

-   Seules certaines personnes ont accès à certains titres à l’intérieur de leur serveur de publication.

-   Le serveur de publication fonctionne avec les fournisseurs à partir de différentes entreprises, et ces fournisseurs peuvent être à court terme.

-   Le serveur de publication doit être en mesure de mettre hors service un titre et en procédant ainsi, empêcher l’accès à toutes les données que les fournisseurs ou FTE avait accès.

Afin de modéliser cette exigence, une structure comme celui ci-dessous peut être adoptée.

Le modèle de suivi ci-dessous est :

-   TitleX et TitleY ont chaque qu’une seule instance de produit dans le bac à sable XLDP.1.

-   TitleZ a des instances de deux produits, un dans le bac à sable XLDP.2 et un autre dans le bac à sable XLDP.3.

-   FTE utilisateur groupe B est d’accéder aux instances de produit dans tous les bacs à sable.

-   Groupe d’utilisateurs de fournisseur A est un groupe d’utilisateurs uniquement dans le fournisseur qui aura un accès à un bac à sable XLDP.1.

-   Groupe d’appareils fournisseur C est un groupe d’utilisateurs uniquement dans le fournisseur qui aura un accès à un bac à sable XLDP.3.

![](../images/sandboxes/sandboxes_image9.png)

## <a name="summary"></a>Résumé

Développement Xbox Live fournit une formidable opportunité aux serveurs de publication pour tester en production avec les services de qualité de production et les comptes de développeurs MSA de production. L’augmentation de la fonctionnalité et la flexibilité nécessite de nouvelles étapes de configuration dans XDP pour créer des données de titre et de gérer l’accès aux titres tout dans le développement et la disposition générale.

Les bacs à sable sont un moyen de partitionner les données en production. Étant donné qu’un environnement de production unique pour tout le contenu, les bacs à sable agissent en tant que « environnements virtuels » où les données générées dans un environnement ne pas traverser plusieurs à l’autre.
