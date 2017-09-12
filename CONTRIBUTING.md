# <a name="contributing-to-uwp-conceptual-documentation"></a>Contribution à la documentation conceptuelle UWP

Nous vous remercions de l’intérêt que vous portez à la documentation de la plateforme Windows universelle (UWP). Les commentaires, modifications et ajouts que vous apportez à notre documentation nous sont précieux.

Cette page décrit la procédure de base à suivre pour contribuer à notre documentation pour développeurs.

## <a name="public-and-private-repos"></a>Référentiels public et privé

Les documentations conceptuelles UWP sont hébergées dans deux référentiels distincts qui sont ensuite fusionnés et mis à jour sur un seul site: le premier référentiel accepte les contributions de tous les utilisateurs, tandis que le second référentiel est uniquement destiné aux employés de Microsoft.

Si vous ne travaillez ***pas*** pour Microsoft, utilisez le [référentiel de contenu public](https://github.com/MicrosoftDocs/windows-uwp).

Si vous ***êtes*** un employé de Microsoft, vous pouvez utiliser indifféremment le référentiel public ou le [référentiel de contenu privé](https://cpubwin.visualstudio.com/_git/windows-uwp). Les employés peuvent transmettre des modifications en direct légèrement plus rapidement par le biais du référentiel privé, ou bien utiliser une [branche spécifique](https://review.docs.microsoft.com/en-us/windows-authoring-guide/uwp/conceptual/setup-local-repo-for-large-changes#what-branch-should-i-use-for-my-authoring) pour les modifications qui doivent rester dans l’ombre jusqu’à une date ultérieure.

## <a name="editing-topics-on-the-public-repo"></a>Modification d’articles dans le référentiel public

Nous nous sommes efforcés de simplifier au maximum la modification d’un fichier existant. 
- Si vous vous trouvez déjà dans le référentiel, il vous suffit d’accéder au fichier et de cliquer sur le bouton **Modifier**.  
- Dans le cas contraire, si vous êtes en train de consulter une page Docs.microsoft.com dans votre navigateur, cliquez sur le bouton **Modifier** dans la partie supérieure de la page. Vous serez alors redirigé vers le fichier source fichier Markdown approprié dans le référentiel, où vous pourrez cliquer sur le bouton **Modifier**. 

GitHub réplique automatiquement le référentiel officiel dans votre compte GitHub personnel, où vous pouvez effectuer vos modifications. Lorsque vous avez terminé, soumettez une requête de tirage à la branche «docs». Une fois que vous avez créé la requête de tirage, un membre de l’équipe de documentation UWP examinera vos modifications. Si votre requête est acceptée, les mises à jour correspondantes seront publiées sur https://docs.microsoft.com/windows/.

Vous pouvez vous familiariser avec les concepts de base de Markdown en quelques minutes seulement.  Pour commencer, consultez l’article [Mastering Markdown](https://guides.github.com/features/mastering-markdown/) (Maîtrise de Markdown).

## <a name="making-more-substantial-changes"></a>Apport de modifications plus substantielles

Pour apporter des modifications majeures à un article existant, ajouter ou modifier des images ou contribuer à un nouvel article, vous devrez créer un clone local de notre référentiel de contenu privé. Pour ce faire, suivez les [instructions de notre Guide de création Windows](https://review.docs.microsoft.com/en-us/windows-authoring-guide/uwp/conceptual/). Si vous n’avez pas encore configuré de compte GitHub ni joint à un domaine votre alias Microsoft, [commencez ici](https://review.docs.microsoft.com/en-us/windows-authoring-guide/github-account).

## <a name="using-issues-to-provide-feedback-on-uwp-conceptual-documentation"></a>Envoi de commentaires sur la documentation conceptuelle UWP en soumettant des problèmes

Si vous préférez simplement envoyer des commentaires plutôt que modifier directement des pages de documentation réelles, vous pouvez [soumettre un problème dans le référentiel public](https://github.com/MicrosoftDocs/windows-uwp/issues). Cliquez sur l’onglet «Issues» (Problèmes), puis sur le bouton **New issue** (Nouveau problème). Veillez à inclure le titre de l’article et l’URL de la page.

Les membres de l’équipe de documentation UWP examinent les problèmes soumis à intervalles réguliers et les trient, les attribuent et les traitent en conséquence.

*Dans le cas des problèmes internes, utilisez l’outil de demande de contenu WDG accessible à l’adresse [http://aka.ms/pubrequest](http://aka.ms/pubrequest). 
