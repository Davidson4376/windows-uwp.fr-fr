---
title: Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan
description: Utilisez une tâche en arrière-plan pour mettre à jour une vignette dynamique de votre application avec du contenu actualisé.
Search.SourceType: Video
ms.assetid: 9237A5BD-F9DE-4B8C-B689-601201BA8B9A
ms.date: 01/11/2018
ms.topic: article
keywords: Windows 10, UWP, tâche en arrière-plan
ms.localizationpriority: medium
ms.openlocfilehash: df2fad68fd1aab9b3b056e962736f3d37f749e63
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393533"
---
# <a name="update-a-live-tile-from-a-background-task"></a>Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan

**API importantes**

-   [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)

Utilisez une tâche en arrière-plan pour mettre à jour une vignette dynamique de votre application avec du contenu actualisé.

La vidéo suivante montre comment ajouter des vignettes dynamiques à vos applications.

<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Updating-a-live-tile-from-a-background-task/player" width="720" height="405" allowFullScreen="true" frameBorder="0"></iframe>

## <a name="create-the-background-task-project"></a>Créer le projet de tâche en arrière-plan  

Pour activer une vignette dynamique pour votre application, ajoutez un nouveau Windows Runtime projet de composant à votre solution. Il s’agit d’un assembly distinct que le système d’exploitation charge et exécute en arrière-plan lorsqu’un utilisateur installe votre application.

1.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur la solution, cliquez sur **Ajouter**, puis cliquez sur **Nouveau projet**.
2.  Dans la boîte de dialogue **Ajouter un nouveau projet**, sélectionnez le modèle **Composant Windows Runtime** dans la section **Installé &gt; Autres langages &gt; Visual C# &gt; Windows Universal**.
3.  Nommez le projet BackgroundTasks, puis cliquez ou appuyez sur **OK**. Microsoft Visual Studio ajoute le nouveau projet à la solution.
4.  Dans le projet principal, ajoutez une référence au projet BackgroundTasks.

## <a name="implement-the-background-task"></a>Implémenter la tâche en arrière-plan


Implémentez l’interface [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) pour créer une classe qui met à jour la vignette dynamique de votre application. Votre tâche en arrière-plan va dans la méthode Run. Dans ce cas, la tâche obtient un flux de syndication pour les blogs MSDN. Pour éviter la fermeture prématurée de la tâche lorsque du code asynchrone est encore en cours d’exécution, obtenez un report.

1.  Dans l’Explorateur de solutions, renommez le fichier généré automatiquement, Class1.cs, sous la forme BlogFeedBackgroundTask.cs.
2.  Dans BlogFeedBackgroundTask.cs, remplacez le code généré automatiquement par le code stub pour la classe **BlogFeedBackgroundTask**.
3.  Dans l’implémentation de la méthode Run, ajoutez du code pour les méthodes **GetMSDNBlogFeed** et **UpdateTile**.

```cs
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

// Added during quickstart
using Windows.ApplicationModel.Background;
using Windows.Data.Xml.Dom;
using Windows.UI.Notifications;
using Windows.Web.Syndication;

namespace BackgroundTasks
{
    public sealed class BlogFeedBackgroundTask  : IBackgroundTask
    {
        public async void Run( IBackgroundTaskInstance taskInstance )
        {
            // Get a deferral, to prevent the task from closing prematurely
            // while asynchronous code is still running.
            BackgroundTaskDeferral deferral = taskInstance.GetDeferral();

            // Download the feed.
            var feed = await GetMSDNBlogFeed();

            // Update the live tile with the feed items.
            UpdateTile( feed );

            // Inform the system that the task is finished.
            deferral.Complete();
        }

        private static async Task<SyndicationFeed> GetMSDNBlogFeed()
        {
            SyndicationFeed feed = null;

            try
            {
                // Create a syndication client that downloads the feed.  
                SyndicationClient client = new SyndicationClient();
                client.BypassCacheOnRetrieve = true;
                client.SetRequestHeader( customHeaderName, customHeaderValue );

                // Download the feed.
                feed = await client.RetrieveFeedAsync( new Uri( feedUrl ) );
            }
            catch( Exception ex )
            {
                Debug.WriteLine( ex.ToString() );
            }

            return feed;
        }

        private static void UpdateTile( SyndicationFeed feed )
        {
            // Create a tile update manager for the specified syndication feed.
            var updater = TileUpdateManager.CreateTileUpdaterForApplication();
            updater.EnableNotificationQueue( true );
            updater.Clear();

            // Keep track of the number feed items that get tile notifications.
            int itemCount = 0;

            // Create a tile notification for each feed item.
            foreach( var item in feed.Items )
            {
                XmlDocument tileXml = TileUpdateManager.GetTemplateContent( TileTemplateType.TileWideText03 );

                var title = item.Title;
                string titleText = title.Text == null ? String.Empty : title.Text;
                tileXml.GetElementsByTagName( textElementName )[0].InnerText = titleText;

                // Create a new tile notification.
                updater.Update( new TileNotification( tileXml ) );

                // Don't create more than 5 notifications.
                if( itemCount++ > 5 ) break;
            }
        }

        // Although most HTTP servers do not require User-Agent header, others will reject the request or return
        // a different response if this header is missing. Use SetRequestHeader() to add custom headers.
        static string customHeaderName = "User-Agent";
        static string customHeaderValue = "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";

        static string textElementName = "text";
        static string feedUrl = @"http://blogs.msdn.com/b/MainFeed.aspx?Type=BlogsOnly";
    }
}
```

