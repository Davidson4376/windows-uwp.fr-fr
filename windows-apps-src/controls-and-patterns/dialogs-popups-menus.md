---
author: Jwmsft
Description: "Un menu volant est une fenêtre contextuelle légère utilisée pour afficher de manière temporaire des éléments de l’interface utilisateur qui sont liés à ce que l’utilisateur est en train de faire."
title: "Menus contextuels et boîtes de dialogue"
ms.assetid: 7CA2600C-A1DB-46AE-8F72-24C25E224417
label: Menus, dialogs, and popups
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: c183f7390c5b4f99cf0f31426c1431066e1bc96d
ms.openlocfilehash: e268a5facebbdb80d7cc5cdd52c1a6f944ef7d00

---
# Menus, boîtes de dialogue, menus volants et fenêtres contextuelles

Les menus, boîtes de dialogue, menus volants et fenêtres contextuelles affichent des éléments temporaires d’interface utilisateur quand l’utilisateur les sollicite ou lorsqu’un événement nécessite une notification ou une approbation.

<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [Classe MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030)
-   [Classe Flyout](https://msdn.microsoft.com/library/windows/apps/dn279496)
-   [Classe ContentDialog](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)

Un menu contextuel fournit les actions instantanées à l’utilisateur. Vous pouvez insérer des commandes texte dans ce type de menu. Vous pouvez abandonner interactivement un menu contextuel en appuyant ou en cliquant sur un élément à l’extérieur du menu.

Les boîtes de dialogue sont des superpositions d’interface utilisateur modales qui fournissent des informations contextuelles sur l’application. Les boîtes de dialogue bloquent les interactions avec la fenêtre de l’application jusqu’à ce qu’elles soient masquées explicitement. Elles exigent souvent une forme d’action de la part de l’utilisateur.

Un menu volant est une fenêtre contextuelle légère qui affiche l’interface utilisateur liée à ce que l’utilisateur fait. Elle comprend une logique de placement et de dimensionnement, et peut être utilisée pour afficher un contrôle masqué, des détails supplémentaires sur un élément, ou pour demander à l’utilisateur de confirmer une action. Vous pouvez abandonner interactivement les menus volants en appuyant ou en cliquant sur un élément hors du menu contextuel.


## Est-ce le contrôle approprié?

Les menus contextuels peuvent être utilisés pour les éléments suivants:

-   Actions contextuelles.
-   Commandes associées à un objet qui doivent être exécutées, mais qui ne peuvent pas être sélectionnées.

Les boîtes de dialogue peuvent être utilisées pour les actions suivantes:

- afficher les informations importantes que l’utilisateur doit lire et accepter avant de poursuivre;
- demander à l’utilisateur d’effectuer une action précise, ou communiquer un message important que l’utilisateur doit confirmer. Par exemple:
  - Utilisez ce type de contrôle pour indiquer à l’utilisateur toute situation d’atteinte possible à la sécurité.
  - Utilisez ce type de contrôle pour signaler à l’utilisateur qu’il s’apprête à modifier de manière irrémédiable un élément utile.
  - Utilisez ce type de contrôle pour signaler à l’utilisateur qu’il s’apprête à supprimer un élément utile.
  - Pour confirmer un achat dans l’application
- Les messages d’erreur qui s’appliquent au contexte global de l’application, liés par exemple à une erreur de connectivité.
- Utilisez une boîte de dialogue à question pour indiquer que l’application doit poser à l’utilisateur une question bloquante, parce qu’elle ne peut pas choisir telle ou telle option à la place de l’utilisateur, par exemple. Une question bloquante ne peut pas être ignorée ni mise de côté, et doit offrir à l’utilisateur des options clairement définies.

Les menus volants peuvent être utilisés pour les éléments suivants:

-   Interface utilisateur contextuelle, temporaire.
-   Avertissements et confirmations, notamment ceux qui sont liés à des actions potentiellement destructrices.
-   Affichage d’informations supplémentaires, comme des détails ou des descriptions plus longues sur un élément de la page.


## Exemples

Voici un menu contextuel standard à volet unique avec une courte liste de commandes simples. Il est possible de le faire défiler si nécessaire. Utilisez des séparateurs au besoin pour grouper des commandes similaires.

![Exemple de menu contextuel standard](images/controls_contextmenu_singlepane.png)

Un menu contextuel en cascade peut être utilisé pour une collection plus complète des commandes. Il présente plusieurs niveaux de menu volant, et vous pouvez le faire défiler. Utilisez des séparateurs au besoin pour grouper des commandes similaires.

![Exemple de menu contextuel en cascade](images/controls_contextmenu_cascading.png)

Voici un exemple de boîte de dialogue de confirmation plein écran, à bouton unique. Avec ce type de boîte de dialogue, l’utilisateur se voit présenter une grande quantité d’informations qu’il est censé lire avant d’appuyer sur le bouton pour continuer.

![Exemple de boîte de dialogue pleine page, à bouton unique](images/controls_dialog_singlebutton.png)

Voici un exemple de boîte de dialogue à deux boutons qui présente à l’utilisateur le choix A/B. En règle générale, la quantité d’informations présentées dans cette boîte de dialogue est brève.

![Exemple de boîte de dialogue à plusieurs boutons](images/controls_dialog_twobutton.png)

## Boîte de dialogue modale et abandon interactif

Les boîtes de dialogue sont modales, ce qui signifie qu’elles bloquent toutes les interactions avec l’application jusqu'à ce que l’utilisateur y sélectionne un bouton. Pour renforcer visuellement leur comportement modal, les boîtes de dialogue dessinent une couche de superposition qui masque partiellement l’interface utilisateur de l’application temporairement inaccessible.

**Remarque** Lorsque l’une des options disponibles de la boîte de dialogue est Annuler, les applications peuvent choisir de laisser les utilisateurs masquer la boîte de dialogue en appuyant sur la touche ÉCHAP. Ce comportement n’est pas intégré au contrôle mais est un raccourci couramment implémenté.

Les menus volants et les menus contextuels sont des contrôles d’abandon interactif, ce qui signifie que les utilisateurs peuvent choisir parmi différentes actions pour masquer rapidement des interfaces utilisateur temporaires. Ces interactions sont destinées à être légères et non bloquantes. Les actions d’abandon interactif incluent
- Cliquer ou appuyer en dehors de l’interface utilisateur temporaire
- Appuyer sur la touche ÉCHAP
- Appuyer sur le bouton RETOUR
- Redimensionner la fenêtre de l’application
- Changer l’orientation de l’appareil


## Instructions d’utilisation de la boîte de dialogue

-   Identifiez clairement le problème ou l’objectif de l’utilisateur dans la première ligne du texte de la boîte de dialogue.
-   Le titre de la boîte de dialogue correspond à l’instruction principale. Il est facultatif.
    -   Utilisez un titre court pour expliquer l’utilisation de cette boîte de dialogue. Les titres longs ne sont pas renvoyés à la ligne et sont tronqués.
    -   Si la boîte de dialogue est destinée à indiquer un message, une erreur ou une question simple, vous pouvez ne pas indiquer de titre. Appuyez-vous sur le texte du contenu pour fournir les informations essentielles.
    -   Assurez-vous que le titre est directement lié aux options des boutons.
-   Le contenu de la boîte de dialogue inclut le texte descriptif. Il est requis.
    -   Présentez le message, l’erreur ou la question bloquante aussi simplement que possible.
    -   Si vous indiquez un titre dans la boîte de dialogue, la zone de contenu doit fournir des détails supplémentaires ou clarifier la terminologie utilisée. Ne répétez pas le titre en utilisant une formulation légèrement différente.
-   Au moins un bouton de boîte de dialogue doit apparaître.
    -   Les boutons sont le seul mécanisme permettant aux utilisateurs d’abandonner la boîte de dialogue.
    -   Utilisez des boutons dont le texte identifie des réponses spécifiques au contenu ou à l’instruction principale. Par exemple, utilisez la question « Autorisez-vous NomApplication à accéder à votre emplacement ? », suivie des boutons Autoriser et Bloquer. Les réponses spécifiques peuvent être comprises plus rapidement, ce qui favorise une prise de décision efficace.
-   Les boîtes de dialogue d’erreur incluent un message d’erreur, ainsi que les informations pertinentes. Le seul bouton utilisé dans une boîte de dialogue d’erreur doit être du type « Fermer », ou similaire.
-   N’utilisez pas de boîtes de dialogue pour les erreurs qui sont liées à un emplacement spécifique de la page, telles que les erreurs de validation (dans les champs de mot de passe, par exemple). Utilisez plutôt le canevas de l’application afin d’afficher les erreurs insérées.

## Menus contextuels et menus volants

Les menus volants et les menus contextuels sont des contrôles étroitement liés qui partagent des comportements d’interaction. La principale différence entre ces contrôles est le type de contenu qu’ils acceptent.

### MenuFlyout
Un menu contextuel, implémenté avec la classe MenuFlyout, peut contenir [**MenuFlyoutItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyoutitem.aspx), [**ToggleMenuFlyoutItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.togglemenuflyoutitem.aspx), [**MenuFlyoutSubItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyoutsubitem.aspx) et [**MenuFlyoutSeparator**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyoutseparator.aspx). Pour afficher un autre type d’interface utilisateur, utilisez Flyout.

- **Instructions d’utilisation**
  - Utilisez un séparateur entre les groupes de commandes d’un menu contextuel pour effectuer les actions suivantes:
    - différencier des groupes de commandes connexes ;
    - regrouper des ensembles de commandes ;
    - séparer un ensemble prédictible de commandes, telles que les commandes du Presse-papiers (Couper/Copier/Coller) des commandes propres à l’application ou à l’affichage.
  -   Sur les portables et ordinateurs de bureau, les info-bulles et les menus contextuels ne sont pas limités à la fenêtre de l’application et peuvent déborder partiellement de celle-ci. Si l’application essaie d’afficher un menu contextuel complètement en dehors de sa fenêtre, une exception est levée.

- **Pratiques conseillées et déconseillées**
  -   Faites en sorte que les commandes de menu contextuel soient courtes. Les commandes plus longues finissent par être tronquées.
  -   Mettez une majuscule au début de chaque nom de commande.
  -   Dans un menu contextuel, affichez le moins possible de commandes.
  -   Si la manipulation directe d’un élément d’interface utilisateur est possible, évitez de placer cette commande dans un menu contextuel. Un menu contextuel doit être réservé pour les commandes contextuelles qui ne sont pas visibles à l’écran en conditions normales.

### Menu volant

Un menu volant est un conteneur ouvert qui peut afficher l’interface utilisateur arbitraire comme étant son contenu.  Les menus volants n’ont pas leurs propres parties visuelles, ils sont simplement des contrôles de contenu. Les menus volants comportent une marge et des barres de défilement facultatives qu’ils ajoutent autour de leur contenu. Pour appliquer un style à un menu volant, modifiez son `FlyoutPresenterStyle`.

Le code suivant affiche un paragraphe d’habillage de texte et rend le bloc de texte accessible à un lecteur d’écran.

````xaml
<Flyout>
  <Flyout.FlyoutPresenterStyle>
    <Style TargetType="FlyoutPresenter">
      <Setter Property="ScrollViewer.HorizontalScrollMode" Value="Disabled"/>
      <Setter Property="ScrollViewer.HorizontalScrollBarVisibility" Value="Disabled"/>
      <Setter Property="IsTabStop" Value="True"/>
      <Setter Property="TabNavigation" Value="Cycle"/>
    </Style>
  </Flyout.FlyoutPresenterStyle>
  <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."/>
</Flyout>
````

### Appel et placement

Les menus volants et les menus contextuels sont attachés à des contrôles spécifiques. Lorsqu’ils sont visibles, ils doivent être ancrés à l’objet appelant et spécifier leur position relative à l’objet privilégiée: haut, gauche, bas ou droite. Le menu volant dispose également d’un mode de placement complet qui tente d’étirer le menu volant et de le centrer à l’intérieur de la fenêtre d’application.

La [classe Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx) inclut une propriété `Flyout` qui vous permet de spécifier l’interface utilisateur temporaire qui s’ouvrira quand l’utilisateur cliquera ou appuiera sur le bouton.

````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="Yay!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````

Pour ouvrir un menu contextuel, les utilisateurs peuvent effectuer l’une des actions suivantes:
- Effectuer un clic droit avec la souris
- Appuyer longuement sur l’interface tactile
- Taper Maj + F10
- Appuyer sur la touche de menu du clavier
- Appuyer sur le bouton de menu du boîtier de commande

Pour ouvrir facilement un menu volant ou un menu contextuel en réponse à l’une des actions ci-dessus, les applications peuvent tirer parti de la nouvelle propriété [`ContextFlyout`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) sur [UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.aspx), la classe de base de la plupart des contrôles.

````xaml
<Rectangle Height="100" Width="100" Fill="Red">
  <Rectangle.ContextFlyout>
     <MenuFlyout>
        <MenuFlyoutItem Text="Close"/>
     </MenuFlyout>
  </Rectangle.Flyout>
</Rectangle>
````

## Articles connexes

**Pour les développeurs**
- [**Classe MenuFlyout**](https://msdn.microsoft.com/library/windows/apps/dn299030)
- [**Classe Flyout**](https://msdn.microsoft.com/library/windows/apps/dn279496)
- [**Classe ContentDialog**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)



<!--HONumber=Jun16_HO4-->


