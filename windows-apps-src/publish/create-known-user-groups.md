---
author: JnHs
Description: "Découvrez comment créer des groupes d’utilisateurs connus à utiliser pour la distribution de version d’évaluation de package et bien davantage."
title: "Créer des groupes d’utilisateurs connus"
ms.author: wdg-dev-content
ms.date: 08/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, segment, segments, groupe ciblé, clients"
ms.openlocfilehash: fc3986520e55ae0c636eb2db731df065463002b5
ms.sourcegitcommit: 6c6f3c265498d7651fcc4081c04c41fafcbaa5e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2017
---
# <a name="create-known-user-groups"></a>Créer des groupes d’utilisateurs connus

Les groupes d’utilisateurs connus vous permettent d’ajouter des personnes spécifiques à un groupe à l’aide de l’adresse e-mail associée à leur compte Microsoft. Ces groupes d’utilisateurs connus sont généralement utilisés avec des [versions d’évaluation de package](package-flights.md) pour distribuer des packages spécifiques à un groupe de personnes sélectionné. Ces groupes permettent également d’envoyer des [notifications ciblées](send-push-notifications-to-your-apps-customers.md) ou des [offres ciblées](use-targeted-offers-to-maximize-engagement-and-conversions.md) à un groupe de clients spécifiques dans le cadre de vos campagnes visant à susciter l’intérêt.

Pour être comptabilisée en tant que membre du groupe, chaque personne doit être authentifiée auprès du WindowsStore à l’aide du compte Microsoft associé à l’adresse e-mail que vous fournissez. Pour les versions d’évaluation de package, ces personnes doivent utiliser [un appareil Windows10 qui prend en charge la distribution de version d’évaluation de package](package-flights.md) afin de télécharger l’application.


## <a name="to-create-a-known-user-group"></a>Pour créer un groupe d’utilisateurs connus

1.  Dans le tableau de bord du Centre de développement Windows, développez **Engager** dans le menu de navigation de gauche, puis sélectionnez **Groupes de clients**. 
2.  Dans la section **Mes groupes de clients**, sélectionnez **Créer un groupe**.
3.  Sur la page suivante, sélectionnez la case d’option **Known user group**.
4.  Dans la zone **Nom du groupe**, entrez un nom pour votre groupe d’utilisateurs connus.
5.  Entrez les adresses e-mail des personnes que vous souhaitez ajouter au groupe. Vous devez inclure au moins une adresse e-mail, le nombre maximal d’adresses étant de 10000. Vous pouvez saisir les adresses e-mail directement dans le champ (séparées par des espaces, des virgules, des points-virgules ou des sauts de ligne) ou cliquer sur le lien **Importer au formatCSV** afin de créer le groupe de versions d’évaluation à partir d’une liste d’adresses e-mail d’un fichier.csv.
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

Notez que l’implémentation des modifications d’appartenance au groupe peut prendre jusqu’à 30minutes. Si vous ajoutez des personnes à un groupe d’utilisateurs connus après avoir publié une version d’évaluation de package pour ce groupe, les packages seront automatiquement transférés à ces personnes; vous n’êtes pas obligé de créer et publier une nouvelle soumission pour cette version d’évaluation de package. 






