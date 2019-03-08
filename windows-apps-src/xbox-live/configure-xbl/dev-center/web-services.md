---
title: Services Web
description: Découvrez comment créer un Service Web pour votre application
ms.assetid: ''
ms.date: 06-04-2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, les services web
ms.openlocfilehash: 66668336e3575201b305e6ecf09b4f277d2fc7b3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600014"
---
# <a name="set-up-web-services-in-udc"></a>Configurer les Services Web dans UDC

> [!WARNING]
> L’article suivant concerne ID@Xbox et aux développeurs de partenaires gérés uniquement en raison de restrictions placées sur la configuration de Service Web. Configuration des Services Web est uniquement disponible pour les développeurs avec l’autorisation de niveau compte Parties de confiance accordée. Contactez votre responsable de compte de développement (DAM) si vous n’avez pas de contrôle des autorisations de niveau de votre compte.

Les éditeurs peuvent créer des Services Web s’ils souhaitent personnaliser la façon dont leurs applications/titres interagir avec les services Xbox Live. Services Web sont des configurations au niveau du serveur de publication et peuvent être appelées par n’importe quel titre dans un bac à sable détenu par le serveur de publication en configurant l’authentification unique.

Raisons pour définir des Services Web :

1. Fournir l’authentification unique aux utilisateurs de Xbox Live - pour votre service web fournir à l’authentification unique aux utilisateurs de Xbox Live, il doit être configuré comme une partie de confiance de Xbox Live. Lors de la configuration de cette façon, les utilisateurs authentifiés à Xbox Live seront authentifiés automatiquement à votre service sans avoir à entrer à nouveau un ensemble différent d’informations d’identification.
2. Appels de service à partir de votre service aux services Xbox Live - si votre produit utilise l’un de vos services web pour effectuer des appels à un service Xbox Live, soit directement, soit pour le compte des utilisateurs individuels, vous devez un certificat de partenaire d’entreprise.

1. ## <a name="create-a-web-service"></a>Créer un Service Web

