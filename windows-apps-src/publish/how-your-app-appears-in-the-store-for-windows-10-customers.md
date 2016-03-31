---
Description: Si vous avez déjà publié des applications pour Windows ou Windows Phone sur le Store, elles sont également disponibles pour les clients disposant d’appareils Windows 10.
title: Apparence de votre application dans le Store pour les clients dotés de Windows 10
ms.assetid: 487BB57E-F547-4764-99B1-84FE396E6D3A
---

# Apparence de votre application dans le Store pour les clients dotés de Windows 10


Si vous avez déjà publié des applications pour Windows ou Windows Phone sur le Store, elles sont également disponibles pour les clients disposant d’appareils Windows 10. Comme le mode de présentation et de classement des applications a été modifié dans le Store pour les clients exécutant Windows 10, cette rubrique décrit ces changements.

**Remarque** Si vous voulez modifier certains détails, [Créez une nouvelle soumission](app-submissions.md), apportez vos modifications, puis soumettez la mise à jour au Store.

 

## Remarques relatives aux applications partageant des identités dans le Windows Store et dans le Windows Phone Store


Si vous avez utilisé le même nom réservé pour une application publiée dans les deux Stores (on parle également de partage d’identité de votre application), ces deux Stores considèrent qu’il existe une application, et non deux. Dans le tableau de bord, elles figurent sous la forme d’une seule application disponible en packages Windows et Windows Phone.

La plupart des développeurs définissent des prix et d’autres propriétés identiques pour l’application et tout produit in-app dans les deux Stores. Cependant, si certaines de ces valeurs diffèrent, vous devez absolument déterminer celles qui sont affichées pour les clients Windows 10.

### Tarification
Si vous avez choisi des prix de base différents pour votre application ou produit in-app dans chaque Store, le prix de base proposé dans le Windows Store a la priorité.

**Remarque** Si vous avez défini une tarification par marché dans le Windows Phone Store, les prix personnalisés s’affichent également au clients Windows 10.

### Évaluation gratuite
les options d’évaluation déféraient dans les deux anciens Stores. Ainsi, si vous les avez utilisés, il se peut que vous ayez choisi des options différentes pour chacun d’eux. L’option d’évaluation disponible pour les clients Windows 10 est déterminée d’après le tableau suivant.

| Application Windows 8       | Application Windows Phone   | Paramètre d’évaluation pour Windows 10                                                  |
|---------------------|---------------------|-------------------------------------------------------------------------------|
| Aucune version d’évaluation gratuite       | Aucune version d’évaluation gratuite       | Aucune version d’évaluation gratuite                                                                 |
| Aucune version d’évaluation gratuite       | La version d’évaluation n’expire jamais | Aucune version d’évaluation gratuite                                                                 |
| La version d’évaluation n’expire jamais | La version d’évaluation n’expire jamais | La version d’évaluation n’expire jamais                                                           |
| La version d’évaluation n’expire jamais | Aucune version d’évaluation gratuite       | Aucune version d’évaluation gratuite                                                                 |
| Version d’évaluation à durée limitée  | La version d’évaluation n’expire jamais | Aucune version d’évaluation gratuite disponible pour Windows Phone 8.1 et versions antérieures ; dans les autres cas, version d’évaluation à durée limitée |
| Version d’évaluation à durée limitée  | Aucune version d’évaluation gratuite       | Aucune version d’évaluation gratuite disponible pour Windows Phone 8.1 et versions antérieures ; dans les autres cas, version d’évaluation à durée limitée |

### Marchés
Votre application sera disponible pour les clients Windows 10, sur tous les marchés où vous avez déjà publié l’application. Cela s’applique même si vous avez sélectionné des marchés différents pour chaque Store.

