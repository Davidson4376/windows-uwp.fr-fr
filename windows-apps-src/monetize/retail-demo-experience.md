---
author: joannaleecy
title: "Créer une application de démonstration commerciale"
description: "Vous pouvez créer une application de démonstration commerciale (RDX), c’est-à-dire une application qui peut être démarrée en mode démo et en mode normal."
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.author: joanlee
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, application de démonstration commerciale"
ms.openlocfilehash: 2fa839c3a559ff0065c98712f73b9bb3ed81b276
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
#  <a name="create-a-retail-demo-experience-rdx-app"></a>Créer une application de démonstration commerciale (RDX)

Lorsque les clients entrent dans un magasin ou un lieu de vente, ils s’attendent à ce que les derniers ordinateurs et téléphones mobiles y soient présentés. Il s’agit d’appareils de démonstration commerciale.
Les appareils de démonstration commerciale et les contenus qui y sont installés jouent un rôle important dans l’expérience des clients en magasin. Ces clients passent en effet généralement un long moment à les découvrir et les tester.

Les applications installées sur ces ordinateurs et ces téléphones mobiles en magasin doivent être des applications de démonstration commerciale (RDX). Cet article offre une vue d’ensemble de la conception et du développement de la version de démonstration commerciale d’une application à installer sur les ordinateurs et les appareils mobiles de démonstration en magasin.

Il existe une seule build de l’application de démonstration commerciale, qui peut être démarrée dans deux modes différents: _normal_ ou _commercial_.
Du point de vue de nos clients, il n’existe qu’une seule application. Pour les aider à faire la distinction entre les deux versions, il est préférable que l’application affiche le mot «Commercial» dans la barre de titre ou dans un emplacement approprié lors de son exécution en mode démonstration commerciale.

En plus des exigences du WindowsStore relatives aux applications, les applicationsRDX doivent également être entièrement compatibles avec le système de configuration, de nettoyage et de mise à jour des appareils de démonstration commerciale pour garantir que les clients bénéficient d’une expérience homogène et positive en magasin.

## <a name="design-principles"></a>Principes de conception

### <a name="show-your-best"></a>Afficher vos points forts

Utilisez la version de démonstration commerciale pour montrer tous les points forts de votre application.  C’est probablement la première fois que votre client verra votre application, vous devez donc la montrer sous son meilleur jour.

### <a name="show-it-fast"></a>Affichez-les rapidement

Les clients sont parfois impatients. Plus un utilisateur peut constater rapidement les points forts de votre application, mieux c’est.

### <a name="keep-the-story-simple"></a>Simplifiez l’utilisation

N’oubliez pas que la démonstration commerciale sert de révélateur des véritables atouts de votre application.

### <a name="focus-on-the-experience"></a>Concentrez-vous sur l’expérience

Donnez à l’utilisateur le temps d’assimiler votre contenu.  S’il est essentiel de les amener rapidement à découvrir les points forts, il faut également aménager des pauses pour leur permettre de profiter pleinement de l’expérience.

## <a name="technical-requirements"></a>Exigences techniques

Les applications de démonstration commerciale sont destinées à mettre en valeur les points forts de votre application pour les clients en magasin. Il est essentiel qu’elles satisfassent ces exigences techniques et se conforment aux réglementations relatives à la confidentialité du Windows Store qui s’appliquent à l’ensemble des applications de démonstration commerciale.
Vous pouvez également vous en servir comme liste de contrôle pour préparer le processus de validation et rendre le processus de test aussi clair que possible. Notez que ces exigences doivent être satisfaites non seulement pendant le processus de validation, mais également pendant toute la durée de vie de l’application de démonstration commerciale tant qu’elle est exécutée sur les appareils de démonstration commerciale.

### <a name="critical-level-requirements"></a>Exigences critiques

Les applicationsRDX ne satisfaisant pas ces exigences critiques seront supprimées de tous les appareils de démonstration commerciale dès que possible.

* Aucune demande d’informations d’identification personnelle

    L’application n’est pas autorisée à demander aux clients des informations d’identification personnelle.  Cela inclut toutes les informations de compte Microsoft, les coordonnées et ainsi de suite.

* Expérience exempte d’erreurs

    Votre application doit s’exécuter sans erreur. En outre, aucune fenêtre ou notification d’erreur ne doit s’afficher lorsque les clients utilisent les appareils de démonstration commerciale. Cela est important car nous voulons faire bénéficier nos clients des meilleures expériences, et donc d’expériences sans erreur.
    Par ailleurs, les erreurs ont également un impact négatif sur l’application elle-même, sur votre marque et sur l’appareil qui l’exécute, sur la marque du fabricant de l’appareil et sur la marque Microsoft.