1. Accédez à la [tableau de bord de partenaires](https://partner.microsoft.com/dashboard/windows/overview)  
2. Cliquez sur l’icône d’engrenage en forme à l’angle supérieur droit de la page pour accéder à la **paramètres** liste déroulante.
3. Dans la liste déroulante, sélectionnez **les paramètres du développeur**.
4. Dans la barre de navigation de gauche, développez l’option **Xbox Live** et sélectionnez **Services Web**.

![Web services gif ](../../images/dev-center/web-services/web-services.gif)

5. Dans la page Services Web, cliquez sur **nouveau Service Web**.
6. Entrez le nom du Service Web et choisissez le type d’accès en fonction des besoins.  
    * Accès à la télémétrie permet à votre service récupérer des données de jeu de données de télémétrie pour une de vos jeux.
    * Accès à la chaîne de l’application de donner au fournisseur de support qui possède le service de l’autorité nécessaire pour publier des canaux d’application pour une utilisation sur la console via la torsion OneGuide par programmation.
7. Cliquez sur **Enregistrer**.

À ce stade, vous avez défini le service et Xbox Live est conscient de l’existence du service. Selon les raisons justifiant la création du service web, vous devrez configurer Parties(Single Sing-On) partie de confiance ou entreprise partenaire Certificates(Service-to-service calls).  

## <a name="configure-relying-party"></a>Configurer la partie de confiance

Un service web doit être configuré comme une partie de confiance de Xbox live afin de fournir l’expérience de l’authentification unique aux utilisateurs de Xbox Live – les utilisateurs authentifiés à Xbox Live sont automatiquement authentifiées au service web sans avoir à entrer à nouveau un ensemble différent d’informations d’identification. Pour faciliter cette tâche, l’approbation doit être établie entre les services Xbox et le service web. Un ensemble de revendications (par exemple, le gamertag, type d’appareil, ID de titre) sont utilisés en tant que partie de confiance des configurations de tiers pour appliquer cette approbation. Il s’agit d’informations échangées entre Xbox Live et le service web afin de vous authentifier automatiquement les utilisateurs.

### <a name="create-a-relying-party"></a>Créer une partie de confiance

1. Accédez au tableau de bord de partenaires  
2. Cliquez sur l’icône d’engrenage en forme à l’angle supérieur droit de la page pour accéder à la **paramètres** liste déroulante.
3. Dans la liste déroulante, sélectionnez **les paramètres du développeur**.
4. Dans la barre de navigation de gauche, développez l’option **Xbox Live** et sélectionnez **Parties de confiance**.
5. Dans la page de Parties de confiance, cliquez sur **nouvelle partie de confiance**.
6. Entrez l’URI de la partie de confiance dans ce format : *example.com*.
7. Sélectionnez le type de chiffrement à utiliser pour garantir la sécurité du service tiers par partie de confiance.
8. Si vous avez sélectionné le chiffrement symétrique avec des clés partagées à l’étape précédente, cliquez sur **générer une nouvelle clé** pour obtenir une nouvelle clé partagée. Suivez les instructions à l’écran pour enregistrer en toute sécurité la clé.
9. Entrez le **durée de vie de jeton** en heures.
10. Sous **revendications**, la liste déroulante propose une liste de revendications que votre service tiers de confiance peut utiliser à des fins d’authentification. Sélectionnez toutes les revendications que vous souhaitez utiliser. Les revendications sélectionnées apparaît sous la liste déroulante. Certaines revendications standards seront renseignées dans cet espace par défaut.
11. Cliquez sur **enregistrer** lorsque vous avez terminé.  

## <a name="configure-a-business-partner-certificate"></a>Configurer un certificat de partenaire d’entreprise

Si votre produit un de vos services web utilisera pour effectuer des appels à un service Xbox Live, soit directement, soit pour le compte des utilisateurs individuels, vous aurez besoin un certificat de partenaire d’entreprise.

### <a name="generate-a-business-partner-certificate"></a>Générer un certificat de partenaire d’entreprise

Effectuez les étapes ci-dessous après avoir créé un Service Web.  

1. Sur la page de Services Web, recherchez le service web que vous souhaitez associer un certificat de partenaire d’entreprise avec.
2. Sélectionnez le **générer un certificat** lien auprès du service web choisi.
3. Cliquez sur **afficher les Options** en regard de la génération d’un nouveau certificat. Ceci affichera les commandes qui doivent être exécutés à partir de PowerShell avec des privilèges d’administrateur.
4. Toutes les commandes que celui en cours d’exécution une fois que l’autre doit fournir avec succès en Base64 du blob codé. Il s’agit de la clé publique. Copiez la clé publique à partir de PowerShell et collez-le dans l’espace réservé pour l’objet Blob CSP.
5. Cliquez sur **télécharger** et suivez les instructions sur la page pour les certificats de liaison.
    1. Utiliser le même ordinateur que vous avez utilisé pour générer la clé publique.
    2. Exécutez la commande suivante dans PowerShell : *mmc.exe*
    3. Sélectionnez le fichier et sélectionnez Ajouter/supprimer un composant logiciel enfichable.
    4. Sélectionnez certificats et sélectionnez Ajouter. Veillez à sélectionner le compte d’ordinateur pour le composant logiciel enfichable Certificats et cliquez sur Terminer, puis cliquez sur OK.
    5. Ouvre le magasin Personal\Certificate.
    6. Cliquez avec le bouton droit sur les certificats et sélectionnez toutes les tâches et sélectionnez Importer.
    7. Sélectionnez le certificat que vous avez téléchargé à partir de UDC.
    8. Un clic droit sur le certificat dans l’interface utilisateur une fois qu’il a été importé et sélectionnez toutes les tâches et sélectionnez Exporter.
    9. Suivez l’Assistant d’exportation et veillez à sélectionner pour exporter la clé privée avec le certificat.
    10. Terminez l’Assistant Exportation.