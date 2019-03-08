---
title: Configurer des stratégies d’accès dans l’espace partenaires
description: Décrit comment configurer des stratégies d’accès dans l’espace partenaires pour autoriser d’autres applications, des jeux et services pour accéder aux paramètres de Xbox Live.
ms.assetid: ''
ms.date: 02/21/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, udc, centre de développement universelle
ms.openlocfilehash: ae26c18abdac30ff988e90ee5c56f178bf14b74a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658214"
---
# <a name="configure-access-policies-in-partner-center"></a>Configurer des stratégies d’accès dans l’espace partenaires

Vous pouvez utiliser [partenaires](https://partner.microsoft.com/dashboard) pour autoriser d’autres services, des jeux et applications d’accéder aux paramètres de Xbox Live et les données de titre de votre. Par exemple, voulez-vous un service web pour afficher des tableaux de résultats sur votre site Web, ou avoir une application Compagnon qui peut accéder au stockage de titre du jeu pour afficher ou modifier des données de jeu enregistrées.

Par défaut, seul le titre lui-même peut accéder les paramètres et les données stockées sur le service Xbox Live. Vous pouvez le modifier en configurant des stratégies d’accès dans l’espace partenaires.

> [!NOTE]
> Cette rubrique ne s’applique pas aux titres de programme Xbox Live Creators.

Ajoutez la configuration en procédant comme suit :

1. Après avoir sélectionné votre titre dans [partenaires](https://partner.microsoft.com/dashboard), accédez à **Services** > **Xbox Live**.

2. Cliquez sur le lien vers **stratégies d’accès**.

3. Cliquez sur le paramètre que vous souhaitez accorder l’accès à, puis cliquez sur le bouton d’application/service ajouter. Cela ajoutera une nouvelle ligne au bas de la liste des applications/services configuré pour accéder à ce paramètre.

4. Sélectionnez le type d’application ou service dans la zone de liste déroulante et renseignez la zone de détails pour indiquer l’application, id de titre ou id de service de l’application ou un service qui accéderont aux données.

5. Sélectionnez si l’application ou le service peut lire uniquement les données, ou si elle a un accès complet aux données.

6. Répétez pour chaque paramètre et pour chaque application ou un service qui doit accéder à ces paramètres. Vous pouvez cliquer sur **supprimer** pour supprimer une entrée.

7. Lorsque vous avez terminé, cliquez sur le **enregistrer** bouton pour enregistrer vos modifications.

![Ajouter des stratégies d’accès écran application ou service](../../images/dev-center/data-sharing-2.png)
