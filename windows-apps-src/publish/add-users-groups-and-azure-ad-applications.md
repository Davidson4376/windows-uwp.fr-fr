---
Description: Vous pouvez ajouter des utilisateurs, groupes et applications Azure AD pour votre compte espace partenaires.
title: Ajouter des utilisateurs, groupes et applications Azure AD pour votre compte espace partenaires
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10 uwp, application azure ad, aad, utilisateur, groupe, plusieurs utilisateurs, utilisateurs multiples
ms.localizationpriority: medium
ms.openlocfilehash: ddbe47d94e17db0d272aedcff56df95fccf3434d
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63787291"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>Ajouter des utilisateurs, groupes et applications Azure AD pour votre compte espace partenaires

Le **utilisateurs** section de [partenaires](https://partner.microsoft.com/dashboard) (sous **paramètres du compte**) vous permet d’utiliser Azure Active Directory pour ajouter des utilisateurs à votre compte espace partenaires. Chaque utilisateur reçoit un rôle (ou un ensemble d’autorisations personnalisées) qui définit son accès au compte. Vous pouvez également ajouter [groupes d’utilisateurs](#groups) et [applications Azure AD](#azure-ad-applications) leur accorder l’accès à votre compte espace partenaires.

Après avoir ajouté des utilisateurs au compte, vous pouvez [modifier les détails du compte](#edit), [changer les rôles et les autorisations](set-custom-permissions-for-account-users.md) ou [supprimer des utilisateurs](#remove).

> [!IMPORTANT]
> Pour ajouter des utilisateurs à votre compte, vous devez d’abord [associer votre compte espace partenaires de locataire Azure Active Directory de votre organisation](associate-azure-ad-with-partner-center.md). 

Lorsque vous ajoutez des utilisateurs, vous devez spécifier leur accès à votre compte espace partenaires en leur attribuant un [rôle ou jeu d’autorisations personnalisées](set-custom-permissions-for-account-users.md). 

N’oubliez pas que tous les utilisateurs partenaires (y compris les groupes et applications Azure AD) doivent avoir un compte actif dans [un locataire Azure AD qui est associé à votre compte espace partenaires](associate-azure-ad-with-partner-center.md). La gestion des utilisateurs s’effectue dans un seul client à la fois. Vous devez vous connecter avec un compte de gestionnaire pour le client dans lequel vous souhaitez ajouter ou modifier des utilisateurs. Création d’un utilisateur dans l’espace partenaires est également créer un compte pour cet utilisateur dans le locataire Azure AD auquel vous êtes connecté, et apporter des modifications à un nom d’utilisateur dans l’espace partenaires est apporter les mêmes modifications dans le locataire Azure AD de votre organisation.

> [!NOTE]
> Si votre organisation utilise [intégration d’annuaire](https://go.microsoft.com/fwlink/p/?LinkID=724033) pour synchroniser le service d’annuaire local avec votre compte Azure AD, vous ne pourrez pas créer de nouveaux utilisateurs, des groupes ou des applications Azure AD dans le centre de partenaires. Vous (ou un autre administrateur de votre annuaire local) devez les créer directement dans le répertoire local avant que vous serez en mesure de voir et de les ajouter dans l’espace partenaires.


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>Ajouter des utilisateurs à votre compte espace partenaires

Pour ajouter des utilisateurs à votre compte espace partenaires, accédez à la **utilisateurs** page **paramètres du compte** et sélectionnez **ajouter des utilisateurs.** Vous devez être connecté avec un compte de gestionnaire pour le client Azure AD que vous souhaitez utiliser. 

### <a name="add-existing-users"></a>Ajouter des utilisateurs existants 

Vous pouvez sélectionner les utilisateurs qui déjà existent dans le locataire de votre organisation et leur donnent accès à votre compte espace partenaires. 

<span id="from-directory" />

1.  Sélectionnez l’icône d’engrenage (en haut à droite de partenaires), puis **les paramètres du développeur**. Dans le **paramètres** menu, sélectionnez **utilisateurs**.
2.  Dans la page **Utilisateurs**, sélectionnez **Ajouter des utilisateurs**. 
3.  Sélectionnez un ou plusieurs utilisateurs dans la liste qui s’affiche. Vous pouvez utiliser la zone de recherche pour rechercher des utilisateurs spécifiques.
    > [!TIP]
    > Si vous sélectionnez plusieurs utilisateurs à ajouter à votre compte espace partenaires, vous devez les attribuer le même rôle ou jeu d’autorisations personnalisées. Si vous souhaitez ajouter plusieurs utilisateurs dotés de rôles ou d’autorisations distincts, répétez les étapes ci-après pour chaque rôle ou ensemble d’autorisations personnalisées.
4.  Une fois les utilisateurs sélectionnés, cliquez sur **Ajouter la sélection**.
5.  Dans la section **Rôles**, spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués aux utilisateurs sélectionnés.
6.  Cliquez sur **Enregistrer**.

### <a name="additional-methods-for-adding-users"></a>Méthodes supplémentaires d’ajout d’utilisateurs

Si vous êtes connecté avec un compte de gestionnaire qui a également [administrateur général](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) autorisations pour le locataire Azure AD que vous travaillez dans, vous disposerez des options supplémentaires pour ajouter des utilisateurs à votre compte espace partenaires. Vous devrez sélectionner l’une des options suivantes :

-   **Ajouter des utilisateurs existants**: Choisissez les utilisateurs qui déjà existent dans le répertoire de votre organisation et leur donnent accès à votre compte espace partenaires, à l’aide de la méthode décrite ci-dessus.
-   **Créer de nouveaux utilisateurs**: Créer les tout nouveaux comptes d’utilisateur à ajouter au répertoire de votre organisation à la fois et votre compte espace partenaires
-   **Inviter des utilisateurs extérieurs**: Envoyer des invitations par courrier électronique aux utilisateurs qui ne sont pas actuellement dans le répertoire de votre organisation. Ils seront invités à accéder à votre compte espace partenaires et un nouveau [utilisateur invité](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) compte sera créé pour eux dans votre locataire Azure AD.

<span id="new-user" />

### <a name="create-new-users"></a>Créer de nouveaux utilisateurs

> [!IMPORTANT]
> Pour être en mesure de créer de nouveaux utilisateurs, vous devez être connecté avec un compte administrateur général dans votre client Azure AD.

1.  À partir de la **utilisateurs** page (sous **paramètres du compte**), sélectionnez **ajouter des utilisateurs**, puis choisissez **créer de nouveaux utilisateurs**.
2.  Entrez le prénom, le nom et le nom d’utilisateur du nouvel utilisateur.
3.  Pour que le nouvel utilisateur dispose d’un [compte d’administrateur général](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) dans l’annuaire de votre organisation, cochez la case **Faire de cet utilisateur un administrateur global dans Azure AD, avec contrôle complet de toutes les ressources de l’annuaire**. Ainsi, l’utilisateur a un accès complet à toutes les fonctionnalités administratives de votre annuaire Azure AD. Ils seront en mesure d’ajouter et gérer des utilisateurs dans le répertoire de votre organisation (même si le partenaire non centrer, sauf si vous accordez au compte approprié [/autorisations du rôle](set-custom-permissions-for-account-users.md)). Si vous activez cette case à cocher, vous devez fournir un **e-mail de récupération de mot de passe** pour l’utilisateur.
4.  Si vous avez coché la case pour **Faire de cet utilisateur un administrateur global dans Azure AD**, entrez une adresse e-mail que l’utilisateur peut utiliser s’ils souhaitent récupérer son mot de passe.
5.  Dans la section **Appartenance au groupe**, sélectionnez les groupes auxquels doit appartenir le nouvel utilisateur.
6.  Dans la section **Rôles**, spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués à l’utilisateur.
7.  Cliquez sur **Enregistrer**.
8.  Les informations de connexion du nouvel utilisateur, y compris un mot de passe temporaire, s’affichent sur la page de confirmation. Veillez à noter ces informations et à les fournir au nouvel utilisateur. Vous n’aurez plus accès au mot de passe temporaire après avoir quitté cette page.


<span id="email" />

### <a name="invite-outside-users"></a>Inviter des utilisateurs extérieurs

> [!IMPORTANT]
> Pour être en mesure d’inviter des utilisateurs extérieurs, vous devez être connecté avec un compte administrateur général dans votre client Azure AD.

1.  À partir de la **utilisateurs** page (sous **paramètres du compte**), sélectionnez **ajouter des utilisateurs**, puis choisissez **inviter des utilisateurs par courrier électronique**.
1.  Entrez une ou plusieurs adresses e-mail (jusqu'à dix), séparées par des virgules ou des points-virgules.
2.  Dans la section **Rôles**, spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués à l’utilisateur.
3.  Cliquez sur **Enregistrer**.

Les utilisateurs que vous avez invités recevront une invitation par e-mail à rejoindre votre compte, et un nouveau compte [d’utilisateur invité](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) sera créé pour eux dans votre client Azure AD. Chaque utilisateur doit accepter son invitation avant de pouvoir accéder à votre compte.

Si vous devez renvoyer une invitation, recherchez l’utilisateur sur votre page **Utilisateurs** et sélectionnez son adresse e-mail (ou le texte indiquant **Invitation en attente**). Puis, en bas de la page, cliquez sur **Renvoyer invitation**.

> [!IMPORTANT]
> En dehors des utilisateurs vous inviter à participer à votre compte espace partenaires peut avoir les mêmes rôles et autorisations que les autres utilisateurs. Toutefois, les utilisateurs extérieurs ne pourront pas effectuer certaines tâches dans Visual Studio, telles que l’association d’une application avec le Store ou la création de packages à charger dans le Store. Si un utilisateur a besoin d’effectuer ces tâches, choisissez **Créer de nouveaux utilisateurs** au lieu de **Inviter des utilisateurs extérieurs**. (Si vous ne souhaitez pas ajouter ces utilisateurs à votre client Azure AD existant, vous pouvez [créer un nouveau client](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account), puis créer de nouveaux comptes d’utilisateur eux dans ce client.) 


### <a name="changing-a-users-directory-password"></a>Modification du mot de passe d’annuaire d’un utilisateur

Si l’un de vos utilisateurs a besoin de modifier son mot de passe, il peut le faire lui-même si vous avez indiqué un **e-mail de récupération de mot de passe** lors de la création du compte d’utilisateur. Vous pouvez également mettre à jour le mot de passe d’un utilisateur en suivant les étapes ci-dessous (si vous êtes connecté avec un compte administrateur général dans votre client Azure AD afin de modifier le mot de passe d’un utilisateur). Notez que cela modifiera le mot de passe dans votre locataire Azure AD, ainsi que le mot de passe qu’ils permettent d’accéder aux partenaires. 

1.  À partir de la **utilisateurs** page (sous **paramètres du compte**), sélectionnez le nom du compte d’utilisateur que vous souhaitez modifier.
2.  Sélectionnez le **réinitialisation de mot de passe** bouton en bas de la page.
3.  Une page de confirmation affiche les informations de connexion de l’utilisateur, y compris un mot de passe temporaire.

    > [!IMPORTANT]
    >  N’oubliez pas d’imprimer ou copier ces informations et fournir à l’utilisateur, comme vous ne pourrez pas accéder à un mot de passe temporaire, une fois que vous quittez cette page.

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>Ajouter des groupes à votre compte espace partenaires

Vous pouvez ajouter un groupe à partir du répertoire de votre organisation à votre compte espace partenaires. Lorsque vous effectuez cette opération, chaque utilisateur qui est membre de ce groupe sera en mesure d’accéder à votre compte, avec les autorisations associées au rôle attribué au groupe.

### <a name="add-groups-from-your-organizations-directory"></a>Ajouter des groupes à partir de l’annuaire de votre organisation

1.  Sélectionnez l’icône d’engrenage (en haut à droite de partenaires), puis **les paramètres du développeur**. Dans le **paramètres** menu, sélectionnez **utilisateurs**.
2. À partir de la **utilisateurs** page, sélectionnez **ajouter des groupes**.
2.  Sélectionnez un ou plusieurs groupes dans la liste qui s’affiche. Vous pouvez utiliser la zone de recherche pour rechercher des groupes spécifiques.
    > [!TIP]
    > Si vous sélectionnez plus d’un groupe à ajouter à votre compte espace partenaires, vous devez les attribuer le même rôle ou jeu d’autorisations personnalisées. Si vous souhaitez ajouter plusieurs groupes dotés de rôles ou d’autorisations distincts, répétez les étapes ci-après pour chaque rôle ou ensemble d’autorisations personnalisées.

3.  Une fois les groupes sélectionnés, cliquez sur **Ajouter la sélection**.
4.  Dans la section **Rôles**, spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués aux groupes sélectionnés. Tous les membres du groupe sera en mesure d’accéder à votre compte de partenaires avec les autorisations que vous appliquez au groupe, indépendamment des rôles/autorisations associées à leur compte individuel.
5.  Cliquez sur **Enregistrer**.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Créer un nouveau compte de groupe dans le répertoire de votre organisation et l’ajouter à votre compte espace partenaires

Si vous souhaitez accorder l’accès de partenaires à un tout nouveau groupe, vous pouvez créer un nouveau groupe dans le **utilisateurs** section. Notez que le nouveau groupe est créé dans le répertoire de votre organisation, pas seulement dans votre compte espace partenaires.

1.  À partir de la **utilisateurs** page (sous **les paramètres du développeur**), cliquez sur **ajouter des groupes**.
2.  Sur la page suivante, sélectionnez **nouveau groupe**.
3.  Entrez le nom d’affichage du nouveau groupe.
4.  Spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués au groupe. Tous les membres du groupe sera en mesure d’accéder à votre compte de partenaires avec les autorisations que vous appliquez au groupe, indépendamment des rôles/autorisations associées à leur compte individuel.
5.  Dans la liste qui s’affiche, sélectionnez les utilisateurs à attribuer au nouveau groupe. Vous pouvez utiliser la zone de recherche pour rechercher des utilisateurs spécifiques.
6.  Une fois les utilisateurs sélectionnés, cliquez sur **Ajouter la sélection** pour les ajouter au nouveau groupe.
7.  Cliquez sur **Enregistrer**.


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>Ajouter des applications Azure AD pour votre compte espace partenaires

Vous pouvez autoriser les applications ou services qui font partie de Azure votre organisation AD pour accéder à votre compte espace partenaires. Ces comptes d’utilisateur d’applications Azure AD peuvent être utilisés pour appeler les API REST fournies par les [services Microsoft Store](../monetize/using-windows-store-services.md).


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>Ajouter des applications Azure AD à partir de l’annuaire de votre organisation

1.  1.  Sélectionnez l’icône d’engrenage (en haut à droite de partenaires), puis **les paramètres du développeur**. Dans le **paramètres** menu, sélectionnez **utilisateurs**.
2. Sur la page **Utilisateurs**, sélectionnez **Ajouter des applications Azure AD**.
3.  Sélectionnez une ou plusieurs applications Azure AD dans la liste qui s’affiche. Vous pouvez utiliser la zone de recherche pour rechercher des applications Azure AD spécifiques.
    > [!TIP]
    > Si vous sélectionnez plusieurs applications Azure AD à ajouter à votre compte espace partenaires, vous devez les attribuer le même rôle ou jeu d’autorisations personnalisées. Si vous souhaitez ajouter plusieurs applications Azure AD dotées de rôles ou d’autorisations distincts, répétez les étapes ci-après pour chaque rôle ou ensemble d’autorisations personnalisées.

4.  Une fois les applications Azure AD sélectionnées, cliquez sur **Ajouter la sélection**.
5.  Dans la section **Rôles**, spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués aux applications Azure AD sélectionnées.
6.  Cliquez sur **Enregistrer**.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Créer une nouvelle application Azure AD dans le répertoire de votre organisation de compte et ajoutez-le à votre compte espace partenaires

Si vous souhaitez accorder l’accès des partenaires à un compte d’application marque nouveau répertoire Azure AD, vous pouvez en créer un dans le **utilisateurs** section. Notez que cela créera un nouveau compte dans le répertoire de votre organisation, pas seulement dans votre compte espace partenaires.

> [!TIP]
> Si vous utilisez principalement cette application Azure AD pour l’authentification des partenaires et que vous n’avez pas aux utilisateurs d’y accéder directement, vous pouvez entrer n’importe quelle adresse valide pour le **URL de réponse** et **URI ID d’application**, tant en tant que ces valeurs ne sont pas utilisés par une autre application Azure AD dans votre répertoire.

1.  À partir de la **utilisateurs** page (sous **paramètres du compte**), sélectionnez **ajouter des applications Azure AD**.
2.  Sur la page suivante, sélectionnez **New Azure AD application**.
3.  Renseignez le champ **URL de réponse** pour la nouvelle application Azure AD. Il s’agit de l’URL qui permet aux utilisateurs de se connecter et d’utiliser votre application Azure AD (parfois également désignée sous le terme d’URL de l’application ou d’URL de connexion). L’**URL de réponse** ne peut pas comporter plus de 256 caractères et doit être unique dans votre annuaire.
4.  Renseignez le champ **URI ID d’application** pour la nouvelle application Azure AD. Il s’agit d’un identificateur logique pour l’application Azure AD qui est présenté lors de l’envoi d’une demande d’authentification unique à Azure AD. Notez que l’**URI ID d’application** doit être unique pour chaque application Azure AD de votre annuaire et ne doit pas comporter plus de 256 caractères. Pour plus d’informations sur l’**URI ID d’application**, voir [Intégration d’applications à Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant).
5.  Dans la section **Rôles**, spécifiez le ou les [rôles ou autorisations personnalisées](set-custom-permissions-for-account-users.md) attribués à l’application Azure AD.
6.  Cliquez sur **Enregistrer**.

Après avoir ajouté ou créé une application Azure AD, revenez à la section **Utilisateurs**, puis sélectionnez le nom de l’application pour revoir les paramètres de l’application, y compris l’ID de locataire, l’ID de client, l’URL de réponse et l’URI d’ID d’application.

> [!NOTE]
> Si vous envisagez d’utiliser les API REST fournies par les [services du Microsoft Store](../monetize/using-windows-store-services.md), vous aurez besoin des valeurs ID de locataire et ID de client indiquées sur cette page pour obtenir un jeton d’accès Azure AD que vous pourrez utiliser pour authentifier les appels aux services.   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>Gérer les clés pour une application Azure AD

Si votre application Azure AD lit et écrit des données dans Microsoft Azure AD, elle doit disposer d’une clé. Vous pouvez créer des clés pour une application Azure AD en modifiant ses informations dans le centre de partenaires. Vous pouvez également supprimer les clés qui ne sont plus nécessaires.

1.  À partir de la **utilisateurs** page (sous **paramètres du compte**), sélectionnez le nom de l’application Azure AD.
    > [!TIP]
    > Lorsque vous cliquez sur le nom de l’application Azure AD, vous verrez toutes les touches actives pour l’application Azure AD, y compris la date sur laquelle la clé a été créée et sa date d’expiration. Pour supprimer une clé devenue inutile, cliquez sur **Supprimer**.

2.  Pour ajouter une nouvelle clé, sélectionnez **ajouter une nouvelle clé**.
3.  Un écran vous présente les valeurs **ID client** et **Clé**.
    > [!IMPORTANT]
    > Veillez à les imprimer ou copier ces informations, comme vous ne pourrez pas y accéder à nouveau une fois que vous quittez cette page.

4.  Si vous souhaitez créer plus de clés, sélectionnez **ajouter une autre clé**.

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>Modifier un utilisateur, un groupe ou une application Azure AD

Une fois que vous avez ajouté des utilisateurs, groupes et/ou applications Azure AD pour votre compte espace partenaires, vous pouvez apporter des modifications à leurs informations de compte. 

> [!IMPORTANT]
> Modifications apportées à [rôles ou autorisations](set-custom-permissions-for-account-users.md) affectent uniquement les accès des partenaires. Toutes les autres modifications (par exemple, la modification d’un nom d’utilisateur ou l’appartenance au groupe, ou l’URL de réponse et l’URI ID d’application pour une application Azure AD) apparaîtront dans le locataire Azure AD de votre organisation, ainsi que ceux de votre compte espace partenaires. 

1.  À partir de la **utilisateurs** page (sous **paramètres du compte**), sélectionnez le nom de l’utilisateur, un groupe ou un compte d’application Azure AD que vous souhaitez modifier.
2.  Apportez les modifications souhaitées. Les éléments que vous pouvez modifier sont les suivants :
    -   Dans le cas d’un **utilisateur**, vous pouvez modifier le prénom, le nom ou le nom d’utilisateur. Vous pouvez également sélectionner ou désélectionner des groupes dans la section **Appartenance au groupe** pour mettre à jour l’appartenance au groupe d’un utilisateur.
    -   Dans le cas d’un **groupe**, vous pouvez modifier le nom du groupe. (Pour mettre à jour l’appartenance au groupe, modifiez les utilisateurs que vous souhaitez ajouter ou supprimer au niveau du groupe et apportez des modifications à la section **Appartenance au groupe**.)
    -   Dans le cas d’une **application Azure AD**, vous pouvez renseigner les champs **URL de réponse** ou **URI ID d’application**.
    N’oubliez pas que ces modifications sont effectuées dans le répertoire de votre organisation, ainsi que ceux de votre compte espace partenaires.
3.  Pour les modifications relatives à l’accès des partenaires, sélectionnez ou désélectionnez l’ou les rôles que vous souhaitez appliquer, ou sélectionnez **personnaliser les autorisations** et apportez les modifications souhaitées. Ces modifications affectent uniquement les partenaires accéder et ne changera pas d’autorisations dans le locataire Azure AD de votre organisation.
3.  Cliquez sur **Enregistrer**.


## <a name="view-history-for-account-users"></a>Afficher l’historique des utilisateurs du compte

En tant que propriétaire de compte, vous pouvez afficher l’historique détaillé de navigation des utilisateurs supplémentaires ajoutés au compte.

Sur le **utilisateurs** page (sous **paramètres du compte**), sélectionnez le lien figurant sous **dernière activité** pour l’utilisateur dont l’historique de navigation vous souhaitez vérifier. Vous serez en mesure de visualiser les URL de toutes les pages auxquelles a accédé l’utilisateur au cours des 30 derniers jours.

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>Supprimer des utilisateurs, des groupes et des applications Azure AD

Pour supprimer un utilisateur, un groupe ou une application Azure AD à partir de votre compte espace partenaires, sélectionnez le **supprimer** lien qui apparaît par leur nom sur le **utilisateurs** page. Après avoir confirmé que vous souhaitez supprimer, cet utilisateur, un groupe ou une application Azure AD sera n’est plus en mesure d’accéder à votre compte espace partenaires (sauf si vous le rajouter plus tard).

> [!IMPORTANT]
> Suppression d’un utilisateur, un groupe ou une application Azure AD signifie qu’il n’a plus accès à votre compte espace partenaires. Cette opération ne supprime **pas** l’utilisateur, le groupe ou l’application Azure AD de l’annuaire de votre organisation.

 

