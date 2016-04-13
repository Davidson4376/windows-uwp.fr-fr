---
Dans une application de plateforme Windows universelle (UWP), les éléments de commande sont les éléments d’interface utilisateur interactifs qui permettent à l’utilisateur d’effectuer des actions telles que l’envoi d’un message électronique, la suppression d’un élément ou l’envoi d’un formulaire.
Informations de base relatives à la conception des commandes pour les applications de plateforme Windows universelle (UWP)
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
Informations de base sur la conception des commandes
template: detail.hbs
---

#  Informations de base relatives à la conception des commandes pour les applications UWP


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Dans une application de plateforme Windows universelle (UWP), les *éléments de commande* sont les éléments d’interface utilisateur interactifs qui permettent à l’utilisateur d’effectuer des actions telles que l’envoi d’un message électronique, la suppression d’un élément ou l’envoi d’un formulaire. Cet article décrit les éléments de commande, comme les boutons et les cases à cocher, les interactions qu’ils prennent en charge, ainsi que les surfaces de commandes (telles que les barres de commandes et les menus contextuels) pouvant les accueillir.

## <span id="Provide_the_right_type_of_interactions"> </span> <span id="provide_the_right_type_of_interactions"> </span> <span id="PROVIDE_THE_RIGHT_TYPE_OF_INTERACTIONS"> </span>Fournissez le type approprié d’interactions


Lorsque vous créez une interface de commande, la décision la plus importante consiste à choisir ce que les utilisateurs auront la possibilité de faire. Par exemple, si vous créez une application de retouche photo, l’utilisateur aura besoin d’outils pour modifier ses photos. Toutefois, si vous créez une application de réseau social qui affiche des photos, la retouche d’images peut ne pas être une priorité. Les outils d’édition peuvent donc être omis pour économiser de l’espace. Choisissez ce que les utilisateurs pourront accomplir et fournissez-leur les outils adéquats.

