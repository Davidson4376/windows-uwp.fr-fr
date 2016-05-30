---
author: mcleblanc
Description: 'Cette rubrique fournit un aperçu complet du point de vue développeurs de la manière dont la fonctionnalité de protection des données d’entreprise (EDP) est liée aux fichiers, aux mémoires tampons, au Presse-papiers, à la mise en réseau, aux tâches en arrière-plan et à la protection des données verrouillées.'
MS-HAID: 'dev\_enterprise.edp\_hub'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: 'Protection des données d’entreprise (EDP, Enterprise Data Protection)'
---

# Protection des données d’entreprise (EDP, Enterprise Data Protection)

__Remarque__ La stratégie de protection des données d’entreprise (EDP) ne peut pas être appliquée sur Windows 10, version 1511 (build 10586) ou antérieure.

Cette rubrique fournit un aperçu complet du point de vue développeurs de la manière dont la fonctionnalité de protection des données d’entreprise (EDP) est liée aux fichiers, aux mémoires tampons, au Presse-papiers, à la mise en réseau, aux tâches en arrière-plan et à la protection des données verrouillées.

Pour en savoir plus sur la protection EDP du point de vue des utilisateurs finaux et des administrateurs, voir [Vue d’ensemble de la protection des données d’entreprise](https://technet.microsoft.com/library/dn985838(v=vs.85).aspx).

## Qu’est-ce que la protection des données d’entreprise ?

La protection des données d’entreprise est un ensemble de fonctionnalités des ordinateurs de bureau, ordinateurs portables, tablettes et téléphones pour la gestion des périphériques mobiles (GPM). La fonctionnalité EDP offre à une entreprise une plus grande maîtrise sur la manière dont ses données (fichiers de l’entreprise et objets BLOB de données) sont gérées sur les appareils gérés par l’entreprise.

-   Les données d’entreprise sont marquées avec le chiffrement. Il s’agit de « données protégées par l’entreprise » ou simplement de « données protégées » pour faire plus court.
-   Seules les applications explicitement autorisées par le biais de la stratégie EDP par l’entreprise de gestion peuvent accéder aux données protégées par l’entreprise, par exemple dans les fichiers.
-   Seules les applications explicitement autorisées via la stratégie EDP disposent d’un accès au réseau privé virtuel (VPN).
-   La stratégie de restriction des applications détermine également la manière dont les applications autorisées doivent gérer les données d’entreprise.
-   Les restrictions basées sur des stratégies s’appliquent même au contenu d’entreprise échangé via le Presse-papiers Windows ou par le biais du contrat de partage.
-   L’entreprise de gestion peut, à la demande, révoquer l’accès au contenu protégé à l’appareil en effaçant essentiellement les données d’entreprise tout en conservant les données personnelles de l’appareil.
-   Une application de canal est une application qui télécharge des données protégées. Il peut s’agir d’applications de synchronisation de messagerie ou de fichiers.

EDP améliore le [système de fichiers EFS](http://technet.microsoft.com/library/cc700811.aspx) et la [réinitialisation sélective Windows](https://technet.microsoft.com/library/dn486874.aspx) pour fournir plus d’options de sécurité et de flexibilité. De nouvelles API de protection des données d’entreprise vous permettent de créer des applications qui protègent et empêchent l’accès au contenu d’entreprise, l’utilisation de propriétés de fichiers protégées et l’accès aux données chiffrées dans leur forme brute. Outre les nouvelles API de protection des fichiers et des données, des API de protection des tampons et des flux sont proposées. Un ensemble d’API qui permettent aux applications d’identifier et d’indiquer l’entreprise chargée d’appliquer la stratégie de protection des données est également introduit.

Afin que l’entreprise de gestion puisse contrôler l’accès à ses données protégées, la stratégie de restriction des applications définit une liste des applications et des restrictions associées à celles-ci. Par défaut, une application ne peut pas accéder aux données protégées de manière autonome. Pour y avoir accès, l’application doit être ajoutée à une liste nommée liste d’autorisation répertoriant les applications autorisées. Sur un appareil géré, Windows peut limiter et/ou auditer l’accès aux données protégées sur le Presse-papiers ou par le biais du contrat de partage, afin que l’accès par une application ne figurant pas sur la liste d’autorisation soit audité et/ou requiert le consentement de l’utilisateur, sous peine de blocage.

La stratégie de la fonctionnalité EDP est fournie à un appareil par l’entreprise de gestion (ce qui fait de l’appareil, un « appareil géré »). La mise en service de la stratégie peut s’effectuer via l’inscription au service de gestion des périphériques mobiles (GPM), une configuration manuelle informatique, ou via un autre mécanisme de gestion et de distribution des stratégies, tel que System Center Configuration Manager (SCCM).

La protection des fichiers EDP exploite les clés Rights Management Services (RMS), si elles sont configurées, dans la mesure où ces clés sont conservées d’un appareil à l’autre et, par conséquent, permettent l’itinérance des données protégées. En l’absence de clés RMS, ces API reviennent aux clés de réinitialisation sélective locales et limitent les fonctionnalités d’itinérance. Les données itinérantes chiffrées seront accessibles sur Windows de niveau inférieur et sur les appareils tiers via des applications RMS spécifiques à la plateforme fournies par Microsoft, ainsi qu’avec des applications tierces compatibles RMS.

En résumé, les données protégées par les API de protection des données d’entreprise peuvent être gérées par l’entreprise, afin que vous puissiez créer votre application de manière à permettre à l’entreprise de protéger et de gérer ses données. En d’autres termes, vous pouvez générer une application d’entreprise. Et, le reste de ce guide va vous aider dans cette démarche.

## Configurer votre ordinateur pour EDP


Afin de développer correctement votre application et de tester la manière dont elle se comporte dans l’entreprise, vous devrez configurer votre ordinateur ou appareil de manière appropriée. Cela implique de réaliser quelques tâches généralement du ressort des administrateurs informatiques.

-   Faites en sorte que votre ordinateur de développement soit inscrit au service de gestion des périphériques mobiles (GPM).
-   Ajoutez votre application à la liste d’autorisation via le [fournisseur de services de configuration AppLocker](https://msdn.microsoft.com/library/windows/hardware/dn920019).

La tâche suivante consiste à créer une application prenant en charge l’identité gérée de l’entreprise exécutée en interne et pouvant y répondre de manière dynamique et la stratégie de protection en vigueur. Cela veut dire « rendre votre application compatible ». Les applications compatibles avec la stratégie sont davantage susceptibles de se trouver sur la liste des applications autorisées à accéder aux données d’entreprise.

## Applications spécifiques aux entreprises


Une fois votre application figurant dans la liste, celle-ci peut lire les données protégées. Et, par défaut, toutes les sorties de données de votre application sont automatiquement protégées par le système. Cette protection automatique est mise en place dans la mesure où l’entreprise de gestion, doit d’une manière ou une autre, s’assurer que les données d’entreprise restent sous son propre contrôle. Toutefois, seule une surveillance étroite de ce type permet d’atteindre cet objectif. Il est préférable de demander au système de vous faire suffisamment confiance pour vous donner plus de pouvoir et de flexibilité. Et, le prix à payer est de rendre votre application plus intelligente. Cela implique non seulement de faire figurer l’application sur la liste d’autorisation, mais également de la rendre spécifique aux entreprises et de la déclarer comme tel.

Votre application est compatible dans la mesure où elle utilise les techniques que nous allons décrire pour protéger les données d’entreprise de manière autonome que les données soient au repos, en cours d’utilisation ou de transfert. Votre application compatible reconnaît les sources de données d’entreprise ainsi que les données d’entreprise et les protège lorsqu’elles arrivent dans votre application. Être compatible signifie également prendre en charge et obéir à la stratégie EDP chaque fois que des données quittent votre application. Cela implique d’empêcher le transfert de contenu vers un point de terminaison réseau n’appartenant pas à l’entreprise, en habillant les données sous forme chiffrée portable avant d’autoriser leur itinérance et en demandant potentiellement (selon les paramètres de stratégie) l’autorisation à l’utilisateur avant de coller les données d’entreprise dans une application qui ne figure pas sur la liste d’autorisation. Une fois votre application rendue compatible, celle-ci l’annonce au système en déclarant la fonctionnalité **enterpriseDataPolicy** restreinte. Pour en savoir plus sur l’utilisation des fonctionnalités restreintes, voir [Fonctionnalités restreintes et spécifiques](https://msdn.microsoft.com/library/windows/apps/mt270968#special_and_restricted_capabilities).

Dans l’idéal, toutes les données d’entreprise sont protégées, à la fois au repos et en cours de transfert. Toutefois, Il existe inévitablement une brève période entre le moment où les données sont générées et celui où elles sont protégées. Parfois, les données d’entreprise peuvent se trouver sur un point de terminaison réseau d’entreprise sans être chiffrées. Une application compatible est capable de protéger de manière autonome de telles données. Les applications autorisées mains non compatibles devront avoir une protection imposée par le système.

Cela vient du fait qu’une application non compatible s’exécute toujours en mode entreprise. Le système s’en assure. Toutefois, une application compatible est libre de passer du mode entreprise au mode personnel à tout moment et en fonction des données qu’elle utilise. Il est également important pour une application compatible de respecter les données personnelles et de ne pas les marquer comme des données d’entreprise. Une application compatible peut gérer simultanément des données d’entreprise et des données personnelles, tant que ces promesses sont tenues. La section suivante montre comment basculer entre ces modes dans le code.

## Confirmer la gestion d’une identité et déterminer le niveau de mise en œuvre d’une stratégie de protection

En règle générale, votre application obtient une identité d’entreprise à partir d’une ressource externe telle qu’une adresse de messagerie, ou d’un domaine qui est géré ou d’un nom d’hôte d’uri. Vous pouvez appeler [**ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync**](https://msdn.microsoft.com/library/windows/apps/dn706027) pour obtenir l’identité gérée, le cas échéant, pour un nom d’hôte de point de terminaison réseau.

L’exemple de code suivant illustre la structure générale d’un comportement compatible, notamment comment déterminer si une identité d’entreprise est réellement gérée et quelle stratégie est actuellement en vigueur.

```CSharp
using Windows.Security.EnterpriseData;

...

string identity = "contoso.com";

if (ProtectionPolicyManager.IsIdentityManaged(identity))
{
    EnforcementLevel enforcementLevel = ProtectionPolicyManager.GetEnforcementLevel();

    // During UI activities or network access, call ProtectionPolicyManager APIs
    // (taking the enforcement level into account) to ensure that the system
    // tags data with the identity as appropriate.

    ProtectionPolicyManager.GetForCurrentView().Identity = identity;
    // The app is now in enterprise mode.

    ProtectionPolicyManager.GetForCurrentView().Identity = string.Empty;
    // The app is back in personal mode.
}
else
{
    // No policy enforcement is done on this identity.
}
```

Comme indiqué, vous déterminez d’abord si la stratégie EDP est définie pour l’identité de l’entreprise. Le terme « géré » est l’abréviation de « géré par une stratégie EDP ». Lorsque la stratégie EDP est définie pour une identité spécifique, [**ProtectionPolicyManager.IsIdentityManaged**](https://msdn.microsoft.com/library/windows/apps/dn705171) renvoie la valeur true pour cette identité. Il s’agit du signal indiquant qu’il convient d’utiliser des API EDP avec cette identité. Bien que le fichier et les API de mémoire tampon soient quelque peu exceptionnels dans la mesure où ils fonctionnent même pour une identité non gérée, ce scénario n’a pas d’intérêt. Si un appareil est géré, celui-ci est géré pour une identité d’entreprise. Si une identité n’est pas gérée, la protection des données de cette identité est inutile.

L’étape suivante consiste à déterminer et mettre en œuvre le niveau de mise en œuvre de la stratégie. Pour déterminer le niveau de mise en œuvre de la stratégie, appelez la méthode [**GetEnforcementLevel**](https://msdn.microsoft.com/library/windows/apps/mt608406). Lorsqu’une stratégie d’entreprise est appliquée sur l’identité, votre application compatible doit aider le système à mettre en œuvre la stratégie en appelant les API [**ProtectionPolicyManager**](https://msdn.microsoft.com/library/windows/apps/dn705170) au cours des activités de l’interface utilisateur ou des accès au réseau pour s’assurer que le système marque les transferts de données avec cette identité si nécessaire. Vous en saurez plus sur la façon d’interpréter le niveau de mise en œuvre et de mettre en pratique en consultant ce guide. L’exemple de code montre également comment passer en mode entreprise et revenir au mode personnel, en définissant la valeur [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) sur l’identité d’entreprise, ou sur la chaîne vide, respectivement. De nouveau, notez que l’entrée et la sortie du mode entreprise n’a de sens qu’avec une identité gérée.

## Fonctionnalités EDP en un coup d’œil


**Protection des fichiers et de la mémoire tampon.**

-   Votre application peut protéger, conteneuriser et effacer les données associées à une identité d’entreprise.
-   La gestion des clés est gérée par Windows. Windows utilise les clés RMS de l’entreprise lorsqu’elles sont disponibles sur l’appareil. Dans le cas contraire, Windows bascule en protection par réinitialisation sélective locale.

**Gestion des stratégies de l’appareil.**

-   Votre application peut demander l’identité (entreprise ou organisation) qui gère l’appareil.
-   Votre application peut protéger les utilisateurs contre la divulgation accidentelle de données en associant une identité avec les données en question.
-   Votre application peut protéger les ressources d’entreprise sur le réseau en activant les connexions de point de terminaison réseau appartenant à l’entreprise (serveurs, plages IP) et en associant les données à une identité gérée (autrement dit, inscrite à la gestion des périphériques mobiles).
-   Les API EDP fonctionnent seulement avec des identités gérées disposant d’une stratégie EDP définie sur l’appareil. Si une identité n’est pas gérée, les API l’indiquent à l’application, si nécessaire.

Voici une liste de liens vers des rubriques décrivant les API EDP et les scénarios spécifiques de ces fonctionnalités.

## Fichiers

Voir [Utiliser la fonctionnalité EDP pour protéger des fichiers](../files/protect-your-enterprise-data-with-edp.md).

## Flux et mémoires tampons

Voir [Utiliser la fonctionnalité EDP pour protéger les flux et les mémoires tampons](../files/use-edp-to-protect-streams-and-buffers.md).

## Presse-papiers, partage et échange de données entre les applications

Voir [Utiliser EDP pour protéger les données d’entreprise transférées entre les applications](../app-to-app/use-edp-to-protect-enterprise-data-transferred-between-apps.md).

## Mise en réseau

Voir [Marquage de connexions réseau avec l’identité EDP](../networking/tagging_network_connections_with_edp_identity.md).

## Protection des données verrouillées et tâches en arrière-plan

Une organisation peut choisir d’administrer une stratégie sécurisée de protection des données verrouillées, dans laquelle les clés de chiffrement nécessaires pour accéder aux ressources protégées sont temporairement supprimées de la mémoire d’un appareil lorsqu’il est verrouillé. Pour préparer votre application à cette éventualité, consultez la section [Gérer les événements de verrouillage d’un appareil et éviter de laisser du contenu non protégé en mémoire](#handle_lock_events) de cette rubrique. En outre, si votre application a une tâche en arrière-plan qui nécessite de protéger les fichiers, voir [Protéger les données d’entreprise dans un nouveau fichier (pour une tâche en arrière-plan)](../files/protect-your-enterprise-data-with-edp.md#protect_data_new_file_bg).

## Mise en œuvre de la stratégie de l’interface utilisateur


Dans cette section, nous allons prendre l’exemple d’une application de messagerie compatible comportant une boîte aux lettres d’entreprise parmi un ensemble de boîtes aux lettres incluant à la fois des boîtes aux lettres d’entreprise et personnelles qui appartiennent à l’utilisateur. Pour vous assurer qu’il n’y a aucune fuite de données d’entreprise depuis la boîte aux lettres d’entreprise, l’application appelle [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791) pour s’assurer que le système d’exploitation connaît le contexte actuel de l’application (entreprise ou personnel). L’API renvoie false si l’identité n’est pas gérée par une stratégie d’entreprise.

```CSharp
using Windows.Security.EnterpriseData;

...

public class Mailbox
{
    public bool HasEnterpriseMail { get { /* implementation */ } }
    public string Identity { get { /* implementation */ } }
}

private void SwitchMailbox(Mailbox targetMailbox)
{
    // Code goes here to perform setup for "targetMailbox".

    if (targetMailbox.HasEnterpriseMail)
    {
        bool result = 
            ProtectionPolicyManager.TryApplyProcessUIPolicy(targetMailbox.Identity);

        // Code goes here to process "result", which indicates whether
        // or not policy enforcement is in place for the identity.
    }
    else
    {
        // For personal mailboxes, we clear policy enforcement (in case
        // it is still set from when we processed an enterprise mailbox).
        ProtectionPolicyManager.ClearProcessUIPolicy();
    }
}
```

## Gérer les événements de verrouillage d’un appareil et éviter de laisser du contenu non protégé en mémoire


Dans ce scénario, nous allons prendre l’exemple d’une application de messagerie compatible conçue pour gérer la messagerie d’entreprise et la messagerie personnelle. Lorsque l’application s’exécute dans une organisation qui a choisi d’administrer une stratégie de protection des données verrouillées sécurisée, cette application doit veiller à supprimer toutes les données sensibles de la mémoire lorsque l’appareil est verrouillé. Pour ce faire, elle s’inscrit aux événements [**ProtectionPolicyManager.ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787) et [**ProtectionPolicyManager.ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786) afin d’être notifiée du verrouillage ou du déverrouillage de l’appareil (si DPL est en vigueur).

[
            **ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787) est déclenché avant la suppression temporaire des clés de protection des données configurées sur l’appareil. Ces clés sont supprimées lorsque l’appareil est verrouillé pour éviter tout accès non autorisé aux données chiffrées lorsque l’appareil est verrouillé et aussi éventuellement lorsque son propriétaire n’est pas en sa possession. [
            **ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786) est déclenché une fois que les clés sont de nouveau disponibles lors du déverrouillage de l’appareil.

En gérant ces deux événements, l’application peut s’assurer de la protection de tout contenu sensible se trouvant en mémoire avec [**DataProtectionManager**](https://msdn.microsoft.com/library/windows/apps/dn706017). Elle doit également fermer les flux de fichier ouverts sur ses fichiers protégés pour s’assurer que le système ne met pas en cache les données sensibles en mémoire. Vous pouvez le faire de plusieurs manières. Pour fermer un flux de fichiers renvoyé à partir d’une méthode Open d’un **StorageFile**, vous pouvez appeler la méthode **Dispose** sur le flux. Vous pouvez limiter l’utilisation du flux à l’aide d’une instruction d’utilisation (C\# ou VB). Ou, vous pouvez encapsuler un objet **DataReader** ou **DataWriter** autour du flux et utiliser la méthode **Dispose** ou l’instruction d’utilisation avec cet objet.

**Remarque**  
Dans un environnement sans stratégie DPL configurée, l’événement [**ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786) est déclenché, mais pas [**ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787). Tenez compte de cela dans votre code et veillez à ne pas supposer que les événements vont toujours par paire sur tous les systèmes et que vous pouvez toujours utiliser les événements pour déterminer l’état verrouillé/déverrouillé de l’appareil. L’exemple de code suivant montre que nous veillons à ne pas supposer quoi que soit concernant l’état de protection de l’e-mail affiché ou l’état d’ouverture du flux de fichier de base de données.

En outre, n’oubliez pas que lors du déverrouillage d’un appareil sans stratégie DPL configurée, [**ProtectedAccessResumedEventArgs.Identities**](https://msdn.microsoft.com/library/windows/apps/dn705772) est une collection vide.

Par souci de brièveté, cet exemple de code est légèrement simplifié et se concentre sur le traitement d’un e-mail d’entreprise. Dans une application réelle, les e-mails personnels seraient écrits dans un fichier de base de données de messagerie différent, et ne seraient pas protégés en cas de verrouillage.

```CSharp
using Windows.Security.Cryptography;
using Windows.Security.EnterpriseData;
using Windows.Storage;
using Windows.Storage.Streams;

...

public class DisplayedMail
{
    public string Body { get; set; }
    public IBuffer ProtectedBody { get; set; }
    public bool IsProtected { get; set; }
}

private IOutputStream mailDatabaseStream = null;
private string currentlyDisplayedMailIdentity = "contoso.com";
private DisplayedMail currentlyDisplayedMail = new DisplayedMail()
    { Body = "Lorem ipsum dolor...", ProtectedBody = null, IsProtected = false };

// Gets the app's protected mail database file, then opens and stores a stream on it.
private async void OpenMailDatabase()
{
    // Only attempt to open the database file stream if we know it's closed.
    if (this.mailDatabaseStream == null)
    {
        StorageFolder appDataStorageFolder = ApplicationData.Current.LocalFolder;
        StorageFile storageFile = await appDataStorageFolder.GetFileAsync("maildb.dat");
        using (IRandomAccessStream randomAccessStream =
            await storageFile.OpenAsync(FileAccessMode.ReadWrite))
        {
            this.mailDatabaseStream = randomAccessStream.GetOutputStreamAt(0);
        }
    }
}

// Called once by the app when starting up.
private void AppSetup()
{
    ProtectionPolicyManager.ProtectedAccessSuspending +=
        this.ProtectionPolicyManager_ProtectedAccessSuspending;
    ProtectionPolicyManager.ProtectedAccessResumed +=
        this.ProtectionPolicyManager_ProtectedAccessResumed;
    this.OpenMailDatabase();
}

// Background work called when the app receives an email.
private async void AppMailReceived(string fauxEmail)
{
    // Only attempt to write to the database file stream if we know it's open.
    if (this.mailDatabaseStream != null)
    {
        IBuffer emailAsBuffer = CryptographicBuffer.ConvertStringToBinary
            (fauxEmail, BinaryStringEncoding.Utf8);
        await this.mailDatabaseStream.WriteAsync(emailAsBuffer);
        await this.mailDatabaseStream.FlushAsync();
    }
    else
    {
        // Code goes here to handle the case where the device is
        // locked and we can't access the protected mail database.
    }
}

// Called by ProtectionPolicyManager when the device is locked if under DPL.
private async void ProtectionPolicyManager_ProtectedAccessSuspending
    (object sender, ProtectedAccessSuspendingEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.currentlyDisplayedMailIdentity))
    {
        // This event is not for our identity. Another will be sent for our identity.
        return;
    }

    // Get suspension deferral.
    Windows.Foundation.Deferral deferral = e.GetDeferral();

    // Protect the displayed mail content.
    if (!this.currentlyDisplayedMail.IsProtected)
    {
        IBuffer mailBodyBuffer = CryptographicBuffer.ConvertStringToBinary
            (this.currentlyDisplayedMail.Body, BinaryStringEncoding.Utf8);
        BufferProtectUnprotectResult result = await DataProtectionManager.ProtectAsync
            (mailBodyBuffer, this.currentlyDisplayedMailIdentity);
        if (result.ProtectionInfo.Status == DataProtectionStatus.Protected)
        {
            // Save the protected version and clear the unprotected version.
            this.currentlyDisplayedMail.ProtectedBody = result.Buffer;
            this.currentlyDisplayedMail.Body = null;
        }
    }

    // Close the mail database file stream to make sure that we have no unprotected
    // content in memory.
    this.mailDatabaseStream.Dispose();
    this.mailDatabaseStream = null;

    // Optionally, code goes here to use e.Deadline to determine whether we have more
    // than 15 seconds left before the suspension deadline. If we do then process any
    // messages queued up for sending while we are still able to access them.

    // All done. Complete deferral.
    deferral.Complete();
}

// Called by ProtectionPolicyManager when the device is unlocked.
private async void ProtectionPolicyManager_ProtectedAccessResumed
    (object sender, ProtectedAccessResumedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.currentlyDisplayedMailIdentity))
    {
        // This event is not for our identity. Another will be sent for our identity.
        return;
    }

    // Unprotect the displayed mail content.
    if (this.currentlyDisplayedMail.IsProtected)
    {
        BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync
            (this.currentlyDisplayedMail.ProtectedBody);
        if (result.ProtectionInfo.Status == DataProtectionStatus.Unprotected)
        {
            // Restore the unprotected version.
            this.currentlyDisplayedMail.Body = CryptographicBuffer.ConvertBinaryToString
                (BinaryStringEncoding.Utf8, result.Buffer);
            this.currentlyDisplayedMail.ProtectedBody = null;
        }
    }

    // Reopen the closed mail database.
    this.OpenMailDatabase();
}
```

## S’inscrire pour être averti en cas de révocation d’un contenu protégé


Imaginez un scénario dans lequel une application de messagerie a configuré une boîte aux lettres d’entreprise sur l’appareil d’un utilisateur. À un moment donné et pour l’une des raisons possibles, l’entreprise souhaite révoquer l’accès aux e-mails protégés de l’entreprise et aux autres ressources de cet appareil. Il existe plusieurs raisons possibles pour la révocation. Il est plus probable que ce ne soit pas la stratégie d’entreprise spécifique qui déclenche une révocation à la désinscription. Dans ce cas, un scénario consiste à ce que l’utilisateur ait désinscrit l’appareil de l’entreprise (peut-être s’il offre ou vend l’appareil, s’il veut en utiliser un autre, s’il change de travail ou s’il part en retraite). Un autre scénario, consiste à ce qu’une notification de désinscription soit envoyée par un administrateur informatique à distance via la gestion des périphériques mobiles (GPM), peut-être si un appareil est considéré comme perdu.

Quel que soit le cas, l’entreprise envoie une demande d’effacement de tous les e-mails de l’appareil de l’utilisateur, dans la mesure où ils ne sont plus nécessaires sur celui-ci. Le client de gestion à distance de l’appareil reçoit la demande issue du serveur de gestion à distance de l’entreprise et appelle [**ProtectionPolicyManager.RevokeContent**](https://msdn.microsoft.com/library/windows/apps/dn705790) pour révoquer les clés requises pour accéder au contenu protégé sur cet appareil selon cette identité d’entreprise.

Si votre application doit être avertie en cas de révocation, vous pouvez vous inscrire avec l’événement [**ProtectionPolicyManager.ProtectedContentRevoked**](https://msdn.microsoft.com/library/windows/apps/dn705788). Lorsque votre application reçoit l’événement, elle peut supprimer toutes les métadonnées associées à la boîte aux lettres d’entreprise, qui ne seront plus nécessaires.

```CSharp
using Windows.Security.EnterpriseData;

...

private string mailIdentity = "contoso.com";

void MailAppSetup()
{
    ProtectionPolicyManager.ProtectedContentRevoked += ProtectionPolicyManager_ProtectedContentRevoked;
    // Code goes here to set up mailbox for 'mailIdentity'.
}

private void ProtectionPolicyManager_ProtectedContentRevoked(object sender, ProtectedContentRevokedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.mailIdentity))
    {
        // This event is not for our identity.
        return;
    }

    // Code goes here to delete any metadata associated with 'mailIdentity'.
}
```

## Le client de gestion à distance lance un effacement des données protégées par l’entreprise


Un utilisateur a plusieurs fichiers d’entreprise protégés selon l’identité d’entreprise sur son appareil personnel. L’utilisateur perd son appareil. Dès que l’entreprise est informée de la perte de l’appareil, celle-ci envoie une demande d’effacement de toutes les données sensibles de l’appareil de l’utilisateur afin d’éviter toute fuite de données. Le client de gestion à distance de l’appareil reçoit la demande issue du serveur de gestion à distance de l’entreprise et appelle [**ProtectionPolicyManager.RevokeContent**](https://msdn.microsoft.com/library/windows/apps/dn705790) pour révoquer les clés requises pour accéder au contenu protégé selon l’identité d’entreprise.

```CSharp
Windows.Security.EnterpriseData.ProtectionPolicyManager.RevokeContent("contoso.com");
```

 

 





<!--HONumber=May16_HO2-->


