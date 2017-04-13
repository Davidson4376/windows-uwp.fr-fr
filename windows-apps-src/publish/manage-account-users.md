---
author: jnHs
Description: "Ajoutez des utilisateurs à votre compte du Centre de développement, affectez-leur des rôles avec des autorisations spécifiques."
title: "Gérer des utilisateurs de compte"
ms.assetid: 9245F0D0-7D8F-4741-AFB4-FBA5601D0A9B
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 7eaddcfc2d02805e60043132328ef482872a9000
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="manage-account-users"></a>Gérer des utilisateurs de compte


Vous pouvez utiliser Azure Active Directory pour ajouter des utilisateurs à votre compte du Centre de développement. Un rôle est attribué à chaque utilisateur. Il lui confère un ensemble d’autorisations spécifique à l’égard du compte. Vous pouvez également attribuer un rôle à un groupe d’utilisateurs ou à une application Azure AD.

> **Important**  Pour ajouter et gérer des utilisateurs de comptes, vous devez d’abord associer votre compte du Centre de développement au service Azure Active Directory de votre organisation. Pour cela, vous devez vous connecter à Azure AD avec un compte [Administrateur global](http://go.microsoft.com/fwlink/?LinkId=746654). Une fois cette association établie, vous ne pouvez plus la supprimer sans prendre contact avec le support.

## <a name="associate-your-dev-center-account-with-your-organizations-azure-active-directory"></a>Associer votre compte du Centre de développement au service Azure Active Directory de votre organisation

Le Centre de développement Windows tire parti d’Azure Active Directory pour la gestion multi-utilisateurs et l’attribution des rôles. Si votre organisation utilise déjà Office 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’Azure AD. Dans le cas contraire, vous pouvez créer un AzureAD depuis le Centre de développement sans frais supplémentaires.

Notez que vous ne pouvez associer qu’un seul compte du Centre de développement à un service Azure AD. De même, il n’est possible d’associer qu’un seul service Azure AD à un compte du Centre de développement.

> **Remarque**   Si les utilisateurs que vous souhaitez ajouter ne font pas partie du service AzureAD de votre organisation et que vous ne souhaitez pas créer de compte AzureAD, vous pouvez les [inviter par courrier électronique](#add-and-manage-account-users).

### <a name="associate-your-dev-center-account-with-your-organizations-existing-azure-ad"></a>Associer votre compte du Centre de développement au service Azure AD existant de votre organisation

Si votre organisation utilise déjà AzureAD, suivez ces étapes pour lier votre compte du Centre de développement.

1.  Sous **Paramètres du compte**, cliquez sur **Gérer les utilisateurs**.
2.  Cliquez sur le bouton **Associer Azure AD avec votre compte du Centre de développement**.
3.  Connectez-vous à votre compte Azure AD. La configuration de l’association implique que ce compte dispose de l’autorisation [Administrateur général](http://go.microsoft.com/fwlink/?LinkId=746654).
4.  Vérifiez le nom d’organisation et le nom de domaine pour votre compte Azure AD. Pour valider l’association, cliquez sur **Confirmer**.
5.  Si l’association réussit, vous êtes prêt à ajouter et gérer des utilisateurs de comptes sur la page **Gérer les utilisateurs** de votre compte, tel que décrit dans les sections ci-dessous.

### <a name="create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account"></a>Créer un tout nouvel AzureAD à associer à votre compte du Centre de développement

Si vous avez besoin de configurer un nouvel Azure AD à lier à votre compte du Centre de développement, procédez comme suit.

1.    Sous **Paramètres du compte**, cliquez sur **Gérer les utilisateurs**.
2.    Cliquez sur le bouton **Créer Azure AD**.
3.    Entrez les informations de répertoire de votre nouvel Azure AD:
 - **Nom de domaine**: nom unique que nous utiliserons pour votre domaine Azure AD, avec «.onmicrosoft.com». Par exemple, si vous avez entré «exemple», votre domaine Azure AD sera «exemple.onmicrosoft.com».
 - **E-mail de contact**: adresse de messagerie à laquelle nous pouvons vous contacter à propos de votre compte si nécessaire.
 - **Informations de compte d’utilisateur administrateur général**: prénom, nom, nom d’utilisateur et mot de passe que vous souhaitez utiliser pour le nouveau compte d’administrateur.
4.    Cliquez sur **Créer** pour confirmer les nouvelles informations de domaine et de compte.
5.    Connectez-vous avec vos nouveaux nom d’utilisateur et mot de passe d’administrateur général Azure AD pour commencer à ajouter et à gérer des utilisateurs de compte supplémentaires sur la page **Gérer les utilisateurs** de votre compte, tel que décrit dans les sections ci-dessous.


> **Important**  Après avoir associé votre compte du Centre de développement Azure AD, vous aurez toujours besoin de vous connecter au Centre de développement à l’aide du compte d’administrateur général Azure AD (et non d’un compte Microsoft personnel) afin d’ajouter et de gérer les utilisateurs de compte.

## <a name="add-and-manage-account-users-groups-and-azure-ad-applications"></a>Ajouter et gérer des utilisateurs, des groupes et des applications Azure AD pour le compte

Une fois l’association établie, vous pouvez ajouter des utilisateurs, des groupes et des applications Azure AD à votre compte. Vous pouvez également modifier les rôles et les détails de compte ou supprimer des utilisateurs.

> **Remarque**  Si votre organisation utilise l’[intégration du répertoire](http://go.microsoft.com/fwlink/p/?LinkID=724033) pour synchroniser le service d’annuaire local avec votre compte Azure AD, vous ne pourrez pas créer d’utilisateurs, de groupes ou d’applications Azure AD dans le Centre de développement. Vous (ou un autre administrateur de votre annuaire local) devrez créer ces éléments directement dans l’annuaire local avant de pouvoir les afficher et les ajouter dans le Centre de développement.

Lorsque vous gérez des utilisateurs, gardez à l’esprit les points suivants :

-   Tous les utilisateurs du Centre de développement doivent disposer d’un compte actif auprès du service Azure AD de votre organisation.
-   Lorsque vous **créez** un utilisateur ou un groupe dans le Centre de développement, il est également ajouté au service Azure AD de votre organisation.
-   Lorsque vous modifiez le nom d’un utilisateur ou d’un groupe dans le Centre de développement, les modifications sont répercutées dans le service Azure AD de votre organisation.
-   Les utilisateurs (y compris les groupes et applications AzureAD) pourront accéder à l’ensemble du compte du Centre de développement avec les autorisations associées au rôle qui leur est attribué. Si vous [personnalisez les autorisations](set-custom-permissions-for-account-users.md), vous pouvez limiter l’accès d’un utilisateur de sorte qu’il puisse utiliser uniquement des applications et/ou modules complémentaires.
-   Vous pouvez autoriser un utilisateur, un groupe ou une application Azure AD à accéder aux fonctionnalités de différents rôles en sélectionnant plusieurs rôles ou en utilisant les [autorisations personnalisées](set-custom-permissions-for-account-users.md) pour accorder l’accès de votre choix.
-   Un utilisateur ayant un certain rôle (ou un ensemble [d’autorisations personnalisées](set-custom-permissions-for-account-users.md)) peut également faire partie d’un groupe ayant un rôle différent (ou un ensemble d’autorisations). Dans ce cas, l’utilisateur a accès à toutes les fonctionnalités associées au groupe et au compte individuel.

## <a name="roles-and-permissions"></a>Rôles et autorisations

Lorsque vous ajoutez un utilisateur, un groupe ou une application AzureAD, vous devez spécifier ses autorisations. Pour cela, vous pouvez leur accorder un **rôle standard** ou [personnaliser leurs autorisations](set-custom-permissions-for-account-users.md).

À moins d’utiliser des autorisations personnalisées, chaque utilisateur, groupe ou application AzureAD ajoutée à un compte doit se voir attribuer au moins l’un des rôles standard ci-après. Chaque rôle dispose d’un ensemble d’autorisations spécifique lui permettant d’exécuter certaines fonctions dans le cadre du compte. 

> **Remarque**  Le propriétaire du compte est la personne qui l’a créé en premier avec un compte Microsoft (et non l’un des utilisateurs ajoutés par le biais d’Azure AD). Il est le seul à disposer d’un accès complet au compte et à pouvoir notamment supprimer des applications, créer et modifier l’ensemble des utilisateurs du compte et modifier tous les paramètres financiers et de compte. 

| Rôle                 | Description              |
|----------------------|--------------------------|
| Manager              | Dispose d’un accès complet au compte, mais ne peut pas modifier les paramètres fiscaux et de revenus. Ceci inclut la gestion des utilisateurs dans le Centre de développement. Cependant, notez que la possibilité de créer et supprimer des utilisateurs dépend des autorisations du compte dans Azure AD. Ainsi, si le rôle Manager est attribué à un utilisateur mais que celui-ci ne dispose pas des autorisations d’administration dans le service Azure AD de l’organisation, il ne pourra pas créer d’utilisateurs ni supprimer des utilisateurs de l’annuaire (mais pourra modifier le rôle d’un utilisateur dans le Centre de développement). |
| Développeur            | Peut charger des packages, soumettre des applications et modules complémentaires et afficher le [Rapport d’utilisation](usage-report.md) pour obtenir des informations de télémétrie détaillées. Il ne peut afficher ni les informations financières ni les paramètres de compte.   |
| Contributeur professionnel | Peut afficher des rapports [d’intégrité](health-report.md) et [d’utilisation](usage-report.md). Impossible de créer ou soumettre des produits, de modifier des paramètres de compte ou d’afficher des informations financières.                                         |
| Contributeur financier  | Peut afficher des [rapports sur les revenus](payout-summary.md), des informations financières et des rapports d’acquisition. Il ne peut apporter aucune modification aux applications, modules complémentaires et paramètres de compte.                                                                                                                                   |
| Responsable marketing             | Peut [répondre aux avis de clients](respond-to-customer-reviews.md) et afficher des [rapports analytiques](analytics.md) non financiers. Il ne peut apporter aucune modification aux applications, modules complémentaires et paramètres de compte.      |

Le tableau ci-dessous présente certaines fonctionnalités spécifiques disponibles pour chacun de ces rôles (et pour le propriétaire du compte).

|                                 |    Propriétaire du compte                 |    Responsable                       |    Développeur                     |    Contributeur professionnel    |    Contributeur financier    |    Responsable marketing                      |
|---------------------------------|----------------------------------|----------------------------------|----------------------------------|----------------------------|---------------------------|----------------------------------|
|    Rapport d’acquisition           |    Peut afficher                      |    Peut afficher                      |     Aucun accès                    |     Aucun accès              |    Peut afficher               |    Aucun accès                     |
|    Rapport de commentaires/réponses    |    Peut afficher et envoyer des commentaires    |    Peut afficher et envoyer des commentaires    |    Peut afficher et envoyer des commentaires    |     Aucun accès              |     Aucun accès             |    Peut afficher et envoyer des commentaires    |
|    Rapport d’intégrité                |    Peut afficher                      |    Peut afficher                      |    Peut afficher                      |    Peut afficher                |     Aucun accès             |    Aucun accès                     |
|    Rapport d’utilisation                 |    Peut afficher                      |    Peut afficher                      |    Peut afficher                      |    Peut afficher                |     Aucun accès             |    Aucun accès                     |
|    Compte de revenu               |    Peut mettre à jour                    |    Aucun accès                     |    Aucun accès                     |    Aucun accès               |    Peut afficher               |    Aucun accès                     |
|    Profil fiscal                  |    Peut mettre à jour                    |    Aucun accès                     |    Aucun accès                     |    Aucun accès               |    Peut afficher               |    Aucun accès                     |
|    Résumé du paiement               |    Peut afficher                      |    Aucun accès                     |    Aucun accès                     |    Aucun accès               |    Peut afficher               |    Aucun accès                     |

Si aucun des rôles standard ne convient, ou si vous souhaitez limiter l’accès à des applications spécifiques et/ou des modules complémentaires, vous pouvez accorder des autorisations personnalisées à l’utilisateur en cliquant sur **Personnaliser les autorisations**. Pour plus d’informations, consultez [Définir des autorisations personnalisées pour les utilisateurs de compte](set-custom-permissions-for-account-users.md).

## <a name="add-and-manage-account-users"></a>Ajouter et gérer des utilisateurs du compte

Pour identifier les utilisateurs que vous souhaitez ajouter à votre compte du Centre de développement et leur attribuer un rôle, cliquez sur **Ajouter des utilisateurs**.

Vous pouvez ajouter un ou plusieurs utilisateurs à votre compte du Centre de développement à partir de l’annuaire de votre organisation. Notez que lorsque vous ajoutez plusieurs utilisateurs simultanément, vous devez leur attribuer le même rôle. Si vous souhaitez ajouter des utilisateurs et leur attribuer des rôles différents, répétez les étapes suivantes pour chaque rôle.

Sinon, vous pouvez inviter les utilisateurs à accéder à votre compte sans les ajouter à l’annuaire de votre organisation.

**Ajouter des utilisateurs à partir de l’annuaire de votre organisation**

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des utilisateurs**.
2.  Sélectionnez un ou plusieurs utilisateurs dans la liste qui s’affiche. Vous pouvez utiliser la zone de recherche pour rechercher des utilisateurs spécifiques.
3.  Une fois les utilisateurs sélectionnés, cliquez sur **Ajouter la sélection**.
4.  Dans la section **Rôles**, sélectionnez un ou plusieurs rôles à attribuer à cet ensemble d’utilisateurs.
5.  Cliquez sur **Enregistrer**.

Si vous souhaitez accorder à un nouveau compte d’utilisateur l’accès au Centre de développement, vous pouvez créer ce compte dans la section **Gérer les utilisateurs**. 

Par défaut, la case d’option **Ajouter à Azure AD** est activée. Si vous la laissez activée, un compte est créé dans l’annuaire de votre organisation, et cet utilisateur est ajouté à votre compte du Centre de développement. Si vous ne voulez pas créer de comptes dans l’annuaire de votre organisation, mais que vous souhaitez laisser les utilisateurs accéder à votre compte à l’aide de leurs comptes Microsoft pour se connecter, sélectionnez **Inviter les utilisateurs par e-mail** à la place.

Pour que le nouvel utilisateur ait un [compte d’administrateur global](http://go.microsoft.com/fwlink/p/?LinkId=746654) dans l’annuaire de votre organisation, activez la case à cocher **Faire de cet utilisateur un administrateur global dans Azure AD, avec contrôle complet de toutes les ressources de l’annuaire**. Ainsi, l’utilisateur a un accès complet à toutes les fonctionnalités administratives de votre annuaire Azure AD. Il peut ajouter et gérer des utilisateurs dans l’annuaire de votre organisation (mais pas dans le Centre de développement, sauf si vous accordez [le rôle/les autorisations](#roles-and-permissions) appropriés au compte. Si vous activez cette case à cocher, vous devez fournir un **e-mail de récupération de mot de passe** pour l’utilisateur.

**Créer un compte d’utilisateur dans le Centre de développement et dans l’annuaire de votre organisation**

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des utilisateurs**.
2.  Sur la page suivante, cliquez sur **Nouvel utilisateur**.
3.  Assurez-vous que la case d’option **Ajouter à Azure AD** est activée.
4.  Entrez le prénom, le nom et le nom d’utilisateur du nouvel utilisateur.
5.  Entrez une adresse e-mail que l’utilisateur peut utiliser s’il a besoin de récupérer son mot de passe. Cette option n’est nécessaire que si vous avez activé la case **Faire de cet utilisateur un administrateur global dans Azure AD**.
6.  Dans la section **Rôles**, sélectionnez un ou plusieurs rôles à attribuer à ce nouvel utilisateur ou attribuez des autorisations personnalisées.
7.  Dans la section **Appartenance au groupe**, sélectionnez les groupes auxquels doit appartenir le nouvel utilisateur.
8.  Cliquez sur **Enregistrer**.
9.  Les informations de connexion du nouvel utilisateur, y compris un mot de passe temporaire, s’affichent sur la page de confirmation. Veillez à noter ces informations et à les fournir au nouvel utilisateur. Vous n’aurez plus accès au mot de passe temporaire après avoir quitté cette page.

**Créer un compte d’utilisateur dans le Centre de développement sans ajouter l’utilisateur à l’annuaire de votre organisation**

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des utilisateurs**.
2.  Sur la page suivante, cliquez sur **Nouvel utilisateur**.
3.  Sélectionnez la case d’option **Inviter les utilisateurs par e-mail**.
3.  Entrez une ou plusieurs adresses e-mail (jusqu'à dix), séparées par des virgules ou des points-virgules.
4.  Dans la section **Rôles**, sélectionnez un ou plusieurs rôles à attribuer à ce nouvel utilisateur ou attribuez des autorisations personnalisées.
6.  Cliquez sur **Enregistrer**.

Les utilisateurs invités reçoivent un e-mail avec une invitation pour accéder à votre compte du Centre de développement. Chaque utilisateur doit accepter son invitation avant de pouvoir accéder à votre compte. Pour renvoyer l’invitation, recherchez l’utilisateur dans votre page **Gestion des utilisateurs** et cliquez sur son adresse e-mail (ou sur le texte indiquant **Invitation en attente**) pour modifier le compte. Puis, en bas de la page, cliquez sur **Renvoyer invitation**.

Vous pouvez modifier des comptes d’utilisateur que vous avez ajoutés à votre compte du Centre de développement dans la section **Gérer les utilisateurs**. Notez que les modifications apportées au nom de l’utilisateur ou à son appartenance à un groupe ne seront pas seulement répercutées dans votre compte du Centre de développement, mais également dans l’annuaire de votre organisation. Les modifications apportées au rôle d’un utilisateur affecteront uniquement son accès au Centre de développement.

**Modifier un compte d’utilisateur**

1.  À partir de la page **Gérer les utilisateurs**, cliquez sur le nom du compte d’utilisateur que vous souhaitez modifier.
2.  Apportez une ou plusieurs des modifications suivantes :
    -   Modifiez le prénom, le nom ou le nom d’utilisateur. N’oubliez pas que ces modifications sont répercutées dans l’annuaire de votre organisation.
    -   Dans la section **Rôles**, sélectionnez ou désélectionnez les rôles que vous souhaitez ajouter ou supprimer pour cet utilisateur, ou attribuez des autorisations personnalisées.
    -   Dans la section **Appartenance au groupe**, sélectionnez ou désélectionnez les groupes auxquels l’utilisateur doit être joint ou dont il doit être supprimé. N’oubliez pas que ces modifications seront répercutées dans l’annuaire de votre organisation.

3.  Cliquez sur **Enregistrer**.

Si vous devez modifier le mot de passe d’un compte d’utilisateur que vous avez ajouté à votre compte du Centre de développement, vous pouvez utiliser la section **Gérer les utilisateurs**. Notez que cela modifiera non seulement le mot de passe pour l’accès au Centre de développement, mais également le mot de passe d’annuaire de l’utilisateur.

**Modification du mot de passe d’annuaire d’un utilisateur**

Si vous avez fourni un **e-mail de récupération du mot de passe** lors de la création du compte d’utilisateur, il peut réinitialiser son propre mot de passe. Vous pouvez également mettre à jour un mot de passe d’utilisateur en suivant les étapes ci-dessous.

1.  À partir de la page **Gérer les utilisateurs**, cliquez sur le nom du compte d’utilisateur que vous souhaitez modifier.
2.  Cliquez sur le bouton **Réinitialiser le mot de passe** en bas de la page.
3.  Une page de confirmation affiche les informations de connexion de l’utilisateur, y compris un mot de passe temporaire.

   > **Important**  Veillez à imprimer ou copier ces informations et à les fournir à l’utilisateur, car vous n’aurez plus accès au mot de passe temporaire après avoir quitté cette page.

## <a name="add-and-manage-groups"></a>Ajouter et gérer des groupes

Lorsque vous ajoutez un groupe à votre compte du Centre de développement partir de l’annuaire de votre organisation, tous les utilisateurs membres de ce groupe peuvent y accéder avec les autorisations associées au rôle attribué à ce groupe. Gardez à l’esprit que toutes les modifications apportées aux groupes (notamment à leur nom ou leur appartenance) seront répercutées dans l’annuaire de votre organisation.

Notez que lorsque vous ajoutez plusieurs groupes simultanément, vous devez leur attribuer le même rôle. Si vous souhaitez ajouter des groupes et leur attribuer des rôles différents, répétez les étapes suivantes pour chaque rôle.

**Ajouter des groupes à partir de l’annuaire de votre organisation**

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des groupes**.
2.  Sélectionnez un ou plusieurs groupes dans la liste qui s’affiche. Vous pouvez utiliser la zone de recherche pour rechercher des groupes spécifiques.
3.  Une fois les groupes sélectionnés, cliquez sur **Ajouter la sélection**.
4.  Dans la section **Rôles**, sélectionnez un ou plusieurs rôles à attribuer à cet ensemble de groupes ou attribuez des autorisations personnalisées.
5.  Cliquez sur **Enregistrer**.

Si vous souhaitez accorder à un nouveau groupe l’accès au Centre de développement, vous pouvez créer un groupe dans la section **Gérer les utilisateurs**. Notez que le groupe sera créé non seulement dans votre compte du Centre de développement, mais également dans l’annuaire de votre organisation.

**Créer un compte de groupe**

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des groupes**.
2.  Sur la page suivante, cliquez sur **Nouveau groupe**.
3.  Entrez le nom d’affichage du nouveau groupe.
4.  Sélectionnez un ou plusieurs rôles à attribuer au nouveau groupe ou attribuez des autorisations personnalisées. Tous les membres du groupe pourront accéder à votre compte du Centre de développement avec les autorisations associées à ce rôle.
5.  Sélectionnez un ou plusieurs utilisateurs dans la liste qui s’affiche. Vous pouvez utiliser la zone de recherche pour rechercher des utilisateurs spécifiques.
6.  Une fois les utilisateurs sélectionnés, cliquez sur **Ajouter la sélection**.
7.  Cliquez sur **Enregistrer**.

Vous pouvez modifier des comptes de groupe que vous avez ajoutés à votre compte du Centre de développement dans la section **Gérer les utilisateurs**. Notez que les modifications apportées au nom du groupe ou à l’appartenance au groupe seront non seulement répercutées dans votre compte du Centre de développement, mais également dans l’annuaire de votre organisation. Les modifications apportées au rôle d’un groupe affecteront uniquement son accès au Centre de développement.

**Modifier un compte de groupe**

1.  À partir de la page **Gérer les utilisateurs**, cliquez sur le nom du compte de groupe que vous souhaitez modifier.
2.  Pour modifier les informations du groupe, apportez les modifications souhaitées au nom du groupe. N’oubliez pas que ces modifications sont répercutées dans l’annuaire de votre organisation.
3.  Pour modifier le rôle du groupe, sélectionnez ou désélectionnez les rôles que vous souhaitez appliquer au groupe, ou attribuez des autorisations personnalisées.
4.  Cliquez sur **Enregistrer**.

## <a name="add-and-manage-azure-ad-applications"></a>Ajouter et gérer des applications Azure AD

Vous pouvez autoriser des applications ou des services faisant partie intégrante du service Azure AD de votre organisation à accéder à votre compte du Centre de développement.

Notez que lorsque vous ajoutez plusieurs applications Azure AD simultanément, vous devez leur attribuer le même rôle. Si vous souhaitez ajouter des groupes et leur attribuer des rôles différents, répétez les étapes suivantes pour chaque rôle.

**Ajouter des applications Azure AD à partir de l’annuaire de votre organisation**

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des applications Azure AD**.
2.  Sélectionnez une ou plusieurs applications Azure AD dans la liste qui s’affiche. Vous pouvez utiliser la zone de recherche pour rechercher des applications Azure AD spécifiques.
3.  Une fois les applications Azure AD sélectionnées, cliquez sur **Ajouter la sélection**.
4.  Dans la section **Rôles**, sélectionnez un ou plusieurs rôles à attribuer à cet ensemble d’applications Azure AD ou attribuez des autorisations personnalisées.
5.  Cliquez sur **Enregistrer**.

Si vous souhaitez accorder à un nouveau compte d’application Azure AD l’accès au Centre de développement, vous pouvez créer ce compte dans la section **Gérer les utilisateurs**. Notez que le compte sera créé non seulement dans votre compte du Centre de développement, mais également dans l’annuaire de votre organisation.

> **Conseil** Si vous utilisez principalement cette application Azure AD pour vous authentifier auprès du Centre de développement et que vous n’avez pas besoin de permettre à des utilisateurs d’y accéder directement, vous pouvez saisir une adresse valide dans les champs **URL de réponse** et **URI ID d’application** à condition que ces valeurs ne soient pas utilisées par une autre application Azure AD de votre répertoire.

**Créer une application Azure AD**

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des applications Azure AD**.
2.  Sur la page suivante, cliquez sur **Nouvelle application Azure AD**.
3.  Renseignez le champ **URL de réponse** pour la nouvelle application Azure AD. Il s’agit de l’URL qui permet aux utilisateurs de se connecter et d’utiliser votre application Azure AD (parfois également désignée sous le terme d’URL de l’application ou d’URL de connexion). L’**URL de réponse** ne peut pas comporter plus de 256 caractères.
4.  Renseignez le champ **URI ID d’application** pour la nouvelle application Azure AD. Il s’agit d’un identificateur logique pour l’application Azure AD qui est présenté lors de l’envoi d’une demande d’authentification unique à Azure AD. Notez que l’**URI ID d’application** doit être unique pour chaque application Azure AD de votre annuaire et ne doit pas comporter plus de 256 caractères.
5.  Dans la section **Rôles**, sélectionnez un ou plusieurs rôles à attribuer à la nouvelle application Azure AD ou attribuez des autorisations personnalisées.
6.  Cliquez sur **Enregistrer**.

Après avoir ajouté ou créé une application Azure AD, revenez à la section **Gérer les utilisateurs**, puis cliquez sur le nom de l’application pour revoir les paramètres de l’application, y compris l’ID de locataire, l’ID de client, l’URL de réponse et l’URI d’ID d’application.

> **Remarque** Si vous envisagez d’utiliser les API REST fournies par les [services du Windows Store](../monetize/using-windows-store-services.md), vous aurez besoin des valeurs ID de locataire et ID de client indiquées sur cette page pour obtenir un jeton d’accès AzureAD que vous pourrez utiliser pour authentifier les appels aux services.   

Vous pouvez modifier les applications Azure AD que vous avez ajoutées à votre compte du Centre de développement dans la section **Gérer les utilisateurs**. Notez que les modifications apportées à l’URL de réponse et à l’URI d’ID d’application seront non seulement répercutées dans votre compte du Centre de développement, mais également dans l’annuaire de votre organisation. Les modifications de rôle affecteront uniquement les autorisations de l’application Azure AD dans le Centre de développement.

**Modifier une application Azure AD**

1.  Sur la page **Gérer les utilisateurs**, cliquez sur le nom du compte d’application Azure AD que vous souhaitez modifier.
2.  Pour modifier la valeur du champ **URL de réponse** ou **URI ID d’application**, entrez les nouvelles valeurs à cet emplacement. N’oubliez pas que ces modifications sont répercutées dans l’annuaire de votre organisation.
3.  Pour modifier le rôle de l’application Azure AD, sélectionnez ou désélectionnez les rôles que vous souhaitez appliquer, ou attribuez des autorisations personnalisées.
4.  Cliquez sur **Enregistrer**.

Si votre application Azure AD lit et écrit des données dans Microsoft Azure AD, elle doit disposer d’une clé. Vous pouvez créer des clés pour une application Azure AD en modifiant les informations de cette dernière dans le Centre de développement. Vous pouvez également supprimer les clés qui ne sont plus nécessaires.

**Gérer les clés pour une application Azure AD**

1.  Sur la page **Gérer les utilisateurs**, cliquez sur le nom de l’application Azure AD.

    > **Conseil**  Lorsque vous cliquez sur le nom de l’application Azure AD, vous voyez toutes les clés actives pour cette application, y compris les dates de création et d’arrivée à expiration des clés. Pour supprimer une clé devenue inutile, cliquez sur **Supprimer**.

2.  Pour ajouter une nouvelle clé, cliquez sur **Ajouter une clé**.

3.  Un écran vous présente les valeurs **ID client** et **Clé**.

    > **Important**  Veillez à imprimer ou copier ces informations, car vous n’y aurez plus accès après avoir quitté cette page.

4.  Si vous souhaitez créer d’autres clés, cliquez sur **Ajouter une autre clé**.

## <a name="view-history-for-account-users"></a>Afficher l’historique des utilisateurs de compte

En tant que propriétaire de compte, vous pouvez afficher l’historique détaillé de navigation des utilisateurs supplémentaires ajoutés au compte.

Sur la page **Gérer les utilisateurs**, cliquez sur le lien présent sous **Dernière activité** pour l’utilisateur dont vous souhaitez passer en revue l’historique de navigation. Vous serez en mesure d’afficher les URL de toutes les pages auxquelles a accédé l’utilisateur au cours des 30derniers jours.

## <a name="removing-users-groups-and-azure-ad-applications"></a>Supprimer des utilisateurs, des groupes et des applications Azure AD

Pour supprimer un utilisateur, un groupe ou une application Azure AD de votre compte du Centre de développement, cliquez sur le lien **Supprimer** en regard de leur nom sur la page **Gérer les utilisateurs**. Une fois la suppression du compte confirmée, l’utilisateur, le groupe ou l’application Azure AD ne pourront plus accéder à votre compte du Centre de développement (sauf si vous l’ajoutez de nouveau ultérieurement).

> **Remarque**  Lorsque vous supprimez un utilisateur, un groupe ou une application Azure AD, ces derniers n’ont plus accès à votre compte du Centre de développement. L’utilisateur, le groupe ou l’application Azure AD n’est pas supprimé de l’annuaire de votre organisation.

 

 

 
