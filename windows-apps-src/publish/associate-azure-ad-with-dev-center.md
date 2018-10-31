---
author: jnHs
Description: In order to add and manage account users, you must first associate your Partner Center account with your organization's Azure Active Directory.
title: Associer Azure Active Directory à votre compte espace partenaires
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: Windows10, uwp, azure ad, client azure, client aad, client azure ad, gestion des clients, clients
ms.localizationpriority: medium
ms.openlocfilehash: a76021f53417d30b91db282a194f6dc6ca268c1f
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5888695"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Associer Azure Active Directory à votre compte espace partenaires

Dans l’ordre pour [Ajouter et gérer les utilisateurs de compte](add-users-groups-and-azure-ad-applications.md), vous devez tout d’abord associer votre compte espace partenaires à Azure Active Directory de votre organisation. 

Azure AD pour accéder de comptes multiutilisateur et tire parti de [L’espace partenaires](https://partner.microsoft.com/dashboard) . Si votre organisation utilise déjà Office365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’AzureAD. Dans le cas contraire, vous pouvez créer un nouveau locataire Azure AD à partir de l’espace partenaires sans frais supplémentaires.

> [!TIP]
> Cette rubrique est spécifique au programme de développeur d’applications Windows dans [L’espace partenaires](https://partner.microsoft.com/dashboard), mais association d’un client et la gestion des utilisateurs fonctionnent de la même façon pour les comptes dans le programme d’Application de bureau Windows (consultez ce [Programme d’Application de bureau Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users) plus d’informations) et dans le programme de développement matériel Windows (où les références au rôle **Gestionnaire** seraient s’appliquent également aux comptes matériel ayant le rôle **d’administrateur** ; pour plus d’informations, consultez [Administration du tableau de bord](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration) ).

Un seul client Azure AD peut être associé à plusieurs comptes espace partenaires. Il vous suffit d’avoir un client Azure AD associé à votre compte espace partenaires pour ajouter plusieurs comptes d’utilisateur, mais vous avez également la possibilité d’ajouter plusieurs clients Azure AD à un seul compte de l’espace partenaires. Tout utilisateur avec le rôle de **responsable** du compte de l’espace partenaires auront la possibilité d’ajouter et supprimer des clients Azure AD à partir du compte.

> [!IMPORTANT]
> Après avoir associé votre compte espace partenaires de votre client Azure AD, afin d’ajouter et gérer des utilisateurs de compte dans ce client, vous devez vous connecter à l’espace partenaires en tant qu’utilisateur dans le client qui a le rôle **Manager** .


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>Associer votre compte espace partenaires au client Azure AD existant de votre organisation

Si votre organisation utilise déjà Azure AD, suivez ces étapes pour lier votre compte espace partenaires.

1.  À partir de [L’espace partenaires](https://partner.microsoft.com/dashboard) , sélectionnez l’icône d’engrenage (près de l’angle supérieur droit du tableau de bord), puis sélectionnez les **paramètres de développement**. Dans le menu **paramètres** , sélectionnez **des clients**.
2.  Sélectionnez **associer Azure AD avec votre compte espace partenaires**.
3.  Entrez vos informations d’identification Azure AD pour le client que vous souhaitez associer.
4.  Vérifiez le nom d’organisation et le nom de domaine pour votre locataire AzureAD. Pour valider l’association, sélectionnez **Confirmer**.
5.  Si l’association réussite, vous serez alors prêt à ajouter et gérer des utilisateurs de compte dans la section **des utilisateurs** dans l’espace partenaires.

> [!IMPORTANT]
> Pour pouvoir créer de nouveaux utilisateurs ou apporter d’autres modifications à votre Azure AD, vous devrez vous connecter à ce client Azure AD en utilisant un compte disposant des [autorisations d’administrateur global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) pour ce client. Toutefois, vous n’avez pas besoin autorisations d’administrateur global pour associer le client, ou pour ajouter des utilisateurs qui existent déjà dans ce client à votre compte espace partenaires.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>Créer un tout nouvel Azure AD à associer à votre compte espace partenaires

Si vous avez besoin configurer un nouvel Azure AD à lier à votre compte espace partenaires, procédez comme suit.

1.  À partir du [Centre de partenaires](https://partner.microsoft.com/dashboard), sélectionnez l’icône d’engrenage (près de l’angle supérieur droit du tableau de bord), puis sélectionnez les **paramètres de développement**. Dans le menu **paramètres** , sélectionnez **des clients**.
2.  Sélectionnez **Créer Azure AD**.
3.  Entrez les informations de répertoire de votre nouvel Azure AD:
    - **Nom de domaine**: nom unique que nous utiliserons pour votre domaine Azure AD, avec «.onmicrosoft.com». Par exemple, si vous avez entré «exemple», votre domaine Azure AD sera «exemple.onmicrosoft.com».
    - **E-mail de contact**: adresse de messagerie à laquelle nous pouvons vous contacter à propos de votre compte si nécessaire.
    - **Informations de compte d’utilisateur administrateur général**: prénom, nom, nom d’utilisateur et mot de passe que vous souhaitez utiliser pour le nouveau compte d’administrateur général.
4.  Cliquez sur **Créer** pour confirmer les nouvelles informations de domaine et de compte.
5.  Connectez-vous avec vos nouveaux nom d’utilisateur et mot de passe d’administrateur général AzureAD pour commencer à [ajouter et gérer des utilisateurs de compte supplémentaires](add-users-groups-and-azure-ad-applications.md).


## <a name="manage-azure-ad-tenant-associations"></a>Gérer des associations de clients AzureAD

Une fois que vous avez associé un client Azure AD à votre compte espace partenaires, vous pouvez ajouter de nouveaux clients ou supprimer des clients existants à partir de la page **des clients** .


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>Ajouter plusieurs clients Azure AD à votre compte espace partenaires

Tout utilisateur qui dispose du rôle de **Gestionnaire** pour un compte espace partenaires peut associer des clients Azure AD avec le compte.

Pour associer un nouveau locataire, sélectionnez **Associer un autre client AzureAD**, puis suivez les étapes indiquées ci-dessus. Notez que vos informations d'identification vous seront demandées dans le client Azure AD que vous souhaitez associer.


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>Supprimer un client Azure AD de votre compte de l’espace partenaires

Tout utilisateur qui dispose du rôle de **Gestionnaire** pour un compte espace partenaires peut supprimer le compte des clients Azure AD.

> [!IMPORTANT]
> Lorsque vous supprimez un client, tous les utilisateurs qui ont été ajoutés au compte de l’espace partenaires à partir de ce client ne sera n’est plus en mesure de se connecter au compte. 

Pour supprimer un client, trouvez son nom dans la page **des clients** (dans **les paramètres de compte**), puis sélectionnez **Supprimer**. Vous serez invité à confirmer que vous souhaitez supprimer le client. Une fois que vous procédez ainsi, aucun utilisateur dans ce client ne sera en mesure de se connecter au compte de l’espace partenaires, et toutes les autorisations que vous avez configurées pour ces utilisateurs seront supprimées.

> [!TIP]
> Vous ne pouvez pas supprimer un client si vous êtes actuellement connecté à l’espace partenaires à l’aide d’un compte dans le client. Pour supprimer un client, vous devez signer vers l’espace partenaires en tant que **Gestionnaire** pour un autre client qui est associé au compte. S’il n'existe qu’un seul client associé au compte, ce client ne peut être supprimé qu'après vous être connecté avec le compte Microsoft ayant ouvert le compte.


