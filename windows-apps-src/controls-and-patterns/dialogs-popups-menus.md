---
Description: Un menu volant est une fenêtre contextuelle légère utilisée pour afficher de manière temporaire des éléments de l’interface utilisateur qui sont liés à ce que l’utilisateur est en train de faire.
title: Menus contextuels et boîtes de dialogue
ms.assetid: 7CA2600C-A1DB-46AE-8F72-24C25E224417
label: Menus, boîtes de dialogue et fenêtres contextuelles
template: detail.hbs
---
# Menus, boîtes de dialogue et fenêtres contextuelles

Les menus, boîtes de dialogue et fenêtres contextuelles affichent des éléments temporaires d’interface utilisateur quand l’utilisateur les sollicite ou lorsqu’un événement nécessite une notification ou une approbation. 

<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [Classe MenuFlyout (XAML)](https://msdn.microsoft.com/library/windows/apps/dn299030)
-   [Classe Flyout (XAML)](https://msdn.microsoft.com/library/windows/apps/dn279496)

Un menu contextuel fournit les actions instantanées à l’utilisateur. Vous pouvez insérer des commandes personnalisées dans ce type de menu. Vous pouvez masquer un menu contextuel en appuyant ou en cliquant sur un élément à l’extérieur du menu.

Les boîtes de dialogue sont des superpositions d’interface utilisateur modales qui fournissent des informations contextuelles sur l’application. Dans la plupart des cas, les boîtes de dialogue bloquent les interactions avec la fenêtre de l’application jusqu’à ce qu’elles soient masquées explicitement. Elles demandent souvent un type d’action de l’utilisateur.

Une fenêtre contextuelle, également appelée menu volant, affiche l’interface utilisateur liée à ce que l’utilisateur fait. Elle comprend une logique de placement et de dimensionnement, et peut être utilisée pour afficher un menu, un contrôle masqué, des détails supplémentaires sur un élément, ou pour demander à l’utilisateur de confirmer une action. Vous pouvez masquer les menus contextuels en appuyant ou en cliquant sur un élément situé en dehors.


## Est-ce le contrôle approprié ?

Les menus contextuels peuvent être utilisés pour les éléments suivants :

-   Actions contextuelles sur des sélections de texte, comme Copier, Couper, Coller, Vérifier l’orthographe, etc.
-   Commandes du Presse-papiers.
-   Commandes personnalisées.
-   Commandes associées à un objet qui doivent être exécutées, mais qui ne peuvent pas être sélectionnées ni indiquées d’une autre manière.

Les boîtes de dialogue peuvent être utilisées pour les actions suivantes :

- afficher les informations importantes que l’utilisateur doit lire et accepter avant de poursuivre ;
- demander à l’utilisateur d’effectuer une action précise, ou communiquer un message important que l’utilisateur doit confirmer. Par exemple :
  - Utilisez ce type de contrôle pour indiquer à l’utilisateur toute situation d’atteinte possible à la sécurité.
  - Utilisez ce type de contrôle pour signaler à l’utilisateur qu’il s’apprête à modifier de manière irrémédiable un élément utile.
  - Utilisez ce type de contrôle pour signaler à l’utilisateur qu’il s’apprête à supprimer un élément utile.
  - Pour confirmer un achat dans l’application
- Les messages d’erreur qui s’appliquent au contexte global de l’application, liés par exemple à une erreur de connectivité.
- Utilisez une boîte de dialogue à question pour indiquer que l’application doit poser à l’utilisateur une question bloquante, parce qu’elle ne peut pas choisir telle ou telle option à la place de l’utilisateur, par exemple. Une question bloquante ne peut pas être ignorée ni mise de côté, et doit offrir à l’utilisateur des options clairement définies.

Les fenêtres contextuelles (menus volants) peuvent être utilisés pour les actions suivantes :

-   Interface utilisateur contextuelle, temporaire.
-   Avertissements et confirmations, notamment ceux qui sont liés à des actions potentiellement destructrices.
-   Affichage d’informations supplémentaires telles que des détails sur un élément à l’écran.
-   Menus de second niveau.
-   Commandes personnalisées.


## Exemples

Voici un menu contextuel à volet unique standard. Il est utilisé pour les listes courtes de commandes simples. Utilisez des séparateurs au besoin pour grouper des commandes similaires.

![Exemple de menu contextuel standard](images/controls_contextmenu_singlepane.png)

Un menu contextuel en cascade doit être utilisé pour une collection plus complète des commandes. Il présente un niveau de menu volant, et vous pouvez le faire défiler. Utilisez des séparateurs au besoin pour grouper des commandes similaires.

![Exemple de menu contextuel en cascade](images/controls_contextmenu_cascading.png)

Voici un exemple de boîte de dialogue de confirmation plein écran, à bouton unique. Avec ce type de boîte de dialogue, l’utilisateur se voit présenter une grande quantité d’informations qu’il est censé lire avant d’appuyer sur le bouton pour continuer.

![Exemple de boîte de dialogue pleine page, à bouton unique](images/controls_dialog_singlebutton.png)

Voici un exemple de boîte de dialogue à deux boutons qui présente à l’utilisateur le choix A/B. En règle générale, la quantité d’informations présentées dans cette boîte de dialogue est brève.

![Exemple de boîte de dialogue à plusieurs boutons](images/controls_dialog_twobutton.png)


## Conseils d’utilisation

**Menus contextuels**
- Utilisez un séparateur entre les groupes de commandes d’un menu contextuel pour effectuer les actions suivantes :
  - différencier des groupes de commandes connexes ;
  - regrouper des ensembles de commandes ;
  - séparer un ensemble prédictible de commandes, telles que les commandes du Presse-papiers (Couper/Copier/Coller) des commandes propres à l’application ou à l’affichage.
-   Sur les portables et ordinateurs de bureau, les info-bulles et les menus contextuels ne sont pas limités à la fenêtre de l’application et peuvent déborder de celle-ci. Si l’application essaie d’afficher un menu contextuel complètement en dehors de sa fenêtre, une exception est levée.

**Boîtes de dialogue**
-   Identifiez clairement le problème ou l’objectif de l’utilisateur dans la première ligne du texte de la boîte de dialogue.
-   Le titre de la boîte de dialogue correspond à l’instruction principale. Il est facultatif.
    -   Utilisez un titre court pour expliquer l’utilisation de cette boîte de dialogue. Les titres longs ne sont pas renvoyés à la ligne et sont tronqués.
    -   Si la boîte de dialogue est destinée à indiquer un message, une erreur ou une question simple, vous pouvez ne pas indiquer de titre. Appuyez-vous sur le texte du contenu pour fournir les informations essentielles.
    -   Assurez-que le titre est directement lié aux options des boutons.
-   Le contenu de la boîte de dialogue inclut le texte descriptif. Il est requis.
    -   Présentez le message, l’erreur ou la question bloquante aussi simplement que possible.
    -   Si vous indiquez un titre dans la boîte de dialogue, la zone de contenu doit fournir des détails supplémentaires ou clarifier la terminologie utilisée. Ne répétez pas le titre en utilisant une formulation légèrement différente.
-   Au moins un bouton de boîte de dialogue doit apparaître.
    -   Utilisez des boutons dont le texte identifie des réponses spécifiques au contenu ou à l’instruction principale. Par exemple, utilisez la question « Autorisez-vous NomApplication à accéder à votre emplacement ? », suivie des boutons Autoriser et Bloquer. Les réponses spécifiques peuvent être comprises plus rapidement, ce qui favorise une prise de décision efficace.
-   Les boîtes de dialogue d’erreur incluent un message d’erreur, ainsi que les informations pertinentes. Le seul bouton utilisé dans une boîte de dialogue d’erreur doit être du type « Fermer », ou similaire.
-   N’utilisez pas de boîtes de dialogue pour les erreurs qui sont liées à un emplacement spécifique de la page, telles que les erreurs de validation (dans les champs de mot de passe, par exemple). Utilisez plutôt le canevas de l’application afin d’afficher les erreurs insérées.

**Fenêtres contextuelles**

-   Comme pour tout autre élément d’interface utilisateur contextuelle, placez une fenêtre contextuelle à côté du point à partir duquel elle est appelée.
    -   Spécifiez l’objet auquel vous souhaitez ancrer la fenêtre contextuelle, ainsi que le côté de l’objet sur lequel la fenêtre contextuelle s’affichera.
    -   Essayez de positionner la fenêtre contextuelle afin qu’elle ne bloque pas d’éléments importants de l’interface utilisateur.
-   La fenêtre contextuelle doit être masquée dès qu’un élément qu’elle contient est sélectionné.
-   Les menus contextuels fonctionnement mieux avec un seul niveau. Les menus contextuels à plusieurs niveaux sont difficiles à parcourir et fournissent une expérience utilisateur médiocre.

## Recommandations

-   Faites en sorte que les commandes de menu contextuel soient courtes. Les commandes plus longues finissent par être tronquées.
-   Mettez une majuscule au début de chaque nom de commande.
-   Dans un menu contextuel, affichez le moins possible de commandes.
-   Si la manipulation directe d’un élément d’interface utilisateur est possible, évitez de placer cette commande dans un menu contextuel. Un menu contextuel doit être réservé pour les commandes contextuelles qui ne sont pas visibles à l’écran en conditions normales.


\[Cet article contient des informations propres aux applications de plateforme Windows universelle (UWP) et à Windows 10. Pour obtenir de l’aide concernant Windows 8.1, téléchargez le [document PDF de recommandations pour Windows 8.1](https://go.microsoft.com/fwlink/p/?linkid=258743) (en anglais).\]

## Rubriques connexes



**Pour les développeurs**
* [**Classe MenuFlyout**](https://msdn.microsoft.com/library/windows/apps/dn299030)
* [**Classe Flyout**](https://msdn.microsoft.com/library/windows/apps/dn279496)
<!--HONumber=Mar16_HO1-->
