---
author: mcleanbyron
Description: "Dans cette procédure pas à pas, vous allez créer, exécuter et gérer votre première expérience avec des tests A/B."
title: "Créer et exécuter votre première expérience avec des tests A/B"
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
translationtype: Human Translation
ms.sourcegitcommit: bfe4862c441ca095a40df4f594fdf9b3e213d142
ms.openlocfilehash: ab15741531b829c496811cdfca35059cd113f91d

---

# Créer et exécuter votre première expérience avec des tests A/B

Dans cette procédure pas à pas, vous allez:
* créer un [projet](run-app-experiments-with-a-b-testing.md#terms) d’expérimentation dans le tableau de bord du Centre de développement qui définit plusieurs variables distantes qui représentent le texte et la couleur d’un bouton d’application;
* créer une application avec du code qui récupère les valeurs des variables distantes, puis utilise ces données pour modifier la couleur d’arrière-plan d’un bouton et consigne les données des événements d’affichage et de conversion dans le Centre de développement;
* créer une expérience dans le projet pour tester si la modification de la couleur d’arrière-plan du bouton d’application augmente effectivement le nombre de clics de bouton;
* exécuter l’application pour collecter des données d’expérience;
* passer en revue les résultats de l’expérience sur le tableau de bord du Centre de développement, choisir une variante à activer pour tous les utilisateurs de l’application, puis terminer l’expérience.

Pour une vue d’ensemble des tests A/B avec le Centre de développement, voir [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md).

## Prérequis

Pour suivre cette procédure pas à pas, vous devez posséder un compte du Centre de développement Windows et devez configurer votre ordinateur de développement comme décrit dans [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md).

## Créer un projet avec des variables distantes dans le Centre de développement Windows

1. Connectez-vous au [tableau de bord du Centre de développement](https://dev.windows.com/overview).
2. Si vous disposez déjà d’une application dans le Centre de développement que vous voulez utiliser pour créer une expérience, sélectionnez-la dans votre tableau de bord. Si vous ne disposez pas encore d’une application dans votre tableau de bord, [créez une application en réservant un nom](../publish/create-your-app-by-reserving-a-name.md), puis sélectionnez cette application dans votre tableau de bord.
3. Dans le volet de navigation, cliquez sur **Services**, puis sur **Expérimentation**.
4. Dans la section **Projets** de la page suivante, cliquez sur le bouton **Nouveau projet**.
5. Dans la page **Nouveau projet**, entrez le nom **Expériences sur les clics de bouton** pour votre nouveau projet.
6. Développez la section **Variables distantes**, puis cliquez sur **Ajouter une variable** à quatre reprises. Vous devez maintenant avoir quatre lignes de variable vides.
  * Dans la première ligne, tapez **buttonText** pour le nom de la variable et **Bouton gris** dans la colonne **Valeur par défaut**.
  * Dans la deuxième ligne, tapez **r** pour le nom de la variable et **128** dans la colonne **Valeur par défaut**.
  * Dans la troisième ligne, tapez **g** pour le nom de la variable et **128** dans la colonne **Valeur par défaut**.
  * Dans la quatrième ligne, tapez **b** pour le nom de la variable et **128** dans la colonne **Valeur par défaut**.
7. Cliquez sur **Enregistrer** et notez la valeur [ID de projet](run-app-experiments-with-a-b-testing.md#terms) qui apparaît dans la section **Intégration au SDK**. Dans la section suivante, vous allez mettre à jour votre code d’application et faire référence à cette valeur dans votre code.

## Coder l’expérience dans votre application

1. Dans VisualStudio2015, créez un projet de plateformeWindows universelle à l’aide de VisualC#. Nommez le projet **SampleExperiment**.
2. Dans l’Explorateur de solutions, développez votre nœud de projet, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence**.
3. Dans le **Gestionnaire de références**, développez **Windows universel**, puis cliquez sur **Extensions**.
4. Dans la liste des kits de développement logiciel (SDK), cochez la case en regard de **Microsoft Engagement Framework** et cliquez sur **OK**.
5. Dans l’**Explorateur de solutions**, double-cliquez sur MainPage.xaml pour ouvrir le concepteur pour la page principale de l’application.
6. Faites glisser un **Bouton** de la **Boîte à outils** vers la page.
7. Double-cliquez sur le bouton dans le concepteur pour ouvrir le fichier de code et ajoutez un gestionnaire d’événements pour l’événement **Click**.  
8. Remplacez l’ensemble du contenu du fichier de code par le code ci-après. Affectez à la variable ```projectId``` la valeur [ID de projet](run-app-experiments-with-a-b-testing.md#terms) que vous avez obtenue à partir du tableau de bord du Centre de développement dans la section précédente.

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
        private StoreServicesExperimentVariation variation;
        private StoreServicesCustomEventLogger logger;

        // Assign this variable to the project ID for your experiment from Dev Center.
        private string projectId = "";

        public MainPage()
        {
            this.InitializeComponent();

            // Because this call is not awaited, execution of the current method
            // continues before the call is completed.
#pragma warning disable CS4014
            InitializeExperiment();
#pragma warning restore CS4014
        }

        private async Task InitializeExperiment()
        {
            // Get the current cached variation assignment for the experiment.
            var result = await StoreServicesExperimentVariation.GetCachedVariationAsync(projectId);
            variation = result.ExperimentVariation;

            // Check whether the cached variation assignment needs to be refreshed.
            // If so, then refresh it.
            if (result.ErrorCode != StoreServicesEngagementErrorCode.None || result.ExperimentVariation.IsStale)
            {
                result = await StoreServicesExperimentVariation.GetRefreshedVariationAsync(projectId);

                // If the call succeeds, use the new result. Otherwise, use the cached value.
                if (result.ErrorCode == StoreServicesEngagementErrorCode.None)
                {
                    variation = result.ExperimentVariation;
                }
            }

            // Get remote variables named "buttonText", "r", "g", and "b" from the variation
            // assignment. If no variation assignment is available, the variables default
            // to "Grey button" for the button text and grey RGB value for the button color.
            var buttonText = variation.GetString("buttonText", "Grey Button");
            var r = (byte)variation.GetInt32("r", 128);
            var g = (byte)variation.GetInt32("g", 128);
            var b = (byte)variation.GetInt32("b", 128);

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
            if (logger == null)
            {
                logger = StoreServicesCustomEventLogger.GetDefault();
            }

            logger.LogForVariation(variation, "userViewedButton");
        }

        private void button_Click(object sender, RoutedEventArgs e)
        {
            // Log the conversion event named "userClickedButton" to Dev Center.
            if (logger == null)
            {
                logger = StoreServicesCustomEventLogger.GetDefault();
            }

            logger.LogForVariation(variation, "userClickedButton");
        }
     }
  }
  ```
10. Enregistrez le fichier de code et créez le projet.

## Créer l’expérience dans le Centre de développementWindows

1. Revenez à la page du projet **Expériences sur les clics de bouton** dans le tableau de bord du Centre de développement Windows.
2. Dans la section **Expériences**, cliquez sur le bouton **Nouvelle expérience**.
5. Dans la section **Experiment details (Détails de l’expérience)**, tapez le nom **Optimiser les clics de bouton** dans le champ **Experiment name (Nom de l’expérience)**.
6. Dans la section **View event (Événement d’affichage)**, tapez **userViewedButton** dans le champ **View event name (Nom de l’événement d’affichage)**. Notez que ce nom correspond à la chaîne de l’événement d’affichage enregistré dans le code que vous avez ajoutée dans la section précédente.
7. Dans la section **Objectifs et événements de conversion**, entrez les valeurs suivantes:
  * Dans le champ **Nom d’objectif**, tapez **Increase Button Clicks**.
  * Dans le champ **Nom de l’événement de conversion**, tapez le nom **userClickedButton**. Notez que ce nom correspond à la chaîne de l’événement de conversion enregistré dans le code que vous avez ajoutée dans la section précédente.
  * Dans le champ **Objectif**, choisissez **Agrandir**.
8. Dans la section **Variables distantes et variantes**, vérifiez que la case **Répartir en valeurs égales** est cochée pour que les variantes soient distribuées équitablement à votre application.
9. Ajoutez des variables à votre expérience:
  9. Cliquez sur le contrôle de liste déroulante, choisissez **buttonText**, puis cliquez sur **Ajouter une variable**. La chaîne **Bouton gris** apparaît automatiquement dans la colonne **VarianteA** (cette valeur est dérivée des paramètres du projet). Dans la colonne **VarianteB**, tapez **Bouton bleu**.
  9. Cliquez une nouvelle fois sur le contrôle de liste déroulante, choisissez **r**, puis cliquez sur **Ajouter une variable**. La chaîne **128** doit apparaître automatiquement dans la colonne **VarianteA**. Dans la colonne **VarianteB**, tapez **1**.
  9. Cliquez une nouvelle fois sur le contrôle de liste déroulante, choisissez **g**, puis cliquez sur **Ajouter une variable**. La chaîne **128** doit apparaître automatiquement dans la colonne **VarianteA**. Dans la colonne **VarianteB**, tapez **1**.  
  9. Cliquez une nouvelle fois sur le contrôle de liste déroulante, choisissez **b**, puis cliquez sur **Ajouter une variable**. La chaîne **128** doit apparaître automatiquement dans la colonne **VarianteA**. Dans la colonne **VarianteB**, tapez **255**.  
10. Cliquez sur **Enregistrer**, puis sur **Activer**.

> **Important**&nbsp;&nbsp;Une fois que vous avez activé une expérience, vous ne pouvez plus en modifier les paramètres, sauf si vous avez coché la case **Editable experiment** (Expérience modifiable) quand vous avez créé l’expérience. D’habitude, nous vous recommandons de coder l’expérience dans votre application avant de l’activer.

## Exécuter l’application pour collecter des données d’expérience

1. Exécutez l’application **SampleExperiment** que vous avez créée précédemment.
2. Vérifiez que vous voyez soit un bouton bleu, soit un bouton gris. Cliquez sur le bouton, puis fermez l’application.
3. Répétez les étapes ci-dessus plusieurs fois sur le même ordinateur pour confirmer que votre application affiche la même couleur de bouton.

## Passer en revue les résultats et terminer l’expérience

Laissez s’écouler plusieurs heures après avoir terminé la section précédente, puis suivez ces étapes pour passer en revue les résultats de votre expérience et terminer l’expérience.

> **Remarque**&nbsp;&nbsp;Dès que vous activez une expérience, le Centre de développement lance immédiatement la collecte de données de toutes les applications consignant des données pour votre expérience. L’apparition des données de l’expérience dans le tableau de bord peut cependant prendre plusieurs heures.

1. Dans le Centre de développement, revenez à la page **Expérimentation** de votre application.
2. Dans la section **Active experiments** (Expériences actives), cliquez sur **Optimize Button Clicks** (Optimiser les clics de bouton) pour accéder à la page de cette expérience.
3. Vérifiez que les résultats affichés dans les sections **Résumé des résultats** et **Détails des résultats** correspondent à ce que vous attendez. Pour en savoir plus sur ces sections, voir [Gérer votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md#review-the-results-of-your-experiment).

  >**Remarque**&nbsp;&nbsp;Le Centre de développement signale uniquement le premier événement de conversion pour chaque utilisateur sur une période de 24heures. Si un utilisateur déclenche plusieurs événements de conversion dans votre application au cours d’une période de 24heures, seul le premier événement de conversion est signalé. Ceci est conçu pour empêcher le fait qu’un utilisateur unique avec plusieurs événements de conversion fausse les résultats de l’expérience pour un groupe représentatif d’utilisateurs.
4. Vous êtes désormais prêt à terminer l’expérience. Dans la section **Résumé des résultats**, cliquez sur **Basculer** dans la colonne **Variante B**. Cela permet de basculer tous les utilisateurs de votre application sur le bouton bleu.
5. Cliquez sur **OK** pour confirmer que vous souhaitez mettre fin à l’expérience.
6. Exécutez l’application **SampleExperiment** que vous avez créée dans la section précédente.
7. Vérifiez que vous voyez un bouton bleu. Notez que jusqu’à 2minutes peuvent être nécessaires pour que votre application reçoive une affectation de variante mise à jour.

## Rubriques connexes

* [Créer un projet et définir des variables distantes dans le tableau de bord du Centre de développement](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md)
* [Définir votre expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md)
* [Gérer votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md)
* [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Sep16_HO1-->


