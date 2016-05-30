---
author: awkoren
Description: 'Cette rubrique présente des exemples de tâches de codage nécessaires pour réaliser certains des scénarios EDP relatifs au transfert de fichiers les plus courants.'
MS-HAID: 'dev\_app\_to\_app.use\_edp\_to\_protect\_enterprise\_data\_transferred\_between\_apps'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: Utiliser EDP pour protéger les données d’entreprise transférées entre les applications
---

# Utiliser EDP pour protéger les données d’entreprise transférées entre les applications

__Remarque__ La stratégie de protection des données d’entreprise (EDP) ne peut pas être appliquée sur Windows 10, version 1511 (build 10586) ou antérieure.

Cette rubrique présente des exemples de tâches de codage nécessaires pour réaliser certains des scénarios EDP relatifs au transfert de fichiers les plus courants. Pour un aperçu complet du point de vue des développeurs de la manière dont la fonctionnalité EDP est liée aux fichiers, aux flux, au Presse-papiers, à la mise en réseau, aux tâches en arrière-plan et à la protection des données verrouillées, voir [protection des données d’entreprise (EDP)](../enterprise/edp-hub.md).

**Remarque** L’[exemple de protection des données d’entreprise (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409) couvre nombre des scénarios expliqués dans cette rubrique.

## Prérequis


-   **Préparation pour la configuration de la fonctionnalité EDP**

    Voir [Configurer votre ordinateur pour EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP).

-   **Impliquez-vous dans la création d’une application spécifique aux entreprises**

    Une application qui assure de manière autonome que les données d’entreprise restent sous le contrôle des entreprises est véritablement spécifique aux entreprises. Une application spécifique aux entreprises est plus puissante, plus intelligente, plus flexible et plus fiable qu’une application ordinaire. Votre application indique au système qu’elle est spécifique en déclarant la fonctionnalité **enterpriseDataPolicy** restreinte. Mais configurer une fonctionnalité ne suffit pas à établir la spécificité. Pour en savoir plus, voir [Applications spécifiques aux entreprises](../enterprise/edp-hub.md#enterprise-enlightened-apps).

-   **Comprendre la programmation asynchrone pour les applications pour la plateforme Windows universelle (UWP)**

    Pour savoir comment écrire des applications asynchrones en C\# ou Visual Basic, voir [Appeler des API asynchrones en C\# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Pour savoir comment écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

## Source de Presse-papiers simple


Dans ce scénario, votre application est une application de type Bloc-notes qui gère les fichiers personnels et d’entreprise. Ici, votre application ne doit pas nécessairement changer sa logique de copier-coller ; elle doit simplement appeler [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791) chaque fois que l’utilisateur ouvre et affiche le contenu d’un document d’entreprise. Une fois le contenu affiché dans l’interface utilisateur de votre application, l’utilisateur peut ensuite le copier et le coller dans une autre application, ce qui explique pourquoi la configuration d’une stratégie de l’interface utilisateur est importante. Le système d’exploitation peut ainsi appliquer la stratégie actuellement définie à une opération de collage impliquant des données protégées. De même, il est important de supprimer la stratégie de l’interface utilisateur lorsqu’elle n’est plus nécessaire afin que l’utilisateur soit de nouveau libre de copier-coller des données personnelles. **TryApplyProcessUIPolicy** retourne false si son argument d’identité n’est pas géré par une stratégie d’entreprise.

```CSharp
using Windows.Security.EnterpriseData;

...

private void OnFileLoaded(FileProtectionInfo fileProtectionInfo, string contentsOfFile)
{
    if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy
            (fileProtectionInfo.Identity);
        if (isIdentityManaged)
        {
            // UI policy is now in effect for the file's identity.
        }
        else
        {
            // Enterprise policy is not in effect, because the file's identity
            // is not managed. In this case, we have a file protected to an
            // unmanaged identity, which is not a valid situation.
            // We still have to call ClearProcessUIPolicy if we want to clear the policy.
            ProtectionPolicyManager.ClearProcessUIPolicy();
        }
    }
    else
    {
        // In case we applied the policy for the previous file, now we need to clear it.
        ProtectionPolicyManager.ClearProcessUIPolicy();
    }
    // Code goes here to display "contentsOfFile" in your UI. Ready for copy-paste...
}
```

## Cible de Presse-papiers simple


Dans ce scénario, votre application est une application de messagerie électronique qui gère les comptes personnels et d’entreprise. L’utilisateur tente de coller des données dans une réponse par courrier électronique à l’aide de son compte personnel. Dans ce cas, avant que votre application récupère le contenu, elle doit simplement appeler [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636). Si l’accès est déjà possible, cette méthode est renvoyée une fois. Toutefois, si nous n’avez pas accès, la méthode entraîne l’affichage d’une boîte de dialogue demandant l’accord de l’utilisateur et « déverrouille » le package de données en cas d’accord. L’utilisateur peut ainsi annuler une opération de collage exécutée par erreur.

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private async void OnPasteSimple()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        // In case we don't already have acccess, request consent from the user
        // for us to access the clipboard data.
        await dataPackageView.RequestAccessAsync();

        try
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            // Code goes here to insert the text into the email...
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the clipboard.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

## La cible du Presse-papiers est un document neutre vide


Dans ce scénario, votre application est une application de traitement de texte. Après avoir créé un nouveau document, tant que le document reste vide, votre application le traite comme étant neutre, à savoir ni personnel ni d’entreprise. Votre utilisateur colle ensuite les données d’entreprise dans ce document de contexte neutre. Étant donné que le contexte du document est neutre, nous pourrions simplement basculer le document dans un contexte d’entreprise et ne pas appeler [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636) (dans la mesure où l’affichage de la boîte de dialogue n’est pas utile dans ce cas de figure). Ainsi, si les données sont protégées, il suffit de passer à un contexte d’entreprise et de coller les données.

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private async void OnPasteWithApplyPolicy()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        // If the data has no enterprise identity, then we already have access.
        if (!string.IsNullOrEmpty(dataPackageView.Properties.EnterpriseId))
        {
            ProtectionPolicyEvaluationResult policyResult =
                await dataPackageView.RequestAccessAsync(dataPackageView.Properties.EnterpriseId);
            if (this.isNewEmptyDocument &&
                policyResult == ProtectionPolicyEvaluationResult.Allowed)
            {
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog.
                bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy(dataPackageView.Properties.EnterpriseId);
                // You can use "isIdentityManaged", but it's not necessary.
            }
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.
                await dataPackageView.RequestAccessAsync();
            }
        }

        try
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            // Code goes here to insert the text into the email...
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the clipboard.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

## Source du Presse-papiers avec identité d’entreprise explicite


Dans ce scénario, votre application est une application de traitement de texte. Elle utilise un thread d’arrière-plan pour valider les opérations de copie de l’utilisateur. L’utilisateur copie des données d’un fichier d’entreprise et bascule immédiatement vers un fichier personnel. Le contexte global de l’application devient alors personnel. À ce stade, dans la mesure où l’état global a changé et ne doit pas être utilisé, le thread d’arrière-plan de l’application doit signaler explicitement au Presse-papiers que les données entrantes sont des données d’entreprise. Ceci est possible en définissant la propriété [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861).

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private void CopyToClipboard(string stringToCopy, string identity)
{
    // Copy the string to the clipboard.
    var dataPackage = new DataPackage();
    dataPackage.SetText(stringToCopy);
    dataPackage.Properties.EnterpriseId = identity;
    Clipboard.SetContent(dataPackage);
}
```

## Fenêtre spécifique de balise avec identité d’entreprise


Dans ce scénario, votre application est une application de traitement de texte qui gère plusieurs documents dans différentes fenêtres, dont certains sont des documents d’entreprise et d’autres des documents personnels. L’application cherche à s’assurer que toutes les données collées dans une fenêtre de document personnel sont contrôlées correctement (c’est-à-dire, refusées ou autorisées s’il s’agit de données d’entreprise), et que toutes les données sortantes d’une fenêtre de document d’entreprise sont marquées correctement. Pour cela, vous devez définir la propriété [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785).

```CSharp
using Windows.Security.EnterpriseData;

...

private void TagCurrentViewWithEnterpriseId(string identity)
{
    ProtectionPolicyManager.GetForCurrentView().Identity = identity;
}
```

## Objet glisser sortant de balise avec identité d’entreprise


Votre application comprend une fenêtre personnelle ouverte avec du contenu d’entreprise pouvant être glissé. L’utilisateur commence à faire glisser du contenu, et votre application doit s’assurer que le contenu est marqué comme étant d’entreprise. Ce scénario n’implique aucune nouvelle API. Pour ce scénario, l’application définira la propriété [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861) (voir [Source de Presse-papiers avec scénario d’identité d’entreprise explicite](#clipboard_source_explicit_id) ci-dessus).

## Objet glisser reçu de requêtes pour son identité d’entreprise


Votre application comprend un nouveau document vide ouvert, supposé être neutre tant qu’il est vide, et l’utilisateur fait glisser-déplacer du contenu dans le document. L’application doit alors déterminer l’identité d’entreprise de l’objet afin de modifier l’état du document en conséquence. Pour ce scénario, l’application obtiendra la propriété **EnterpriseId** du package de données en lisant [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861) (voir [Source de Presse-papiers avec scénario d’identité d’entreprise explicite](#clipboard_source_explicit_id) ci-dessus).

## Votre application comme source de contrat de partage


Lorsque vous prenez en charge le contrat de partage dans votre application, définissez le contexte d’identité d’entreprise dans le [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/br205873) comme illustré dans cet exemple de code.

**Remarque** Cet exemple de code suppose que vous avez déjà défini l’identité dans l’objet du gestionnaire de stratégies de protection de votre vue en cours (voir [Fenêtre spécifique de balise avec identité d’entreprise](#tag_window_with_id)). Sinon, la propriété [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) contient une chaîne vide.



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private void OnShareSourceOperation(object sender, RoutedEventArgs e)
{
    // Register the current page as a share source (or you could do this earlier in your app).
    DataTransferManager.GetForCurrentView().DataRequested += OnDataRequested;
    DataTransferManager.ShowShareUI();
}

private void OnDataRequested(DataTransferManager sender, DataRequestedEventArgs args)
{
    if (!string.IsNullOrEmpty(this.shareSourceContent))
    {
        var protectionPolicyManager = ProtectionPolicyManager.GetForCurrentView();
        DataPackage requestData = args.Request.Data;
        requestData.Properties.Title = this.shareSourceTitle;
        requestData.Properties.EnterpriseId = protectionPolicyManager.Identity;
        requestData.SetText(this.shareSourceContent);
    }
}
```

## Votre application comme cible de contrat de partage


Cet exemple de code poursuit notre stratégie tant que le fichier sur lequel nous travaillons est vide. Nous pouvons alors changer librement de contexte, le cas échéant, des données entrantes et éviter autant que possible de demander un accord sur l’interface utilisateur. Ainsi, lorsque votre application reçoit des données en tant que cible de contrat de partage, elle doit appeler [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791) s’il n’existe aucun contenu. Sinon, elle doit appeler [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636). L’exemple de code montre comment faire.

```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.ApplicationModel.DataTransfer.ShareTarget;

...

string identity = "microsoft.com";

protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    ShareOperation shareOperation = args.ShareOperation;
    if (shareOperation.Data.Contains(StandardDataFormats.Text))
    {
        // If the data has no enterprise identity, then we already have access.
        if (!string.IsNullOrEmpty(shareOperation.Data.Properties.EnterpriseId))
        {
            ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult =
                await ProtectionPolicyManager.RequestAccessAsync
                    (shareOperation.Data.Properties.EnterpriseId, identity);
            if (this.isNewEmptyDocument && protectionPolicyEvaluationResult ==
                ProtectionPolicyEvaluationResult.Allowed)
            {
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog.
                bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy
                    (shareOperation.Data.Properties.EnterpriseId);
                // You can use "isIdentityManaged", but it';s not necessary.
            }
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.
                protectionPolicyEvaluationResult = await shareOperation.Data.RequestAccessAsync();
            }
        }

        try
        {
            // Get the text from the share operation...
            App.shareTargetContent = await shareOperation.Data.GetTextAsync();
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the share operation.
        }
    }
    else
    {
        // The value in the share operation is not in the text format.
    }
}
```

## Stratégie de copier-coller de requête de manière passive


Dans ce scénario, votre application permet de coller l’interface utilisateur lorsque des données sont présentes dans le Presse-papiers uniquement. Pour cette fonctionnalité, vous pouvez utiliser la méthode [**ProtectionPolicyManager.CheckAccess**](https://msdn.microsoft.com/library/windows/apps/dn705783), qui permet de vérifier la stratégie de manière passive.

**Remarque** Cet exemple de code suppose que vous avez déjà défini l’identité dans l’objet du gestionnaire de stratégies de protection de votre vue en cours (voir [Fenêtre spécifique de balise avec identité d’entreprise](#tag_window_with_id)). Sinon, la propriété [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) contient une chaîne vide.



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private bool IsClipboardPeekAllowedAsync()
{
    ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = ProtectionPolicyEvaluationResult.Blocked;
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);
    }

    // Since this is a convenience feature to allow a peek of clipboard content,
    // if state is Blocked or ConsentRequired, do not show peek if this helper
    // method returns false.
    return (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed);
}
```

## Accès de requête à l’opération copier-coller


Ce scénario montre comment vérifier l’accès pour une opération de collage.

**Remarque** Cet exemple de code suppose que vous avez déjà défini l’identité dans l’objet du gestionnaire de stratégies de protection de votre vue en cours (voir [Fenêtre spécifique de balise avec identité d’entreprise](#tag_window_with_id)). Sinon, la propriété [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) contient une chaîne vide.



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private async void OnPasteWithRequestAccess()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);

        if (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed)
        {
            try
            {
                string contentsOfClipboard = await dataPackageView.GetTextAsync();
                // Code goes here to insert the text into the app...
                this.fileContentsTextBox.Text = contentsOfClipboard;
            }
            catch (Exception)
            {
                // Code goes here to handle the exception retrieving text from the clipboard.
            }
        }
        else
        {
            // Paste from the enterprise context is not allowed by policy.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

**Remarque** Cet article s’adresse aux développeurs de Windows 10 qui créent des applications pour la plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).



## Rubriques connexes


[exemple de protection des données d’entreprise (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Espace de noms Windows.Security.EnterpriseData**](https://msdn.microsoft.com/library/windows/apps/dn279153)







<!--HONumber=May16_HO2-->


