---
ms.openlocfilehash: a91c080805bca5d536aad3755ca7edf052d1fe0e
ms.sourcegitcommit: b8087f8b6cf8367f8adb7d6db4581d9aa47b4861
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67414059"
---
# <a name="contributing-to-uwp-conceptual-documentation"></a>Contribution à la documentation conceptuelle UWP

Nous vous remercions de l’intérêt que vous portez à la documentation de la plateforme Windows universelle (UWP). Les commentaires, modifications et ajouts que vous apportez à notre documentation nous sont précieux.

## <a name="writing-content"></a>Écriture de contenu

Notre documentation est écrite dans Markdown, une syntaxe de style de texte légère. Si vous n’êtes pas familiarisé avec Markdown, vous pouvez [apprendre les notions de base sur GitHub](https://guides.github.com/features/mastering-markdown/). En cas doute, vous pouvez toujours copier le style de mise en forme à partir d’autres pages de notre documentation.

## <a name="public-contributions"></a>Contributions publiques

Si vous ne travaillez **pas** pour Microsoft, vous pouvez quand même apporter vos contributions via le [référentiel de contenu public](https://github.com/MicrosoftDocs/windows-uwp). Vos contributions publiques permettent d’apporter des modifications et des éclaircissements aux pages existantes.

### <a name="editing-a-file"></a>Modification d’un fichier

Si vous êtes déjà dans le référentiel de contenu public, commencez par naviguer vers le fichier que vous souhaitez modifier. De là, sélectionnez l’icône de crayon au-dessus du contenu affiché pour commencer à modifier le fichier.

Sinon, si vous êtes en train de consulter une page sur le site docs.microsoft.com dans votre navigateur, cliquez sur le bouton **Modifier** dans la partie supérieure de la page. Cette option vous redirige vers le fichier source associé dans le référentiel.

Lorsque vous commencez à modifier un fichier, GitHub réplique automatiquement le référentiel officiel dans votre compte GitHub personnel, où vous pouvez effectuer vos modifications. Lorsque vous avez terminé, soumettez une requête de tirage à la branche **docs**.

### <a name="pull-requests"></a>Requêtes de tirage

Après avoir soumis votre requête de tirage, celle-ci est évaluée par rapport à une liste de contrôle qualité du contenu pour s’assurer qu’elle répond à nos normes de base. Si la requête est satisfaisante, elle est affectée à un membre de l’équipe de documentation UWP en vue d’une vérification ultérieure. Sinon, vous êtes averti des modifications à apporter.

Les réviseurs assignés peuvent approuver ou rejeter la requête de tirage ou collaborer avec vous pour apporter d’autres modifications.

## <a name="internal-contributions"></a>Contributions internes

Si vous ne travaillez pour Microsoft, vous pouvez apporter vos contributions via le [référentiel de contenu public](https://github.com/microsoftdocs/windows-uwp-pr). Vous trouverez des conseils sur l’utilisation de ce référentiel dans le [Guide de création Windows](https://review.docs.microsoft.com/windows-authoring-guide/uwp/?branch=master). La documentation sur les fonctionnalités à venir doit être fournie via le référentiel privé uniquement.

### <a name="editing-a-file"></a>Modification d’un fichier

Comme dans le référentiel public, vous pouvez apporter de petites modifications au référentiel privé dans votre navigateur, sans avoir à créer un clone local. Vous **devez** vous assurer que vos contributions sont publiées dans la bonne branche. Pour plus d’informations sur la création d’une branche personnelle, consultez [les instructions du Guide de création Windows](https://review.docs.microsoft.com/windows-authoring-guide/uwp/conceptual/branches?branch=master).

### <a name="making-substantial-changes"></a>Apport de modifications substantielles

Pour apporter d’importantes modifications à un article existant, ajouter ou modifier des images, ou contribuer à un nouvel article, créez un clone local du référentiel de contenu privé. Pour plus d’informations, suivez les [instructions du Guide de création Windows](https://review.docs.microsoft.com/windows-authoring-guide/uwp/conceptual/).

### <a name="pull-requests"></a>Requêtes de tirage

Lorsque vous créez une requête de tirage dans le référentiel interne, assurez-vous de fusionner votre branche personnelle dans la branche à partir de laquelle elle a été créée.

Une fois que vous avez soumis votre demande de tirage (pull request), celle-ci est évaluée par [PR Merger](https://review.docs.microsoft.com/help/contribute/prmerger-overview?branch=master) pour vérifier sa conformité à nos standards de base. En cas de réussite, vous pouvez la commenter avec `#sign-off` et la transmettre à un membre de l’équipe de documentation UWP pour un passage en revue plus approfondi. En cas d’échec, vous êtes informé des changements à apporter pour permettre sa validation.

Les réviseurs assignés peuvent approuver ou rejeter la requête de tirage ou collaborer avec vous pour apporter d’autres modifications. Les réviseurs ne fusionneront pas la requête de tirage tant que vous ne l’avez pas approuvée vous-même.

## <a name="using-issues-to-provide-feedback-on-uwp-conceptual-documentation"></a>Envoi de commentaires sur la documentation conceptuelle UWP en soumettant des problèmes

Si vous souhaitez fournir des commentaires sur la documentation au lieu d’apporter vous-même des modifications, vous pouvez [créer un problème dans le référentiel public](https://github.com/MicrosoftDocs/windows-uwp/issues). Cliquez sur l’onglet **Issues** (Problèmes), puis sur le bouton **New issue** (Nouveau problème). Veillez à inclure le titre de la rubrique et l’URL de la page. Votre problème est assigné aux membres de l’équipe de documentation UWP, qui l’examineront.

* Dans le cas des problèmes internes, utilisez l’[outil de demande de contenu WDG](https://aka.ms/pubrequest).
