---
author: jnHs
Description: In order to add and manage account users, you must first associate your Dev Center account with your organization's Azure Active Directory.
title: Associer Azure Active Directory à votre compte du Centre de développement
ms.author: wdg-dev-content
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, uwp, azure ad, client azure, client aad, client azure ad, gestion des clients, clients
ms.localizationpriority: medium
ms.openlocfilehash: dd729d76705849c981516109da39bbd27c140286
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2018
ms.locfileid: "4953429"
---
# <a name="associate-azure-active-directory-with-your-dev-center-account"></a>Associer Azure Active Directory à votre compte du Centre de développement

Pour [ajouter et gérer des utilisateurs de comptes](add-users-groups-and-azure-ad-applications.md), vous devez commencer par associer votre compte du Centre de développement au service Azure Active Directory de votre organisation. 

Le Centre de développement Windows tire profit d’AzureAD pour l'accès et la gestion des comptes multiutilisateur. Si votre organisation utilise déjà Office365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’AzureAD. Dans le cas contraire, vous pouvez créer un client AzureAD à partir du Centre de développement sans frais supplémentaires.

> [!TIP]
> Cette rubrique est spécifique au programme destiné aux développeurs d’applications Windows, mais l’association d’un client et la gestion des utilisateurs fonctionnent de la même façon pour les comptes dans le Programme pour applications de bureau Windows (voir [Programme pour applications de bureau Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users) pour plus d’informations) et dans le Programme pour développeurs de matériel Windows (où les références au rôle **Manager** s’appliquent également aux comptes Matériel ayant le rôle **Administrator**; voir [Administration du tableau de bord](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration) pour plus d’informations).

Un seul client AzureAD peut être associé à plusieurs comptes du Centre de développement. Il vous suffit d'avoir un client Azure AD associé à votre compte du Centre de développement pour ajouter plusieurs comptes d’utilisateur, mais vous avez également la possibilité d’ajouter plusieurs clients Azure AD à un seul compte du Centre de développement. Tout utilisateur disposant du rôle **Manager** dans le compte du Centre de développement aura la possibilité d’ajouter et de supprimer des clients Azure AD.

> [!IMPORTANT]
> Après avoir associé votre compte du Centre de développement avec votre client AzureAD, pour ajouter des comptes utilisateur à ce client et les gérer, vous devrez vous connecter au Centre de développement en tant qu'utilisateur dans le client qui dispose du rôle **Manager**.


## <a name="associate-your-dev-center-account-with-your-organizations-existing-azure-ad-tenant"></a>Associer votre compte du Centre de développement au client AzureAD existant de votre organisation

Si votre organisation utilise déjà AzureAD, procédez comme suit pour lier votre compte du Centre de développement.

1.  À partir du [tableau de bord du centre de développement Windows](https://partner.microsoft.com/dashboard), sélectionnez l’icône d’engrenage (près de l’angle supérieur droit du tableau de bord), puis sélectionnez **les paramètres de compte**. Dans le menu **paramètres** , sélectionnez **des clients**.
2.  Sélectionnez **Associer Azure AD à votre compte du Centre de développement**.
3.  Entrez vos informations d’identification Azure AD pour le client que vous souhaitez associer.
4.  Vérifiez le nom d’organisation et le nom de domaine pour votre locataire AzureAD. Pour valider l’association, sélectionnez **Confirmer**.
5.  Si l’association réussit, vous êtes prêt à ajouter et gérer des utilisateurs de compte dans la section **Utilisateurs** du Centre de développement.

> [!IMPORTANT]
> Pour pouvoir créer de nouveaux utilisateurs ou apporter d’autres modifications à votre Azure AD, vous devrez vous connecter à ce client Azure AD en utilisant un compte disposant des [autorisations d’administrateur global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) pour ce client. Toutefois, vous n’avez pas besoin des autorisations d’administrateur global pour associer le client, ni pour ajouter des utilisateurs qui existent déjà dans ce client dans votre compte du Centre de développement.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account"></a>Créer un tout nouvel AzureAD à associer à votre compte du Centre de développement

Si vous avez besoin de configurer un nouvel AzureAD à lier à votre compte du Centre de développement, procédez comme suit.

1.  À partir du [tableau de bord du centre de développement Windows](https://partner.microsoft.com/dashboard), sélectionnez l’icône d’engrenage (près de l’angle supérieur droit du tableau de bord), puis sélectionnez **les paramètres de compte**. Dans le menu **paramètres** , sélectionnez **des clients**.
2.  Sélectionnez **Créer Azure AD**.
3.  Entrez les informations de répertoire de votre nouvel Azure AD:
    - **Nom de domaine**: nom unique que nous utiliserons pour votre domaine Azure AD, avec «.onmicrosoft.com». Par exemple, si vous avez entré «exemple», votre domaine Azure AD sera «exemple.onmicrosoft.com».
    - **E-mail de contact**: adresse de messagerie à laquelle nous pouvons vous contacter à propos de votre compte si nécessaire.
    - **Informations de compte d’utilisateur administrateur général**: prénom, nom, nom d’utilisateur et mot de passe que vous souhaitez utiliser pour le nouveau compte d’administrateur général.
4.  Cliquez sur **Créer** pour confirmer les nouvelles informations de domaine et de compte.
5.  Connectez-vous avec vos nouveaux nom d’utilisateur et mot de passe d’administrateur général AzureAD pour commencer à [ajouter et gérer des utilisateurs de compte supplémentaires](add-users-groups-and-azure-ad-applications.md).


## <a name="manage-azure-ad-tenant-associations"></a>Gérer des associations de clients AzureAD

Une fois que vous avez associé un client Azure AD avec votre compte du Centre de développement, vous pouvez ajouter de nouveaux clients ou en supprimer d'existants à partir de la page **clients** page.


### <a name="add-multiple-azure-ad-tenants-to-your-dev-center-account"></a>Ajouter plusieurs clients AzureAD à votre compte du Centre de développement

Tout utilisateur qui dispose du rôle **Manager** rôle pour un compte du Centre de développement peut associer des clients Azure AD avec le compte.

Pour associer un nouveau locataire, sélectionnez **Associer un autre client AzureAD**, puis suivez les étapes indiquées ci-dessus. Notez que vos informations d'identification vous seront demandées dans le client Azure AD que vous souhaitez associer.


### <a name="remove-an-azure-ad-tenant-from-your-dev-center-account"></a>Supprimer un client Azure AD de votre compte du Centre de développement

Tout utilisateur qui dispose du rôle **Manager** rôle pour un compte du Centre de développement peut supprimer des clients Azure AD du compte.

> [!IMPORTANT]
> Lorsque vous supprimez un client, tous les utilisateurs qui ont été ajoutés au compte du Centre de développement à partir de ce client ne pourront plus se connecter au compte. 

Pour supprimer un client, trouvez son nom dans la page **des clients** (dans **les paramètres de compte**), puis sélectionnez **Supprimer**. Vous serez invité à confirmer que vous souhaitez supprimer le client. Cela fait, aucun utilisateur du Centre de développement dans ce client ne pourra plus se connecter au compte du Centre de développement, et toutes les autorisations que vous aurez configurées pour ces utilisateurs seront supprimées.

> [!TIP]
> Vous ne pouvez pas supprimer un client si vous êtes actuellement connecté au Centre de développement à l’aide d’un compte de ce même client. Pour supprimer un client, vous devez vous connecter au Centre de développement en tant que **Manager** pour un autre client qui est associé au compte. S’il n'existe qu’un seul client associé au compte, ce client ne peut être supprimé qu'après vous être connecté avec le compte Microsoft ayant ouvert le compte.


