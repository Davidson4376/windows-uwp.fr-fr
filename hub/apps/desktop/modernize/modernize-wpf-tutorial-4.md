---
description: Ce didacticiel montre comment ajouter des interfaces utilisateur UWP XAML, créer des packages MSIX et incorporer d’autres composants modernes dans votre application WPF.
title: Ajouter des notifications et des activités des utilisateurs Windows 10
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, uwp, WinForms, wpf, xaml (îles)
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 8443ac25ba678986046b967a90a8899eaffb76aa
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420124"
---
# <a name="part-4-add-windows-10-user-activities-and-notifications"></a>Partie 4 : Ajouter des notifications et des activités des utilisateurs Windows 10

Il s’agit de la quatrième partie d’un didacticiel qui montre comment moderniser une application de bureau WPF exemple nommée dépenses de Contoso. Pour une vue d’ensemble du didacticiel, configuration requise et des instructions pour télécharger l’exemple d’application, consultez [didacticiel : Moderniser une application WPF](modernize-wpf-tutorial.md). Cet article suppose que vous avez déjà effectué [partie 3](modernize-wpf-tutorial-3.md).

Dans les parties précédentes de ce didacticiel, vous avez ajouté de contrôles UWP XAML à l’application à l’aide de XAML (îles). En tant qu’un par produit de cela, l’application d’appeler n’importe quelle API WinRT également activée. La possibilité pour l’application à utiliser de nombreuses autres fonctionnalités offertes par Windows 10, pas seulement les contrôles UWP XAML s’ouvre.

Dans le scénario fictif de ce didacticiel, l’équipe de développement de Contoso a décidé d’ajouter deux nouvelles fonctionnalités à l’application : les activités et les notifications. Cette partie du didacticiel montre comment implémenter ces fonctionnalités.

## <a name="add-a-user-activity"></a>Ajouter une activité utilisateur

Dans Windows 10, applications peuvent suivre les activités effectuées par l’utilisateur telles que l’ouverture d’un fichier ou l’affichage d’une page spécifique. Ces activités sont ensuite rendues disponibles via la chronologie, une fonctionnalité introduite dans Windows 10 version 1803, ce qui lui permet de rapidement revenir en arrière dans le passé et reprendre une activité que déjà lancés.

![Image de la chronologie de Windows](images/wpf-modernize-tutorial/WindowsTimeline.png)