## <a name="set-up-the-package-manifest"></a>Configurer le manifeste du package


Pour configurer le manifeste du package, ouvrez-le et ajoutez une nouvelle déclaration de tâche en arrière-plan. Affectez au point d’entrée de la tâche le nom de la classe, y compris son espace de noms.

1.  Dans l’Explorateur de solutions, ouvrez Package.appxmanifest.
2.  Cliquez ou appuyez sur l’onglet **Déclarations**.
3.  Sous **Déclarations disponibles**, sélectionnez **BackgroundTasks**, puis cliquez sur **Ajouter**. Visual Studio ajoute **BackgroundTasks** sous **Déclarations prises en charge**.
4.  Sous **Types de tâches pris en charge**, vérifiez que la case **Minuterie** est cochée.
5.  Sous **Paramètres de l’application**, affectez **BackgroundTasks.BlogFeedBackgroundTask** au point d’entrée.
6.  Cliquez ou appuyez sur l’onglet **Interface utilisateur de l’application**.
7.  Affectez à **Notifications de verrouillage de l’écran** la valeur **Badge et texte de mosaïque**.
8.  Définissez un chemin d’accès à une icône de 24x24 pixels dans le champ **Logo du badge**.
    **Important cette icône**nedoitutiliserquedespixelsmonochromeettransparent  .
9.  Dans le champ **Petit logo**, définissez un chemin d’accès à une icône de 30x30 pixels.
10. Dans le champ **Logo large**, définissez un chemin d’accès vers une icône de 310x150 pixels.

## <a name="register-the-background-task"></a>Inscrire la tâche en arrière-plan


Créez un élément [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) pour inscrire votre tâche.

> **Remarque à compter**de Windows 8.1, les paramètres d’inscription des tâches en arrière-plan sont validés au moment de l’inscription.   Si l’un des paramètres d’inscription n’est pas valide, une erreur est renvoyée. Votre application doit être en mesure de gérer les scénarios dans lesquels l’inscription de la tâche en arrière-plan échoue. Par exemple utilisez une instruction conditionnelle pour rechercher les erreurs d’inscription, puis retentez l’inscription qui a échoué avec d’autres valeurs de paramètre.
 

Dans la page principale de votre application, ajoutez la méthode **RegisterBackgroundTask** et appelez-la dans le gestionnaire d’événements **OnNavigatedTo**.

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Windows.ApplicationModel.Background;
using Windows.Data.Xml.Dom;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;
using Windows.Web.Syndication;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/p/?LinkID=234238

namespace ContosoApp
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }

        /// <summary>
        /// Invoked when this page is about to be displayed in a Frame.
        /// </summary>
        /// <param name="e">Event data that describes how this page was reached.  The Parameter
        /// property is typically used to configure the page.</param>
        protected override void OnNavigatedTo( NavigationEventArgs e )
        {
            this.RegisterBackgroundTask();
        }


        private async void RegisterBackgroundTask()
        {
            var backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();
            if( backgroundAccessStatus == BackgroundAccessStatus.AllowedMayUseActiveRealTimeConnectivity ||
                backgroundAccessStatus == BackgroundAccessStatus.AllowedWithAlwaysOnRealTimeConnectivity )
            {
                foreach( var task in BackgroundTaskRegistration.AllTasks )
                {
                    if( task.Value.Name == taskName )
                    {
                        task.Value.Unregister( true );
                    }
                }

                BackgroundTaskBuilder taskBuilder = new BackgroundTaskBuilder();
                taskBuilder.Name = taskName;
                taskBuilder.TaskEntryPoint = taskEntryPoint;
                taskBuilder.SetTrigger( new TimeTrigger( 15, false ) );
                var registration = taskBuilder.Register();
            }
        }

        private const string taskName = "BlogFeedBackgroundTask";
        private const string taskEntryPoint = "BackgroundTasks.BlogFeedBackgroundTask";
    }
}
```

## <a name="debug-the-background-task"></a>Déboguer la tâche en arrière-plan


Pour déboguer la tâche en arrière-plan, définissez un point d’arrêt dans la méthode Run de la tâche. Dans la barre d’outils **Emplacement de débogage**, sélectionnez votre tâche en arrière-plan. Le système appelle immédiatement la méthode Run.

1.  Définissez un point d’arrêt dans la méthode Run de la tâche.
2.  Appuyez sur F5 ou sur **Déboguer &gt; Démarrer le débogage** pour déployer et exécuter l’application.
3.  Une fois l’application lancée, revenez à Visual Studio.
4.  Vérifiez que la barre d’outils **Emplacement de débogage** est visible. Elle se trouve dans le menu **Affichage &gt; Barres d’outils**.
5.  Dans la barre d’outils **Emplacement de débogage**, cliquez sur la liste déroulante **Interrompre** et sélectionnez **BlogFeedBackgroundTask**.
6.  Visual Studio interrompt l’exécution au niveau du point d’arrêt.
7.  Appuyez sur F5 ou sur **Déboguer &gt; Continuer** pour continuer à exécuter l’application.
8.  Appuyez sur Maj+F5 ou sur **Déboguer &gt; Arrêter le débogage** pour arrêter le débogage.
9.  Revenez à la vignette de l’application sur l’écran d’accueil. Après quelques secondes, les notifications par vignette apparaissent sur la vignette de votre application.

## <a name="related-topics"></a>Rubriques connexes


* [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
* [**TileUpdateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdateManager)
* [**TileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification)
* [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md)
* [Instructions et liste de vérification pour les vignettes et les badges](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles)

 

 
