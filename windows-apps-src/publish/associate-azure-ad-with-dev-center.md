---
author: jnHs
Description: "Pour ajouter et gérer des utilisateurs de comptes, vous devez commencer par associer votre compte du Centre de développement au service Azure Active Directory de votre organisation."
title: "Associer Azure Active Directory à votre compte du Centre de développement"
ms.author: wdg-dev-content
ms.date: 07/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: ace9470cc707206461baa8c3dd72828ea68a8eb4
ms.sourcegitcommit: eaacc472317eef343b764d17e57ef24389dd1cc3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/17/2017
---
# <a name="associate-azure-active-directory-with-your-dev-center-account"></a>Associer Azure Active Directory à votre compte du Centre de développement

Pour [ajouter et gérer des utilisateurs de comptes](add-users-groups-and-azure-ad-applications.md), vous devez commencer par associer votre compte du Centre de développement au service Azure Active Directory de votre organisation. 

Le Centre de développement Windows tire profit d’AzureAD pour la gestion multi-utilisateur et l’attribution des rôles. Si votre organisation utilise déjà Office365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’AzureAD. Dans le cas contraire, vous pouvez créer un locataire AzureAD à partir du Centre de développement sans frais supplémentaires.

> [!IMPORTANT]
> Pour associer AzureAD à votre compte du Centre de développement, vous devez vous connecter à votre locataire AzureAD avec un compte [d’administrateur général](http://go.microsoft.com/fwlink/?LinkId=746654).
> 
> Après avoir associé votre compte du Centre de développement à AzureAD, vous aurez toujours besoin de vous connecter au Centre de développement à l’aide du compte d’administrateur général AzureAD (et non d’un compte Microsoft personnel) afin d’ajouter et de gérer les utilisateurs du compte.

Notez que vous ne pouvez associer qu’un seul compte du Centre de développement à un locataire AzureAD. De même, il n’est possible d’associer qu’un seul locataire AzureAD à un compte du Centre de développement. Une fois l’association établie, vous ne pouvez plus la supprimer sans prendre contact avec le support.


## <a name="associate-your-dev-center-account-with-your-organizations-existing-azure-ad-tenant"></a>Associer votre compte du Centre de développement au locataire AzureAD existant de votre organisation

Si votre organisation utilise déjà AzureAD, procédez comme suit pour lier votre compte du Centre de développement.

1.  Sous **Paramètres du compte**, cliquez sur **Gérer les utilisateurs**.
2.  Cliquez sur le bouton **Associer Azure AD avec votre compte du Centre de développement**.
3.  Connectez-vous à votre compte AzureAD. La configuration de l’association implique que ce compte dispose d’une autorisation [d’administrateur général](http://go.microsoft.com/fwlink/?LinkId=746654).
4.  Vérifiez le nom d’organisation et le nom de domaine pour votre locataire AzureAD. Pour valider l’association, cliquez sur **Confirmer**.
5.  Si l’association réussit, vous êtes prêt à ajouter et gérer des utilisateurs de compte dans la section **Gérer les utilisateurs** du Centre de développement.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account"></a>Créer un tout nouvel AzureAD à associer à votre compte du Centre de développement

Si vous avez besoin de configurer un nouvel Azure AD à lier à votre compte du Centre de développement, procédez comme suit.

1.  Sous **Paramètres du compte**, cliquez sur **Gérer les utilisateurs**.
2.  Cliquez sur le bouton **Créer Azure AD**.
3.  Entrez les informations de répertoire de votre nouvel Azure AD:
 - **Nom de domaine**: nom unique que nous utiliserons pour votre domaine Azure AD, avec «.onmicrosoft.com». Par exemple, si vous avez entré «exemple», votre domaine Azure AD sera «exemple.onmicrosoft.com».
 - **E-mail de contact**: adresse de messagerie à laquelle nous pouvons vous contacter à propos de votre compte si nécessaire.
 - **Informations de compte d’utilisateur administrateur général**: prénom, nom, nom d’utilisateur et mot de passe que vous souhaitez utiliser pour le nouveau compte d’administrateur général.
4.  Cliquez sur **Créer** pour confirmer les nouvelles informations de domaine et de compte.
5.  Connectez-vous avec vos nouveaux nom d’utilisateur et mot de passe d’administrateur général AzureAD pour commencer à [ajouter et gérer des utilisateurs de compte supplémentaires](add-users-groups-and-azure-ad-applications.md).



