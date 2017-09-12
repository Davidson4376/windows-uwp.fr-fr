---
author: jnHs
Description: "Vous pouvez ajouter des utilisateurs, des groupes et des applications AzureAD à votre compte du Centre de développement."
title: "Ajouter des utilisateurs, des groupes et des applications AzureAD à votre compte du Centre de développement"
ms.author: wdg-dev-content
ms.date: 07/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 32f62d6022d075e71ce6bcc15e1603a2e11a68a5
ms.sourcegitcommit: 23cda44f10059bcaef38ae73fd1d7c8b8330c95e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/19/2017
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-dev-center-account"></a>Ajouter des utilisateurs, des groupes et des applications AzureAD à votre compte du Centre de développement

La section [Gérer les utilisateurs](manage-account-users.md) du Centre de développement vous permet d’utiliser Azure Active Directory pour ajouter des utilisateurs à votre compte du Centre de développement. Chaque utilisateur reçoit un rôle (ou un ensemble d’autorisations personnalisées) qui définit son accès au compte. Vous pouvez également ajouter des [groupes d’utilisateurs](#groups) et des [applications AzureAD](#azure-ad-applications) pour leur accorder un accès à votre compte du Centre de développement.

Après avoir ajouté des utilisateurs au compte, vous pouvez [modifier les détails du compte](#edit), [changer les rôles et les autorisations](set-custom-permissions-for-account-users.md) ou [supprimer des utilisateurs](#remove).

> [!IMPORTANT]
> Pour ajouter des utilisateurs à votre compte, vous devez commencer par [associer votre compte du Centre de développement au locataire Azure Active Directory de votre organisation](associate-azure-ad-with-dev-center.md). 

Lorsque vous ajoutez des utilisateurs, gardez à l’esprit les considérations ci-après. (Ces dernières s’appliquent aussi bien aux groupes et aux applications AzureAD qu’aux utilisateurs individuels.)

-   Tous les utilisateurs du Centre de développement doivent disposer d’un compte actif dans l’annuaire de votre organisation. La création d’un utilisateur dans le Centre de développement crée également un compte pour cet utilisateur dans le locataire AzureAD de votre organisation.
-   La [modification](#edit) du nom d’un utilisateur dans le Centre de développement est répercutée dans le locataire AzureAD de votre organisation.
-   Lorsque vous ajoutez des utilisateurs, vous devez spécifier l’accès dont ils disposent au compte du Centre de développement en leur attribuant un [rôle ou ensemble d’autorisations personnalisées](set-custom-permissions-for-account-users.md). 

> [!NOTE]
> Si votre organisation utilise [l’intégration d’annuaire](http://go.microsoft.com/fwlink/p/?LinkID=724033) pour synchroniser le service d’annuaire local avec votre annuaire AzureAD, vous ne pourrez pas créer d’utilisateurs, de groupes ou d’applications AzureAD dans le Centre de développement. Vous (ou un autre administrateur de votre annuaire local) devrez créer ces éléments directement dans l’annuaire local avant de pouvoir les afficher et les ajouter dans le Centre de développement.


<span id="users" />
## <a name="add-users-to-your-dev-center-account"></a>Ajouter des utilisateurs à votre compte du Centre de développement

Vous pouvez ajouter des utilisateurs individuels à votre compte du Centre de développement en procédant de l’une des manières suivantes:
-   Ajouter des utilisateurs déjà présents dans l’annuaire de votre organisation
-   Créer un utilisateur pour l’ajouter à la fois à l’annuaire de votre organisation et à votre compte du Centre de développement
-   Ajouter des utilisateurs existants ne figurant pas dans l’annuaire de votre organisation


<span id="from-directory" />
### <a name="add-users-from-your-organizations-directory"></a>Ajouter des utilisateurs à partir de l’annuaire de votre organisation

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des utilisateurs**.
2.  Sélectionnez un ou plusieurs utilisateurs dans la liste qui s’affiche. Vous pouvez utiliser la zone de recherche pour rechercher des utilisateurs spécifiques.
    > [!TIP]
    > Si vous sélectionnez plusieurs utilisateurs à ajouter à votre compte du Centre de développement, vous devez leur attribuer le même rôle ou ensemble d’autorisations personnalisées. Si vous souhaitez ajouter plusieurs utilisateurs dotés de rôles ou d’autorisations distincts, répétez les étapes ci-après pour chaque rôle ou ensemble d’autorisations personnalisées.

3.  Une fois les utilisateurs sélectionnés, cliquez sur **Ajouter la sélection**.
4.  Dans la section **Rôles**, spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués aux utilisateurs sélectionnés.
5.  Cliquez sur **Enregistrer**.


<span id="new-user" />
### <a name="create-a-new-user-account-in-your-organizations-directory-and-add-them-to-your-dev-center-account"></a>Créer un compte d’utilisateur dans l’annuaire de votre organisation et l’ajouter à votre compte du Centre de développement

Si vous souhaitez accorder à un nouveau compte de votre locataire AzureAD l’accès au Centre de développement, vous pouvez créer ce compte dans la section **Gérer les utilisateurs** en cliquant sur **Nouvel utilisateur**. 

> [!IMPORTANT]
> Pour être en mesure d’ajouter de nouveaux utilisateurs à l’annuaire, vous devez être connecté avec un compte d’administrateur général dans votre locataire AzureAD.

Pour que le nouvel utilisateur dispose d’un [compte d’administrateur général](http://go.microsoft.com/fwlink/p/?LinkId=746654) dans l’annuaire de votre organisation, cochez la case **Faire de cet utilisateur un administrateur global dans AzureAD, avec contrôle complet de toutes les ressources de l’annuaire**. Ainsi, l’utilisateur a un accès complet à toutes les fonctionnalités administratives de votre annuaire Azure AD. Il peut ajouter et gérer des utilisateurs dans l’annuaire de votre organisation (mais non dans le Centre de développement, sauf si vous octroyez[le rôle/les autorisations](set-custom-permissions-for-account-users.md) appropriés au compte). Si vous cochez cette case, vous devrez fournir un **e-mail de récupération de mot de passe** pour l’utilisateur.

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des utilisateurs**.
2.  Sur la page suivante, cliquez sur **Nouvel utilisateur**.
3.  Assurez-vous que la case d’option **Ajouter à AzureAD** est sélectionnée pour créer un compte dans l’annuaire de votre organisation et ajouter cet utilisateur à votre compte du Centre de développement. 
4.  Entrez le prénom, le nom et le nom d’utilisateur du nouvel utilisateur.
5.  Entrez une adresse e-mail que l’utilisateur peut utiliser s’il a besoin de récupérer son mot de passe. Cette opération n’est requise que si vous avez coché la case **Faire de cet utilisateur un administrateur global dans AzureAD**.
6.  Dans la section **Appartenance au groupe**, sélectionnez les groupes auxquels doit appartenir le nouvel utilisateur.
7.  Dans la section **Rôles**, spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués à l’utilisateur.
8.  Cliquez sur **Enregistrer**.
9.  Les informations de connexion du nouvel utilisateur, y compris un mot de passe temporaire, s’affichent sur la page de confirmation. Veillez à noter ces informations et à les fournir au nouvel utilisateur. Vous n’aurez plus accès au mot de passe temporaire après avoir quitté cette page.


<span id="email" />
### <a name="add-a-user-from-outside-of-your-azure-ad-tenant-to-your-dev-center-account-and-your-directory"></a>Ajouter à votre compte du Centre de développement et à votre annuaire un utilisateur extérieur à votre locataire AzureAD

Pour inviter dans votre compte des utilisateurs qui n’appartiennent pas à votre locataire AzureAD, procédez comme suit. Les utilisateurs seront invités par e-mail à joindre leur compte, et un nouveau compte [d’utilisateur invité](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) sera créé pour eux dans votre locataire AzureAD. 

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des utilisateurs**.
2.  Sur la page suivante, cliquez sur **Nouvel utilisateur**.
3.  Sélectionnez la case d’option **Inviter les utilisateurs par e-mail**.
3.  Entrez une ou plusieurs adresses e-mail (jusqu’à dix), séparées par des virgules ou des points-virgules.
4.  Dans la section **Rôles**, spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués à l’utilisateur.
6.  Cliquez sur **Enregistrer**.

Les utilisateurs invités reçoivent un e-mail avec une invitation pour accéder à votre compte du Centre de développement. Chaque utilisateur doit accepter son invitation avant de pouvoir accéder à votre compte.

Si vous devez renvoyer une invitation, recherchez l’utilisateur sur votre page **Gérer les utilisateurs** et sélectionnez son adresse e-mail (ou le texte indiquant **Invitation en attente**) pour modifier le compte. Puis, au bas de la page, cliquez sur **Renvoyer l’invitation**.


### <a name="changing-a-users-directory-password"></a>Modification du mot de passe d’annuaire d’un utilisateur

Si vous devez modifier le mot de passe d’un compte d’utilisateur que vous avez ajouté à votre compte du Centre de développement, vous pouvez utiliser la section **Gérer les utilisateurs**. Notez que cette opération modifiera le mot de passe de l’utilisateur dans votre locataire AzureAD, ainsi que le mot de passe qu’il utilise pour accéder au Centre de développement. 

Si vous avez fourni un **e-mail de récupération du mot de passe** lors de la création du compte d’utilisateur, l’utilisateur pourra réinitialiser son propre mot de passe. Vous pouvez également mettre à jour un mot de passe d’utilisateur en suivant les étapes ci-dessous.

1.  À partir de la page **Gérer les utilisateurs**, cliquez sur le nom du compte d’utilisateur que vous souhaitez modifier.
2.  Cliquez sur le bouton **Réinitialiser le mot de passe** en bas de la page.
3.  Une page de confirmation affiche les informations de connexion de l’utilisateur, y compris un mot de passe temporaire.
    > [!IMPORTANT]
    >  Veillez à imprimer ou copier ces informations et à les fournir à l’utilisateur, car vous n’aurez plus accès au mot de passe temporaire après avoir quitté cette page.

<span id="groups" />
## <a name="add-groups-to-your-dev-center-account"></a>Ajouter des groupes à votre compte du Centre de développement

Vous pouvez ajouter un groupe à votre compte du Centre de développement à partir de l’annuaire de votre organisation. Lorsque vous effectuez cette opération, chaque utilisateur qui est membre de ce groupe sera en mesure d’accéder à votre compte, avec les autorisations associées au rôle attribué au groupe.

### <a name="add-groups-from-your-organizations-directory"></a>Ajouter des groupes à partir de l’annuaire de votre organisation

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des groupes**.
2.  Sélectionnez un ou plusieurs groupes dans la liste qui s’affiche. Vous pouvez utiliser la zone de recherche pour rechercher des groupes spécifiques.
    > [!TIP]
    > Si vous sélectionnez plusieurs groupes à ajouter à votre compte du Centre de développement, vous devez leur attribuer le même rôle ou ensemble d’autorisations personnalisées. Si vous souhaitez ajouter plusieurs groupes dotés de rôles ou d’autorisations distincts, répétez les étapes ci-après pour chaque rôle ou ensemble d’autorisations personnalisées.

3.  Une fois les groupes sélectionnés, cliquez sur **Ajouter la sélection**.
4.  Dans la section **Rôles**, spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués aux groupes sélectionnés. Tous les membres du groupe seront en mesure d’accéder à votre compte du Centre de développement avec les autorisations que vous appliquez au groupe, quels que soient les rôles/autorisations associés à leur compte individuel.
5.  Cliquez sur **Enregistrer**.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-dev-center-account"></a>Créer un compte de groupe dans l’annuaire de votre organisation et l’ajouter à votre compte du Centre de développement

Si vous souhaitez accorder à un nouveau groupe l’accès au Centre de développement, vous pouvez créer un groupe dans la section **Gérer les utilisateurs**. Notez que le groupe sera créé non seulement dans votre compte du Centre de développement, mais également dans l’annuaire de votre organisation.

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des groupes**.
2.  Sur la page suivante, cliquez sur **Nouveau groupe**.
3.  Entrez le nom d’affichage du nouveau groupe.
4.  Spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués au groupe. Tous les membres du groupe seront en mesure d’accéder à votre compte du Centre de développement avec les autorisations que vous appliquez au groupe, quels que soient les rôles/autorisations associés à leur compte individuel.
5.  Dans la liste qui s’affiche, sélectionnez les utilisateurs à attribuer au nouveau groupe. Vous pouvez utiliser la zone de recherche pour rechercher des utilisateurs spécifiques.
6.  Une fois les utilisateurs sélectionnés, cliquez sur **Ajouter la sélection** pour les ajouter au nouveau groupe.
7.  Cliquez sur **Enregistrer**.


<span id="azure-ad-applications" />
## <a name="add-azure-ad-applications-to-your-dev-center-account"></a>Ajouter des applications AzureAD à votre compte du Centre de développement

Vous pouvez autoriser des applications ou des services faisant partie intégrante du service AzureAD de votre organisation à accéder à votre compte du Centre de développement.

### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>Ajouter des applications AzureAD à partir de l’annuaire de votre organisation

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des applications Azure AD**.
2.  Sélectionnez une ou plusieurs applications Azure AD dans la liste qui s’affiche. Vous pouvez utiliser la zone de recherche pour rechercher des applications AzureAD spécifiques.
    > [!TIP]
    > Si vous sélectionnez plusieurs applications AzureAD à ajouter à votre compte du Centre de développement, vous devez leur attribuer le même rôle ou ensemble d’autorisations personnalisées. Si vous souhaitez ajouter plusieurs applications AzureAD dotées de rôles ou d’autorisations distincts, répétez les étapes ci-après pour chaque rôle ou ensemble d’autorisations personnalisées.

3.  Une fois les applications AzureAD sélectionnées, cliquez sur **Ajouter la sélection**.
4.  Dans la section **Rôles**, spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués aux applications AzureAD sélectionnées.
5.  Cliquez sur **Enregistrer**.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-dev-center-account"></a>Créer un compte d’application AzureAD dans l’annuaire de votre organisation et l’ajouter à votre compte du Centre de développement

Si vous souhaitez accorder à un nouveau compte d’application Azure AD l’accès au Centre de développement, vous pouvez créer ce compte dans la section **Gérer les utilisateurs**. Notez que le compte sera créé non seulement dans votre compte du Centre de développement, mais également dans l’annuaire de votre organisation.

> [!TIP]
> Si vous utilisez principalement cette application AzureAD pour vous authentifier auprès du Centre de développement et que vous n’avez pas besoin de permettre à des utilisateurs d’y accéder directement, vous pouvez saisir une adresse valide dans les champs **URL de réponse** et **URI ID d’application**, à condition que ces valeurs ne soient pas utilisées par une autre application AzureAD de votre annuaire.

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des applications AzureAD**.
2.  Sur la page suivante, cliquez sur **Nouvelle application Azure AD**.
3.  Renseignez le champ **URL de réponse** pour la nouvelle application Azure AD. Il s’agit de l’URL qui permet aux utilisateurs de se connecter et d’utiliser votre application Azure AD (parfois également désignée sous le terme d’URL de l’application ou d’URL de connexion). L’**URL de réponse** ne peut pas comporter plus de 256 caractères.
4.  Renseignez le champ **URI ID d’application** pour la nouvelle application Azure AD. Il s’agit d’un identificateur logique pour l’application Azure AD qui est présenté lors de l’envoi d’une demande d’authentification unique à Azure AD. Notez que l’**URI ID d’application** doit être unique pour chaque application Azure AD de votre annuaire et ne doit pas comporter plus de 256 caractères.
5.  Dans la section **Rôles**, spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués à l’application AzureAD.
6.  Cliquez sur **Enregistrer**.

Après avoir ajouté ou créé une application Azure AD, revenez à la section **Gérer les utilisateurs**, puis cliquez sur le nom de l’application pour revoir les paramètres de l’application, y compris l’ID de locataire, l’ID de client, l’URL de réponse et l’URI d’ID d’application.

> [!NOTE]
> Si vous envisagez d’utiliser les API REST fournies par les [services du WindowsStore](../monetize/using-windows-store-services.md), vous aurez besoin des valeurs ID de locataire et ID de client indiquées sur cette page pour obtenir un jeton d’accès AzureAD que vous pourrez utiliser pour authentifier les appels aux services.   

<span id="manage-keys" />
### <a name="manage-keys-for-an-azure-ad-application"></a>Gérer les clés pour une application AzureAD

Si votre application AzureAD lit et écrit des données dans Microsoft AzureAD, elle doit disposer d’une clé. Vous pouvez créer des clés pour une application Azure AD en modifiant les informations de cette dernière dans le Centre de développement. Vous pouvez également supprimer les clés qui ne sont plus nécessaires.

1.  Sur la page **Gérer les utilisateurs**, cliquez sur le nom de l’application AzureAD.
    > [!TIP]
    > Lorsque vous cliquez sur le nom de l’application AzureAD, vous voyez toutes les clés actives pour cette application, y compris les dates de création et d’arrivée à expiration des clés. Pour supprimer une clé devenue inutile, cliquez sur **Supprimer**.

2.  Pour ajouter une nouvelle clé, cliquez sur **Ajouter une clé**.
3.  Un écran vous présente les valeurs **ID client** et **Clé**.
    > [!IMPORTANT]
    > Veillez à imprimer ou copier ces informations, car vous n’y aurez plus accès après avoir quitté cette page.

4.  Si vous souhaitez créer d’autres clés, cliquez sur **Ajouter une autre clé**.

<span id="edit" />
## <a name="edit-a-user-group-or-azure-ad-application"></a>Modifier un utilisateur, un groupe ou une application AzureAD

Après avoir ajouté des utilisateurs, des groupes et/ou des applications AzureAD à votre compte du Centre de développement, vous pouvez modifier leurs informations de compte. 

> [!IMPORTANT]
> Les modifications apportées aux rôles ou aux autorisations affecteront uniquement l’accès au Centre de développement. Toutes les autres modifications (telles que le changement du nom d’un utilisateur ou de l’appartenance au groupe d’un utilisateur, ou des valeurs d’URL de réponse et d’URI d’ID d’application pour une application AzureAD) seront répercutées dans le locataire AzureAD de votre organisation, ainsi que dans votre compte du Centre de développement. 

1.  Sur la page **Gérer les utilisateurs**, cliquez sur le nom du compte d’utilisateur, de groupe ou d’application AzureAD que vous souhaitez modifier.
2.  Apportez les modifications souhaitées. Les éléments que vous pouvez modifier sont les suivants:
    -   Dans le cas d’un **utilisateur**, vous pouvez modifier le prénom, le nom ou le nom d’utilisateur. Vous pouvez également sélectionner ou désélectionner des groupes dans la section **Appartenance au groupe** pour mettre à jour l’appartenance au groupe d’un utilisateur.
    -   Dans le cas d’un **groupe**, vous pouvez modifier le nom du groupe. (Pour mettre à jour l’appartenance au groupe, modifiez les utilisateurs que vous souhaitez ajouter ou supprimer au niveau du groupe et apportez des modifications à la section **Appartenance au groupe**.)
    -   Dans le cas d’une **application AzureAD**, vous pouvez renseigner les champs **URL de réponse** ou **URI ID d’application**.
    Souvenez-vous que ces modifications seront effectuées non seulement dans votre compte du Centre de développement, mais également dans l’annuaire de votre organisation.
3.  Pour les modifications associées à l’accès au Centre de développement, sélectionnez ou désélectionnez le ou les rôles que vous souhaitez appliquer, ou sélectionnez **Personnaliser les autorisations** et apportez les modifications souhaitées. Ces modifications affecteront uniquement l’accès au Centre de développement et ne changeront pas les autorisations au sein du locataire AzureAD de votre organisation.
3.  Cliquez sur **Enregistrer**.


## <a name="view-history-for-account-users"></a>Afficher l’historique des utilisateurs du compte

En tant que propriétaire de compte, vous pouvez afficher l’historique détaillé de navigation des utilisateurs supplémentaires ajoutés au compte.

Sur la page **Gérer les utilisateurs**, cliquez sur le lien présent sous **Dernière activité** pour l’utilisateur dont vous souhaitez passer en revue l’historique de navigation. Vous serez en mesure de visualiser les URL de toutes les pages auxquelles a accédé l’utilisateur au cours des 30derniers jours.

<span id="remove" />
## <a name="remove-users-groups-and-azure-ad-applications"></a>Supprimer des utilisateurs, des groupes et des applications AzureAD

Pour supprimer un utilisateur, un groupe ou une application Azure AD de votre compte du Centre de développement, cliquez sur le lien **Supprimer** en regard de leur nom sur la page **Gérer les utilisateurs**. Une fois la suppression du compte confirmée, l’utilisateur, le groupe ou l’application Azure AD ne pourront plus accéder à votre compte du Centre de développement (sauf si vous l’ajoutez de nouveau ultérieurement).

> [!IMPORTANT] 
> Lorsque vous supprimez un utilisateur, un groupe ou une application AzureAD, ces derniers n’ont plus accès à votre compte du Centre de développement. Cette opération ne supprime **pas** l’utilisateur, le groupe ou l’application AzureAD de l’annuaire de votre organisation.

 

