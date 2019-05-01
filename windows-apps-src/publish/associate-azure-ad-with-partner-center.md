---
Description: Pour ajouter et gérer des utilisateurs de compte, vous devez tout d’abord associer votre compte espace partenaires Azure Active Directory de votre organisation.
title: Associer Azure Active Directory à votre compte espace partenaires
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, azure ad, client azure, client aad, client azure ad, gestion des clients, clients
ms.localizationpriority: medium
ms.openlocfilehash: 5d830fb3a984faecd93519b4e682ade59077d2d6
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63787226"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Associer Azure Active Directory à votre compte espace partenaires

Dans l’ordre aux [ajouter et gérer des utilisateurs de compte](add-users-groups-and-azure-ad-applications.md), vous devez tout d’abord associer votre compte espace partenaires Azure Active Directory de votre organisation. 

[Partenaires](https://partner.microsoft.com/dashboard) tire parti d’Azure AD pour la gestion et accès aux comptes d’utilisateurs multiples. Si votre organisation utilise déjà Office 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’Azure AD. Sinon, vous pouvez créer un nouveau client à partir d’Azure AD au sein des partenaires sans frais supplémentaires.

> [!TIP]
> Cette rubrique est spécifique du programme de développeur d’applications Windows dans [partenaires](https://partner.microsoft.com/dashboard), mais l’association d’un client et la gestion des utilisateurs fonctionne de la même façon pour les comptes dans le programme d’Application de bureau Windows (consultez [Windows Programme d’Application de bureau](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users) pour plus d’informations) et dans le programme de développement de matériel Windows (où fait référence à la **Manager** rôle s’applique également aux comptes de matériel avec les **administrateur**  rôle ; consultez [Administration du tableau de bord](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration) pour plus d’informations).

Un seul locataire Azure AD peut être associé à plusieurs comptes de partenaires. Vous devez uniquement un seul locataire Azure AD associé à votre compte espace partenaires pour ajouter plusieurs utilisateurs de compte, mais vous avez également la possibilité d’ajouter plusieurs locataires Azure AD à un seul compte de partenaires. Tout utilisateur ayant le **Manager** rôle dans le compte de partenaires aura l’option pour ajouter et supprimer des locataires Azure AD à partir du compte.

> [!IMPORTANT]
> Une fois que vous associez votre compte espace partenaires à votre client Azure AD, afin d’ajouter et gérer des utilisateurs de compte dans ce client, vous devez vous connecter à des partenaires en tant qu’utilisateur dans le même client qui a le **Manager** rôle.


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>Associer votre compte espace partenaires de locataire Azure AD de votre organisation

Si votre organisation utilise déjà Azure AD, suivez ces étapes pour lier votre compte espace partenaires.

1.  À partir de [partenaires](https://partner.microsoft.com/dashboard), sélectionnez l’icône d’engrenage (dans le coin supérieur droit du tableau de bord) puis **les paramètres du développeur**. Dans le **paramètres** menu, sélectionnez **locataires**.
2.  Sélectionnez **associer Azure AD avec votre compte espace partenaires**.
3.  Entrez vos informations d’identification Azure AD pour le client que vous souhaitez associer.
4.  Vérifiez le nom d’organisation et le nom de domaine pour votre locataire Azure AD. Pour valider l’association, sélectionnez **Confirmer**.
5.  Si l’association est réussie, vous serez ensuite prêt à ajouter et gérer des utilisateurs de compte dans le **utilisateurs** section dans l’espace partenaires.

> [!IMPORTANT]
> Pour pouvoir créer de nouveaux utilisateurs ou apporter d’autres modifications à votre Azure AD, vous devrez vous connecter à ce client Azure AD en utilisant un compte disposant des [autorisations d’administrateur global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) pour ce client. Toutefois, vous n’avez pas besoin une autorisation d’administrateur global afin d’associer le client, ou pour ajouter des utilisateurs qui existent déjà dans ce locataire à votre compte espace partenaires.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>Créer un toute nouvel application Azure AD à associer à votre compte espace partenaires

Si vous avez besoin configurer une nouvelle annonce Azure pour créer un lien avec votre compte espace partenaires, procédez comme suit.

1.  À partir de [partenaires](https://partner.microsoft.com/dashboard), sélectionnez l’icône d’engrenage (dans le coin supérieur droit du tableau de bord) puis **les paramètres du développeur**. Dans le **paramètres** menu, sélectionnez **locataires**.
2.  Sélectionnez **Créer Azure AD**.
3.  Entrez les informations de répertoire de votre nouvel Azure AD :
    - **Nom de domaine**: Le nom unique que nous allons utiliser pour votre domaine Azure AD, avec «. onmicrosoft.com ». Par exemple, si vous avez entré « exemple », votre domaine Azure AD sera « exemple.onmicrosoft.com ».
    - **Adresse de messagerie**: Une adresse de messagerie où nous pouvons vous contacter sur votre compte si nécessaire.
    - **Informations de compte d’utilisateur administrateur général**: Le prénom, dernier nom, nom d’utilisateur et mot de passe que vous souhaitez utiliser pour le nouveau compte d’administrateur général.
4.  Cliquez sur **Créer** pour confirmer les nouvelles informations de domaine et de compte.
5.  Connectez-vous avec vos nouveaux nom d’utilisateur et mot de passe d’administrateur général Azure AD pour commencer à [ajouter et gérer des utilisateurs de compte supplémentaires](add-users-groups-and-azure-ad-applications.md).


## <a name="manage-azure-ad-tenant-associations"></a>Gérer des associations de clients Azure AD

Une fois que vous avez associé un locataire Azure AD avec votre compte espace partenaires, vous pouvez ajouter de nouveaux locataires ou supprimer des clients existants à partir de la **locataires** page.


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>Ajouter plusieurs locataires Azure AD à votre compte espace partenaires

Tout utilisateur qui a le **Manager** rôle pour un compte espace partenaires peut associer des locataires Azure AD avec le compte.

Pour associer un nouveau locataire, sélectionnez **Associer un autre client Azure AD**, puis suivez les étapes indiquées ci-dessus. Notez que vos informations d'identification vous seront demandées dans le client Azure AD que vous souhaitez associer.


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>Supprimer un client Azure AD à partir de votre compte espace partenaires

Tout utilisateur qui a le **Manager** rôle pour un compte espace partenaires peut supprimer des locataires Azure AD à partir du compte.

> [!IMPORTANT]
> Lorsque vous supprimez un client, tous les utilisateurs qui ont été ajoutés au compte de partenaires à partir de ce client ne seront n’est plus en mesure de se connecter au compte. 

Pour supprimer un client, recherchez son nom sur le **locataires** page (dans **paramètres du compte**), puis sélectionnez **supprimer**. Vous serez invité à confirmer que vous souhaitez supprimer le client. Une fois que vous procédez ainsi, aucun utilisateur dans ce client ne sera en mesure de vous connecter au compte de partenaires et les autorisations que vous avez configurée pour ces utilisateurs seront supprimées.

> [!TIP]
> Vous ne pouvez pas supprimer un locataire si vous êtes actuellement connecté au Partner Center à l’aide d’un compte dans le même client. Pour supprimer un client, vous devez vous connecter à des partenaires comme un **Manager** pour un autre client qui est associé au compte. S’il n'existe qu’un seul client associé au compte, ce client ne peut être supprimé qu'après vous être connecté avec le compte Microsoft ayant ouvert le compte.


