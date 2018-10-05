---
author: JnHs
Description: Learn how to create known user groups to use for package flighting and more.
title: Créer des groupes d’utilisateurs connus
ms.author: wdg-dev-content
ms.date: 03/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, groupe ciblé, clients, groupe de versions d’évaluation, groupes d’utilisateurs connus, utilisateurs connus
ms.localizationpriority: medium
ms.openlocfilehash: e15b5a4a2f76cbfc33db593c3110ac6f2d054b5b
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4394410"
---
# <a name="create-known-user-groups"></a>Créer des groupes d’utilisateurs connus

Les groupes d’utilisateurs connus vous permettent d’ajouter des personnes spécifiques à un groupe à l’aide de l’adresse e-mail associée à leur compte Microsoft. Ces groupes d’utilisateurs connus sont le plus souvent utilisés pour distribuer des packages spécifiques à un groupe de personnes sélectionné avec les [versions d’évaluation de package](package-flights.md) ou pour la distribution d’une soumission à un [public privé](choose-visibility-options.md#audience). Ils peuvent également être utilisés pour les campagnes visant à susciter l’intérêt, comme l’envoi de [notifications ciblées](send-push-notifications-to-your-apps-customers.md) ou d’[offres ciblées](use-targeted-offers-to-maximize-engagement-and-conversions.md) à un groupe de clients spécifiques.

Pour être comptabilisée en tant que membre du groupe, chaque personne doit être authentifiée auprès du WindowsStore à l’aide du compte Microsoft associé à l’adresse e-mail que vous fournissez. Pour télécharger l’application avec la distribution de version d’évaluation de package, les membres du groupe doivent utiliser une version de Windows10 qui prend en charge les versions d’évaluation de package (Windows.Desktop build10586 ou ultérieure, Windows.Mobile build10586.63 ou ultérieure, ou Xbox One). Avec les soumissions pour un public privé, les membres du groupe doivent utiliser Windows10, version1607 ou ultérieure (y compris XboxOne).

## <a name="to-create-a-known-user-group"></a>Pour créer un groupe d’utilisateurs connus

1. Dans le tableau de bord du Centre de développement Windows, développez **Engager** dans le menu de navigation de gauche, puis sélectionnez **Groupes de clients**. 
2. Dans la section **Mes groupes de clients**, sélectionnez **Créer un groupe**.
3. Sur la page suivante, entre un nom pour votre groupe dans la zone **Nom du groupe**.
4. Assurez-vous que la case d’option **Groupe d'utilisateurs connus** est sélectionnée.
5. Entrez les adresses e-mail des personnes que vous souhaitez ajouter au groupe. Vous devez inclure au moins une adresse e-mail, le nombre maximal d’adresses étant de 10000. Vous pouvez saisir les adresses e-mail directement dans le champ (séparées par des espaces, des virgules, des points-virgules ou des sauts de ligne) ou cliquer sur le lien **Importer au formatCSV** afin de créer le groupe de versions d’évaluation à partir d’une liste d’adresses e-mail d’un fichier.csv.
6. Sélectionnez **Enregistrer**.

Vous pouvez alors utiliser le groupe à votre convenance.

Vous pouvez également créer un groupe d’utilisateurs connus en sélectionnant **Create a flight group** sur la page de création de [version d’évaluation de package](package-flights.md). Notez que si vous effectuez cette opération, vous devrez saisir de nouveau toutes les informations que vous avez déjà fournies sur la page de création de versions d’évaluation de package.

> [!IMPORTANT]
> Lorsque vous utilisez des groupes d’utilisateurs connus avec la distribution de version d’évaluation de package, veillez à obtenir le consentement des utilisateurs ajoutés à votre groupe, et vérifiez qu’ils comprennent que les packages qui leur seront proposés seront différents de ceux de votre soumission sans version d’évaluation. 

## <a name="to-edit-a-known-user-group"></a>Pour modifier un groupe d’utilisateurs connus

Vous ne pouvez pas supprimer un groupe d’utilisateurs connus de votre tableau de bord (ou en modifier le nom) après sa création, mais vous pouvez en modifier l’appartenance à tout moment.

Pour examiner et modifier vos groupes d’utilisateurs connus, développez le menu **Engager** dans le menu de navigation de gauche, puis sélectionnez **Groupes de clients**. Sous **Mes groupes de clients**, sélectionnez le nom du groupe que vous souhaitez modifier. Vous pouvez également modifier un groupe d’utilisateurs connus de la page de création de version d’évaluation de package en sélectionnant **Afficher et gérer les groupes existants** lorsque vous créez une version d’évaluation, ou en sélectionnant le nom du groupe sur la page de présentation d’une version d’évaluation de package. 

Une fois que vous avez sélectionné le groupe à modifier, vous pouvez ajouter ou supprimer des adresses e-mail directement dans le champ.

Si vous souhaitez apporter des modifications plus importantes, sélectionnez **Exporter au formatCSV** pour enregistrer vos informations d’appartenance au groupe dans un fichier.csv. Effectuez vos modifications dans ce fichier, puis cliquez sur **Importer au formatCSV** pour utiliser la nouvelle version afin de mettre à jour l’appartenance au groupe.

Notez que l’implémentation des modifications d’appartenance au groupe peut prendre jusqu’à 30minutes. Vous n’avez pas besoin de publier une nouvelle soumission pour que les nouveaux membres du groupe puissent accéder à votre soumission par le biais de versions d’évaluation de package ou du public privé; ils y auront accès dès que les modifications seront appliquées. 