Activités de l’utilisateur sont suivies à l’aide de [Microsoft Graph](https://developer.microsoft.com/graph/). Toutefois, lorsque vous créez une application Windows 10, vous n’avez pas besoin d’interagir directement avec les points de terminaison REST fournies par Microsoft Graph. Au lieu de cela, vous pouvez utiliser un ensemble pratique d’APIs de WinRT. Nous allons utiliser ces APIs WinRT dans l’application de dépenses de Contoso pour effectuer le suivi de chaque fois que l’utilisateur ouvre une dépense au sein de l’application et utiliser des cartes adaptatives pour permettre aux utilisateurs de créer l’activité.

### <a name="introduction-to-adaptive-cards"></a>Présentation des cartes adaptatives

Cette section fournit une brève présentation de [des cartes adaptatives](https://docs.microsoft.com/adaptive-cards/). Si vous ne devez pas ces informations, vous pouvez ignorer cette étape et passer directement à la [ajouter une carte adaptative](#add-an-adaptive-card) obtenir des instructions.

Des cartes adaptatives permettent aux développeurs d’échanger du contenu de la carte de manière commune et cohérente. Une carte adaptative est décrite par une charge utile JSON qui définit son contenu, ce qui peut inclure de texte, des images, des actions et bien plus encore.

Une carte adaptative définit simplement le contenu et pas l’apparence visuelle du contenu. La plateforme sur laquelle la carte adaptative est reçue peut afficher le contenu à l’aide de l’application d’un style plus appropriée. Est le moyen de cartes adaptatives sont conçus [un convertisseur](https://docs.microsoft.com/adaptive-cards/rendering-cards/getting-started), qui est en mesure de prendre la charge utile JSON et convertir en une interface utilisateur native. Par exemple, l’interface utilisateur peut être XAML pour une WPF ou UWP application AXML pour une application Android ou HTML pour un site Web ou une conversation de robot.

Voici un exemple d’une charge utile de carte adaptative simple.

```json
{
    "type": "AdaptiveCard",
    "body": [
        {
            "type": "Container",
            "items": [
                {
                    "type": "TextBlock",
                    "size": "Medium",
                    "weight": "Bolder",
                    "text": "Publish Adaptive Card schema"
                },
                {
                    "type": "ColumnSet",
                    "columns": [
                        {
                            "type": "Column",
                            "items": [
                                {
                                    "type": "Image",
                                    "style": "Person",
                                    "url": "https://pbs.twimg.com/profile_images/3647943215/d7f12830b3c17a5a9e4afcc370e3a37e_400x400.jpeg",
                                    "size": "Small"
                                }
                            ],
                            "width": "auto"
                        },
                        {
                            "type": "Column",
                            "items": [
                                {
                                    "type": "TextBlock",
                                    "weight": "Bolder",
                                    "text": "Matt Hidinger",
                                    "wrap": true
                                },
                                {
                                    "type": "TextBlock",
                                    "spacing": "None",
                                    "text": "Created {{DATE(2017-02-14T06:08:39Z,SHORT)}}",
                                    "isSubtle": true,
                                    "wrap": true
                                }
                            ],
                            "width": "stretch"
                        }
                    ]
                }
            ]
        }
    ],
    "actions": [
        {
            "type": "Action.ShowCard",
            "title": "Set due date",
            "card": {
                "type": "AdaptiveCard",
                "style": "emphasis",
                "body": [
                    {
                        "type": "Input.Date",
                        "id": "dueDate"
                    },
                    {
                        "type": "Input.Text",
                        "id": "comment",
                        "placeholder": "Add a comment",
                        "isMultiline": true
                    }
                ],
                "actions": [
                    {
                        "type": "Action.OpenUrl",
                        "title": "OK",
                        "url": "http://adaptivecards.io"
                    }
                ],
                "$schema": "http://adaptivecards.io/schemas/adaptive-card.json"
            }
        },
        {
            "type": "Action.OpenUrl",
            "title": "View",
            "url": "http://adaptivecards.io"
        }
    ],
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.0"
}
```

L’image ci-dessous montre comment ce JSON est rendu de différentes façons par ta canal Cortana et les équipes, une notification de Windows.

![Image de rendu de carte adaptative](images/wpf-modernize-tutorial/AdaptiveCards.png)

Des cartes adaptatives jouent un rôle important dans la chronologie, car il est la façon dont Windows restitue des activités. Chaque vignette affichée dans la chronologie est en fait une carte adaptative. Par conséquent, lorsque vous allez créer une activité de l’utilisateur à l’intérieur de votre application, vous devez fournir une carte adaptative pour rendre.

> [!NOTE]
> À l’aide d’un excellent moyen de réfléchir sur la conception d’une carte adaptative [le concepteur en ligne](https://adaptivecards.io/designer/). Vous aurez la possibilité de concevoir la carte avec des blocs de construction (images, textes, colonnes, etc.) et obtenir le JSON correspondant. Une fois que vous avez une idée de la conception finale, vous pouvez utiliser une bibliothèque nommée [des cartes adaptatives](https://www.nuget.org/packages/AdaptiveCards/) pour le rendre plus facile de créer votre carte adaptative à l’aide C# classes au lieu d’un JSON simple, qui peuvent être difficiles à déboguer et à générer.

### <a name="add-an-adaptive-card"></a>Ajouter une carte adaptative

1. Cliquez avec le bouton droit sur le **ContosoExpenses.Core** projet dans l’Explorateur de solutions, puis choisissez **gérer les packages NuGet**.

2. Dans le **Gestionnaire de Package NuGet** fenêtre, cliquez sur **Parcourir**. Recherchez le `Newtonsoft.Json` empaqueter et installer la dernière version disponible. Il s’agit d’une bibliothèque de manipulation JSON populaires que vous utiliserez pour aider à mainipulate les chaînes JSON requis par des cartes adaptatives.

    ![Package NewtonSoft.Json NuGet](images/wpf-modernize-tutorial/JsonNetNuGet.png)

    > [!NOTE]
    > Si vous n’installez pas le `Newtonsoft.Json` package séparément, la bibliothèque de cartes adaptatives le référencera une version antérieure de le `Newtonsoft.Json` package qui ne prennent pas en charge .NET Core 3.0.

2. Dans le **Gestionnaire de Package NuGet** fenêtre, cliquez sur **Parcourir**. Recherchez le `AdaptiveCards` empaqueter et installer la dernière version disponible.

    ![Adaptive package NuGet de cartes](images/wpf-modernize-tutorial/AdaptiveCardsNuGet.png)

3. Dans **l’Explorateur de solutions**, avec le bouton droit le **ContosoExpenses.Core** de projet, choisissez **Ajouter -> classe**. Nommez la classe **TimelineService.cs** et cliquez sur **OK**.

4. Dans le **TimelineService.cs** , ajoutez les instructions suivantes au début du fichier.

    ```csharp
    using AdaptiveCards;
    using ContosoExpenses.Data.Models;
    ```

5. Modification de l’espace de noms déclaré dans le fichier à partir de `ContosoExpenses.Core` à `ContosoExpenses`.

5. Ajoutez la méthode suivante à la `TimelineService` classe.

   ```csharp
    private string BuildAdaptiveCard(Expense expense)
    {
        AdaptiveCard card = new AdaptiveCard("1.0");

        AdaptiveTextBlock title = new AdaptiveTextBlock
        {
            Text = expense.Description,
            Size = AdaptiveTextSize.Medium,
            Wrap = true
        };

        AdaptiveColumnSet columnSet = new AdaptiveColumnSet();
        AdaptiveColumn photoColumn = new AdaptiveColumn
        {
            Width = "auto"
        };

        AdaptiveImage image = new AdaptiveImage
        {
            Url = new Uri("https://appmodernizationworkshop.blob.core.windows.net/contosoexpenses/Contoso192x192.png"),
            Size = AdaptiveImageSize.Small,
            Style = AdaptiveImageStyle.Default
        };
        photoColumn.Items.Add(image);

        AdaptiveTextBlock amount = new AdaptiveTextBlock
        {
            Text = expense.Cost.ToString(),
            Weight = AdaptiveTextWeight.Bolder,
            Wrap = true
        };

        AdaptiveTextBlock date = new AdaptiveTextBlock
        {
            Text = expense.Date.Date.ToShortDateString(),
            IsSubtle = true,
            Spacing = AdaptiveSpacing.None,
            Wrap = true
        };

        AdaptiveColumn expenseColumn = new AdaptiveColumn
        {
            Width = "stretch"
        };
        expenseColumn.Items.Add(amount);
        expenseColumn.Items.Add(date);

        columnSet.Columns.Add(photoColumn);
        columnSet.Columns.Add(expenseColumn);

        card.Body.Add(title);
        card.Body.Add(columnSet);

        string json = card.ToJson();
        return json;
    }
    ```

#### <a name="about-the-code"></a>À propos du code

Cette méthode reçoit un **dépenses** objet avec toutes les informations sur les dépenses liées à restituer et il génère un nouveau **AdaptiveCard** objet. La méthode ajoute les éléments suivants à la carte :

- Un titre, qui utilise la description de la dépense.
- Une image, qui est le logo de Contoso.
- Le montant de la dépense.
- La date de la dépense.

Les 3 derniers éléments sont divisées en deux colonnes différentes, afin que le logo de Contoso et les détails de la dépense peuvent être placées côte à côte. Une fois que l’objet est créé, la méthode retourne la chaîne JSON correspondante à l’aide de la **ToJson** (méthode).

### <a name="define-the-user-activity"></a>Définir l’activité des utilisateurs

Maintenant que vous avez défini la carte adaptative, vous pouvez créer une activité des utilisateurs basée sur celui-ci.

1. Ajoutez les instructions suivantes au début du **TimelineService.cs** fichier :

    ```csharp
    using Windows.ApplicationModel.UserActivities;
    using System.Threading.Tasks;
    using Windows.UI.Shell;
    ```

    > [!NOTE]
    > Il s’agit d’espaces de noms UWP. Ces résoudre, car le `Microsoft.Toolkit.Wpf.UI.Controls` package NuGet que vous avez installé à l’étape 2 inclut une référence à la `Microsoft.Windows.SDK.Contracts` du package, ce qui permet la **ContosoExpenses.Core** projet référence WinRT APIs même s’il est .NET Projet de Core 3.

2. Ajoutez les déclarations de champ suivantes à la `TimelineService` classe.

    ```csharp
    private UserActivityChannel _userActivityChannel;
    private UserActivity _userActivity;
    private UserActivitySession _userActivitySession;
    ```

3. Ajoutez la méthode suivante à la `TimelineService` classe.

    ```csharp
    public async Task AddToTimeline(Expense expense)
    {
        _userActivityChannel = UserActivityChannel.GetDefault();
        _userActivity = await _userActivityChannel.GetOrCreateUserActivityAsync($"Expense-{expense.ExpenseId}");

        _userActivity.ActivationUri = new Uri($"contosoexpenses://expense/{expense.ExpenseId}");
        _userActivity.VisualElements.DisplayText = "Contoso Expenses";

        string json = BuildAdaptiveCard(expense);

        _userActivity.VisualElements.Content = AdaptiveCardBuilder.CreateAdaptiveCardFromJson(json);

        await _userActivity.SaveAsync();
        _userActivitySession?.Dispose();
        _userActivitySession = _userActivity.CreateSession();
    }
    ```

4. Enregistrez les modifications apportées à **TimelineService.cs**.

#### <a name="about-the-code"></a>À propos du code

Le `AddToTimeline` méthode obtient d’abord un **UserActivityChannel** objet qui est requis pour stocker les activités de l’utilisateur. Puis il crée une nouvelle activité utilisateur à l’aide du **GetOrCreateUserActivityAsync** (méthode), ce qui nécessite un identificateur unique. De cette façon, si une activité existe déjà, l’application peut mettre à jour ; dans le cas contraire, il créera un nouveau. Dépend de l’identificateur à passer par le type d’application que vous créez :

* Si vous souhaitez toujours mettre à jour de la même activité afin que la chronologie n’affiche que celui qui est plus récente, vous pouvez utiliser un identificateur fixe (tel que **dépenses**).
* Si vous souhaitez effectuer le suivi de chaque activité en tant qu’une autre, afin que la chronologie affiche tous les, vous pouvez utiliser un identificateur dynamique.

Dans ce scénario, l’application suivra chaque dépense ouvert en tant qu’autre utilisateur activité, le code crée chaque identificateur à l’aide du mot clé **dépenses -** suivi par l’ID de notes de frais uniques.

Une fois que la méthode crée un **UserActivity** de l’objet, il remplit l’objet avec les informations suivantes :

* Un **ActivationUri** qui est appelé lorsque l’utilisateur clique sur l’activité dans la chronologie. Le code utilise un protocole personnalisé appelé **contosoexpenses** qui gère l’application plus tard.
* Le **VisualElements** objet qui contient un ensemble de propriétés qui définissent l’apparence visuelle de l’activité. Ce code définit le **DisplayText** (qui est le titre affiché en haut de l’entrée dans la chronologie) et le **contenu**. 

Il s’agit dans lequel la carte adaptative défini précédemment joue un rôle. L’application transmet la carte adaptative vous conçu précédemment en tant que contenu à la méthode. Toutefois, Windows 10 utilise un autre objet pour représenter une carte par rapport à celui utilisé par le `AdaptiveCards` package NuGet. Par conséquent, la méthode recrée la carte à l’aide de la **CreateAdaptiveCardFromJson** méthode exposée par le **AdaptiveCardBuilder** classe. Une fois que la méthode crée l’activité des utilisateurs, il enregistre l’activité et crée une nouvelle session.

Lorsqu’un utilisateur clique sur une activité dans la chronologie, la **contosoexpenses : / /** protocole est activé et l’URL inclut les informations de l’application doit récupérer la dépense sélectionnée. Comme une tâche facultative, vous pouvez implémenter l’activation de protocole afin que l’application réagit correctement quand l’utilisateur utilise la chronologie.

### <a name="integrate-the-application-with-timeline"></a>Intégrer l’application de la chronologie

Maintenant que vous avez créé une classe qui interagit avec la chronologie, nous pouvons commencer à l’utiliser pour améliorer l’expérience de l’application. Le meilleur endroit pour utiliser le **AddToTimeline** méthode exposée par le **TimelineService** classe est lorsque l’utilisateur ouvre la page de détails d’une dépense.

1. Dans le **ContosoExpenses.Core** de projet, développez le **ViewModels** dossier et ouvrez le **ExpenseDetailViewModel.cs** fichier. Il s’agit de ViewModel qui prend en charge de la fenêtre de détail de dépenses.

2. Recherchez le constructeur public de la **ExpenseDetailViewModel** classe et ajoutez le code suivant à la fin du constructeur. Chaque fois que la fenêtre de dépense est ouvert, la méthode appelle la **AddToTimeline** (méthode) et transmet la dépense actuelle. Le **TimelineService** classe utilise ces informations pour créer une activité de l’utilisateur en utilisant les informations de notes de frais.

    ```csharp
    TimelineService timeline = new TimelineService();
    timeline.AddToTimeline(expense);
    ```

    Lorsque vous avez terminé, le constructeur doit ressembler à ceci.

    ```csharp
    public ExpensesDetailViewModel(IDatabaseService databaseService, IStorageService storageService)
    {
        var expense = databaseService.GetExpense(storageService.SelectedExpense);

        ExpenseType = expense.Type;
        Description = expense.Description;
        Location = expense.Address;
        Amount = expense.Cost;

        TimelineService timeline = new TimelineService();
        timeline.AddToTimeline(expense);
    }
    ```

3. Appuyez sur F5 pour générer et exécuter l’application dans le débogueur. Choisissez un employé dans la liste, puis choisissez une dépense. Dans la page de détails, notez la description de la dépense, la date et la quantité.

4. Appuyez sur **Start + onglet** pour ouvrir la chronologie.

5. Défilement vers le bas de la liste des applications actuellement ouvertes jusqu'à ce que vous consultez la section intitulée **précédemment aujourd'hui**. Cette section présente certaines de vos activités utilisateur les plus récentes. Cliquez sur le **voir toutes les activités** situé en regard du **précédemment aujourd'hui** titre.

6. Vérifiez que vous voyez une nouvelle carte avec les informations concernant les frais que vous venez de sélectionner dans l’application.

    ![Chronologie des dépenses de Contoso](images/wpf-modernize-tutorial/ContosoExpensesTimeline.png)

7. Si vous ouvrez maintenant autres dépenses, vous verrez les nouvelles cartes ajoutés en tant qu’activités de l’utilisateur. N’oubliez pas que le code utilise un autre identificateur pour chaque activité, il crée une carte pour chaque dépense vous ouvrez dans l’application.

8. Fermez l’application.

## <a name="add-a-notification"></a>Ajouter une notification

La deuxième fonctionnalité que l’équipe de développement Contoso souhaite ajouter est une notification qui s’affiche à l’utilisateur chaque fois qu’une nouvelle dépense est enregistrée dans la base de données. Pour ce faire, vous pouvez exploiter le système de notifications intégrées dans Windows 10, qui est exposé aux développeurs via WinRT APIs. Ce système de notification présente de nombreux avantages :

- Notifications sont cohérentes avec le reste du système d’exploitation.
- Ils sont exploitables.
- Ils sont stockés dans le centre de maintenance afin de vérifier plus tard.

Pour ajouter une notification à l’application :

1. Dans **l’Explorateur de solutions**, avec le bouton droit le **ContosoExpenses.Core** de projet, choisissez **Ajouter -> classe**. Nommez la classe **NotificationService.cs** et cliquez sur **OK**.

2. Dans le **NotificationService.cs** , ajoutez les instructions suivantes au début du fichier.

    ```csharp
    using Windows.Data.Xml.Dom;
    using Windows.UI.Notifications;
    ```

3. Modification de l’espace de noms déclaré dans le fichier à partir de `ContosoExpenses.Core` à `ContosoExpenses`.

4. Ajoutez la méthode suivante à la `NotificationService` classe.

    ```csharp
    public void ShowNotification(string description, double amount)
    {
        string xml = $@"<toast>
                          <visual>
                            <binding template='ToastGeneric'>
                              <text>Expense added</text>
                              <text>Description: {description} - Amount: {amount} </text>
                            </binding>
                          </visual>
                        </toast>";

        XmlDocument doc = new XmlDocument();
        doc.LoadXml(xml);

        ToastNotification toast = new ToastNotification(doc);
        ToastNotificationManager.CreateToastNotifier().Show(toast);
    }
    ```

    Notifications toast sont représentées par une charge utile XML, qui peut inclure de texte, des images, des actions et bien plus encore. Vous pouvez trouver tous les éléments pris en charge [ici](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-schema). Ce code utilise un schéma très simple avec deux lignes de texte : le titre et le corps. Une fois que le code définit la charge utile XML et les charge dans un **XmlDocument** de l’objet, qu’il encapsule le code XML dans un **ToastNotification** de l’objet et les affiche à l’aide d’un le **ToastNotificationManager** classe.

5. Dans le **ContosoExpenses.Core** de projet, développez le **ViewModels** dossier et ouvrez le **AddNewExpenseViewModel.cs** fichier. 

6. Recherchez le `SaveExpenseCommand` (méthode), qui est déclenché lorsque l’utilisateur appuie sur le bouton pour enregistrer une nouvelle dépense. Ajoutez le code suivant à cette méthode, juste après l’appel à la `SaveExpense` (méthode).

    ```csharp
    NotificationService notificationService = new NotificationService();
    notificationService.ShowNotification(expense.Description, expense.Cost);
    ```

    Lorsque vous avez terminé, le `SaveExpenseCommand` méthode doit ressembler à ceci.

    ```csharp
    private RelayCommand _saveExpenseCommand;
    public RelayCommand SaveExpenseCommand
    {
        get
        {
            if (_saveExpenseCommand == null)
            {
                _saveExpenseCommand = new RelayCommand(() =>
                {
                    Expense expense = new Expense
                    {
                        Address = Address,
                        City = City,
                        Cost = Cost,
                        Date = Date,
                        Description = Description,
                        EmployeeId = storageService.SelectedEmployeeId,
                        Type = ExpenseType
                    };

                    databaseService.SaveExpense(expense);

                    NotificationService notificationService = new NotificationService();
                    notificationService.ShowNotification(expense.Description, expense.Cost);

                    Messenger.Default.Send<UpdateExpensesListMessage>(new UpdateExpensesListMessage());
                    Messenger.Default.Send<CloseWindowMessage>(new CloseWindowMessage());
                }, () => IsFormFilled
                );
            }

            return _saveExpenseCommand;
        }
    }
    ```

7. Appuyez sur F5 pour générer et exécuter l’application dans le débogueur. Un employé dans la liste et cliquez sur le **ajouter nouvelle dépense** bouton. Renseignez tous les champs dans le formulaire et appuyez sur **enregistrer**.

8. Vous recevrez l’exception suivante.

    ![Erreur de notification de toast](images/wpf-modernize-tutorial/ToastNotificationError.png)

Cette exception est provoquée par le fait que l’application de dépenses de Contoso ne dispose pas encore identité du package. Certaines APIs WinRT, y compris les notifications d’API, nécessitent l’identité du package avant de pouvoir être utilisés dans une application. Les applications UWP réception identité du package par défaut, car ils peuvent uniquement être distribués via les packages MSIX. Autres types d’applications Windows, y compris les applications WPF, peuvent également être déployés via les packages MSIX pour obtenir l’identité du package. La partie suivante de ce didacticiel explorerons comment effectuer cette opération.

## <a name="next-steps"></a>Étapes suivantes

À ce stade du didacticiel, vous avez ajouté une activité de l’utilisateur à l’application qui s’intègre à Windows chronologie, et vous avez également ajouté une notification à l’application qui est déclenchée quand les utilisateurs créent une nouvelle dépense. Toutefois, la notification ne fonctionne pas encore, car l’application nécessite l’identité du package à utiliser les API de notifications. Pour savoir comment générer un package MSIX pour l’application à obtenir l’identité du package et obtenir d’autres avantages du déploiement, consultez [partie 5 : Empaqueter et déployer avec MSIX](modernize-wpf-tutorial-5.md).
