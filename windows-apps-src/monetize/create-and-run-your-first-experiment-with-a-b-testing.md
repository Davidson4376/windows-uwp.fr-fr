---
author: mcleanbyron
Description: Dans cette procédure pas à pas, vous découvrirez comment créer et exécuter votre première expérience avec des tests A/B
title: Créer et exécuter votre première expérience avec des tests A/B
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
---

# Créer et exécuter votre première expérience avec des tests A/B

Dans cette procédure pas à pas, vous allez &#58;
* créer une expérience sur le tableau de bord du Centre de développement Windows qui observe si le changement de la couleur d’arrière-plan d’un bouton d’application augmente le nombre de clics sur le bouton en question ;
* créer une application qui récupère les paramètres de variante à partir du Centre de développement, puis utilise ces données pour modifier la couleur d’arrière-plan d’un bouton et consigne les données des événements d’affichage et de conversion dans le Centre de développement ;
* exécuter l’application pour collecter des données d’expérience ;
* passer en revue les résultats de l’expérience sur le tableau de bord du Centre de développement, choisir une variante à activer pour tous les utilisateurs de l’application, puis terminer l’expérience.

Pour une vue d’ensemble des tests A/B avec le Centre de développement, voir [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md).

## Prérequis

Pour suivre cette procédure pas à pas, vous devez posséder un compte du Centre de développement Windows et devez configurer votre ordinateur de développement comme décrit dans [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md).

## Créer l’expérience dans le Centre de développement Windows