Pour obtenir des recommandations sur la planification des interactions appropriées pour votre application, voir [Planifier votre application](https://msdn.microsoft.com/library/windows/apps/hh465427.aspx).

## <span id="Use_the_right_command_element_for_the_interaction"> </span> <span id="use_the_right_command_element_for_the_interaction"> </span> <span id="USE_THE_RIGHT_COMMAND_ELEMENT_FOR_THE_INTERACTION"> </span>Utilisez l’élément de commande approprié pour l’interaction


Utiliser les éléments appropriés pour les interactions appropriées peut faire toute la différence entre une application intuitive et une application dont l’utilisation est complexe ou déroutante. La plateforme Windows universelle (UWP) fournit un large éventail d’éléments de commande, sous la forme de contrôles, que vous pouvez utiliser dans votre application. Le tableau suivant répertorie quelques-uns des contrôles les plus courants et offre un résumé des interactions qu’ils permettent.

| Catégorie              | Éléments                                                                                                                                                                                                            | Interaction                                                                                                                                        |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| Boutons               | [Bouton](https://msdn.microsoft.com/library/windows/apps/hh465470)                                                                                                                                                     | Déclenche une action immédiate, telle que l’envoi d’un message électronique, la confirmation d’une action dans une boîte de dialogue ou l’envoi des données d’un formulaire.                                    |
| Sélecteurs de date et d’heure | [sélecteur de date de calendrier, affichage de calendrier, sélecteur de date, sélecteur d’heure](https://msdn.microsoft.com/library/windows/apps/hh465466)                                                                                                                 | Permet à l’utilisateur d’afficher et de modifier les informations de date et d’heure, notamment lors de l’entrée de la date d’expiration d’une carte de crédit ou de la configuration d’une alarme.                   |
| Listes                 | [Liste déroulante, zone de liste, affichage Liste et affichage Grille](https://msdn.microsoft.com/library/windows/apps/mt186889)                                                                                                                                              | Présente les éléments dans une liste ou une grille interactives. Utilisez ces éléments pour permettre aux utilisateurs de sélectionner un film parmi une liste de nouveautés ou de gérer un inventaire. |
| Saisie de texte prédictive | [Zone de suggestion automatique](https://msdn.microsoft.com/library/windows/apps/dn997762)                                                                                                                                                                    | Fait gagner du temps aux utilisateurs quand ils entrent des données ou effectuent des requêtes en leur fournissant des suggestions au fur et à mesure de la saisie.                                                   |
| Contrôles de sélection    | [Case à cocher](https://msdn.microsoft.com/library/windows/apps/hh700393), [case d’option](https://msdn.microsoft.com/library/windows/apps/hh700395), [bouton bascule](https://msdn.microsoft.com/library/windows/apps/hh465475) | Permet à l’utilisateur de choisir entre différentes options, par exemple lors du remplissage d’un questionnaire ou de la configuration des paramètres d’application.                                      |

 

Pour en obtenir la liste complète, voir [Contrôles et éléments d’interface utilisateur](https://dev.windows.com/design/controls-patterns)

## <span id="_________Place_commands_on_the_right_surface_______"> </span> <span id="_________place_commands_on_the_right_surface_______"> </span> <span id="_________PLACE_COMMANDS_ON_THE_RIGHT_SURFACE_______"> </span>Placez les commandes sur la surface appropriée


Vous pouvez placer les éléments de commande sur un certain nombre de surfaces dans votre application, notamment le Canvas (zone de contenu de votre application) ou des éléments de commande spéciaux pouvant servir de conteneurs de commandes, telles que des barres de commandes, des menus, des boîtes de dialogue et des menus volants. Voici quelques recommandations générales pour placer les commandes :

-   Laissez les utilisateurs manipuler directement, autant que possible, le contenu sur le Canvas de l’application, au lieu d’ajouter des commandes qui agissent sur le contenu. Par exemple, dans l’application de voyage, permettez aux utilisateurs de réorganiser leur itinéraire en faisant glisser et en déplaçant des activités d’une liste de la zone de dessin, plutôt qu’en sélectionnant l’activité et en utilisant les boutons haut et bas de la barre de commandes.
-   Si les utilisateurs ne peuvent pas manipuler le contenu directement, vous pouvez placer les commandes sur l’une de ces surfaces d’interface utilisateur :

    -   Dans la [barre de commandes](https://msdn.microsoft.com/library/windows/apps/hh465302) : nous vous recommandons de placer la plupart des commandes sur la barre de commandes pour faciliter leur organisation et leur accès.
    -   Sur la zone de dessin de l’application : si l’utilisateur consulte une page ou une vue avec un objet unique, vous pouvez placer les commandes adéquates directement sur cette zone. Il doit y avoir très peu de commandes de ce type.
    -   Dans un [menu contextuel](https://msdn.microsoft.com/library/windows/apps/hh465308) : vous pouvez utiliser des menus contextuels pour les actions du Presse-papiers (comme couper, copier et coller), ou pour les commandes qui concernent le contenu qui ne peut pas être sélectionné (ajouter une punaise pour localiser un lieu sur une carte, par exemple).

Voici une liste des surfaces de commande offertes par Windows, ainsi que des recommandations pour en tirer le meilleur parti.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Surface</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Canvas (zone de contenu) de l’application
<p><img src="images/content-area.png" alt="The content area of an app" /></p></td>
<td align="left"><p>Si une commande est fondamentale et est requise en permanence pour permettre à l’utilisateur d’effectuer les actions de base, placez-la sur le Canvas (zone de contenu de votre application). Comme vous pouvez placer les commandes près des objets qu’elles affectent (ou sur ceux-ci), le fait de les placer sur le Canvas rend leur utilisation facile et évidente.</p>
<p>Toutefois, choisissez les commandes que vous placez sur le Canvas avec soin. S’il y en a trop, vous perdrez de l’espace précieux sur l’écran et risquez de submerger l’utilisateur. Si la commande n’est pas vouée à être fréquemment utilisée, envisagez de la placer sur une autre surface de commande, telle qu’un menu ou la zone &quot;Plus&quot; de la barre de commandes.</p></td>
</tr>
<tr class="even">
<td align="left">[Command bar](https://msdn.microsoft.com/library/windows/apps/hh465302)
<p><img src="images/controls-appbar-icons-200.png" alt="Example of a command bar with icons" /></p></td>
<td align="left"><p>Les barres de commandes permettent aux utilisateurs d’accéder facilement aux actions. Utilisez une barre de commandes pour afficher les commandes et les options propres au contexte de l’utilisateur, par exemple celles relatives à une sélection de photos ou au mode dessin.</p>
<p>Les barres de commandes peuvent être placées en haut de l’écran et/ou au bas de l’écran. Cette conception d’une application de retouche photo montre la zone de contenu et la barre de commandes :</p>
<p><img src="images/commands-appcanvas-example.png" alt="A photo app" /></p>
<p>Pour plus d’informations sur les barres de commandes, voir l’article [Guidelines for command bar](https://msdn.microsoft.com/library/windows/apps/hh465302).</p></td>
</tr>
<tr class="odd">
<td align="left">[Menus and context menus](../controls-and-patterns/dialogs-popups-menus.md)
<p><img src="images/controls-contextmenu-singlepane.png" alt="Example of a single-pane context menu" /></p></td>
<td align="left"><p>Parfois, il est plus efficace de regrouper plusieurs commandes dans un menu de commandes. Les menus permettent de proposer davantage d’options dans moins d’espace. Les menus peuvent comprendre des contrôles interactifs.</p>
<p>Les menus contextuels peuvent fournir des raccourcis pour les actions courantes et un accès aux commandes secondaires qui ne sont pertinentes que dans certains contextes.</p>
<p>Les menus contextuels sont destinés aux types de commande et aux scénarios de commandes suivants :</p>
<ul>
<li>Les actions contextuelles sur des parties de texte, comme Copier, Couper, Coller, Vérifier l’orthographe, etc.</li>
<li>Les commandes associées à un objet qui doivent être exécutées, qui ne peuvent pas être sélectionnées ou indiquées d’une autre manière.</li>
<li>L’affichage des commandes du Presse-papiers.</li>
<li>Les commandes personnalisées.</li>
</ul>
<p>Cet exemple illustre la conception d’une application de plan de métro qui utilise un menu contextuel pour modifier l’itinéraire, marquer un itinéraire ou sélectionner un autre métro.</p>
<p><img src="images/subway/uap-subway-ak-8in-dashboard-200.png" alt="A context menu in an subway app" /></p>
<p>Pour plus d’informations sur les menus contextuels, voir l’article [Guidelines for context menu](https://msdn.microsoft.com/library/windows/apps/hh465308).</p></td>
</tr>
<tr class="even">
<td align="left">[Dialog controls](../controls-and-patterns/dialogs-popups-menus.md)
<p><img src="images/controls-dialog-twobutton-200.png" alt="Example of a simple two-button dialog" /></p></td>
<td align="left"><p>Les boîtes de dialogue sont des superpositions d’interface utilisateur modales qui fournissent des informations contextuelles sur l’application. Dans la plupart des cas, les boîtes de dialogue bloquent les interactions avec la fenêtre de l’application jusqu’à ce qu’elles soient masquées explicitement. Elles demandent souvent un type d’action de l’utilisateur.</p>
<p>Les boîtes de dialogue peuvent nuire à la fluidité de l’expérience et ne doivent être utilisées que dans certaines situations. Pour plus d’informations, voir la section dans [When to confirm or undo actions](#whentoconfirm).</p></td>
</tr>
<tr class="odd">
<td align="left">[Flyout](../controls-and-patterns/dialogs-popups-menus.md)
<p><img src="images/controls-flyout-default-200.png" alt="Image of default flyout" /></p></td>
<td align="left"><p>Fenêtre contextuelle légère qui affiche l’interface utilisateur liée à ce que l’utilisateur fait. Utilisez un menu volant pour :</p>
<p></p>
<ul>
<li>Afficher un menu.</li>
<li>Afficher plus de détails sur un élément.</li>
<li>Demander à l’utilisateur de confirmer une action sans bloquer l’interaction avec l’application.</li>
</ul>
<p>Vous pouvez masquer les menus volants en appuyant ou en cliquant sur un élément hors du menu volant. Pour plus d’informations sur les menus volants, voir l’article [Dialogs, menus, and popups](../controls-and-patterns/dialogs-popups-menus.md).</p></td>
</tr>
</tbody>
</table>

 

## <span id="whentoconfirm"> </span> <span id="WHENTOCONFIRM"> </span>Quand confirmer ou annuler des actions


Même si l’interface utilisateur est parfaitement pensée et que l’utilisateur fait preuve de la plus grande précaution, il arrivera inévitablement à celui-ci d’effectuer une action non souhaitée. Votre application peut remédier à ce genre de situation en demandant à l’utilisateur de confirmer une action ou en lui offrant un moyen d’annuler les actions récentes.

-   Pour les actions qui ne peuvent pas être annulées et dont les conséquences sont importantes, nous vous recommandons d’utiliser une boîte de dialogue de confirmation. Exemples de telles actions :
    -   Remplacement d’un fichier
    -   Non-enregistrement d’un fichier avant la fermeture
    -   Confirmation de la suppression définitive d’un fichier ou de données
    -   Achat (sauf si l’utilisateur refuse de demander une confirmation)
    -   Envoi d’un formulaire, par exemple pour s’inscrire à un programme
-   Pour les actions qui peuvent être annulées, une simple commande d’annulation est généralement suffisante. Exemples de telles actions :
    -   Suppression d’un fichier
    -   Suppression d’un e-mail (pas définitivement)
    -   Modification du contenu ou d’un texte
    -   Changement de nom d’un fichier

**Conseil** Prenez garde à ne pas utiliser un trop grand nombre de boîtes de dialogue de confirmation dans votre application : elles peuvent être très utiles en cas d’erreur de l’utilisateur, mais elles représentent un obstacle chaque fois que l’utilisateur tente d’effectuer une action intentionnellement.

 

## <span id="_________Optimize_for_specific_input_types_______"> </span> <span id="_________optimize_for_specific_input_types_______"> </span> <span id="_________OPTIMIZE_FOR_SPECIFIC_INPUT_TYPES_______"> </span> Optimiser les types d’entrée spécifiques


Voir les [Notions fondamentales sur les interactions](../input-and-devices/input-primer.md) pour plus d’informations sur l’optimisation des expériences utilisateur avec un type ou un périphérique d’entrée spécifique.


\[Cet article contient des informations propres aux applications UWP et à Windows 10. Pour obtenir de l’aide concernant Windows 8.1, téléchargez le [document PDF de recommandations pour Windows 8.1](https://go.microsoft.com/fwlink/p/?linkid=258743) (en anglais).\]

 

 






<!--HONumber=Mar16_HO1-->


