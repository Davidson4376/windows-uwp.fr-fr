---
author: jnHs
Description: You can add users, groups, and Azure AD applications to your Dev Center account.
title: Ajouter des utilisateurs, des groupes et des applications AzureAD à votre compte du Centre de développement
ms.author: wdg-dev-content
ms.date: 07/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10 uwp, application azure ad, aad, utilisateur, groupe, plusieurs utilisateurs, multi-utilisateur
ms.localizationpriority: medium
ms.openlocfilehash: 97502a0a2863ed6f7ab2ce5d842fbebc1ae8091c
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4361144"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-dev-center-account"></a>Ajouter des utilisateurs, des groupes et des applications AzureAD à votre compte du Centre de développement

La section **des utilisateurs** du centre de développement Windows (sous **paramètres de compte**) vous permet d’utiliser Azure Active Directory pour ajouter des utilisateurs à votre compte du centre de développement. Chaque utilisateur reçoit un rôle (ou un ensemble d’autorisations personnalisées) qui définit son accès au compte. Vous pouvez également ajouter des [groupes d’utilisateurs](#groups) et des [applications AzureAD](#azure-ad-applications) pour leur accorder un accès à votre compte du Centre de développement.

Après avoir ajouté des utilisateurs au compte, vous pouvez [modifier les détails du compte](#edit), [changer les rôles et les autorisations](set-custom-permissions-for-account-users.md) ou [supprimer des utilisateurs](#remove).

> [!IMPORTANT]
> Pour ajouter des utilisateurs à votre compte, vous devez commencer par [associer votre compte du Centre de développement au client Azure Active Directory de votre organisation](associate-azure-ad-with-dev-center.md). 

Lorsque vous ajoutez des utilisateurs, vous devez spécifier l’accès dont ils disposent à votre compte du Centre de développement en leur attribuant un [rôle ou ensemble d’autorisations personnalisées](set-custom-permissions-for-account-users.md). 

N’oubliez pas que tous les utilisateurs du Centre de développement (y compris les groupes et les applications AzureAD) doivent disposer d’un compte actif dans [un client AzureAD associé à votre compte du Centre de développement](associate-azure-ad-with-dev-center.md). La gestion des utilisateurs s’effectue dans un seul client à la fois. Vous devez vous connecter avec un compte de gestionnaire pour le client dans lequel vous souhaitez ajouter ou modifier des utilisateurs. La création d’un utilisateur dans le Centre de développement crée également un compte pour cet utilisateur dans le client AzureAD auquel vous êtes connecté. Toute modification apportée au nom d’un utilisateur dans le Centre de développement sera répercutée dans le client AzureAD de votre organisation.

> [!NOTE]
> Si votre organisation utilise [l’intégration d’annuaire](http://go.microsoft.com/fwlink/p/?LinkID=724033) pour synchroniser le service d’annuaire local avec votre annuaire AzureAD, vous ne pourrez pas créer d’utilisateurs, de groupes ou d’applications AzureAD dans le Centre de développement. Vous (ou un autre administrateur de votre annuaire local) devrez créer ces éléments directement dans l’annuaire local avant de pouvoir les afficher et les ajouter dans le Centre de développement.


<span id="users" />

## <a name="add-users-to-your-dev-center-account"></a>Ajouter des utilisateurs à votre compte du Centre de développement

Pour ajouter des utilisateurs à votre compte du Centre de développement, accédez à la page **Utilisateurs** dans **Paramètres du compte** et sélectionnez **Ajouter des utilisateurs.** Vous devez être connecté avec un compte de gestionnaire pour le client AzureAD que vous souhaitez utiliser. 

### <a name="add-existing-users"></a>Ajouter des utilisateurs existants 

Vous pouvez sélectionner les utilisateurs qui déjà existent déjà dans le client de votre organisation et leur donner accès à votre compte du Centre de développement. 

<span id="from-directory" />

1.  Sélectionnez l’icône d’engrenage (près de l’angle supérieur droit du tableau de bord), puis sélectionnez **les paramètres de compte**. Dans le menu **paramètres** , sélectionnez **les utilisateurs**.
2.  Dans la page **Utilisateurs**, sélectionnez **Ajouter des utilisateurs**. 
3.  Sélectionnez un ou plusieurs utilisateurs dans la liste qui s’affiche. Vous pouvez utiliser la zone de recherche pour rechercher des utilisateurs spécifiques.
    > [!TIP]
    > Si vous sélectionnez plusieurs utilisateurs à ajouter à votre compte du Centre de développement, vous devez leur attribuer le même rôle ou ensemble d’autorisations personnalisées. Si vous souhaitez ajouter plusieurs utilisateurs dotés de rôles ou d’autorisations distincts, répétez les étapes ci-après pour chaque rôle ou ensemble d’autorisations personnalisées.
4.  Une fois les utilisateurs sélectionnés, cliquez sur **Ajouter la sélection**.
5.  Dans la section **Rôles**, spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués aux utilisateurs sélectionnés.
6.  Cliquez sur **Enregistrer**.

### <a name="additional-methods-for-adding-users"></a>Méthodes supplémentaires d’ajout d’utilisateurs

Si vous êtes connecté avec un compte de gestionnaire qui dispose également des [autorisations d’administrateur global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) pour le client AzureAD sur lequel vous travaillez, vous disposerez d’options supplémentaires pour ajouter des utilisateurs à votre compte du Centre de développement. Vous devrez sélectionner l’une des options suivantes:

-   **Ajouter des utilisateurs existants**: choisissez les utilisateurs qui existent déjà dans l’annuaire de votre organisation et leur donnent accès à votre compte du centre de développement, à l’aide de la méthode décrite ci-dessus.
-   **Créer de nouveaux utilisateurs**: créer tout nouveau comptes utilisateurs ajoutés à l’annuaire de votre organisation et votre compte du centre de développement
-   **Inviter des utilisateurs extérieurs**: envoyez une invitation par e-mail aux utilisateurs qui ne figurent pas encore dans l’annuaire de votre organisation. Ils seront invités à accéder à votre compte du Centre de développement, et un nouveau compte [d'utilisateur invité](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) sera créé pour eux dans votre client AzureAD.

<span id="new-user" />

### <a name="create-new-users"></a>Créer de nouveaux utilisateurs

> [!IMPORTANT]
> Pour être en mesure de créer de nouveaux utilisateurs, vous devez être connecté avec un compte administrateur général dans votre client AzureAD.

1.  Dans la page **utilisateurs** (sous **paramètres de compte**), sélectionnez **Ajouter des utilisateurs**, puis choisissez **créer de nouveaux utilisateurs**.
2.  Entrez le prénom, le nom et le nom d’utilisateur du nouvel utilisateur.
3.  Pour que le nouvel utilisateur dispose d’un [compte d’administrateur général](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) dans l’annuaire de votre organisation, cochez la case **Faire de cet utilisateur un administrateur global dans AzureAD, avec contrôle complet de toutes les ressources de l’annuaire**. Ainsi, l’utilisateur a un accès complet à toutes les fonctionnalités administratives de votre annuaire Azure AD. Il peut ajouter et gérer des utilisateurs dans l’annuaire de votre organisation (mais non dans le Centre de développement, sauf si vous octroyez[le rôle/les autorisations](set-custom-permissions-for-account-users.md) appropriés au compte). Si vous cochez cette case, vous devrez fournir un **e-mail de récupération de mot de passe** pour l’utilisateur.
4.  Si vous avez coché la case pour **Faire de cet utilisateur un administrateur global dans AzureAD**, entrez une adresse e-mail que l’utilisateur peut utiliser s’ils souhaitent récupérer son mot de passe.
5.  Dans la section **Appartenance au groupe**, sélectionnez les groupes auxquels doit appartenir le nouvel utilisateur.
6.  Dans la section **Rôles**, spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués à l’utilisateur.
7.  Cliquez sur **Enregistrer**.
8.  Les informations de connexion du nouvel utilisateur, y compris un mot de passe temporaire, s’affichent sur la page de confirmation. Veillez à noter ces informations et à les fournir au nouvel utilisateur. Vous n’aurez plus accès au mot de passe temporaire après avoir quitté cette page.


<span id="email" />

### <a name="invite-outside-users"></a>Inviter des utilisateurs extérieurs

> [!IMPORTANT]
> Pour être en mesure d’inviter des utilisateurs extérieurs, vous devez être connecté avec un compte administrateur général dans votre client AzureAD.

1.  Dans la page **utilisateurs** (sous **paramètres de compte**), sélectionnez **Ajouter des utilisateurs**, puis choisissez **Inviter des utilisateurs par e-mail**.
1.  Entrez une ou plusieurs adresses e-mail (jusqu’à dix), séparées par des virgules ou des points-virgules.
2.  Dans la section **Rôles**, spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués à l’utilisateur.
3.  Cliquez sur **Enregistrer**.

Les utilisateurs que vous avez invités recevront une invitation par e-mail à rejoindre votre compte, et un nouveau compte [d’utilisateur invité](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) sera créé pour eux dans votre client AzureAD. Chaque utilisateur doit accepter son invitation avant de pouvoir accéder à votre compte.

Si vous devez renvoyer une invitation, recherchez l’utilisateur sur votre page **Utilisateurs** et sélectionnez son adresse e-mail (ou le texte indiquant **Invitation en attente**). Puis, au bas de la page, cliquez sur **Renvoyer l’invitation**.

> [!IMPORTANT]
> Les utilisateurs extérieurs que vous invitez à rejoindre votre compte du Centre de développement peuvent être affectés aux mêmes rôles et autorisations que les autres utilisateurs. Toutefois, les utilisateurs extérieurs ne pourront pas effectuer certaines tâches dans VisualStudio, telles que l’association d’une application avec le Store ou la création de packages à charger dans le Store. Si un utilisateur a besoin d’effectuer ces tâches, choisissez **Créer de nouveaux utilisateurs** au lieu de **Inviter des utilisateurs extérieurs**. (Si vous ne souhaitez pas ajouter ces utilisateurs à votre client AzureAD existant, vous pouvez [créer un nouveau client](../publish/associate-azure-ad-with-dev-center.md#create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account), puis créer de nouveaux comptes d’utilisateur eux dans ce client.) 


### <a name="changing-a-users-directory-password"></a>Modification du mot de passe d’annuaire d’un utilisateur

Si l’un de vos utilisateurs a besoin de modifier son mot de passe, il peut le faire lui-même si vous avez indiqué un **e-mail de récupération de mot de passe** lors de la création du compte d’utilisateur. Vous pouvez également mettre à jour le mot de passe d’un utilisateur en suivant les étapes ci-dessous (si vous êtes connecté avec un compte administrateur général dans votre client AzureAD afin de modifier le mot de passe d’un utilisateur). Notez que cette opération modifiera le mot de passe de l’utilisateur dans votre client AzureAD, ainsi que le mot de passe qu’il utilise pour accéder au Centre de développement. 

1.  Dans la page **utilisateurs** (sous **paramètres de compte**), sélectionnez le nom du compte d’utilisateur que vous souhaitez modifier.
2.  Sélectionnez le bouton **Réinitialiser le mot de passe** en bas de la page.
3.  Une page de confirmation affiche les informations de connexion de l’utilisateur, y compris un mot de passe temporaire.

    > [!IMPORTANT]
    >  Veillez à imprimer ou copier ces informations et à les fournir à l’utilisateur, car vous n’aurez plus accès au mot de passe temporaire après avoir quitté cette page.

<span id="groups" />

## <a name="add-groups-to-your-dev-center-account"></a>Ajouter des groupes à votre compte du Centre de développement

Vous pouvez ajouter un groupe à votre compte du Centre de développement à partir de l’annuaire de votre organisation. Lorsque vous effectuez cette opération, chaque utilisateur qui est membre de ce groupe sera en mesure d’accéder à votre compte, avec les autorisations associées au rôle attribué au groupe.

### <a name="add-groups-from-your-organizations-directory"></a>Ajouter des groupes à partir de l’annuaire de votre organisation

1.  Sélectionnez l’icône d’engrenage (près de l’angle supérieur droit du tableau de bord), puis sélectionnez **les paramètres de compte**. Dans le menu **paramètres** , sélectionnez **les utilisateurs**.
2. Dans la page **utilisateurs** , sélectionnez **Ajouter des groupes**.
2.  Sélectionnez un ou plusieurs groupes dans la liste qui s’affiche. Vous pouvez utiliser la zone de recherche pour rechercher des groupes spécifiques.
    > [!TIP]
    > Si vous sélectionnez plusieurs groupes à ajouter à votre compte du Centre de développement, vous devez leur attribuer le même rôle ou ensemble d’autorisations personnalisées. Si vous souhaitez ajouter plusieurs groupes dotés de rôles ou d’autorisations distincts, répétez les étapes ci-après pour chaque rôle ou ensemble d’autorisations personnalisées.

3.  Une fois les groupes sélectionnés, cliquez sur **Ajouter la sélection**.
4.  Dans la section **Rôles**, spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués aux groupes sélectionnés. Tous les membres du groupe seront en mesure d’accéder à votre compte du Centre de développement avec les autorisations que vous appliquez au groupe, quels que soient les rôles/autorisations associés à leur compte individuel.
5.  Cliquez sur **Enregistrer**.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-dev-center-account"></a>Créer un compte de groupe dans l’annuaire de votre organisation et l’ajouter à votre compte du Centre de développement

Si vous souhaitez accorder à un nouveau groupe l’accès au Centre de développement, vous pouvez créer un groupe dans la section **Utilisateurs**. Notez que le groupe sera créé non seulement dans votre compte du Centre de développement, mais également dans l’annuaire de votre organisation.

1.  Dans la page **utilisateurs** (sous **paramètres de compte**), cliquez sur **Ajouter des groupes**.
2.  Sur la page suivante, sélectionnez le **Nouveau groupe**.
3.  Entrez le nom d’affichage du nouveau groupe.
4.  Spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués au groupe. Tous les membres du groupe seront en mesure d’accéder à votre compte du Centre de développement avec les autorisations que vous appliquez au groupe, quels que soient les rôles/autorisations associés à leur compte individuel.
5.  Dans la liste qui s’affiche, sélectionnez les utilisateurs à attribuer au nouveau groupe. Vous pouvez utiliser la zone de recherche pour rechercher des utilisateurs spécifiques.
6.  Une fois les utilisateurs sélectionnés, cliquez sur **Ajouter la sélection** pour les ajouter au nouveau groupe.
7.  Cliquez sur **Enregistrer**.


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-dev-center-account"></a>Ajouter des applications AzureAD à votre compte du Centre de développement

Vous pouvez autoriser des applications ou des services faisant partie intégrante du service AzureAD de votre organisation à accéder à votre compte du Centre de développement. Ces comptes d’utilisateur d’applications AzureAD peuvent être utilisés pour appeler les API REST fournies par les [services MicrosoftStore](../monetize/using-windows-store-services.md).


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>Ajouter des applications AzureAD à partir de l’annuaire de votre organisation

1.  1.  Sélectionnez l’icône d’engrenage (près de l’angle supérieur droit du tableau de bord), puis sélectionnez **les paramètres de compte**. Dans le menu **paramètres** , sélectionnez **les utilisateurs**.
2. Sur la page **Utilisateurs**, sélectionnez **Ajouter des applications AzureAD**.
3.  Sélectionnez une ou plusieurs applications AzureAD dans la liste qui s’affiche. Vous pouvez utiliser la zone de recherche pour rechercher des applications AzureAD spécifiques.
    > [!TIP]
    > Si vous sélectionnez plusieurs applications AzureAD à ajouter à votre compte du Centre de développement, vous devez leur attribuer le même rôle ou ensemble d’autorisations personnalisées. Si vous souhaitez ajouter plusieurs applications AzureAD dotées de rôles ou d’autorisations distincts, répétez les étapes ci-après pour chaque rôle ou ensemble d’autorisations personnalisées.

4.  Une fois les applications AzureAD sélectionnées, cliquez sur **Ajouter la sélection**.
5.  Dans la section **Rôles**, spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués aux applications AzureAD sélectionnées.
6.  Cliquez sur **Enregistrer**.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-dev-center-account"></a>Créer un compte d’application AzureAD dans l’annuaire de votre organisation et l’ajouter à votre compte du Centre de développement

Si vous souhaitez accorder à un nouveau compte d’application AzureAD l’accès au Centre de développement, vous pouvez créer ce compte dans la section **Utilisateurs**. Notez que le compte sera créé non seulement dans votre compte du Centre de développement, mais également dans l’annuaire de votre organisation.

> [!TIP]
> Si vous utilisez principalement cette application AzureAD pour vous authentifier auprès du Centre de développement et que vous n’avez pas besoin de permettre à des utilisateurs d’y accéder directement, vous pouvez saisir une adresse valide dans les champs **URL de réponse** et **URI ID d’application**, à condition que ces valeurs ne soient pas utilisées par une autre application AzureAD de votre annuaire.

1.  Dans la page **utilisateurs** (sous **paramètres de compte**), sélectionnez **Ajouter des applications Azure AD**.
2.  Sur la page suivante, sélectionnez **l’application nouvel Azure AD**.
3.  Renseignez le champ **URL de réponse** pour la nouvelle application Azure AD. Il s’agit de l’URL qui permet aux utilisateurs de se connecter et d’utiliser votre application Azure AD (parfois également désignée sous le terme d’URL de l’application ou d’URL de connexion). L’**URL de réponse** ne peut pas comporter plus de 256caractères et doit être unique dans votre annuaire.
4.  Renseignez le champ **URI ID d’application** pour la nouvelle application AzureAD. Il s’agit d’un identificateur logique pour l’application Azure AD qui est présenté lors de l’envoi d’une demande d’authentification unique à Azure AD. Notez que l’**URI ID d’application** doit être unique pour chaque application Azure AD de votre annuaire et ne doit pas comporter plus de 256 caractères. Pour plus d’informations sur l’**URI ID d’application**, voir [Intégration d’applications à Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant).
5.  Dans la section **Rôles**, spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués à l’application AzureAD.
6.  Cliquez sur **Enregistrer**.

Après avoir ajouté ou créé une application Azure AD, revenez à la section **Utilisateurs**, puis sélectionnez le nom de l’application pour revoir les paramètres de l’application, y compris l’ID de locataire, l’ID de client, l’URL de réponse et l’URI d’ID d’application.

> [!NOTE]
> Si vous envisagez d’utiliser les API REST fournies par les [services du MicrosoftStore](../monetize/using-windows-store-services.md), vous aurez besoin des valeurs ID de locataire et ID de client indiquées sur cette page pour obtenir un jeton d’accès AzureAD que vous pourrez utiliser pour authentifier les appels aux services.   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>Gérer les clés pour une application AzureAD

Si votre application AzureAD lit et écrit des données dans Microsoft AzureAD, elle doit disposer d’une clé. Vous pouvez créer des clés pour une application Azure AD en modifiant les informations de cette dernière dans le Centre de développement. Vous pouvez également supprimer les clés qui ne sont plus nécessaires.

1.  Dans la page **utilisateurs** (sous **paramètres de compte**), sélectionnez le nom de l’application Azure AD.
    > [!TIP]
    > Lorsque vous cliquez sur le nom de l’application AzureAD, vous voyez toutes les clés actives pour cette application, y compris les dates de création et d’arrivée à expiration des clés. Pour supprimer une clé devenue inutile, cliquez sur **Supprimer**.

2.  Pour ajouter une nouvelle clé, sélectionnez **Ajouter une clé**.
3.  Un écran vous présente les valeurs **ID client** et **Clé**.
    > [!IMPORTANT]
    > Veillez à imprimer ou copier ces informations, car vous n’y aurez plus accès après avoir quitté cette page.

4.  Si vous souhaitez créer d’autres clés, sélectionnez **Ajouter une autre clé**.

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>Modifier un utilisateur, un groupe ou une application AzureAD

Après avoir ajouté des utilisateurs, des groupes et/ou des applications AzureAD à votre compte du Centre de développement, vous pouvez modifier leurs informations de compte. 

> [!IMPORTANT]
> Les modifications apportées aux [rôles ou aux autorisations](set-custom-permissions-for-account-users.md) affecteront uniquement l’accès au Centre de développement. Toutes les autres modifications (telles que le changement du nom d’un utilisateur ou de l’appartenance au groupe d’un utilisateur, ou des valeurs d’URL de réponse et d’URI d’ID d’application pour une application AzureAD) seront répercutées dans le locataire AzureAD de votre organisation, ainsi que dans votre compte du Centre de développement. 

1.  Dans la page **utilisateurs** (sous **paramètres de compte**), sélectionnez le nom de l’utilisateur, un groupe ou un compte d’application Azure AD que vous souhaitez modifier.
2.  Apportez les modifications souhaitées. Les éléments que vous pouvez modifier sont les suivants:
    -   Dans le cas d’un **utilisateur**, vous pouvez modifier le prénom, le nom ou le nom d’utilisateur. Vous pouvez également sélectionner ou désélectionner des groupes dans la section **Appartenance au groupe** pour mettre à jour l’appartenance au groupe d’un utilisateur.
    -   Dans le cas d’un **groupe**, vous pouvez modifier le nom du groupe. (Pour mettre à jour l’appartenance au groupe, modifiez les utilisateurs que vous souhaitez ajouter ou supprimer au niveau du groupe et apportez des modifications à la section **Appartenance au groupe**.)
    -   Dans le cas d’une **application AzureAD**, vous pouvez renseigner les champs **URL de réponse** ou **URI ID d’application**.
    Souvenez-vous que ces modifications seront effectuées non seulement dans votre compte du Centre de développement, mais également dans l’annuaire de votre organisation.
3.  Pour les modifications associées à l’accès au Centre de développement, sélectionnez ou désélectionnez le ou les rôles que vous souhaitez appliquer, ou sélectionnez **Personnaliser les autorisations** et apportez les modifications souhaitées. Ces modifications affecteront uniquement l’accès au Centre de développement et ne changeront pas les autorisations au sein du locataire AzureAD de votre organisation.
3.  Cliquez sur **Enregistrer**.


## <a name="view-history-for-account-users"></a>Afficher l’historique des utilisateurs du compte

En tant que propriétaire de compte, vous pouvez afficher l’historique détaillé de navigation des utilisateurs supplémentaires ajoutés au compte.

Sur la page **utilisateurs** (sous **paramètres de compte**), sélectionnez le lien présent sous la **dernière activité** pour l’utilisateur dont l’historique de navigation vous souhaitez passer en revue. Vous serez en mesure de visualiser les URL de toutes les pages auxquelles a accédé l’utilisateur au cours des 30derniers jours.

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>Supprimer des utilisateurs, des groupes et des applications AzureAD

Pour supprimer un utilisateur, groupe ou application Azure AD de votre compte du centre de développement, sélectionnez le lien **Supprimer** qui s’affiche en leur nom sur la page **utilisateurs** . Une fois la suppression du compte confirmée, l’utilisateur, le groupe ou l’application Azure AD ne pourront plus accéder à votre compte du Centre de développement (sauf si vous l’ajoutez de nouveau ultérieurement).

> [!IMPORTANT]
> Lorsque vous supprimez un utilisateur, un groupe ou une application AzureAD, ces derniers n’ont plus accès à votre compte du Centre de développement. Cette opération ne supprime **pas** l’utilisateur, le groupe ou l’application AzureAD de l’annuaire de votre organisation.

 