1. Connectez-vous au [tableau de bord du Centre de développement](https://dev.windows.com/overview).
2. Si vous disposez déjà d’une application dans le Centre de développement que vous souhaitez utiliser pour créer une expérience, sélectionnez l’application en question sous **Vos applications**. Si vous ne disposez pas encore d’une application dans votre tableau de bord, [créez une application en réservant un nom](../publish/create-your-app-by-reserving-a-name.md), puis sélectionnez cette application dans votre tableau de bord.
3. Dans le volet de navigation, cliquez sur **Services**, puis sur **Expérimentation**.
4. Dans la section **Clés API**, sélectionnez **Nouvelle clé API** pour générer une nouvelle clé API, puis entrez le nom **My First Experiment** pour la clé en question. Vous utiliserez cette clé API dans la section suivante de cette procédure pas à pas.
5. Dans la section **Expériences**, cliquez sur **Nouvelle expérience**. Dans le champ **Nom d’expérience**, tapez le nom **Optimize Button Clicks**.
6. Dans le champ **Nom d’événement d’affichage**, tapez le nom **userViewedButton**. Plus loin dans cette procédure pas à pas, vous ajouterez du code qui consigne cet événement d’affichage lorsque la page principale de votre application est initialisée et que le bouton est visible à l’utilisateur.
7. Dans la section **Objectifs et événements de conversion**, entrez les valeurs suivantes :
  * Dans le champ **Nom d’objectif**, tapez **Increase Button Clicks**.
  * Dans le champ **Nom de l’événement de conversion**, tapez le nom **userClickedButton**. Plus loin dans cette procédure pas à pas, vous ajouterez du code qui consigne cet événement de conversion dans le gestionnaire d’événements Click pour le bouton en question.
  * Dans le champ **Objectif**, choisissez **Agrandir**.
8. Dans la section **Variantes et paramètres**, cliquez trois fois sur **Ajouter un paramètre**. Vous devez à présent avoir quatre lignes de paramètres vides.
  * Dans la première ligne, tapez **buttonText** pour le nom du paramètre, **Grey Button** dans la colonne **Variante A** et **Blue Button** dans la colonne **Variante B**.
  * Dans la seconde ligne, tapez **r** pour le nom du paramètre, **128** dans la colonne **Variante A** et **1** dans la colonne **Variante B**.
  * Dans la troisième ligne, tapez **g** pour le nom du paramètre, **128** dans la colonne **Variante A** et **1** dans la colonne **Variante B**.
  * Dans la quatrième ligne, tapez **b** pour le nom du paramètre, **128** dans la colonne **Variante A** et **255** dans la colonne **Variante B**.  
9. Vérifiez que la case **Répartir en valeurs égales** est cochée pour que les variantes soient distribuées équitablement à votre application.
10. Cliquez sur **Enregistrer**, puis sur **Activer**.

> **Important** Lorsque vous activez une expérience, vous ne pouvez plus modifier les paramètres de l’expérience, sauf s’il s’agit d’une expérience de test (vous avez coché la case **Expérience de test** lorsque vous avez créé l’expérience). D’habitude, nous vous recommandons de coder l’expérience dans votre application avant d’activer votre expérience. Pour plus de simplicité dans cette procédure pas à pas, vous pouvez activer l’expérience dès à présent.

## Coder l’expérience dans votre application

1. Dans Visual Studio 2015, créez un projet de plateforme Windows universelle à l’aide de Visual C#. Nommez le projet **SampleExperiment**.
2. Dans l’Explorateur de solutions, développez votre nœud de projet, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence**.
3. Dans le **Gestionnaire de références**, développez **Windows universel** et cliquez sur **Extensions**.
4. Dans la liste des Kits de développement logiciel (SDK), cochez la case en regard de **Kit de développement logiciel (SDK) d’engagement de la Boutique Microsoft** et cliquez sur **OK**.
5. Dans l’**Explorateur de solutions**, double-cliquez sur MainPage.xaml pour ouvrir le concepteur pour la page principale de l’application.
6. Faites glisser un **Bouton** de la **Boîte à outils** vers la page.
7. Double-cliquez sur le bouton dans le concepteur pour ouvrir le fichier de code et ajoutez un gestionnaire d’événements pour l’événement **Click**.  
8. Remplacez l’ensemble du contenu du fichier de code par le code suivant :

  ```CSharp
  using System;
  using Windows.UI.Xaml;
  using Windows.UI.Xaml.Controls;
  using Windows.UI.Xaml.Media;
  using System.Threading.Tasks;
  using Windows.UI;
  using Windows.UI.Core;

  // Namespace for A/B testing.
  using Microsoft.Services.Store.Engagement;

  namespace SampleExperiment
  {  
    public sealed partial class MainPage : Page
    {
        private readonly ExperimentClient experiment;
        private ExperimentVariation variation;

        // Assign this variable to your API key from Dev Center. The API key
        // shown below is for example purposes only.
        private string apiKey = "F48AC670-4472-4387-AB7D-D65B095153FB";    

        public MainPage()
        {
            this.InitializeComponent();

            // Initialize the ExperimentClient for A/B testing.
            experiment = new ExperimentClient(apiKey);

            // Because this call is not awaited, execution of the current method
            // continues before the call is completed.
            #pragma warning disable CS4014
            Initialize();
            #pragma warning restore CS4014
        }

        private async Task Initialize()
        {
            // Get the current cached variation assignment for the experiment.
            ExperimentVariationResult result = await experiment.GetVariationAsync();
            variation = result.Variation;

            // Check whether the cached variation assignment needs to be refreshed.
            // If so, then refresh it.
            if (result.ErrorCode != EngagementErrorCode.Success || result.Variation.NeedsRefresh)
            {
                result = await experiment.RefreshVariationAsync();

                // If the call succeeds, use the new result. Otherwise, use the cached value.
                if (result.ErrorCode == EngagementErrorCode.Success)
                {
                    variation = result.Variation;
                }
            }

            // Get settings named "buttonText", "r", "g", and "b" from the variation
            // assignment. If no variation assignment is available, the settings default
            // to "Grey button" for the button text and grey RGB value for the button color.
            var buttonText = variation.GetString("buttonText", "Grey Button");
            var r = (byte)variation.GetInteger("r", 128);
            var g = (byte)variation.GetInteger("g", 128);
            var b = (byte)variation.GetInteger("b", 128);

            // Assign button text and color.
            await button.Dispatcher.RunAsync(
                CoreDispatcherPriority.Normal,
                () =>
                {
                    button.Background = new SolidColorBrush(Color.FromArgb(255, r, g, b));
                    button.Content = buttonText;
                    button.Visibility = Visibility.Visible;
                });

            // Log the view event named "userViewedButton" to Dev Center.
            StoreServicesCustomEvents.Log("userViewedButton", variation);
        }

        private void button_Click(object sender, RoutedEventArgs e)
        {
            // Log the conversion event named "userClickedButton" to Dev Center.
            StoreServicesCustomEvents.Log("userClickedButton", variation);
        }
     }
  }
  ```
9. Dans la ligne de code suivante, affectez les variables *apiKey* à la clé API que vous avez obtenue à partir du tableau de bord du Centre de développement dans la section précédente. La clé API illustrée ci-dessous est uniquement fournie à titre d’exemple.
```CSharp
private string apiKey = "F48AC670-4472-4387-AB7D-D65B095153FB";
```
10. Enregistrez le fichier de code et créez le projet.

## Exécuter l’application pour collecter des données d’expérience

1. Exécutez l’application **SampleExperiment** que vous avez créée dans la section précédente.
2. Vérifiez que vous voyez un bouton bleu ou gris. Cliquez sur le bouton, puis fermez l’application.
3. Répétez les étapes ci-dessus plusieurs fois sur le même ordinateur pour confirmer que votre application affiche la même couleur de bouton.

## Passer en revue les résultats et terminer l’expérience

Laissez s’écouler plusieurs heures après avoir terminé la section précédente, puis suivez ces étapes pour passer en revue les résultats de votre expérience et terminer l’expérience.

> **Remarque** Dès que vous activez une expérience, le Centre de développement lance immédiatement la collecte de données de toutes les applications consignant des données pour votre expérience. L’apparition sur le tableau de bord des données de l’expérience peut cependant prendre plusieurs heures.

1. Dans le Centre de développement, revenez à la page **Expérimentation** de votre application.
2. Dans la section **Expériences**, cliquez sur le filtre **Actives**, puis cliquez sur **Optimize Button Clicks** pour accéder à la page de cette expérience.
3. Vérifiez que les résultats affichés dans les sections **Résumé des résultats** et **Détails des résultats** correspondent à ce que vous attendez. Pour en savoir plus sur ces sections, voir [Gérer votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md#review-the-results-of-your-experiment).

  >**Remarque** Le Centre de développement signale uniquement le premier événement de conversion pour chaque utilisateur sur une période de 24 heures. Si un utilisateur déclenche plusieurs événements de conversion dans votre application au cours d’une période de 24 heures, seul le premier événement de conversion est signalé. Ceci est conçu pour empêcher le fait qu’un utilisateur unique avec plusieurs événements de conversion fausse les résultats de l’expérience pour un groupe représentatif d’utilisateurs.

4. Vous êtes désormais prêt à terminer l’expérience. Dans la section **Résumé des résultats**, cliquez sur **Basculer** dans la colonne **Variante B**. Cela permet de basculer tous les utilisateurs de votre application sur le bouton bleu.
5. Cliquez sur **OK** pour confirmer que vous souhaitez mettre fin à l’expérience.
6. Exécutez l’application **SampleExperiment** que vous avez créée dans la section précédente.
7. Vérifiez que vous voyez un bouton bleu. Notez que jusqu’à 2 minutes peuvent être nécessaires pour que votre application reçoive une affectation de variante mise à jour.

## Rubriques connexes

  * [Définir votre expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md)
  * [Coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md)
  * [Gérer votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md)
  * [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md)


<!--HONumber=May16_HO2-->