* Les applications payantes doivent proposer un mode d’évaluation

    Pour pouvoir être installée sur des appareils de démonstration commerciale, votre application doit être gratuite ou doit proposer un mode d’évaluation.  Les clients ne souhaitent pas payer pour une expérience en magasin. Pour plus d’informations, consultez [Exclure ou limiter des fonctionnalités de la version d’évaluation](https://msdn.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app).

### <a name="high-priority-requirements"></a>Exigences de priorité élevée

Les applications RDX qui ne satisfont pas les exigences de priorité élevée doivent être examinées en vue d’être corrigées immédiatement. En l’absence de correctif applicable, cette application sera probablement supprimée des appareils de démonstration commerciale.

* Expérience mémorable hors connexion

    Votre application de démonstration commerciale doit fournir une excellente expérience hors connexion dans la mesure où environ 50% des appareils sont hors connexion dans les magasins. Vous devez vous assurer que les clients qui interagissent avec votre application hors connexion vivent une expérience positive et significative.

* Utilisation de contenus à jour

    Pour offrir la meilleure expérience possible, votre application doit toujours être à jour et les clients ne doivent jamais être invités à effectuer des mises à jour lorsque votre application est en ligne.

* Aucune communication anonyme

    Dans la mesure où un client utilisant un appareil de démonstration commerciale, est un utilisateur anonyme, il ne doit pas pouvoir envoyer des messages ou partager du contenu à partir de l’appareil.

* Proposition d’une expérience cohérente en utilisant le processus de nettoyage

    Chaque client doit vivre la même expérience lorsqu’il utilise un appareil de démonstration commerciale. Votre application doit utiliser le [processus de nettoyage](#clean-up-process) pour revenir au même état par défaut après chaque utilisation, car nous ne souhaitons pas que le client suivant voit ce que le client précédent a fait.  Cela inclut les scores, les réussites et les déverrouillages.

* Contenu approprié par âge

    Tous les contenus d’une application de démonstration commerciale doivent être affectés à une catégorie Adolescents ou inférieure. Pour plus d’informations, voir [comment obtenir une classification IARC de votre application](https://www.globalratings.com/for-developers.aspx) et les [classifications ESRB](https://www.esrb.org/ratings/ratings_guide.aspx).

### <a name="medium-priority-requirements"></a>Exigences de priorité moyenne

L’équipe commerciale Windows Store peut contacter directement les développeurs pour discuter de la manière de corriger ces problèmes.

* Capacité à s’exécuter correctement sur différents appareils

    Les applications de démonstration commerciale doivent s’exécuter correctement sur tous les appareils, y compris les appareils ayant des spécifications bas de gamme. Si l’application de démonstration commerciale est installée sur des appareils ne satisfaisant pas les spécifications minimales requises pour exécuter l’application, celle-ci doit clairement en informer l’utilisateur. La configuration minimale requise pour l’appareil doit être connue des clients afin qu’elle soit toujours exécutée avec des performances élevées.

* Satisfaction des exigences de taille de l’application commerciale

    L’application doit être inférieure à 800Mo. Si votre application ne satisfait pas les conditions de taille requises, contactez l’équipe commerciale Windows Store directement pour en discuter.

## <a name="preparing-codebase-for-retail-demo-mode-development"></a>Préparation du code base pour le développement du mode démo commerciale

La propriété [**IsDemoModeEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.retailinfo.isdemomodeenabled.aspx) dans la classe utilitaire [**RetailInfo**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.retailinfo.aspx), qui fait partie de l’espace de noms [Windows.System.Profile](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.aspx) dans le Kit de développement logiciel (SDK) Windows10, est utilisée comme indicateur booléen pour spécifier le chemin de code sur lequel votre application s’exécute: mode _normal_ ou mode _commercial_.

Quand [**RetailInfo.IsDemoModeEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.retailinfo.isdemomodeenabled.aspx) renvoie la valeur true, vous pouvez exécuter une requête concernant différentes propriétés de l’appareil à l’aide de [**RetailInfo.Properties**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.retailinfo.properties.aspx) pour créer une expérience de démonstration commerciale plus personnalisée. Ces propriétés incluent [**ManufacturerName**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.knownretailinfoproperties.manufacturername.aspx), [**Screensize**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.knownretailinfoproperties.screensize.aspx), [**Memory**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.knownretailinfoproperties.memory.aspx) et ainsi de suite.


## <a name="clean-up-process"></a>Processus de nettoyage

Le processus de nettoyage permet de rétablir automatiquement les paramètres par défaut d’origine des appareils de démonstration commerciale lorsqu’il n’y a aucune interaction avec l’appareil pendant une durée fixe. Cela permet de s’assurer que tous les utilisateurs du magasin vivent la même expérience prévue par défaut lors de leur interaction avec un appareil. Lors du développement d’une application de démonstration commerciale, il est essentiel de comprendre quand et comment le processus de nettoyage est déclenché et ce qui se passe pendant le processus de nettoyage par défaut. Vous devez également apprendre à personnaliser le processus de nettoyage de manière à satisfaire les conditions requises pour l’expérience de démonstration commerciale prévue.

### <a name="when-does-clean-up-begin"></a>Quand commence le nettoyage?

La séquence de nettoyage commence après une certaine durée d’inactivité de l’appareil. La durée d’inactivité commence lorsqu’il n’y a plus aucune saisie via l’écran tactile, la souris et le clavier de l’appareil.

#### <a name="desktoppc"></a>Ordinateur de bureau

Après 120secondes d’inactivité, la lecture de la vidéo d’attrait de l’application commence. 5secondes plus tard, le processus de nettoyage débute.

#### <a name="phone"></a>Téléphone

Après 60secondes d’inactivité, la lecture de la vidéo d’attrait de l’application commence et le processus de nettoyage débute immédiatement.

### <a name="what-happens-during-a-default-clean-up-process"></a>Que se passe-t-il pendant un processus de nettoyage par défaut?

#### <a name="step-1-clean-up"></a>Étape1: nettoyage
* Toutes les applications Win32 et du Windows Store sont fermées
* Tous les fichiers des dossiers connus comme __Images__, __Vidéos__, __Musique__, __Documents__, __Photos enregistrées__, __Pellicule__, __Bureau__ et __Téléchargements__ sont supprimés.
* Les états d’itinérance non structurés et structurés sont supprimés
* Les états locaux structurés sont supprimés

#### <a name="step-2-set-up"></a>Étape2: configuration
* Pour les appareils hors connexion: les dossiers restent vides
* Pour les appareils en ligne: les ressources de démonstration commerciale peuvent être transférées vers l’appareil à partir du Windows Store

### <a name="how-to-store-data-across-user-sessions"></a>Comment stocker les données entre des sessions utilisateur?

Si vous voulez stocker des données d’une session utilisateur à l’autre, vous pouvez les stocker dans __ApplicationData.Current.TemporaryFolder__ car le processus de nettoyage par défaut ne supprime pas automatiquement les données dans ce dossier. Notez que les informations stockées à l’aide de *LocalState* sont supprimées pendant le processus de nettoyage.

### <a name="how-to-customize-the-clean-up-process"></a>Comment personnaliser le processus de nettoyage?

Si vous souhaitez personnaliser le processus de nettoyage, vous devez implémenter le service d’application `Microsoft-RetailDemo-Cleanup` dans votre application.

Les scénarios exigeant une logique de nettoyage personnalisée sont les suivants: exécution d’une installation coûteuse, téléchargement et mise en cache de données ou volonté de ne pas supprimer les données *LocalState*.

Étape 1: Déclarez le service _Microsoft-RetailDemo-Cleanup_ dans votre manifeste d’application.
``` CSharp
  <Applications>
      <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MyCompany.MyApp.RDXCustomCleanupTask">
          <uap:AppService Name="Microsoft-RetailDemo-Cleanup" />
        </uap:Extension>
      </Extensions>
   </Application>
  </Applications>

```

Étape 2: Implémentez votre logique de nettoyage personnalisée sous la fonction de cas _AppdataCleanup_ à l’aide de l’exemple de modèle ci-dessous.
``` CSharp
using System;
using System.IO;
using System.Runtime.Serialization.Json;
using System.Threading;
using System.Threading.Tasks;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;
using Windows.Storage;

namespace MyCompany.MyApp
{
    public sealed class RDXCustomCleanupTask : IBackgroundTask
    {
        BackgroundTaskCancellationReason _cancelReason = BackgroundTaskCancellationReason.Abort;
        BackgroundTaskDeferral _deferral = null;
        IBackgroundTaskInstance _taskInstance = null;
        AppServiceConnection _appServiceConnection = null;

        const string MessageCommand = "Command";

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get the deferral object from the task instance, and take a reference to the taskInstance;
            _deferral = taskInstance.GetDeferral();
            _taskInstance = taskInstance;
            _taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

            AppServiceTriggerDetails appService = _taskInstance.TriggerDetails as AppServiceTriggerDetails;
            if ((appService != null) && (appService.Name == "Microsoft-RetailDemo-Cleanup"))
            {
                _appServiceConnection = appService.AppServiceConnection;
                _appServiceConnection.RequestReceived += _appServiceConnection_RequestReceived;
                _appServiceConnection.ServiceClosed += _appServiceConnection_ServiceClosed;
            }
            else
            {
                _deferral.Complete();
            }
        }

        void _appServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
        {
        }

        async void _appServiceConnection_RequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            //Get a deferral because we will be calling async code
            AppServiceDeferral requestDeferral = args.GetDeferral();
            string command = null;
            var returnData = new ValueSet();

            try
            {
                ValueSet message = args.Request.Message;
                if (message.ContainsKey(MessageCommand))
                {
                    command = message[MessageCommand] as string;
                }

                if (command != null)
                {
                    switch (command)
                    {
                        case "AppdataCleanup":
                            {
                                // Do custom clean up logic here
                                break;
                            }
                    }
                }
            }
            catch (Exception e)
            {
            }
            finally
            {
                requestDeferral.Complete();
                // Also release the task deferral since we only process one request per instance.
                _deferral.Complete();
            }
        }

        private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            _cancelReason = reason;
        }
    }
}
```

## <a name="related-links"></a>Liens connexes

* [Stocker et récupérer des données d’application](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)
* [Comment créer et utiliser un service d’application](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)
* [Localiser les contenus d’une application](https://msdn.microsoft.com/windows/uwp/globalizing/globalizing-portal)


 

 
