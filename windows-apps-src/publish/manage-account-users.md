---
Vous pouvez utiliser Azure Active Directory pour ajouter des utilisateurs à votre compte du Centre de développement.
Gérer des utilisateurs de compte
ms.assetid: 9245F0D0-7D8F-4741-AFB4-FBA5601D0A9B
---

# Gérer des utilisateurs de compte


Vous pouvez utiliser Azure Active Directory pour ajouter des utilisateurs à votre compte du Centre de développement. Un rôle est attribué à chaque utilisateur. Il lui confère un ensemble d’autorisations spécifique à l’égard du compte. Vous pouvez également attribuer un rôle à un groupe d’utilisateurs ou à une application Azure AD.

> **Important** Pour ajouter et gérer des utilisateurs de comptes, vous devez d’abord associer votre compte du Centre de développement au service Azure Active Directory de votre organisation. Pour cela, vous devez vous connecter à Azure AD avec un compte [Administrateur global](http://go.microsoft.com/fwlink/?LinkId=746654). Une fois cette association établie, vous ne pouvez plus la supprimer sans prendre contact avec le support.

 

## Associer votre compte du Centre de développement au service Azure Active Directory de votre organisation


Le Centre de développement Windows tire parti d’Azure Active Directory pour la gestion multi-utilisateurs et l’attribution des rôles. Si votre organisation utilise déjà Office 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’Azure AD. Sinon, vous pouvez l’[obtenir gratuitement](http://go.microsoft.com/fwlink/p/?LinkId=703757).

Notez que vous ne pouvez associer qu’un seul compte du Centre de développement à un service Azure Active Directory. De même, il n’est possible d’associer qu’un seul service Azure Active Directory à un compte du Centre de développement.

Pour configurer l’association :

1.  Sous **Paramètres du compte**, cliquez sur **Gérer les utilisateurs**.
2.  Cliquez sur le bouton pour associer votre compte à Azure AD.
3.  Connectez-vous à votre compte Azure AD. La configuration de l’association implique que ce compte dispose de l’autorisation [Administrateur général](http://go.microsoft.com/fwlink/?LinkId=746654).
4.  Vérifiez le nom d’organisation et le nom de domaine pour votre compte Azure AD. Pour valider l’association, cliquez sur **Confirmer**.

Si l’association réussit, vous êtes prêt à ajouter et gérer des utilisateurs de comptes sur la page **Gérer les utilisateurs** de votre compte.

> **Remarque** Vous ne pouvez ajouter des utilisateurs à votre compte du Centre de développement que s’ils font partie du service Azure Active Directory de votre organisation. Vous ne pourrez pas ajouter d’utilisateurs à votre compte du Centre de développement en utilisant leur compte Microsoft.

## Ajouter et gérer des utilisateurs, des groupes et des applications Azure AD pour le compte


Une fois l’association établie, vous pouvez ajouter des utilisateurs, des groupes et des applications Azure AD à votre compte. Vous pouvez également modifier les rôles et les détails de compte ou supprimer des utilisateurs.

> **Remarque** Si votre organisation utilise l’[intégration du répertoire](http://go.microsoft.com/fwlink/p/?LinkID=724033) pour synchroniser le service d’annuaire local avec votre compte Azure AD, vous ne pourrez pas créer d’utilisateurs, de groupes ou d’applications Azure AD dans le Centre de développement. Vous (ou un autre administrateur de votre annuaire local) devrez créer ces éléments directement dans l’annuaire local avant de pouvoir les afficher et les ajouter dans le Centre de développement.

Lorsque vous gérez des utilisateurs, gardez à l’esprit les points suivants :

-   Tous les utilisateurs du Centre de développement doivent disposer d’un compte actif auprès du service Azure AD de votre organisation.
-   Lorsque vous **créez** un utilisateur ou un groupe dans le Centre de développement, il est également ajouté au service Azure AD de votre organisation.
-   Lorsque vous modifiez le nom d’un utilisateur ou d’un groupe dans le Centre de développement, les modifications sont répercutées dans le service Azure AD de votre organisation.
-   Les utilisateurs (y compris les groupes et applications Azure AD) pourront accéder à l’ensemble du compte du Centre de développement avec les autorisations associées au rôle qui leur est attribué. Vous ne pouvez pas limiter l’accès d’un utilisateur de sorte qu’il puisse utiliser uniquement des applications et/ou produits in-app spécifiques.
-   Vous pouvez autoriser un utilisateur, un groupe ou une application Azure AD à accéder aux fonctionnalités de différents rôles en sélectionnant plusieurs rôles.
-   Un utilisateur ayant un rôle spécifique peut également faire partie d’un groupe ayant un rôle différent. Dans ce cas, il aura accès aux fonctionnalités associées aux deux rôles.

### Rôles et autorisations

Chaque utilisateur, groupe ou application Azure AD que vous ajoutez à un compte doit se voir attribuer l’un ou plusieurs des rôles ci-après. Chaque rôle dispose d’un ensemble d’autorisations spécifique lui permettant d’exécuter certaines fonctions dans le cadre du compte.

> **Remarque** Le propriétaire du compte est la personne qui l’a créé en premier avec un compte Microsoft (et non l’un des utilisateurs ajoutés par le biais d’Azure AD). Il est le seul à disposer d’un accès complet au compte et à pouvoir notamment supprimer des applications, créer et modifier l’ensemble des utilisateurs du compte et modifier tous les paramètres financiers et de compte. Le compte Microsoft utilisé pour créer le compte doit être utilisé lors de la création de packages d’application dans Microsoft Visual Studio.

| Rôle                 | Description              |
|----------------------|--------------------------|
| Manager              | Dispose d’un accès complet au compte, mais ne peut pas modifier les paramètres fiscaux et de revenus. Ceci inclut la gestion des utilisateurs dans le Centre de développement. Cependant, notez que la possibilité de créer et supprimer des utilisateurs dépend des autorisations du compte dans Azure AD. Ainsi, si le rôle Manager est attribué à un utilisateur mais que celui-ci ne dispose pas des autorisations d’administration dans le service Azure AD de l’organisation, il ne pourra pas créer d’utilisateurs ni supprimer des utilisateurs de l’annuaire (mais pourra modifier le rôle d’un utilisateur dans le Centre de développement). |
| Développeur            | Peut charger des packages, soumettre des applications et produits in-app et afficher le [Rapport d’utilisation](usage-report.md) pour obtenir des informations de télémétrie détaillées. Il ne peut pas afficher les informations financières ni les paramètres de compte.                                                                                                                                                                                                                                                                                                                     |
| Contributeur professionnel | Peut accéder aux informations financières et définir les détails de tarification. Il ne peut pas créer ou soumettre de nouveaux produits in-app ou applications ni modifier les paramètres de compte.                                                                                                                                                                                                                                                                                                                                                              |
| Contributeur financier  | Peut afficher les [rapports de revenus](payout-summary.md). Il ne peut apporter aucune modification aux applications, produits in-app et paramètres de compte.                                                                                                                                                                                                                                                                                                                                                                                 |
| Responsable marketing             | Peut [répondre aux avis de clients](respond-to-customer-reviews.md) et afficher des [rapports analytiques](analytics.md) non financiers. Il ne peut apporter aucune modification aux applications, produits in-app et paramètres de compte.                                                                                                                                                                                                                                                                                                            |

> **Remarque** Les utilisateurs présentant le rôle Manager ou Développeur peuvent soumettre des applications par le biais du tableau de bord. Toutefois, lors de la création de packages d’application dans Visual Studio, il convient d’utiliser le compte Microsoft ayant servi à ouvrir le compte de développeur, plutôt qu’un compte Azure AD.

### Ajouter et gérer des utilisateurs du compte

Pour identifier les utilisateurs que vous souhaitez ajouter à votre compte du Centre de développement et leur attribuer un rôle, cliquez sur **Ajouter des utilisateurs**.

Vous pouvez ajouter un ou plusieurs utilisateurs à votre compte du Centre de développement à partir de l’annuaire de votre organisation. Notez que lorsque vous ajoutez plusieurs utilisateurs simultanément, vous devez leur attribuer le même rôle. Si vous souhaitez ajouter des utilisateurs et leur attribuer des rôles différents, répétez les étapes suivantes pour chaque rôle.

**Ajouter des utilisateurs à partir de l’annuaire de votre organisation**

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des utilisateurs**.
2.  Sélectionnez un ou plusieurs utilisateurs dans la liste qui s’affiche. Vous pouvez utiliser la zone de recherche pour rechercher des utilisateurs spécifiques.
3.  Une fois les utilisateurs sélectionnés, cliquez sur **Ajouter la sélection**.
4.  Dans la section **Rôles**, sélectionnez un ou plusieurs rôles à attribuer à cet ensemble d’utilisateurs.
5.  Cliquez sur **Enregistrer**.

Si vous souhaitez accorder à un nouveau compte d’utilisateur l’accès au Centre de développement, vous pouvez créer ce compte dans la section **Gérer les utilisateurs**. Notez que le compte sera créé non seulement dans votre compte du Centre de développement, mais également dans l’annuaire de votre organisation.

**Créer un compte d’utilisateur**

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des utilisateurs**.
2.  Sur la page suivante, cliquez sur **Nouvel utilisateur**.
3.  Entrez le prénom, le nom et le nom d’utilisateur du nouvel utilisateur.
4.  Dans la section **Rôles**, sélectionnez un ou plusieurs rôles à attribuer à ce nouvel utilisateur.
5.  Dans la section **Appartenance au groupe**, sélectionnez les groupes auxquels doit appartenir le nouvel utilisateur.
6.  Cliquez sur **Enregistrer**.
7.  Les informations de connexion du nouvel utilisateur, y compris un mot de passe temporaire, s’affichent sur la page de confirmation. Veillez à noter ces informations et à les fournir au nouvel utilisateur. Vous n’aurez plus accès au mot de passe temporaire après avoir quitté cette page.

Vous pouvez modifier des comptes d’utilisateur que vous avez ajoutés à votre compte du Centre de développement dans la section **Gérer les utilisateurs**. Notez que les modifications apportées au nom de l’utilisateur ou à son appartenance à un groupe ne seront pas seulement répercutées dans votre compte du Centre de développement, mais également dans l’annuaire de votre organisation. Les modifications apportées au rôle d’un utilisateur affecteront uniquement son accès au Centre de développement.

**Modifier un compte d’utilisateur**

1.  À partir de la page **Gérer les utilisateurs**, cliquez sur le nom du compte d’utilisateur que vous souhaitez modifier.
2.  Apportez une ou plusieurs des modifications suivantes :
    -   Modifiez le prénom, le nom ou le nom d’utilisateur. N’oubliez pas que ces modifications seront répercutées dans l’annuaire de votre organisation.
    -   Dans la section **Rôles**, sélectionnez ou désélectionnez les rôles que vous souhaitez ajouter ou supprimer pour cet utilisateur.
    -   Dans la section **Appartenance au groupe**, sélectionnez ou désélectionnez les groupes auxquels l’utilisateur doit être joint ou dont il doit être supprimé. N’oubliez pas que ces modifications seront répercutées dans l’annuaire de votre organisation.

3.  Cliquez sur **Enregistrer**.

Si vous devez modifier le mot de passe d’un compte d’utilisateur que vous avez ajouté à votre compte du Centre de développement, vous pouvez utiliser la section **Gérer les utilisateurs**. Notez que cela modifiera non seulement le mot de passe pour l’accès au Centre de développement, mais également le mot de passe d’annuaire de l’utilisateur.

**Modification du mot de passe d’annuaire d’un utilisateur**

1.  À partir de la page **Gérer les utilisateurs**, cliquez sur le nom du compte d’utilisateur que vous souhaitez modifier.
2.  Cliquez sur le bouton **Réinitialiser le mot de passe** en bas de la page.
3.  Une page de confirmation affiche les informations de connexion de l’utilisateur, y compris un mot de passe temporaire.
  > **Important** Veillez à imprimer ou copier ces informations et à les fournir à l’utilisateur, car vous n’aurez plus accès au mot de passe temporaire après avoir quitté cette page.

### Ajouter et gérer des groupes

Lorsque vous ajoutez un groupe à votre compte du Centre de développement partir de l’annuaire de votre organisation, tous les utilisateurs membres de ce groupe peuvent y accéder avec les autorisations associées au rôle attribué à ce groupe. Gardez à l’esprit que toutes les modifications apportées aux groupes (notamment à leur nom ou leur appartenance) seront répercutées dans l’annuaire de votre organisation.

Notez que lorsque vous ajoutez plusieurs groupes simultanément, vous devez leur attribuer le même rôle. Si vous souhaitez ajouter des groupes et leur attribuer des rôles différents, répétez les étapes suivantes pour chaque rôle.

**Ajouter des groupes à partir de l’annuaire de votre organisation**

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des groupes**.
2.  Sélectionnez un ou plusieurs groupes dans la liste qui s’affiche. Vous pouvez utiliser la zone de recherche pour rechercher des groupes spécifiques.
3.  Une fois les groupes sélectionnés, cliquez sur **Ajouter la sélection**.
4.  Dans la section **Rôles**, sélectionnez un ou plusieurs rôles à attribuer à cet ensemble de groupes.
5.  Cliquez sur **Enregistrer**.

Si vous souhaitez accorder à un nouveau groupe l’accès au Centre de développement, vous pouvez créer un groupe dans la section **Gérer les utilisateurs**. Notez que le groupe sera créé non seulement dans votre compte du Centre de développement, mais également dans l’annuaire de votre organisation.

**Créer un compte de groupe**

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des groupes**.
2.  Sur la page suivante, cliquez sur **Nouveau groupe**.
3.  Entrez le nom d’affichage du nouveau groupe.
4.  Sélectionnez un ou plusieurs rôles à attribuer à ce nouveau groupe. Tous les membres du groupe pourront accéder à votre compte du Centre de développement avec les autorisations associées à ce rôle.
5.  Sélectionnez un ou plusieurs utilisateurs dans la liste qui s’affiche. Vous pouvez utiliser la zone de recherche pour rechercher des utilisateurs spécifiques.
6.  Une fois les utilisateurs sélectionnés, cliquez sur **Ajouter la sélection**.
7.  Cliquez sur **Enregistrer**.

Vous pouvez modifier des comptes de groupe que vous avez ajoutés à votre compte du Centre de développement dans la section **Gérer les utilisateurs**. Notez que les modifications apportées au nom du groupe ou à l’appartenance au groupe seront non seulement répercutées dans votre compte du Centre de développement, mais également dans l’annuaire de votre organisation. Les modifications apportées au rôle d’un groupe affecteront uniquement son accès au Centre de développement.

**Modifier un compte de groupe**

1.  À partir de la page **Gérer les utilisateurs**, cliquez sur le nom du compte de groupe que vous souhaitez modifier.
2.  Pour modifier les informations du groupe, apportez les modifications souhaitées au nom du groupe. N’oubliez pas que ces modifications seront répercutées dans l’annuaire de votre organisation.
3.  Pour modifier le rôle du groupe, sélectionnez ou désélectionnez les rôles que vous souhaitez appliquer au groupe.
4.  Cliquez sur **Enregistrer**.

### Ajouter et gérer des applications Azure AD

Vous pouvez autoriser des applications ou des services faisant partie intégrante du service Azure AD de votre organisation à accéder à votre compte du Centre de développement.

Notez que lorsque vous ajoutez plusieurs applications Azure AD simultanément, vous devez leur attribuer le même rôle. Si vous souhaitez ajouter des groupes et leur attribuer des rôles différents, répétez les étapes suivantes pour chaque rôle.

**Ajouter des applications Azure AD à partir de l’annuaire de votre organisation**

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des applications Azure AD**.
2.  Sélectionnez une ou plusieurs applications Azure AD dans la liste qui s’affiche. Vous pouvez utiliser la zone de recherche pour rechercher des applications Azure AD spécifiques.
3.  Une fois les applications Azure AD sélectionnées, cliquez sur **Ajouter la sélection**.
4.  Dans la section **Rôles**, sélectionnez un ou plusieurs rôles à attribuer à cet ensemble d’applications Azure AD.
5.  Cliquez sur **Enregistrer**.

Si vous souhaitez accorder à un nouveau compte d’application Azure AD l’accès au Centre de développement, vous pouvez créer ce compte dans la section **Gérer les utilisateurs**. Notez que le compte sera créé non seulement dans votre compte du Centre de développement, mais également dans l’annuaire de votre organisation.

**Créer une application Azure AD**

1.  Sur la page **Gérer les utilisateurs**, cliquez sur **Ajouter des applications Azure AD**.
2.  Sur la page suivante, cliquez sur **Nouvelle application Azure AD**.
3.  Renseignez le champ **URL de réponse** pour la nouvelle application Azure AD. Il s’agit de l’URL qui permet aux utilisateurs de se connecter et d’utiliser votre application Azure AD (parfois également désignée sous le terme d’URL de l’application ou d’URL de connexion). L’**URL de réponse** ne peut pas comporter plus de 256 caractères.
4.  Renseignez le champ **URI ID d’application** pour la nouvelle application Azure AD. Il s’agit d’un identificateur logique pour l’application Azure AD qui est présenté lors de l’envoi d’une demande d’authentification unique à Azure AD. Notez que l’**URI ID d’application** doit être unique pour chaque application Azure AD de votre annuaire et ne doit pas comporter plus de 256 caractères.
5.  Dans la section **Rôles**, sélectionnez un ou plusieurs rôles à attribuer à la nouvelle application Azure AD.
6.  Cliquez sur **Enregistrer**.

Vous pouvez modifier les applications Azure AD que vous avez ajoutées à votre compte du Centre de développement dans la section **Gérer les utilisateurs**. Notez que les modifications apportées à l’URL de réponse et à l’URI d’ID d’application seront non seulement répercutées dans votre compte du Centre de développement, mais également dans l’annuaire de votre organisation. Les modifications de rôle affecteront uniquement les autorisations de l’application Azure AD dans le Centre de développement.

**Modifier une application Azure AD**

1.  Sur la page **Gérer les utilisateurs**, cliquez sur le nom du compte d’application Azure AD que vous souhaitez modifier.
2.  Pour modifier la valeur du champ **URL de réponse** ou **URI ID d’application**, entrez les nouvelles valeurs à cet emplacement. N’oubliez pas que ces modifications seront répercutées dans l’annuaire de votre organisation.
3.  Pour modifier le rôle de l’application Azure AD, sélectionnez ou désélectionnez les rôles que vous souhaitez appliquer.
4.  Cliquez sur **Enregistrer**.

Si votre application Azure AD lit et écrit des données dans Microsoft Azure AD, elle doit disposer d’une clé. Vous pouvez créer des clés pour une application Azure AD en modifiant les informations de cette dernière dans le Centre de développement. Vous pouvez également supprimer les clés qui ne sont plus nécessaires.

**Gérer les clés pour une application Azure AD**

1.  Sur la page **Gérer les utilisateurs**, cliquez sur le nom de l’application Azure AD.

    > **Conseil** Lorsque vous cliquez sur le nom de l’application Azure AD, vous voyez toutes les clés actives pour cette application, y compris les dates de création et d’arrivée à expiration des clés. Pour supprimer une clé devenue inutile, cliquez sur **Supprimer**.

2.  Pour ajouter une nouvelle clé, cliquez sur **Ajouter une clé**.

3.  Un écran vous présente les valeurs **ID client** et **Clé**.

    > **Important** Veillez à imprimer ou copier ces informations, car vous n’y aurez plus accès après avoir quitté cette page.

4.  Si vous souhaitez créer d’autres clés, cliquez sur **Ajouter une autre clé**.

### Supprimer des utilisateurs, des groupes et des applications Azure AD

Pour supprimer un utilisateur, un groupe ou une application Azure AD de votre compte du Centre de développement, cliquez sur le lien **Supprimer** en regard de leur nom sur la page **Gérer les utilisateurs**. Une fois la suppression du compte confirmée, l’utilisateur, le groupe ou l’application Azure AD ne pourront plus accéder à votre compte du Centre de développement (sauf si vous l’ajoutez de nouveau ultérieurement).

> **Remarque** Lorsque vous supprimez un utilisateur, un groupe ou une application Azure AD, ces derniers n’ont plus accès à votre compte du Centre de développement. L’utilisateur, le groupe ou l’application Azure AD n’est pas supprimé de l’annuaire de votre organisation.

 

 

 






<!--HONumber=Mar16_HO2-->