### Catégories
Si votre application s’affichait dans des catégories différentes dans les deux Stores, nous utilisons la catégorie du Windows Store comme nouvelle catégorie. Remarque : certaines catégories sont différentes dans le Store pour les clients Windows 10. Veillez donc à consulter le [tableau](#cat) ci-dessous.

### Évaluation de l’âge
Si vous avez fourni des évaluations d’âge différentes, la plus stricte (l’âge le plus élevé) est utilisée.

### Politique de confidentialité
Si votre application inclut une politique de confidentialité, c’est celle que vous avez fournie lors de la soumission de votre application Windows 8 qui s’affiche pour les clients Windows 10.

### Captures d’écran
Nous récupérons toutes les captures d’écran que vous avez soumises, en affichant la version appropriée de celles-ci aux clients Windows 10, selon le type d’appareil qu’ils utilisent. Dans les rares cas où les langues prises en charge par votre application diffèrent selon le Store, certains clients peuvent voir une capture d’écran dans une autre langue, choisie de manière à représenter au mieux l’expérience dont ils profiteront lorsqu’ils achèteront l’application.

### Descriptions
Nous nous efforçons de choisir la description la plus appropriée pour les clients Windows 10 en fonction de leur langue. Lorsque des descriptions sont disponibles à partir de plusieurs sources dans la même langue, la description de votre application du Windows Store est présentée aux clients Windows 10. Dans les rares cas où vos langues prises en charge diffèrent pour chaque Store, certains clients peuvent voir une description de votre application Windows Phone, s’il s’agit de la seule description que vous avez fournie dans cette langue.

Si vous voulez mettre à jour la description visible par vos clients Windows 10 afin de leur indiquer les expériences fonctionnant sur plusieurs appareils, vous pouvez le faire en mettant à jour la [description de votre application](create-app-descriptions.md). Les clients Windows 10 voient la description par défaut de votre application, mais vous pouvez également [créer des descriptions spécifiques de la plateforme](create-platform-specific-descriptions.md) si vous souhaitez que votre description diffère en fonction de la version du système d’exploitation.

## Changements de catégorie


Dans de nombreux cas, les nouvelles [catégories et sous-catégories](category-and-subcategory-table.md) associées aux applications et aux jeux sont les mêmes que celles du Store pour les version de système d’exploitation antérieure. Toutefois, quelques modifications ont été apportées. Consultez le tableau ci-dessous pour comprendre comment votre application est classée dans le Store pour les clients Windows 10, en fonction de sa catégorie précédente.

**Remarque** Vous pouvez voir la nouvelle catégorie répertoriée dans le tableau de bord lorsque vous affichez la [catégorie de votre application](category-and-subcategory-table.md) dans la page [Propriétés de l’application](enter-app-properties.md) d’une soumission. Les clients consultant le Store sur des appareils Windows 10 voient votre application dans la nouvelle catégorie. En revanche, les clients consultant le Store à partir d’un système d’exploitation antérieur continuent de voir l’application dans sa catégorie d’origine.


**Changements de catégorie pour les applications Windows Phone :**

| Catégorie précédente                       | Nouvelle catégorie                  |
|-----------------------------------------|-------------------------------|
| Gouvernement + politique &gt; Commentaire   | Gouvernement + politique         |
| Gouvernement + politique &gt; Questions juridiques | Gouvernement + politique         |
| Gouvernement + politique &gt; Politique     | Gouvernement + politique         |
| Gouvernement + politique &gt; Resources    | Gouvernement + politique         |
| Santé + forme &gt; Alimentation + nutrition  | Santé + forme              |
| Santé + forme &gt; Forme           | Santé + forme              |
| Santé + forme &gt; Santé            | Santé + forme              |
| Loisirs &gt; Art + divertissement      | Loisirs                     |
| Loisirs &gt; Sorties              | Loisirs                     |
| Loisirs &gt; Gastronomie + restaurants            | Alimentation + cuisine                 |
| Loisirs &gt; Achats                 | Achats                      |
| Actualités + météo &gt; International       | Actualités + météo                |
| Actualités + météo &gt; Local + national    | Actualités + météo                |
| Utilitaires + productivité                | Utilitaires + outils             |
| Voyages + navigation                     | Voyages                        |
| Voyages + navigation &gt; Planification       | Voyages                        |
| Voyages + navigation  &gt; Outils          | Voyages                        |
| Voyages + navigation &gt; Déplacements avec des enfants      | Enfants + famille &gt; Voyages     |
| Voyages + navigation &gt; Langue       | Formation &gt; Langue       |
| Voyages + navigation &gt; Cartographie        | Navigation + cartes             |
| Voyages + navigation &gt; Navigation     | Navigation + cartes             |
| Jeux &gt; Classiques                     | Jeux &gt; Action + aventure |
| Jeux &gt; Famille                       | Jeux &gt; Famille + enfants      |
| Jeux &gt; Sports + loisirs          | Jeux &gt; Sports             |
| Jeux &gt; Stratégie + simulation        | Jeux &gt; Stratégie           |

 

**Changements de catégorie pour les applications Windows 8 :**

| Catégorie précédente           | Nouvelle catégorie                         |
|-----------------------------|--------------------------------------|
| Livres + référence &gt; Enfants | Enfants + famille &gt; Livres + référence |
| Musique + vidéos &gt; Vidéo   | Photo + vidéo                        |
| Musique + vidéos &gt; Musique   | Musique                                |
| Gouvernement                  | Gouvernement + politique                |
| Finance                     | Finances personnelles                     |
| Jeux &gt; Action           | Jeux &gt; Action + aventure        |
| Jeux &gt; Aventure        | Jeux &gt; Action + aventure        |
| Jeux &gt; Arcade           | Jeux &gt; Action + aventure        |
| Jeux &gt; Cartes             | Jeux &gt; Cartes + plateaux              |
| Jeux &gt; Enfants             | Jeux &gt; Famille + enfants             |
| Jeux &gt; Famille           | Jeux &gt; Famille + enfants             |
| Jeux &gt; Puzzle           | Jeux &gt; Puzzle + jeux d’esprit           |
| Jeux &gt; Courses           | Jeux &gt; Course + vol           |
<!--HONumber=Mar16_HO1-->
